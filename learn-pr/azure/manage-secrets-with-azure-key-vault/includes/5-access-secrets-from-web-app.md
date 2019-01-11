Když teď víte, jak povolení spravovaných identit pro prostředky Azure vytvoří identitu pro vaši aplikaci k ověřování, vytvoříme aplikaci, která tuto identitu použije pro přístup k tajným kódům v trezoru.

::: zone pivot="csharp"

## <a name="reading-secrets-in-an-aspnet-core-app"></a>Čtení tajných kódů v aplikaci ASP.NET Core

Rozhraní API služby Azure Key Vault je rozhraní REST API, které zpracovává všechny operace správy a používání klíčů a trezorů. Každý tajný kód v trezoru má jedinečnou adresu URL a hodnoty tajných kódů se načítají pomocí požadavků HTTP GET.

Oficiální klient služby Key Vault pro .NET Core je třída `KeyVaultClient` v balíčku NuGet Microsoft.Azure.KeyVault. Není nutné používat ji přímo, prostřednictvím &mdash; s použitím metody `AddAzureKeyVault` v ASP.NET Core, můžete také načíst všechny tajné kódy z trezoru do rozhraní Configuration API při spuštění. Tato technika vám poskytne přístup ke všem tajným kódům s použitím stejného rozhraní `IConfiguration`, které používáte pro zbytek konfigurace. Aplikace, které používají `AddAzureKeyVault`, vyžadují k trezoru oprávnění **Get** i **List**.

> [!TIP]
> Bez ohledu na architekturu nebo jazyk, které používáte při vývoji aplikace, byste ji měli navrhnout tak, aby ukládala hodnoty tajných kódů místně do mezipaměti nebo je načítala do paměti při spuštění aplikace, pokud nemáte konkrétní důvod, proč to nedělat. Jejich čtení přímo z trezoru při každém použití je zbytečně pomalé a drahé.

`AddAzureKeyVault` vyžaduje jako vstup jen název trezoru, který získáme z konfigurace naší místní aplikace. Také automaticky zpracovává ověřování spravované identity &mdash; při použití v aplikaci nasazené do služby Azure App Service s povolenými spravovanými identitami pro prostředky Azure – bude zjišťovat službu tokenů spravovaných identit a používat ji k ověřování. Je to dobrá volba pro většinu scénářů a implementuje všechny osvědčené postupy, proto ji použijeme i v tomto cvičení.

::: zone-end

::: zone pivot="javascript"

## <a name="reading-secrets-in-a-nodejs-app"></a>Čtení tajných kódů v aplikaci Node.js

Rozhraní API služby Azure Key Vault je rozhraní REST API, které zpracovává všechny operace správy a používání klíčů a trezorů. Každý tajný kód v trezoru má jedinečnou adresu URL a hodnoty tajných kódů se načítají pomocí požadavků HTTP GET.

Oficiální klient služby Key Vault pro aplikace Node.js je třída `KeyVaultClient` v balíčku npm `azure-keyvault`. Aplikace zahrnující názvy tajných kódů ve své konfiguraci nebo kódu, budou obecně potřebovat pouze metodu `getSecret`, která načte tajnou hodnotu na základě názvu. `getSecret` vyžaduje, aby identita vaší aplikace měla k trezoru oprávnění **Get**. Aplikace navržené tak, aby z trezoru načítaly veškeré tajné kódy, budou také používat metodu `getSecrets`, která načítá seznam tajných kódů a vyžaduje oprávnění **List**.

Než bude vaše aplikace moct vytvořit instanci `KeyVaultClient`, musí ověřením v trezoru získat objekt přihlašovacích údajů. K ověření použijte jednu z funkcí přihlášení poskytovaných balíčkem npm `ms-rest-azure`. Každá z těchto funkcí vrátí objekt přihlašovacích údajů, který lze použít k vytvoření `KeyVaultClient`. Funkce `loginWithAppServiceMSI` bude automaticky používat přihlašovací údaje spravované identity, které aplikaci zpřístupní služba App Service prostřednictvím proměnných prostředí. Pro testovací prostředí nebo jiná prostředí mimo službu App Service, ve kterých vaše aplikace nemá přístup ke spravované identitě, můžete pro aplikaci vytvořit instanční objekt ručně a funkci `loginWithServicePrincipalSecret` použít k ověření.

> [!TIP]
> Bez ohledu na architekturu nebo jazyk, které používáte při vývoji aplikace, byste ji měli navrhnout tak, aby ukládala hodnoty tajných kódů místně do mezipaměti nebo je načítala do paměti při spuštění aplikace, pokud nemáte konkrétní důvod, proč to nedělat. Jejich čtení přímo z trezoru při každém použití je zbytečně pomalé a drahé.

::: zone-end

## <a name="handling-secrets-in-an-app"></a>Používání tajných kódů v aplikaci

