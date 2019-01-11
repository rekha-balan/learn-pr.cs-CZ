::: zone pivot="csharp"
Pojďme přidat kód, který načte z konfigurace připojovací řetězec, a použít ho pro připojení k účtu Azure Storage.

## <a name="retrieve-the-connection-string"></a>Získání připojovacího řetězce

1. Otevřete soubor **Program.cs** v editoru kódu.

1. Do horní části souboru přidejte příkaz `using`, který bude odkazovat na obor názvů `Microsoft.WindowsAzure.Storage`:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. Na konec metody `Main` přidejte následující řádek, abyste z konfiguračního souboru získali připojovací řetězec účtu Azure Storage. Předaný _klíč_ musí odpovídat názvu použitému v souboru **appsettings.json**.

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>Vytvoření klienta objektů blob

1. Použijte statickou metodu `CloudStorageAccount.TryParse` a vytvořte objekt `CloudStorageAccount`, který vezme připojovací řetězec a parametr `out` a vrátí vytvořený objekt. Vrátí se hodnota `bool`, která udává, zda došlo k úspěšnému parsování řetězce.
    - Pokud metoda selže, odešlete do konzoly zprávu a metodu vraťte.

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString,
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. Vrácený objekt `CloudStorageAccount` použijte k vytvoření klienta objektů blob.

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Pak pomocí klienta objektů blob získejte odkaz na kontejner s názvem „photoblobs“. Podobně jako u názvů účtů musí názvy kontejnerů objektů blob obsahovat pouze malá písmena a čísla.

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. K vytvoření kontejneru použijte metodu `CreateIfNotExistsAsync`. Ta vrátí klíčové slovo `bool` určující, zda došlo k vytvoření kontejneru. Tuto informaci uložte do proměnné s názvem `created`.
    - Všimněte si, že jde o **asynchronní** metodu – to znamená, že se provede skutečné volání přes síť.
    - Abyste získali výsledek `await`, budete muset použít klíčové slovo `bool`.

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. Vzhledem k tomu, že používáme klíčové slovo `await`, změňte podpis metody `Main` na `async` a vraťte `Task`.

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

    Také bude třeba přidat nový příkaz `using` v horní části:

    ```csharp
    using System.Threading.Tasks;
    ```

1. Nakonec odešlete výstup, zda jsme kontejner objektů blob vytvořili.

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. Soubor uložte.

Pokud si chcete zkontrolovat svou práci, konečný soubor by měl vypadat nějak takto.

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using Microsoft.WindowsAzure.Storage;
using System.Threading.Tasks;

namespace PhotoSharingApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
            var connectionString = configuration["StorageAccountConnectionString"];

            if (!CloudStorageAccount.TryParse(connectionString, out CloudStorageAccount storageAccount))
            {
                Console.WriteLine("Unable to parse connection string");
                return;
            }

            var blobClient = storageAccount.CreateCloudBlobClient();
            var blobContainer = blobClient.GetContainerReference("photoblobs");
            bool created = await blobContainer.CreateIfNotExistsAsync();

            Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
        }
    }
}
```

## <a name="use-c-71-to-build-our-app"></a>Použití jazyka C# 7.1 k sestavení aplikace

Do verze 7.1 jazyka C# byla u metod `Main` přidána podpora funkcí `async` a `await`. Tato podpora nemusí být výchozí verze kompilátoru, který používáte. Ujistěme se, že kompilátor tuto verzi používá tak, že ji nastavíme v konfiguračním souboru.

1. Otevřete soubor `PhotoSharingApp.csproj` a do `PropertyGroup` určující `TargetFramework` přidejte `<LangVersion>7.1</LangVersion>`. Po dokončení by to mělo vypadat nějak takto:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <LangVersion>7.1</LangVersion>
        <TargetFramework>netcoreapp2.0</TargetFramework>
      </PropertyGroup>
      ...
    </Project>
    ```

1. Soubor uložte.

## <a name="run-the-app"></a>Spuštění aplikace

1. Sestavte a spusťte aplikaci. **Poznámka:** ujistěte se, že jste ve správném pracovním adresáři.

    ```bash
    dotnet run
    ```

::: zone-end

::: zone pivot="javascript"
Pojďme přidat kód, kterým se pomocí uloženého připojovacího řetězce připojíme k účtu Azure Storage. Klientská knihovna Azure automaticky použije k získání připojovacího řetězce proměnnou prostředí **AZURE_STORAGE_CONNECTION_STRING**.

