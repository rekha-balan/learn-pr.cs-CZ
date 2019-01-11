Pomocí dvou dokumentů, které jste přidali do databáze jako cíl našich dotazů, si projdeme základy dotazů. Azure Cosmos DB používá k provedení dotazovacích operací dotazy SQL, stejně jako SQL Server. Všechny vlastnosti se standardně automaticky indexují, takže všechna data v databázi jsou okamžitě k dispozici pro dotazy.

## <a name="sql-query-basics"></a>Základy dotazů SQL
Každý dotaz SQL se skládá z klauzule SELECT a volitelně obsahuje klauzule FROM a WHERE. Kromě toho můžete k upřesnění požadovaných informací přidat další klauzule, jako ORDER BY a JOIN.

Dotaz SQL má následující formát:

    SELECT <select_list>
    [FROM <optional_from_specification>]
    [WHERE <optional_filter_condition>]
    [ORDER BY <optional_sort_specification>]
    [JOIN <optional_join_specification>]

## <a name="select-clause"></a>Klauzule SELECT

Klauzule SELECT určuje typ hodnot, které se budou generovat při spuštění dotazu. Hodnota `SELECT *` označuje, že se vrátí celý dokument JSON.

**Dotaz**
```
SELECT *
FROM Products p
WHERE p.id ="1"
```

**Vrací**
```
[
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "price": "14.99",
        "shipping": {
            "weight": 1,
            "dimensions": {
                "width": 6,
                "height": 8,
                "depth": 1
            }
        },
        "_rid": "iAEeANrzNAAJAAAAAAAAAA==",
        "_self": "dbs/iAEeAA==/colls/iAEeANrzNAA=/docs/iAEeANrzNAAJAAAAAAAAAA==/",
        "_etag": "\"00003a02-0000-0000-0000-5b9208440000\"",
        "_attachments": "attachments/",
        "_ts": 1536297028
    }
]
```

Nebo můžete omezit výstup tak, aby zahrnoval pouze některé vlastnosti, zadáním seznamu vlastností do klauzule SELECT. Následující dotaz vrátí pouze ID, výrobce a popisu produktu.

**Dotaz**
```
SELECT 
    p.id, 
    p.manufacturer, 
    p.description
FROM Products p
WHERE p.id ="1"
```

**Vrací**
```
[
    {
        "id": "1",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt"
    }
]
```

## <a name="from-clause"></a>Klauzule FROM

Klauzule FROM určuje zdroj dat, pro který se dotaz provádí. Můžete jako zdroj dotazu použít celou kolekci, nebo můžete místo toho určit jen podmnožinu kolekce. Klauzule FROM je nepovinná, pokud zdroj není filtrovaný nebo projektovaný v dotazu později.

Dotaz, jako je `SELECT * FROM Products`, označuje, že celá kolekce Products tvoří zdroj, u kterého se má provést výčet dotazu.

Kolekce může mít alias, jako například `SELECT p.id FROM Products AS p` nebo jednoduše `SELECT p.id FROM Products p`, kde `p` je ekvivalentem `Products`. `AS` je nepovinné klíčové slovo, kterým přidělujete alias identifikátoru.

Pokud je jednou alias přidělen, nemůže se použít původní zdroj. Například zápis `SELECT Products.id FROM Products p` je syntakticky neplatný, protože identifikátor „Products“ už nejde vyhodnotit.

Všechny vlastnosti, na které se musí odkazovat, musí být plně kvalifikované. V případě chybějícího přísného dodržování schématu je toto vynuceno, aby nedocházelo k nejednoznačným vazbám. Proto je zápis `SELECT id FROM Products p` syntakticky neplatný, protože vlastnost `id` není svázána.

### <a name="subdocuments-in-a-from-clause"></a>Dílčí dokumenty v klauzuli FROM
Zdroj je také možné redukovat na menší podmnožinu. Například pokud chcete získat výčet jenom z podstromu v jednotlivých dokumentech, může se zdrojem stát dílčí kořen, jak je znázorněno v následujícím příkladu:

