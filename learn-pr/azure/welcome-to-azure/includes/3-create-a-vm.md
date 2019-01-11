Jako odborník na technologie pravděpodobně máte odborné znalosti v určité oblasti. Možná jste správce úložiště nebo expert na virtualizaci nebo se možná zaměřujete na nejnovější postupy zabezpečení. Pokud jste student, možná stále objevujete, co vás zajímá nejvíce.

::: zone pivot="windows-cloud"

Ať je vaše role jakákoli, většina lidí začíná s využíváním cloudu tak, že si vytvoří virtuální počítač. Tady si vytvoříte virtuální počítač s Windows Serverem 2016.

::: zone-end

::: zone pivot="linux-cloud"

Ať je vaše role jakákoli, většina lidí začíná s využíváním cloudu tak, že si vytvoří virtuální počítač. Tady si vytvoříte virtuální počítač s Ubuntu 16.04.

::: zone-end

V Azure můžete virtuální počítač vytvořit mnoha způsoby. Tady vytvoříte virtuální počítač se systémem Windows nebo Linux pomocí interaktivního terminálu Cloud Shell. Pokud každý den pracujete v terminálu, víte, že je to často nejrychlejší způsob, jak něco udělat.

::: zone pivot="windows-cloud"

> [!TIP]
> Preferujete Linux, nebo chcete vyzkoušet něco nového? Pokud chcete spustit virtuální počítač s Linuxem, v horní části této stránky vyberte **Linux**.

::: zone-end

::: zone pivot="linux-cloud"

> [!TIP]
> Preferujete Windows, nebo chcete vyzkoušet něco nového? Pokud chcete spustit virtuální počítač s Windows Serverem, v horní části této stránky vyberte **Windows**.

::: zone-end

Pojďme si osvěžit některé základní pojmy a zprovoznit první virtuální počítač.

## <a name="what-is-a-virtual-machine"></a>Co je virtuální počítač?

Virtuální počítač (také se označuje zkratkou VM) je softwarová emulace fyzického počítače. Protože virtuální počítače existují jako software, je možné za pár minut vygenerovat desítky, stovky nebo i tisíce virtuálních počítačů Azure, a pak, když už je nepotřebujete, je odstranit. Díky nízkým sazbám a účtování po minutách platíte jenom za výpočetní prostředky, které využíváte, a jen za dobu, po kterou je využíváte. Navíc si můžete virtuální počítače mnoha způsoby nakonfigurovat podle svých potřeb.

::: zone pivot="windows-cloud"

Snímek spuštěného virtuálního počítače se nazývá _image_. Azure poskytuje image pro Windows a několik typů Linuxu. Můžete také vytvořit vlastní, předem nakonfigurované image, abyste urychlili jejich nasazení. Tady si vytvoříte virtuální počítač s Windows Serverem 2016 od Microsoftu.

::: zone-end

::: zone pivot="linux-cloud"

Snímek spuštěného virtuálního počítače se nazývá _image_. Azure poskytuje image pro Windows a několik typů Linuxu. Můžete také vytvořit vlastní, předem nakonfigurované image, abyste urychlili jejich nasazení. Tady si vytvoříte virtuální počítač s Ubuntu 16.04 od Canonical.

::: zone-end

## <a name="what-defines-a-virtual-machine-on-azure"></a>Co definuje virtuální počítač v Azure?

Virtuální počítač je definován několika faktory včetně jeho velikosti a umístění. Ještě než si virtuální počítač vytvoříte, pojďme stručně nastínit, co to obnáší.

:::row:::
    :::column:::
        **Size**
    :::column-end:::
    :::column span="3":::
_Velikost_ virtuálního počítače definuje rychlost jeho procesoru, velikost paměti, počáteční velikost úložiště a očekávanou šířku pásma sítě. Některé velikosti v sobě zahrnují speciální hardware, jako je například GPU pro vykreslování náročné grafiky a střih videa.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Region**
    :::column-end:::
    :::column span="3":::