## <a name="create-a-blob-client"></a>Vytvoření klienta objektů blob

1. V editoru kódu otevřete soubor **index.js**.

1. Začněte tím, že přidáte modul **azure-storage**. Uložte tento modul do proměnné s názvem **storage**.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    const storage = require('azure-storage');
    ```

1. Hned potom tento objekt **storage** použijte k vytvoření objektu `BlobService` a uložte ho do globálního objektu s názvem **blobService**. Nezapomínejte, že jde o odlehčené objekty představující přístup k účtu úložiště.

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. Přidejte konstantu zastupující kontejner, který chceme vytvořit. Tento kontejner pojmenujeme „photoblobs“.

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a>Vytvoření kontejneru

K práci v Azure Storage s rozhraními API objektu blob můžeme použít objekt `BlobService`. Jak už jsme zmínili, všechna rozhraní API, která volají přes sít, jsou kvůli odezvě aplikace asynchronní. Jednou z těchto metod je i `createContainerIfNotExists`. Ke zpracování zpětného volání, které obsahuje odpověď, použijeme _přísliby_.

1. Použijte `util.promisify`, abyste získali verzi zpětného volání metody `createContainerIfNotExists`, a přetvořte ji na metodu vracející přísliby.
    - Jelikož metoda zpětně volá objekt, nezapomeňte na konec přidat volání `bind`, abyste ho připojili k danému kontextu.
    - Návratovou hodnotu přiřaďte konstantě s názvem `createContainerAsync` v horní části souboru, jak je znázorněno níže.
    - Budete také potřebovat `require` pro modul `util` v horní části souboru.

    ```javascript
    const util = require('util');
    const storage = require('azure-storage');
    const blobService = storage.createBlobService();

    const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

    const containerName = 'photoblobs';
    ```

2. Odeberte "Hello, World!" z výstupu `main()`.

3. Zavolejte nový příslib `createContainerAsync`.
    - Předejte mu konstantu **containerName**.
    - Na volání použijte klíčové slovo `await`.
    - Zabalte volání do konstruktoru `try` / `catch` a získejte výstup všech chyb.

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. Příslib `createContainerAsync` vrátí první hodnotu ze základní metody `createContainerIfNotExists`, která je výsledkem kontejneru. Obsahuje informace o vytvoření kontejneru. Zaznamenejte výsledek do proměnné a odešlete výstup, zda se kontejner na základě vlastnosti `result.created` vytvořil.

    ```javascript
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. Nakonec funkci `main` opatřete klíčovým slovem `async`.

1. Soubor uložte.

Pokud si chcete zkontrolovat svou práci, konečný soubor by měl vypadat nějak takto.

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function main() {
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

main();
```

## <a name="run-the-app"></a>Spuštění aplikace

1. Sestavte a spusťte aplikaci. **Poznámka:** Ujistěte se, že jste v adresáři PhotoSharingApp.

    ```bash
    node index.js
    ```

::: zone-end

Měli byste získat zprávu, že se kontejner objektů blob vytvořil. Pokud soubor spustíte podruhé, měli byste získat zprávu, že už existuje.

Postup ověření kontejneru:

1. Pomocí stejného účtu, kterým jste aktivovali sandbox, se přihlaste k portálu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Přejděte na svůj účet úložiště. K vyhledání účtu úložiště můžete použít **Všechny prostředky** nebo ho vyhledat podle názvu ve _vyhledávacím poli_ v horní části okna portálu.

1. V části **Služby Storage Blob** vyberte položku účtu úložiště **Objekty blob**.

1. Na panelu objektů blob byste měli vidět kontejner **photoblobs**. Kontejner můžete odstranit kliknutím na tři tečky (...) napravo od této položky a zkusit ho vytvořit znovu pomocí aplikace.

> [!NOTE]
> Kontejner z portálu zmizí velmi rychle, ale než se skutečně odstraní, trvá to několik minut. Pokud se ho pokusíte znovu vytvořit, když ho Azure odstraňuje, zobrazí se chybová odpověď.

## <a name="delete-the-app"></a>Odstranění aplikace
Pokud se rozhodnete, že si zdrojový kód aplikace nechcete ve svém prostředí Cloud Shell ponechat, můžete použít následující příkaz, kterým odstraníte složku a veškerý její obsah.

```bash
rm -r PhotoSharingApp/
```