**Dotaz**
```
SELECT * 
FROM Products.shipping
```

**Výsledky**  

```
[
    {
        "weight": 1,
        "dimensions": {
            "width": 6,
            "height": 8,
            "depth": 1
        }
    },
    {
        "weight": 2,
        "dimensions": {
            "width": 8,
            "height": 11,
            "depth": 3
        }
    }
]
```

I když výše uvedený příklad používá jako zdroj pole, je možné jako zdroj použít také objekt, což ukazuje následující příklad. Libovolná platná hodnota JSON (tj. která není nedefinovaná), kterou lze najít ve zdroji, se bere v úvahu pro zahrnutí do výsledku dotazu. Pokud některé produkty nemají hodnotu `shipping.weight`, jsou z výsledku dotazu vyloučené.

**Dotaz**

    SELECT * 
    FROM Products.shipping.weight

**Výsledky**

    [
        1,
        2
    ]

## <a name="where-clause"></a>Klauzule WHERE
Klauzule WHERE určuje podmínky, které musí dokumenty JSON poskytované zdrojem splňovat, aby byly zahrnuté jako součást výsledku. Aby byl nějaký dokument JSON brán v úvahu při zařazení do výsledku, musí se zadané podmínky vyhodnotit na **true**. Klauzule WHERE je nepovinná.

Následující dotaz požaduje dokumenty, které obsahují ID, jehož hodnota je 1:

**Dotaz**

    SELECT p.description
    FROM Products p 
    WHERE p.id = "1"

**Výsledky**

    [
        {
            "description": "Quick dry crew neck t-shirt"
        }
    ]

## <a name="order-by-clause"></a>Klauzule ORDER BY

Klauzule ORDER BY umožňuje řazení výsledků ve vzestupném nebo sestupném pořadí.

Následující dotaz ORDER BY vrátí cenu, popis a ID produktu pro všechny produkty, seřazené vzestupně podle ceny:

**Dotaz**

    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC

**Výsledky**

```
[
    {
        "price": "14.99",
        "description": "Quick dry crew neck t-shirt",
        "productId": "33218896"
    },
    {
        "price": "49.99",
        "description": "Black wool pea-coat",
        "productId": "33218897"
    }
]
```

## <a name="join-clause"></a>Klauzule JOIN

Klauzule JOIN umožňuje provádět vnitřní spojení s dokumentem a dílčími kořeny dokumentu. Proto v databázi produktů můžete například připojit dokumenty s daty o doručení.  

V následujícím dotazu se vrátí ID produktů pro každý produkt, který má způsob doručení. Pokud jste přidali třetí produkt, který nemá vlastnost shipping, bude výsledek stejný, protože třetí položka bude vyloučena z důvodu chybějící vlastnosti shipping.

**Dotaz**

    SELECT p.productId
    FROM Products p
    JOIN p.shipping

**Výsledky**

    [
        {
            "productId": "33218896"
        },
        {
            "productId": "33218897"
        }
    ]

## <a name="geospatial-queries"></a>Geoprostorové dotazy

Geoprostorové dotazy umožňují provádět prostorové dotazy pomocí bodů GeoJSON. Pomocí souřadnic v databázi můžete vypočítat vzdálenost mezi dvěma body a zjistit, zda jsou bod, mnohoúhelník nebo lomená čára obsaženy v jiném bodu, mnohoúhelníku nebo lomené čáře.

Pro data katalogu produktů by to umožnilo vašim uživatelům zadat informace o jejich poloze a určit, zda v okolí 50 kilometrů jsou nějaké obchody, které mají hledané položky. 

## <a name="summary"></a>Shrnutí

Schopnost rychle se dotazovat na všechna vaše data po jejich přidání do Azure Cosmos DB a použití známých technik dotazů SQL může pomoct vám a vašim zákazníkům prozkoumávat uložená data a získávat o nich přehledy.