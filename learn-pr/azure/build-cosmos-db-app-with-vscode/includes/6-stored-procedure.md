V databázi často potřebujete aktualizovat více dokumentů současně. V této lekci se zaměříme na to, jak vytvořit, zaregistrovat a spustit uložené procedury z konzolové aplikace .NET.

## <a name="create-a-stored-procedure-in-your-app"></a>Vytvoření uložené procedury v aplikaci

V této uložené proceduře se používá OrderId (obsahující seznam všech položek v objednávce) k výpočtu celkového součtu objednávky. Celkový součet objednávky se počítá ze součtu položek v objednávce, od kterého se odečítá případný kredit zákazníka a slevové kupóny.

1. Ve Visual Studio Code na kartě **Azure: Cosmos DB** rozbalte účet služby Azure Cosmos DB > **Uživatelé** > **WebCustomers** a potom klikněte pravým tlačítkem na **Uložené procedury** a potom klikněte na **Vytvořit uloženou proceduru**.

1. Do textového pole v horní části obrazovky zadejte `UpdateOrderTotal` a stisknutím klávesy Enter dejte uložené proceduře název.

1. Na kartě **Azure: Cosmos DB** rozbalte **Uložené procedury** a klikněte na **UpdateOrderTotal**.

    Ve výchozím nastavení obdržíte uloženou proceduru, která načte první položku.

1. Pokud chcete tuto uloženou proceduru spustit z aplikace, přidejte do souboru **Program.cs** následující kód.

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
        Console.WriteLine("Stored procedure complete");
    }
    ```

1. Teď zkopírujte následující kód a vložte ho před řádek `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` v metodě **BasicOperations**.

    ```csharp
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. V integrovaném terminálu spusťte následující příkaz, kterým spustíte ukázku pomocí uložené procedury.

    ```bash
    dotnet run
    ```

Konzola zobrazí výstup, že se uložená procedura dokončila.
