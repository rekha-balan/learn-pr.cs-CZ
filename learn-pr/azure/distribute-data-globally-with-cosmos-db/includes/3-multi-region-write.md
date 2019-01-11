Zajištění skvělého uživatelského prostředí ve scénáři elektronického obchodování, jako je ten váš, vyžaduje vysokou dostupnost a nízkou latenci.

Když u účtu Azure Cosmos DB, který jste vytvořili, povolíte podporu architektury multi-master, přinese to oběma těmto výkonovým aspektům řadu výhod a všechny tyto výhody jsou garantovány finančně zajištěnými smlouvami o úrovni služeb (SLA) poskytovanými službou Azure Cosmos DB.

## <a name="what-is-multi-master-support"></a>Co je podpora architektury multi-master?

Podpora architektury multi-master je možnost, která může být povolená u nových účtů Azure Cosmos DB. Když se účet replikuje do více oblastí, je každá z nich hlavní (master) oblastí, která se stejnou měrou podílí na modelu zápis–kdekoli, který je známý také jako model aktivní–aktivní.

Oblasti Azure Cosmos DB, které fungují v konfiguraci multi-master jako hlavní (master) oblasti, automaticky soustřeďují data zapisovaná do všech replik a zajišťují globální konzistenci a integritu dat.

S podporou architektury Azure Cosmos DB multi-master můžou zápisy probíhat na libovolném kontejneru v oblastech s povoleným zápisem po celém světě. Zapsaná data se okamžitě rozšíří do všech ostatních oblastí.  

## <a name="what-are-the-benefits-of-multi-master-support"></a>Jaké jsou výhody podpory architektury multi-master?

Výhody podpory architektury multi-master jsou tyto:

* Jednociferná latence zápisu – Účty multi-master mají vylepšenou latenci zápisu <10 ms pro 99 % zápisů, ta má u účtů, které nejsou typu multi-master, hodnotu <15 ms.
* 99,999% dostupnost čtení a zápisu – Dostupnost zápisu u účtů multi-master vzrostla na 99,999 % v porovnání s 99,99 % u účtů, které nejsou typu multi-master.
* Neomezená škálovatelnost a propustnost zápisu – Účty multi-master umožňují zapisovat do každé oblasti a poskytují tak neomezenou škálovatelnost a propustnost zápisu, která umožňuje podporovat miliardy zařízení.
* Integrované řešení konfliktů – Účty multi-master mají tři způsoby řešení konfliktů, které zajišťují globální integritu a konzistenci dat.

## <a name="conflict-resolution"></a>Řešení konfliktů

S podporou architektury multi-master přichází možnost vzniku konfliktů mezi zápisy do různých oblastí. Konflikty se u služby Azure Cosmos DB vyskytují zřídka. Může k nim dojít jenom v případě, že se některá položka současně změní ve více oblastech a až potom nastane šíření mezi oblastmi. Vzhledem ke globální rychlosti replikace byste konflikty neměli zaznamenat často, pokud vůbec. Služba Azure Cosmos DB přesto poskytuje režimy pro řešení konfliktů, které uživatelům umožňují rozhodnout, jak řešit situace, když různí zapisovači ve dvou nebo více oblastech současně aktualizují stejný záznam.  

Služba Azure Cosmos DB nabízí tři režimy pro řešení konfliktů:

* **Poslední zápis rozhoduje – Last-Writer-Wins (LWW).** V tomto režimu se konflikty řeší na základě hodnoty uživatelem definované celočíselné vlastnosti v dokumentu. Ve výchozím nastavení se k určení naposledy zapisovaného dokumentu používá _ts. „Poslední zápis rozhoduje“ je výchozím mechanismem pro zpracování konfliktů.
* **Vlastní – uživatelem definovaná funkce.** V tomto režimu je možné plně řídit řešení konfliktů registrací uživatelem definované funkce do kolekce. Uživatelem definovaná funkce je zvláštní typ uložené procedury se specifickým podpisem. Pokud uživatelem definovaná funkce selže nebo neexistuje, služba Azure Cosmos DB přidá všechny konflikty do kanálu konfliktů jen pro čtení, který může být zpracováván asynchronně.  
* **Vlastní – asynchronní.** V tomto režimu vyloučí služba Azure Cosmos DB všechny konflikty z potvrzování a zaregistruje je do kanálu konfliktů jen pro čtení k odloženému řešení aplikací uživatele. Tato aplikace může provádět řešení konfliktů asynchronně a používat při řešení konfliktů libovolnou logiku nebo jakýkoli externí zdroj, aplikaci nebo službu.

## <a name="summary"></a>Shrnutí

V této lekci jste se seznámili s účty multi-master, které umožňují zapisovat data do více oblastí. To vede ke zlepšené dostupnosti a výkonu.