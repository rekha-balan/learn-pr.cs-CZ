Mnozí vývojáři mají za to, že o architekturách a knihovnách, pomocí kterých vyvíjejí svůj software, se primárně rozhodují podle funkcí nebo osobních preferencí. Architektura, kterou zvolíte, ale představuje důležité rozhodnutí nejen z hlediska návrhu a funkčnosti, ale také z hlediska _zabezpečení_. Volba architektury, která má moderní funkce zabezpečení a je udržovaná v aktualizovaném stavu, je jedním z nejlepších způsobů, jak zajistit zabezpečení aplikací.

## <a name="choose-your-framework-carefully"></a>Architekturu zvolte pečlivě

Při výběru architektury je nejdůležitějším faktorem ohledně zabezpečení úroveň její podpory. Nejlepší architektury mají stanovené bezpečnostní postupy a jsou podporované velkými komunitami, které je vylepšují a testují. Žádný software není 100% bezchybný ani zcela neprůstřelný, při zjištění chyby ale chceme mít jistotu, že bude rychle opravena nebo nabídnuto alternativní řešení.

Pojem „dobře podporovaná“ je často synonymem pojmu „moderní“. Starší architektury jsou většinou buď nahrazeny, nebo postupně ztrácejí na popularitě. Přestože máte bohaté zkušenosti s některou starší architekturou (nebo máte aplikace v ní vytvořené), měli byste raději zvolit moderní knihovnu obsahující funkce, které potřebujete. Moderní architektury stavějí na poznatcích získaných ze starších verzí, takže když je zvolíte pro své nové aplikace, omezíte tím prostor pro hrozby. Pokud se ve starší architektuře, kterou jste použili k vytvoření svých dřívějších aplikací, objeví nějaká chyba, budete mít na starost o jednu aplikaci více.

Další informace o zabezpečeném návrhu a snížení množství hrozeb najdete v tématu [Navrhování s ohledem na zajištění zabezpečení v Azure](../../design-for-security-in-azure/index.yml).

## <a name="keep-your-framework-updated"></a>Udržujte architekturu v aktualizovaném stavu

Architektury pro vývoj softwaru, například Java Spring a .NET Core, vydávají pravidelně aktualizace a nové verze. Součástí těchto aktualizací jsou nové funkce, vyřazení starších funkcí a často také opravy nebo vylepšení zabezpečení. Když architekturu necháme zastarat, vznikne „technický dluh“. Čím více ji necháme zastarat, tím obtížnější a riskantnější bude převedení kódu na nejnovější verzi. Při používání starších verzí architektury se navíc vystavujete bezpečnostním hrozbám, které byly v novějších verzích architektury opraveny, což souvisí s prvotní volbou architektury.

Příkladem může být [přes 30 chyb zabezpečení](https://www.cvedetails.com/product/6117/Apache-Struts.html?vendor_id=45) objevených v letech 2016–2017 v architektuře Apache Struts. Tyto chyby byly rychle vyřešeny vývojovým týmem, ale některé firmy nenainstalovaly opravy a [zaplatily za to porušením zabezpečení dat](https://www.zdnet.com/article/equifax-confirms-apache-struts-flaw-it-failed-to-patch-was-to-blame-for-data-breach/). **Udržujte architektury a knihovny v aktualizovaném stavu**.

### <a name="how-do-i-update-my-framework"></a>Jak můžu architekturu aktualizovat?

Některé architektury, jako Java nebo .NET, vyžadují instalaci a vydávají aktualizace ve známých intervalech. Je proto vhodné sledovat nové verze a naplánovat vytvoření větve kódu, abyste ji po vydání mohli vyzkoušet. Například .NET Core udržuje [stránku se zprávou k vydání verze](https://github.com/dotnet/core/tree/master/release-notes), na které najdete nejnovější dostupné verze.

Specializovanější knihovny, například architektury JavaScript nebo komponenty .NET, lze aktualizovat pomocí správce balíčků. Oblíbenými volbami pro webové projekty jsou **NPM** a **Bower**, které podporuje většina integrovaných vývojových prostředí nebo sestavovacích nástrojů. V rozhraní .NET používáme ke správě závislostí mezi komponentami **NuGet**. Kromě aktualizace základní architektury, vytvoření větve kódu, aktualizace komponent a testování je také vhodné ověřit novou verzi závislosti.

> [!NOTE]
> Nástroj pro příkazový řádek `dotnet` má parametry `add package` a `remove package` pro přidání nebo odebrání balíčků NuGet, nemá ale odpovídající příkaz `update package`. Spuštěním příkazu `dotnet add package <package-name>` v projektu se ale balíček automaticky _upgraduje_ na nejnovější verzi. Jde o snadný způsob aktualizace závislostí bez otevření integrovaného vývojového prostředí.

## <a name="take-advantage-of-built-in-security"></a>Využijte integrované zabezpečení

Vždy zjistěte, jaké funkce zabezpečení vaše architektura nabízí. **Nikdy** nezavádějte vlastní zabezpečení, pokud je k dispozici nějaká integrovaná standardní technika nebo funkce. Kromě toho se spoléhejte na osvědčené algoritmy a pracovní postupy, protože byly často pečlivě prověřeny mnoha odborníky, posouzeny a posíleny, takže můžete mít jistotu, že jsou spolehlivé a bezpečné.

Architektura .NET Core má bezpočet funkcí zabezpečení; zde jsou některé z nich zmíněné v této dokumentaci.
* [Ověřování – správa identit](https://docs.microsoft.com/aspnet/core/security/authentication/index?view=aspnetcore-2.1)
* [Autorizace](https://docs.microsoft.com/aspnet/core/security/authorization/index?view=aspnetcore-2.1)
* [Ochrana dat](https://docs.microsoft.com/aspnet/core/security/data-protection/index?view=aspnetcore-2.1)
* [Zabezpečená konfigurace](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/index?view=aspnetcore-2.1)
* [Rozhraní API pro rozšiřitelnost zabezpečení](https://docs.microsoft.com/aspnet/core/security/data-protection/extensibility/index?view=aspnetcore-2.1)

Každá z těchto funkcí byla vytvořena odborníky ve svém oboru a důkladně otestována, aby bylo zajištěno, že funguje tak, jak má, a nedělá nic jiného. Jiné architektury nabízejí podobné funkce – u dodavatele, který danou architekturu poskytuje, můžete zjistit, co nabízí v jednotlivých kategoriích.

> [!WARNING]
> Psaní vlastních kontrol pro zabezpečení místo použití kontrol nabízených architekturou není jen plýtvání časem, ale také méně bezpečné.


## <a name="azure-security-center"></a>Azure Security Center

Při hostování webových aplikací v Azure vás Security Center na kartě s doporučeními upozorní, pokud jsou vaše architektury zastaralé.  Nezapomeňte se na tuto kartu čas od času podívat a zjistit, jestli neobsahuje nějaká upozornění související s vašimi aplikacemi.

![Azure Security Center doporučující upgrade architektury](../media/5-ASCFramework.png)


## <a name="summary"></a>Shrnutí

Kdykoli to jde, zvolte k vytváření aplikací moderní architekturu, vždy používejte integrované funkce zabezpečení a udržujte architekturu v aktualizovaném stavu. Tato jednoduchá pravidla vám pomohou zajistit, aby vaše aplikace stála na pevných základech.
