Dál vytvoříme pomocí Azure CLI skupinu prostředků, do které pak nasadíme webovou aplikaci.

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="using-a-resource-group"></a>Použití skupiny prostředků

Pokud pracujete s vlastním počítačem a předplatné Azure budete potřebovat k prvnímu přihlášení do Azure pomocí příkazu `az login`. To není nutné s prostředím Cloud Shell.

Za normálních okolností byste dále vytvořili skupinu prostředků pro všechny související prostředky Azure pomocí příkazu `az group create`, ale pro tato cvičení jsme už skupinu prostředků za vás vytvořili. Jako skupinu prostředků použijte **<rgn>[název skupiny prostředků sandboxu]</rgn>**.

1. Můžete požádat o výpis všech vašich skupin prostředků do tabulky v Azure CLI. Pokud jste v bezplatném Sandboxu Azure, měla by existovat jen jedna.

    ```azurecli
    az group list --output table
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Během vašeho dalšího vývoje pro Azure se může vytvořit několik skupin prostředků. Pokud máte v seznamu skupin několik položek, můžete vrácené hodnoty filtrovat pomocí parametru `--query`. Vyzkoušejte tento příkaz:

    ```azurecli
    az group list --query "[?name == '<rgn>[sandbox resource group name]</rgn>']"
    ```

    Dotaz je naformátovaný pomocí **JMESPath**, což je standardní dotazovací jazyk žádostí JSON. Další informace o tomto výkonném filtrovacím jazyku najdete na adrese <http://jmespath.org/>. Dotazy jsou podrobněji rozebrány také v modulu **Správa virtuálních počítačů pomocí Azure CLI**.

### <a name="steps-to-create-a-service-plan"></a>Postup vytvoření plánu služby

Pokud používáte Web Apps ve službě Azure App Service, platíte za výpočetní prostředky Azure, které aplikace využívá. Výše těchto poplatků závisí na plánu služby App Service přidruženém k vaší funkci Web Apps. Plány služby určují oblast datacentra aplikace, počet použitých virtuálních počítačů a cenovou úroveň.

1. Vytvořte plán služby App Service pro vaši aplikaci. Následující příkaz neurčuje cenovou úroveň ani podrobnosti o instanci virtuálního počítače, takže ve výchozím nastavení získáte plán **Basic** s 1 **malou** instancí virtuálního počítače.

    > [!WARNING]
    > Název aplikace a plánu musí být _jedinečný_, proto v níže uvedeném příkazu přidejte k názvu příponu a text `<unique>` nahraďte sadou číslic, vašimi iniciálami nebo jiným textem, aby byl v rámci Azure jedinečný.

    Pro parametr `--location` použijte jednu z uvedených hodnot umístění.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --location <location>
    ```

    Provedení tohoto příkazu může trvat i několik minut.

1. Ověřte úspěšné vytvoření plánu služby tím, že do tabulky vypíšete všechny vaše plány.

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Postup vytvoření webové aplikace

Dále ve svém plánu služby vytvoříte webovou aplikaci. Současně můžete i nasadit kód, ale v našem příkladu to provedeme jako samostatné kroky.

1. Vytvořte webovou aplikaci a zadejte název dříve vytvořeného plánu. **Název aplikace musí být stejně jako název plánu jedinečný.** Nahraďte značku `<unique>` nějakým textem, aby byl název globálně jedinečný.

    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --plan popupappplan-<unique>
    ```

1. Ověřte úspěšné vytvoření aplikace tím, že do tabulky vypíšete všechny vaše aplikace.

    ```azurecli
    az webapp list --output table
    ```

1. Poznamenejte si hodnotu **DefaultHostName** uvedenou v tabulce. To je dostupná webová adresa pro nový web. Azure zpřístupní tento web přes jedinečný název aplikace v doméně `azurewebsites.net`. Pokud máte například aplikaci s názvem „popupwebapp-mslearn123“, vypadala by adresa URL webu takto: `http://popupwebapp-mslearn123.azurewebsites.net`.

1. Váš web má stránku „Rychlý start“ vytvořenou Azure, kterou můžete zobrazit v prohlížeči nebo pomocí nástroje CURL, stačí použít **DefaultHostName**:

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
### <a name="steps-to-deploy-code-from-github"></a>Postup nasazení kódu z GitHubu

1. Posledním krokem je nasazení kódu z úložiště GitHubu do webové aplikace. Použijeme jednoduchou stránku PHP dostupnou v úložišti Githubu s ukázkami pro Azure, která při spuštění zobrazí text „HelloWorld!“. Použijte název webové aplikace, kterou jste vytvořili.

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. Po nasazení přejděte na váš web znovu pomocí prohlížeče nebo nástroje CURL.

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
    Na stránce se zobrazí zpráva „Hello World!“.

    ```output
    Hello World!
    ```

V tomto cvičení jste se seznámili s typickým postupem pro interaktivní relaci Azure CLI. Nejprve jste pomocí standardního příkazu vytvořili novou skupinu prostředků. Potom jste pomocí sady příkazů do této skupiny prostředků nasadili prostředek (v tomto příkladu webovou aplikaci). Tuto sadu příkazů je možné snadno zkombinovat do skriptu prostředí a ten spouštět pokaždé, když budete potřebovat vytvořit stejný prostředek.