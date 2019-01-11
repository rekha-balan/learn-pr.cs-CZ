## <a name="face-api-registration"></a>Registrace rozhraní API pro rozpoznávání tváře

Jakmile získáte své předplatné sandboxu Azure, můžete přidat prostředek rozhraní API pro rozpoznávání tváře a získat adresu URL a klíč tohoto rozhraní, pomocí nichž se k rozhraní připojíte.

1. Přejděte na [službu Tvář v Azure Cognitive Services](https://azure.microsoft.com/en-us/services/cognitive-services/face?azure-portal=true) a klikněte na **Vyzkoušet Tvář**. Klikněte na tlačítko **Přihlásit** v poli **Existující účet Azure**. Tím se dostanete na portál Azure Portal a otevře se dialogové okno **Vytvořit Tvář**. Zadejte následující informace:

    - název prostředku Azure pro rozpoznávání tváře
    - `Concierge Subscription` jako předplatné
    - pro umístění nechejte výchozí hodnotu
    - `F0` jako cenovou úroveň
    - **<rgn>Sandbox Resource Group</rgn>** jako skupinu prostředků

1. Až se prostředek Azure úspěšně nasadí, přejděte na něj a poznamenejte si **koncový bod** a **klíče** rozhraní API. Jeden z těchto klíčů budete potřebovat, až budete volat rozhraní API pro rozpoznávání tváře. Můžete použít kterýkoli z nich.

## <a name="slack-workspace"></a>Pracovní prostor Slacku

Pokud chcete vytvořit příkaz Slacku, potřebujete oprávnění správce k pracovnímu prostoru Slacku.

1. Pokud už pracovní prostor Slacku s oprávněními správce máte, můžete ho použít. Kromě toho můžete [vytvořit i zcela nový pracovní prostor Slacku](https://slack.com/create?azure-portal=true).

## <a name="local-setup"></a>Místní nastavení

Většinu cvičení v tomto modulu provedete na místním počítači a až jako poslední krok vše nasadíte do sandboxu Azure.

1. Nainstalujte Visual Studio Code.

    Pokud ještě nemáte sadu [Visual Studio Code](https://code.visualstudio.com?azure-portal=true), stáhněte a nainstalujte si ji.

1. Přidejte rozšíření Azure do sady Visual Studio Code.

    Pokud je ještě nemáte, stáhněte a nainstalujte si tato rozšíření sady VS Code:
    - [Azure Account](https://marketplace.visualstudio.com/items?azure-portal=true&itemName=ms-vscode.azure-account)
    - [Azure Functions](https://marketplace.visualstudio.com/items?azure-portal=true&itemName=ms-azuretools.vscode-azurefunctions)

1. Nainstalujte node a npm.

   Pokud je ještě na počítači nemáte místně nainstalované, nainstalujte [node a npm](https://nodejs.org/en/download?azure-portal=true) pro svůj operační systém.

1. Naklonujte počáteční kód.

    Díky klonování počátečního kódu budete moct z tohoto modulu získat maximum. Abyste mohli začít pracovat, máte zdarma k dispozici veškerý kód, který potřebujete k dokončení aplikace, a navíc i nějaký kód pro počáteční spuštění.

    ```bash
    git clone https://github.com/MicrosoftDocs/mslearn-the-mojifier.git
    ```

    Pomocí poskytnutého kódu byste měli být schopní projekt dokončit, ale pokud se vám něco nepovede vyřešit, najdete úplný kód ve větvi `completed`.

1. Přejděte na adresář, který obsahuje naklonované zdrojové úložiště, a pak nainstalujte potřebné balíčky:

    ```bash
    cd mslearn-the-mojifier
    npm install
    ```

1. Zkompilujte kód TypeScriptu do JavaScriptu.

    Svoji aplikaci napíšete pomocí TypeScriptu. TypeScript podporuje kontrolu typů a definice tříd, které využijeme v kódu Mojifieru. Node.js neumí spouštět TypeScript, takže v průběhu vývoje bude nutné převádět typescriptový kód na JavaScript. Kompilátor TypeScriptu `tsc` se nainstaluje ve chvíli, kdy spustíte příkaz `npm install` uvedený výše, a soubor `package.json` se nakonfiguruje tak, aby se daný kompilátor spustil během sestavování. Spusťte následující příkaz:

    ```bash
    npm run build
    ```

    Nechte tento příkaz spuštěný v prostředí terminálu. Sleduje totiž všechny změny v souborech TypeScriptu a převádí je na soubory JavaScriptu. Pokud během cvičení zjistíte, že se něco chová jinak, než očekáváte, podívejte se na výstup v okně terminálu, protože je možné, že v kódu TypeScriptu budou nějaké chyby.
