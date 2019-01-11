V této lekci budete pokračovat s příkladem společnosti, která vytváří linuxové nástroje pro správu. Připomínáme, že plánujete využití linuxových virtuálních počítačů k tomu, aby si potenciální zákazníci mohli vyzkoušet váš software. Máte nachystanou skupinu prostředků a teď je čas vytvořit virtuální počítače.

Vaše společnost si zaplatila stánek na velkém linuxovém veletrhu. Plánujete ukázkovou zónu se třemi terminály, z nichž každý je připojený k samostatnému linuxovému virtuálnímu počítači. Na konci každého dne chcete tyto virtuální počítače odstranit a znovu je vytvořit, abyste každé ráno začínali s čistým prostředím. Kdybyste po práci vytvářeli virtuální počítače ručně, mohli byste udělat chyby, protože už jste unavení. Chcete napsat powershellový skript a proces vytváření virtuálních počítačů tak automatizovat.

## <a name="write-a-script-that-creates-virtual-machines"></a>Napsání skriptu, který vytvoří virtuální počítače

Napište skript podle těchto kroků v Cloud Shellu vpravo:

1. Přejděte do vaší domovské složky v Cloud Shellu.

    ```powershell
    cd $HOME\clouddrive
    ```

1. Vytvořte nový textový soubor s názvem **ConferenceDailyReset.ps1**.

    ```powershell
    touch "./ConferenceDailyReset.ps1"
    ```

1. Otevřete integrovaný editor a vyberte soubor **ConferenceDailyReset.ps1**.

    ```powershell
    code "./ConferenceDailyReset.ps1"
    ```
    > [!TIP]
    > Integrovaný Cloud Shell podporuje také editory vim, nano a emacs, pokud byste raději používali některý z nich.

1. Začněte tím, že zachytíte vstupní parametr v proměnné. Do skriptu přidejte tento řádek:

    ```powershell
    param([string]$resourceGroup)
    ```

    > [!NOTE]
    > Za normálních okolností byste museli provést ověření v Azure přihlašovacími údaji pomocí `Connect-AzAccount` a to by se dalo udělat ve skriptu. Ale v prostředí Cloud Shellu už ověření budete, takže to není potřeba.

1. Vyzvěte k zadání uživatelského jména a hesla pro účet správce virtuálního počítače a výsledek zachyťte v proměnné:

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. Vytvořte smyčku, která se provede třikrát:

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. V těle smyčky vytvořte název pro každý virtuální počítač a uložte ho do proměnné a proveďte jeho výstup do konzoly:

    ```powershell
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    ```

1. Pak pomocí proměnné `$vmName` vytvořte virtuální počítač:

   ```powershell
   New-AzVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
   ```

1. Soubor uložte. Můžete použít nabídku „...“ v pravém horním rohu editoru. Existují také běžné klávesové zkratky pro uložení.

Hotový skript by měl vypadat takto:

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    New-AzVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a>Spuštění skriptu

Zavřete editor pomocí místní nabídky „...“.

1. Spusťte skript.

    ```powershell
    .\ConferenceDailyReset.ps1 <rgn>[sandbox resource group name]</rgn>
    ```
    
Dokončení skriptu trvá několik minut. Po dokončení ověřte, že se úspěšně provedl, zkontrolováním prostředků, které teď máte ve skupině prostředků:

```powershell
Get-AzResource -ResourceType Microsoft.Compute/virtualMachines
```

Měli byste vidět tři virtuální počítače, každý s jedinečným názvem.

Napsali jste skript, který automatizuje vytvoření tří virtuálních počítačů ve skupině prostředků určené parametrem skriptu. Skript je krátký a jednoduchý, ale i tak automatizuje proces, který byste na portálu dělali ručně dlouhou dobu.