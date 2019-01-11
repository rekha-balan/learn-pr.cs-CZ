Vybavte si náš původní scénář – vytvoření virtuálních počítačů k otestování našeho softwaru CRM. Když je k dispozici nové sestavení, chceme aktivovat nový virtuální počítač, abychom mohli otestovat kompletní postup instalace z čisté image. Po dokončení pak chceme virtuální počítač odstranit.

Pojďme si vyzkoušet příkazy, které byste použili k vytvoření virtuálního počítače.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm-with-azure-powershell"></a>Vytvoření virtuálního počítače s Linuxem pomocí Azure PoweShellu

Protože používáme sandbox Azure, nemusíte vytvářet skupinu prostředků. Místo toho použijte skupinu prostředků **<rgn>[název skupiny prostředků sandboxu]</rgn>**. Kromě toho pamatujte na omezení umístění.

Pojďme vytvořit nový virtuální počítač Azure pomocí PowerShellu.

1. Pomocí rutiny `New-AzVm` vytvořte virtuální počítač.
    - Použijte skupinu prostředků **<rgn>[název skupiny prostředků sandboxu]</rgn>**.
    - Pojmenujte virtuální počítač – obvykle je vhodné použít výstižný název, který identifikuje účely virtuálního počítače, umístění a číslo instance (pokud jich existuje víc). Použijeme „testvm-eus-01“ označující „testovací virtuální počítač v oblasti východní USA, instance 1“. Navrhněte vlastní název podle toho, kde virtuální počítač umístíte.
    - Vyberte umístění blízko vás z následujícího seznamu dostupného v sandboxu Azure. Pokud používáte kopírování a vkládání, nezapomeňte v následujícím ukázkovém příkazu změnit hodnotu.

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - Použijte „UbuntuLTS“ pro image – je to Ubuntu Linux.
    - Použijte rutinu `Get-Credential` a zaveďte výsledky do parametru `Credential`.
      > [!IMPORTANT]
      > Podívejte se prosím v [nejčastějších dotazech k virtuálním počítačům s Linuxem](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm?azure-portal=true) na omezení pro uživatelská jména a hesla. Hesla musí mít délku 12 až 123 znaků a musí splňovat 3 ze 4 následujících požadavků na složitost:
      > - Musí obsahovat malá písmena.
      > - Musí obsahovat velká písmena.
      > - Musí obsahovat číslici.
      > - Musí obsahovat speciální znak (odpovídající regulárnímu výrazu [\W_]).
    - Přidejte parametr `-OpenPorts` a předejte „22“ jako port – to nám do počítače přidá SSH.
 
    ```powershell
    New-AzVm -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -Name "testvm-eus-01" -Credential (Get-Credential) -Location "East US" -Image UbuntuLTS -OpenPorts 22
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
    
1. Dokončení může několik minut trvat. Po dokončení se můžete dotázat a přiřadit objekt virtuálního počítače proměnné (`$vm`).

    ```powershell
    $vm = Get-AzVM -Name "testvm-eus-01" -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```
    
1. Pak se dotažte na hodnotu pro výpis informací o virtuálním počítači:

    ```powershell
    $vm
    ```

    Mělo by se vám zobrazit přibližně toto:

    ```powershell
    ResourceGroupName : <rgn>[sandbox resource group name]</rgn>
    Id                : /subscriptions/xxxxxxxx-xxxx-aaaa-bbbb-cccccccccccc/resourceGroups/<rgn>[sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/testvm-eus-01
    VmId              : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    Name              : testvm-eus-01
    Type              : Microsoft.Compute/virtualMachines
    Location          : eastus
    Tags              : {}
    HardwareProfile   : {VmSize}
    NetworkProfile    : {NetworkInterfaces}
    OSProfile         : {ComputerName, AdminUsername, LinuxConfiguration, Secrets}
    ProvisioningState : Succeeded
    StorageProfile    : {ImageReference, OsDisk, DataDisks}
    ```
    
1. Ke komplexním objektům se můžete dostat prostřednictvím syntaxe tečky („.“). Pokud třeba chcete zobrazit vlastnosti v objektu `VMSize` přidruženém k části HardwareProfile, můžete zadat:

    ```powershell
    $vm.HardwareProfile
    ```

1. Nebo pokud chcete získat informace o některém z disků:

    ```powershell
    $vm.StorageProfile.OsDisk
    ```

1. Objekt virtuálního počítače můžete dokonce předat do jiných rutin. Tímto se třeba načte veřejná IP adresa vašeho virtuálního počítače:

    ```powershell
    $vm | Get-AzPublicIpAddress
    ```

1. S IP adresou se můžete připojit k virtuálnímu počítači pomocí protokolu SSH. Pokud jste použili třeba uživatelské jméno „bob“ a IP adresa je „205.22.16.5“, pak by tento příkaz provedl připojení k počítači s Linuxem:

    ```powershell
    ssh bob@205.22.16.5
    ```

    Pokračujte odhlášením, a to zadáním příkazu `exit`.


## <a name="delete-a-vm"></a>Odstranění virtuálního počítače

Abychom vyzkoušeli pár dalších příkazů, pojďme virtuální počítač odstranit. Nejdříve ho vypneme.

```powershell
Stop-AzVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

Teď můžeme virtuální počítač odstranit pomocí rutiny `Remove-AzVM`:

```powershell
Remove-AzVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

Zkuste tento příkaz k vypsání všech prostředků ve vaší skupině prostředků:

```powershell
Get-AzResource -ResourceGroupName $vm.ResourceGroupName | ft
```

Měli byste vidět řadu prostředků (disky, virtuální sítě apod.), které stále existují. 

```output
Microsoft.Compute/disks
Microsoft.Network/networkInterfaces
Microsoft.Network/networkSecurityGroups
Microsoft.Network/publicIPAddresses
Microsoft.Network/virtualNetworks
```

To proto, že příkaz `Remove-AzVM` _jenom odstraní virtuální počítač_. Nevyčistí žádné jiné prostředky. V tomto okamžiku bychom pravděpodobně chtěli jenom odstranit skupinu prostředků a tím skončit. Projděme si ale cvičením, kterým ji vyčistíme ručně. V příkazech byste měli vidět jisté schéma.

1. Odstraňte síťové rozhraní.

    ```powershell
    $vm | Remove-AzNetworkInterface –Force
    ```
    
1. Odstraňte spravované disky operačního systému a účet úložiště.

    ```powershell
    Get-AzDisk -ResourceGroupName $vm.ResourceGroupName -DiskName $vm.StorageProfile.OSDisk.Name | Remove-AzDisk -Force
    ```

1. Pak odstraňte virtuální síť.

    ```powershell
    Get-AzVirtualNetwork -ResourceGroup $vm.ResourceGroupName | Remove-AzVirtualNetwork -Force
    ```

1. Odstraňte skupinu zabezpečení sítě.

    ```powershell
    Get-AzNetworkSecurityGroup -ResourceGroup $vm.ResourceGroupName | Remove-AzNetworkSecurityGroup -Force
    ```

1. A nakonec veřejnou IP adresu.

    ```powershell
    Get-AzPublicIpAddress -ResourceGroup $vm.ResourceGroupName | Remove-AzPublicIpAddress -Force
    ```

To by měly být všechny vytvořené prostředky. Pro jistotu skupinu prostředků zkontrolujte. Použili jsme tady hodně ručních příkazů, ale lepším řešením by bylo napsat _skript_, abychom tuto logiku mohli později k vytvoření nebo odstranění virtuálního počítače znovu použít. Pojďme se podívat na skriptování pomocí PowerShellu.