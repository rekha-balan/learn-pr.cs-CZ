Zde nasadíte šablonu pro rychlý start, kterou jste prozkoumali v předchozí části. Tato šablona pro rychlý start zřídí základní konfiguraci virtuálního počítače.

Než se do toho pustíte, krátce si zopakujeme proces nasazení.

[!include[](../../../../includes/azure-sandbox-activate.md)]

## <a name="how-do-i-deploy-a-resource-manager-template"></a>Jak nasadím šablonu Resource Manageru?

K nasazení prostředků ze šablon můžete použít automatizační skriptovací nástroje, jako je Azure CLI, Azure PowerShell nebo dokonce rozhraní Azure REST API s oblíbeným programovacím jazykem. Šablony můžete nasadit také přes Visual Studio, Visual Studio Code nebo Azure Portal. Brzy nasadíte šablonu Resource Manageru pomocí Azure CLI z prostředí Cloud Shell.

### <a name="verifying-a-template"></a>Ověření šablony

Před spuštěním šablony ji možná budete chtít napřed ověřit.

Jako první krok je vhodné šablonu zpracovat přes _linter_, což je nástroj, který u šablony ověřuje správnost syntaxe JSON. K tomuto účelu můžete použít nástroje, které běží na příkazovém řádku, v prohlížeči nebo v oblíbeném editoru kódu.

