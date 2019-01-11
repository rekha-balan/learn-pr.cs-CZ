Aplikaci zásobníku MEAN můžete, stejně jako většinu aplikačních rozhraní, spouštět v mnoha různých prostředích. Můžete ji spustit na fyzickém počítači v serverovně, na virtuálním počítači nebo v kontejnerech.

Tady spustíte aplikaci na virtuálním počítači v Azure. Zásobník MEAN podporuje různé operační systémy. V našem příkladu použijeme pro studijní účely Ubuntu Linux.

## <a name="create-an-ubuntu-linux-vm"></a>Vytvoření virtuálního počítače s Ubuntu Linuxem

[!include[](../../../includes/azure-sandbox-activate.md)]

Před vytvořením dalších prostředků v Azure napřed normálně vytvořte _skupinu prostředků_. Ve skupině prostředků jsou seskupené příbuzné prostředky. Sandbox je v Azure vaše skupina prostředků. Pokud pracujete ve vlastním předplatném Azure, použijte následující příkaz k vytvoření skupiny prostředků v některém blízkém umístění.

```azurecli
az group create \
  --name <resource-group-name> \
  --location <resource-group-location>
```

1. V editoru Cloud Shell spusťte příkaz `az vm create`, který vytvoří virtuální počítač s Ubuntu.

    ```azurecli
    az vm create \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name MeanStack \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys
    ```

    Provedení příkazu trvá asi dvě minuty. Po provedení příkazu se zobrazí výstup podobný tomuto:

    ```json
    {
      "fqdns": "",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/MeanStack",
      "location": "eastus",
      "macAddress": "00-0D-3A-1E-1B-3B",
      "powerState": "VM running",
      "privateIpAddress": "10.0.0.5",
      "publicIpAddress": "104.211.9.245",
      "resourceGroup": "<rgn>[sandbox resource group name]</rgn>",
      "zones": ""
    }
    ```

    Název virtuálního počítače je „MeanStack“. Použijete ho v dalších příkazech k identifikaci virtuálního počítače, se kterým budete pracovat.

1. Na virtuálním počítači otevřete port 80, abyste povolili příchozí provoz HTTP webové aplikaci, kterou vytvoříte později.

    ```azurecli
    az vm open-port \
      --port 80 \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name MeanStack
    ```

1. Vytvořte připojení SSH k virtuálnímu počítači.

    Veřejná IP adresa vašeho virtuálního počítače se zobrazuje ve výstupu příkazu `az vm create`, ale můžete ji z praktických důvodů uložit do proměnné prostředí Bash.

    Nejprve spusťte příkaz `az vm show`. Tímto příkazem uložíte IP adresu do proměnné prostředí Bash s názvem `ipaddress`.

    ```azurecli
    ipaddress=$(az vm show \
      --name MeanStack \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --show-details \
      --query [publicIps] \
      --output tsv)
    ```

    Připojte se k virtuálnímu počítači:

    ```bash
    ssh azureuser@$ipaddress
    ```

    Na dotaz odpovězte „yes“ a uložte identitu virtuálního počítače lokálně pro budoucí důvěryhodná připojení.

    Připojení SSH nechte otevřené. Použijete ho ke konfiguraci softwaru v dalších částech.

## <a name="summary"></a>Shrnutí

Připravili jste virtuální počítač s Ubuntu a teď jste připraveni na instalaci jednotlivých komponent zásobníku MEAN. Začnete instalací MongoDB.