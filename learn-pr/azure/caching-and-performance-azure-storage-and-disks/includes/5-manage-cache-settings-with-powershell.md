Vytváření skriptů pro správu je výkonný způsob, jak optimalizovat pracovní postup. Běžné opakované úkoly můžete automatizovat. Jakmile se skript ověří, bude běžet konzistentně, čímž pravděpodobně dojde ke snížení počtu chyb. V předchozím cvičení jsme vytvořili virtuální počítač, přidali datový disk a změnili nastavení mezipaměti. To vše pomocí webu Azure Portal. Co když tyto úkoly potřebujeme opakovat na mnoha virtuálních počítačích nebo v mnoha oblastech? To je možné provádět pomocí Azure PowerShell.

> [!TIP]
> Prostředí Azure PowerShell podrobně rozebíráme v modulu **Automatizace úloh Azure pomocí rozhraní Powershell**. Tento modul si nezapomeňte projít, naleznete v něm další podrobnosti o instalaci, konfiguraci a používání prostředí PowerShell.

## <a name="what-is-azure-powershell"></a>Co je Azure PowerShell?

Prostředí Azure PowerShell je meziplatformní nástroj příkazového řádku určený k připojení předplatného Azure a správě prostředků. Představuje kombinaci dvou věcí: **PowerShellu**, který poskytuje podporu nástroje příkazového řádku, a modulu **Az** PowerShellu, který poskytuje příkazy (označované jako „rutiny“) pro práci s Azure. 

Prostředí Azure PowerShell obsahuje rutiny pro manipulaci s většinou aspektů prostředků Azure. Můžete pracovat se skupinami prostředků, úložištěm, virtuálními počítači, Azure Active Directory, kontejnery, strojovým učením a podobně. Všechny tyto podrobnosti probíráme v ostatních modulech školení.

### <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>Rutiny PowerShellu pro správu ukládání do mezipaměti disku Azure

Azure PowerShell má konkrétní rutiny, které pomáhají spravovat virtuální počítače a disky.

|Příkaz  | Popis |
|---------|-------------|
| `Get-AzVM`         | Získá vlastnosti virtuálního počítače.       |
| `Update-AzVM`      | Aktualizuje stav virtuálního počítače Azure.  |
| `New-AzDiskConfig` | Vytvoří konfigurovatelný objekt disku.             |
| `Add-AzVMDataDisk` | Přidá datový disk k virtuálnímu počítači.          |

Pomocí těchto možností můžeme provádět všechny úlohy, které jsme prováděli na webu Azure. Pojďme si to vyzkoušet na našich virtuálních počítačích.