Azure se skládá z datových center rozmístěných po celém světě. _Oblast_ je sada datových center Azure v pojmenované zeměpisné oblasti. Každý prostředek Azure, včetně virtuálních počítačů, má přiřazenou oblast. Příklady oblastí jsou Východní USA a Severní Evropa.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Network**
    :::column-end:::
    :::column span="3":::
_Virtuální síť_ je logicky izolovaná síť v Azure. Každý virtuální počítač v Azure je přidružený k určité virtuální síti. Azure poskytuje brány firewall na úrovni cloudu pro vaše virtuální sítě, nazývané _skupiny zabezpečení sítě_.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Resource groups**
    :::column-end:::
    :::column span="3":::
Virtuální počítače a jiné cloudové prostředky jsou seskupené do logických kontejnerů nazývaných _skupiny prostředků_. Skupiny se zpravidla používají k uspořádání sad prostředků, které se nasazují společně jako součást nějaké aplikace nebo služby. Na skupinu prostředků můžete odkazovat pomocí jejího názvu.
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a>Co je Azure Cloud Shell?

Azure Cloud Shell je prostředí příkazového řádku založené na prohlížeči pro správu a vývoj prostředků Azure. Cloud Shell si můžete představit jako interaktivní konzolu, která běží v cloudu.

Cloud Shell poskytuje na výběr dvě možnosti prostředí: Bash a PowerShell. Obě zahrnují přístup k Azure CLI, rozhraní příkazového řádku pro Azure.

Můžete použít jakékoli rozhraní pro správu Azure, včetně webu Azure Portal, Azure CLI nebo Azure PowerShellu, ke správě jakéhokoli virtuálního počítače. Pro účely výuky zde použijeme k vytvoření a správě virtuálního počítače s Windows nebo s Linuxem rozhraní Azure CLI.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-resources-in-azure"></a>Vytvoření prostředků v Azure

První věcí, kterou bychom za normálních okolností udělali, je vytvoření _skupiny prostředků_ pro uchování všeho, co budeme potřebovat vytvořit. To nám umožní spravovat všechny virtuální počítače, disky, síťová rozhraní a další prvky, ze kterých se naše řešení skládá. Můžeme přes Azure CLI vytvořit skupinu prostředků pomocí příkazu `az group create`. Ten má dva parametry: `--name` pro zadání jedinečného názvu v rámci našeho předplatného a `--location`, jehož hodnota Azure říká, v jaké části světa mají být prostředky ve výchozím nastavení umístěné.

Vzhledem k tomu, že jste v bezplatném sandboxovém prostředí Azure, nemusíte tento krok provádět. Místo toho použijete předem vytvořenou skupinu prostředků **<rgn>[název skupiny prostředků]</rgn>**.

## <a name="choosing-a-location"></a>Volba umístění

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

::: zone pivot="windows-cloud"

## <a name="create-a-windows-vm"></a>Vytvoření virtuálního počítače s Windows

Pojďme zprovoznit váš virtuální počítač s Windows.

1. V Cloud Shellu na okraji této stránky spusťte následující příkazy, které vytvoří uživatelské jméno a vygenerují náhodné heslo.

    ```bash
    USERNAME=azureuser
    PASSWORD=$(openssl rand -base64 32)
    ```

    Uživatelské jméno a heslo se zde ukládají jako proměnné prostředí Bash. Pokud používáte PowerShell, stačí vědět, že proměnné prostředí Bash jsou podobné. Tyto proměnné použijete v dalším kroku k nastavení jména a hesla správce Windows.

    V tomto příkladu se jako uživatelské jméno používá **azureuser**. Toto jméno ale můžete podle potřeby změnit.

    Existuje mnoho způsobů, jak generovat hesla. Příkaz uvedený tady je jenom jedním z nich.

