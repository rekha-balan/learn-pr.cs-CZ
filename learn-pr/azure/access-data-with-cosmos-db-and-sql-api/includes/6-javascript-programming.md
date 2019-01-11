V databázi často potřebujete aktualizovat více dokumentů současně. 

Když třeba v online aplikaci pro maloobchodní prodej uživatel vystaví objednávku a chce použít kód kupónu, kredit nebo bonus (případně všechno najednou), potřebujete zadat dotaz na tyto možnosti v účtu zákazníka, aktualizovat informace v účtu o použití těchto možností, aktualizovat celkovou částku objednávky a objednávku zpracovat.

Všechny tyto akce musí proběhnout současně v rámci jediné transakce. Pokud se uživatel rozhodne objednávku zrušit, budete chtít vrátit změny zpět, a neměnit informace o účtu, aby kódy kupónů, kredity a bonusy mohly být využity k dalším nákupům.

V Azure Cosmos DB se k těmto transakcím používají uložené procedury a funkce definované uživatelem (UDF). Uložené procedury představují jediný způsob, jak zajistit dělitelnost, jednotnost, oddělenost a trvanlivost transakcí (ACID), protože transakce se spouštějí na serveru, a proto se označují jako programování na straně serveru. Funkce definované uživatelem se také ukládají na serveru. Používají se v dotazech k uplatnění výpočetní logiky u hodnot nebo dokumentů v dotazu. 

V tomto modulu se seznámíte s uloženými procedurami a funkcemi definovanými uživatelem a pak některé z nich spustíte na portálu.

## <a name="stored-procedure-basics"></a>Základy uložených procedur

Uložené procedury provádějí složité transakce s dokumenty a vlastnostmi. Uložené procedury jsou napsané v Javascriptu a uložené v kolekci v Azure Cosmos DB. Spuštěním uložených procedur na databázovém stroji v blízkosti dat dosáhnete lepšího výkonu, než kdyby byly naprogramované na straně klienta.

Uložené procedury představují jediný způsob, jak v Azure Cosmos DB dosáhnout atomických transakcí, protože klientské sady SDK transakce nepodporují.

V uložených procedurách se také doporučuje provádět dávkové operace, protože se tím zmenší potřeba vytvářet samostatné transakce.

## <a name="stored-procedure-example"></a>Příklad uložené procedury

V následující ukázce je jednoduchá uložená procedura HelloWorld, která získá aktuální kontext a odešle odpověď, která zobrazí „Hello, World“.

```javascript
function helloWorld() {
    var context = getContext();
    var response = context.getResponse();

    response.setBody("Hello, World");
}
```

## <a name="user-defined-function-basics"></a>Základy funkcí definovaných uživatelem

Funkce definované uživatelem (UDF) rozšiřují gramatiku dotazovacího jazyka SQL v Azure Cosmos DB tím, že u vlastností a dokumentů implementují vlastní obchodní logiku, například výpočty. Funkce definované uživatelem mohou být volány jenom z dotazů. Na rozdíl od uložených procedur nemají přístup ke kontextovému objektu, takže nemohou číst dokumenty ani do nich zapisovat.

Ve scénáři online obchodu můžeme UDF použít ke stanovení daně z přidané hodnoty, která se vypočte z celkové hodnoty objednávky, nebo k určení procenta slevy použité u produktů nebo objednávek.

## <a name="user-defined-function-example"></a>Příklad funkce definované uživatelem

V následujícím příkladu vytvoříme funkci definovanou uživatelem, která ve fiktivní společnosti na základě nákladů na produkt vypočítá daň z produktu:

```javascript
function producttax(price) {
    if (price == undefined) 
        throw 'no input';

    var amount = parseFloat(price);

    if (amount < 1000) 
        return amount * 0.1;
    else if (amount < 10000) 
        return amount * 0.2;
    else
        return amount * 0.4;
}
```

## <a name="create-a-stored-procedure-in-the-portal"></a>Vytvoření uložené procedury na portálu

Pojďme na portálu vytvořit novou uloženou proceduru. Portál automaticky nabídne jednoduchou uloženou proceduru, která načte první položku kolekce. Napřed si tuto proceduru spustíme.

1. V Průzkumníku dat klikněte na **New Stored Procedure** (Nová uložená procedura).

    V Průzkumníku dat se zobrazí nová karta s ukázkovou uloženou procedurou.

2. Do pole **Stored Procedure ID** (ID uložené procedury) zadejte název *sample*, klikněte na **Save** (Uložit) a pak klikněte na **Execute** (Spustit).


3. Do pole **Input parameters** (Vstupní parametry) zadejte název klíče oddílu *33218896* a klikněte na **Execute** (Spustit). Všimněte si, že uložené procedury fungují v jednom oddílu.

    ![Spuštění uložené procedury na portálu](../media/6-stored-procedure.gif)

    V podokně **výsledků** se zobrazí informační kanál z prvního dokumentu v kolekci.

## <a name="create-a-stored-procedure-that-creates-documents"></a>Vytvoření uložené procedury, která vytváří dokumenty

Teď pojďme vytvořit uloženou proceduru, která vytváří dokumenty.

1. V Průzkumníku dat klikněte na **New Stored Procedure** (Nová uložená procedura). Pojmenujte tuto uloženou proceduru *createMyDocument*, zkopírujte a vložte následující kód do pole **Tělo uložené procedury** klikněte na **Uložit** a pak klikněte na tlačítko **Provést**.

    ```javascript
    function createMyDocument() {
        var context = getContext();
        var collection = context.getCollection();

        var doc = {
            "id": "3",
            "productId": "33218898",
            "description": "Contoso microfleece zip-up jacket",
            "price": "44.99"
        };

        var accepted = collection.createDocument(collection.getSelfLink(),
            doc,
            function (err, documentCreated) {
                if (err) throw new Error('Error' + err.message);
                context.getResponse().setBody(documentCreated)
            });
        if (!accepted) return;
    }
    ```

2. Do pole Vstupní parametry zadejte hodnotu klíče oddílu *33218898* a pak klikněte na **Spustit**.

    V Průzkumníku dat v oblasti výsledků se zobrazí nově vytvořený dokument.

    Můžete přejít zpět na kartu Dokumenty, kliknout na tlačítko **Aktualizovat** a podívat se nový dokument. 

## <a name="create-a-user-defined-function"></a>Vytvoření funkce definované uživatelem

Teď v Průzkumníku dat vytvoříme funkci definovanou uživatelem.

V Průzkumníku dat klikněte na **New UDF** (Nová funkce definovaná uživatelem). Budete muset klikněte na šipku dolů vedle položky **Nový uložený postup** a zobrazit **Nová UDF**. Zkopírujte do okna následující kód, pojmenujte funkci definovanou uživatelem *producttax* a pak klikněte na tlačítko **Uložit**.

```javascript
function producttax(price) {
    if (price == undefined) 
        throw 'no input';

    var amount = parseFloat(price);

    if (amount < 1000) 
        return amount * 0.1;
    else if (amount < 10000) 
        return amount * 0.2;
    else
        return amount * 0.4;
}
```

Po definování funkce definované uživatelem přejděte na kartu **Dotaz 1** a zkopírujte a vložte následující dotaz do oblasti dotazů ke spouštění funkce definované uživatelem.

```sql
SELECT c.id, c.productId, c.price, udf.producttax(c.price) AS producttax FROM c
```
