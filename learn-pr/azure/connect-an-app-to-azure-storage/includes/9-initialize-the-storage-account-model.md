Klientská knihovna Azure Storage poskytuje objektový model, který se používá k interakci s účty Azure Storage. Slouží k rychlému připojení k účtu Azure Storage a používá rozhraní API služby Azure Storage. 

## <a name="azure-storage-client-library-object-model"></a>Objektový model klientské knihovny Azure Storage

::: zone pivot="csharp"

Základem objektového modelu účtu úložiště v klientské knihovně .NET Core je třída `CloudStorageAccount`. Nejjednodušší způsob, jak inicializovat objektový model, je parsovat připojovací řetězec pomocí metody `CloudStorageAccount.Parse` nebo `CloudStorageAccount.TryParse`.

> [!NOTE]
> Klientská knihovna se o připojení nepokusí, dokud se nevyvolá operace, která to vyžaduje. Metody `Parse()` a `TryParse()` jenom zaručují, že připojovací řetězec je správně naformátovaný. Neověřují, jestli daný účet existuje nebo jestli je klíč správný. 

Výsledná instance `CloudStorageAccount`, kterou vrátí volání metody `Parse()` nebo `TryParse()`, zveřejňuje metody vytvoření objektů klientů pro přístup ke službám úložišť objektů Blob, Souborů, Front a Tabulek Azure. 

Fragment kódu uvedený níže ukazuje příklad vytvoření klienta, který se použije pro Blob Storage:

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Objekt `CloudStorageAccount` a objekty klientů jsou zjednodušené a lze je vytvořit na vyžádání nebo předem tak, aby se sdílely v rámci aplikace. Standardní přístupem je vytvoření instance pomocí metody `CloudStorageAccount.TryParse()` poblíž vstupního bodu aplikace a její zpřístupnění v rámci aplikace za účelem vytváření klientských instancí.

Jakmile budete mít objekt klienta pro určitý typ úložiště, můžete k provedení práce použít metody. Metody provádějící síťová volání jsou záměrně asynchronní – v prostředí .NET používáme klíčová slova `async` a `await`, která s těmito metodami efektivně pracují.

Například můžeme k vytvoření _kontejneru objektů blob_ použít `CloudBlobClient` a soubor nahrát do úložiště objektů blob.

```csharp
// Create a local CloudBlobContainer object. No network call.
string containerName = "myblobcontainer";
CloudBlobContainer blobContainer = blobClient.GetContainerReference(containerName);

// This makes an actual service call to the Azure Storage service. 
// Unless this call fails, the container will have been created.
await blobContainer.CreateAsync();

// Create a local object to represent our blob. No network call.
string blobName = "myphoto";
CloudBlockBlob blob = blobContainer.GetBlockBlobReference(blobName);

// This transfers data in the file to the blob on the service.
string filename = "photo.png";
await blob.UploadFromFileAsync(fileName);
```

::: zone-end

::: zone pivot="javascript"

Základem objektového modelu účtu úložiště v **klientské knihovně pro Microsoft Azure Storage pro Node.js a JavaScript** je objekt `azurestorage`. Ten se vytvoří přidáním modulu **azure-storage** do vaší aplikace pomocí příkazu `require`.

```javascript
const storage = require('azure-storage');
```

Tento objekt obsahuje řadu _metod pro vytváření objektů_, které vytváří konkrétní objekt pro spolupráci s každou omezující vlastností Azure Storage. K vytvoření každého objektu voláte metody `createXXX`.

| Typ | Metoda | Vrací |
|--------|---------|-------------|
| **Objekt blob** | `createBlobService` | `BlobService` |
| **Tabulka** | `createTableService` | `TableService` |
| **Fronta** | `createQueueService` | `QueueService` |
| **Soubor** | `createFileService` | `FileService` |

> [!NOTE]
> Klientská knihovna se o připojení nepokusí, dokud se nevyvolá operace, která to vyžaduje. Každá z těchto metod `create` vrátí zjednodušený objekt představující přístup k typu úložiště – neověřuje připojení ani použití přístupového klíče.

Jakmile budete mít objekt služby pro určitý typ úložiště, můžete k provedení práce použít metody. Metody provádějící síťová volání jsou záměrně asynchronní. Knihovna aktuálně podporuje _zpětná volání_, která vrací asynchronní výsledky. Tady je například kód, který vytvoří kontejner objektů blob.

```javascript
var azure = require('azure-storage');
var blobService = azure.createBlobService();

blobService.createContainerIfNotExists('myblobcontainer', function(err, result, response) {
  if (!err) {
    // if result.created = true, container was created.
    // if result.created = false, container already existed.
  }
});
```

Tento přístup funguje, ale může vést k neovladatelnému nárůstu kódu přidaného do zpětných volání. Lepším řešením pro práci s těmito metodami v JavaScriptu je použít _přísliby_. Existuje několik skvělých knihoven, které metody ve stylu zpětného volání převedou na přísliby – můžete si vybrat svou oblíbenou.

Zde použijeme funkci `util.promisify` prostředí Node a k vytvoření kontejneru a nahrání souboru do úložiště objektů blob použijeme `BlobService`. Kromě toho použijeme kvůli přirozenější práci s přísliby klíčová slova `async` a `await`.

```javascript
const util = require('util');
const storage = require('azure-storage');

const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);
const uploadBlobAsync = util.promisify(blobService.createBlockBlobFromLocalFile).bind(blobService);

async function main() {
    try {
        // This makes an actual service call to the Azure Storage service. 
        // Unless this call fails, the container will have been created.
        await createContainerAsync(containerName);

        // This transfers data in the file to the blob on the service.
        var uploadResult = await uploadBlobAsync(containerName, "myphoto", "photo.png");
        if (uploadResult) {
            console.log("blob uploaded");
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

main();
```
::: zone-end

Pojďme si to vyzkoušet v aplikaci.