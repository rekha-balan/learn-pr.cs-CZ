Migrace místních serverů do Azure vyžaduje plánování a péči. Můžete je přesunout všechny najednou, nebo pravděpodobněji v malých dávkách, dokonce i jednotlivě. Před vytvořením prvního virtuálního počítače byste měli načrtnout aktuální model infrastruktury a vymyslet, jak provádět namapování do cloudu.

#### <a name="required-resources-for-iaas-virtual-machines"></a>Požadované prostředky pro virtuální počítače IaaS

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWjVUg]

Projděme si kontrolní seznam věcí k promyšlení.

- Začínáme se sítí
- Název virtuálního počítače
- Rozhodnutí umístění prostředků virtuálního počítače
- Určení velikosti virtuálního počítače
- Porozumění principům cenového modelu
- Úložiště virtuálního počítače
- Výběr operačního systému

## <a name="start-with-the-network"></a>Začínáme se sítí

Než začnete přemýšlet o virtuálním počítači, měli byste nejdříve promyslet virtuální síť.

Virtuální sítě v Azure poskytují privátní připojení mezi Azure Virtual Machines a dalšími službami Azure. Virtuální počítače a služby, které jsou součástí stejné virtuální sítě, k sobě mohou vzájemně přistupovat. Ve výchozím nastavení se služby mimo virtuální síť nemohou připojit ke službám ve virtuální síti. Síť můžete ale nakonfigurovat tak, aby byl přístup k externí službě povolen, a to i pro místní servery.

To je důvod, proč byste se měli zamyslet nad konfigurací sítě. Síťové adresy a podsítě se po úvodním nastavení nemění jednoduše. Pokud tedy máte v plánu připojit privátní podnikové sítě ke službám Azure, bude potřeba před vytvořením virtuálních počítačů zvážit topologii.

Při nastavování virtuální sítě se zadávají dostupné adresní prostory, podsítě a zabezpečení. Pokud má být virtuální síť připojená k dalším virtuálním sítím, je třeba vybrat rozsahy adres, které se nepřekrývají. Toto je rozsah privátních adres, které můžou použít virtuální počítače a služby ve vaší síti. Můžete použít nesměrovací IP adresy, například 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, nebo určit vlastní rozsah. Azure bude považovat všechny rozsahy adres za součást privátního IP adresního prostoru virtuální sítě, pokud je dostupný pouze v rámci příslušné virtuální sítě, v rámci vzájemně propojených virtuálních sítí a z místního umístění. Pokud je za interní sítě zodpovědný někdo jiný, měli byste společně před výběrem adresního prostoru zajistit, aby nedošlo k překrývání, a zjistit, zda plánovaný rozsah IP adres už neplánují využít.

### <a name="segregate-your-network"></a>Rozdělení sítě

Jakmile jsou určené rozsahy adres virtuální sítě, můžete na virtuální síti vytvořit jednu nebo více podsítí. Rozdělením sítě do více menších oddílů usnadníte jejich správu. Můžete například přiřadit adresy 10.1.0.0 virtuálním počítačům, 10.2.0.0 back endovým službám a 10.3.0.0 virtuálním počítačům s SQL serverem.

> [!NOTE]
> Azure si vyhrazuje první čtyři adresy a poslední adresu v každé podsíti pro vlastní využití.

### <a name="secure-the-network"></a>Zabezpečení sítě

Ve výchozím nastavení mezi podsítěmi neexistuje žádná hranice zabezpečení, takže služby v každé z těchto podsítí mohou vzájemně komunikovat. Můžete ale nastavit skupiny zabezpečení sítě (NSG), které umožňují řídit tok do a ze sítí a také do a z virtuálních počítačů. Skupiny zabezpečení sítě fungují jako softwarové brány firewall, které u každé příchozí nebo odchozí žádosti používají vlastní pravidla na úrovni síťového rozhraní a podsítě. To umožňuje plně řídit všechny síťové žádosti přicházející do virtuálního počítače nebo z něj.

