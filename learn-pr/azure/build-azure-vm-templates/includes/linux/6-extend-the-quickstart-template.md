Teď, když jste definovali prostředek šablony pro rozšíření vlastních skriptů, které na virtuálním počítači nakonfiguruje Nginx, ho můžeme přidat do existující šablony virtuálního počítače a spustit.

Konfigurace Nginx bude vypadat takto.

![Webový prohlížeč zobrazující výslednou konfiguraci Nginx](../../media/6-browser-linux.png)

## <a name="build-the-template"></a>Sestavení šablony

Zde si můžete stáhnout šablonu a upravit ji.

1. Spuštěním příkazu `curl` z prostředí Cloud Shell stáhněte z GitHubu dříve použitou šablonu.

    ```bash
    curl https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json > azuredeploy.json
    ```

1. Prostřednictvím editoru Cloud Shell otevřete soubor **azuredeploy.json**.

    ```bash
    code azuredeploy.json
    ```

1. Vyhledejte v souboru oddíl `resources`. Na začátek tohoto oddílu přidejte prostředek Rozšíření vlastních skriptů, který jste vytvořili v předchozí části, a soubor uložte.

    Pro připomenutí zde tento kód uvádíme.

    ```json
    {
      "name": "[concat(variables('vmName'),'/', 'ConfigureNginx')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "apiVersion": "2018-06-01",
      "location": "[parameters('location')]",
      "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "customScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "fileUris": [
            "https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"
          ]
        },
        "protectedSettings": {
           "commandToExecute": "./configure-nginx.sh"
        }
      },
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
      ]
    },
    ```

    Všimněte si znaku čárky `,` na konci, který je nutný k oddělení prostředků. Na pořadí definování prostředků nezáleží, ale zde ho kvůli jednoduchosti přidáte na začátek.

    Pokud si nevíte rady nebo chcete svou práci porovnat, můžete si výsledný soubor stáhnout z GitHubu.

    ```bash
    curl https://raw.githubusercontent.com/MicrosoftDocs/mslearn-build-azure-vm-templates/master/linux/azuredeploy.json > azuredeploy.json
    ```

    Všechny soubory jsou upravené. Uložte změny do souboru **azuredeploy.json** a zavřete editor.

    Editor zavřete tak, že v rohu kliknete na tři tečky a vyberete **Zavřít editor**.

## <a name="verify-the-template"></a>Ověření šablony

Zde si šablonu ověříte z rozhraní příkazového řádku.

V praxi můžete před spuštěním testovacího prostředí provést testy lint nebo tuto šablonu spustit prostřednictvím vizualizéru Azure Resource Manageru.

Podobně jako dříve ověřte šablonu spuštěním příkazu `az group deployment validate`.

```azurecli
az group deployment validate \
  --resource-group <rgn>[sandbox resource group name]</rgn> \
  --template-file azuredeploy.json \
  --parameters adminUsername=$USERNAME \
  --parameters adminPassword=$PASSWORD \
  --parameters dnsLabelPrefix=$DNS_LABEL_PREFIX
```

Všimněte si, že tentokrát použijete argument `--template-file` a nikoli `--template-uri`, protože odkazujete na místní soubor.

Pokud se zobrazí chyby, vraťte se k předchozí části nebo svůj kód porovnejte s [referenční implementací](https://raw.githubusercontent.com/MicrosoftDocs/mslearn-build-azure-vm-templates/master/linux/azuredeploy.json?azure-portal=true).

## <a name="deploy-the-template"></a>Nasazení šablony

Zde spustíte příkaz podobný příkazu, který jste spustili dříve za účelem nasazení šablony. Protože jste nezměnili žádné existující prostředky &ndash; virtuální počítač, nastavení sítě ani účet úložiště &ndash; neprovede Resource Manager s těmito prostředky žádnou akci. Použije jen právě přidaný prostředek, který spustí rozšíření vlastních skriptů, jež nainstaluje Nginx na virtuální počítač.

Spuštěním příkazu `az group deployment create` aktualizujte nasazení.

```azurecli
az group deployment create \
  --name MyDeployment \
  --resource-group <rgn>[sandbox resource group name]</rgn> \
  --template-file azuredeploy.json \
  --parameters adminUsername=$USERNAME \
  --parameters adminPassword=$PASSWORD \
  --parameters dnsLabelPrefix=$DNS_LABEL_PREFIX
```

Znovu si všimněte, že tentokrát použijete argument `--template-file`, protože odkazujete na místní soubor.

Dokončení tohoto příkazu trvá několik minut. Jako výstup se zobrazí velký blok JSON, který popisuje nasazení.

## <a name="verify-the-deployment"></a>Ověření nasazení

Nasazení bylo úspěšné, takže se podíváme na výslednou konfiguraci v akci.

1. Spuštěním příkazu `az vm show` získejte IP adresu virtuálního počítače a uložte výsledek do proměnné Bash.

    ```azurecli
    IPADDRESS=$(az vm show \
      --name MyUbuntuVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --show-details \
      --query [publicIps] \
      --output tsv)
    ```

1. Spuštěním příkazu `curl` získejte přístup k webovému serveru.

    ```bash
    curl $IPADDRESS
    ```

    Uvidíte toto.

    ```html
    <html><body><h2>Welcome to Azure! My name is MyUbuntuVM.</h2></body></html>
    ```

1. Na samostatné kartě prohlížeče přejděte na daný web.

    Nejprve zobrazte IP adresu.

    ```bash
    echo $IPADDRESS
    ```

    Přejděte na IP adresu, kterou vidíte na samostatné kartě prohlížeče. Uvidíte toto.

    ![Webový prohlížeč zobrazující výslednou konfiguraci Nginx](../../media/6-browser-linux.png)

Skvělá práce! S využitím prostředku Rozšíření vlastních skriptů můžete nasazení rozšířit o další činnosti.