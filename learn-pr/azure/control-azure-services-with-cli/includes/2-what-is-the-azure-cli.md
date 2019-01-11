Azure CLI je program v příkazovém řádku, který zajišťuje připojení k Azure a provádí příkazy správy na prostředcích Azure. Běží v systémech Linux, macOS a Windows a umožňuje správcům a vývojářům spouštět příkazy prostřednictvím terminálu nebo příkazového řádku (případně skriptu) místo webového prohlížeče. Pokud byste například chtěli restartovat virtuální počítač, použili byste podobný příkaz:

 ```azurecli
 az vm restart -g MyResourceGroup -n MyVm
 ```

Azure CLI poskytuje nástroje příkazového řádku pro správu prostředků Azure na různých platformách a dá se nainstalovat místně na počítače se systémy Linux, Mac nebo Windows. Azure CLI je možné používat také v prohlížeči prostřednictvím služby Azure Cloud Shell. V obou případech se dá použít interaktivně nebo pomocí skriptu. Při interaktivním použití nejprve spusťte prostředí, například cmd.exe ve Windows nebo Bash v Linuxu nebo macOS, a pak v příkazovém řádku daného prostředí zadejte příslušný příkaz. Pokud chcete automatizovat opakující se úlohy, sestavte příkazy CLI do skriptu prostředí pomocí syntaxe skriptu pro dané prostředí a pak spusťte tento skript.

## <a name="how-to-install-the-azure-cli"></a>Postup instalace Azure CLI

V Linuxu i macOS nainstalujete Azure CLI pomocí správce balíčků. Doporučený správce balíčků se liší podle operačního systému a distribuce:

- Linux: **apt-get** pro Ubuntu, **yum** pro Red Hat a **zypper** pro OpenSUSE
- Mac: **Homebrew**

Azure CLI je k dispozici v úložišti Microsoftu, takže nejdříve bude nutné přidat do vašeho správce balíčků toto úložiště.

Ve Windows nainstalujete Azure CLI tak, že stáhnete a spustíte soubor MSI.

## <a name="using-the-azure-cli-in-scripts"></a>Používání Azure CLI ve skriptech

Pokud chcete používat příkazy Azure CLI ve skriptech, je nutné vědět o všech záležitostech týkajících se prostředí, ve kterém se skript spouští. Například v prostředí Bash se k nastavení proměnných použije následující syntaxe:

```azurecli
variable="value"
variable=integer
```

Pokud ke spouštění skriptů Azure CLI použijete prostředí PowerShell, bude nutné pro proměnné použít tuto syntaxi:

```powershell
$variable="value"
$variable=integer
```

Azure CLI lze ke správě prostředků Azure z místního počítače používat až poté, co se tento program nainstaluje. Instalační postup pro Windows, Linux a macOS se liší, ale po dokončení instalace jsou příkazy pro všechny uvedené platformy společné.
