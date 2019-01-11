Předpokládejme, že plánujete architekturu své aplikace pro sdílení hudby. Chcete mít jistotu, že se hudební soubory budou z mobilní aplikace spolehlivě nahrávat do webového rozhraní API a také chcete doručit podrobnosti o nových skladbách přímo do aplikace, kdykoli interpret přidá do své sbírky nový hudební soubor. To je ideální případ pro použití systému založeného na zprávách a Azure pro takovou situaci nabízí dvě řešení:

- Azure Queue Storage
- Azure Service Bus

## <a name="what-is-azure-queue-storage"></a>Co je Azure Queue Storage?
Queue Storage je služba, která využívá Azure Storage k ukládání velkého množství zpráv. Ke zprávám je možné snadno získat přístup odkudkoli na světě prostřednictvím jednoduchého rozhraní založeného na architektuře REST. Fronty můžou obsahovat miliony zpráv: jejich počet je omezen jen kapacitou účtu úložiště, který je vlastní.

## <a name="what-is-azure-service-bus-queues"></a>Co jsou fronty služby Azure Service Bus?
Service Bus je systém zprostředkování zpráv určený pro podnikové aplikace. Tyto aplikace často používají řadu komunikačních protokolů, mají různé datové kontrakty, vyšší požadavky na zabezpečení a mohou zahrnovat jak cloudové, tak místní služby. Service Bus je postavený na vyhrazené infrastruktuře pro zasílání zpráv vytvořené přímo pro tyto scénáře.

Obě tyto služby jsou založené na konceptu „fronty“, kde odeslané zprávy zůstanou, dokud cíl není připravený je přijmout.

## <a name="what-are-azure-service-bus-topics"></a>Co jsou témata služby Azure Service Bus?
Témata služby Azure Service Bus jsou jako fronty, ale mohou mít více odběratelů. Když je zpráva odeslána do tématu místo fronty, může se aktivovat víc komponent, aby dělaly svou práci. Představte si uživatele poslouchajícího skladbu v aplikaci pro sdílení hudby. Mobilní aplikace může odeslat zprávu do tématu „Poslechnuto“. Toto téma bude mít odběr pro „UpdateUserListenHistory“ a jiný odběr pro „UpdateArtistsFanList“. Každá z těchto funkcí je zpracovávána jinou komponentou, která obdrží vlastní kopii zprávy.

Interně témata používají fronty. Když přidáte příspěvek do tématu, zpráva se zkopíruje a přemístí do fronty pro jednotlivé odběry. Fronta znamená, že kopie zprávy se zachová pro zpracování **jednotlivými větvemi odběrů** i v případě, že komponenta zpracovávající daný odběr je příliš zaneprázdněná a nestíhá.

## <a name="benefits-of-queues"></a>Výhody front
Infrastruktury front můžou podporovat řadu pokročilých funkcí, takže můžou být velmi užitečné. 

### <a name="increased-reliability"></a>Vyšší spolehlivost
Distribuované aplikace využívají fronty jako dočasné umístění úložiště pro zprávy, které čekají na doručení do cílové komponenty. Zdrojová komponenta může přidat zprávu do fronty a cílová komponenta může načíst zprávu ze začátku fronty za účelem zpracování. Fronty zvyšují spolehlivost výměny zpráv, protože v dobách vysokého zatížení můžou zprávy jednoduše počkat, dokud nebude některá cílová komponenta připravená je zpracovat.

### <a name="message-delivery-guarantees"></a>Záruky doručení zpráv
Systémy řazení do front většinou zaručují doručení každé zprávy ve frontě do cílové komponenty. Přístupy k těmto zárukám ale můžou být různé:

- **Alespoň jedno doručení:** Tento přístup znamená, že se zaručuje doručení každé zprávy alespoň do jedné z komponent, které načítají zprávy z fronty. Mějte ale na paměti, že za určitých okolností se může stát, že se stejná zpráva doručí vícekrát. Pokud například existují dvě instance webové aplikace načítající zprávy z fronty, většinou se každá zpráva doručí jenom do jedné z těchto instancí. Pokud ale jedné instanci trvá zpracování zprávy dlouhou dobu a dojde k vypršení časového limitu, zpráva se může odeslat i do druhé instance. To by měl kód vaší webové aplikace zohledňovat.

