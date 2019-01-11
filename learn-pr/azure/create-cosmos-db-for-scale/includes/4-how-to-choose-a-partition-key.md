Online obchodník, pro kterého pracujete, plánuje v blízké budoucnosti expanzi do nové geografické oblasti. Tento krok rozšíří základnu zákazníků a objemy transakcí. Vy musíte zajistit, aby databáze umožňovala rozšiřování podle potřeby.

Zavedením strategie oddílů zajistíte, že pokud vaše databáze potřebuje růst, je možné to snadno provést a pokračovat v efektivním provádění dotazů a transakcí.

## <a name="what-is-a-partition-strategy"></a>Co je strategie oddílů?

Když budete neustále přidávat data na jeden server nebo do jednoho oddílu, nakonec vám dojde volné místo. Chcete-li tomu zabránit, potřebujete strategii oddílů, která **navyšuje kapacitu horizontálně** místo vertikálně. Horizontální navýšení kapacity nebo též horizontální škálování umožňuje přidat k databázi další oddíly podle toho, jak je aplikace potřebuje.

Strategie oddílů a horizontálního škálování je ve službě Azure Cosmos DB založená na klíči oddílu, což je hodnota, která se nastavuje při vytváření kolekce. Jakmile je klíč oddílu jednou nastavený, nelze ho změnit bez opětovného vytvoření celé kolekce, takže výběr správného klíče oddílu je důležité rozhodnutí v rané fázi vývojového procesu.  

V této lekci se dozvíte, jak zvolit klíč oddílu, který je pro váš scénář nejvhodnější a který bude využívat automatické škálování služby Azure Cosmos DB.

## <a name="what-is-a-partition-key"></a>Co je klíč oddílu?

Klíč oddílu je hodnota, podle které Azure organizuje data do logických oddílů. V našem scénáři online prodejce je použití hodnoty `userID` nebo `productId` jako klíče oddílu dobrou volbou. Tato hodnota je totiž jedinečná a je pravděpodobné, že se použije k vyhledání záznamů. `userID` je dobrou volbou, protože vaše aplikace často potřebuje načítat osobní nastavení, nákupní košík, historii objednávek a profilové informace uživatelů, jen tak namátkou. `productId` se zase hodí, když je potřeba zodpovídat dotazy na stav zásob, prodejní cenu, barevné varianty, umístění ve skladu a další informace.

Klíč oddílu by měl co nejrovnoměrněji distribuovat provoz do všech oddílů databáze. Cílem rovnoměrné distribuce požadavků je zabránit přetížení některých oddílů. Takové oddíly (nazývané také horké oddíly) budou vyřizovat mnohem víc požadavků než jiné, což může vytvořit kritický bod propustnosti. Například v aplikaci elektronického obchodu by špatně zvoleným klíčem oddílu byl aktuální čas, protože pak by všechna příchozí data končila v jednom oddílu. Hodnota `userID` nebo `productId` by byly lepší, protože všichni uživatelé na vašem webu budou pravděpodobně pracovat se svým košíkem a profilovými informacemi s podobnou frekvencí a tím se čtení a zápisy rovnoměrně rozdělí mezi všechny oddíly uživatelů a produktů.

Úložný prostor pro data spojená s každým klíčem oddílu nesmí přesáhnout 10 GB, což je velikost jednoho fyzického oddílu ve službě Azure Cosmos DB. Pokud by tedy některý záznam `userID` nebo `productId` byl větší než 10 GB, zvažte, za nepoužit složené klíče, aby jednotlivé záznamy byly vždy menší. Příkladem složeného klíče může být `userID-date`, který by vypadal například jako **jméno_zákazníka-08072018**. Složené klíče umožňují vytvořit nový oddíl pokaždé, když v určitém dni zákazník navštíví web.

## <a name="best-practices"></a>Osvědčené postupy

Když se snažíte správně zvolit klíč oddílu a řešení není zřejmé, vezměte v úvahu následující tipy.

- Nemusíte se bát zvolit klíč oddílu, který má velký počet hodnot. Čím víc hodnot má váš klíč oddílů, tím máte i lepší škálovatelnost.
- Při volbě nejlepšího klíče oddílu pro úlohy skutečně náročné na čtení se zaměřte na tři až pět nejčastějších dotazů, které budete používat. Hodnota nejčastěji zahrnutá v klauzuli WHERE je nejvhodnějším kandidátem na klíč oddílu.
- Pro úlohy náročné na zápis bude nutné pochopení transakčních potřeb vaší úlohy, protože klíč oddílu tvoří rozsah transakcí s více dokumenty.
