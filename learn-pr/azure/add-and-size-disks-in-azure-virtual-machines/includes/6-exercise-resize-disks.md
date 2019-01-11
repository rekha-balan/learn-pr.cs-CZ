Řekněme, že jste podcenili velikost některých nahrávaných souborů a na příslušném disku dochází místo. Rozhodnete se toto místo zdvojnásobit z 64 GB na 128 GB.

Tady si vyzkoušíte proces, o kterém jste se dozvěděli v předchozích lekcích.

## <a name="resize-the-data-disk"></a>Změna velikosti datového disku

Ke změně velikosti disku potřebujete jeho ID nebo název. V tomto případě už název známe, **uploadDataDisk1**. Pokud si ale název nepamatujete, nebo ho vytvořil někdo jiný, můžete spustit příkaz `az disk list` a název vyhledat.

1. Spuštěním příkazu `az disk list` zobrazte seznam spravovaných disků ve skupině prostředků. Pokud máte ve stejné skupině prostředků více virtuálních počítačů, může tento seznam obsahovat jiné disky.

    ```azurecli
    az disk list \
      --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
      --output table
    ```

    Zobrazí se disk s názvem **uploadDataDisk1**.

    ```output
    Name                                                        Gb
    ----------------------------------------------------------  ----
    support-web-vm01_OsDisk_1_141859cb21d64b85b9db3f70f0f5e851  30
    uploadDataDisk1                                             64
    ```

1. Spuštěním následujícího příkazu `az vm deallocate` zastavte virtuální počítač a zrušte jeho přidělení. Tím nedojde k odstranění virtuálního počítače, ale uvede ho to do stavu, ve kterém budete moct upravit virtuální disky.

    ```azurecli
    az vm deallocate --name support-web-vm01
    ```

1. Spuštěním příkazu `az disk update` změňte velikost disku na 128 GB.

    ```azurecli
    az disk update --name uploadDataDisk1 --size-gb 128
    ```

1. Spuštěním příkazu `az vm start` virtuální počítač restartujte.

    ```azurecli
    az vm start --name support-web-vm01
    ```

    Tím to ale ještě nekončí. Operační systém na virtuálním počítači zatím místo navíc nemůže využít. To provedeme v další části.

## <a name="expand-the-disk-partition"></a>Rozšíření oddílu disku

Posledním krokem je informovat operační systém o dostupném místě. Stejně jako u postupu pro dělení a formátování, který jsme provedli dříve, je tento proces identický jako ten pro rozšíření fyzického místního disku.

1. I když pro virtuální počítač můžete vyhradit pevnou veřejnou IP adresu, virtuální počítač ve výchozím nastavení obdrží novou veřejnou IP adresu, když zrušíte jeho přidělení a restartujete ho. Spuštěním následujícího příkazu `az vm show` aktualizujte proměnnou Bash novou veřejnou IP adresu vašeho virtuálního počítače.

    ```azurecli
    ipaddress=$(az vm show --name support-web-vm01 -d --query [publicIps] --o tsv)
    ```

1. Stejně jako jste to udělali předtím, spusťte `lsblk` na virtuálním počítači přes SSH, abyste zjistili jeho aktuální stav.

    ```bash
    ssh azureuser@$ipaddress lsblk
    ```

    Uvidíte, že disk `sdc/sdc1` má stále velikost 64 GB.

    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk 
    └─sdb1   8:17   0   14G  0 part /mnt
    sdc      8:32   0  128G  0 disk 
    └─sdc1   8:33   0   64G  0 part /uploads
    sda      8:0    0   30G  0 disk 
    └─sda1   8:1    0   30G  0 part /
    ```

1. Postupujte podobně jako dříve při inicializaci disku a spusťte tento příkaz `az vm extension set`, který operační systém na virtuálním počítači informuje o nově dostupném místu díky spuštění předem připraveného skriptu Bash, který vytvoříme, abyste to měli snadnější.

    ```azurecli
    az vm extension set \
      --vm-name support-web-vm01 \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-add-and-size-disks-in-azure-virtual-machines/master/resize-data-disk.sh"]}' \
      --protected-settings '{"commandToExecute": "./resize-data-disk.sh"}'
    ```

    Pokud budete chtít, můžete si v době spuštění příkazu [prostudovat příslušný skript Bash](https://raw.githubusercontent.com/MicrosoftDocs/mslearn-add-and-size-disks-in-azure-virtual-machines/master/resize-data-disk.sh?azure-portal=true) na samostatné kartě prohlížeče.

    Abychom to shrnuli, skript:

    * Odpojí disk `/dev/sdc1`.
    * Změní velikost oddílu 1 na 128 GB.
    * Ověří konzistenci oddílu.
    * Změní velikost systému souborů.
    * Znovu připojí jednotku `/dev/sdc1` zpět k přípojnému bodu `/uploads`.

1. Pokud chcete ověřit konfiguraci, spusťte `lsblk` na svém virtuálním počítači přes SSH ještě jednou.

    ```bash
    ssh azureuser@$ipaddress lsblk
    ```

    Tentokrát uvidíte, že disk `sdc/sdc1` je rozšířený tak, aby se přizpůsobil zvětšené velikosti vašeho disku.

    ```output
    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0    14G  0 disk 
    └─sdb1   8:17   0    14G  0 part /mnt
    sdc      8:32   0   128G  0 disk 
    └─sdc1   8:33   0 119.2G  0 part /uploads
    sda      8:0    0    30G  0 disk 
    └─sda1   8:1    0    30G  0 part /
    ```

1. Jako poslední ověřovací krok spusťte nástroj `df` operačního systému na virtuálním počítači přes SSH, aby bylo zřejmě, že ho operační systém vidí správně.

    ```bash
    ssh azureuser@$ipaddress df -h
    ```

    Uvidíte, že velikost jednotky je 128 GB.

    ```output
    Filesystem      Size  Used Avail Use% Mounted on
    udev            3.4G     0  3.4G   0% /dev
    tmpfs           697M  8.6M  689M   2% /run
    /dev/sda1        30G  1.4G   28G   5% /
    tmpfs           3.5G     0  3.5G   0% /dev/shm
    tmpfs           5.0M     0  5.0M   0% /run/lock
    tmpfs           3.5G     0  3.5G   0% /sys/fs/cgroup
    /dev/sdb1        14G   35M   13G   1% /mnt
    /dev/sdc1        63G   52M   60G   1% /uploads
    tmpfs           697M     0  697M   0% /run/user/1000
    ```