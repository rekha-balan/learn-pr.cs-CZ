Když teď máme v Azure vytvořenou mezipaměť Redis, vytvoříme aplikaci, která ji bude používat. Ujistěte se, že máte informace o připojovacím řetězci z webu Azure Portal.

> [!NOTE]
> Integrovaný Cloud Shell je k dispozici na pravé straně. Tento příkazový řádek můžete použít k vytvoření a spuštění ukázkového kódu, který zde kompilujeme nebo provést tyto kroky místně, pokud máte nastavení vývojového prostředí .NET Core.

## <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Abychom se mohli soustředit na implementaci mezipaměti Redis, použijeme jednoduchou konzolovou aplikaci.

1. V Cloud Shellu vytvořte novou konzolovou aplikaci .NET Core a pojmenujte ji „SportsStatsTracker“.

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. Po vytvoření složky projektu se do ní přesuňte.

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a>Získání připojovacího řetězce

Pojďme do kódu přidat připojovací řetězec získaný z webu Azure Portal. Nikdy takovéto přihlašovací údaje neukládejte do zdrojového kódu. Aby tento ukázkový kód zůstal co nejjednodušší, použijeme konfigurační soubor. Lepším řešením pro aplikaci v Azure na straně serveru by bylo použití služby Azure Key Vault s certifikáty.

1. Vytvořte nový soubor **appsettings.json**, který přidáte k projektu.

    ```bash
    touch appsettings.json
    ```

1. Zadáním příkazu `code .` v projektové složce otevřete editor kódu. Pokud pracujete místně, doporučujeme používat **Visual Studio Code**. Uvedené kroky se budou ve většině případů shodovat.

1. V editoru vyberte soubor **appsettings.json** a přidejte do něj následující text. Vložte svůj připojovací řetězec do **hodnoty** nastavení.

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. Uložte změny.

    > [!IMPORTANT]
    > Kdykoliv vložíte nebo změníte kód do souboru v editoru, nezapomeňte ho následně uložit pomocí nabídky „...“ nebo tlačítka akcelerátoru (<kbd>Ctrl+S</kbd> ve Windows a Linuxu, <kbd>Cmd+S</kbd> v macOS).

1. Kliknutím na soubor **SportsStatsTracker.csproj** v editoru ho otevřete.

1. Přidejte následující blok konfigurace `<ItemGroup>` do kořenového prvku `<Project>` a zahrňte nový soubor do projektu a zkopírujte ho do výstupní složky. Tím se zajistí, že se konfigurační soubor aplikace po kompilaci nebo sestavení aplikace umístí do výstupního adresáře.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. Soubor uložte. (Nezapomeňte to udělat, jinak změny po přidání balíčku níže ztratíte.)

## <a name="add-support-to-read-a-json-configuration-file"></a>Přidání podpory pro čtení konfiguračního souboru JSON

Aplikace .NET Core ke čtení konfiguračního souboru JSON potřebuje další balíčky NuGet.

1. V části okna s příkazovým řádkem přidejte odkaz na balíček NuGet **Microsoft.Extensions.Configuration.Json**.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Přidání kódu pro čtení konfiguračního souboru

Když jsme teď přidali požadované knihovny umožňující čtení konfigurace, je potřeba povolit tuto funkci v konzolové aplikaci.

1. Vyberte v editoru soubor **Program.cs**.

1. V horní části souboru se nachází řádek `using System`. Pod tento řádek přidejte následující řádky kódu:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Obsah metody **Main** nahraďte následujícím kódem. Tento kód provede inicializaci konfiguračního systému tak, aby se umožnilo čtení ze souboru **appsettings.json**.

    ```csharp
    var config = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json")
        .Build();
    ```

