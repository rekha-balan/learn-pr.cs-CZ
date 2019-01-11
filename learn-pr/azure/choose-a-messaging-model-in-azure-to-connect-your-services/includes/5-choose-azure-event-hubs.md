Existují určité aplikace, které vytvářejí velké množství událostí z podobně obrovského počtu zdrojů. V těchto situacích často slýcháme termín „Big Data“ a tyto velké objemy dat vyžadují specifickou infrastrukturu, která je zvládne zpracovat.

Představte si, že pracujete pro firmu Contoso Aircraft Engines. Letecké motory, které vaše firma vyrábí, mají stovky senzorů. Než letadlo každé ráno vzlétne, je třeba motory připojit k testovacímu systému a pečlivě vyzkoušet jejich funkci. Kromě toho se v době, kdy je letadlo připojené k pozemnímu systému, streamují i nashromážděná letová data.

Chcete použít historická data ze senzorů k vyhledání vzorů v naměřených hodnotách, které mohou ukazovat na možné selhání v blízké budoucnosti. Také chcete porovnat hodnoty získané v reálném čase s historickými údaji o těchto vzorech naznačujících poruchu. Pak budete moci uživatele téměř v reálném čase varovat, že jejich motor vykazuje znepokojivé známky blížící se poruchy.

## <a name="what-is-azure-event-hubs"></a>Co je služba Azure Event Hubs?
Služba Event Hubs zprostředkovává komunikační vzor založený na principu publikování a odběru. Na rozdíl od služby Event Grid je ale optimalizovaná pro extrémně vysokou propustnost, velký počet vydavatelů, zabezpečení a odolnost proti chybám.

#### <a name="what-is-an-event-hub"></a>Co je centrum událostí?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuat]

Zatímco Event Grid přesně kopíruje vzorec publikovat-odebírat v tom, že jednoduše spravuje odběry a komunikační trasy k odběratelům, Event Hubs zajišťuje mnoho dalších služeb. Díky těmto dalším službám je podobnější službě Service Bus nebo frontě zpráv než jednoduchému vysílání událostí.

#### <a name="partitions"></a>Oddíly
Když služba Event Hubs přijme nějakou komunikaci, rozdělí ji do oddílů. Oddíly jsou vyrovnávací paměti, do kterých se komunikace ukládají. Protože jde o vyrovnávací paměti, události nejsou čistě dočasné a neztratí se jen proto, že je odběratel zaneprázdněný nebo dokonce offline. Odběratel může díky vyrovnávací paměť zdroj dat kdykoli „dohnat“. Ve výchozím nastavení zůstávají události ve vyrovnávací paměti 24 hodin, než automaticky vyprší.

Vyrovnávací paměti se nazývají oddíly, protože se data mezi ně rozdělují. Každé centrum událostí má alespoň dva oddíly a každý oddíl má samostatnou sadu odběratelů.

#### <a name="capture"></a>Zachytávání
Event Hubs může všechny události odesílat okamžitě do služby Azure Data Lake nebo Blob Storage pro levné trvalé uchování.

#### <a name="authentication"></a>Ověřování
Všichni vydavatelé jsou ověřováni a dostanou token. To znamená, že Event Hubs může přijímat události z externích zařízení a mobilních aplikací bez starosti o to, aby případná podvržená data neznehodnotila výslednou analýzu. 

## <a name="using-event-hubs"></a>Použití Event Hubs
Služba Event Hubs obsahuje podporu pro kanálové odesílání proudů událostí do jiných služeb Azure. Propojení se službou Azure Stream Analytics například umožňuje komplexní analýzy dat téměř v reálném čase, korelaci více událostí a vyhledávání vzorů. V takovém případě se Stream Analytics bude považovat za odběratele.

Pro naše letecké motory vytvoříme architekturu tak, aby služba Event Hubs ověřovala komunikaci od našich motorů. Tu potom zachytíme a všechna data uložíme do Data Lake. Veškerá tato data pak můžeme použít později k obnovení a vylepšení našich modelů strojového učení. A konečně bude náš datový proud událostí předán odběratelům služby Stream Analytics. Stream Analytics bude pomocí modelu strojového učení vyhledávat vzory v datech ze senzorů, které mohou značit problémy.

Protože máme několik oddílů a každý motor odesílá všechna svoje data jen do jednoho oddílu, každé instanci odběratele Stream Analytics stačí zabývat se jen podmnožinou všech dat. Nemusí filtrovat a korelovat veškerá data.

## <a name="which-service-should-i-choose"></a>Kterou službu mám zvolit?
Stejně jako při výběru řešení pro fronty se může i volba mezi těmito dvěma službami doručování událostí zdát zpočátku složitá. Obě podporují sémantiku *alespoň jednou*.

#### <a name="choose-event-hubs-if"></a>Zvolte Event Hubs, pokud:  

- budete potřebovat podporu ověřování velkého počtu vydavatelů,
- budete chtít uložit datový proud událostí do Data Lake nebo Blob Storage,
- potřebujete agregaci nebo analýzu datového proudu událostí,
- potřebujete spolehlivé zasílání zpráv nebo odolnost proti chybám.  

Pokud potřebujete jednoduchou infrastrukturu pro publikování a odběr událostí s důvěryhodnými vydavateli (jako je například váš vlastní webový server), měli byste zvolit Event Grid.

Event Hubs vám umožní vytvořit kanál pro velký objem dat, který je schopný zpracovat miliony událostí za sekundu při nízké latenci. Dokáže zpracovat data ze souběžných zdrojů a přesměrovat je do celé řady infrastruktur pro zpracování datového proudu a analytických služeb. Umožňuje zpracování v reálném čase a podporuje opětovné přehrání uložených nezpracovaných dat. 