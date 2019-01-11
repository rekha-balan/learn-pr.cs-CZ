Předpokládejme, že vyvíjíte aplikace správy financí pro nové začínající obchodní firmy. Chcete zajistit, aby data všech vašich zákazníků byla zabezpečená, proto jste se rozhodli na serverech, které budou hostovat vaši aplikaci, implementovat ADE (Azure Disk Encryption) u všech disků operačního systému i datových disků. V rámci požadavků na dodržování předpisů musíte také sami zodpovídat za správu vašich šifrovacích klíčů.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

V této jednotce zašifrujete disky stávajících virtuálních počítačů a postaráte se o šifrovací klíče pomocí vlastní služby Azure Key Vault.

## <a name="prepare-the-environment"></a>Příprava prostředí

Začneme nasazením nového virtuálního počítače s Windows v prostředím Azure Virtual Machine.

### <a name="deploy-windows-vm"></a>Nasazení virtuálního počítače s Windows

Pomocí Azure Powershell vytvořte a nasaďte nový virtuální počítač s Windows.

1. Začněte tím, že se rozhodnete, kam umístit nové prostředky. Vyberte umístění blízko vás z následujícího seznamu.

    <!-- Resource selection -->  
    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]
    

1. Definujte proměnnou PowerShellu tak, aby obsahovala vybrané umístění. Je zde definována jako „USA – východ“. Změňte ji na upřednostňované umístění.

    ```powershell
    $location = "eastus"
    ```
    
    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Dále definujte pár další praktické proměnné pro zachycení _názvu_ virtuálního počítače a _skupiny prostředků_. Všimněte si, že zde používáme předem vytvořenou skupinu prostředků. Normálně byste vytvořili _novou_ skupinu prostředků v předplatném pomocí `New-AzResourceGroup`.

    ```powershell
    $vmName = "fmdata-vm01"
    $rgName = "<rgn>[sandbox Resource Group]</rgn>"
    ```
    
1. Vytvořte nový virtuální počítač pomocí `New-AzVm`.
    
    ```powershell
    New-AzVm `
        -ResourceGroupName $rgName `
        -Name $vmName `
        -Location $location `
        -OpenPorts 3389
    ```
    
    Zadejte uživatelské jméno a heslo pro virtuální počítač, až k tomu budete vyzváni službou Cloud Shell. Použijí se jako počáteční účet vytvořený pro virtuální počítač.
    
    > [!NOTE]
    > Tento příkaz použije některé výchozí hodnoty, protože jsme nezadali několik možností. Konkrétně se tím vytvoří image _Windows 2016 Server_ s velikostí pro _Standard_DS1_v2_. Mějte na paměti, že virtuální počítače na úrovni Basic nepodporují ADE, pokud se rozhodnete zadat velikost virtuálního počítače.

1. Po dokončení nasazení virtuálního počítače uveďte podrobnosti o virtuálním počítači do proměnné. Tuto proměnnou můžete použít k prozkoumání toho, co bylo vytvořeno.

    ```powershell
    $vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName
    ```
    
1. Vidíte disk s operačním systémem připojený k virtuálnímu počítači:

    ```powershell
    $vm.StorageProfile.OSDisk
    ```

    ```output
    OsType                  : Windows
    EncryptionSettings      :
    Name                    : fmdata-vm01_OsDisk_1_6bcf8dcd49794aa785bad45221ec4433
    Vhd                     :
    Image                   :
    Caching                 : ReadWrite
    WriteAcceleratorEnabled :
    CreateOption            : FromImage
    DiskSizeGB              : 127
    ManagedDisk             : Microsoft.Azure.Management.Compute.Models.ManagedDiskP
                              arameters
    ```
        
1. Zkontrolujte aktuální stav šifrování disku s operačním systémem (a všechny datové disky).

    ```powershell
    Get-AzVmDiskEncryptionStatus  `
        -ResourceGroupName $rgName `
        -VMName $vmName
    ```

    Jak můžete vidět, disky jsou momentálně _nešifrované_. 

    ```output
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : NotEncrypted
    OsVolumeEncryptionSettings :
    ProgressMessage            : No Encryption extension or metadata found on the VM
    ```
    
Pojďme to změnit.
    
## <a name="encrypt-the-vm-disks-with-azure-disk-encryption"></a>Šifrování disků virtuálních počítačů pomocí Azure Disk Encryption

Je potřeba chránit tato data, takže disky zašifrujeme. Pamatujte, že existuje několik kroků, které je třeba provést:

1. Vytvořte trezor klíčů.
1. Nastavte trezor klíčů pro podporu šifrování disků.
1. Sdělte prostředí Azure, aby zašifrovalo disky virtuálních počítačů pomocí klíče uloženého ve službě Key Vault.

> [!TIP]
> Provede vás jednotlivými kroky, ale když budete provádět tento úkol ve svém vlastním předplatném, můžete použít praktický skript PowerShellu, který je připojen v souhrnu tohoto modulu.

