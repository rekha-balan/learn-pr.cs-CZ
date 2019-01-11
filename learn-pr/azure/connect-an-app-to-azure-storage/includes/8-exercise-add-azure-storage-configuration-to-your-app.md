::: zone pivot="csharp"
Teď do naší aplikace .NET Core přidáme podporu načtení připojovacího řetězce z konfiguračního souboru. Začneme přidáním potřebné technologie pro správu konfigurace v souboru JSON.

## <a name="create-a-json-configuration-file"></a>Vytvoření konfiguračního souboru JSON

1. Příkazem pro změnu adresáře (`cd`) přejděte do složky PhotoSharingApp, pokud v ní ještě nejste.

1. Pomocí nástroje `touch` na příkazovém řádku vytvořte soubor **appsettings.json**.

    ```bash
    touch appsettings.json
    ```

1. Otevřete projekt v editoru. Pokud pracujete místně, použijte editor, kterému dáváte přednost. Doporučujeme **Visual Studio Code**, což je rozšiřitelné integrované vývojové prostředí (IDE) pro různé platformy. Pokud pracujete ve službě Cloud Shell, doporučujeme použít editor Cloud Shell. Následující příkaz lze použít v obou prostředích.

    ```bash
    code .
    ```

1. V editoru vyberte soubor **appsettings.json** a přidejte do něj následující text. Soubor uložte. V pravém horním rohu editoru Cloud Shell je nabídka s běžnými operacemi se soubory.

    ```json
    {
      "StorageAccountConnectionString": "<value>"
    }
    ```

1. Teď potřebujeme získat připojovací řetězec účtu úložiště a umístit ho do konfigurace pro naši aplikaci. V Cloud Shellu spusťte následující příkaz.

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. Zkopírujte připojovací řetězec, který bude vrácen z tohoto příkazu, a nahraďte jím `"<value>"` v souboru **appsettings.json**. Soubor uložte.

1. Potom v editoru otevřete soubor projektu (**PhotoSharingApp.csproj**).

1. Přidejte následující blok konfigurace, který do projektu zahrne nový soubor a zkopíruje ho do výstupní složky. Tím se zajistí, že se konfigurační soubor aplikace po kompilaci nebo sestavení aplikace umístí do výstupního adresáře.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. Soubor uložte. (Nezapomeňte to udělat, jinak změny po přidání balíčku níže ztratíte.)

## <a name="add-support-to-read-a-json-configuration-file"></a>Přidání podpory pro čtení konfiguračního souboru JSON

Aplikace .NET Core ke čtení konfiguračního souboru JSON potřebuje další balíčky NuGet.

1. V části okna s příkazovým řádkem přidejte odkaz na balíček NuGet **Microsoft.Extensions.Configuration.Json**.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Přidání kódu pro čtení konfiguračního souboru

Když jsme teď přidali požadované knihovny umožňující čtení konfigurace, je potřeba povolit tuto funkci v konzolové aplikaci.

1. Vyberte v editoru soubor **Program.cs**.

1. V horní části souboru se nachází řádek **using System;**. Pod tento řádek přidejte následující řádky kódu:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Obsah metody **Main** nahraďte následujícím kódem. Tento kód provede inicializaci konfiguračního systému tak, aby se umožnilo čtení ze souboru **appsettings.json**.

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

Soubor **Program.cs** by teď měl vypadat takto:

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace PhotoSharingApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
        }
    }
}
```

::: zone-end

::: zone pivot="javascript"

Teď do naší aplikace Node.js přidáme podporu načtení připojovacího řetězce z konfiguračního souboru. Začneme přidáním potřebné technologie pro správu konfigurace ze souboru JavaScriptu.

## <a name="create-a-env-configuration-file"></a>Vytvoření konfiguračního souboru .env

1. Ujistěte se, že jste ve správném pracovním adresáři pro váš projekt.

1. Pomocí nástroje `touch` na příkazovém řádku vytvořte soubor **.env**.

    ```bash
    touch .env
    ```

1. Otevřete projekt v editoru Cloud Shell.

    ```bash
    code .
    ```

1. V editoru vyberte soubor **.env** a přidejte do něj následující text. Možná bude nutné kliknout na tlačítko aktualizace v kódu, aby se nové soubory zobrazily. Soubor uložte – v pravém horním rohu online editoru je nabídka s běžnými operacemi se soubory.

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > Hodnota **AZURE_STORAGE_CONNECTION_STRING** je pevně zakódovaná proměnná prostředí, kterou rozhraní API služby Storage používají k vyhledání přístupových klíčů. Pokud chcete, můžete použít vlastní název, který však musíte zadat pro objekt `BlobService` při jeho vytváření v aplikaci Node.js.

1. Teď potřebujeme získat připojovací řetězec účtu úložiště a umístit ho do konfigurace pro naši aplikaci. V Cloud Shellu spusťte následující příkaz.

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. Zkopírujte připojovací řetězec, který bude vrácen z tohoto příkazu, bez uvozovek, a nahraďte jím `<value>` v souboru **.env**.

1. Soubor uložte.

## <a name="add-support-to-read-an-environment-configuration-file"></a>Přidání podpory pro čtení konfiguračního souboru prostředí

Do aplikace Node.js můžete zahrnout podporu pro čtení ze souboru **.env** přidáním balíčku **dotenv**.

1. V části okna s příkazovým řádkem přidejte závislost do balíčku **dotenv** pomocí `npm`.

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Přidání kódu pro čtení konfiguračního souboru

Teď, když jsme přidali požadované knihovny umožňující čtení konfigurace, je potřeba povolit tuto funkci v aplikaci.

1. V editoru vyberte soubor **index.js**.

1. V horní části souboru se nachází řádek **#!/usr/bin/env node**. Pod tento řádek přidejte příkaz `require` pro načtení balíčku **dotenv**. Tím se programu zpřístupní proměnné prostředí definované v souboru **.env**.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

Když teď máme všechno připravené, můžeme začít přidávat kód pro použití našeho účtu úložiště.
