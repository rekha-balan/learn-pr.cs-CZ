Začněme vytvořením instance Azure Cache for Redis a následným vytvořením jednoduché transakce, která vkládá dvě hodnoty dat do mezipaměti.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-cache-for-redis"></a>Vytvoření instance Azure Cache for Redis

Začněme vytvořením instance Azure Cache for Redis pomocí Azure CLI. Použijte Cloud Shell na pravé straně okna prohlížeče k interakci s Azure.

K vytvoření nové instance Azure Cache for Redis použijeme příkaz `az redis create`. Má několik různých parametrů. Tady jsou ty nejběžnější (úplný seznam najdete v dokumentaci).

> [!div class="mx-tableFixed"]
> | Parametr | Popis |
> |-----------|-------------|
> | `--name`    | Název mezipaměti – musí být globálně jedinečný a skládat se z písmen, číslic a pomlček. |
> | `--resource-group` | Použijte předem vytvořenou skupinu prostředků **<rgn>[název skupiny prostředků sandboxu]</rgn>**, která je součástí Azure Sandboxu. |
> | `--location` | Určete umístění, kde má být mezipaměť umístěna. Obvykle je vhodné vybrat umístění blízko spotřebitelům dat. V tomto případě jste omezeni na umístění dostupná v Azure Sandbox. Vyberte umístění, které je vám nejblíže. |
> | `--size` | Velikost Azure Cache for Redis. Platné hodnoty jsou [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]. |
> | `--sku` | Skladová položka Azure Cache for Redis. Platné hodnoty jsou [Basic, Standard, Premium]. |

### <a name="select-a-location"></a>Výběr umístění
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Vytvořte mezipaměť s použitím následujících možností:
    - Velikost: C0
    - Skladová položka: Basic

1. Tady je příklad příkazového řádku. Nezapomeňte nahradit parametr `[name]` jedinečným názvem. Můžete nahradit umístění, pokud chcete jinou oblast než USA – východ.

    ```azurecli
    REDIS_NAME=[name]

    az redis create \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location eastus \
        --vm-size C0 \
        --sku Basic
    ```

## <a name="create-a-net-core-console-application"></a>Vytvoření konzolové aplikace .NET Core

Poté vytvořte konzolovou aplikaci .NET Core, která se použije k vložení hodnot dat do služby Azure Cache for Redis.

1. Vytvořte novou aplikaci .NET Core s použitím integrovaného Cloud Shellu na pravé straně okna. Pojmenujte ji „RedisData“.

    ```bash
    cd ~
    dotnet new console --name RedisData
    ```

1. Přejděte do nového adresáře vytvořeného pro vaši aplikaci.

    ```bash
    cd RedisData
    ```

1. Zkompilujte a spusťte aplikaci – jejím výstupem by mělo být „Hello World!“.

    ```bash
    dotnet run
    ```

## <a name="add-the-servicestackredis-nuget-package"></a>Přidání balíčku ServiceStack.Redis NuGet

Když teď máme konzolovou aplikaci, musíme přidat balíček NuGet **ServiceStack.Redis**. To nám umožní připojit se ke službě Azure Cache for Redis a zadat příkazy v jazyce C#.

1. Přidejte balíček NuGet **ServiceStack.Redis** s použitím prostředí terminálu.

    ```bash
    dotnet add package ServiceStack.Redis
    ```

1. Znovu zkompilujte a spusťte aplikaci a ujistěte se, že se zkompiluje vše. Jejím výstupem by stále mělo být „Hello World!“.

## <a name="get-your-azure-cache-for-redis-connection-string"></a>Získání připojovacího řetězce služby Azure Cache for Redis

Pokud se chcete připojit ke službě Azure Cache for Redis, potřebujete připojovací řetězec obsahující vaše heslo a adresu URL. **ServiceStack.Redis** má svůj vlastní formát připojovacího řetězce: `[password]@[hostname]:[sslport]?ssl=true`.

