Aby bylo možné se připojit ke zdroji dat, musíte nakonfigurovat *vstupní vazbu*. Tato vazba umožňuje vytvořit zprávu napsáním jen minimálního množství kódu. Nemusíte psát kód pro jednotlivé úkoly, jako je otevření připojení k úložišti. O tyto věci se za vás postará modul runtime a vazba Azure Functions.

## <a name="input-binding-types"></a>Typy vstupních vazeb

Existuje více typů vstupu. Ne všechny typy ale podporují jak vstup, tak výstup. Používají se, kdykoli chcete ingestovat data daného typu. Podívejme se teď na typy, které podporují vstupní vazby, a kdy je použít.

- **Úložiště objektů blob** Vazby úložiště objektů blob umožňují čtení z objektu blob.

- **Azure Cosmos DB** Vstupní vazba služby Azure Cosmos DB načítá pomocí rozhraní SQL API jeden nebo více dokumentů Azure Cosmos DB a předává je do vstupního parametru funkce. Parametry ID dokumentu nebo dotaz se dají určit podle aktivační události, která funkci volá.

- **Microsoft Graph** Vstupní vazby Microsoft Graph umožňují čtení souborů z OneDrivu, čtení dat z Excelu a získávání ověřovacích tokenů, takže budete moct interagovat s jakýmkoliv rozhraním Microsoft Graph API.

- **Mobile Apps** Vstupní vazba Mobile Apps načte záznam z koncového bodu mobilní tabulky a předá ji do vaší funkce.

- **Table Storage** Můžete číst data a pracovat s Azure Table Storage.

Pokud chcete vytvořit vazbu jako vstup, je nutné definovat `direction` jako `in`.
Parametry pro každý typ vazby se mohou lišit.

## <a name="what-is-a-binding-expression"></a>Co je vazbový výraz?

Vazbový výraz je specializovaný text v souboru **function.json**, parametrech funkce nebo kódu, který se vyhodnocuje v momentě volání funkce, aby vrátil hodnotu. Pokud máte například vazbu služby Service Bus Queue, můžete použít vazbový výraz k získání názvu fronty z nastavení aplikace.

### <a name="types-of-binding-expressions"></a>Typy vazbových výrazů

- Nastavení aplikace
- Název souboru aktivační události
- Metadata aktivační události
- Datové části JSON
- Nový GUID
- Aktuální datum a čas

Většina výrazů je označena a tím, že je zabalena ve složených závorkách. Vazbové výrazy nastavení aplikací jsou však zabaleny ve znacích procent místo ve složených závorkách. Pokud je například cesta výstupní vazby objektů blob `%Environment%/newblob.txt` a hodnota nastavení Environment aplikace je Development, vytvoří se objekt blob v kontejneru Development.

## <a name="summary"></a>Shrnutí

Vstupní vazby vám umožňují připojit si funkci ke zdroji dat. Můžete se připojovat k různým typům zdrojů dat, přičemž parametry každého z nich se liší. K překládání hodnot z různých zdrojů můžeme použít vazbové výrazy v souboru *function.json*, parametrech funkce nebo kódu.