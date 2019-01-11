Celý tento modul se zabýval integrací dat a služeb do vašich funkcí. Začali jsme stručným přehledem typů vazeb, které se zobrazí, když je přidáte do funkce. Potom jsme se podívali na čtení dat z databáze Azure Cosmos DB pomocí vstupní vazby. Platforma se postará o správu připojovacích řetězců, takže jsme viděli, jak snadno lze pomocí vazby v našem kódu číst data. Nakonec jsme se zaměřili na tvorbu různých zdrojů dat s pomocí výstupních vazeb. Celý náš postup shrnuje následující tabulka:

[!INCLUDE [summary table](./summary-table.md)]

Postupy, které jste se tady naučili, můžete použít k přidávání a testování vazeb ve vašich funkcích. Tady je několik zajímavých nápadů pro získání dalších praktických zkušeností s vazbami a pro rozvíjení získaných poznatků.

* Vytvořte další funkci, která bude číst ze služby Blob Storage, a další vstupní vazby, které jsme nepoužili v tomto modulu.

* Vytvořte další funkci pro zápis do více cílů pomocí jiných podporovaných typů výstupních vazeb.

* V předchozí jednotce jsme zavedli frontu a publikovali do ní zprávy pomocí výstupní vazby. V dalším kroku zvažte přidání další funkce, která čte zprávy ve frontě a vytiskne **TEXT ZPRÁVY** na konzole s použitím `console.log()`.

Jak jsme viděli v tomto modulu, Azure Portal nabízí snadno použitelné funkce, abyste mohli začít vytvářet funkce a připojovat je k datům a dalším službám.

Pokud vás zajímá provádění bezserverových integrací tohoto typu s vizuálními pracovními postupy a s minimem vlastního kódu nebo zcela bez něho, podívejte se na Azure Logic Apps.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>Další zdroje

Tento seznam sice nemá být vyčerpávající, ale obsahuje několik zdrojů informací souvisejících s tématy probíranými v tomto modulu, které by vás mohly zajímat:

* [Dokumentace ke službě Azure Functions](https://docs.microsoft.com/azure/azure-functions/)
* [Výzva Azure Functions](https://aka.ms/afc)
* [Podrobný návod k bezserverové architektuře Azure](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)
* [Používání úložiště Queue z Node.js](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)
* [Úvod do služby Azure Cosmos DB: rozhraní SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)
* [Technický přehled služby Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)
* [Dokumentace ke službě Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
