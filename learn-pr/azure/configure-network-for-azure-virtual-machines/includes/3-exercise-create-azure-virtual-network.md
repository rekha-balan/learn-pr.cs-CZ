V tomto cvičení vytvoříte v Microsoft Azure virtuální síť. Pak vytvoříte dva virtuální počítače a propojíte je virtuální sítí, která je také připojí k internetu.

> [!IMPORTANT]
> K cvičením v tomto modulu potřebujete plné předplatné Azure. Cvičení jsou volitelná, a nejsou k dokončení tohoto modulu potřeba. Pokud se zúčastníte interaktivních cvičení, která jsou v tomto modulu, budou se vám v předplatném Azure používaném k jejich dokončení účtovat poplatky.  Abyste tyto poplatky minimalizovali, vymažte co nejdříve vytvořené prostředky. Pokyny k vyčištění najdete v závěrečné lekci.

## <a name="log-in-to-your-subscription-with-the-azure-cli"></a>Přihlášení k předplatnému z Azure CLI

V prvním cvičení použijeme Azure CLI. Pokud máte místní instalaci Azure CLI, můžete ji klidně použít, ale nezapomeňte se k používanému předplatnému přihlásit pod účtem `az account`. Pokud místní instalaci nemáte, můžete se k účtu Azure přihlásit na webu [shell.azure.com](https://shell.azure.com) a používat prostředí odtud.

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Nejprve vytvořte skupinu prostředků pro všechny prostředky vytvořené v tomto modulu. Pojmenujte ji `vm-networks`.

```azurecli
az group create --name vm-networks
```

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě

Pokud chcete vytvořit virtuální síť, spusťte následující příkaz a stiskněte Enter.

```azurecli
az network vnet create \
    --name myVnet \
    --resource-group vm-networks \
    --subnet-name default
```

## <a name="create-two-virtual-machines"></a>Vytvoření dvou virtuálních počítačů

Všechny virtuální počítače Azure jsou připojené k virtuální síti. Pokud k vytvoření virtuálního počítače použijete Azure CLI, a nezadáte název stávající virtuální sítě, vyhledá CLI v cílové skupině prostředků příslušnou virtuální síť, kterou použije, podle umístění a dostupnosti podsítě. Pokud vhodnou virtuální síť nenajde, automaticky vytvoří novou.

Tady vytvoříme dva virtuální počítače bez zadávání informací o virtuální síti. Výchozí specifikace sítě odpovídá atributu `myVnet`, který CLI automaticky najde a použije.

1. K vytvoření prvního virtuálního počítače použijte následující příkaz, který vytvoří virtuální počítač s Windows a veřejnou IP adresou, která je přístupná na portu 3389 (přes Vzdálenou plochu). Tím vytvoříte virtuální počítač Windows 2016 Datacenter, který má název `dataProcStage1`.

    ```azurecli
    az vm create \
        --name dataProcStage1 \
        --resource-group vm-networks \
        --admin-username "DataAdmin" \
        --image Win2016Datacenter
    ```

1. Po zobrazení výzev zadejte heslo. Nezapomeňte si heslo poznamenat, protože ho budete potřebovat později pro přístup k serveru.

1. Zkopírujte si hodnotu **publicIpAddress** v souboru JSON vráceném při vytvoření virtuálního počítače, abyste ji mohli později použít.

1. Teď vytvoříte druhý virtuální počítač. Tento virtuální počítač se bude jmenovat `dataProcStage2`, a nebude mít veřejnou IP adresu.

    ```azurecli
    az vm create \
        --name dataProcStage2 \
        --resource-group vm-networks \
        --public-ip-address "" \
        --admin-username "DataAdmin" \
        --image Win2016Datacenter
    ```

## <a name="connect-to-dataprocstage1-using-remote-desktop"></a>Připojení k virtuálnímu počítači dataProcStage1 pomocí Vzdálené plochy

1. Otevřete Vzdálenou plochu a připojte se k virtuálnímu počítači `dataProcStage1` s použitím veřejné IP adresy, kterou jste si poznamenali v předchozích krocích.

1. Pro přihlášení ke vzdálenému počítači použijte vytvořené uživatelské jméno `DataAdmin` a heslo.

1. Ve vzdálené relaci otevřete příkazový řádek Windows a spusťte následující příkaz:

    ```cmd
    ping dataProcStage2 -4
    ```

1. Ve výsledcích vidíte, že u všech požadavků směřovaných na `dataProcStage2` vypršel časový limit. Je to způsobené tím, že výchozí konfigurace brány firewall ve Windows na počítači `dataProcStage2` brání, aby počítač reagoval na příkazy ping.

## <a name="connect-to-dataprocstage2-using-remote-desktop"></a>Připojení k virtuálnímu počítači dataProcStage2 pomocí Vzdálené plochy

Ke konfiguraci brány firewall ve Windows na počítači `dataProcStage2` použijete novou relaci vzdálené plochy. K virtuálnímu počítači `dataProcStage2` ale nebudete mít přístup ze své plochy. Uvědomte si, že `dataProcStage2` nemá veřejnou IP adresu. Pro připojení k `dataProcStage2` použijete Vzdálenou plochu z virtuálního počítače `dataProcStage1`.

1. Ve vzdálené relaci s `dataProcStage1` otevřete Vzdálenou plochu.

1. Připojte se k virtuálnímu počítači `dataProcStage2`. Použijte jeho jméno. Na základě výchozí konfigurace sítě může `dataProcStage1` přeložit adresu virtuálního počítače `dataProcStage2` s použitím jeho názvu.

1. Přihlaste se k virtuálnímu počítači `dataProcStage2`. Použijte vytvořené uživatelské jméno `DataAdmin` a heslo.

1. Na virtuálním počítači `dataProcStage2` klikněte na nabídku Start, zadejte **Firewall** a stiskněte Enter. Zobrazí se konzola **Brána Windows Firewall s pokročilým zabezpečením**.

1. V levém podokně klikněte na **Příchozí pravidla**.

1. V pravém podokně se posuňte dolů, klikněte pravým tlačítkem na **Sdílení souborů a tiskáren (Požadavek na odezvu – ICMPv4-In)** a pak klikněte na **Povolit pravidlo**.

1. Přepněte zpět na vzdálenou relaci s počítačem `dataProcStage1` a na příkazovém řádku spusťte následující příkaz.

    ```cmd
    ping dataProcStage2 -4
    ```

1. Z virtuálního počítače `dataProcStage2` přijdou čtyři odpovědi, které dokazují propojení obou virtuálních počítačů.

Úspěšně jste vytvořili virtuální síť a dva virtuální počítače připojené k této virtuální síti, připojili jste se k jednomu z nich a zobrazili jste síťové připojení ke druhému virtuálnímu počítači ve stejné virtuální síti. Pomocí virtuálních sítí Azure můžete propojovat prostředky v rámci sítě Azure. Tyto prostředky však musí být ve stejné skupině prostředků a stejném předplatném. Dále se podíváme na brány VPN Gateway, které umožňují propojení virtuálních sítí v různých skupinách prostředků, předplatných, a dokonce i v různých geografických umístěních.
