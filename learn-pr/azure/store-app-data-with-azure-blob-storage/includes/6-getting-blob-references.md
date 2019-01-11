Při práci s jednotlivými objekty blob v sadě Azure Storage SDK pro .NET Core se vyžaduje *odkaz na objekt blob* &mdash; instance objektu `ICloudBlob`.

Objekt `ICloudBlob` můžete získat tak, že si ho vyžádáte s použitím názvu objektu blob nebo ho vyberete ze seznamu objektů blob v kontejneru. Obě metody vyžadují `CloudBlobContainer`. Jak ho získat jsme si ukázali v poslední lekci.

## <a name="getting-blobs-by-name"></a>Získání objektů blob podle názvu

Zavoláním některé z metod `GetXXXReference` pro objekt `CloudBlobContainer` můžete získat objekt `ICloudBlob` podle názvu. Pokud znáte typ objektu blob, který získáváte, použijte některou z konkrétních metod (`GetBlockBlobReference`, `GetAppendBlobReference` nebo `GetPageBlobReference`) k získání objektu, který obsahuje metody a vlastnosti upravené pro daný typ objektu blob.

Žádná z těchto metod neprovádí volání přes síť ani neověřuje, jestli cílový objekt blob skutečně existuje. Vytvářejí jenom místně odkaz na objekt blob. Ten se potom dá použít k volání metod, které *ale fungují* přes síť a pracují s objekty blob v úložišti. Oddělená metoda `GetBlobReferenceFromServerAsync` volá rozhraní API úložiště objektů blob a vyvolá výjimku, pokud objekt blob ještě neexistuje.

## <a name="listing-blobs-in-a-container"></a>Výpis objektů blob v kontejneru

Pomocí metody `ListBlobsSegmentedAsync` objektu `CloudBlobContainer` můžete získat seznam objektů blob v kontejneru. *Segmented* odkazuje na jednotlivé stránky vrácených výsledků &mdash; u žádného jednotlivého volání metody `ListBlobsSegmentedAsync` totiž není nikdy zaručeno, že se všechny výsledky vrátí na jedné stránce. Možná bude nutné ji volat opakovaně s použitím tokenu `ContinuationToken`, který vrací a který umožňuje procházení jednotlivých stránek. Kvůli tomu je kód pro výpis objektů blob trochu složitější než kód pro jejich nahrání nebo stažení. Existuje však standardní postup, jak můžete získat každý objekt blob v kontejneru:

```csharp
BlobContinuationToken continuationToken = null;
BlobResultSegment resultSegment = null;

do
{
    resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

    // Do work here on resultSegment.Results

    continuationToken = resultSegment.ContinuationToken;
} while (continuationToken != null);
```

Tento kód bude opakovaně volat metodu `ListBlobsSegmentedAsync`, dokud token `continuationToken` nebude mít hodnotu `null`, která značí konec výsledků.

> [!IMPORTANT]
> Nikdy nepředpokládejte, že výsledky `ListBlobsSegmentedAsync` budou doručeny na jedné stránce. Vždy hledejte token pro pokračování, a pokud je k dispozici, použijte ho.

### <a name="processing-list-results"></a>Zpracování výsledků výpisu

Objekt, který získáte z metody `ListBlobsSegmentedAsync`, obsahuje vlastnost `Results` typu `IEnumerable<IListBlobItem>`. Rozhraní `IListBlobItem` obsahuje pouze několik vlastností pro kontejner objektu blob a adresu URL a samo o sobě není příliš užitečné.

Pokud chcete z vlastnosti `Results` získat užitečné objekty blob, můžete použít metodu `OfType<>` a vyfiltrovat a přetypovat výsledky na konkrétnější typy objektů blob. Tady je pár příkladů:

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

> [!TIP]
> Použití filtru `OfType<>` vyžaduje odkaz na obor názvů `System.Linq` (`using System.Linq;`).

## <a name="exercise"></a>Cvičení

Jedna z funkcí v naší aplikaci vyžaduje získání seznamu objektů blob z rozhraní API. Pomocí výše uvedeného postupu vypíšeme všechny objekty blob v našem kontejneru. Během zpracování seznamu získáme názvy jednotlivých objektů blob.

V editoru nahraďte `GetNames` v `BlobStorage.cs` následujícím kódem a změny uložte.

```csharp
public async Task<IEnumerable<string>> GetNames()
{
    List<string> names = new List<string>();

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);

    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    do
    {
        resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

        // Get the name of each blob.
        names.AddRange(resultSegment.Results.OfType<ICloudBlob>().Select(b => b.Name));

        continuationToken = resultSegment.ContinuationToken;
    } while (continuationToken != null);

    return names;
}
```

> [!TIP]
> Všimněte si, že podpis metody teď vyžaduje specifikaci `async`.

Názvy, které tato metoda vrací, zpracovává `FilesController` a převádí je na adresy URL. Když se vrátí do klienta, vykreslí se na stránce jako hypertextové odkazy.