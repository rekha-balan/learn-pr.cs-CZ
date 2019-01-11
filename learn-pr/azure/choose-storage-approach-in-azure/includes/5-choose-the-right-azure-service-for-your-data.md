Výběr správného řešení úložiště může zlepšit výkon, ušetřit náklady a zlepšit možnosti správy. Tady uplatníte to, co jste se o datech naučili ve scénáři online obchodu, a pro každou sadu dat najdete tu nejlepší službu Azure. 

## <a name="product-catalog-data"></a>Data v katalogu produktů

**Klasifikace dat:** Částečně strukturovaná, protože potřebujeme mít možnost rozšířit nebo upravit schéma pro nové produkty.

**Operace:**

- Zákazníci vyžadují velký počet operací čtení spolu s možností dotazování na řadu polí v rámci databáze.
- Firma zase vyžaduje velký počet operací zápisu pro sledování stále se měnícího stavu skladových zásob.

**Latence a propustnost:** Vysoká propustnost a nízká latence

**Podpora transakcí:** Požadovaná

### <a name="recommended-service-azure-cosmos-db"></a>Doporučená služba: Azure Cosmos DB

Služba Azure Cosmos DB je navržená tak, aby podporovala částečně strukturovaná data i data NoSQL. Podpora nových polí, jako je třeba pole „S funkcí Bluetooth“, nebo jakýchkoli jiných nových polí, která budou zapotřebí, je s Azure Cosmos DB samozřejmost.

Azure Cosmos DB podporuje dotazy v jazyce SQL a každá vlastnost se standardně indexuje. Můžete vytvářet dotazy, které vašim zákazníkům umožní filtrovat katalog podle jakékoli vlastnosti.

Azure Cosmos DB také splňuje vlastnosti ACID, takže si můžete být jistí, že vaše transakce probíhají v souladu s těmito striktními požadavky.

A navíc vám Azure Cosmos DB umožní replikovat vaše data kdekoli na světě, a to pouhým kliknutím na tlačítko. Pokud má tedy váš internetový obchod zákazníky v USA, Francii a Anglii, můžete svá data replikovat do datacenter v těchto umístěních. Sníží se tak latence, protože se data fyzicky přesunou blíže k vašim uživatelům. 

I když se data replikují po celém světě, můžete si zvolit jednu z pěti úrovní konzistence. Výběrem správné úrovně konzistence můžete určit požadovaný kompromis mezi konzistencí, dostupností, latencí a propustností. Kapacitu můžete vertikálně navýšit a pokrýt tak vyšší poptávku zákazníků během nákupní špičky a potom ji v méně exponovaném období zase z důvodu úspory finančních prostředků snížit.

### <a name="why-not-other-azure-services"></a>Proč nevyužít jiné služby Azure?

Pro tuto sadu dat by byla skvělou volbou služba Azure SQL Database, kdyby nebylo potřeba ad hoc rozšiřovat schéma pro nové produkty. Ve službě Azure SQL Database musí veškerá data odpovídat schématu. Azure SQL Database může poskytnout řadu stejných výhod jako Azure Cosmos DB, ale nedokáže zpracovat heterogenní data. 

Existují i jiné služby Azure, které dokážou ukládat data NoSQL, jako například Azure Table Storage, Azure HBase jako součást HDInsight a Azure Cache for Redis. V tomto scénáři se uživatelé budou chtít dotazovat na více polí, proto je vhodnější Azure Cosmos DB. Azure Cosmos DB ve výchozím nastavení indexuje všechna pole, zatímco ostatní služby jsou omezené daty v indexu a dotazování neindexovaných polí má za následek snížení výkonu.

## <a name="photos-and-videos"></a>Fotky a videa

**Klasifikace dat:** Nestrukturovaná

**Operace:**

- Stačí načítat jen podle ID.
- Zákazníci vyžadují velký počet operací čtení s nízkou latencí.
- Vytváření a aktualizace nebudou probíhat tak často a je u nich možná vyšší latence než u operací čtení.