Jakmile je tajný kód načtený do vaší aplikace, záleží na aplikaci, aby s ním nakládala bezpečně. V aplikaci, kterou v tomto modulu vytváříme, zapíšeme tajný kód do odpovědi klienta a zobrazíme ho ve webovém prohlížeči, abychom prokázali, že se úspěšně načetl. **Návrat tajného kódu klientovi *není* něco, co byste měli běžně dělat!** Obvykle budete tajné kódy používat třeba pro inicializaci klientské knihovny pro databáze nebo pro vzdálená rozhraní API.

> [!IMPORTANT]
> Vždy pečlivě zkontrolujte kód a přesvědčte se, že aplikace nikdy zapíše tajné kódy na žádný druh výstupu, včetně protokolů, úložiště a odpovědí.

## <a name="exercise"></a>Cvičení

::: zone pivot="csharp"

Vytvoříme nové webové API ASP.NET Core a použijeme `AddAzureKeyVault` k načtení tajného kódu z trezoru.

### <a name="create-the-app"></a>Vytvoření aplikace

V terminálu Azure Cloud Shell spusťte následující příkaz, kterým vytvoříte novou webovou aplikaci ASP.NET Core API a otevřete ji v editoru.

```console
dotnet new webapi -o KeyVaultDemoApp
cd KeyVaultDemoApp
code .
```

Po načtení editoru spusťte v prostředí následující příkazy které přidají balíček NuGet obsahující `AddAzureKeyVault` a obnoví všechny závislé součásti aplikace.

```console
dotnet add package Microsoft.Extensions.Configuration.AzureKeyVault -v 2.1.1
dotnet restore
```

### <a name="add-code-to-load-and-use-secrets"></a>Přidání kódu pro načtení a použití tajných kódů

Abychom si předvedli správné využití služby Key Vault, upravíme naši aplikaci tak, aby načítala tajné kódy z trezoru při spuštění. Přidáme také nový kontroler s koncovým bodem, který získá tajný kód **SecretPassword** z trezoru.

Nejprve při spuštění aplikace: Otevřete `Program.cs`, odstraňte obsah a nahraďte ho následujícím kódem:

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;

namespace KeyVaultDemoApp
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((context, config) =>
                {
                    // Build the current set of configuration to load values from
                    // JSON files and environment variables, including VaultName.
                    var builtConfig = config.Build();

                    // Use VaultName from the configuration to create the full vault URL.
                    var vaultUrl = $"https://{builtConfig["VaultName"]}.vault.azure.net/";

                    // Load all secrets from the vault into configuration. This will automatically
                    // authenticate to the vault using a managed identity. If a managed identity
                    // is not available, it will check if Visual Studio and/or the Azure CLI are
                    // installed locally and see if they are configured with credentials that can
                    // access the vault.
                    config.AddAzureKeyVault(vaultUrl);
                })
                .UseStartup<Startup>();
    }
}
```

> [!IMPORTANT]
> Až dokončíte úpravy souborů, nezapomeňte je uložit. Můžete to udělat prostřednictvím nabídky „...“ nebo klávesové zkratky (<kbd>Ctrl+S</kbd> ve Windows a Linuxu, <kbd>Cmd+S</kbd> v macOS).

Jediná změna oproti výchozímu kódu je přidání `ConfigureAppConfiguration`. V tomto místě načteme název trezoru z konfigurace a zavoláme s ním `AddAzureKeyVault`.

Dále následuje kontroler: Vytvořte nový soubor ve složce `Controllers` nazvaný `SecretTestController.cs` a vložte do něj následující kód.

> [!TIP]
> Pro vytvoření souboru použijte v prostředí příkaz `touch`. V tomto případě použijte `touch Controllers/SecretTestController.cs`. Budete muset v editoru v podokně Soubory kliknout na tlačítko pro obnovení, aby se zobrazil.

```csharp
using System;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;

namespace KeyVaultDemoApp.Controllers
{
    [Route("api/[controller]")]
    public class SecretTestController : ControllerBase
    {
        private readonly IConfiguration _configuration;

        public SecretTestController(IConfiguration configuration)
        {
            _configuration = configuration;
        }

