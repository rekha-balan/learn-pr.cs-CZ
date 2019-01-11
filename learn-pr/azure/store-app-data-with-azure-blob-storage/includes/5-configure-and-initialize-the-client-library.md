Níže najdete typický pracovní postup pro aplikace, které používají službu Azure Blob Storage:

1. **Načtení konfigurace**: Na začátku načtěte konfiguraci účtu úložiště. Obvykle se jedná o připojovací řetězec účtu úložiště.

1. **Inicializace klienta:** Pomocí připojovacího řetězce se inicializuje klientská knihovna Azure Storage. Tím se vytvoří objekty, které aplikace použije ke spolupráci s rozhraním API úložiště objektů blob.

1. **Použití:** V klientské knihovně se provedou volání rozhraní API působící na kontejnery a objekty blob.

## <a name="configure-your-connection-string"></a>Konfigurace připojovacího řetězce

Než spustíte tuto aplikaci, potřebujete připojovací řetězec účtu úložiště, který budete používat. Můžete ho získat pomocí jakéhokoli rozhraní pro správu Azure, včetně webu Azure Portal, Azure CLI nebo Azure PowerShellu. Při nastavování webové aplikace pro spuštění našeho kódu ke konci tohoto modulu použijeme Azure CLI k získání připojovacího řetězce účtu úložiště, který jste vytvořili dříve.

Připojovací řetězec účtu úložiště zahrnuje klíč účtu. Klíč účtu se považuje za tajný kód a měl by být bezpečně uložený. My uložíme připojovací řetězec do nastavení aplikace služby App Service. Nastavení aplikace App Service nabízí bezpečné místo pro tajné kódy aplikace, nepodporuje ale místní vývoj a nejedná se o robustní a kompletní řešení.

> [!WARNING]
> **Neumísťujte klíče účtu úložiště do kódu ani do nechráněných konfiguračních souborů.** Klíče účtu úložiště umožňují plný přístup k vašemu účtu úložiště. Prozrazení klíče může vést k nenapravitelným škodám a vysokým fakturám. V části Další materiály na konci tohoto modulu najdete pokyny pro úložiště a rady, jak obnovit prozrazený klíč.

## <a name="initialize-the-blob-storage-object-model"></a>Inicializace objektového modelu úložiště objektů blob

V sadě SDK Azure Storage pro .NET Core se standardní model použití úložiště objektů blob skládá z následujících kroků:

1. Volání metody `CloudStorageAccount.Parse` (nebo `TryParse`) s vaším připojovacím řetězcem za účelem získání `CloudStorageAccount`

1. Volání metody `CreateCloudBlobClient` v `CloudStorageAccount` za účelem získání `CloudBlobClient`

1. Volání metody `GetContainerReference` v `CloudBlobClient` za účelem získání `CloudBlobContainer`

1. Pomocí metod v kontejneru získejte seznam objektů blob nebo načtěte odkazy na jednotlivé objekty blob, abyste mohli nahrát a stáhnout data.

V kódu vypadají kroky 1 až 3 takto:

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

Tento inicializační kód neprovádí žádná volání přes síť. To znamená, že některé výjimky, ke kterým dochází z důvodu nesprávných informací, nebudou vyvolány dříve. Volání `CloudStorageAccount.Parse` například vyvolá okamžitě výjimku, pokud je připojovací řetězec nesprávně naformátovaný, ale výjimka se nevyvolá, pokud účet úložiště, na který připojovací řetězec odkazuje, neexistuje.

## <a name="create-containers-at-startup"></a>Vytvoření kontejnerů při spuštění

Volání `CreateIfNotExistsAsync` v kontejneru `CloudBlobContainer` je nejvhodnějším způsobem, jak vytvořit kontejner při spuštění aplikace nebo při prvním pokusu o jeho použití.

`CreateIfNotExistsAsync` nevyvolá výjimku, pokud už kontejner existuje. Provede ale volání sítě do služby Azure Storage. Volání proveďte jednou během inicializace, neprovádějte ho při každém pokusu o použití kontejneru.

## <a name="exercise"></a>Cvičení

### <a name="clone-and-explore-the-unfinished-app"></a>Klonování a prozkoumání nedokončené aplikace

Nejdříve si naklonujeme aplikaci starter z GitHubu. V terminálu Cloud Shell spusťte následující příkaz, který získá kopii zdrojového kódu, a tu potom otevřete v editoru:

```console
git clone https://github.com/MicrosoftDocs/mslearn-store-data-in-azure.git
cd mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start
code .
```

V editoru otevřete soubor `Controllers/FilesController.cs`. Tady není potřeba nic provádět, rychle se tu jen podíváme na to, co aplikace dělá.

Tento kontroler implementuje rozhraní API se třemi akcemi:

- Při **indexaci** (GET /api/Files) se vrátí seznam adres URL pro každý nahraný soubor. Front-end aplikace volá tuto metodu za účelem vypsání seznamu hypertextových odkazů na nahrané soubory.
- Při **nahrání** (POST /api/Files) dojde k přijetí nahraného souboru a jeho uložení.
- Při **stažení** (GET /api/Files/{název_souboru}) se stáhne jednotlivý soubor s určitým názvem.

Každá metoda používá k práci instanci `IStorage` s názvem `storage`. V `Models/BlobStorage.cs` se nachází neúplná implementace `IStorage`, kterou vyplníme.

### <a name="add-the-nuget-package"></a>Přidání balíčku NuGet

Nejdříve přidejte odkaz na sadu Azure Storage SDK. Z terminálu spusťte následující příkazy:

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

Tím zajistíte použití nejnovější verze klientské knihovny úložiště objektů blob.

### <a name="configure"></a>Konfigurace

Hodnoty konfigurace, které potřebujeme, jsou připojovací řetězec účtu úložiště a název kontejneru, který aplikace použije k ukládání souborů. V tomto modulu pouze spustíme aplikaci ve službě Azure App Service, a proto využijeme osvědčený postup pro službu App Service a uložíme hodnoty v nastavení aplikace App Service. Uděláme to při vytvoření instance služby App Service. V tuto chvíli tak nemusíme nic dělat.

Co se týká *použití* konfigurace, naše úvodní aplikace už obsahuje základy, které potřebujeme. Parametr konstruktoru `IOptions<AzureStorageConfig>` ve službě `BlobStorage` má dvě vlastnosti: připojovací řetězec účtu úložiště a název kontejneru, do kterého bude naše aplikace ukládat objekty blob. V metodě `ConfigureServices` ve `Startup.cs` existuje kód, který při spuštění aplikace načte hodnoty z konfigurace.

### <a name="initialize"></a>Inicializace

V editoru otevřete soubor `Models/BlobStorage.cs`. Na začátek souboru přidejte následující příkazy `using`, abyste ho připravili na kód, který během cvičení přidáte.

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Vyhledejte metodu `Initialize`. Tuto metodu bude naše aplikace volat při prvním použití `BlobStorage`. Pokud vás zajímá, jak to funguje, můžete se podívat na `ConfigureServices` v `Startup.cs`.

Pokud náš kontejner ještě neexistuje, vytvoříme ho v `Initialize`. Aktuální implementaci `Initialize` nahraďte následujícím kódem a práci uložte:

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```