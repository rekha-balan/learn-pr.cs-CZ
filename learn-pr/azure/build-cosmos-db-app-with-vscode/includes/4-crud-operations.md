<!--TODO: explain Etag in knowledge needed-->

Jakmile se naváže spojení s Azure Cosmos DB, dalším krokem je vytvoření, přečtení, nahrazení a odstranění dokumentů uložených v databázi. V této lekci vytvoříte dokumenty uživatele ve vaší kolekci WebCustomer. Poté je budete moci načíst podle ID, nahradit a odstranit.

## <a name="working-with-documents-programmatically"></a>Práce s dokumenty prostřednictvím kódu programu

Data se ve službě Azure Cosmos DB ukládají do dokumentů JSON. [Dokumenty](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) je možné vytvářet, načítat, nahrazovat nebo odstraňovat na portálu, jak je znázorněno v předchozím modulu, nebo prostřednictvím kódu programu, jak je popsáno v tomto modulu. Azure Cosmos DB poskytuje klientské sady SDK pro .NET, .NET Core, Javu, Node.js a Python, z nichž každá tyto operace podporuje. V tomto modulu budeme k provádění operací CRUD (vytvoření, načtení, aktualizace a odstranění) s daty typu NoSQL uloženými v Azure Cosmos DB používat sadu .NET Core SDK.

