Microsoft Azure se skládá z datových center umístěných po celém světě. Při využívání služby nebo vytváření prostředku, jako je databáze SQL nebo virtuální počítač, používáte procesor a úložiště, které se nacházejí v jednom nebo několika z těchto datových center.

Konkrétní datová centra se koncovým uživatelům přímo nezveřejní, místo toho je Azure uspořádá do _oblastí_.

## <a name="what-is-a-region"></a>Co je oblast?

**Oblast** je zeměpisné území, které obsahuje nejméně jedno, ale potenciálně více datových center, která jsou v těsné blízkosti a jsou propojená prostřednictvím sítě s nízkou latencí. Azure inteligentně přiřazuje a řídí prostředky v každé oblasti a zajišťuje tak správné vyrovnávání zatížení. 

Při nasazování prostředku v Azure je často potřeba zvolit oblast, ve které chcete daný prostředek nasadit. 

> [!IMPORTANT]
> Některé služby nebo funkce virtuálních počítačů jsou dostupné jenom v některých oblastech, třeba konkrétní velikosti virtuálních počítačů nebo typy úložišť. Existují také globální služby Azure, které nevyžadují výběr konkrétní oblasti, jako je Microsoft Azure Active Directory, Microsoft Azure Traffic Manager a Azure DNS. 

Několik příkladů oblastí: *Západ USA*, *Kanada – střed*, *Západní Evropa*, *Austrálie – východ* a *Japonsko – západ*. Tady je zobrazení všech dostupných oblastí k prosinci 2018:

[ ![Mapa dostupných oblastí k prosinci 2018](../media/2b-regions-small.png) ](../media/2b-regions-large.png#lightbox)

Azure má více globálních oblastí než nabízí jakýkoli jiný poskytovatel cloudu. Poskytuje vám to flexibilitu umožňující dostat aplikace blíž k uživatelům bez ohledu na to, kde se nacházejí. Je také k dispozici lepší škálovatelnost a redundance a zachovává se rezidence dat pro vaše služby.

#### <a name="special-azure-regions"></a>Speciální oblasti Azure

Azure nabízí několik speciálních oblastí, které lze využívat při vytváření aplikací pro právní účely nebo dodržování předpisů. Mezi ně patří:

- *US DoD – střed*, *US Gov – Virginie*, *US Gov – Iowa* a další: Jedná se o fyzické a logické síťově izolované instance Azure pro partnery a úřady státní správy USA. Jsou obsluhované prověřenými občany USA. Zahrnují další certifikace dodržování předpisů.

- *Čína – východ*, *Čína – sever* a další: Tyto oblasti jsou dostupné prostřednictvím jedinečného partnerství mezi společnostmi Microsoft a 21Vianet, kdy společnost Microsoft nespravuje tato datová centra přímo.

- *Německo – střed* a *Německo – severovýchod*: Tyto oblasti jsou dostupné prostřednictvím modelu důvěryhodných uživatelů dat, kdy zákaznická data zůstanou v Německu pod kontrolou společnosti T-Systems, která je součástí skupiny Deutsche Telekom a funguje jako důvěryhodný uživatel dat pro Německo. Tuto službu může využít libovolný uživatel nebo podnik, který vyžaduje, aby jeho data byla uložená v Německu.

S oblastmi budete běžně pracovat při určování umístění pro vaše prostředky, ale měli byste mít povědomí také o těchto dvou termínech: _zeměpisné oblasti_ a _zóny dostupnosti_.

## <a name="what-is-a-geography"></a>Co je zeměpisná oblast?

*Zeměpisná oblast* je diskrétní trh, který obvykle obsahuje dvě nebo více oblastí a zachovává rezidenci dat a hranice dodržování předpisů.

Zeměpisné oblasti umožňují zákazníkům se specifickými požadavky na rezidenci dat a dodržování předpisů, aby měli svoje data a aplikace blízko. Zeměpisné oblasti zajišťují, že se v daných zeměpisných hranicích dodržují požadavky na rezidenci dat, suverenitu, dodržování předpisů a odolnost. Zeměpisné oblasti jsou odolné proti chybám a díky připojení k naší vyhrazené vysokokapacitní síťové infrastruktuře zvládnou i kompletní selhání oblasti.

Zeměpisné oblasti jsou rozdělené takto: *Amerika*, *Evropa*, *Asie a Tichomoří*, *Střední východ a Afrika*.

## <a name="what-is-an-availability-zone"></a>Co je zóna dostupnosti?

*Zóny dostupnosti* jsou fyzicky oddělená datová centra uvnitř oblasti Azure.

Každou zónu dostupnosti tvoří jedno nebo několik datových center vybavených nezávislým napájením, chlazením a sítí. Zóna dostupnosti je nastavená tak, aby byla hranicí izolace. Pokud se jedna zóna dostupnosti ocitne mimo provoz, druhá nadále funguje. Zóny dostupnosti bývají vzájemně propojené prostřednictvím velmi rychlých privátních optických sítí.

![Obrázek znázorňující tři datová centra, která jsou propojená v rámci jedné oblasti a představují zónu dostupnosti](../media/2b-availability-zones.png)

Mezi oblasti podporující zóny dostupnosti patří *Střed USA*, *Severní Evropa* a *Jihovýchodní Asie* – úplný seznam je dostupný v dokumentaci.

Zóny dostupnosti umožňují zákazníkům provozovat důležité aplikace s vysokou dostupností a replikací s nízkou latencí. Nabízejí se jako služba v Azure a kvůli zajištění odolnosti jsou ve všech povolených oblastech minimálně tři samostatné zóny.

> [!NOTE]
> Ne všechny služby podporují použití zón dostupnosti – zóny dostupnosti se používají především pro virtuální počítače, spravované disky, nástroje pro vyrovnávání zatížení a databáze SQL. Ověřte si v dokumentaci, které prvky vaší architektury můžete k zóně dostupnosti přidružit.

## <a name="what-is-a-region-pair"></a>Co je párování oblastí?

Každá oblast Azure je vždy spárovaná s jinou oblastí ve stejné zeměpisné oblasti (jako je USA, Evropa nebo Asie), která je vzdálená přinejmenším 300 mil (přibližně 483 kilometrů). Tento přístup umožňuje replikaci prostředků (jako je třeba úložiště virtuálních počítačů) napříč danou zeměpisnou oblastí, čímž se snižuje pravděpodobnost, že události jako přírodní katastrofy, občanské nepokoje, výpadky napájení nebo fyzické výpadky sítě způsobí přerušení v obou oblastech současně. 

![Obrázek znázorňující vztah mezi zeměpisnou oblastí, párováním oblastí, oblastí a datovým centrem](../media/2b-region-pairs.png)

Spárované oblasti jsou propojené přímo a jsou od sebe dostatečně vzdálené, aby byly izolovány od místních havárií, takže je lze použít k zajištění redundance služeb a dat. Některé služby dokonce poskytují geograficky redundantní úložiště pomocí spárovaných oblastí automaticky.

Mezi další výhody párování oblastí patří:

- V případě rozsáhlejšího výpadku Azure je z každého páru jedna oblast určená jako prioritní. To pomáhá zkrátit čas potřebný k jejich obnovení pro aplikace. 

- Plánované aktualizace Azure se pro spárované oblasti nasazují postupně po jedné oblasti. Tím se minimalizují prostoje a riziko výpadku aplikací. 

- Pro daňové účely a účely vymáhání zákonů jsou data nadále uložená ve stejné zeměpisné oblasti jako spárovaná oblast (s výjimkou oblasti Brazílie – jih).

Příkladem párování oblastí může být pár Západ USA a Východ USA a pár Jihovýchodní Asie a Východní Asie.

Prozkoumali jsme některé základní služby v Azure a uspořádání fyzické infrastruktury. Teď si povíme něco o využívání služeb s účtem Azure.