Soubor **Program.cs** by teď měl vypadat takto:

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace SportsStatsTracker
{
    class Program
    {
        static void Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();
        }
    }
}
```

## <a name="get-the-connection-string-from-configuration"></a>Získání připojovacího řetězce z konfigurace

1. V souboru **Program.cs** použijte na konci metody **Main** novou proměnnou **config**, abyste získali připojovací řetězec a uložili ho do nové proměnné s názvem **connectionString**.
    - Proměnná **config** má indexer, do kterého můžete předat řetězec, který se má získat ze souboru **appSettings.json**.

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a>Přidání podpory pro klienta .NET s mezipamětí Redis

Dále nakonfigurujeme konzolovou aplikaci, aby pro .NET používala klienta **StackExchange.Redis**.

1. Pomocí příkazového řádku v dolní části editoru Cloud Shell přidejte k projektu balíček NuGet **StackExchange.Redis**.

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. V editoru vyberte soubor **Program.cs** a u oboru názvů **StackExchange.Redis** přidejte příkaz `using`.

    ```csharp
    using StackExchange.Redis;
    ```
    
Po dokončení instalace můžete ve svém projektu používat klienta mezipaměti Redis.

## <a name="connect-to-the-cache"></a>Připojení k mezipaměti

Teď přidáme kód, abychom se mohli k mezipaměti připojit.

1. Vyberte v editoru soubor **Program.cs**.

1. Předáním `ConnectionMultiplexer.Connect` do připojovacího řetězce vytvořte `ConnectionMultiplexer`. Vrácenou hodnotu pojmenujte **cache**.

1. Vzhledem k tomu, že vytvořené připojení je možné _zcela odstranit_, zabalte ho do bloku `using`. Váš kód by měl vypadat přibližně takto:

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> Připojení k Azure Cache for Redis spravuje třída `ConnectionMultiplexer`. V klientské aplikaci byste měli tuto třídu sdílet a opakovaně používat. U každé operace _nechceme_ vytvářet nové připojení. Místo toho ho chceme uložit jako pole v třídě a u každé operace ho opakovaně použít. Zde ho sice použijeme pouze v metodě **Main**, ale v produkční aplikaci by se mělo uložit do pole třídy nebo jednoúčelové databáze.

## <a name="add-a-value-to-the-cache"></a>Přidání hodnoty do mezipaměti

Když teď máme připojení, pojďme přidat do mezipaměti hodnotu.

1. Jakmile se vytvoří připojení, použijte uvnitř bloku `using` metodu `GetDatabase`, abyste získali instanci `IDatabase`.

1. Na objekt `IDatabase` zavolejte příkazem `StringSet`, abyste hodnotě „some value“ nastavili klíč „test:key“.
    - Vrácenou hodnotou z příkazu `StringSet` je `bool` označující, zda se klíč přidal.

1. Vrácenou hodnotu z příkazu `StringSet` zobrazte v konzole.

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a>Získání hodnoty z mezipaměti

1. Dále získáme hodnotu pomocí příkazu `StringGet`. Tento příkaz vezme klíč, aby hodnotu získal a vrátil.

1. Vrácenou hodnotu vypište.

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. Váš kód měl vypadat přibližně takto:

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;
    using StackExchange.Redis;
    
    namespace SportsStatsTracker
    {
        class Program
        {
            static void Main(string[] args)
            {
                var config = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile("appsettings.json")
                    .Build();
    
                string connectionString = config["CacheConnection"];
    
                using (var cache = ConnectionMultiplexer.Connect(connectionString))
                {
                    var db = cache.GetDatabase();
    
                    bool setValue = db.StringSet("test:key", "some value");
                    Console.WriteLine($"SET: {setValue}");
    
                    string getValue = db.StringGet("test:key");
                    Console.WriteLine($"GET: {getValue}");
                }
            }
        }
    }
    ```
    
1. Spuštěním aplikace si zobrazte výsledek. Do okna terminálu pod editorem zadejte `dotnet run`. Ujistěte, že jste v projektové složce, jinak terminál váš kód nenajde a nebude ho moct sestavit a spustit.
    
    ```bash
    dotnet run
    ```
    
> [!TIP] 
> Pokud program nedělá, co očekáváte, ale zkompiluje se, může to být způsobené tím, že jste neuložili změny v editoru. Nezapomeňte vždy ukládat změny při přepínání mezi terminálem a okny editoru 

## <a name="use-the-async-versions-of-the-methods"></a>Použijte asynchronní verze metod

Podařilo se nám získat a nastavit hodnoty z mezipaměti, ale používáme starší synchronní verze. U aplikací na straně serveru to znamená neúčinné využití našich vláken. Místo toho chceme u těchto metod použít _asynchronní_ verze. Poznáte je snadno, všechny končí na **Async**.

