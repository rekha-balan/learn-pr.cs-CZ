Vaše právní firma rozšiřuje své portfolio a vy jste dostali úkol vytvořit nový webový server Linux k ukládání nejdůležitějších dokumentů ze široké škály zdrojů – od klientů, dalších právních firem a dalších úřadů pro vymáhání práv. Webový server umožňuje uživatelům nahrávat dokumenty a ukládat je na disk.

> [!TIP]
> Toto cvičení používá jako příklad Linux, ale základní proces vytváření virtuálních počítačů a přidávání disků je stejný i pro Windows. Hlavní rozdíl je ve vytváření oddílů disku a jeho formátování. Ve Windows se můžete ke svému virtuálnímu počítači připojit prostřednictvím vzdálené plochy a používat integrované nástroje pro správu disků nebo nasadit skript PowerShellu, který je podobný zde používanému skriptu prostředí Bash.

Cílem je vytvořit virtuální počítač s Linuxem a připojit nový virtuální pevný disk (VHD) nazvaný **uploadDataDisk1**, na kterém bude uložený adresář `/uploads`.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="set-azure-cli-default-values"></a>Nastavení výchozích hodnot Azure CLI

Azure CLI umožňuje nastavit výchozí hodnoty, abyste je nemuseli při každém spuštění příkazu zadávat stále dokola.

Tady zadáte výchozí umístění Azure neboli oblast. Jedná se o umístění, ve kterém se bude virtuální počítač Azure nacházet.