1. Spuštěním následujícího příkazu `az vm create` vytvořte virtuální počítač. Příkaz vytvoří virtuální počítač v umístění USA – východ, ale můžete je změnit na kterékoli z umístění uvedených výše.

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --location eastus \
      --admin-username $USERNAME \
      --admin-password $PASSWORD
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    Váš virtuální počítač bude aktivovaný za 4 až 5 minut. Porovnejte to s časem potřebným k nákupu, umístění a konfiguraci systému ve vašem datacentru. Docela rozdíl!

Zatímco čekáte, pojďme si zopakovat příkaz, který jste právě spustili.

* Virtuální počítač má název **myVM**. Tento název identifikuje virtuální počítač v Azure. Bude to také interní název hostitele virtuálního počítače, neboli název počítače.
* Skupina prostředků (nebo logický kontejner virtuálního počítače) je pojmenována ** <rgn>[název skupiny prostředků sandboxu]</rgn>**.
* **Win2016Datacenter** určuje image virtuálního počítače s Windows Serverem 2016.
* **Standard_DS2_v2** odkazuje na velikost virtuálního počítače. Váš virtuální počítač má teď 2 virtuální procesory a 7 GB paměti.
* Uživatelské jméno a heslo vám umožňují připojit se k vašemu virtuálnímu počítači později. Můžete se třeba připojit přes vzdálenou plochu nebo WinRM, abyste mohli pracovat se systémem a nakonfigurovat ho.

> [!NOTE]
> Teď máte všechno, co potřebujete k přímému připojení k virtuálnímu počítači, ale ještě chvilku počkejte.
> V další lekci si ukážeme jeden způsob připojení k virtuálnímu počítači, který dovoluje měnit jeho konfiguraci.

Ve výchozím nastavení Azure přiřadí vašemu virtuálnímu počítači veřejnou IP adresu. Virtuální počítač můžete nakonfigurovat, aby byl přístupný z Internetu, nebo jenom z interní sítě.

Můžete se také podívat na toto krátké video o některých možnostech, které máte pro vytvoření a správu virtuálních počítačů.

#### <a name="options-to-create-and-manage-vms"></a>Možnosti pro vytváření a správu virtuálních počítačů

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Až bude virtuální počítač připravený, zobrazí se o tom zpráva. Tady je příklad.