Hlavní operace pro dokumenty Azure Cosmos DB jsou součástí třídy [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet):
* [CreateDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [ReadDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [ReplaceDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* [UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet). Upsert provede operaci vytvoření nebo nahrazení v závislosti na tom, jestli už příslušný dokument existuje.
* [DeleteDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

Pokud chcete provést jakoukoli z těchto operací, musíte vytvořit třídu, která reprezentuje objekt uložený v databázi. Protože pracujeme s databází uživatelů, budete chtít vytvořit třídu **User**, která bude uchovávat primární data, jako je jméno, příjmení a ID uživatele (které je povinné, protože se jedná o klíč oddílu umožňující horizontální škálování), a třídy pro předvolby dopravy a historii objednávek.

Jakmile budete mít vytvořené tyto třídy reprezentující vaše uživatele, vytvoříte pro každou instanci nové dokumenty uživatele a pak s těmito dokumenty provedeme několik jednoduchých operací CRUD.

## <a name="create-documents"></a>Vytvoření dokumentů

1. Nejprve vytvořte třídu **User** reprezentující objekty, které se mají uložit do služby Azure Cosmos DB. Vytvoříme také třídy **OrderHistory** a **ShippingPreference**, které se používají v rámci třídy **User**. Povšimněte si, že dokumenty musí mít vlastnost **Id** serializovanou jako **id** ve formátu JSON.

    Tyto třídy můžete vytvořit zkopírováním následujících tříd **User**, **OrderHistory** a **ShippingPreference** a jejich vložením pod metodu **BasicOperations**.

    ```csharp
    public class User
    {
        [JsonProperty("id")]
        public string Id { get; set; }
        [JsonProperty("userId")]
        public string UserId { get; set; }
        [JsonProperty("lastName")]
        public string LastName { get; set; }
        [JsonProperty("firstName")]
        public string FirstName { get; set; }
        [JsonProperty("email")]
        public string Email { get; set; }
        [JsonProperty("dividend")]
        public string Dividend { get; set; }
        [JsonProperty("OrderHistory")]
        public OrderHistory[] OrderHistory { get; set; }
        [JsonProperty("ShippingPreference")]
        public ShippingPreference[] ShippingPreference { get; set; }
        [JsonProperty("CouponsUsed")]
        public CouponsUsed[] Coupons { get; set; }
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class OrderHistory
    {
        public string OrderId { get; set; }
        public string DateShipped { get; set; }
        public string Total { get; set; }
    }

    public class ShippingPreference
    {
        public int Priority { get; set; }
        public string AddressLine1 { get; set; }
        public string AddressLine2 { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string ZipCode { get; set; }
        public string Country { get; set; }
    }

    public class CouponsUsed
    {
        public string CouponCode { get; set; }

    }

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }
    ```

1. V integrovaném terminálu zadejte následující příkaz, který program spustí a ověří, že funguje.

    ```csharp
    dotnet run
    ```

1. Nyní zkopírujte a vložte úlohu **CreateUserDocumentIfNotExists** v rámci funkce **WriteToConsoleAndPromptToContinue** na konci souboru Program.cs.

    ```csharp
    private async Task CreateUserDocumentIfNotExists(string databaseName, string collectionName, User user)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            this.WriteToConsoleAndPromptToContinue("User {0} already exists in the database", user.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), user);
                this.WriteToConsoleAndPromptToContinue("Created User {0}", user.Id);
            }
            else
            {
                throw;
            }
        }
    }
    ```

1. Poté se vraťte k metodě **BasicOperations** a přidejte následující kód na konec metody.

    ```csharp
    User yanhe = new User
    {
        Id = "1",
        UserId = "yanhe",
        LastName = "He",
        FirstName = "Yan",
        Email = "yanhe@contoso.com",
        OrderHistory = new OrderHistory[]
            {
                new OrderHistory {
                    OrderId = "1000",
                    DateShipped = "08/17/2018",
                    Total = "52.49"
                }
            },
            ShippingPreference = new ShippingPreference[]
            {
                    new ShippingPreference {
                            Priority = 1,
                            AddressLine1 = "90 W 8th St",
                            City = "New York",
                            State = "NY",
                            ZipCode = "10001",
                            Country = "USA"
                    }
            },
    };

    await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", yanhe);

    User nelapin = new User
    {
        Id = "2",
        UserId = "nelapin",
        LastName = "Pindakova",
        FirstName = "Nela",
        Email = "nelapin@contoso.com",
        Dividend = "8.50",
        OrderHistory = new OrderHistory[]
        {
            new OrderHistory {
                OrderId = "1001",
                DateShipped = "08/17/2018",
                Total = "105.89"
            }
        },
         ShippingPreference = new ShippingPreference[]
        {
            new ShippingPreference {
                    Priority = 1,
                    AddressLine1 = "505 NW 5th St",
                    City = "New York",
                    State = "NY",
                    ZipCode = "10001",
                    Country = "USA"
            },
            new ShippingPreference {
                    Priority = 2,
                    AddressLine1 = "505 NW 5th St",
                    City = "New York",
                    State = "NY",
                    ZipCode = "10001",
                    Country = "USA"
            }
        },
        Coupons = new CouponsUsed[]
        {
            new CouponsUsed{
                CouponCode = "Fall2018"
            }
        }
    };

    await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);
    ```

1. V integrovaném terminálu znovu zadejte následující příkaz, který program spustí.

    ```csharp
    dotnet run
    ```

    Terminál zobrazí výstup vždy, když aplikace vytvoří nový dokument uživatele. Stiskněte libovolnou klávesu, aby se program dokončil.

    ```output
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a>Čtení dokumentů

1. Pokud chcete číst dokumenty z databáze, zkopírujte následující kód a vložte ho za metodu WriteToConsoleAndPromptToContinue v souboru Program.cs.

    ```csharp
    private async Task ReadUserDocument(string databaseName, string collectionName, User user)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            this.WriteToConsoleAndPromptToContinue("Read user {0}", user.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not read", user.Id);
            }
            else
            {
                throw;
            }
        }
    }
    ```

1. Zkopírujte následující kód a vložte ho na konec metody **BasicOperations** za řádek `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

1. V integrovaném terminálu zadejte následující příkaz, který program spustí.

    ```bash
    dotnet run
    ```

    V terminálu se zobrazí následující výstup, kde výstup „Read user 1“ (Byl načten uživatel 1) značí načtení dokumentu.

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a>Nahrazení dokumentů

Azure Cosmos DB podporuje nahrazování dokumentů JSON. V tomto případě aktualizujeme záznam uživatele, aby se projevila změna příjmení.

1. Zkopírujte a vložte metodu **ReplaceUserDocument** za metodu ReadUserDocument v souboru Program.cs.

    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, User updatedUser)
    {
        try
        {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, updatedUser.Id), updatedUser, new RequestOptions { PartitionKey = new PartitionKey(updatedUser.UserId) });
            this.WriteToConsoleAndPromptToContinue("Replaced last name for {0}", updatedUser.LastName);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not found for replacement", updatedUser.Id);
            }
            else
            {
                throw;
            }
        }
    }
    ```

1. Zkopírujte následující kód a vložte ho na konec metody **BasicOperations** za řádek `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe);
    ```

1. V integrovaném terminálu spusťte následující příkaz:

    ```bash
    dotnet run
    ```
    V terminálu se zobrazí následující výstup, kde výstup „Replaced last name for Suh“ (Příjmení se nahradilo za Suh) značí nahrazení dokumentu.

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a>Odstranění dokumentů

1. Zkopírujte metodu **DeleteUserDocument** a vložte ji pod metodu **ReplaceUserDocument**.

    ```csharp
    private async Task DeleteUserDocument(string databaseName, string collectionName, User deletedUser)
    {
        try
        {
            await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, deletedUser.Id), new RequestOptions { PartitionKey = new PartitionKey(deletedUser.UserId) });
            Console.WriteLine("Deleted user {0}", deletedUser.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not found for deletion", deletedUser.Id);
            }
            else
            {
                throw;
            }
        }
    }
    ```

1. Zkopírujte následující kód a vložte ho na konec metody **BasicOperations**.

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", yanhe);
    ```

1. V integrovaném terminálu spusťte následující příkaz:

    ```bash
    dotnet run
    ```

    V terminálu se zobrazí následující výstup, kde výstup „Deleted user 1“ (Odstranil se uživatel 1) značí odstranění dokumentu.

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    Deleted user 1
    End of demo, press any key to exit.
    ```

V této lekci jste vytvořili, nahradili a odstranili dokumenty v databázi Azure Cosmos DB.
