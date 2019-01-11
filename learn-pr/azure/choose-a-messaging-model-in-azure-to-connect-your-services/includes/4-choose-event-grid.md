Řada aplikací oznamuje distribuovaným komponentám, že se něco stalo nebo že se změnil některý objekt, pomocí modelu publikování a odběru. Předpokládejme, že máte aplikaci pro sdílení hudby s webovým rozhraním API, která běží v Azure. Když některý uživatel nahraje novou skladbu, potřebujete to oznámit všem mobilním aplikacím nainstalovaným na uživatelských zařízeních po celém světě pro uživatele, kteří mají zájem o daný žánr.

V této architektuře nemusí vydavatel zvukového souboru vědět o žádných odběratelích, kteří mají o sdílenou hudbu zájem. Kromě toho chceme mít vztah 1:N, který umožní mít řadu odběratelů, z nichž každý se může rozhodnout, jestli má o novou skladbu zájem. Pro tento typ architektury je ideálním řešením Azure Event Grid.

## <a name="what-is-azure-event-grid"></a>Co je Azure Event Grid?
Azure Event Grid je plně spravovaná služba směrování událostí, která využívá Azure Service Fabric. Event Grid distribuuje _události_ z různých zdrojů, jako jsou účty Azure Blob Storage nebo služba Azure Media Services, různým obslužným rutinám, jako je služba Azure Functions nebo webhooky. Event Grid je navržený tak, aby v Azure usnadnil vytváření aplikací založených na událostech a bez serverů.

Event Grid podporuje většinu služeb Azure jako vydavatele nebo odběratele a lze ho používat se službami jiných dodavatelů. Nabízí dynamicky škálovatelný a finančně dostupný systém zasílání zpráv, který umožňuje vydavatelům informovat uživatele o změně stavu. Následující obrázek znázorňuje, jak Azure Event Grid přijímá zprávy od několika zdrojů a distribuuje je obslužným rutinám události na základě předplatného.

V Azure Event Gridu existuje několik konceptů pro propojení zdroje s odběratelem:

1. **Události:** Co se stalo.
1. **Zdroje událostí:** Kde k události došlo.
1. **Témata:** Koncový bod, kam vydavatelé odesílají události.
1. **Odběry událostí:** Koncový bod nebo integrovaný mechanismus pro směrování událostí, někdy i do více obslužných rutin. Pomocí odběrů také obslužné rutiny inteligentně filtrují příchozí události.
1. **Obslužné rutiny událostí:** Aplikace nebo služba reagující na danou událost.

![Obrázek znázorňující pozici Azure Event Gridu mezi několika zdroji událostí a několika obslužnými rutinami událostí. Zdroje událostí odesílají událostí do Event Gridu a Event Grid předává relevantní události odběratelům. K rozhodování, které události se mají odeslat kterým obslužným rutinám, používá Event Grid témata. Zdroje událostí označují jednotlivé události jedním nebo více tématy a obslužné rutiny události se přihlašují k odběru témat, o která mají zájem.](../media/4-event-grid.png)

### <a name="what-is-an-event"></a>Co je událost?
**Události** jsou datové zprávy procházející přes Event Grid, které popisují, k čemu došlo. Každá událost je samostatná, může mít velikost až 64 kB a obsahuje několik informací na základě schématu, které definuje Event Grid:

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

| Pole | Popis |
|-------|-------------|
| **topic** | Úplná cesta prostředku ke zdroji události. Tuto hodnotu poskytuje Event Grid. |
| **subject** | Cesta k předmětu události, kterou definuje vydavatel. |
| **id** | Jedinečný identifikátor události. |
| **eventType** | Jeden z registrovaných typů události pro tento zdroj události. Jedná se o hodnotu, pro kterou lze vytvářet filtry, např. `CustomerCreated`, `BlobDeleted`, `HttpRequestReceived` atd. |
| **eventTime** | Čas, kdy byla událost vygenerována, na základě času UTC poskytovatele. |
| **data** | Určité informace, které jsou relevantní pro daný typ události. Například událost související s vytvářením nového souboru v Azure Storage obsahuje podrobnosti o daném souboru, jako je hodnota `lastTimeModified`. Událost služby Event Hubs zase obsahuje adresu URL souboru Capture. Toto pole je volitelné. |
| **dataVersion** | Verze schématu datového objektu. Verzi schématu definuje vydavatel. |
| **metadataVersion** | Verze schématu metadat události. Schéma vlastností nejvyšší úrovně definuje Event Grid. Tuto hodnotu poskytuje Event Grid. |

