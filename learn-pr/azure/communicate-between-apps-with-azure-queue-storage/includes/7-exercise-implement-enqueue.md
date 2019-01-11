Po splnění všech požadavků můžete napsat kód, který vytvoří novou frontu úložiště a přidá do ní zprávu. Tento kód bychom obvykle umístili do front-endových aplikací, které generují data.

## <a name="add-the-client-library-for-azure-storage"></a>Přidání klientské knihovny pro službu Azure Storage

Začneme přidáním **klientské knihovny Azure Storage pro .NET** do naší aplikace. Můžete ji nainstalovat pomocí NuGet (správce balíčků .NET). 

1. Nainstalujte do projektu balíček `WindowsAzure.Storage` pomocí příkazu `dotnet add package`. Můžete to provést v okně terminálu _pod_ Cloud Shellem nebo v okně terminálu/konzoly ve stejné složce, v jaké je projekt, pokud pracujete na místním počítači.

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a>Přidání metody pro odeslání upozornění na zprávu

Teď vytvoříme novou metodu, která do fronty odešle zprávu.

1. Otevřete soubor `Program.cs` v editoru kódu.

1. Na začátek souboru přidejte následující obory názvů. K připojení ke službě Azure Storage a následné práci s frontami budeme používat typy z obou těchto oborů názvů.

    - `System.Threading.Tasks`
    - `Microsoft.WindowsAzure.Storage`
    - `Microsoft.WindowsAzure.Storage.Queue`

1. Ve třídě `Program` vytvořte statickou metodu `SendArticleAsync`, která přijímá typ `string` a vrací objekt `Task`. Pomocí této metody odešleme do naší služby nový článek. Pojmenujte vstupní parametr `newsMessage`, jak je vidět níže.

    ```csharp
    ...
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue; 
    
    class Program
    {
        ...
    
        static async Task SendArticleAsync(string newsMessage)
        {
        }
    }
    ```
    
1. V nové metodě použijte statickou metodu `CloudStorageAccount.Parse` k parsování připojovacího řetězce (vzpomeňte si, že jsme ho uložili do řetězcové konstanty). Tato metoda vrátí objekt `CloudStorageAccount`, který je potřeba uložit do proměnné.

1. Zavoláním metody `CreateCloudQueueClient()` pro objekt účtu úložiště získejte objekt klienta a uložte ho do proměnné.

1. Pak zavolejte metodu `GetQueueReference` pro objekt klienta a předejte jí řetězec "newsqueue" s názvem fronty. Tato metoda vrátí objekt `CloudQueue`, který můžeme použít k práci s frontou. Je v pořádku, pokud fronta ještě neexistuje.

1. Zavolejte metodu `CreateIfNotExistsAsync()` pro objekt `CloudQueue`, abyste zajistili, že bude fronta připravená k použití. Tato metoda frontu v případě potřeby vytvoří.
    - Vzhledem k tomu, že se jedná o asynchronní metodu, použijte klíčové slovo jazyka C# `await`, abyste zajistili, že s ní pracujete správně. Znamená to také, že je k _metodě_ potřeba připojit klíčové slovo `async`. 
    - Metoda `CreateIfNotExistsAsync` vrátí hodnotu `bool`, která bude `true`, pokud se fronta vytvořila, a `false`, pokud už fronta existuje. Pokud došlo k vytvoření fronty, vypište do konzoly zprávu.
    - Pokud potřebujete pomoc, tady je příklad.

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        // Not Shown here - code from prior steps
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. Vytvořte instanci objektu `CloudQueueMessage`. 
    - Přijímá parametr `string`, takže předejte parametr metody (`newsMessage`). To bude _text_ (tělo) zprávy. K dispozici je také statická metoda, která dokáže vytvořit binární datovou část zprávy.

1. Voláním metody `AddMessageAsync` pro objekt `CloudQueue` přidejte zprávu do fronty. Toto je také asynchronní metoda a bude potřeba použít klíčové slovo `await`, abyste zajistili, že s ní pracujete správně.

1. Soubor uložte a sestavte ho zadáním příkazu `dotnet build` do příkazového okna. Opravte případné chyby – ke kontrole můžete použít následující kód.

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference("newsqueue");
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a>Přidání kódu pro odeslání zprávy

Teď upravíme metodu `Main` tak, aby předávala všechny přijaté parametry do naší nové metody, abychom mohli otestovat novou metodu pro odesílání.

1. Vyhledejte metodu `Main`.

1. Zkontrolujte předaný parametr `args` a zjistěte, jestli se do příkazového řádku předala nějaká data. Pokud ano, pomocí metody `String.Join` vytvořte ze všech slov jeden řetězec a jako oddělovač použijte mezeru.

