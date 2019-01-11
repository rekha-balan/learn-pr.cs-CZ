Použití a zpracovávání dat jsou klíčové úlohy v mnoha softwarových řešeních. Zvažte některé z těchto scénářů:

* Byli jste požádáni o implementaci metody pro přesun příchozích dat z úložiště objektů blob od Azure Cosmos DB.
* Chcete umístit příchozí zprávy do fronty pro zpracování jinou komponentou ve vašem podniku.
* Vaše služba potřebuje získat hráčské výsledky z fronty a aktualizovat online žebříčky.

Všechny tyto příklady jsou o přesunu dat. Zdroj dat a cíle se mezi scénáři liší, ale vzor je podobný. Připojíte se ke zdroji dat a zapisujete a čtete data. Služba Azure Functions vám pomáhá integrovat data a služby s použitím vazeb. 

## <a name="what-is-a-binding"></a>Co je vazba?

Vazby v Azure Functions poskytují deklarativní způsob připojení k datům z vašeho kódu. Zjednodušují konzistentní integraci s datovými proudy ve funkci. Můžete mít více vazeb, které budou poskytovat přístup k různým datovým prvkům. To je velmi efektivní, protože se můžete ke zdrojům dat připojit bez nutnosti programovat konkrétní připojovací logiku (například databázová připojení nebo webová rozhraní API).

### <a name="types-of-bindings"></a>Typy vazeb

Existují dva druhy vazeb, které můžete u funkcí použít:

1. **Vstupní vazba** Vstupní vazba je připojení ke **zdroji** dat. Naše funkce může číst data z těchto vstupů.

1. **Výstupní vazba** Výstupní vazba je připojení k **cíli** dat. Naše funkce může zapisovat data do těchto cílů.

K dispozici jsou také aktivační události. Aktivační události jsou zvláštní typy vstupních vazeb, které vyvolají provedení funkce. Jako aktivační událost můžete například nakonfigurovat oznámení služby Azure Event Grid. Při výskytu události se funkce spustí.

### <a name="types-of-supported-bindings"></a>Typy podporovaných vazeb

*Typ* vazby definuje, odkud čteme nebo kam odesíláme data. Existuje vazba pro odezvu na webové požadavky a široká škála vazeb pro přímou interakci s různými službami Azure a také s externími službami.

Typ vazby je možné použít jako vstup, výstup nebo obojí. Určitá funkce může například zapisovat do výstupní vazby služby Azure Blob Storage, ale aktualizace úložiště objektů blob může spouštět jinou funkci.

Tady jsou některé běžné typy vazeb:
- Blob Storage
- Fronty služby Azure Service Bus
- Azure Cosmos DB
- Azure Event Hubs
- Externí soubory
- Externí tabulky
- Koncové body HTTP

Tyto typy uvádíme jen pro představu. Je jich více a kromě toho mají funkce model rozšiřitelnosti, který umožňuje přidat další vazby.

### <a name="binding-properties"></a>Vlastnosti vazeb

Ve všech vazbách se vyžadují tři vlastnosti. Na základě vámi používaného typu vazby a úložiště může být nutné určit další vlastnosti.

1. **Název:** Definuje parametr funkce, jehož prostřednictvím přistupujete k datům. Ve vstupní vazbě fronty se například jedná o název parametru funkce, která obdrží obsah zprávy fronty. 

1. **Typ:** Identifikuje typ vazby, tj. typ dat nebo služby, se kterými chceme provádět interakci.

1. **Směr:** Označuje směr toku dat, tzn. jedná se o vstupní, nebo výstupní vazbu?

Kromě toho většina typů vazeb potřebuje i čtvrtou vlastnost: 

4. **Připojení:** Jde o název klíče nastavení aplikace, který obsahuje připojovací řetězec. Vazby používají připojovací řetězce, které jsou uložené v nastavení aplikace, aby se tajné kódy uchovávaly mimo kód funkce. Díky tomu je váš kód lépe konfigurovatelný a více zabezpečený.

## <a name="create-a-binding"></a>Vytvoření vazby

Vazby jsou definované v souboru JSON. Vazba je konfigurovaná v konfiguračním souboru vaší funkce, který má název *function.json* a nachází se ve stejné složce jako kód funkce.

 Pojďme prozkoumat příklad *vstupní vazby*:

```json
    ...
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
    ...
```

Vytvoření této vazby

1. Vytvořte vazbu v našem souboru *function.json*.

1. Zadejte hodnotu proměnné `name`. V tomto příkladu proměnná obsahuje data objektu blob.

1. Zadejte vlastnost `type` pro úložiště. V předchozím příkladu používáme úložiště objektů blob.

1. Zadejte vlastnost `path`, která určuje kontejner a název položky, která do ní patří. Pro objekty blob se vyžaduje vlastnost `path`.

1. Zadejte název nastavení řetězce `connection` definovaný v souboru nastavení aplikace. Používá se jako klíč k vyhledání připojovacího řetězce pro připojení k vašemu účtu úložiště.

1. Definujte `direction` jako `in`. Čte data z objektu blob.

Vazby se ve funkci používají k připojení k datům. V tomto příkladu jsme použili vstupní vazbu pro připojení obrázků uživatelů, které mají být vaší funkcí zpracovány jako miniatury.
