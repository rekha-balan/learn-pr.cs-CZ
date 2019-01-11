Kliničtí lékaři v Lamna Healthcare mají jen velmi malou toleranci k výpadkům. Potřebují mít neustálý přístup ke klinickým IT systémům, aby měli jistotu, že v kterémkoli okamžiku poskytují péči nejvyšší kvality. Aby se vyhovělo požadavku lékařů na trvalou dostupnost, musí být aplikace Lamna Healthcare schopné zvládat selhání s minimálním dopadem na uživatele. Jak mohou zajistit, aby jejich aplikace zůstaly provozuschopné, a to jak v případě místních incidentů, tak i v případě rozsáhlých katastrofických událostí?

V tomto tématu se podíváme na to, jak do návrhu architektury zabudovat dostupnost a obnovitelnost.

## <a name="availability-and-recoverability"></a>Dostupnost a obnovitelnost

U složitějších aplikací se může zvrtnout úplně cokoli a v libovolném měřítku. Můžou selhat jednotlivé servery nebo pevné disky. Problém s nasazením by mohl nechtěně vést k odstranění všech tabulek v databázi. Celá datacentra nemusí být dostupná. Mohlo by dojít k napadení ransomwarem, který by vám mohl zlomyslně zašifrovat všechna vaše data.

Navrhování s ohledem na *dostupnost* se zaměřuje na udržování provozuschopnosti během místních incidentů a dočasných situací, jako jsou částečné výpadky sítě. Zvládnutí místních selhání můžete zajistit integrováním vysoké dostupnosti do jednotlivých komponent aplikace a eliminováním kritických prvků způsobujících selhání. Takový návrh také minimalizuje dopad údržby infrastruktury. Návrhy s důrazem na vysokou dostupnost obvykle cílí na rychlou a automatickou eliminaci dopadu incidentů, aby byl jejich vliv minimální nebo žádný a aby se v systému zajistilo plynulé zpracovávání žádostí.

Návrh zohledňující *obnovitelnost* se zaměřuje na zotavení ze ztráty dat a z katastrofických událostí většího měřítka. Obnovení z těchto typů incidentů s sebou obvykle nese aktivní zásah, přestože postupy pro automatické obnovení mohou dobu potřebnou k obnovení zkrátit. Tyto typy incidentů mohou vést k určitým výpadkům nebo k trvalé ztrátě dat. Zotavení po havárii je nejen otázkou pečlivého plánování, ale také otázkou provedení takového plánu.

Zahrnutí vysoké dostupnosti a obnovitelnosti do návrhu architektury chrání vaši firmu před finančními ztrátami vyplývajícími z výpadků a ztráty dat. Zajišťuje také, že kvůli ztrátě důvěryhodnosti neutrpí vaše dobrá pověst u zákazníků.

## <a name="architecting-for-availability-and-recoverability"></a>Návrh architektury s ohledem na zajištění dostupnosti a obnovitelnosti

Návrh architektury s ohledem na zajištění dostupnosti a obnovitelnosti zajistí, že v rámci aplikace dokážete plnit svoje závazky vůči zákazníkům.

Pro zajištění dostupnosti identifikujte smlouvu o úrovni služeb (SLA), k jejímuž dodržování jste se zavázali. Zkontrolujte potenciální možnosti vysoké dostupnosti aplikace vzhledem k smlouvě SLA a zjistěte, kde máte odpovídající pokrytí a kde bude potřeba něco vylepšit. Cílem je přidat komponentám architektury redundanci, aby se snížila pravděpodobnost výpadku. Příkladem komponent s návrhem vysoké dostupnosti je třeba clustering a vyrovnávání zatížení. Clustering nahrazuje jeden virtuální počítač sadou koordinovaných virtuálních počítačů. Pokud jeden virtuální počítač selže nebo je nedostupný, služby převezme jiný, který žádosti dokáže obsluhovat. Vyrovnávání zatížení distribuuje žádosti na jednotlivé instance služby, detekuje instance, které selhaly, a zabraňuje tomu, aby se na ně požadavky směrovaly.

Z hlediska obnovitelnosti proveďte analýzu možných scénářů ztráty dat a závažných výpadků. Součástí analýzy by u každého z těchto scénářů mělo být prozkoumání strategií zotavení a kompromisů z hlediska efektivity nákladů. Toto cvičení vám poskytne důležitý přehled o prioritách vaší organizace a pomůže vám vyjasnit si roli vaší aplikace. Výsledky by měly zahrnovat cíl bodu obnovení (RPO) a plánovanou dobu obnovení (RTO) vaší aplikace.

* **Cíl bodu obnovení:** Maximální přijatelná doba trvání ztráty dat. RPO se měří v jednotkách času, ne objemu, například: „30 minut dat“, „čtyři hodiny dat“ atd. Účelem RPO je omezování *ztráty* dat a zotavení z ní, *krádež* dat neřeší.
* **Plánovaná doba obnovení (RTO):** Maximální přijatelná doba trvání výpadku, kde „výpadek“ je potřeba definovat ve specifikaci. Pokud je třeba v případě havárie přijatelná doba trvání výpadku 8 hodin, pak je plánovaná doba obnovení 8 hodin.

Po definování cíle bodu obnovení a plánované doby obnovení můžete pro svou architekturu navrhnout funkce zálohování, obnovení, replikace a zotavení tak, aby tyto cíle byly splněny.

Každý poskytovatel cloudových služeb nabízí sadu služeb a funkcí, které vám pomůžou zlepšit dostupnost a obnovitelnost aplikace. Pokud je to možné, použijte existující služby a osvědčené postupy a zkuste odolat pokušení vytvářet si svoje vlastní.

Můžou selhat pevné disky, datová centra nemusí být dostupná a může dojít k útoku hackerů. Je důležité si pomocí dostupnosti a obnovitelnosti zachovat dobrou reputaci u zákazníků. Dostupnost se zaměřuje na zachování provozuschopnosti během situací, jako jsou výpadky sítě, a obnovitelnost se zaměřuje na obnovení dat po haváriích.