        [HttpGet]
        public IActionResult Get()
        {
            // Get the secret value from configuration. This can be done anywhere
            // we have access to IConfiguration. This does not call the Key Vault
            // API, because the secrets were loaded at startup.
            var secretName = "SecretPassword";
            var secretValue = _configuration[secretName];

            if (secretValue == null)
            {
                return StatusCode(
                    StatusCodes.Status500InternalServerError,
                    $"Error: No secret named {secretName} was found...");
            }
            else {
                return Content($"Secret value: {secretValue}" +
                    Environment.NewLine + Environment.NewLine +
                    "This is for testing only! Never output a secret " +
                    "to a response or anywhere else in a real app!");
            }
        }
    }
}
```

Spusťte v prostředí příkaz `dotnet build` a zkontrolujte, že se všechno zkompilovalo. Aplikace je připravena ke spuštění – teď ji pojďme přenést do Azure!

::: zone-end

::: zone pivot="javascript"

Vytvoříme nové webové rozhraní API se souborem Express.js a použijeme balíčky `azure-keyvault` a `ms-rest-azure` k načtení tajného kódu z našeho trezoru.

### <a name="create-the-app"></a>Vytvoření aplikace

V terminálu Azure Cloud Shell spusťte následující příkazy pro inicializaci nové aplikace Node.js, nainstalujte potřebné balíčky a nový soubor otevřete v editoru.

```console
mkdir KeyVaultDemoApp
cd KeyVaultDemoApp
npm init -y
npm install ms-rest-azure azure-keyvault express
touch app.js
code app.js
```

### <a name="add-code-to-load-and-use-secrets"></a>Přidání kódu pro načtení a použití tajných kódů

Abychom si předvedli správné využití služby Key Vault, načte naše aplikace tajné kódy z trezoru při spuštění. Abychom si předvedli, že se naše tajné kódy načetly, vytvoříme koncový bod, který zobrazuje hodnotu tajného kódu **SecretPassword**.

Nejprve nastavte aplikaci vložením následujícího kódu do editoru. Naimportujete tak potřebné balíčky, nastavíte konfiguraci portu a adresy URL trezoru a vytvoříte nový objekt pro ukládání názvů a hodnot tajných kódů.

```javascript
// Importing dependencies
const msRestAzure = require('ms-rest-azure');
const keyVault = require('azure-keyvault');
const app = require('express')();

// Initialize port
const port = process.env.PORT || 3000;

// Create Vault URL from App Settings
const vaultUrl = `https://${process.env.VaultName}.vault.azure.net/`;

// Map of key vault secret names to values
let vaultSecretsMap = {};
```

> [!IMPORTANT]
> Ujistěte se, že při práci soubory ukládáte, zejména, když práci dokončíte. Můžete to udělat prostřednictvím nabídky „...“ nebo klávesové zkratky (<kbd>Ctrl+S</kbd> ve Windows a Linuxu, <kbd>Cmd+S</kbd> v macOS).

V dalším kroku přidáme kód pro ověření v trezoru a načteme tajné kódy. Tento krok přidáme jako dvě samostatné funkce. Za dříve přidaný kód vložte několik prázdných řádků a potom vložte následující kód:

```javascript
const authenticateToKeyVault = async () => {
  try {
    let credentials;
    if (process.env.NODE_ENV === 'production') {
      credentials = await msRestAzure.loginWithAppServiceMSI({ resource: 'https://vault.azure.net' });
    } else {
      // For non-App Service environments. Set the APP_ID, APP_SECRET and TENANT_ID environment
      // variables to use.
      const appId = process.env.APP_ID;
      const appSecret = process.env.APP_SECRET;
      const tenantId = process.env.TENANT_ID;
      credentials = await msRestAzure.loginWithServicePrincipalSecret(appId, appSecret, tenantId);
    }
    return credentials;
  } catch(err) {
    throw err.message;
  }
}

const getKeyVaultSecrets = async credentials => {
  // Create a key vault client
  let keyVaultClient = new keyVault.KeyVaultClient(credentials);
  try {
    let secrets = await keyVaultClient.getSecrets(vaultUrl);
    // For each secret name, get the secret value from the vault
    for (const secret of secrets) {
      // Only load enabled secrets - getSecret will return an error for disabled secrets
      if (secret.attributes.enabled) {
        let secretId = secret.id;
        let secretName = secretId.substring(secretId.lastIndexOf('/') + 1);
        let secretValue = await keyVaultClient.getSecret(vaultUrl, secretName, '');
        vaultSecretsMap[secretName] = secretValue.value;
      }
    }
  } catch(err) {
    console.log(err.message)
  }
}
```

Teď vytvořte koncový bod Express, který použijeme k ověření toho, jestli se náš tajný kód načetl. Potom vložte tento kód:

```javascript
app.get('/api/SecretTest', (req, res) => {
  let secretName = 'SecretPassword';
  let response;
  if (secretName in vaultSecretsMap) {
    response = `Secret value: ${vaultSecretsMap[secretName]}\n\nThis is for testing only! Never output a secret to a response or anywhere else in a real app!`;
  } else {
    response = `Error: No secret named ${secretName} was found...`
  }
  res.type('text');
  res.send(response);
});
```

Nakonec zavoláme naše funkce, aby načetly tajné kódy z trezoru. Potom se spustí aplikace. K dokončení aplikace vložte tento poslední fragment kódu:

```javascript
(async () =>  {
  let credentials = await authenticateToKeyVault();
  await getKeyVaultSecrets(credentials);
  app.listen(port, () => {
    console.log(`Server running at http://localhost:${port}`);
  });
})().catch(err => console.log(err));
```

Dokončili jsme psaní kódu, takže nezapomeňte soubor uložit. Aplikace je připravena ke spuštění – teď ji pojďme přenést do Azure!

::: zone-end