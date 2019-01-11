Začněme nejčastějším úkolem, kterým je vytvoření virtuálního počítače Azure.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="logins-subscriptions-and-resource-groups"></a>Přihlášení, předplatná a skupiny prostředků

Budete pracovat napravo ve službě Azure Cloud Shell. Po aktivaci sandboxu budete přihlášeni k Azure v rámci bezplatného předplatného spravovaného v prostředí Microsoft Learn. Nemusíte se sami přihlašovat k Azure ani vybírat předplatné, protože to uděláme za vás. Normálně byste také vytvořili _skupinu prostředků_, ve které se budou nové prostředky uchovávat. V tomto modulu Azure Sandbox vytvoří skupinu prostředků, kterou použijete ke spuštění všech příkazů.

## <a name="create-a-linux-vm-with-the-azure-cli"></a>Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI

Azure CLI obsahuje příkaz `vm` pro práci s virtuálními počítači v Azure. Můžeme použít několik podpříkazů pro provedení konkrétních úkolů. Nejběžnější příkazy:

| Podpříkaz | Popis |
|-------------|-------------|
| `create`    | Vytvoření nového virtuálního počítače |
| `deallocate` | Uvolnění virtuálního počítače |
| `delete` | Odstranění virtuálního počítače |
| `list` | Zobrazení seznamu vytvořených virtuálních počítačů v předplatném |
| `open-port` | Otevření konkrétního síťového portu pro příchozí provoz |
| `restart` | Restartování virtuálního počítače |
| `show` | Získání podrobných informací o virtuálním počítači |
| `start` | Spuštění zastaveného virtuálního počítače |
| `stop` | Zastavení spuštěného virtuálního počítače |
| `update` | Aktualizace vlastnosti virtuálního počítače |

> [!NOTE]
> Kompletní seznam příkazů najdete v [referenční dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).

Začněme prvním příkazem: `az vm create`. Tento příkaz slouží k vytvoření virtuálního počítače ve skupině prostředků. K dispozici je několik parametrů, jejichž předáním můžete konfigurovat všechny aspekty nového virtuálního počítače. Následující tři parametry je nutné zadat:

> [!div class="mx-tableFixed"]
> | Parametr | Popis |
> |-----------|-------------|
> | `resource-group` | Skupina prostředků, která tento virtuální počítač vlastní. Použijte **<rgn>[skupinu prostředků sandboxu]</rgn>**. |
> | `name` | Název virtuálního počítače musí být v rámci skupiny prostředků jedinečný. |
> | `image` | Image operačního systému použitého k vytvoření virtuálního počítače. |
> | `location` | Oblast, ve které bude virtuální počítač. Obvykle by byl v blízkosti uživatele virtuálního počítače. V tomto cvičení vybereme nejbližší umístění z následujícího seznamu. |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Je také vhodné použít příznak `--verbose`, abychom mohli sledovat průběh vytváření virtuálního počítače. 

## <a name="create-a-linux-virtual-machine"></a>Vytvoření virtuálního počítače s Linuxem

Teď vytvoříme nový virtuální počítač s Linuxem. Spuštěním následujícího příkazu v Azure Cloud Shellu vytvořte počítač s Linuxem Debian v oblasti Západní USA. Pokud je toto umístění od vás daleko, změňte ho.

```azurecli
az vm create --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --location westus --verbose 
```

[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


Tímto příkazem vytvoříte nový virtuální počítač s Linuxem **Debian**, který má název `SampleVM`. Všimněte si, že při vytváření virtuálního počítače nástroj Azure CLI čeká. Pokud chcete dát nástroji Azure CLI pokyn, aby se ihned vrátil a služba Azure pokračovala ve vytváření virtuálního počítače na pozadí, přidejte možnost `--no-wait`. Je to užitečné, když spouštíte příkaz ve skriptu. V další části skriptu použijete příkaz `azure vm wait --name [vm-name]`, aby se počkalo na vytvoření virtuálního počítače.

Když si prohlédnete podrobné odpovědi, uvidíte také, že název `SampleVM` se používá k pojmenování různých závislostí pro příslušný virtuální počítač.

```output
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

Tyto automaticky generované názvy prostředků můžete přepsat volitelnými parametry příkazu `vm create`, jako je `--vnet-name` nebo `--public-ip-address-dns-name`.

K zadání názvu účtu správce použijeme příznak `admin-username`. Název bude **aldis**. Pokud tento krok vynecháte, příkaz `vm create` použije _jméno aktuálního uživatele_. Vzhledem k tomu, že pravidla pro názvy účtů se u jednotlivých operačních systémů liší, je bezpečnější zadat konkrétní název. 

> [!NOTE]
> Běžné názvy jako root nebo admin nejsou pro většinu imagí povolené.

Používáme také příznak `generate-ssh-keys`. Tento parametr se používá pro linuxové distribuce a vytváří dvojici klíčů zabezpečení, takže pro vzdálený přístup k tomuto virtuálnímu počítači můžeme použít nástroj `ssh`. Příslušné dva soubory se uloží do složky `.ssh` ve vašem počítači a ve virtuálním počítači. Pokud už v cílové složce máte klíč SSH nazvaný `id_rsa`, použije se tento klíč. Nový klíč se v takovém případě nebude generovat.

Jakmile se dokončí vytváření virtuálního počítače, zobrazí se odpověď JSON, která obsahuje aktuální stav tohoto virtuálního počítače a jeho veřejné a privátní IP adresy, které přiřadila platforma Azure:

```json
{
  "fqdns": "",
  "id": "/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-58-F8-45",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
  "resourceGroup": "2568d0d0-efe3-4d04-a08f-df7f009f822a",
  "zones": ""
}
```