> [!TIP]
> Visual Studio Code obsahuje spoustu [integrovaných funkcí](https://code.visualstudio.com/docs/languages/json?azure-portal=true), které usnadňují práci se soubory JSON. Podobné integrované funkce nebo moduly plug-in může nabízet i váš oblíbený editor.

Dalším krokem může být vizualizace šablony. [Vizualizér Azure Resource Manageru](http://armviz.io?azure-portal=true) umožňuje nahrát šablonu Resource Manageru a graficky znázornit, jak spolu prostředky vzájemně souvisejí. Kontrolou této vizualizace ověříte, že nasazení je správně nastavené a splňuje vaše požadavky.

Nakonec můžete provést testovací nasazení z Azure CLI nebo Azure PowerShellu. Testovací nasazení nevytváří žádné prostředky, ale poskytuje vám zpětnou vazbu o tom, co by se stalo při jeho spuštění. Brzy provedete testovací nasazení, abyste viděli, jak tento proces funguje.

## <a name="creating-resources-in-azure"></a>Vytvoření prostředků v Azure

První věcí, kterou bychom za normálních okolností udělali, je vytvoření _skupiny prostředků_ pro uchování všeho, co budeme potřebovat vytvořit. To nám umožní spravovat všechny virtuální počítače, disky, síťová rozhraní a další prvky, ze kterých se naše řešení skládá. Můžeme přes Azure CLI vytvořit skupinu prostředků pomocí příkazu `az group create`. Ten má dva parametry: `--name` pro zadání jedinečného názvu v rámci našeho předplatného a `--location`, jehož hodnota Azure říká, v jaké části světa mají být prostředky ve výchozím nastavení umístěné.

Vzhledem k tomu, že jste v bezplatném sandboxovém prostředí Azure, nemusíte tento krok provádět. Místo toho použijete předem vytvořenou skupinu prostředků **<rgn>[název skupiny prostředků]</rgn>**.

## <a name="create-template-parameters"></a>Vytvoření parametrů šablony

Vzpomeňte si, že šablona Resource Manageru je rozdělená na oddíly, z nichž jeden je určený pro **parametry**.

V blízkosti začátku šablony uvidíte oddíl s názvem `parameters`. Tento oddíl definuje těchto pět parametrů:

* `adminUsername`
* `adminPassword`
* `dnsLabelPrefix`
* `windowsOSVersion`
* `location`

![Zdrojový kód pro oddíl parametrů šablony se zvýrazněnými názvy jednotlivých parametrů](../../media/4-armviz-params-windows.png)

Dva z těchto parametrů &ndash; `windowsOSVersion` a `location` &ndash; mají výchozí hodnoty. Výchozí hodnota pro `windowsOSVersion` je 2016-Datacenter a výchozí hodnota pro `location` je umístění nadřazené skupiny prostředků.

Nechejme u těchto parametrů výchozí hodnoty. U zbývajících tří parametrů máte dvě možnosti:

1. Zadejte hodnoty do souboru JSON.
1. Zadejte hodnoty jako argumenty příkazového řádku.

Pro účely výuky tyto hodnoty zadáte jako argumenty příkazového řádku. Aby se tato šablona snadněji nasazovala, začnete uložením těchto hodnot jako proměnných prostředí Bash.

1. Z prostředí Cloud Shell vytvořte uživatelské jméno. V tomto příkladu použijeme **azureuser**.

    ```bash
    USERNAME=azureuser
    ```

1. Spuštěním nástroje **openssl** vygenerujte náhodné heslo.

    ```bash
    PASSWORD=$(openssl rand -base64 32)
    ```

    Náhodná hesla můžete vygenerovat mnoha způsoby. Metoda, kterou zvolíte, závisí na vašem pracovním postupu a požadavcích. Tato metoda používá nástroj **openssl** k vygenerování 32 náhodných bajtů a zakódování výstupu pomocí base64. Kódování base64 zajišťuje, aby výsledek obsahoval pouze tisknutelné znaky.

1. Vygenerujte jedinečnou předponu názvu DNS.

    ```bash
    DNS_LABEL_PREFIX=mydeployment-$RANDOM
    ```

    Tato předpona názvu DNS musí být jedinečná. Předpona názvu DNS začíná řetězcem „mydeployment“, za kterým následuje náhodné číslo. `$RANDOM` je funkce Bash, která vygeneruje náhodné kladné celé číslo.

    V praxi byste zvolili předponu názvu DNS, která vyhovuje vašim požadavkům.

## <a name="validate-and-launch-the-template"></a>Ověření a spuštění šablony

Se zadanými parametry máte všechno, co potřebujete ke spuštění šablony.

Konečné ověření zahájíte tím, že zkontrolujete, jestli je šablona syntakticky správná.

1. Spuštěním příkazu `az group deployment validate` z prostředí Cloud Shell ověřte šablonu.

    ```azurecli
    az group deployment validate \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json" \
      --parameters adminUsername=$USERNAME \
      --parameters adminPassword=$PASSWORD \
      --parameters dnsLabelPrefix=$DNS_LABEL_PREFIX
    ```

    Argument `--template-uri` odkazuje na šablonu na GitHubu. Název souboru šablony je **azuredeploy.json**. Později zjistíte, jak šablonu ověřit a spustit v místním systému souborů.

    Vidíte velký blok JSON jako výstup, který vás informuje, že šablona byla úspěšně ověřena.

    Azure Resource Manager vyplní parametry šablony a zkontroluje, jestli by se šablona úspěšně spustila ve vašem předplatném.

    Pokud je ověření neúspěšné, zobrazí se ve výstupu podrobný popis chyby.

1. Spuštěním příkazu `az group deployment create` šablonu nasaďte.

    ```azurecli
    az group deployment create \
      --name MyDeployment \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json" \
      --parameters adminUsername=$USERNAME \
      --parameters adminPassword=$PASSWORD \
      --parameters dnsLabelPrefix=$DNS_LABEL_PREFIX
    ```

    Tento příkaz připomíná předchozí příkaz, ale obsahuje navíc argument `--name`, který vašemu nasazení přiřadí název.

    Dokončení tohoto příkazu trvá 4 až 5 minut. Během čekání je vhodná doba, abychom se [blíže podívali na zdrojový kód](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-simple-windows/azuredeploy.json?azure-portal=true) této šablony. S prohlížením existujících šablon a vytvářením vlastních vám bude obsah šablony Resource Manageru připadat stále srozumitelnější.

    Po dokončení nasazení uvidíte jako výstup další velký blok JSON, který popisuje nasazení.

## <a name="verify-the-deployment"></a>Ověření nasazení

Nasazení bylo úspěšné. Pojďme to ale ověřit spuštěním několika příkazů.

1. Spuštěním příkazu `az group deployment show` ověřte nasazení.

    ```azurecli
    az group deployment show \
      --name MyDeployment \
      --resource-group <rgn>[sandbox resource group name]</rgn>
    ```

    Zobrazí se stejný blok JSON, který jste viděli už dříve. Pokud někdy budete potřebovat podrobnosti o nasazení, můžete tento příkaz spustit později. Výstup je strukturovaný jako JSON, aby bylo snazší ho předat do jiného nástroje, který můžete používat ke sledování nasazení a využití cloudu.

1. Pomocí příkazu `az vm list` zjistíte, které virtuální počítače jsou spuštěné.

    ```azurecli
    az vm list \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --output table
    ```

    Výstup bude vypadat nějak takto. Vaše oblast se zobrazuje ve sloupci **Umístění**.

    ```bash
    Name         ResourceGroup                         Location        Zones
    -----------  ------------------------------------  --------------  -------
    SimpleWinVM  <rgn>[sandbox resource group name]</rgn>  southcentralus
    ```

    Vzpomeňte si, že šablona pojmenovala virtuální počítač jako „SimpleWinVM“. Tady vidíte, že tento virtuální počítač existuje ve vaší skupině prostředků.