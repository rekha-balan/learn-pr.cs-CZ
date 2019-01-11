Velmi podobně jako u místních počítačů může být často výkon virtuálního počítače přímo svázán s tím, jak rychle dokáže číst a zapisovat data. Abychom pochopili, jak je tento výkon možné zvýšit, musíme nejprve porozumět tomu, jak se měří a jaká nastavení a možnosti ho ovlivňují.

Podíváme se konkrétně na základní disky a úložiště používané pro virtuální počítače. Když sledujete výkon, je třeba mít na paměti, že je nutné vzít v úvahu i aplikační vrstvu. Pokud například na virtuálním počítači provozujete databázi, bude vhodné se podívat, jestli je nastavení výkonu specifické pro databázi optimalizované pro používaný virtuální počítač a úložiště.

Začněme tím, že si definujeme několik pojmů a řekneme si, jaké záruky v příslušných oblastech nabízí Azure.

## <a name="io-operations-per-second"></a>Vstupně-výstupní operace za sekundu

Rychlost vašich disků je určena typem úložiště, který si vyberete (Standard nebo Premium). Tento výkon měříme jako vstupně-výstupní operace za sekundu neboli IOPS.

IOPS uvádí počet žádostí, které disk dokáže zpracovat za sekundu. Každá žádost představuje operaci čtení nebo zápisu. Tato veličina se vztahuje přímo na úložiště. Pokud například máte disk, který zvládne **5 000 IOPS**, znamená to, že je teoreticky schopen zpracovat 5 000 operací čtení a zápisu za sekundu.

Hodnota IOPS přímo ovlivňuje výkon aplikace. Některé aplikace, jako jsou maloobchodní weby, potřebují vysokou hodnotu IOPS, aby zvládly všechny ty drobné a náhodné vstupně-výstupní žádosti, které je potřeba rychle zpracovat, aby web rychle reagoval.

### <a name="iops-in-azure"></a>IOPS v Azure

Když ke svému virtuálnímu počítači s vysokou kapacitou připojíte disk úložiště úrovně Premium, Azure zřídí zaručenou hodnotu IOPS podle specifikace disku. Například disk **P50** zřídí **7 500 IOPS**. Každý virtuální počítač s vysokou kapacitou má také konkrétní limit IOPS, který dokáže plnit. Například virtuální počítač **Standard GS5** má limit **80 000 IOPS**.

IOPS je veličina používaná u disků úložiště, ale jde o _teoretický_ limit – na skutečný výkon aplikace mají vliv dva další faktory: **propustnost** a **latence**.

### <a name="what-is-throughput"></a>Co je propustnost?
Propustnost (také označovaná jako „šířka pásma“) je množství dat, které vaše aplikace odesílá na disky úložiště v zadaných intervalech (obvykle za sekundu). Pokud aplikace provádí vstupně-výstupní operace s velkými bloky dat, vyžaduje vysokou propustnost.

Azure zřizuje propustnost na discích úložiště úrovně Premium na základě jejich specifikace. Například disk **P50** zřídí propustnost **250 MB za sekundu**. Každý virtuální počítač s vysokou kapacitou má také konkrétní _limit propustnosti_, který dokáže plnit. Například virtuální počítač **Standard GS5** má maximální propustnost **2000 MB za sekundu**.

#### <a name="iops-vs-throughput"></a>IOPS versus propustnost

Hodnoty pro propustnost a IOPS mají mezi sebou přímý vztah – změna jedné bude mít přímý vliv na druhou. Teoretický limit propustnosti zjistíte pomocí tohoto vzorce: `IOPS x I/O size = throughput`. Při plánování vaší aplikace je důležité vzít v úvahu obě tyto hodnoty.

### <a name="what-is-latency"></a>Co je latence?

Čtení a zápis dat trvá určitou dobu. A právě tady mluvíme o _latenci_. Latence je doba, kterou aplikaci zabere odeslání žádosti na disk a získání odpovědi. V podstatě nám latence říká, jak dlouho trvá _zpracování_ jedné vstupně-výstupní žádosti o čtení nebo zápis.

Latence představuje omezení pro IOPS. Pokud například disk dokáže zpracovat 5000 IOPS, ale zpracování každé operace trvá 10 ms, pak bude aplikace kvůli době zpracování omezená na 100 operací za sekundu. Toto je jednoduchý příklad – většinou bude časová latence mnohem nižší. V konečném důsledku bude rychlost, s jakou dokáže aplikace zpracovávat data z úložiště, určena latencí a propustností. 

Premium Storage poskytuje konsistentně nízkou latenci a ještě nižší latence můžete v případě potřeby dosáhnout prostřednictvím _ukládání do mezipaměti_. 

## <a name="testing-your-disk-performance"></a>Testování výkonu disků

IOPS, propustnost a latenci disků virtuálního počítače můžete upravit a vyvážit tím, že si vyberete správnou velikost virtuálního počítače a správný typ úložiště. Virtuální počítače s větší nebo dražší velikostí budou obvykle nabízet vyšší záruky pro maximální IOPS a propustnost. Do této rovnice přidejte ještě volbu mezi úložištěm Standard a Premium a mezi pevným diskem a diskem SSD a máte hned několik parametrů, se kterými si můžete pohrát.

Při výběru správné kombinace je třeba pochopit, jaké jsou požadavky vaší aplikace. Aplikace s vysokým objemem vstupně-výstupních operací, jako jsou databázové servery nebo systémy pro online zpracování transakcí, budou vyžadovat vyšší IOPS, zatímco u spíše výpočetních aplikací můžou být tyto požadavky mnohem nižší. Kromě toho budou mít na propustnost vliv _typy_ operací, které aplikace provádějí. Vstupně-výstupní operace s vysoce náhodnými přístupy mají tendenci být pomalejší než dlouhá sekvenční čtení.

Jakmile vyberete konfiguraci, můžete pomocí nástrojů, jako je [Iometer](http://iometer.org/), otestovat výkon disků na virtuálních počítačích s Windows a Linuxem. Tím získáte reálnější představu o tom, jaký výkon můžete očekávat. Tímto způsobem můžete také zjistit, jak vylepšit využívání úložiště ze strany aplikace. Vezměme si například aplikaci, která provádí jednovláknové vstupně-výstupní operace. U ní existuje větší pravděpodobnost sníženého vstupně-výstupního výkonu kvůli latenci.

Podívejme se na některé další možnosti, jak vylepšit výkon disků.

