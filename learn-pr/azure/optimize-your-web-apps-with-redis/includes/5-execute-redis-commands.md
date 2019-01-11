Jak už bylo zmíněno dříve, Redis je databáze NoSQL v paměti, kterou je možné replikovat napříč více servery. Často se používá jako mezipaměť, ale může sloužit i jako formální databáze nebo i jako zprostředkovatel zpráv. 

Je možné do ní ukládat různé datové typy a struktury a podporuje širokou škálu příkazů, které můžete použít k načtení dat uložených v mezipaměti nebo můžete zadávat dotazy na informace o samotné mezipaměti. Data, se kterými pracujete, jsou vždy uložena jako páry klíč/hodnota.

## <a name="executing-commands-on-the-redis-cache"></a>Provádění příkazů na mezipaměti Redis

Obvykle to funguje tak, že klientské aplikace používá _klientskou knihovnu_ k formulování požadavků a spouštění příkazů na mezipaměť Redis. Seznam klientských knihoven můžete získat přímo ze [stránky klientů Redis](https://redis.io/clients). Oblíbený a vysoce výkonný klient Redis pro jazyk .NET je **StackExchange.Redis**. Tento balíček je k dispozici prostřednictvím balíčku NuGet a jde ho přidat do vašeho kódu .NET pomocí příkazového řádku nebo integrovaného vývojového prostředí.

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a>Připojení k mezipaměti Redis pomocí StackExchange.Redis

Připomeňme si, že pro připojení k serveru Redis používáme adresu hostitele, číslo portu a přístupový klíč. Azure také pro některé klienty Redis nabízí _připojovací řetězec_, který spojuje tyto údaje do jednoho řetězce.

### <a name="what-is-a-connection-string"></a>Co je připojovací řetězec?

Připojovací řetězec je jeden řádek textu, který zahrnuje všechny požadované informace pro připojení a ověření pro mezipaměť Redis v Azure. Bude vypadat nějak takto (pole **název-mezipaměti** a **tady-je-heslo** nahraďte skutečnými hodnotami):

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> Připojovací řetězec by měl být ve vaší aplikaci dobře chráněn. Pokud je aplikace hostovaná v Azure, zvažte, jestli k uložení jeho hodnoty nebude nejlepší použít službu Azure Key Vault.

Tento řetězec pak můžete předat do **StackExchange.Redis** na vytvoření připojení k serveru. 

Všimněte si, že na konci jsou další dva parametry: 

- **ssl** – zajišťuje, že je komunikace šifrovaná.
- **abortConnection** – dovoluje vytvořit připojení, i když server v daném okamžiku není dostupný.

Existuje i několik dalších [volitelných parametrů](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options), které můžete připojit na konec řetězce, abyste nakonfigurovali klientskou knihovnu.

### <a name="creating-a-connection"></a>Vytvoření připojení

Hlavní objekt připojení v **StackExchange.Redis** je třída `StackExchange.Redis.ConnectionMultiplexer`. Tento objekt abstrahuje proces připojení k serveru Redis (nebo skupině serverů). Je optimalizovaný, aby efektivně spravoval připojení, a je zamýšlený tak, že bude k dispozici po celou dobu, co potřebujete přístup k mezipaměti.

Vytvoříte instanci objektu `ConnectionMultiplexer` pomocí statické metody `ConnectionMultiplexer.Connect` nebo `ConnectionMultiplexer.ConnectAsync`, a to předáním buď připojovacího řetězce, nebo objektu `ConfigurationOptions`. 

Tady je jednoduchý příklad:

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

Jakmile budete mít objekt `ConnectionMultiplexer`, jsou tři primární věcí, které můžete chtít provádět:

1. Přistupovat k databázi Redis. Právě na tohle se v tomto článku zaměřujeme.
2. Použít funkce vydavatele/odběratele Redisu. To je nad rámec tohoto modulu.
3. Přistupovat k jednotlivému serveru za účelem údržby nebo monitorování.

### <a name="accessing-a-redis-database"></a>Přístup k databázi Redis

Databáze Redis je reprezentována typem `IDatabase`. Můžete ji načíst pomocí metody `GetDatabase()`:

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> Objekt vrácený metodou `GetDatabase` je zjednodušený objekt a není nutné ho ukládat. Jenom objekt `ConnectionMultiplexer` je nutné udržovat aktivní.

Jakmile budete mít objekt `IDatabase`, můžete spouštět metody pro interakci s mezipamětí. Všechny metody mají synchronní a asynchronní verze, které vracejí objekty `Task` tak, aby byly kompatibilní s klíčovými slovy `async` a `await`.

Tady je příklad uložení páru klíč/hodnota do mezipaměti:

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

Metoda `StringSet` vrátí hodnotu `bool`, která znamená, jestli hodnota byla nastavena (`true`) nebo nebyla nastavena (`false`). Potom můžeme tuto hodnotu načíst pomocí metody `StringGet`:

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a>Získání a nastavení binárních hodnot

Připomeňme si, že klíče a hodnoty Redis _binárně bezpečné_. Stejné metody je možné použít k ukládání binárních dat. Pro práci s typy `byte[]` existují implicitní operátory převodu, abyste mohli s těmito daty pracovat přirozeně:

```csharp
byte[] key = ...;
byte[] value = ...;

db.StringSet(key, value);
```

```csharp
byte[] key = ...;
byte[] value = db.StringGet(key);
```

> [!TIP]
> **StackExchange.Redis** představuje klíče používající typ `RedisKey`. Tato třída má implicitní převody na a z typů `string` a `byte[]`, což umožňuje bez komplikací používat textové i binární klíče. Hodnoty jsou reprezentovány typem `RedisValue`. Stejně jako u typu `RedisKey` jsou k dispozici implicitní převody, aby bylo možné předávat hodnoty typu `string` nebo `byte[]`.

#### <a name="other-common-operations"></a>Další běžné operace

Rozhraní `IDatabase` obsahuje několik dalších metod pro práci s mezipamětí Redis. Jsou k dispozici metody pro práci s hodnotami hash, seznamy, sadami a seřazenými sadami.

Tady jsou některé z nejběžnějších, které pracují s jedním klíčem. Úplný seznam můžete najít ve [zdrojovém kódu](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) rozhraní.

| Metoda | Popis |
|--------|-------------|
| `CreateBatch` | Vytvoří _skupinu operací_, která bude odeslána na server jako jedna jednotka. Nemusí být ale nutně jako jedna jednotka zpracována. |
| `CreateTransaction` | Vytvoří skupinu operací, která bude odeslána na server jako jedna jednotka _a také_ bude na serveru jako jedna jednotka zpracována. |
| `KeyDelete` | Odstraní klíč/hodnotu. |
| `KeyExists` | Vrátí informaci, jestli zadaný klíč v mezipaměti existuje. |
| `KeyExpire` | Nastaví hodnotu TTL (vypršení platnosti) pro klíč. |
| `KeyRename` | Přejmenuje klíč. |
| `KeyTimeToLive` | Vrátí hodnotu TTL pro klíč. |
| `KeyType` | Vrátí řetězcovou reprezentaci typu hodnoty uložené v klíči. Vráceny mohou být tyto typy: string, list, set, zset a hash. |
       
### <a name="executing-other-commands"></a>Provádění jiných příkazů

Objekt `IDatabase` má metody `Execute` a `ExecuteAsync`, které jde použít k předávání textových příkazů serveru Redis. Příklad:

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

Metody `Execute` a `ExecuteAsync` vrací objekt `RedisResult`, což je zásobník dat obsahující dvě vlastnosti:

- `Type` – vrátí hodnotu `string` označující typ výsledku ("STRING", "INTEGER" atd.)
- `IsNull` – hodnota true/false na zjištění, jestli je výsledek `null`.

Pak můžete použít `ToString()` na `RedisResult` a získat skutečnou vrácenou hodnotu.

Můžete použít `Execute` k provádění jakýchkoli podporovaných příkazů – například je možné získat všechny klienty připojené k mezipaměti ("CLIENT LIST"):

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

To vypíše jako výstup všechny připojené klienty:

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a>Ukládání komplexnějších hodnot
Redis je orientovaný na binárně bezpečné řetězce, můžete ale uložit do mezipaměti i objektové grafy, když je serializujete do textového formátu – obvykle XML nebo JSON. Máme například pro naše statistiky objekt `GameStats`, který vypadá nějak takhle:

```csharp
public class GameStat
{
    public string Id { get; set; }
    public string Sport { get; set; }
    public DateTimeOffset DatePlayed { get; set; }
    public string Game { get; set; }
    public IReadOnlyList<string> Teams { get; set; }
    public IReadOnlyList<(string team, int score)> Results { get; set; }

    public GameStat(string sport, DateTimeOffset datePlayed, string game, string[] teams, IEnumerable<(string team, int score)> results)
    {
        Id = Guid.NewGuid().ToString();
        Sport = sport;
        DatePlayed = datePlayed;
        Game = game;
        Teams = teams.ToList();
        Results = results.ToList();
    }

    public override string ToString()
    {
        return $"{Sport} {Game} played on {DatePlayed.Date.ToShortDateString()} - " +
               $"{String.Join(',', Teams)}\r\n\t" + 
               $"{String.Join('\t', Results.Select(r => $"{r.team } - {r.score}\r\n"))}";
    }
}
```

Mohli bychom použít knihovnu **Newtonsoft.Json** a převést instanci tohoto objektu na řetězec:

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

Ten pak můžeme načíst a převést zpět na objekt pomocí opačného procesu:

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a>Vyčištění připojení
Jakmile už připojení Redis nepotřebujete, můžete **vyhodit** objekt `ConnectionMultiplexer`. Tím zavřete všechna připojení a vypnete komunikaci se serverem.

```csharp
redisConnection.Dispose();
redisConnection = null;
```

Pojďme vytvořit aplikaci a udělat pár jednoduchých akcí s naší mezipamětí Redis.