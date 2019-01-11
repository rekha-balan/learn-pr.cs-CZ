Předpokládejme, že jste pro řešení automatizace zvolili prostředí Azure PowerShell. Vaši správci raději spouští skripty místně než v prostředí Azure Cloud Shell. Tým používá počítače se systémy Linux, macOS a Windows. Prostředí Azure PowerShell potřebujete zprovoznit na všech zařízeních. 

## <a name="what-must-be-installed"></a>Co se musí nainstalovat?
V další lekci si projdeme vlastní pokyny k instalaci, teď se ale podívejme na dvě složky, ze kterých se prostředí Azure PowerShell skládá.

- **Základní produkt PowerShell** Existují dvě varianty: PowerShell ve Windows a PowerShell Core v macOS a Linuxu.
- **Modul Azure PowerShell** Tento doplňkový modul se musí nainstalovat, aby prostředí PowerShell získalo specifické příkazy pro Azure.

> [!TIP]
> PowerShell je součástí systému Windows (ale může mít k dispozici aktualizaci). PowerShell Core budete asi muset na Linux a macOS nainstalovat.

Jakmile budete mít základní produkt nainstalovaný, můžete k instalaci přidat modul Azure PowerShell.

## <a name="how-to-install-powershell-core"></a>Postup instalace prostředí PowerShell Core
V Linuxu i macOS nainstalujete prostředí PowerShell Core pomocí správce balíčků. Doporučený správce balíčků se liší podle operačního systému a distribuce.

> [!NOTE]
> PowerShell Core je k dispozici v úložišti Microsoftu, takže nejdříve bude nutné toto úložiště přidat do vašeho správce balíčků.

### <a name="linux"></a>Linux
V Linuxu se bude správce balíčků lišit podle vybrané distribuce.

| Distribuce  | Správce balíčků |
|------------------|-----------------|
| Ubuntu, Debian   | `apt-get`       |
| Red Hat, CentOS  | `yum`           |
| OpenSUSE         | `zypper`        |
| Fedora           | `dnf`           |

### <a name="mac"></a>Mac
V systému macOS použijete k instalaci prostředí PowerShell Core `Homebrew`.

V další části si ukážeme podrobný postup instalace na některých běžných platformách.