**Latence a propustnost:** Načtení podle ID musí podporovat nízkou latenci a vysokou propustnost. U vytváření a aktualizací je možná vyšší latence než u operací čtení.

**Podpora transakcí:** Nepožadovaná

### <a name="recommended-service-azure-blob-storage"></a>Doporučená služba: Azure Blob Storage

Azure Blob Storage podporuje ukládání souborů, jako jsou fotky a videa. Spolupracuje také se sítí Azure Content Delivery Network (CDN) a uchovává nejčastěji používaný obsah uložený v mezipaměti na hraničních serverech. Azure CDN snižuje latenci při poskytování těchto obrázků uživatelům.

Díky použití služby Azure Blob Storage můžete také přesouvat obrázky z horké úrovně úložiště do studené nebo archivní úrovně, snížit tak náklady a zaměřit propustnost na nejčastěji zobrazované obrázky a videa.

### <a name="why-not-other-azure-services"></a>Proč nevyužít jiné služby Azure?

Mohli byste obrázky nahrát do služby Azure App Service, což by znamenalo, že by obrázky poskytoval stejný server, na kterém běží vaše aplikace. Toto řešení by fungovalo v případě, že byste neměli mnoho souborů. Pokud ale máte velké množství souborů a zákazníky po celém světě, získáte lepší výsledky využitím služby Azure Blob Storage spolu s Azure CDN.

## <a name="business-data"></a>Firemní data

**Klasifikace dat:** Strukturovaná

**Operace:** Složité analytické dotazy jen pro čtení napříč řadou databází

**Latence a propustnost:** Vzhledem ke složité povaze dotazů se u výsledků počítá s určitou latencí.

**Podpora transakcí:** Požadovaná

### <a name="recommended-service-azure-sql-database"></a>Doporučená služba: Azure SQL Database

Dotazování budou u firemních dat nejspíše provádět firemní analytici, a dotazovací jazyk, který s největší pravděpodobností znají, je SQL. Azure SQL Database může sloužit jako řešení samostatně, ale když tuto službu spárujete se službou Azure Analysis Services, umožníte datovým analytikům vytvořit sémantický model pro data ve službě SQL Database. Datoví analytici ho pak můžou sdílet s firemními uživateli a těm bude stačit pouze připojit se k modelu z jakéhokoli nástroje BI a můžou okamžitě začít prozkoumávat data a získávat přehledy. 

### <a name="why-not-other-azure-services"></a>Proč nevyužít jiné služby Azure?

Azure SQL Data Warehouse podporuje řešení OLAP a dotazy SQL. Firemní analytici ale budou potřebovat provádět mezidatabázové dotazy, což SQL Data Warehouse nepodporuje.

Kromě služby Azure SQL Database můžete použít i službu Azure Analysis Services. V SQL jsou ale firemní analytici mnohem zběhlejší než v práci s Power BI. Proto by chtěli databázi, která podporuje dotazy SQL, což není případ služby Azure Analysis Services. Kromě toho finanční data, která ukládáte ve firemní sadě dat, jsou ze své podstaty relační a multidimenzionální. Azure Analysis Services podporuje tabulková data uložená v samotné službě, ale nikoli multidimenzionální data. Pro analýzu multidimenzionálních dat v Azure Analysis Services můžete použít přímý dotaz na službu SQL Database.

Azure Stream Analytics je skvělý způsob, jak analyzovat data a vyvozovat z nich závěry pro další kroky, zaměřuje se ale na streamovaná data v reálném čase. V tomto scénáři zkoumají firemní analytici pouze historická data.

## <a name="summary"></a>Shrnutí

Každý typ dat má jiné požadavky na úložiště a vaším úkolem je najít pro každý z nich to nejlepší řešení. Vždy berte v úvahu typ dat, požadované operace, očekávanou latenci a nutnost podpory transakcí.