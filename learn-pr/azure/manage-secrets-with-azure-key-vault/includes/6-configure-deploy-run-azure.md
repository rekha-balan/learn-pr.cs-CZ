Nyní je čas spustit aplikaci v Azure. Potřebujeme vytvořit aplikaci služby Azure App Service, nastavit pro ni spravovanou identitu a konfiguraci trezoru a nasadit kód.

## <a name="create-the-app-service-plan-and-app"></a>Vytvoření plánu a aplikace služby App Service

Vytvoření aplikace služby App Service se skládá ze dvou kroků: Nejprve vytvoříte *plán* a teprve potom *aplikaci*.

Název *plánu* musí být jedinečný pouze v rámci vašeho předplatného, takže můžete použít dříve použitý název: **keyvault-exercise-plan**. Název aplikace ale musí být globálně jedinečný, takže si nějaký vymyslete. Použijte stejné umístění jako při vytváření trezoru.

V Azure Cloud Shellu spusťte následující příkaz pro vytvoření plánu služby App Service:

```azurecli
az appservice plan create \
    --name keyvault-exercise-plan \
    --resource-group <rgn>[sandbox resource group name]</rgn>
```

Dále spusťte příkaz pro vytvoření webové aplikace, která používá právě vytvořený plán služby App Service:

::: zone pivot="csharp"

```azurecli
az webapp create \
    --plan keyvault-exercise-plan \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name <your-unique-app-name>
```

::: zone-end

::: zone pivot="javascript"

```azurecli
az webapp create \
    --plan keyvault-exercise-plan \
    --runtime "node|10.6" \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name <your-unique-app-name>
```

::: zone-end

## <a name="add-configuration-to-the-app"></a>Přidání konfigurace do aplikace

::: zone pivot="csharp"

U nasazení do Azure budeme postupovat podle osvědčeného postupu služby App Service, takže konfiguraci VaultName dáme do nastavení aplikace místo do konfiguračního souboru. Spusťte tento příkaz pro vytvoření nastavení aplikace:

```azurecli
az webapp config appsettings set \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name <your-unique-app-name> \
    --settings 'VaultName=<your-unique-vault-name>'
```

::: zone-end

::: zone pivot="javascript"

U nasazení do Azure budeme postupovat podle osvědčeného postupu služby App Service, takže konfiguraci VaultName dáme do nastavení aplikace místo do konfiguračního souboru. Dále nastavíme nastavení `SCM_DO_BUILD_DURING_DEPLOYMENT` na hodnotu `true`, aby služba App Service obnovila balíčky aplikace na serveru a vytvořila konfiguraci potřebnou ke spuštění aplikace. Spusťte tento příkaz pro vytvoření nastavení aplikace:

```azurecli
az webapp config appsettings set \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name <your-unique-app-name> \
    --settings 'VaultName=<your-unique-vault-name>' 'SCM_DO_BUILD_DURING_DEPLOYMENT=true'
```

::: zone-end

## <a name="enable-managed-identity"></a>Povolení spravované identity

K povolení spravované identity u aplikace stačí jeden řádek – k jejímu povolení u vaší aplikace spusťte toto:

```azurecli
az webapp identity assign \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name <your-unique-app-name>
```

Z výsledného výstupu JSON zkopírujte hodnotu **principalId**. PrincipalId je jedinečná hodnota ID nové identity aplikace v Azure Active Directory a budeme ji potřebovat v dalším kroku.

## <a name="grant-access-to-the-vault"></a>Udělení přístupu k trezoru

Posledním krokem před nasazením je přiřazení oprávnění služby Key Vault spravované identitě vaší aplikace. Hodnotu **principalId**, kterou jste zkopírovali v předchozím kroku, použijte jako hodnotu pro **object-id** v následujícím příkazu. Spuštěním tohoto příkazu udělíte oprávnění k přístupu **Get** a **List**:

```azurecli
az keyvault set-policy \
    --secret-permissions get list \
    --name <your-unique-vault-name> \
    --object-id <your-managed-identity-principleid>
```

## <a name="deploy-the-app-and-try-it-out"></a>Nasazení a vyzkoušení aplikace

::: zone pivot="csharp"

Veškerá konfigurace je nastavená a jste připraveni aplikaci nasadit. Následující příkazy publikují web do složky `pub`, zazipují ji do souboru `site.zip` a tento soubor nasadí do služby App Service.

> [!NOTE]
> Pokud jste to ještě neudělali, budete muset `cd` zpět do adresáře KeyVaultDemoApp.

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*

az webapp deployment source config-zip \
    --src site.zip \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name <your-unique-app-name>
```

::: zone-end

::: zone pivot="javascript"

Veškerá konfigurace je nastavená a jste připraveni aplikaci nasadit. Následující příkazy zazipují vaši aplikaci do `site.zip` a tento soubor nasadí do služby App Service. Ze souboru zip vyloučíme `node_modules`, protože služba App Service je automaticky obnoví při nasazení.

> [!NOTE]
> Pokud jste to ještě neudělali, budete muset `cd` zpět do adresáře KeyVaultDemoApp.

```azurecli
zip site.zip * -x node_modules/

az webapp deployment source config-zip \
    --src site.zip \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name <your-unique-app-name>
```

::: zone-end

Dokončení nasazení může trvat minutu či dvě. Jakmile získáte výsledek, který značí, že je web nasazený, otevřete v prohlížeči adresu `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest`. První spuštění aplikace na serveru bude chvíli trvat, ale až se spustí, měli byste vidět tajnou hodnotu **reindeer_flotilla**.

Vaše aplikace je dokončená a nasazená.