1. Tento řetězec předejte do nové metody `SendArticleAsync`. 

1. Jakmile metoda vrátí výsledek, pomocí metody `Console.WriteLine` vypište zprávu, kterou jsme odeslali.

1. Soubor uložte a sestavte program (`dotnet build`) – ke kontrole můžete použít následující kód.

    > [!NOTE]
    > Vzhledem k tomu, že je naše metoda technicky vzato asynchronní, budeme chtít použít klíčové slovo `await`, pokud ale nepoužíváte jazyk C# 7.1 nebo novější, není tato funkce v metodě `Main` k dispozici. Stačí pro metodu zavolat metodu `Wait()`, která ve skutečnosti zablokuje čekání na vrácení výsledku metodou. To během chvilky opravíme.

    ```csharp
    static void Main(string[] args)
    {
        if (args.Length > 0)
        {
            string value = String.Join(" ", args);
            SendArticleAsync(value).Wait();
            Console.WriteLine($"Sent: {value}");
        }
    }
    ```

## <a name="execute-the-application"></a>Spuštění aplikace

Aplikace teď může odesílat zprávy. Pokud ji chcete otestovat, můžete ji spustit z příkazového řádku pomocí příkazu `dotnet run`. Všechny další řetězce se do aplikace předají jako parametry. 

> [!WARNING]
> Než program sestavíte a spustíte, ujistěte se, že jste v online editoru uložili všechny soubory.

Například můžete zadat:

```azurecli
dotnet run Send this message
```

Tímto by se měl do fronty přidat řetězec `"Send this message"`.

## <a name="check-your-results"></a>Kontrola výsledků

Fronty můžete zkontrolovat na webu Azure Portal pomocí **Průzkumníku služby Storage**, který vám umožní zkoumat data ve všech účtech úložiště, které vlastníte.

Alternativně můžete použít Azure CLI nebo PowerShell. Nahraďte hodnotu `<connection-string>` vaším připojovacím řetězcem a vyzkoušejte v prostředí tento příkaz:

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

Příkaz by měl vypsat informace o vaší zprávě, které budou vypadat přibližně takto:

```json
[
  {
    "content": "U2VuZCB0aGlzIG1lc3NhZ2U=",
    "dequeueCount": 0,
    "expirationTime": "2018-09-05T02:43:40+00:00",
    "id": "b47cbe9f-a246-4a81-86ae-fa02ea8d98bc",
    "insertionTime": "2018-09-24T02:43:40+00:00",
    "popReceipt": null,
    "timeNextVisible": null
  }
]
```

K dispozici je několik dalších příkazů, které můžete s nástroji vyzkoušet – podívejte se na příkazy `az storage queue --help` a `az storage message --help` a prozkoumejte je.

> [!TIP]
> Abyste nemuseli pokaždé zadávat parametr `--connection-string`, uložte hodnotu připojovacího řetězce do proměnné prostředí `AZURE_STORAGE_CONNECTION_STRING`.

## <a name="optional---use-the-async-versions-of-the-methods"></a>Volitelné – Použijte asynchronní verze metod.

Výše jsme použili metodu `Wait()` pro metodu send, to ale není efektivní využití našich výpočetních prostředků. Místo toho chceme použít metody `async` a `await` jazyka C#. Abychom však mohli tato klíčová slova na metodu **Main** použít, budeme potřebovat C# _alespoň_ ve verzi 7.1.

### <a name="switch-to-c-71"></a>Přechod na C# 7.1

Klíčová slova `async` a `await` jazyka C# nebyla platnými klíčovými slovy u metod **Main** až do verze 7.1. Na tento kompilátor se můžeme snadno přepnout pomocí příznaku v souboru **.csproj**.

1. Otevřete v editoru soubor **QueueApp.csproj**.

1. Do první skupiny vlastností (`PropertyGroup`) v souboru sestavení přidejte `<LangVersion>7.1</LangVersion>`. Po dokončení by to mělo vypadat nějak takto.
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>Použití asynchronního klíčového slova

Dále na metodu **Main** použijte klíčové slovo `async`. Budeme muset provést tři úkoly.

1. Přidat do podpisu metody **Main** klíčové slovo `async`.
1. Změnit návratový typ z `void` na `Task`.
1. Odeberte volání metody `Wait()` při volání metody `SendArticleAsync` a nahraďte ho hodnotou `await`.

Zkuste znovu spustit aplikaci – měla by i nadále fungovat stejným způsobem jako předtím, kód ale teď dokáže uvolnit vlákno zpět do fondu vláken .NET, zatímco budeme čekat na zařazení zprávy do fronty.

Teď, když jsme odeslali zprávu, je posledním krokem přidání podpory pro _příjem_ zpráv.