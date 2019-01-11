Váš výzkumný tým shromáždil velké objemy obrazových dat, které by mohly vést k určitému objevu na Marsu. Potřebuje provést výpočetně náročné zpracování dat, ale nemá k dispozici potřebné vybavení. Podívejme se, proč je Azure dobrou volbou pro datovou analýzu.

## <a name="what-is-azure-compute"></a>Co je Azure Compute?

Azure Compute je výpočetní služba pro provozování cloudových aplikací dostupná na vyžádání. Prostřednictvím virtuálních počítačů a kontejnerů poskytuje výpočetní prostředky, jako jsou vícejádrové procesory a superpočítače. Kromě toho zajišťuje architekturu bez serveru, která umožňuje provozování aplikací bez nutnosti nastavení a konfigurace infrastruktury. Prostředky jsou k dispozici na vyžádání a obvykle je můžete vytvořit v řádu minut nebo i několika sekund. Přitom platíte jen za skutečně využité prostředky, a to po dobu jejich používání.

V Azure existují čtyři běžné techniky pro provádění výpočtů:

- Virtuální počítače
- Kontejnery
- Azure App Service
- Bezserverová architektura

## <a name="what-are-virtual-machines"></a>Co jsou virtuální počítače?

**Virtuální počítače** (anglická zkratka: VM) jsou softwarové emulace fyzických počítačů. Virtuální počítače obsahují virtuální procesor, paměť, úložiště a síťové prostředky. Hostují operační systém a můžete na nich instalovat a spouštět software stejně jako na fyzických počítačích. Pomocí klienta vzdálené plochy můžete virtuální počítač používat a řídit stejně, jako byste seděli před ním.

## <a name="what-are-containers"></a>Co jsou kontejnery?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

Kontejnery jsou prostředí virtualizace, která (na rozdíl od virtuálního počítače) neobsahují operační systém. Místo toho odkazují na operační systém hostujícího prostředí, v němž je kontejner spuštěný. Pokud je například na serveru s konkrétním jádrem Linuxu spuštěno pět kontejnerů, běží všechny na tomto jednom jádru.

Kontejnery zpravidla obsahují vámi vytvořenou aplikaci – a také všechny knihovny, které tato aplikace potřebuje, aby mohla běžet na jádru hostitelského prostředí.

## <a name="what-is-azure-app-service"></a>Co je Azure App Service?

Azure App Service je nabídka typu platforma jako služba (PaaS) v Azure, která je určena pro hostování webově orientovaných aplikací na podnikové úrovni. Díky údržbě infrastruktury prostřednictvím plně spravované platformy dokážete vyhovět náročným požadavkům na výkon, škálovatelnost, zabezpečení a dodržování předpisů.

#### <a name="what-is-serverless-computing"></a>Co je bezserverová architektura?

Architektura bez serveru je cloudové běhové prostředí, ve kterém se spouští váš kód, ale které má zcela abstrahované podkladové hostitelské prostředí. Vytvoříte instanci služby a přidáte svůj kód – žádná konfigurace nebo údržba infrastruktury není nutná (a dokonce ani povolená).

## <a name="which-computing-strategy-is-right-for-me"></a>Která výpočetní strategie je pro mě nejlepší?

K volbě cloudové výpočetní strategie není nutné přistupovat stylem „všechno nebo nic“. Virtuální počítače, kontejnery, App Service a bezserverová architektura mají výhody i nevýhody proti dalším možnostem.

Například u bezserverové architektury už sice není nutné spravovat infrastrukturu, očekává se u ní, že práce bude provedena rychle, obvykle během několika sekund nebo i rychleji. Proto byste mohli svojí základní aplikaci spustit na virtuálním počítači nebo kontejneru, ale přenést část zátěže ze zpracování dat na bezserverovou aplikaci.

Podívejme se na jednotlivé možnosti blíže, abyste se mohli lépe rozhodnout, kdy kterou službu používat.