## <a name="plan-each-vm-deployment"></a>Plánování nasazení každého virtuálního počítače

Jakmile jsou zmapovány požadavky na komunikaci a síť, je čas začít přemýšlet o vytvoření virtuálních počítačů. Dobrý přístup je vybrat server a provést inventarizaci:

- S čím server komunikuje?
- Které porty jsou otevřené?
- Který operační systém se používá?
- Kolik místa na disku se používá?
- Jaký typ dat se používá? Existují omezení (zákonná nebo jiná) zakazující jiné než místní umístění?
- Jaký má server procesor, paměť a vstupně-výstupní zatížení disku? Vyskytuje se nárazový provoz?

Poté můžeme odpovídat na otázky, které bude mít Azure ohledně nového virtuálního počítače.

### <a name="name-the-vm"></a>Název virtuálního počítače

Jedna z informací, o které lidé často příliš nepřemýšlí, je **název** virtuálního počítače. Název virtuálního počítače se používá jako název počítače, který je nakonfigurovaný jako součást operačního systému. U virtuálního počítače s Windows můžete zadat název dlouhý až 15 znaků a u virtuálního počítače s Linuxem až 64 znaků.

Tento název také definuje **prostředek Azure** vhodný ke správě a není jednoduché jej později změnit. To znamená, že byste měli volit smysluplné a konzistentní názvy, abyste mohli snadno identifikovat, co je úlohou virtuálního počítače. Dobrou konvencí je zahrnout do názvu následující informace:

| Prvek | Příklad | Poznámky |
| --- | --- | --- |
| Prostředí |dev (pro vývoj), prod (pro výrobu), QA (pro kontrolu kvality) |Identifikuje prostředí prostředku. |
| Umístění |uw (USA – západ), ue (USA – východ) |Identifikuje oblast, do které se prostředek nasazuje. |
| Instance |01, 02 |Pro prostředky, které mají více než jednu pojmenovanou instanci (webové servery atd.). |
| Produkt nebo služba |service |Identifikuje produkt, aplikaci nebo službu, kterou prostředek podporuje. |
| Role |sql, web, messaging (pro zasílání zpráv) |Identifikuje roli přidruženého prostředku. | 

Například `devusc-webvm01` může představovat první vývojový webový server, který je hostovaný v lokalitě Střed USA – jih. 

#### <a name="what-is-an-azure-resource"></a>Co je prostředek Azure?

**Prostředek Azure** je položka spravovaná v Azure. Stejně jako fyzický počítač v datacentru i virtuální počítače mají různé prvky potřebné k tomu, aby fungovaly:

- Samotný virtuální počítač
- Účet úložiště pro disky
- Virtuální síť (sdílená s dalšími virtuálními počítači a službami)
- Síťové rozhraní ke komunikaci v síti
- Skupiny zabezpečení sítě k zabezpečení provozu sítě
- Veřejná internetová adresa (volitelné)

Azure v případě potřeby všechny tyto prostředky vytvoří nebo můžete v průběhu nasazení zadat stávající prostředky. Každý prostředek musí mít název, který se použije k jeho identifikaci. Pokud prostředek vytvoří Azure, použije ke generování názvu prostředku název virtuálního počítače, a to je další důvod, proč být při pojmenovávání virtuálních počítačů velmi konzistentní.

### <a name="decide-the-location-for-the-vm"></a>Rozhodnutí umístění prostředků virtuálního počítače

Azure má datová centra se servery a disky po celém světě. Tato datová centra jsou seskupená do geografických _oblastí_ (Západní USA, Severní Evropa, Jihovýchodní Asie atd.), aby byla zajištěna redundance a dostupnost.

Při vytváření a nasazování virtuálního počítače musíte vybrat oblast, kam chcete prostředky (procesor, úložiště atd.) přidělit. To umožňuje umístit virtuální počítače co nejblíže uživatelům, což zlepší výkon a současně zajistí dodržování předpisů a splnění všech právních a daňových požadavků.

