V této části si nastavíte prostředky, které budete v rámci tohoto modulu používat. Představme si základní architekturu se serverem hostujícím aplikaci, kterou využívají vaši zákazníci a která se za účelem ukládání dat připojuje k databázi. Aplikace je spuštěná na virtuálním počítači a databáze se nedávno migrovala z databáze SQL Serveru spuštěné na virtuálním počítači do databáze ve službě Azure SQL Database. Abychom si ukázali, jak můžete databázi zabezpečit, nastavíme si pro práci s tímto modulem následující prostředky:

- Virtuální počítač s Linuxem s názvem _appServer_. Tento server má sloužit jako aplikační server, k němuž se budou uživatelé připojovat, a bude potřeba ho připojit k databázi. Instalací `sqlcmd` na virtuální počítač nasimulujeme aplikaci spuštěnou na virtuálním počítači _appServer_, která se připojuje k databázi.
- Logický server Azure SQL Database. Tento logický server potřebujeme k hostování jedné nebo více databází.
- Databázi s názvem _marketplaceDb_ na našem logickém serveru. Vytvoříme ji pomocí ukázkové databáze _AdventureWorksLT_, abychom měli k dispozici tabulky a data. Budou mezi ně patřit i některá citlivá data (například e-mailové adresy a telefonní čísla), která budeme chtít náležitým způsobem zabezpečit.

Pojďme se tedy pustit do práce.

<!-- Activate the sandbox -->
[!INCLUDE [azure-sandbox-activate](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-sql-database"></a>Vytvoření databáze Azure SQL Database

1. Nejprve si nastavíme několik proměnných. Nahraďte níže uvedené hodnoty ve špičatých závorkách (`<>`) libovolnými hodnotami. Nezapomeňte, že `<password>` musí mít aspoň osm znaků a obsahovat znaky z minimálně tří z těchto kategorií: velká písmena, malá písmena, číslice a jiné než alfanumerické znaky. Uložte si přihlášení po pozdější použití.

    ```bash
    # Set an admin login and password for your database
    export ADMINLOGIN=<ServerAdmin>
    export PASSWORD=<password>
    # Set the logical SQL server name. We'll add a random string as it needs to be globally unique.
    export SERVERNAME=server$RANDOM
    export RESOURCEGROUP=<rgn>[sandbox resource group name]</rgn>
    # Set the location, we'll pull the location from our resource group.
    export LOCATION=$(az group show --name <rgn>[sandbox resource group name]</rgn> | jq -r '.location')
    ```

1. Spuštěním následujícího příkazu vytvořte nový logický server Azure SQL Database.

    ```azurecli
    az sql server create \
        --name $SERVERNAME \
        --resource-group $RESOURCEGROUP \
        --location $LOCATION \
        --admin-user $ADMINLOGIN \
        --admin-password $PASSWORD
    ```

1. Spuštěním příkazu níže teď na tomto novém logickém serveru vytvořte databázi s názvem **marketplaceDb**. Jako šablona se použijte databáze _AdventureWorksLT_, takže budeme mít k dispozici předem vyplněné tabulky.

    ```azurecli
    az sql db create --resource-group $RESOURCEGROUP \
        --server $SERVERNAME \
        --name marketplaceDb \
        --sample-name AdventureWorksLT \
        --service-objective Basic
    ```

1. Posledním krokem bude získání připojovacího řetězce pro databázi.

    ```azurecli
    az sql db show-connection-string --client sqlcmd --name marketplaceDb --server $SERVERNAME | jq -r
    ```

    Výstup bude vypadat nějak takto. Mějte tento příkaz po ruce, protože ho později v tomto modulu budete potřebovat k připojení k databázi. Všimněte si, že `<username>` a `<password>` slouží jako zástupné symboly, které nahradíte přihlašovacími údaji `ADMINLOGIN` a `PASSWORD` zadanými v proměnných dříve.

    ```output
    sqlcmd -S tcp:server12345.database.windows.net,1433 -d marketplaceDb -U <username> -P <password> -N -l 30
    ```

## <a name="create-and-configure-a-linux-virtual-machine"></a>Vytvoření a konfigurace virtuálního počítače s Linuxem

Teď si vytvoříme virtuální počítač s Linuxem, který budeme používat v několika příkladech.

1. Spuštěním následujícího příkazu vytvořte virtuální počítač.

    ```azurecli
    az vm create \
      --resource-group $RESOURCEGROUP \
      --name appServer \
      --image UbuntuLTS \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

1. Po úspěšném vytvoření virtuálního počítače se k němu připojte přes SSH.

    ```azurecli
    ssh <X.X.X.X>
    ```

    > [!NOTE]
    > Je třeba poznamenat dvě věci. Za prvé: heslo nepotřebujeme, protože jsme při vytváření virtuálního počítače vygenerovali pár klíčů SSH. Za druhé: při počátečním připojení prostředí k virtuálnímu počítači se zobrazí se výzva týkající se pravosti hostitele. K tomu dochází, protože se připojujeme k IP adrese místo k názvu hostitele. Když odpovíte „ano“, IP adresa se uloží jako platný hostitel a připojení může pokračovat.

1. Na závěr na virtuálním počítači s Linuxem nainstalujeme mssql-tools, abychom se mohli připojit k databázi pomocí sqlcmd.

    ```bash
    echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
    echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
    source ~/.bashrc
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    sudo apt-get update
    sudo ACCEPT_EULA=Y apt-get install -y mssql-tools unixodbc-dev
    ```

    > [!NOTE]
    > U některých z těchto příkazů se bude posunovat velké množství textu, a proto nezapomeňte po posledním příkazu stisknout Enter, abyste měli jistotu, že se spustil.

Vytvořili jsme logický server Azure SQL Database, databázi na tomto logickém serveru a virtuální počítač s názvem _appServer_, který použijeme k simulaci připojení k síti z aplikačního serveru. Teď pojďme zjistit, jak správně databázi zabezpečit.