Teď nainstalujeme Azure CLI na místní počítač a pak spustíme příkaz k ověření instalace. Metoda použitá k instalaci Azure CLI závisí na operačním systému počítače. Zvolte postup podle svého operačního systému.

> [!NOTE]
> Toto cvičení vás provede místní instalací nástroje Azure CLI. Ve zbývající části modulu budeme používat Azure Cloud Shell, abyste mohli využít podpory bezplatného předplatného v kurzech Microsoft Learn. Toto cvičení můžete brát jako volitelnou činnost a podle potřeby si můžete jenom projít pokyny.

::: zone pivot="linux"

## <a name="linux"></a>Linux

Tento postup nainstaluje Azure CLI na **Ubuntu Linux** pomocí nástroje **apt** (Advanced Packaging Tool) a příkazového řádku Bash.

> [!TIP]
> Následující příkazy jsou pro Ubuntu verze 18.04. Pro jiné verze a distribuce Linuxu jsou jiné pokyny. Pokud používáte jinou verzi Linuxu, podívejte se do [oficiální dokumentace](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Upravte seznam zdrojů tak, aby bylo úložiště Microsoftu zaregistrované a správce balíčků mohl balíček Azure CLI najít.

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. Importujte šifrovací klíč úložiště Microsoftu Ubuntu. To umožní správci balíčků ověřit, že instalovaný balíček Azure CLI pochází od Microsoftu.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. Nainstalujte Azure CLI.

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

::: zone-end

::: zone pivot="macos"

## <a name="macos"></a>macOS

Tento postup nainstaluje Azure CLI na macOS pomocí správce balíčků Homebrew.

> [!IMPORTANT]
> Pokud příkaz **brew** není k dispozici, budete asi muset správce balíčků Homebrew nainstalovat. Podrobnosti najdete na [webu Homebrew](https://brew.sh/).

1. Aktualizujte úložiště brew, abyste měli jistotu, že získáte nejnovější balíček Azure CLI.

    ```bash
    brew update
    ```

1. Nainstalujte Azure CLI.

    ```bash
    brew install azure-cli
    ```

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows

Tento postup nainstaluje Azure CLI do Windows pomocí instalačního programu MSI.

1. Přejděte na [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) a v dialogovém okně zabezpečení prohlížeče klikněte na **Spustit**.
1. V instalačním programu přijměte licenční podmínky a pak klikněte na **Instalovat**.
1. V dialogovém okně **Řízení uživatelských účtů** vyberte **Ano**.

::: zone-end

## <a name="running-the-azure-cli"></a>Spuštění Azure CLI

Azure CLI spustíte otevřením prostředí bash (Linux a macOS) nebo z příkazového řádku PowerShellu (Windows).

1. Spusťte Azure CLI a ověřte instalaci spuštěním kontroly verze.

    ```azurecli
    az --version
    ```

::: zone pivot="windows"

> [!TIP]
> Spuštění Azure CLI z PowerShellu má oproti spuštění z příkazového řádku Windows několik výhod. V porovnání s funkcemi dostupnými na příkazovém řádku nabízí PowerShell navíc další funkce automatického doplňování.

::: zone-end

Místní počítače jste nastavili pro správu prostředků Azure pomocí Azure CLI. Teď můžete Azure CLI používat místně k zadávání příkazů nebo spouštění skriptů. Azure CLI předává příkazy datovým centrům Azure, kde se v rámci vašeho předplatného Azure provedou.