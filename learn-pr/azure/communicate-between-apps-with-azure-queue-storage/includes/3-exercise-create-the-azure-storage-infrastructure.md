Zjistili jste, že špičky provozu můžou zahltit střední vrstvu. Tomu předejdete tak, že do své aplikace pro nahrávání článků mezi front-end a prostřední vrstvu přidáte frontu.

Prvním krokem při vytváření fronty je vytvoření účtu úložiště Azure, ve kterém se budou data ukládat.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-storage-account-with-the-azure-cli"></a>Vytvoření účtu úložiště pomocí Azure CLI

> [!TIP] 
> Normálně byste nový projekt začali vytvořením _skupiny prostředků_ pro všechny přidružené prostředky. V tomto případě budeme používat sandbox Azure, který poskytuje skupinu prostředků s názvem <rgn>[název skupiny prostředků sandboxu]</rgn>.

Vytvořte účet úložiště pomocí příkazu `az storage account create`. Příkaz můžete zadat do okna Cloud Shell napravo.

Příkaz potřebuje několik parametrů:

| Parametr | Hodnota |
|-----------|-------|
| `--name`  | Nastaví název. Nezapomeňte, že účty úložiště používají název k vygenerování veřejné adresy URL, takže musí být jedinečný. Kromě toho musí mít název účtu délku 3 až 24 znaků a může obsahovat jenom číslice a malá písmena. Doporučujeme použít předponu **articles** a příponu náhodného čísla, ale můžete použít libovolný název. |
| `-g`        | Určuje **skupinu prostředků**, jako hodnotu použijte _<rgn>[název skupiny prostředků sandboxu]</rgn>_. |
| `--kind`    | Nastaví **typ účtu úložiště** – použijte _StorageV2_, aby se vytvořil účet pro obecné účely V2. |
| `--sku`     | Nastaví **typ replikace a úložiště**, jeho výchozí hodnotou je _Standard_RAGRS_. Použijeme _Standard_LRS_, což znamená, že účet bude pouze místně redundantní v rámci datacentra. |
| `-l`        | Nastaví **umístění** nezávisle na vlastníkovi skupiny prostředků. Tento parametr je volitelný, ale můžete ho použít k umístění fronty do jiné oblasti než skupinu prostředků. Umístěte ji blízko sebe – vyberte níže ze seznamu dostupných regionů v sandboxu. |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Tady je příklad příkazového řádku s použitím výše uvedených parametrů. Nezapomeňte změnit parametr `--name`.

```azurecli
az storage account create --name [unique-name] -g <rgn>[sandbox resource group name]</rgn> --kind StorageV2 --sku Standard_LRS
```

<!-- Paste tip-->
[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