> [!TIP]
> Event Grid odešle událost, která indikuje, že se něco stalo nebo změnilo. _Skutečný objekt_, který se změnil, ale není součástí dat události. Místo něho se často jako odkaz na změněný objekt předává adresa URL nebo identifikátor.

### <a name="what-is-an-event-source"></a>Co je zdroj události?
Za odesílání událostí do Event Gridu zodpovídají zdroje událostí. Každý zdroj události souvisí s jedním nebo více typy událostí. Například Azure Storage je zdroj události pro události vytvořené objektem blob. IoT Hub je zdroj události pro události vytvořené zařízením. Vaše aplikace je zdroj události pro vlastní události, které definujete. Na zdroje událostí se podrobněji podíváme za chvíli.

> [!NOTE]
> Centrum událostí Azure také definuje koncept vydavatele události. Vydavatelem pro centrum událostí je uživatel nebo organizace, která se rozhodne odesílat události do Event Gridu. Například společnost Microsoft publikuje události pro několik služeb Azure. Můžete publikovat události z vaší vlastní aplikace. Organizace, které hostují služby mimo Azure, můžou publikovat události přes Event Grid. Vydavatel se často směšuje se zdrojem události. Plně definovaným zdrojem události je vydavatel a konkrétní služba generující událost pro tohoto vydavatele. Tady budeme termíny „vydavatel“ a „zdroj události“ používat zaměnitelně pro entitu, která odesílá zprávy do centra událostí.

### <a name="what-is-an-event-topic"></a>Co je téma události?
Témata událostí kategorizují události do skupin. Témata zastupuje veřejný koncový bod. Do tohoto umístění zdroj události odesílá události _to_. Při návrhu aplikace můžete rozhodnout, kolik témat se má vytvořit. V rozsáhlejších řešeních se bude vytvářet vlastní téma pro každou kategorii souvisejících událostí, zatímco v menších řešeních se můžou všechny události odesílat do jediného tématu. Představme si například aplikaci, která odesílá události související s úpravami uživatelských účtů a zpracováním objednávek. Není pravděpodobné, že nějaká obslužná rutina události chce přijímat obě kategorie událostí. Vytvořte dvě vlastní témata a nechte obslužné rutiny událostí odebírat to téma, které je zajímá. Odběratelé událostí můžou vyfiltrovat požadované typy událostí z určitého tématu.

Témata se dělí na **systémová** témata a **vlastní** témata.

#### <a name="system-topics"></a>Systémová témata
Systémová témata jsou předdefinovaná témata, která poskytují služby Azure. Ve vašem předplatném Azure nevidíte systémová témata, protože tato témata vlastní vydavatel, ale můžete se přihlásit k jejich odběru. Při přihlášení k odběru zadáte informace o prostředku, ze kterého chcete přijímat události. Pokud máte k danému prostředku přístup, můžete se přihlásit k odběru jeho událostí.

#### <a name="custom-topics"></a>Vlastní témata
Vlastní témata jsou témata aplikací a témata třetích stran. Po vytvoření vlastního tématu nebo po přiřazení vašeho přístupu k vlastnímu tématu se dané vlastní téma zobrazí ve vašem předplatném.

### <a name="what-is-an-event-subscription"></a>Co je odběr události?
Odběry událostí definují v tématu události, které obslužná rutina události chce přijímat. Odběr může také filtrovat události podle jejich typu nebo předmětu, abyste měli jistotu, že obslužná rutina události přijímá jenom relevantní události.

### <a name="what-is-an-event-handler"></a>Co je obslužná rutina události?
Obslužná rutina události (někdy se označuje jako „odběratel“ události) je jakákoli součást (aplikace nebo prostředek), která může přijímat události z Event Gridu. Azure Functions může například spustit kód v reakci na nové skladby přidané do účtu služby Blob Storage. Odběratelé se můžou rozhodnout, které události chtějí zpracovat, a Event Grid každého odběratele, který o to má zájem, efektivním způsobem informuje vždy, když se zveřejní nová událost. Není tedy potřeba žádné dotazování.