```json
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

> [!NOTE]
> Na webu Microsoft Learn se můžete často setkat s ukázkovým výstupem, který se podobá předchozímu příkladu. Tyto příklady vám ukazujeme, abyste je mohli porovnat s výstupem, který vidíte u sebe. Jde ale jenom o příklad, takže ho nezadávejte do relace Cloud Shellu.

## <a name="verify-your-vm-is-running"></a>Ověření spuštění virtuálního počítače

Příkaz `az vm create` proběhl úspěšně, ale pojďme ověřit, jestli je virtuální počítač připravený a spuštěný.

1. Spusťte příkaz `az vm get-instance-view`, kterým ověříte úspěšné vytvoření virtuálního počítače a jeho spuštění.

    ```azurecli
    az vm get-instance-view \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --output table
    ```

    Zobrazený výstup bude vypadat přibližně takto:

    ```azurecli
    Name    ResourceGroup                         Location    ProvisioningState    PowerState
    ------  ------------------------------------  ----------  -------------------  ------------
    myVM    <rgn>[sandbox resource group name]</rgn>  eastus      Succeeded            VM running
    ```

    Zobrazí se název virtuálního počítače, skupina prostředků a jeho umístění. Dále vidíte, že virtuální počítač je úspěšně zřízený nebo vytvořený a že je spuštěný.

::: zone-end

::: zone pivot="linux-cloud"

## <a name="create-a-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem

Pojďme zprovoznit váš virtuální počítač s Linuxem.

1. Z prostředí Cloud Shell na pravé straně této stránky spusťte příkaz `az vm create` pro vytvoření virtuálního počítače. Následující příkaz vytvoří virtuální počítač v umístění USA – východ, což můžete změnit na kterékoli z umístění uvedených výše – doporučujeme vybrat umístění, které máte blízko.

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --location eastus \
      --size Standard_DS2_v2 \
      --admin-username azureuser \
      --generate-ssh-keys
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    Váš virtuální počítač bude aktivovaný asi za 2 minuty. Porovnejte to s časem potřebným k nákupu, umístění a konfiguraci systému ve vašem datacentru. Docela rozdíl!

Zatímco čekáte, pojďme si zopakovat příkaz, který jste právě spustili.

* Virtuální počítač má název **myVM**. Tento název identifikuje virtuální počítač v Azure. Bude to také interní název hostitele virtuálního počítače, neboli název počítače.
* Skupina prostředků (nebo logický kontejner virtuálního počítače) je pojmenována ** <rgn>[název skupiny prostředků sandboxu]</rgn>**.
* **UbuntuLTS** určuje image virtuálního počítače s Ubuntu 16.04 LTS.
* **Standard_DS2_v2** odkazuje na velikost virtuálního počítače. Váš virtuální počítač má teď dva virtuální procesory a 7 GB paměti.
* Možnost `--admin-username` definuje, že uživatelem virtuálního počítače je **azureuser**. Uživatelské jméno můžete podle potřeby změnit.
* Možnost `--generate-ssh-keys` vytvoří pár klíčů SSH, které vám umožní se k virtuálnímu počítači přihlásit.

> [!NOTE]
> Teď máte všechno, co potřebujete k přímému připojení k virtuálnímu počítači, ale ještě chvilku počkejte.
> V další lekci si ukážeme jeden způsob připojení k virtuálnímu počítači, který dovoluje měnit jeho konfiguraci.

Ve výchozím nastavení Azure přiřadí vašemu virtuálnímu počítači veřejnou IP adresu. Virtuální počítač můžete nakonfigurovat, aby byl přístupný z Internetu, nebo jenom z interní sítě.

Můžete se také podívat na toto krátké video o některých možnostech, které máte pro vytvoření a správu virtuálních počítačů.

#### <a name="options-to-create-and-manage-vms"></a>Možnosti pro vytváření a správu virtuálních počítačů

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Až bude virtuální počítač připravený, zobrazí se o tom zpráva. Tady je příklad.

```json
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1D-EB-02",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "137.135.110.210",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

> [!NOTE]
> Na webu Microsoft Learn se můžete často setkat s ukázkovým výstupem, který se podobá předchozímu příkladu. Tyto příklady vám ukazujeme, abyste je mohli porovnat s výstupem, který vidíte u sebe. Jde ale jenom o příklad, takže ho nezadávejte do relace Cloud Shellu.

## <a name="verify-your-vm-is-running"></a>Ověření spuštění virtuálního počítače

Příkaz `az vm create` proběhl úspěšně, ale pojďme ověřit, jestli je virtuální počítač připravený a spuštěný.

1. Spusťte příkaz `az vm get-instance-view`, kterým ověříte úspěšné vytvoření virtuálního počítače a jeho spuštění.

    ```azurecli
    az vm get-instance-view \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --output table
    ```

    Zobrazený výstup bude vypadat přibližně takto:

    ```azurecli
    Name    ResourceGroup                         Location    ProvisioningState    PowerState
    ------  ------------------------------------  ----------  -------------------  ------------
    myVM    <rgn>[sandbox resource group name]</rgn>  eastus      Succeeded            VM running
    ```

    Zobrazí se název virtuálního počítače, skupina prostředků a jeho umístění. Dále vidíte, že virtuální počítač je úspěšně zřízený nebo vytvořený a že je spuštěný.

::: zone-end

## <a name="summary"></a>Shrnutí

Stačí zvládnout jen pár konceptů a jste schopni zprovoznit virtuální počítač v Azure za několik minut. Mnohé z těchto konceptů, jako je například velikost virtuálního počítače a pravidla brány firewall, jsou vám již nejspíš známé.

V dalším kroku nainstalujete na virtuální počítač webový server a nakonfigurujete ho, aby zajišťoval provoz základního webu.