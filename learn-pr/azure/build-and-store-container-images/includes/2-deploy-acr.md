V této lekci vytvoříte registr kontejneru Azure pomocí Azure CLI.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]
 
## <a name="create-an-azure-container-registry"></a>Vytvoření registru kontejneru Azure

Budeme pracovat s bezplatnými sandboxovými prostředky, takže není potřeba vytvářet vlastní skupiny prostředků. Pomocí příkazu `az acr create` vytvořte registr kontejneru Azure. Název registru kontejneru musí být v rámci Azure jedinečný a musí obsahovat 5 až 50 alfanumerických znaků.

V tomto příkladu se nasadí SKU registru úrovně Premium. SKU úrovně Premium se vyžaduje kvůli geografické replikaci. 

Nejprve v Cloud Shellu definujeme proměnnou prostředí s názvem **ACR_NAME** pro uchovávání názvu, který chceme dát našemu novému registru kontejneru.

1. Spuštěním následujícího příkazu definujte proměnnou s názvem ACR_NAME.

    > [!IMPORTANT]
    > Před spuštěním příkazu nahraďte `<registry-name>` jedinečným názvem, který chcete použít pro nový registr kontejneru. Název registru musí být jedinečný v rámci Azure a musí obsahovat 5 až 50 **alfanumerických** znaků. Další informace o vytváření názvů najdete v tématu [Zásady vytváření názvů pro prostředky Azure](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions?azure-portal=true).

    ```azurecli
    ACR_NAME=<registry-name>
    ```
1. Zadáním následujícího příkazu do editoru Cloud Shell vytvořte nový registr kontejneru.

    ```azurecli
    az acr create --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACR_NAME --sku Premium
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    Následující fragment kódu představuje ukázkovou odpověď z příkazu `az acr create`. V tomto příkladu se registr nazýval *myACR*. Všimněte si, že níže uvedená hodnota loginServer představuje ve výchozím nastavení název registru zapsaný malými písmeny.  
    
    ```output
    {
      "adminUserEnabled": false,
      "creationDate": "2018-08-15T19:19:07.042178+00:00",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myACR0007",
      "location": "eastus",
      "loginServer": "myacr.azurecr.io",
      "name": "myACR",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sku": {
        "name": "Premium",
        "tier": "Premium"
      },
      "status": null,
      "storageAccount": null,
      "tags": {},
      "type": "Microsoft.ContainerRegistry/registries"
    }
    ```

> [!IMPORTANT]
> Příkazy ve zbývající části modulu budou používat hodnotu proměnné **ACR_NAME**. 

V této lekci jste vytvořili registr kontejneru Azure pomocí Azure CLI. Tento nový registr kontejneru použijeme v další lekci při sestavování imagí kontejneru.