Dvě věci, které je dobré mít na paměti při volbě umístění. Zaprvé, umístění může omezovat vaše dostupné možnosti. V každé oblasti je k dispozici různá úroveň hardwaru a některé konfigurace nejsou k dispozici ve všech oblastech. Za druhé, mezi umístěními jsou značné cenové rozdíly. Pokud nejsou vaše úlohy vázané na konkrétní umístění, můžete ušetřit tím, že si prověříte ceny potřebné konfigurace ve více oblastech a najdete nejnižší cenu.

### <a name="determine-the-size-of-the-vm"></a>Určení velikosti virtuálního počítače

Jakmile budete mít nastaven název a umístění virtuálního počítače, je potřeba rozhodnout o jeho velikosti. Místo určení výpočetního výkonu, paměti a kapacity úložiště nezávisle na sobě nabízí Azure různé _velikosti virtuálních počítačů_, které nabízejí varianty těchto prvků v různých velikostech. Azure nabízí virtuální počítače různých velikostí, abyste si mohli vybrat správnou kombinaci výpočetního výkonu, paměti a úložiště podle požadované činnosti.

Když chcete správně určit velikost virtuálního počítače, je nejlepší vzít v úvahu typ úloh, které na něm poběží. Na základě typu úloh budete moct vybrat z podmnožiny dostupných velikostí virtuálních počítačů. Možnosti typů úloh jsou v Azure klasifikovány následovně:

| Možnost              | Popis |
|---------------------|-------------|
| **Obecné účely** | Virtuální počítače pro obecné účely jsou navržené tak, aby měly vyvážený poměr procesoru a paměti. Tato možnost je ideální pro testování a vývoj, malé až střední databáze a webové servery s nízkým až středním provozem. |
| **Optimalizované pro výpočty** | Virtuální počítače optimalizované pro výpočty jsou navržené tak, aby měly vysoký poměr procesoru k paměti. Možnost vhodná pro webové servery se středním provozem, síťová zařízení, dávkové procesy a aplikační servery. |
| **Optimalizované pro paměť** | Virtuální počítače optimalizované pro paměť jsou navržené tak, aby měly vysoký poměr paměti k procesoru. Jsou velmi vhodné pro servery s relační databází, střední a velké mezipaměti a analýzu v paměti. |
| **Optimalizované pro úložiště** | Virtuální počítače optimalizované pro úložiště jsou navržené tak, aby měly vysokou propustnost disku a vstupně-výstupních operací. Jsou ideální pro virtuální počítače s databázemi. |
| **GPU** | Virtuální počítače GPU jsou specializované virtuální počítače určené pro vykreslování náročné grafiky a úpravy videa. Tyto virtuální počítače jsou ideální možností pro trénování modelů a odvozování s hloubkovým učením. |
| **Vysokovýkonné výpočetní prostředí** | Vysokovýkonné výpočetní prostředí je možnost virtuálního počítače s nejrychlejším a nejvýkonnějším procesorem a volitelnou vysokou propustností síťového rozhraní. |

Při konfiguraci virtuálního počítače v Azure budete moct filtrovat podle typu úloh. Vybraná velikost přímo ovlivní náklady za služby. Čím výkonnější procesor, paměť nebo GPU budete potřebovat, tím vyšší bude cena.

### <a name="what-if-my-size-needs-change"></a>Co když se moje potřeby s ohledem na velikost mění?

Azure vám umožní změnit velikost virtuálního počítače, pokud již stávající velikost nevyhovuje vašim potřebám. Virtuální počítač můžete upgradovat nebo downgradovat – pokud vaše aktuální konfigurace hardwaru odpovídá nové velikosti. Tím získáte zcela agilní a pružný přístup ke správě virtuálních počítačů.

Velikost virtuálního počítače můžete měnit, i když virtuální počítač běží, za předpokladu, že je nová velikost k dispozici v aktuálním clusteru hardwaru, na kterém je virtuální počítač spuštěný. Portál Microsoft Azure vám usnadňuje rozhodování tím, že zobrazuje pouze dostupné velikosti. Pokud se pokusíte změnit velikost virtuálního počítače na nedostupnou velikost, nástroje příkazového řádku ohlásí chybu. Při změně velikosti spuštěného virtuálního počítače dojde automaticky k restartování počítače za účelem dokončení žádosti.