## <a name="types-of-event-sources"></a>Typy zdrojů událostí
Události můžou být generovány následujícími typy prostředků Azure:

- **Předplatná a skupiny prostředků Azure:** Předplatná a skupiny prostředků generují události související s operacemi správy v Azure. Když například uživatel vytvoří virtuální počítač, vygeneruje tento zdroj událost.
- **Container Registry:** Služba Azure Container Registry generuje události při přidání, odebrání nebo změně image v registru.
- **Event Hub:** Služba Event Hub se používá ke zpracování a uložení událostí z různých zdrojů dat – obvykle souvisejících s protokolováním nebo telemetrií. Služba Event Hub může generovat události do Event Gridu po zachycení souborů.
- **Service Bus:** Služba Service Bus může generovat události do Event Gridu, pokud existují aktivní zprávy bez aktivních naslouchacích procesů.
- **Účty úložiště:** Účty úložiště můžou generovat události, když uživatelé přidají objekty blob, soubory, položky tabulky nebo zprávy do fronty. Jako zdroje událostí můžete používat jak účty objektů blob, tak účty pro obecné účely verze 2.
- **Media Services:** Služba Media Services hostuje videa a zvukové soubory a poskytuje pokročilé funkce správy mediálních souborů. Služba Media Services může generovat události při spuštění nebo dokončení úlohy kódování souboru videa.
- **Azure IoT Hub:** Služba IoT Hub komunikuje se zařízeními IoT a shromažďuje z nich telemetrická data. Při přijetí takových komunikací může generovat události.
- **Vlastní události:** Vlastní události se můžou generovat pomocí rozhraní REST API nebo sady Azure SDK na platformách Java, GO, .NET, Node, Python a Ruby. Například můžete vytvořit vlastní události pomocí funkce Web Apps služby Azure App Service. Může k tomu dojít v roli pracovního procesu při vyzvednutí zprávy z fronty úložiště.

Tato těsná integrace s různorodými zdroji událostí v Azure umožňuje službě Event Grid distribuovat události, které se můžou týkat skoro všech prostředků Azure.

## <a name="event-handlers"></a>Obslužné rutiny událostí
Přijímat a zpracovávat události z Event Gridu můžou následující typy objektů v Azure:

- **Azure Functions:** Vlastní kód běžící v Azure bez potřeby explicitní konfigurace hostitelského virtuálního serveru nebo kontejneru. Funkci Azure můžete použít jako obslužnou rutinu událostí, pokud chcete nakódovat vlastní odpověď na událost.
- **Webhooky:** Webhook je webové rozhraní API, které implementuje architekturu nabízených oznámení.
- **Azure Logic Apps:** Aplikace Azure Logic Apps hostuje obchodní proces jako pracovní postup.
- **Microsoft Flow:** Flow také hostuje pracovní postupy, ale nabízí jednodušší použití pro pracovníky, kteří nejsou technickými odborníky.

## <a name="should-you-use-event-grid"></a>Měli byste používat Event Grid?
Službu Event Grid použijte tehdy, když potřebujete tyto funkce:

- **Jednoduchost:** V Event Gridu se dají zdroje velmi snadno propojit s odběrateli.
- **Rozšířené filtrování:** Odběry můžou do velké míry určovat, jaké události chtějí z určitého tématu přijímat.
- **Větvení:** Ke stejným událostem a tématům můžete přihlásit neomezený počet koncových bodů.
- **Spolehlivost:** Event Grid opakuje pokusy o doručení události pro každý odběr až 24 hodin.
- **Platba za události:** Platíte jenom za počet událostí, které odešlete.

Event Grid je jednoduchý, ale všestranný systém distribuce událostí. Můžete ho použít k doručování samostatných událostí odběratelům a zajistit tak, že je spolehlivě a rychle dostanou. Zbývá ještě jeden model zasílání zpráv, který jsme zatím neprozkoumali: co když chceme doručit objemný _datový proud_ událostí? Pro tyto situace není Event Grid právě ideální, protože je navržený k průběžnému doručování jednotlivých událostí. Namísto toho nám teď pomůže jiná služba Azure: Event Hubs.
