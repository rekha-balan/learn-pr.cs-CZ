K vytvoření virtuálního počítače pomocí image jsme použili `Debian`. Azure má několik standardních imagí virtuálních počítačů, které můžete použít k vytvoření virtuálního počítače. 

## <a name="listing-images"></a>Výpis imagí

Seznam dostupných imagí získáte následujícím příkazem. 

```azurecli
az vm image list --output table
```

Výstupem budou nejoblíbenější image, které jsou v offline seznamu integrovaném do Azure CLI. Na Azure Marketplace jsou ale k dispozici _stovky_ imagí, ze kterých si můžete vybrat. 

## <a name="getting-all-images"></a>Získání seznamu všech imagí

Seznam všech imagí získáte tak, že do příkazu přidáte příznak `--all`. Vzhledem k tomu, že je seznam imagí v Azure Marketplace je velice dlouhý, doporučujeme ho filtrovat podle kritérií `--publisher`, `--sku` nebo `–-offer`.

Třeba následujícím příkazem zobrazíte _všechny_ dostupné image Wordpressu:

```azurecli
az vm image list --sku Wordpress --output table --all
```

Nebo tímto příkazem zobrazíte všechny image od Microsoftu:

```azurecli
az vm image list --publisher Microsoft --output table --all
```

## <a name="location-specific-images"></a>Image z určitého umístění

Některé image jsou dostupné jenom v určitých umístěních. Zkuste k příkazu přidat příznak `--location [location]`, abyste omezili výsledky pouze na image v oblasti, ve které chcete virtuální počítač vytvořit. Zkuste například do Azure Cloud Shellu zadat následující příkaz, abyste získali seznam imagí dostupných v oblasti `eastus`.

```azurecli
az vm image list --location eastus --output table
```

Vyzkoušejte některé image, které jsou v dalších dostupných umístěních sandboxu Azure:

[!include[](../../../includes/azure-sandbox-regions-note.md)]

> [!TIP]
> Toto jsou standardní image poskytnuté službou Azure. Nezapomeňte, že také můžete [vytvořit a nahrát vlastní image](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images), ze kterých vytvoříte virtuální počítače založené na jedinečných konfiguracích nebo na méně běžných verzích a distribucích operačního systému.

Příkazové okno je teď pravděpodobně plné. Pokud chcete, můžete zadáním příkazu `clear` obrazovku vymazat.