Aby se nám s těmito metodami dobře pracovalo, můžeme používat klíčová slova jazyka C# `async` a `await`. Abychom však mohli tato klíčová slova na metodu **Main** použít, budeme potřebovat C# _alespoň_ ve verzi 7.1.

### <a name="switch-to-c-71"></a>Přechod na C# 7.1

Klíčová slova `async` a `await` jazyka C# nebyla platnými klíčovými slovy u metod **Main** až do verze 7.1. Na tento kompilátor se můžeme snadno přepnout pomocí příznaku v souboru **.csproj**.

1. Otevřete v editoru soubor **SportsStatsTracker.csproj**.

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
1. Přidat příkaz `using`, aby zahrnoval `System.Threading.Tasks`.

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
        ...
```

### <a name="get-and-set-values-asynchronously"></a>Asynchronní získání a nastavení hodnot

Můžeme nechat na místě i synchronní metody, přidejte volání na `StringSetAsync` a `StringGetAsync` metody a přidejte další hodnotu do mezipaměti. Nastavte „Čítač“ na hodnotu „100“.  

1. Pomocí metod `StringSetAsync` a `StringGetAsync` nastavte a získejte klíč s názvem „counter“. Nastavte jeho hodnotu na 100.

1. Použitím klíčového slova `await` získejte výsledky z každé metody.

1. Výsledky vypište do okna konzoly – stejně jako u synchronních verzí.

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. Spusťte aplikaci znova – měla by stále fungovat, ale teď má dvě hodnoty.

#### <a name="increment-the-value"></a>Zvýšení hodnoty

1. Pomocí metody `StringIncrementAsync` zvyšte hodnotu **counter**. Předáním čísla **50** ho připočtěte k hodnotě counter.
    - Všimněte si, že tato metoda vezme klíč _a_ buď parametr `long`, nebo `double`.
    - Podle předaných parametrů vrátí buď `long`, nebo `double`.

1. Výsledky metody vypište do konzoly.

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a>Další operace

Nakonec si vyzkoušíme několik dalších metod podporujících příkaz `ExecuteAsync`.

1. Spusťte příkaz „PING“, abyste otestovali připojení serveru. Měla by se vrátit odpověď „PONG“.
1. Spuštěním příkazu „FLUSHDB“ vymažte hodnoty databáze. Měla by se vrátit odpověď „OK“.

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

Konečný kód by měl vypadat přibližně takto:

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();

            string connectionString = config["CacheConnection"];

            using (var cache = ConnectionMultiplexer.Connect(connectionString))
            {
                var db = cache.GetDatabase();

                bool setValue = db.StringSet("test:key", "some value");
                Console.WriteLine($"SET: {setValue}");

                string getValue = db.StringGet("test:key");
                Console.WriteLine($"GET: {getValue}");

                setValue = await db.StringSetAsync("test", "100");
                Console.WriteLine($"SET: {setValue}");

                getValue = await db.StringGetAsync("test");
                Console.WriteLine($"GET: {getValue}");

                var result = await db.ExecuteAsync("ping");
                Console.WriteLine($"PING = {result.Type} : {result}");
                
                result = await db.ExecuteAsync("flushdb");
                Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
            }
        }
    }
}
```

## <a name="challenge"></a>Výzva

Jako výzvu zkuste serializovat typ objektu do mezipaměti. Tady jsou základní kroky.

1. Vytvořte novou třídu `class` s nějakými veřejnými vlastnostmi. Můžete si vymyslet vlastní (Osoby nebo auta jsou oblíbené) nebo použít příklad „GameStats“ z předchozí lekce.

1. Pomocí příkazu `dotnet add package` přidejte balíček NuGet **Newtonsoft.Json**.

1. Do oboru názvů souboru `Newtonsoft.Json` přidejte příkaz `using`.

1. Vytvořte si vlastní objekt.

1. Serializujte ho pomocí `JsonConvert.SerializeObject` a odešlete ho do mezipaměti příkazem `StringSetAsync`.

1. Získejte ho z mezipaměti zpátky příkazem `StringGetAsync` a potom ho deserializujte pomocí `JsonConvert.DeserializeObject<T>`.

