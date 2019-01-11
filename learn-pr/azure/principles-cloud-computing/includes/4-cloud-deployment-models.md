Existují tři různé modely nasazení cloudu. Model nasazení cloudu definuje, kde jsou data uložená a jak s nimi zákazníci pracují – jak k nim získávají přístup a kde se aplikace spouští. Také závisí na tom, kolik vlastní infrastruktury potřebujete nebo chcete spravovat.

## <a name="explore-the-three-deployment-methods-of-cloud-computing"></a>Prozkoumání tří metod nasazení cloud computingu

#### <a name="public-versus-private-versus-hybrid"></a>Veřejný, privátní a hybridní

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEv7]

![Ikona veřejného cloudu](../media/4-public-cloud.png)

**Veřejný cloud**

Toto je nejběžnější model nasazení. V tomto případě nemusíte místní hardware spravovat ani aktualizovat – vše běží na hardwaru poskytovatele cloudových služeb. V některých případech můžete ušetřit další náklady tím, že budete výpočetní prostředky sdílet s jinými uživateli cloudu.

Firmy můžou používat více poskytovatelů veřejného cloudu různého rozsahu. Příkladem poskytovatele veřejného cloudu je Microsoft Azure.

Některé výhody veřejného cloudu:

- Vysoká škálovatelnost/agilita – když chcete navýšit kapacitu, nemusíte kupovat nový server.
- Průběžné platby – platíte jenom za to, co využijete, žádné náklady CapEx.
- Nezodpovídáte za údržbu ani aktualizace hardwaru.
- Minimální technické znalosti k nastavení a používání – k zabezpečení a zajištění vysoké dostupnosti úloh můžete využít dovednosti a znalosti poskytovatele cloudu.

Běžný případ použití je takový, že webovou aplikaci nebo web blogu nasadíte na hardware a prostředky, jejichž vlastníkem je poskytovatel cloudu. Díky použití veřejného cloudu v tomto scénáři můžou uživatelé cloudu svůj web nebo blog rychle zprovoznit a pak se zaměřit na údržbu webu, aniž by se museli starat o zakoupení, správu a údržbu hardwaru, na kterém web poběží.

Veřejný cloud není vhodný pro všechny scénáře. Některé nevýhody, na které je třeba pamatovat:

- Při použití veřejného cloudu nemusí jít splnit některé požadavky na zabezpečení.
- Veřejné cloudy nemusí splňovat některé zásady státní správy, oborové standardy nebo právní požadavky.
- Protože nejste vlastníky hardwaru a služeb, nemůžete je vždy spravovat, jak byste si možná přáli.
- Může být obtížné splnit některé zvláštní obchodní požadavky, jako třeba udržování starších aplikací.

![Ikona privátního cloudu](../media/4-private-cloud.png)

**Privátní cloud**

V privátním cloudu vytvoříte cloudové prostředí ve vlastním datacentru a uživatelům v organizaci poskytujete samoobslužný přístup k výpočetním prostředkům. Tento model simuluje uživatelům veřejný cloud, ale za nákup a údržbu hardwaru a poskytované softwarové služby plně odpovídáte vy.

Tento přístup má několik výhod:

- Máte úplnou kontrolu nad prostředky a můžete zajistit, aby konfigurace podporovala libovolný scénář nebo starší aplikaci.
- Máte úplnou kontrolu nad zabezpečením (ale i zodpovědnost).
- Privátní cloudy můžou splňovat přísné požadavky na zabezpečení, dodržování předpisů a právní ustanovení tak, jak to u veřejných cloudů nemusí být možné.

Mezi důvody, proč se vyhnout použití privátního cloudu, patří:

- Musíte vynaložit počáteční náklady CapEx a zakoupit hardware pro rozjezd a údržbu.
- Vlastnictví vybavení omezuje agilitu – když chcete navýšit kapacitu, musíte koupit, nainstalovat a nastavit nový hardware.
- Privátní cloudy vyžadují IT dovednosti a znalosti, které není snadné získat.

Scénář vhodný pro použití privátního cloudu je třeba takový, že organizace má data, která nemůže umístit do veřejného cloudu, například z právních důvodů. Můžete mít třeba lékařská data, která nesmíte veřejně vystavit. V jiném scénáři můžou zásady státní správy vyžadovat, aby byla určitá data uchovávaná uvnitř země nebo soukromě.

Privátní cloud může poskytovat funkce cloudu i externím zákazníkům, nebo konkrétním interním oddělením, jako je účtárna nebo osobní oddělení.

![Ikona hybridního cloudu](../media/4-hybrid-cloud.png)

**Hybridní cloud**

Hybridní cloud kombinuje veřejný a privátní cloud a umožňuje provozovat aplikace v nejvhodnějším umístění. Můžete třeba hostovat web ve veřejném cloudu a propojit ho s vysoce zabezpečenou databází hostovanou v privátním cloudu (nebo místním datacentru).

To je užitečné v případě, že máte nějaké položky, které například z právních důvodů nemůžete umístit do cloudu. Můžete mít třeba určitá data, která nesmíte veřejně vystavit (například lékařská data) a která musí být uchovávaná v privátním datacentru. Dalším příkladem může být jedna nebo více aplikací běžících na starém hardwaru, který nejde aktualizovat. V takovém případě můžete nechat běžet starý systém místně a připojit ho k veřejnému cloudu určenému k ověřování nebo ukládání.

Několik výhod hybridního oproti privátnímu cloudu:

- Můžete nechat běžet systémy, které používají zastaralý hardware nebo zastaralý operační systém.
- Můžete si flexibilně vybírat mezi tím, co provozovat místně a co v cloudu.
- Když je to levnější, můžete pro služby a prostředky využívat výhod úspor z rozsahu u poskytovatelů veřejného cloudu, a když tomu tak není, můžete doplnit vlastní vybavení.
- Když potřebujete mít úplnou kontrolu nad prostředím, můžete pro splnění požadavků na zabezpečení, dodržování předpisů a scénáře se staršími verzemi použít vlastní vybavení.

Na několik věcí byste si ale měli dát pozor:

- Může to být dražší, než když si vyberete jeden model nasazení, protože to zahrnuje určité počáteční náklady CapEx.
- Může to být složitější z hlediska nastavení a správy.

## <a name="summary"></a>Shrnutí

Cloud computing je flexibilní a umožňuje vám vybrat způsob, jak ho chcete nasadit. Model nasazení cloudu, který zvolíte, závisí na rozpočtu a na potřebách z hlediska zabezpečení, škálovatelnosti a údržby.
