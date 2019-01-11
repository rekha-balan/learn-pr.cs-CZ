První věcí, kterou musíme udělat, je vytvoření prázdné databáze Azure Cosmos DB a kolekce, která s ní bude fungovat. Chceme, aby odpovídaly těm, které jsme vytvořili v posledním modulu v tomto postupu výuky: databáze s názvem **Products** (Produkty) a kolekce s názvem **Clothing** (Oblečení). Pomocí následujících pokynů a Azure Cloud Shellu na pravé straně obrazovky znovu vytvořte databázi.

## <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a>Vytvoření účtu a databáze Azure Cosmos DB pomocí příkazového řádku Azure CLI

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="select-a-subscription"></a>Výběr předplatného

Pokud používáte Azure už nějakou dobu, můžete mít k dispozici několik předplatných. To je často případ vývojářů, kteří mívají jedno předplatné pro Visual Studio a druhé pro podnikové prostředky.

Sandbox Azure pro vás v Cloud Shellu už vybral předplatné Concierge. Nastavení předplatného můžete ověřit pomocí těchto kroků. Nebo, když pracujete s vaším vlastním předplatným, můžete použít následující kroky a přepnout mezi předplatnými prostřednictvím příkazového řádku Azure CLI.

1. Začněte výpisem dostupných předplatných.

    ```azurecli
    az account list --output table
    ```

   Pokud pracujete s předplatným Concierge, mělo by být na seznamu uvedené jako jediné.

1. Dále pak, pokud výchozí předplatné není to, které chcete použít, můžete ho změnit pomocí příkazu `account set`:

    ```azurecli
    az account set --subscription "<subscription name>"
    ```
    
1. Získejte skupinu prostředků, která pro vás byla vytvořená v sandboxu. Pokud používáte svoje vlastní předplatné, přeskočte tento krok a zadejte jedinečný název, který chcete použít, do níže uvedené proměnné prostředí `RESOURCE_GROUP`. Poznamenejte si název skupiny prostředků. Tam vytvoříme naši databázi.

    ```azurecli
    az group list --out table
    ```
### <a name="setup-environment-variables"></a>Nastavení proměnných prostředí

1. Nastavte několik proměnných prostředí, abyste nemuseli běžné hodnoty pokaždé zadávat. Začněte tím, že nastavíte název účtu Azure Cosmos DB, například `export NAME="mycosmosdbaccount"`. Pole může obsahovat jenom malá písmena, číslice a znak spojovníku a musí se skládat ze 3 až 31 znaků.

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

1. Nastavte skupinu prostředků, která bude používat existující skupinu prostředků sandboxu.

    ```azurecli
    export RESOURCE_GROUP="<rgn>[sandbox resource group name]</rgn>"
    ```

1. Vyberte oblast, která je k vám nejblíže, a nastavte proměnnou prostředí, například `export LOCATION="EastUS"`.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

1. Nastavte proměnnou pro název databáze. Pojmenujte ji Products (Produkty), aby odpovídala databázi, kterou jsme vytvořili v posledním modulu.

    ```azurecli
    export DB_NAME="Products"
    ```

### <a name="create-a-resource-group-in-your-subscription"></a>Vytvoření skupiny prostředků v předplatném

Při vytváření služby Cosmos DB ve svém vlastním předplatném můžete vytvořit novou skupinu prostředků, aby obsahovala všechny související prostředky.

> [!IMPORTANT]
> Pokud používáte sandbox Azure poskytovaný v rámci Microsoft Learn, nemusíte tento krok provádět. Místo toho zajistěte, aby byla výše uvedená proměnná `RESOURCE_GROUP` nastavená na **<rgn>[název skupiny prostředků sandboxu]</rgn>**.

Ve svém vlastním předplatném byste k vytvoření skupiny prostředků použili následující příkaz. 

```azurecli
az group create --name <name> --location <location>
```

### <a name="create-the-azure-cosmos-db-account"></a>Vytvoření účtu Azure Cosmos DB

1. Pomocí příkazu `cosmosdb create` vytvořte účet Azure Cosmos DB. Příkaz používá následující parametry a pokud nastavíte proměnné prostředí podle doporučení, lze ho spustit bez jakýchkoliv úprav.
    - `--name`: Jedinečný název prostředku.
    - `--kind`: Druh databáze, použijte _GlobalDocumentDB_.
    - `--resource-group`: Požadovaná skupina prostředků. Použijte **<rgn>[název skupiny prostředků sandboxu]</rgn>**. Měl by být přiřazený v proměnné `RESOURCE_GROUP`.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

    Dokončení příkazu trvá několik minut. Jakmile bude nový účet nasazený, služba cloud shell zobrazí jeho nastavení.

1. Vytvořte v rámci účtu databázi `Products`.

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

1. Nakonec vytvořte kolekci `Clothing`.

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

Účet, databázi a kolekci Azure Cosmos DB už tedy máte, teď přidáme nějaká data.
