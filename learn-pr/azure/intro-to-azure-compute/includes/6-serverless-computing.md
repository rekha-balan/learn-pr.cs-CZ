:::row:::
  :::column:::
    ![Obrázek představující bezserverovou architekturu](../media/6-serverless.png)
  :::column-end:::
    :::column span="3":::
    Prostřednictvím _bezserverové_ architektury Azure zajišťuje správu serverové infrastruktury a přidělování a navracení prostředků na základě poptávky. O infrastrukturu se nemusíte vůbec starat. Škálování a výkon jsou zajišťovány automaticky a poplatky se účtují výhradně za prostředky, které použijete. Není ani potřeba si rezervovat kapacitu.

    Můžete se zaměřit výhradně na logiku, kterou je nutné provádět a _spouštět_ a na jejímž základě kód funguje. Bezserverové aplikace konfigurujete tak, aby reagovaly na _události_. Ty může představovat koncový bod REST, pravidelný časovač nebo zpráva obdržená z jiné služby Azure. Aplikace bez serveru se spouští jen při aktivaci nějakou událostí.
  :::column-end:::
:::row-end:::

#### <a name="serverless-computing-in-azure"></a>Bezserverová architektura v Azure

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yzjL]

Azure má dvě implementace bezserverové architektury:

- **Azure Functions** – tyto funkce můžou provádět kód v téměř libovolném moderním jazyce.
- **Azure Logic Apps** – tyto aplikace se navrhují ve webovém návrháři a můžou provádět logiku spouštěnou službami Azure bez nutnosti psát jakýkoliv kód.

## <a name="azure-functions"></a>Azure Functions

Služba Azure Functions je ideální volbou v případě, že vás zajímá jen kód vaší služby, nikoliv podkladová platforma nebo infrastruktura. Obvykle se využívá tehdy, když potřebujete provádět práci v reakci na konkrétní událost, často prostřednictvím požadavku REST, časovače nebo zprávy z jiné služby Azure, a když je nutné takovou práci dokončit rychle (v řádu sekund nebo ještě rychleji). 

Služba Azure Functions provádí škálování automaticky podle poptávky, takže se vcelku hodí tam, kde jsou požadavky proměnlivé. Můžete například dostávat zprávy z řešení IoT, které slouží ke sledování vozového parku logistické společnosti. Je pravděpodobné, že během pracovní doby vám bude přicházet více dat.

S řešením na bázi virtuálních počítačů by nám poplatky nabíhaly i při nečinnosti virtuálního počítače. Naproti tomu služba Azure Functions spouští váš kód v případě, že je aktivován, a po dokončení funkce přidělené prostředky automaticky vrátí. V tomto modelu se vám účtuje jen čas procesoru spotřebovaný při běhu vaší funkce.  

Kromě toho můžou být funkce v rámci Azure Functions bezstavové (výchozí nastavení; chovají se, jako by se při každé reakci na událost restartovaly) nebo stavové (označují se jako Durable Functions), kde se prostřednictvím funkce předává kontext, aby bylo možné sledovat předchozí aktivity.

## <a name="azure-logic-apps"></a>Azure Logic Apps

Služba Azure Logic Apps se podobá službě Functions – obě umožňují spuštění logiky na základě události. Ale zatímco služba Functions spouští kód, služba Logic Apps spouští _pracovní postupy_ z předdefinovaných bloků logiky. Ty jsou speciálně navržené pro automatizaci vašich obchodních procesů.

Pracovní postupy pro aplikaci logiky vytvoříte pomocí vizuálního návrháře na webu Azure Portal nebo v sadě Visual Studio. Pracovní postupy jsou trvale uložené jako soubory JSON se známým schématem pracovního postupu.

Azure poskytuje více než 200 různých konektorů a procesních bloků pro interakci s různými službami – včetně nejpopulárnějších firemních aplikací. Pokud služba pro vámi požadovanou aplikaci není k dispozici, máte také možnost sestavit si vlastní konektory a kroky pracovního postupu. Pomocí vizuálního návrháře pak můžete konektory a bloky vzájemně propojit, aby se přes pracovní postup předávala a určeným způsobem zpracovávala data – a to vše často bez psaní kódu.

Představme si situaci, že ZenDesk přijde nový lístek. Mohli byste:

1. Detekovat účel zprávy pomocí Cognitive Services
1. Vytvořit na SharePointu položku pro sledování problému
1. Pokud zákazník není ve vaší databázi, přidat ho do systému Dynamics 365 CRM
1. Odeslat e-mail potvrzující přijetí žádosti

To vše je možné navrhnout ve vizuálním návrháři – v něm je velmi přehledně vidět logický tok, takže je ideální pro roli obchodního analytika.

## <a name="functions-vs-logic-apps"></a>Functions versus Logic Apps

Ve službě Functions i Logic Apps je možné vytvářet složité orchestrace. Orchestrace je kolekce funkcí nebo kroků, jejichž spouštěním se provede složitý úkol. V Azure Functions vytvoříte kód pro jednotlivé kroky a v Logic Apps přes grafické uživatelské rozhraní definujete akce a jejich vzájemné vztahy.

Při sestavování orchestrace lze služby kombinovat, takže je možné volat funkce z aplikací logiky a volat aplikace logiky z funkcí. Tady je několik běžných rozdílů mezi nimi.

|-| Functions |    Logic Apps |
|-|-------------------|------------|
| Stav | Obvykle bezstavové, ale Durable Functions vykazují stav. | Stavové |
| Vývoj | Založeno na kódu (imperativní) | Založeno na návrháři (deklarativní) |
| Připojení | Více než desítka předdefinovaných typů vazeb, zápis kódu pro vlastní vazby | Rozsáhlá kolekce konektorů, Enterprise Integration Pack pro scénáře B2B, sestavení vlastních konektorů |
| Akce | Každá aktivita je funkce Azure; zápis kódu pro funkce aktivity | Rozsáhlá kolekce předdefinovaných akcí |
| Monitorování | Azure Application Insights | Azure Portal, Log Analytics |
| Správa | REST API, Visual Studio | Azure Portal, REST API, PowerShell, Visual Studio |
| Kontext spuštění | Může se spouštět místně nebo v cloudu. | Spouští se jenom v cloudu. |