- **Nanejvýš jedno doručení:** Tento přístup znamená, že u žádné zprávy se nezaručuje doručení a existuje velmi malá šance, že zpráva vůbec nedorazí. Neexistuje ale žádná šance, že se zpráva doručí dvakrát, jako je tomu u přístupu Alespoň jedno doručení. Tento přístup se někdy označuje jako „automatické vyhledávání duplicit“.

- **FIFO (First-In-First-Out):** Ve většině systémů zasílání zpráv obvykle zprávy opouštějí frontu ve stejném pořadí, ve kterém do ní přibyly, ale měli byste zvážit, jestli má být toto pořadí zaručené. Pokud vaše distribuovaná aplikace vyžaduje zpracování zpráv přesně ve správném pořadí, je potřeba zvolit systém řazení do front, který obsahuje záruku FIFO.

### <a name="transactional-support"></a>Podpora transakcí
Některé úzce související skupiny zpráv můžou způsobovat potíže, pokud se nezdaří doručení jedné zprávy ze skupiny.

Představte si například aplikaci pro elektronické obchodování. Jakmile uživatel klikne na tlačítko **Koupit**, může se vygenerovat celá série zpráv, které se rozešlou ke zpracování do různých cílů:

- Zpráva s podrobnostmi o objednávce se odešle do centra pro vyřizování objednávek.
- Zpráva s částkou k zaplacení a údaji o platbě se odešle zpracovateli platební karty. 
- Zpráva s informacemi o vyúčtování se odešle do databáze, která pro zákazníka vygeneruje fakturu.

V takovém případě chceme mít jistotu, že se zpracují buď _všechny_ zprávy, nebo žádná z nich. Pokud se přestanou doručovat zprávy o platebních kartách, ale všechny objednávky se zpracují, přestože nejsou zaplacené, nečeká naši firmu příliš růžová budoucnost. Takovýmto potížím se můžete vyhnout tím, že obě zprávy seskupíte do transakce. Zprávy v transakcích se chovají jako jeden celek, jako je tomu u databází: doručí se buď všechny, nebo žádná. Pokud se nezdaří doručení zprávy s údaji o platební kartě, nezdaří se ani doručení zprávy s podrobnostmi o objednávce.

## <a name="which-service-should-i-choose"></a>Kterou službu mám zvolit?
Ujasnili jste si, že strategie komunikace v této architektuře by měla být založena na zprávách. Teď se tedy potřebujete rozhodnout, jestli použijete fronty služby Azure Storage, nebo službu Azure Service Bus. Obě možnosti se dají použít k ukládání a doručování zpráv mezi vašimi komponentami. Každá z nich má mírně odlišnou sadu funkcí, takže si můžete vybrat jednu z nich nebo použít obě podle toho, jakou situaci řešíte.

#### <a name="choose-service-bus-topics-if"></a>Témata služby Service Bus si vyberte v případě, že:

- Potřebujete více příjemců pro zpracování jednotlivých zpráv.


#### <a name="choose-service-bus-queues-if"></a>Fronty služby Service Bus si vyberte v případě, že:

- Potřebujete záruku Nanejvýš jedno doručení.
- Potřebujete záruku FIFO.
- Potřebujete seskupovat zprávy do transakcí.
- Chcete dostávat zprávy bez dotazování fronty.
- Potřebujete na fronty uplatnit řízení přístupu na základě role.
- Potřebujete zpracovávat zprávy větší než 64 kB, ale menší než 256 kB.
- Velikost fronty nepřekročí 80 GB.
- Chcete mít možnost publikovat a používat dávky zpráv.

Queue Storage nenabízí tak širokou škálu funkcí, ale pokud žádnou z těchto funkcí nepotřebujete, může to pro vás být jednodušší volba. Kromě toho je to nejlepší řešení, pokud má vaše aplikace některé z následujících požadavků.

#### <a name="choose-queue-storage-if"></a>Queue Storage si vyberte v případě, že:

- Potřebujete protokol auditu pro všechny zprávy, které projdou frontou.
- Očekáváte, že bude fronta větší než 80 GB.
- Chcete sledovat průběh zpracování zprávy ve frontě.

Fronta je jednoduché dočasné úložiště zpráv odesílaných mezi komponentami distribuované aplikace. Fronty slouží k uspořádání zpráv a hladkému zvládnutí nepředvídatelných špiček zatížení. 

Pokud chcete jednoduchý systém front se snadným kódováním, použijte fronty služby Storage. Pokud máte pokročilejší potřeby, použijte fronty služby Service Bus. Pokud máte pro jednu zprávu více cílů, ale potřebujete chování jako u front, použijte témata.