Pokud zastavíte a uvolníte virtuální počítač, můžete poté vybrat libovolnou velikost, která je k dispozici ve vaší oblasti, jelikož dojde k odebrání vašeho virtuálního počítače z clusteru, na kterém byl spuštěn.

> [!WARNING]
> Při změně velikosti produkčních virtuálních počítačů buďte opatrní – dojde k jejich automatickému restartování, což může způsobit dočasný výpadek a změnit některá nastavení konfigurace, jako je například IP adresa.

### <a name="understanding-the-pricing-model"></a>Porozumění principům cenového modelu

Za každý virtuální počítač se budou k předplatnému účtovat dvě samostatné ceny: výpočetní výkon a úložiště. Pokud tyto náklady oddělíte, můžete je škálovat nezávisle a platit jen za to, co potřebujete.

**Náklady na výpočetní výkon** – výpočetní náklady jsou naceněny hodinově, ale fakturují se po minutách. Pokud tedy bude virtuální počítač nasazený 55 minut, bude vám účtováno pouze 55 minut. Za výpočetní kapacitu nebudete platit, pokud zastavíte a uvolníte virtuální počítač, protože tím uvolníte hardware. Hodinová sazba se s velikostí virtuálního počítače a operačního systému liší. Náklady na virtuální počítač obsahují i platbu za operační systém Windows. Instance založené na Linuxu jsou levnější, protože se vám neúčtují licenční poplatky za operační systém.

> [!TIP]
> Je možné ušetřit opětovným použitím stávajících licencí pro Windows se **zvýhodněným hybridním využitím Azure**.

**Náklady na úložiště** – úložiště, které virtuální počítač používá, se vám bude účtovat samostatně. Stav virtuálního počítače nijak nesouvisí s poplatky za úložiště využité disky, které vzniknou i v případě, že je virtuální počítač zastaven/uvolněn a za spuštěný virtuální počítač se nic neúčtuje.

Náklady na výpočetní prostředky budete moct platit dvěma způsoby.

| Možnost | Popis |
|--------|-------------|
| **Průběžné platby** | Při **průběžných platbách** platíte za výpočetní kapacitu po sekundách, bez dlouhodobých závazků a plateb předem. Výpočetní kapacitu můžete kdykoliv na vyžádání zvýšit nebo snížit. Službu také můžete kdykoliv spustit nebo zastavit. Tuto možnost doporučujeme, pokud máte aplikace s krátkodobým nebo nepředvídatelným zatížením, kde nesmí dojít k přerušení. Tato možnost se hodí, když chcete na virtuálním počítači něco rychle otestovat nebo ho používáte k vývoji aplikace. |
| **Rezervované instance virtuálního počítače** | Možnost rezervovaných instancí virtuálního počítače (RI) představuje koupi virtuálního počítače na jeden nebo tři roky dopředu v určité oblasti. Závazek vzniká předem a na oplátku uspoříte až 72 % z ceny ve srovnání s průběžnými platbami. **Rezervované instance** jsou flexibilní a dají se snadno změnit nebo vrátit za poplatek za předčasné ukončení. Tuto možnost doporučujeme v případě, kdy musí být virtuální počítač spuštěn nepřetržitě nebo je potřeba mít předvídatelný rozpočet **a** můžete se zavázat k využívání virtuálního počítače po dobu nejméně jednoho roku. |

### <a name="storage-for-the-vm"></a>Úložiště virtuálního počítače

Všechny virtuální počítače Azure budou mít alespoň dva virtuální pevné disky (VHD). První disk obsahuje operační systém a druhý se používá jako dočasné úložiště. Další disky můžete přidat k ukládání aplikačních dat. Maximální počet disků je určen výběrem velikosti virtuálního počítače (obvykle dva na jeden procesor). Často se vytváří jeden nebo více datových disků. Je to hlavně kvůli tomu, že disk s operačním systémem je poměrně malý. Když data rozdělíte na různé virtuální pevné disky, můžete nezávisle spravovat zabezpečení, spolehlivost a výkon disku.