V ideálním případě by to mělo být blízko vašim klientům. V tomto případě vyberte sobě nejbližší oblast z umístění dostupných sandboxu Azure.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Spuštěním příkazu `az configure` nastavte výchozí umístění, které chcete použít. Nahraďte **eastus** umístěním zvoleným v předchozím kroku.

    ```azurecli
    az configure --defaults location=eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Nastavte výchozí název skupiny prostředků na skupinu předem nakonfigurovaných prostředků vytvořenou pro vás prostřednictvím Azure sandboxu: **<rgn>[skupina prostředků sandboxu]</rgn>**

    ```azurecli
    az configure --defaults group="<rgn>[sandbox Resource Group]</rgn>"
    ```

## <a name="create-a-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem

Tady vytvoříte virtuální počítač s Linuxem pro hostování webového serveru.

1. Spusťte příkaz `az vm create` pro vytvoření virtuálního počítače s Ubuntu Linuxem.

    ```azurecli
    az vm create \
      --name support-web-vm01 \
      --image UbuntuLTS \
      --size Standard_DS2_v2 \
      --admin-username azureuser \
      --generate-ssh-keys
    ```

    * Název virtuálního počítače je **support-web-vm01**.
    * Jeho velikost je **Standard_DS2_v2**.
    * Uživatelské jméno správce je **azureuser**. Ve skutečnosti si můžete zvolit libovolný název.
    * Argument `--generate-ssh-keys` vygeneruje dvojici klíčů SSH a umožní vám tak připojení k virtuálnímu počítači přes SSH.

    Spuštění virtuálního počítače trvá několik minut. Až bude virtuální počítač připravený, zobrazí se o tom zpráva ve formátu JSON. Tady je příklad.

    ```json
    {
      "fqdns": "",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/680469d8-edB7-42ec-b118-cd80d51741e7/providers/Microsoft.Compute/virtualMachines/support-web-vm01",
      "location": "eastus",
      "macAddress": "00-0D-3A-10-63-0A",
      "powerState": "VM running",
      "privateIpAddress": "10.0.0.4",
      "publicIpAddress": "104.211.38.211",
      "resourceGroup": "680469d8-edB7-42ec-b118-cd80d51741e7",
      "zones": ""
    }
    ```

    > [!NOTE]
    > V této lekci používáte tento virtuální počítač k učení, jak spravovat disky. V praxi můžete také nainstalovat webový server a další software a pak spustit příkaz `az vm open-port` ke zpřístupnění potřebných portů pro vnější svět.

## <a name="add-an-empty-data-disk-to-your-vm"></a>Přidání prázdného datového disku k virtuálnímu počítači

Vytvoříte prázdný datový disk a připojíte ho k virtuálnímu počítači. Datový disk bude mít nejdříve velikost 64 GB. Později tento disk připojíte k adresáři `/uploads` ve vašem virtuálním počítači.

> [!TIP]
> Pro účely výuky vytvoříte virtuální počítač a datový disk v samostatných krocích. V praxi můžete určit argument `--data-disk-sizes-gb` v příkazu `az vm create` a přidat s ním datové disky po vytvoření virtuálního počítače.

1. Spuštěním následujícího příkazu `az vm disk attach` přidejte nový prázdný disk do virtuálního počítače.

    ```azurecli
    az vm disk attach \
      --vm-name support-web-vm01 \
      --disk uploadDataDisk1 \
      --size-gb 64 \
      --sku Premium_LRS \
      --new
    ```

    Tento příkaz:

    * Pojmenuje disk **uploadDataDisk1**.
    * Nastaví jeho velikost na 64 GB.
    * Určuje, že se má použít úložiště úrovně Premium s místní redundancí.

Abyste disk mohli používat, musíte vytvořit oddíly a naformátovat ho. Uděláte to za chvíli.

## <a name="initialize-and-format-your-data-disk"></a>Inicializace a formátování datového disku

Prázdný datový disk je potřeba inicializovat a naformátovat. Proces je stejný jako u fyzického disku.

U jednorázových úloh se můžete k virtuálnímu počítači připojit ručně přes SSH a spustit potřebné příkazy. Aby byl proces opakovatelnější a méně náchylný k chybám, můžete použít skript Bash (nebo skript prostředí PowerShell, pokud je dostupný), který specifikuje potřebné příkazy.

Když k automatizaci procesu použijete skript, získáte další výhodu &ndash; prostřednictvím skriptu zdokumentujete provedení procesu. Ostatní uživatelé si můžou váš skript přečíst a pochopit, jak je systém nakonfigurovaný. Pokud chcete proces změnit, stačí jednoduše upravit skript a otestovat ho na dočasném pomocném virtuálním počítači. Teprve potom změnu nasaďte do produkčního prostředí.

K automatizaci procesu v této lekci použijete _rozšíření vlastních skriptů_. Rozšíření vlastních skriptů představuje snadný způsob, jak stahovat a spouštět skripty na virtuálních počítačích Azure. Je to jen jeden z mnoha způsobů, jak můžete systém nakonfigurovat po zprovoznění virtuálního počítače.

Skripty můžete ukládat v úložišti Azure nebo ve veřejném umístění, jako je GitHub. Skripty můžete spouštět ručně nebo v rámci automatizovanějšího nasazení. Tady spustíte příkaz na rozhraní příkazového řádku Azure, kterým stáhnete předpřipravený skript Bash z GitHubu a spustíte ho na virtuálním počítači.

Pro účely výuky teď také na virtuálním počítači spustíte několik příkazů, abyste ověřili, že je virtuální počítač nakonfigurovaný podle vašich očekávání.

1. Spuštěním příkazu `az vm show` získejte veřejnou IP adresu virtuálního počítače a tuto IP adresu uložte jako proměnnou Bash.

    ```azurecli
    ipaddress=$(az vm show \
      --name support-web-vm01 \
      --show-details \
      --query [publicIps] \
      --o tsv)
    ```

1. Spuštěním následujícího příkazu `ssh` spusťte příkaz `lsblk` na virtuálním počítači přes připojení SSH pomocí dat proměnné `ipaddress`, kterou jste právě vytvořili. Vzpomeňte si, že `azureuser` bylo uživatelské jméno správce, které jsme použili, když jsme vytvořili virtuální počítač. Pokud jste si zvolili jiné jméno, použijte ho. Po zobrazení výzvy zadejte „ano“.

    ```bash
    ssh azureuser@$ipaddress lsblk
    ```

    Výstup tohoto příkazu by měl vypadat asi takto.

    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk 
    └─sdb1   8:17   0   14G  0 part /mnt
    sr0     11:0    1  628K  0 rom  
    sdc      8:32   0   64G  0 disk 
    sda      8:0    0   30G  0 disk 
    └─sda1   8:1    0   30G  0 part /
    ```

    Zobrazí se 64GB disk `sdc`, který jste právě vytvořili. Vidíte, že není připojený. Je to proto, že ještě nebyl inicializován.

