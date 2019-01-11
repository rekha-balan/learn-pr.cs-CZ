Visual Studio Code umožňuje vytvářet konzolové aplikace pomocí integrovaného terminálu a několika krátkých příkazů.

V této lekci vytvoříte jednoduchou konzolovou aplikaci pomocí integrovaného terminálu, získáte připojovací řetězec služby Azure Cosmos DB z rozšíření a poté nakonfigurujete připojení z vaší aplikace ke službě Azure Cosmos DB.

## <a name="create-a-console-app"></a>Vytvoření konzolové aplikace

1. V nástroji Visual Studio Code vyberte **File** (Soubor)  > **Open File** (Otevřít soubor).

1. Vytvořte novou složku s názvem `learning-module` ve zvoleném umístění a poté klikněte na tlačítko **Vybrat složku**.

1. Zajistěte, aby bylo povolené automatické ukládání souboru. Uděláte to tak, že kliknete na nabídku File (Soubor) a zaškrtnete políčko **Auto Save** (Automatické ukládání), pokud už není zaškrtnuté. Budete provádět kopírování v několika blocích kódu, tím se zajistí, že budete vždy pracovat s nejnovějšími změnami souborů.

1. Ve Visual Studio Code otevřete integrovaný terminál výběrem **View** (Zobrazit)  > **Terminal** (Terminál) z hlavní nabídky.

1. V okně terminálu zkopírujte a vložte následující příkaz.

    ```bash
    dotnet new console
    ```

    Tento příkaz vytvoří ve vaší složce soubor **Program.cs**, který obsahuje jednoduchý hotový program Hello World, a soubor projektu jazyka C# **learning-module.csproj**.

1. V okně terminálu zkopírujte a vložte následující příkaz a spusťte tak program „Hello World“.

    ```bash
    dotnet run
    ```

    V okně terminálu se zobrazí „Hello world!“ jako výstup.

## <a name="connect-the-app-to-azure-cosmos-db"></a>Připojení aplikace ke službě Azure Cosmos DB

1. Ve výzvě terminálu zkopírujte a vložte následující blok příkazů a proveďte instalaci požadovaných balíčků NuGet.

    ```bash
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package System.Configuration.ConfigurationManager
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet restore
    ```

1. V horní části podokna Explorer klikněte na tlačítko **Program.cs** a otevřete soubor.

1. Přidejte následující příkazy Using za `using System;`.

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

    Pokud se zobrazí zpráva o přidání požadovaných chybějících prostředků, klikněte na tlačítko **Ano**.

1. Vytvořte nový soubor s názvem App.config ve složce learning-module a přidejte následující kód.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. Zkopírujte připojovací řetězec po kliknutí na ikonu Azure ![Ikona Azure](../media/2-setup/visual-studio-code-explorer-icon.png) na levé straně, rozbalte předplatné Concierge, pravým tlačítkem myši klikněte na nový účet služby Azure Cosmos DB a poté klikněte na možnost **Copy Connection String** (Zkopírovat připojovací řetězec).

1. Vložte připojovací řetězec na konec souboru App.config a poté zkopírujte část **AccountEndpoint** z připojovacího řetězce do hodnoty **accountEndpoint** v souboru App.config.

    accountEndpoint by se mělo podobat následujícímu kódu:

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. Nyní zkopírujte hodnotu **AccountKey** z připojovacího řetězce do hodnoty **accountKey** a pak odstraňte původní připojovací řetězec, který jste kopírovali.

1. Ve zprávě terminálu zkopírujte a vložte následující příkaz a spusťte tak program.

    ```csharp
    dotnet run
    ```

    Program zobrazí zprávu Hello World! v terminálu.

## <a name="create-the-documentclient"></a>Vytvoření instance DocumentClient

Nyní je čas vytvořit instanci DocumentClient, což je reprezentace služby Azure Cosmos DB na straně klienta. Tento klient slouží ke konfiguraci a provádění požadavků na službu.

1. V souboru Program.cs přidejte následující na začátek třídy Program.

    ```csharp
    private DocumentClient client;
    ```

1. Přidáním nového asynchronního úkolu vytvořte nového klienta a zkontrolujte, zda databáze Users existuje, přidáním následující metody za metodu `Main`.

    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["accountEndpoint"]), ConfigurationManager.AppSettings["accountKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });

        Console.WriteLine("Database and collection validation complete");
    }
    ```

1. V integrovaném terminálu znovu zkopírujte a vložte následující příkaz, který program spustí a ověří, že funguje.

    ```csharp
    dotnet run
    ```

1. Zkopírujte a vložte následující kód do metody **Main** a přepište stávající řádek `Console.WriteLine("Hello World!");`.

    ```csharp
    try
    {
        Program p = new Program();
        p.BasicOperations().Wait();
    }
    catch (DocumentClientException de)
    {
        Exception baseException = de.GetBaseException();
        Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
    }
    catch (Exception e)
    {
        Exception baseException = e.GetBaseException();
        Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
    }
    finally
    {
        Console.WriteLine("End of demo, press any key to exit.");
        Console.ReadKey();
    }
    ```

1. V integrovaném terminálu znovu zadejte následující příkaz, který program spustí a ověří, že funguje.

    ```csharp
    dotnet run
    ```

    Konzola zobrazí následující výstup.

    ```output
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

V této lekci jste vytvořili základy pro vaši aplikaci využívající Azure Cosmos DB. Nastavili jste vývojové prostředí v aplikaci Visual Studio Code, vytvořili jednoduchý projekt Hello World, propojili ho s koncovým bodem služby Azure Cosmos DB a ověřili, že databáze a kolekce existují.
