Velkou část kódu moderních aplikací představují knihovny a závislosti, které jste jako vývojáři zvolili. Jedná se o běžnou praxi, která šetří čas a peníze. Nevýhodou však je, že použitím tohoto kódu ve svém projektu na sebe přebíráte odpovědnost za tento kód, i když ho napsal někdo jiný. Pokud výzkumník (nebo hůře hacker) v některé z těchto knihoven třetích stran zjistí ohrožení zabezpečení, bude se stejný problém pravděpodobně vyskytovat i ve vaší aplikaci.

Používání komponent se známými ohroženími zabezpečení je v našem oboru velký problém. Tento problém je tak velký, že se dostal na [seznam deseti nejhorších](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) ohrožení zabezpečení webových aplikací podle OWASP a už několik let se na tomto seznamu drží na 9. místě.

## <a name="track-known-security-vulnerabilities"></a>Sledování známých ohrožení zabezpečení

Náš problém spočívá ve zjištění, že se objevil problém. Zajištění aktualizací knihoven a závislostí (číslo 4 na našem seznamu) samozřejmě pomůže, ale je vhodné udržovat si přehled o zjištěných ohroženích zabezpečení, které můžou aplikaci ovlivnit.

> [!IMPORTANT]
> Pokud systém obsahuje známé ohrožení zabezpečení, je mnohem pravděpodobnější, že existuje kód, který můžou lidé použít k útoku na takový systém. Pokud se kód umožňující zneužití zveřejní, je důležité všechny ovlivněné systémy okamžitě aktualizovat.

**Mitre** je nezisková organizace, která udržuje [seznam známých ohrožení zabezpečení a rizik](https://cve.mitre.org). Tento seznam je veřejně vyhledatelná sada známých ohrožení zabezpečení v aplikacích, knihovnách a architekturách. **Pokud najdete knihovnu nebo komponentu v databázi CVE, pak obsahuje známá ohrožení zabezpečení.**

Problémy odesílá bezpečnostní komunita, když se v produktu nebo komponentě najde ohrožení zabezpečení. Každému publikovanému problému se přiřadí ID a každý obsahuje datum zjištění, popis ohrožení zabezpečení a odkazy na publikovaná alternativní řešení nebo vyjádření dodavatelů k danému problému.

### <a name="how-to-verify-if-you-have-known-vulnerabilities-in-your-3rd-party-components"></a>Jak ověřit, jestli vaše komponenty třetích stran obsahují známá ohrožení zabezpečení

Můžete si do telefonu dát denní úkol zkontrolovat tento seznam, ale naštěstí pro nás existuje řada nástrojů, které nám umožňují ověřit, jestli jsou naše závislosti ohrožené. Tyto nástroje můžete spouštět proti vašemu kódu, nebo ještě lépe, přidat do kanálu CI/CD, aby automaticky vyhledávaly problémy v rámci procesu vývoje.

- [OWASP Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check), pro který existuje [modul plug-in pro Jenkinse](https://wiki.jenkins.io/display/JENKINS/OWASP+Dependency-Check+Plugin).
- [OWASP SonarQube](https://www.owasp.org/index.php/OWASP_SonarQube_Project)
- [Synk](https://snyk.io), který je zdarma pro úložiště open source na GitHubu.
- [Black Duck](https://www.blackducksoftware.com), který používá řada podniků.
- [RubySec](https://rubysec.com) – poradní databáze výhradně pro Ruby.
- [Retire.js](https://github.com/retirejs/retire.js/) – nástroj pro ověření aktuálnosti knihoven JavaScriptu, který je možné použít jako modul plug-in pro různé nástroje, včetně sady [Burp Suite](https://www.portswigger.net).

K tomuto účelu je možné použít i některé nástroje vytvořené speciálně pro analýzu statického kódu.

- [Roslyn Security Guard](https://dotnet-security-guard.github.io)
- [Puma Scan](https://pumascan.com)
- [PT Application Inspector](https://www.ptsecurity.com/ww-en/products/ai/)
- [Apache Maven Dependency Plugin](https://maven.apache.org/plugins/maven-dependency-plugin/)
- [Source Clear](https://www.sourceclear.com)
- [Sonatype](https://ossindex.sonatype.org)
- [Node Security Platform](https://nodesecurity.io)
- [WhiteSource](https://www.whitesourcesoftware.com/what-is-whitesource/)
- [Hdiv](https://hdivsecurity.com)
- [A řada dalších...](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)

Další informace o rizicích spojených s používáním ohrožených komponent najdete na [stránce OWASP](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities) věnované tomuto tématu.

## <a name="summary"></a>Shrnutí

Při používání knihoven nebo jiných komponent třetích stran v rámci vaší aplikace přijímáte také případná rizika, která můžou přinášet. Nejlepší způsob, jak toto riziko snížit, je zajistit, abyste používali pouze komponenty, se kterými nejsou spojená žádná známá ohrožení zabezpečení.