Data každého virtuálního pevného disku se nacházejí ve službě **Azure Storage** jako objekty blob stránky, které umožňují Azure přidělovat prostor jenom pro používané úložiště. Záleží také na tom, jak se měří náklady na úložiště. Platíte jenom za úložiště, které využíváte.

#### <a name="what-is-azure-storage"></a>Co je Azure Storage?

Azure Storage je datové úložiště Microsoftu založené na cloudu. Podporuje téměř libovolný typ dat a poskytuje zabezpečení, redundanci a škálovatelný přístup k uloženým datům. Účet úložiště poskytuje přístup k objektům ve službě Azure Storage pro konkrétní předplatné. Virtuální počítače mají vždy jeden nebo více účtů úložiště pro uložení každého připojeného virtuálního disku.

Virtuální disky můžou být zálohovány účty úložiště **Standard** nebo **Premium**. Služba Azure Premium Storage využívá disky SSD, aby poskytla vysoký výkon a nízkou latenci pro virtuální počítače, které mají intenzivní vstupně-výstupní operace. Použijte Azure Premium Storage pro produkční úlohy, zejména pro ty, které jsou citlivé na změny výkonu nebo jsou náročné na vstupy a výstupy. Úložiště úrovně Standard je dostačující pro vývoj a testování.

Při vytváření disků budete mít dvě možnosti správy vztahu mezi účtem úložiště a jednotlivými virtuálními pevnými disky. Zvolit můžete buď **nespravované disky**, nebo **spravované disky**.

| Možnost | Popis |
|--------|-------------|
| **Nespravované disky** | S nespravovanými disky jste zodpovědní za účty úložiště, které se používají k uložení virtuálních pevných disků odpovídajících diskům virtuálních počítačů. Poplatky za účet úložiště odpovídají velikosti používaného místa. Jeden účet úložiště má pevný rychlostní limit 20 000 vstupně-výstupních operací za sekundu. To znamená, že je schopný zajistit podporu pro 40 plně využívaných standardních virtuálních pevných disků. Pokud potřebujete zvýšit počet disků, budete potřebovat více účtů úložiště, což může být složité. |
| **Spravované disky** | Spravované disky představují **novější a doporučený model diskového úložiště**. Elegantně řeší tyto komplikace tím, že problémy se správou účtů úložiště nechají na Azure. Po zadání velikosti disku (až do 4 TB) Azure vytvoří a bude spravovat disk _a zároveň_ úložiště. Nemusíte se obávat o omezení účtu úložiště, což u spravovaných disků usnadňuje horizontální navýšení kapacity. |

### <a name="select-an-operating-system"></a>Výběr operačního systému

Azure nabízí různé image operačního systému, které můžete nainstalovat do virtuálního počítače, včetně několika verzí Windows a distribucí Linuxu. Jak už bylo řečeno, volba operačního systému ovlivní hodinové sazby výpočetních prostředků, protože v ceně Azure jsou i náklady na licenci operačního systému.

Pokud hledáte sofistikovanější image operačního systému než jsou ty základní, můžete prohledat instalační image na Azure Marketplace, kde najdete operační systémy a oblíbené softwarové nástroje nainstalované pro konkrétní scénáře. Pokud například potřebujete nový web WordPress, obsahovala by sada standardních technologií server s Linuxem, webový server Apache, databázi MySQL a PHP. Místo nastavování a konfigurace jednotlivých komponent můžete využít image z Marketplace a nainstalovat celou sadu najednou.

Pokud tam nenajdete vhodnou image operačního systému, můžete si vytvořit vlastní image disku s potřebnými součástmi, nahrát ji do úložiště Azure a pak ji použít k vytvoření virtuálního počítače Azure. Nezapomeňte, že Azure podporuje jenom 64bitové operační systémy.
