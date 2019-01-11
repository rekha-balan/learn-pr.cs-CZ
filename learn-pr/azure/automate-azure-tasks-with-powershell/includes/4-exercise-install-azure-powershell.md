V této lekci nainstalujete **PowerShell** na místní počítač.

> [!NOTE]
> V tomto cvičení vás provede instalací nástroje PowerShell místně. Ve zbývající části modulu budeme používat Azure Cloud Shell, abyste mohli využít podpory bezplatného předplatného v kurzech Microsoft Learn. Toto cvičení můžete brát jako volitelnou činnost a podle potřeby si můžete jenom projít pokyny.

::: zone pivot="linux"

## <a name="linux"></a>Linux

Instalace Powershellu pro Linux bude zahrnovat použití Správce balíčků. V tomto příkladu použijeme **Ubuntu 18.04**, ale [v naší dokumentaci najdete podrobné pokyny i pro ostatní verze a distribuce](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).

PowerShell Core v systému Ubuntu Linux nainstalujete pomocí nástroje **apt** (Advanced Packaging Tool) a příkazového řádku Bash. 

1. Importujte šifrovací klíč úložiště Microsoftu Ubuntu. To umožní správci balíčků ověřit, že instalovaný balíček Azure PowerShell Core pochází od Microsoftu.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. Zaregistrujte **úložiště Microsoft Ubuntu**, aby správce balíčků mohl najít balíček PowerShell Core.

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. Aktualizujte seznam balíčků.

    ```bash
    sudo apt-get update
    ```

1. Nainstalujte PowerShell Core.

    ```bash
    sudo apt-get install -y powershell
    ```

1. Spuštěním PowerShellu ověřte, že se úspěšně nainstaloval.

    ```bash
    pwsh
    ```
::: zone-end

::: zone pivot="macos"

## <a name="macos"></a>MacOS

V systému macOS je prvním krokem instalace **PowerShellu Core**. To se provádí pomocí Správce balíčků Homebrew.

> [!IMPORTANT]
> Pokud příkaz **brew** není k dispozici, budete asi muset správce balíčků Homebrew nainstalovat. Podrobnosti najdete na [webu Homebrew](https://brew.sh/).

1. Nainstalujte Homebrew-Cask, abyste získali další balíčky, včetně balíčku PowerShell Core:

    ```bash
    brew tap caskroom/cask
    ```

1. Nainstalujte PowerShell Core:

    ```bash
    brew cask install powershell
    ```

1. Spuštěním PowerShellu Core ověřte, že se úspěšně nainstaloval:

    ```bash
    pwsh
    ```

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows
PowerShell je součástí Windows, ale pro váš počítač může být k dispozici aktualizace. Podpora Azure, kterou budeme používat, vyžaduje PowerShell verze 5.0 nebo vyšší. Nainstalovanou verzi můžete zkontrolovat pomocí následujících kroků:

1. Otevřete nabídku **Start** a zadejte **Windows PowerShell**. Může vám vyskočit více zástupců pro různé verze:
    - Windows PowerShell – toto je 64bitová verze a obecně ta, kterou byste měli zvolit.
    - Windows PowerShell (x86) – toto je 32bitová verze nainstalovaná v 64bitové verzi Windows.
    - Windows PowerShell ISE – integrované skriptovací prostředí (ISE) se používá k psaní skriptů v prostředí PowerShell. 
    - Windows PowerShell ISE (x86) – 32bitová verze ISE.

1. Vyberte ikonu Windows PowerShell.

1. Zadejte následující příkaz, který určí nainstalovanou verzi prostředí PowerShell.

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
Pokud je číslo verze nižší než 5.0, postupujte podle těchto pokynů k [upgradu stávajícího Windows PowerShellu](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

::: zone-end

Nastavili jste místní počítač pro podporu prostředí PowerShell. V dalším kroku se podíváme na další příkazy, které můžete přidat, včetně modulu Azure.