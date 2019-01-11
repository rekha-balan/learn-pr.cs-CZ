Pojďme vytvořit klientskou aplikaci pro práci s frontou. Potom do kódu přidáme náš připojovací řetězec.

> [!NOTE]
> Klientskou aplikaci můžete vytvořit na místním počítači, pokud máte nainstalovaný .NET Core, nebo přímo v prostředí Cloud Shell.

## <a name="create-the-application"></a>Vytvoření aplikace

Vytvoříme aplikaci .NET Core, kterou můžete spustit v systému Linux, macOS nebo Windows. Pojmenujeme ji **QueueApp**. Pro zjednodušení použijeme jednu aplikaci, která bude prostřednictvím naší fronty odesílat i přijímat zprávy.

1. Pomocí příkazu `dotnet new` vytvořte novou konzolovou aplikaci **QueueApp**. Příkazy můžete zadávat do služby Cloud Shell na pravé straně nebo (v případě, že pracujete místně) do okna terminálu nebo konzoly. Tímto příkazem se vytvoří jednoduchá aplikace s jedním zdrojovým souborem: `Program.cs`.

    ```azurecli
    dotnet new console -n QueueApp
    ```

1. Přepněte do nově vytvořené složky `QueueApp` a sestavte aplikaci, abyste ověřili, že je vše v pořádku.

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a>Získání připojovacího řetězce

Nezapomeňte, že připojovací řetězec je k dispozici na webu Azure Portal v části **Nastavení > Přístupové klíče** vašeho účtu úložiště.

Můžete ho načíst také pomocí nástrojů PowerShellu nebo Azure CLI. Použijeme příkaz Azure CLI. Nezapomeňte nahradit `<name>` konkrétním názvem účtu úložiště, který jste vytvořili. Pokud si ho nepamatujete, můžete použít příkaz `az storage account list`.

```azurecli
az storage account show-connection-string --name <name> --resource-group <rgn>[sandbox resource group name]</rgn>
```

Tento příkaz vrátí blok JSON s vaším připojovacím řetězcem. Ten bude obsahovat název účtu úložiště a klíč účtu:

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a>Přidání připojovacího řetězce do aplikace

Nakonec přidáme připojovací řetězec do naší aplikace, aby měla přístup k účtu úložiště.

> [!WARNING]
> Pro zjednodušení umístíte připojovací řetězec do souboru **Program.cs**. V aplikaci určené pro ostrý provoz by měl být uložen na bezpečném místě. Na straně serveru doporučujeme použít službu Azure Key Vault.

1. Zadáním příkazu `code .` v terminálu otevřete online editor kódu. Alternativně můžete použít libovolné integrované vývojové prostředí (IDE). Doporučujeme Visual Studio Code, což je skvělé integrované vývojové prostředí (IDE) pro různé platformy.

1. V projektu otevřete zdrojový soubor `Program.cs`.

1. Do třídy `Program` přidejte hodnotu `const string`, do které se uloží připojovací řetězec. Potřebujete pouze hodnotu (začíná textem **DefaultEndpointsProtocol**).

1. Soubor uložte. Provedete to kliknutím na tři tečky ... v pravém rohu editoru cloudu nebo pomocí klávesové zkratky (<kbd>Ctrl+S</kbd> ve Windows a Linuxu, <kbd>Cmd+S</kbd> v macOS).

Váš kód by měl vypadat přibližně takto (hodnota řetězce bude pro váš účet jedinečná).

```csharp
...
namespace QueueApp
{
    class Program
    {
        private const string ConnectionString = "DefaultEndpointsProtocol=https; ...";
        
        ...
    }
}
```

Když teď máme tento úvodní projekt nastavený, podívejme se, jak s frontou pracovat v kódu. Vše začíná _zprávami_.