Heslo můžete načíst pomocí webu Azure Portal nebo pomocí příkazového řádku. Pojďme tady použít druhé jmenované, protože přístup s portálem jsme použili v modulu „Optimalizace vašich webových aplikací pomocí ukládání dat jen pro čtení do mezipaměti s využitím mezipaměti Redis“.

Použijte příkaz `az redis list-keys` k získání přístupových klíčů. Spusťte tyto příkazy, abyste získali primární klíč, uložili ho v proměnné s názvem `REDIS_KEY` a zobrazili ho:

```azurecli
REDIS_KEY=$(az redis list-keys \
    --name "$REDIS_NAME" \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query primaryKey \
    --output tsv)

echo $REDIS_KEY
```

Dál spusťte tento příkaz, abyste sestavili připojovací řetězec a zobrazili ho na příkazovém řádku. Všimněte si, že název hostitele je název mezipaměti, za nímž následuje `.redis.cache.windows.net`, a port je **6380**, což je výchozí port Redis protokolu SSL.

```bash
echo "$REDIS_KEY"@"$REDIS_NAME".redis.cache.windows.net:6380?ssl=true
```

Zkopírujte připojovací řetězec do schránky – použijete ho v dalším kroku.

## <a name="add-the-connection-string-to-your-app"></a>Přidání připojovacího řetězce do aplikace

1. Otevřete editor Cloud Shell ve složce aplikace.

    ```bash
    cd ~/RedisData
    code .
    ```

1. Vyberte zdrojový soubor **Program.cs**.

1. Vytvořte následující pole v třídě `Program` a vložte do něj jako hodnotu připojovací řetězec.

    ```csharp
    static string redisConnectionString = "<connection string>";
    ```

    Tady je příklad:

    ```csharp
    static string redisConnectionString = "ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6380?ssl=true";
    ```

## <a name="insert-two-data-values-into-your-azure-cache-for-redis"></a>Vložení dvou hodnot dat do Azure Cache for Redis

Nakonec do Azure Cache for Redis přidáme data.

1. Na začátek souboru **Program.cs** přidejte následující příkaz `using`.

    ```csharp
    using ServiceStack.Redis;
    ```

1. Obsah metody `Main` nahraďte následujícím kódem. Tímto dojde k použití transakce pro přidání dvou hodnot.

    ```csharp
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    if (transactionResult)
    {
        Console.WriteLine("Transaction committed");
    }
    else
    {
        Console.WriteLine("Transaction failed to commit");
    }
    ```

1. Před sestavením a spuštěním aplikace zkontrolujte, že Redis Cache je plně zřízená. Dokončení příkazu `az redis create` občas může zabrat několik minut. Spuštěním následujícího příkazu zkontrolujte stav.

    ```azurecli
    az redis show \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --query provisioningState
    ```

    Pokud je stav `Creating`, zkuste to znovu za pár minut. Po dokončení se zobrazí `Succeeded`.

1. Spusťte aplikaci a ověřte, že konzole zobrazuje zprávu **Transakce potvrzena**.

    ```bash
    dotnet run
    ```

## <a name="verify-your-data"></a>Ověření dat

Na úplný konec ověříme, jestli se data do Azure Cache for Redis přidala.

1. Pomocí stejného účtu, kterým jste aktivovali sandbox, se přihlaste k webu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Vyhledejte Azure Cache for Redis výběrem volby **Všechny prostředky** na levém bočním panelu a použitím pole filtru vlevo k výběru instancí Azure Cache for Redis. Případně můžete použít vyhledávací pole nahoře a zadat název mezipaměti.

1. Vyberte instanci Azure Cache for Redis.

1. V okně **Přehled** pro Azure Cache for Redis vyberte **Konzola**. Otevře se konzola Azure Cache for Redis, která vám umožňuje zadat nízkoúrovňové příkazy Azure Cache for Redis.

1. Spusťte příkaz `get MyKey1`. Ověřte, že vrácená hodnota je **MyValue1**.

1. Spusťte příkaz `get MyKey2`. Ověřte, že vrácená hodnota je **MyValue2**.

    ![Snímek obrazovky konzoly Azure Cache for Redis zobrazující hodnoty MyKey1 a MyKey2](../media/4-redis-console.png)