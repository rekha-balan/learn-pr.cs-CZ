Blahopřejeme, dokončili jste modul týkající se výkonu a škálovatelnosti. Pojďme si tedy shrnout, čím jsme se zabývali:

## <a name="scaling-up-and-scaling-out"></a>Vertikální a horizontální navýšení kapacity

- Rozdíl mezi vertikálním zvýšením nebo snížením kapacity (úroveň prostředku zřízeného v instanci) a horizontálním zvýšením nebo snížením kapacity (zvýšení nebo snížení počtu dostupných instancí). Zabývali jsme se také jednotlivými příklady těchto možností v Azure.
- Aspekty, které je potřeba brát v úvahu při horizontálním zvýšení nebo snížení kapacity s ohledem na správu stavu a časy spuštění aplikací
- Automatické škálování a možnosti, jak může tato funkce pomoct při škálování počtu instancí na základě plánu nebo požadavků aplikace
- Vysvětlení alternativních technologií, které můžou usnadnit škálovatelnost, například prostředí bez serveru a kontejnery

## <a name="optimize-network-performance"></a>Optimalizace výkonu sítě

- Latence sítě je míra zpoždění dat posílaných od odesílatele k příjemci.
- V cloudovém prostředí se při srovnání s místním nasazením může u „upovídanějších“ aplikací projevit vliv na výkon, protože prostředky už nejsou umístěné na jednom místě vedle sebe.
- Umístění rozhraní API v blízkosti koncového bodu pro zápis do databáze výkon příznivě ovlivnit, protože se sníží latence sítě mezi těmito dvěma prostředky.
- Ke směrování uživatelů do nasazené instance s nejnižší latencí sítě lze použít Azure Traffic Manager.
- Pomocí služby Azure Content Delivery Network (CDN) lze přesměrovat výpočetní služby z hlavní aplikace a zkrátit časy načítání aplikací díky ukládání obsahu do mezipaměti na hraničním uzlu CDN v blízkosti uživatele.

## <a name="optimize-storage-performance"></a>Optimalizace výkonu úložiště

- Pro nasazení typu IaaS jsou k dispozici tři hlavní typy disků. Disky HDD úrovně Standard (nekonzistentní latence a nižší úrovně propustnosti), disky SSD úrovně Standard (konzistentní latence a nižší úrovně propustnosti) a disky SSD úrovně Premium (konzistentní latence a vysoké úrovně propustnosti).
- Aby se zkrátily časy načítání aplikace, je možné použít v aplikační vrstvě ukládání do mezipaměti. Často požadované informace lze uložit do mezipaměti před databází, což může optimalizovat časy načítání nejvíce požadovaných dat.
- Při sestavování řešení by se mělo zvážit použití vhodného back-endového úložiště dat pro danou úlohu (polyglotní použití).

## <a name="identify-performance-bottlenecks"></a>Určení kritických bodů z hlediska výkonu

- Důležitost porozumění, jaká jsou očekávání od aplikace, než začne navrhování nebo sestavování jakýchkoli operací
- Vysvětlení, jak efektivní strategie DevOps můžou pomoct vytvářet robustnější a dobře fungující aplikace
- Shrnutí možností monitorování, které jsou k dispozici v Azure, například Azure Monitor (řídicí panel pro monitorování v Azure), Azure Log Analytics (příjem protokolů a monitorování IaaS) a Application Insights (monitorování výkonu aplikací včetně dostupnosti, výkonu a informací o výjimkách)
