Připomínáme, že pracujeme na aplikaci pro sdílení fotek, která bude využívat službu Azure Storage ke správě obrázků a dalších dat, která ukládáme jménem uživatelů.

[!include[](../../../includes/azure-sandbox-activate.md)]

::: zone pivot="csharp"

Pro zjednodušení našeho scénáře, abychom se mohli zaměřit na rozhraní API služby Storage, vytvoříme novou konzolovou aplikaci .NET Core. Budeme také předpokládat, že má vždy připojení k síti. U aplikací byste však vždy měli posílit ochranu, abyste zajistili, že selhání sítě neovlivní uživatelské prostředí ani nepovede k selhání samotné aplikace.

## <a name="create-a-net-core-application"></a>Vytvoření aplikace .NET Core

.NET Core je multiplatformní verze .NET, která funguje v systémech macOS, Windows a Linux. Nástroje můžete nainstalovat místně nebo můžete k provedení následujících kroků použít Cloud Shell na pravé straně okna.

1. Přihlaste se do služby Cloud Shell nebo otevřete relaci příkazového řádku a vytvořte novou konzolovou aplikaci .NET Core s názvem PhotoSharingApp. Pokud chcete aplikaci vytvořit v konkrétní složce, můžete přidat příznak `-o` nebo `--output`.

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. Spusťte aplikaci a ujistěte se, že se správně sestaví a spustí. V konzole by se měl zobrazit text „Hello, World!“.

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
::: zone-end

::: zone pivot="javascript"

Pro zjednodušení našeho scénáře, abychom se mohli zaměřit na rozhraní API služby Storage, vytvoříme novou aplikaci Node.js, kterou je možné spustit z konzoly. Budeme také předpokládat, že má vždy připojení k síti. U aplikací byste však vždy měli posílit ochranu, abyste zajistili, že selhání sítě neovlivní uživatelské prostředí ani nepovede k selhání samotné aplikace.

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js

Node.js je oblíbená architektura pro spouštění aplikací v jazyce JavaScript. Nejčastěji se používá pro webové aplikace, ale můžete ji použít i ke spouštění logiky z příkazového řádku. Pokud máte nástroje nainstalované místně, můžete následující kroky provést z příkazového řádku. Alternativně můžete k provedení následujících kroků použít Cloud Shell na pravé straně okna.

1. Přihlaste se do služby Cloud Shell nebo otevřete relaci příkazového řádku a vytvořte novou složku PhotoSharingApp.

    ```bash
    mkdir PhotoSharingApp
    ```

1. Přejděte do nové složky a pomocí příkazu `npm` inicializujte novou aplikaci Node.js. Tím se vytvoří soubor **package.json** obsahující metadata popisující aplikaci.

    ```bash
    cd PhotoSharingApp
    npm init -y
    ```

1. Vytvořte nový zdrojový soubor **index.js**, do kterého umístíme náš kód.

    ```bash
    touch index.js
    ```

1. Otevřete soubor **index.js** v editoru. Pokud používáte Cloud Shell, můžete editor otevřít zadáním příkazu `code .`.

1. Do souboru **index.js** vložte následující program. Můžete použít klávesovou zkratku **Ctrl+V** nebo ho vložit po kliknutí pravým tlačítkem myši.

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. Uložte soubor. Můžete k tomu použít nabídku „...“ v pravém horním rohu editoru Cloud Shell.

1. Spusťte aplikaci a ujistěte se, že se správně spustí. V konzole by se měl zobrazit text „Hello, World!“.

    ```bash
    node index.js
    ```

::: zone-end