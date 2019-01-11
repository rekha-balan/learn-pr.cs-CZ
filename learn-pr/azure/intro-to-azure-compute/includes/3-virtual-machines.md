:::row:::
  :::column:::
    ![Obrázek představující virtuální počítač Azure](../media/3-azure-vms.png)
  :::column-end:::
  :::column span="3":::
Služba Azure Virtual Machines umožňuje vytváření a používání virtuálních počítačů v cloudu. Poskytuje infrastrukturu jako službu (IaaS) ve formě virtualizovaného serveru. Lze ji použít mnoha různými způsoby. Veškerý software spuštěný na virtuálním počítači můžete přizpůsobovat – stejně jako na fyzickém počítači. Díky tomu je to ideální volba, pokud potřebujete úplnou kontrolu nad operačním systémem nebo chcete používat vlastní software nebo konfigurace hostování.
  :::column-end:::
:::row-end:::

Nový virtuální počítač se obvykle dá vytvořit a zřídit během několika minut výběrem image předem nakonfigurovaného virtuálního počítače. Při vytváření takového počítače je výběr image jedním z nejdůležitějších rozhodnutí, které je potřeba učinit. Image představuje šablonu sloužící k vytvoření virtuálního počítače. Tyto šablony už obsahují operační systém a často i další software, jako jsou nástroje pro vývoj nebo prostředí pro hostování webů.

## <a name="moving-to-the-cloud-with-vms"></a>Přechod na cloud prostřednictvím virtuálních počítačů

Virtuální počítače jsou také vynikající volba při přechodu z fyzického serveru na cloud (metodou „lift and shift“). Můžete vytvořit kopii fyzického serveru a hostovat ji na virtuálním počítači, a to s minimem změn nebo úplně bez nich. Stejně jako u fyzického místního počítače budete muset i u virtuálního počítače zajistit údržbu. To znamená provádění aktualizací operačního systému a jeho softwaru. 

## <a name="scaling-vms-in-azure"></a>Škálování virtuálních počítačů v Azure

Můžete provozovat jednotlivé virtuální počítače pro testování, vývoj či menší úlohy nebo virtuální počítače seskupit pro zajištění vysoké dostupnosti, škálovatelnosti a redundance. Azure nabízí několik speciálních funkcí pro naplnění těchto zásadních požadavků, takže ať už jsou vaše potřeby v oblasti provozuschopnosti jakékoli, Azure je dokáže splnit.

- Skupiny dostupnosti
- Škálovací sady virtuálních počítačů
- Azure Batch

### <a name="what-are-availability-sets"></a>Co jsou skupiny dostupnosti?

**Skupina dostupnosti** je logické seskupení dvou nebo více virtuálních počítačů, které zajišťují dostupnost vaší aplikace během plánované i neplánované údržby.

_Událost plánované údržby_ nastává, když Microsoft zahájí aktualizaci základních prostředků infrastruktury Azure, která je hostitelem virtuálních počítačů. Cílem takové aktualizace je oprava chyb zabezpečení, zlepšení výkonu a přidání nebo aktualizace funkcí. Ve většině případů je možné tyto aktualizace provést bez jakéhokoli dopadu na hostující virtuální počítače – ale někdy aktualizace vyžadují restart. Pokud je virtuální počítač součástí skupiny dostupnosti, prostředky infrastruktury Azure zajistí, že se aktualizace budou provádět postupně, takže se všechny přidružené virtuální počítače nebudou restartovat najednou. Toto seskupení se označuje jako _aktualizační doména_. Aktualizační domény jsou logickou součástí každého datacentra a implementují se společně se softwarem a logikou.

> [!IMPORTANT]
> Azure plánovanou údržbu nijak neoznamuje.

_Události neplánované údržby_ souvisí se selháním hardwaru v datacentru, například s výpadkem napájení nebo selháním disku. Virtuální počítače, které jsou součástí skupiny dostupnosti, se automaticky přepínají na fungující fyzický server, aby se jejich běh nepřerušil. Tato skupina se označuje jako _doména selhání_. Doména selhání je v podstatě rack serverů. Zajišťuje fyzické rozdělení dané úlohy mezi různé hardwarové prostředky v datacentru. Ty zahrnují napájecí, chladicí a síťový hardware, který podporuje fyzické servery umístěné v serverových rackových skříních. V případě, že se hardware serverového racku stane nedostupným, výpadek ovlivní pouze příslušný rack.

Azure vytváří 2 domény selhání (2 racky, každý z nichž má vyhrazené napájecí a síťové prostředky) a 5 logických aktualizačních domén. Vaše virtuální počítače se pak postupně rozdělí mezi vytvořené domény, jak je znázorněno zde.

![Skupiny dostupnosti v Azure se znázorněním aktualizačních domén a domén selhání, které jsou napříč servery duplikované](../media/3-availability-sets.png)

Za skupinu dostupnosti se neúčtují žádné poplatky – platíte jen za virtuální počítače v ní. Důrazně doporučujeme zařazení každé úlohy do skupiny dostupnosti, abyste ve své architektuře virtuálních počítačů vyloučili existenci jediného bodu selhání.

### <a name="what-are-virtual-machine-scale-sets"></a>Co jsou škálovací sady virtuálních počítačů?

Škálovací sady virtuálních počítačů Azure umožňují vytvářet a spravovat skupiny identických virtuálních počítačů s vyrovnáváním zatížení. Představte si, že používáte web, který vědcům umožňuje nahrávání astronomických snímků určených ke zpracování. Pokud byste virtuální počítač duplikovali, za normálních okolností byste museli nakonfigurovat další službu, která by směrovala žádosti mezi různými instancemi tohoto webu. A to je přesně to, co za vás budou dělat škálovací sady virtuálních počítačů.

Škálovací sady umožňují centrální správu, konfiguraci a aktualizaci velkého počtu virtuálních počítačů pro zajištění vysoce dostupných aplikací, a to v řádu minut. Počet instancí virtuálních počítačů se může automaticky zvyšovat nebo snižovat v reakci na poptávku nebo podle definovaného plánu. S využitím škálovacích sad virtuálních počítačů můžete sestavovat rozsáhlé služby v oblastech, jako jsou výpočty, velké objemy dat a kontejnerové úlohy.

### <a name="what-is-azure-batch"></a>Co je Azure Batch?

Azure Batch umožňuje plánování úloh a správu výpočetního výkonu ve velkém rozsahu s možností škálovat desítky, stovky i tisíce virtuálních počítačů. 

Když zahájíte úlohu, služba Batch automaticky spustí fond výpočetních virtuálních počítačů, přičemž nainstaluje aplikace a připraví data, spustí potřebný počet úloh, identifikuje případná selhání a znovu úlohy zařadí do fronty a po dokončení práce fond používaných prostředků opět zmenší. 

Někdy se může stát, že potřebujete obrovský výpočetní výkon v „surové“ podobě nebo výkon na úrovni superpočítače – Azure tyto schopnosti poskytuje.