### <a name="create-a-key-vault"></a>Vytvořte trezor klíčů.

Abychom mohli vytvořit Azure Key Vault, musíme povolit službu v našem předplatném. Jedná se o jednorázový úkon.

> [!TIP]
> V závislosti na vašem předplatném může být potřeba povolit poskytovatele **Microsoft.KeyVault** pomocí rutiny `Register-AzResourceProvider`. To není nutné v předplatném sandboxu Azure.

1. Určete název nového trezoru klíčů. Musí být jedinečný a obsahovat 3 až 24 znaků – číslice, písmena a pomlčky. Zkuste na konec přidat několik náhodných čísel za účelem nahrazení řetězce „1234“ níže.

    ```powershell
    $keyVaultName = "mvmdsk-kv-1234"
    ```
        
1. Vytvořte Azure Key Vault pomocí `New-AzKeyVault`.
    - Ujistěte se, že se nachází ve stejné _skupině prostředků_ a umístění jako virtuální počítač.
    - Povolte službu Key Vault pro použití se šifrováním disku. 
    - Zadejte jedinečný název služby Key Vault.

    ```powershell
    New-AzKeyVault -VaultName $keyVaultName `
        -Location $location `
        -ResourceGroupName $rgName `
        -EnabledForDiskEncryption
    ```

    Zobrazí se upozornění z tohoto příkazu o tom, že žádní uživatelé nemají přístup.

    ```output
    WARNING: Access policy is not set. No user or application have access permission to use this vault. This can happen if the vault was created by a service principal. Please use Set-AzKeyVaultAccessPolicy to set access policies.
    ```

    To je v pořádku, protože právě používáme trezor k ukládání šifrovacích klíčů pro virtuální počítač a uživatelé nebudou potřebovat přístup k těmto datům.

### <a name="encrypt-the-disk"></a>Šifrování disku

Jsme téměř připraveni k šifrování disků. Než se do toho pustíme, je tu varování týkající se vytváření záloh.

> [!IMPORTANT]
> Pokud by se jednalo o výrobní systém, museli bychom provést zálohování spravovaných disků – buď pomocí Azure Backup nebo vytvořením snímku. Snímky můžete vytvořit na webu Azure Portal nebo prostřednictvím příkazového řádku. V prostředí PowerShell použijte rutinu `New-AzSnapshot`. Protože toto je jednoduché cvičení a po dokončení tato data zahodíte, tento krok přeskočíme. 

1. Začněte tak, že definujete proměnnou pro uchování informace Key Vaultu.

    ```powershell
    $keyVault = Get-AzKeyVault `
        -VaultName $keyVaultName `
        -ResourceGroupName $rgName
    ```

1. Potom použijte rutinu `Set-AzVmDiskEncryptionExtension` k šifrování disků virtuálních počítačů.
    - Parametr `VolumeType` umožňuje určit, které disky chcete šifrovat: [_Vše_ | _OS_ | _Data_]. Výchozí hodnota je _Vše_. Pro některé distribuce Linuxu lze zašifrovat pouze datové disky.
    - Je nutné zadat příznak `SkipVmBackup` pro spravované disky, nebo příkaz selže, protože neexistuje žádný snímek.

    ```powershell
    Set-AzVmDiskEncryptionExtension `
        -ResourceGroupName $rgName `
        -VMName $vmName `
        -VolumeType All `
        -DiskEncryptionKeyVaultId $keyVault.ResourceId `
        -DiskEncryptionKeyVaultUrl $keyVault.VaultUri `
        -SkipVmBackup
    ```

1. Rutina vás upozorní, že virtuální počítač musí být převeden do režimu offline a že úloha může trvat několik minut. Nechte ji běžet.

    ```output
    Enable AzureDiskEncryption on the VM
    This cmdlet prepares the VM and enables encryption which may reboot the machine and takes 10-15 minutes to
    finish. Please save your work on the VM before confirming. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    ```
    
1. Po dokončení znovu zkontrolujte stav šifrování.

    ```powershell
    Get-AzVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
    ```

    Teď by měl být disk s operačním systémem zašifrován. Všechny připojené datové disky, které jsou viditelné pro systém Windows, budou také zašifrovány.

    ```output
    OsVolumeEncrypted          : Encrypted
    DataVolumesEncrypted       : NoDiskFound
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : Provisioning succeeded
    ```

> [!NOTE]        
> Nové disky, které jsou přidány po šifrování, _nebudou_ automaticky šifrovány. Můžete znovu spustit rutinu `Set-AzVMDiskEncryptionExtension` a zašifrovat nové disky. Nezapomeňte zadat nové pořadové číslo, pokud přidáváte disky do virtuálního počítače, který již má disky šifrované. Kromě toho disky, které nejsou viditelné v operačním systému, nebudou zašifrovány – disk musí být správně rozdělen na oddíly, naformátován a připojen, aby jej rozšíření nástroje Bitlocker vidělo.
