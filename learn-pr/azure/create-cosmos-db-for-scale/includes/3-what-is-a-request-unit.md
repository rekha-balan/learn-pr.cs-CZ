Dále se pojďme podívat na data pro vaši databázi. Odpovídající propustnost je důležitá pro zajištění, že dokážete zpracovat objem transakcí diktovaný obchodními potřebami. Požadavky na propustnost nejsou vždycky konzistentní. Můžete třeba vytvářet web internetového obchodu, který při výprodejích nebo o svátcích vyžaduje škálování. Nejprve si řekneme něco o jednotkách žádostí a o tom, jak odhadnout požadavky na propustnost.

## <a name="what-is-database-throughput"></a>Co je propustnost databáze? 

Propustnost databáze je počet čtení a zápisů, které dokáže databáze provést za jednu sekundu.

Ke strategickému škálování propustnosti je potřeba odhadnout požadavky na propustnost na základě odhadu počtu čtení a zápisů a různých velikostí dokumentů, které budete potřebovat v různých okamžicích zpracovat. Pokud odhad provedete správně, vaši uživatelé budou spokojení i v dobách největší špičky. Při nesprávném odhadu se může u požadavků projevit omezená přenosová rychlost a operace budou muset čekat a provádět opakované pokusy, což pravděpodobně povede k vysoké latenci a nespokojenosti zákazníků.

## <a name="what-is-a-request-unit"></a>Co je jednotka žádosti?

Azure Cosmos DB měří propustnost v takzvaných **jednotkách žádosti (RU)**. Využití jednotek žádosti se měří po sekundách, takže výslednou měrnou jednotkou je **počet jednotek žádostí za sekundu (RU/s)**. Počet RU/s, které chcete zřídit ve službě Azure Cosmos DB, je potřeba předem rezervovat, aby služba dokázala zpracovat odhadovanou zátěž, a počet RU/s potom můžete kdykoli škálovat nahoru nebo dolů podle aktuální potřeby.

## <a name="request-unit-basics"></a>Základní informace o jednotkách žádosti

Jedna jednotka žádosti, tedy 1 RU, přibližně odpovídá nákladům na provedení jednoho požadavku GET u dokumentu o velikosti 1 kB za použití ID dokumentu. Provádění požadavků GET za použití ID dokumentu představuje efektivní způsob načtení dokumentu s malými náklady. Vytvoření, náhrada nebo odstranění stejné položky vyžaduje od služby další zpracování, takže spotřebuje víc jednotek žádosti.

Počet jednotek žádosti spotřebovaných při operaci se mění podle velikosti dokumentu, počtu vlastností v dokumentu, prováděné operace a dalších složitých faktorů, jako je konzistence a zásady indexování.

Následující tabulka uvádí počet jednotek žádosti nutných pro položky tří různých velikostí (1 kB, 4 kB a 64 kB) a při dvou různých úrovní výkonu (500 čtení za sekundu, 100 zápisů za sekundu a 500 čtení za sekundu, 500 zápisů za sekundu). V tomto příkladu je konzistence dat nastavená na možnost **Relace** a zásady indexování na možnost **Žádné**.

| Velikost položky | Čtení za sekundu | Zápisů za sekundu | Jednotky žádostí
| --- | --- | --- | --- |
| 1 kB | 500 | 100 | (500 * 1) + (100 * 5) = 1000 RU/s
| 1 kB | 500 | 500 | (500 * 1) + (500 * 5) = 3000 RU/s
| 4 kB | 500 | 100 | (500 * 1,3) + (100 * 7) = 1350 RU/s
| 4 kB | 500 | 500 | (500 * 1,3) + (500 * 7) = 4150 RU/s
| 64 kB | 500 | 100 | (500 * 10) + (100 * 48) = 9800 RU/s
| 64 kB | 500 | 500 | (500 * 10) + (500 * 48) = 29000 RU/s
 
Jak vidíte, čím větší je položka a čím více čtení a zápisů je potřeba, tím více jednotek žádosti se musí rezervovat. Když potřebujete odhadnout požadavky na propustnost aplikace, k dispozici je online nástroj [Capacity Planner](https://www.documentdb.com/capacityplanner) služby Azure Cosmos DB, který umožňuje nahrát ukázkový dokument JSON a nastavit počet operací za sekundu, které potřebujete provést. Na základě toho vám poskytne odhad celkového počtu k rezervaci.

## <a name="exceeding-throughput-limits"></a>Překročení omezení propustnosti

I když nerezervujete dostatek jednotek žádosti a pokusíte se o čtení nebo zápis více dat, než umožňuje zřízená propustnost, u vašeho požadavku se projeví omezení rychlosti. Když má požadavek omezenou rychlost, opakované pokusy můžou proběhnout až po zadaném intervalu. Pokud používáte sadu .NET SDK, opakovaný pokus požadavku automaticky proběhne po vyčkávání po dobu uvedenou v hlavičce retry-after.

## <a name="creating-an-account-built-to-scale"></a>Vytvoření účtu přizpůsobeného požadavkům na škálování

Počet jednotek žádosti zřízených v databázi můžete kdykoli změnit. V dobách zvýšeného objemu tedy můžete navýšit kapacitu, abyste vyhověli zvýšení počtu požadavků, a v klidnějších obdobích zase zřízenou propustnost snížit, aby vám klesly náklady.

Při vytváření účtu můžete na portálu zřídit minimálně 400 RU/s a maximálně 250 000 RU/s. Pokud potřebujete větší propustnost, vytvořte na webu Azure Portal lístek. Skoro pro všechny účty se doporučuje nastavit počáteční propustnost na 1000 RU/s, protože se jedná o minimální hodnotu, při které bude databáze provádět automatické škálování, pokud byste potřebovali více než 10 GB úložiště. Pokud jste nastavili počáteční propustnost na hodnotu nižší než 1000 RU/s, vaše databáze nebude moc provést škálování na více než 10 GB, pokud databázi znovu nezřídíte a nezadáte klíč oddílu. Klíče oddílů umožňují rychlé vyhledávání dat ve službě Azure Cosmos DB a automatické škálování databáze v případě potřeby. Klíče oddílů jsou v modulu diskutovány později.

## <a name="summary"></a>Shrnutí

Teď rozumíte tomu, jak odhadnout a nastavit propustnost služby Azure Cosmos DB pomocí jednotek žádostí, a při vytváření nové kolekce Azure Cosmos DB vyberete správnou hodnotu. Jednotky žádosti můžete kdykoli upravit, ale pokud tuto hodnotu nastavíte při vytváření účtu na 1 000 RU/s, pomůžete tím zajistit připravenost databáze na pozdější škálování.