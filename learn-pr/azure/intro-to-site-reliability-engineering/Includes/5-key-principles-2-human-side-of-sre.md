Úspěšný provozní proces, který dosáhne požadované spolehlivosti a dokáže ji udržet, závisí na tom, jak zacházíme s počítači, a zároveň ve stejné míře na tom, jak zacházíme s lidmi zodpovědnými za dané prostředí. SRE toto uznává různými způsoby, které jsou pro její praxi zásadní.

## <a name="toil"></a>Lopota

Nejprve se zaměřme na pojem „lopota“. V kontextu SRE se pod pojmem lopota rozumí pracovní činnosti prováděné člověkem, které mají určité charakteristiky. Lopota nepřináší dlouhodobou hodnotu, která by přinášela uspokojení. Žádným smysluplným způsobem neposouvá službu vpřed. Často je to repetitivní a převážně manuální činnost (i když by mohla být automatizovaná). S tím, jak služba nebo systémy časem rostou, zvýší se pravděpodobně poměrně s tím také počet požadavků na daný systém a budou vyžadovat ještě více manuální práce.

Pokud například služba vyžaduje, aby tým SRE něco každý týden resetoval, aby ručně přidělil nové účty a místo na disku, nebo je opakovaně ručně restartoval, jde o provozní zátěž, která je lopotou. Provádění těchto úkonů službu dlouhodobě trvale nijak nezlepšuje. Tyto úkony budou pravděpodobně muset být opakovány stále dokola.

> [!NOTE]
> Dokonce i tehdy, pokud si takové požadavky uchováváte v nějaké podobě lístku, jak je tomu na mnoha místech, provést úkon a vyřešit lístek je stále lopota. Je to jen dobře sledovaná lopota.

SRE nesnáší lopotu. Usiluje o její eliminaci, kdykoli je to možné a vhodné. Jde o jedna z oblastí, kde v SRE vstupuje do hry automatizace. Pokud je možné tyto požadavky zpracovávat automaticky, uvolní to týmu ruce pro uspokojivější práci na hodnotnějších věcech, než je odbavování fronty požadavků.

Je třeba poznamenat, že použití slova „vhodné“ je zde podobné jako jeho použití ohledně spolehlivosti. Existují situace, kdy má práce na eliminaci lopoty nižší prioritou než jiná práce, ale celkově je eliminace lopoty ze služby v SRE klíčem k úspěchu.

## <a name="project-work-vs-reactive-ops-work"></a>Práce na projektu vs. práce na provozu

Pokud chcete udělat práci potřebnou k eliminaci lopoty nebo ke zlepšení spolehlivosti systému, je potřeba rozdělit čas pracovníků SRE takovým způsobem, že nebudou trávit veškerý svůj čas hašením požárů v provozu, reagováním na požadavky nebo vyřizováním fronty lístků. Musí mít čas vyčleněný na to, aby psali kód k eliminaci lopoty, samoobslužné automatizaci, aby lístky nebyly potřeba, nebo vytváření projektů, které zvýší efektivitu služby a pracovníků. Obvykle citované číslo (které pochází z původního modelu Google) je nezatěžovat tým provozními záležitostmi víc než 50 % pracovního času.

> [!NOTE]
> 50 % je číslo trochu vypálené od boku, ale v praxi se ukazuje pro mnoho uživatelů jako přiměřený cíl.

Existují chvíle, kdy musí pracovníci SRE veškerý čas hašení požárů v provozu, ale nesmí to být standard. Pokud práce týmu na provozních akcích (většina z toho je lopota) zabírá více než 50 % jeho času po delší dobu, je to nejlepší cesta k vyhoření a špatné spolehlivosti. V takové nemůžou cykly zlepšování, o kterých jsme dřív hovořili, fungovat nebo vůbec vzniknout. SRE podobně klade důraz na špatně vyváženou zátěž pracovní pohotovosti, která má také potenciál silného negativního dopadu na tým.

Teď, když jsme měli možnost se seznámit s některými z hlavních postupů a principů SRE, si můžeme povědět něco o tom, jak začít.
