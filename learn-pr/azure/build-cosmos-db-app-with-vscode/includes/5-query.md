<!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Když jste vytvořili ve své aplikaci dokumenty, můžeme se na ně teď z aplikace zkusit dotázat. Služba Azure Cosmos DB používá dotazy SQL a LINQ. Tato lekce se zaměřuje na spouštění dotazů SQL a LINQ z aplikace místo z portálu

K testování těchto dotazů použijeme dokumenty uživatele, které jste vytvořili pro aplikaci online prodejce.

## <a name="linq-query-basics"></a>Základy dotazů LINQ

LINQ je programovací model rozhraní .NET, který na proudech objektů vyjadřuje výpočty jako dotazy. Můžete vytvořit objekt **IQueryable**, který se přímo dotáže služby Azure Cosmos DB, což dotaz LINQ přeloží na dotaz Cosmos DB. Dotaz se pak předá serveru služby Azure Cosmos DB, aby se ve formátu JSON načetly sady výsledků. Vrácené výsledky se na straně klienta deserializují do proudu objektů .NET. Mnoho vývojářů upřednostňuje dotazy LINQ, protože poskytují jeden konzistentní programovací model mezi prací s objekty v kódu aplikace a vyjádřením logiky dotazu spuštěné v databázi.

Následující tabulka ukazuje, jak se dotazy LINQ překládají do SQL.

| Výraz LINQ | Překlad SQL |
|---|---|
| `input.Select(family => family.parents[0].familyName);`| `SELECT VALUE f.parents[0].familyName FROM Families f` |
|`input.Select(family => family.children[0].grade + c); // c is an int variable` | `SELECT VALUE f.children[0].grade + c FROM Families f` |
|`input.Select(family => new { name = family.children[0].familyName, grade = family.children[0].grade + 3});`| `SELECT VALUE {"name":f.children[0].familyName, "grade": f.children[0].grade + 3 } FROM Families f`|
|`input.Where(family=> family.parents[0].familyName == "Smith");`|`SELECT * FROM Families f WHERE f.parents[0].familyName = "Smith"`|

## <a name="run-sql-and-linq-queries"></a>Spuštění dotazů SQL a LINQ

1. Následující příklad ukazuje, jak je možné v kódu .NET provést dotaz v jazycích SQL, LINQ nebo LINQ lambda. Zkopírujte kód a přidejte ho na konec souboru Program.cs.

    ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1, EnableCrossPartitionQuery = true };

        // Here we find nelapin via their LastName
        IQueryable<User> userQuery = this.client.CreateDocumentQuery<User>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(u => u.LastName == "Pindakova");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (User user in userQuery)
        {
            Console.WriteLine("\tRead {0}", user);
        }

        // Now execute the same query via direct SQL
        IQueryable<User> userQueryInSql = this.client.CreateDocumentQuery<User>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM User WHERE User.lastName = 'Pindakova'", queryOptions );

        Console.WriteLine("Running direct SQL query...");
        foreach (User user in userQueryInSql)
        {
                Console.WriteLine("\tRead {0}", user);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }
    ```

1. Následující kód zkopírujte a vložte do metody **BasicOperations** před řádek `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);`.

    ```csharp
    this.ExecuteSimpleQuery("Users", "WebCustomers");
    ```

1. V integrovaném terminálu spusťte následující příkaz:

    ```bash
    dotnet run
    ```

    Konzola zobrazí výstup dotazů LINQ a SQL.

V této lekci jste se seznámili s dotazy LINQ a pak jste do aplikace přidali dotazy LINQ a SQL, abyste získali záznamy uživatelů.
