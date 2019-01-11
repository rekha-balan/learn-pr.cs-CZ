Rozhodnete se vytvořit server Azure Database for PostgreSQL k ukládání tras získaných z fitness zařízení běžců. Na základě historických objemů zachycených dat víte, že požadované úložiště vašeho serveru musí být nastavené na 20 GB. K zajištění podpory vašich požadavků na zpracování potřebujete podporu výpočetních prostředků 5. generace s 1 virtuálním jádrem. Také víte, že zálohy dat potřebujete uchovávat 15 dnů.

## <a name="create-an-azure-postgresql-database-server-with-the-azure-cli"></a>Vytvoření databázového serveru Azure PostGreSQL pomocí Azure CLI

Mějte na paměti, že chcete nastavit velikost úložiště vašeho serveru na 20 GB, podporu výpočetních prostředků 5. generace s 1 virtuálním jádrem a dobu uchování záloh dat o délce 15 dnů.

1. K vytvoření nové databáze použijte metodu `az postgres server create`. Existuje několik parametrů, které určíte:
    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`

2. Zkuste, jestli dokážete sestavit příkaz a vyplnit parametry, aniž byste se dívali na řešení níže. Tady je několik tipů.
    - Nahraďte `<values>` vlastními hodnotami. 
    - Nezapomeňte, že název serveru se musí skládat z malých písmen (a–z), čísel (0–9) a spojovníku (-).
    - Jako skupinu prostředků použijte <rgn>[název skupiny prostředků sandboxu]</rgn>.
    - Umístění vyberte z následujícího seznamu:  
        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    ```azurecli
    az postgres server create \
        --name <unique_server_name> \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location eastus \
        --sku-name B_Gen5_1 \
        --storage-size 20480 \
        --backup-retention 15 \
        --version 10 \
        --admin-user <admin_user_name> \
        --admin-password <server_admin_password>
    ```

    Po spuštění příkazu bude systém několik minut zpracovávat informace. Počkejte, až se příkaz dokončí.

    Jakmile se dokončí, systém vrátí řetězec jazyka JSON (JavaScript Object Notation), který popisuje server. Pokud došlo k chybě, zobrazí se chybová zpráva. Díky informacím v této chybě si můžete zkontrolovat a opravit parametry příkazu a zkusit ho spustit znovu.

    Objekt JSON bude vypadat přibližně takto:

    ```json
    {
      "administratorLogin": "azureuser",
      "earliestRestoreDate": "2018-09-17T00:35:50.170000+00:00",
      "fullyQualifiedDomainName": "secondserver8.postgres.database.azure.com",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/<rgn>[sandbox Resource Group]</rgn>/providers/Microsoft.DBforPostgreSQL/servers/secondserver8",
      "location": "eastus",
      "name": "secondserver8",
      "resourceGroup": "<rgn>[sandbox Resource Group]</rgn>",
      "sku": {
        "capacity": 1,
        "family": "Gen5",
        "name": "B_Gen5_1",
        "size": null,
        "tier": "Basic"
      },
      "sslEnforcement": "Enabled",
      "storageProfile": {
        "backupRetentionDays": 15,
        "geoRedundantBackup": "Disabled",
        "storageMb": 20480
      },
      "tags": null,
      "type": "Microsoft.DBforPostgreSQL/servers",
      "userVisibleState": "Ready",
      "version": "10"
    }
    ```

Úspěšně jste vytvořili server PostgreSQL pomocí rozhraní příkazového řádku Azure (Azure CLI). V další lekci uvidíte, jak nakonfigurovat nastavení zabezpečení serveru.
