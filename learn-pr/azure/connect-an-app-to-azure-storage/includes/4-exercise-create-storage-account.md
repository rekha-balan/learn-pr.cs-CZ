Když teď máme aplikaci, potřebujeme účet Azure Storage, abychom s ní mohli pracovat.

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Použití Azure CLI k vytvoření účtu Azure Storage

K vytvoření nového účtu úložiště použijeme příkaz `az storage account create`. Pro řízení konfigurace účtu úložiště se používá několik parametrů.

> [!div class="mx-tableFixed"]
> | Možnost | Popis |
> |--------|-------------|
> | `--name` | **Název účtu úložiště** Název bude sloužit pro vygenerování veřejné adresy URL používané pro přístup k datům v účtu. Musí být jedinečný v rámci všech existujících názvů účtů úložiště v Azure. Musí být dlouhý 3 až 24 znaků a může obsahovat pouze malá písmena a číslice. |
> | `--resource-group` | K umístění účtu do bezplatného sandboxu použijte **<rgn>[název skupiny prostředků sandboxu]</rgn>**. |
> | `--location` | Vyberte umístění blízko vás (viz níže). |
> | `--kind` | Určíte tak _typ_ účtu úložiště. Dostupné možnosti: `BlobStorage`, `Storage` a `StorageV2`. |
> | `--sku` | Tato možnost rozhoduje o výkonu účtu úložiště a modelu replikace. Dostupné možnosti: `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS` a `Standard_ZRS`. |
> | `--access-tier` | **Úroveň přístupu** se používá pouze pro úložiště objektů Blob. Dostupné možnosti: [`Cool` \| `Hot`]. **Horká úroveň přístupu** je ideální pro data, která se používají často, a **studená úroveň přístupu** je lepší pro méně často využívaná data. Všimněte si, že tímto se nastaví pouze _výchozí_ hodnota. Při vytváření objektu blob můžete pro data nastavit jinou hodnotu. |
    
Výše uvedenou tabulku použijte pro vytvoření příkazového řádku v Cloud Shellu vpravo a vytvořte účet.
- Použijte jedinečný název. Doporučujeme vám něco jako „photostore“ s vašimi iniciálami a náhodným číslem. Pokud název nebude jedinečný, zobrazí se vám chyba.
- Obvykle bychom k uchovávání prostředků aplikace vytvořili novou skupinu prostředků, v tomto případě ale použijeme skupinu prostředků sandboxu s názvem **<rgn>[název skupiny prostředků sandboxu]</rgn>**.
- Pro **sku** vyberte Standard_LRS. Použijete tak standardní úložiště s místní replikací, což je pro účely tohoto příkladu vhodné.
- Pro **Úroveň přístupu** použijte studenou úroveň.

### <a name="selecting-a-location"></a>Výběr umístění
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>Ukázkový příkaz

Následující ukázkový příkaz můžete použít k vytvoření účtu úložiště. Nezapomeňte, že je třeba nahradit <kbd><name></kbd> a <kbd><region></kbd> informacemi, které jste zvolili v předchozích částech.

```azurecli
az storage account create \
        --name <name> \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \
        --access-tier Cool
```

> [!TIP]
> Pokud vás zajímají možnosti účtu úložiště, projděte si modul **Vytvoření účtu Azure Storage**, ve kterém se těmto možnostem věnujeme hlouběji.

Nasazení účtu může několik minut trvat. Zatímco Azure nasazuje účet, podívejme se podrobněji na rozhraní API, která budeme s tímto účtem používat.
