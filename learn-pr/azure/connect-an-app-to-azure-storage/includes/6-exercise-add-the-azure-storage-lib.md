::: zone pivot="csharp"
Pojďme integrovat klientskou knihovnu Azure Storage do konzolové aplikace .NET Core.

Klientskou knihovnu Azure Storage pro .NET distribuuje NuGet. K aplikacím .NET nebo .NET Core budete chtít přidat balíček **WindowsAzure.Storage**.

## <a name="add-the-azure-storage-nuget-package"></a>Přidání balíčku NuGet pro Azure Storage

1. Příkazem pro změnu adresáře (`cd`) přejděte na terminálu do složky PhotoSharingApp, pokud v ní ještě nejste.

1. Přidejte do aplikace balíček **WindowsAzure.Storage**.

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. Zatímco se stahuje klientská knihovna a všechny požadované závislosti, měla by v konzole probíhat nějaká aktivita. Jakmile se vše dokončí, znovu sestavte a spusťte aplikaci, abyste se přesvědčili, že je všechno připravené.

    ```bash
    dotnet run
    ```

1. Jejím výstupem by stále mělo být „Hello World!“.

::: zone-end

::: zone pivot="javascript"

Pojďme do aplikace integrovat **klientskou knihovnu Microsoft Azure Storage pro Node.js a JavaScript**.

Klientská knihovna pro Node.js je k dispozici prostřednictvím správce balíčků pro uzly (Node Package Manager). Do souboru **packages.json** budete chtít přidat balíček **azure-storage**.

> [!NOTE]
> **Klientská knihovna Microsoft Azure Storage pro Node.js a JavaScript** je určená pro serverové aplikace. Pokud používáte JavaScript na straně klienta, použijte **klientskou knihovnu Azure Storage pro JavaScript**, která poskytuje stejné funkce, ale je upravená k tomu, aby běžela v prohlížeči.

## <a name="add-the-azure-storage-package"></a>Přidání balíčku Azure Storage

1. Příkazem pro změnu adresáře (`cd`) přejděte v Cloud Shellu do složky PhotoSharingApp, pokud v ní ještě nejste.

1. Přidejte do aplikace balíček **azure-storage**. Nezapomeňte použít možnost `--save`, aby se balíček trvale zachoval v souboru **packages.json**.

    ```bash
    npm install azure-storage --save
    ```

1. Zatímco se stahuje klientská knihovna a všechny požadované závislosti, měla by v konzole probíhat nějaká aktivita. Jakmile se vše dokončí, znovu sestavte a spusťte aplikaci, abyste se přesvědčili, že je všechno připravené.

    ```bash
    node index.js
    ```

1. Jejím výstupem by stále mělo být „Hello, World!“.

::: zone-end

Když teď máme potřebné knihovny integrované, podívejme se na běžné úkoly, které budete ve svém kódu při práci s Azure Storage provádět.