1. Spuštěním následujícího příkazu `az vm extension set` spusťte na virtuálním počítači předpřipravený skript Bash.

    > [!WARNING]
    > Skript upraví `/etc/fstab`. Pokud byste soubor `/etc/fstab` upravili nesprávně, může se stát, že systém nepůjde spustit. Změny konfigurace vždy testujte na dočasném pomocném systému, než je nasadíte do produkčního prostředí. Pokud chcete zjistit, jak správně upravit tento soubor, podívejte se do dokumentace k vaší distribuci. V produkčním prostředí dále doporučujeme vytvořit zálohu tohoto souboru, abyste si mohli konfiguraci v případě potřeby obnovit.

    ```azurecli
    az vm extension set \
      --vm-name support-web-vm01 \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-add-and-size-disks-in-azure-virtual-machines/master/add-data-disk.sh"]}' \
      --protected-settings '{"commandToExecute": "./add-data-disk.sh"}'
    ```

    Pokud budete chtít, můžete si v době spuštění příkazu [prostudovat příslušný skript Bash](https://raw.githubusercontent.com/MicrosoftDocs/mslearn-add-and-size-disks-in-azure-virtual-machines/master/add-data-disk.sh?azure-portal=true) na samostatné kartě prohlížeče.

    Abychom to shrnuli, skript:

    * Rozdělí jednotku `/dev/sdc` na oddíly.
    * Vytvoří na jednotce systém souborů ext4.
    * Vytvoří adresář `/uploads`, který použijeme jako přípojný bod.
    * Připojí disk k přípojnému bodu.
    * Aktualizuje `/etc/fstab` tak, aby se jednotka po restartování systému automaticky připojila.

1. Pokud chcete konfiguraci ověřit, spusťte stejný příkaz `ssh`, kterým jste předtím spustili příkaz `lsblk` na virtuálním počítači přes připojení SSH.

    ```bash
    ssh azureuser@$ipaddress lsblk
    ```

    Uvidíte, že `sdc/sdc1` je rozdělený na oddíly a dle očekávání připojený k adresáři `/uploads`.

    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk 
    └─sdb1   8:17   0   14G  0 part /mnt
    sr0     11:0    1  628K  0 rom  
    sdc      8:32   0   64G  0 disk 
    └─sdc1   8:33   0   64G  0 part /uploads
    sda      8:0    0   30G  0 disk 
    └─sda1   8:1    0   30G  0 part /
    ```

> [!TIP]
> Některá linuxová jádra podporují příkaz TRIM k zahození nepoužívaných bloků na discích. Tato funkce je k dispozici na discích Azure. Ušetří vám peníze, pokud vytváříte velké soubory a pak je odstraňujete. V dokumentaci k Azure najdete pokyny, jak [tuto funkci zapnout](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure?azure-portal=true).

## <a name="summary"></a>Shrnutí

Vytvořili jste datový disk a připojili ho k virtuálnímu počítači. Použili jste rozšíření vlastních skriptů ke spuštění předpřipraveného skriptu Bash na virtuálním počítači, aby byl proces opakovatelnější. Skript Bash rozdělí disk na oddíly, naformátuje ho a připojí ho tak, aby na něj mohl webový server zapisovat.

Teď, když jste si připravili datový disk ve virtuálním počítači, pojďme zjistit trochu více o různých typech disků, které můžete vytvořit. Nejdůležitějším rozhodnutím je, jestli zvolíte úložiště úrovně Standard nebo Premium.