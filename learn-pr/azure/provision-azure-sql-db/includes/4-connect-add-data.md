Než k vaší aplikaci připojíte databázi, měli byste zkontrolovat, jestli se k ní můžete připojit a můžete také přidat základní tabulku a pracovat s ukázkovými daty.

O infrastrukturu, aktualizace softwaru a opravy pro vaši databázi Azure SQL Database se staráme my. Jinak můžete s databází Azure SQL zacházet stejně jako s jakoukoli jinou instalací SQL Serveru. Ke správě Azure SQL Database můžete používat třeba Visual Studio, SQL Server Management Studio, Azure Data Studio a další nástroje.

Jak budete k databázi přistupovat a jak ji připojíte k aplikacím, je na vás. Abyste ale získali s prací s databází nějaké zkušenosti, tady se dozvíte, jak se k ní připojit přímo z portálu, jak vytvořit tabulku a spustit pár základních operací CRUD. Naučíte se:

- Co je Cloud Shell a jak se k němu dostat z portálu.
- Jak pomocí Azure CLI získat informace o vaší databázi, včetně připojovacích řetězců.
- Jak se k databázi připojit pomocí `sqlcmd`.
- Jak databázi inicializovat základní tabulkou a nějakými ukázkovými daty.

## <a name="what-is-azure-cloud-shell"></a>Co je Azure Cloud Shell?

Azure Cloud Shell je prostředí založené na prohlížeči, které je určené ke správě a vývoji prostředků Azure. Cloud Shell si můžete představit jako interaktivní konzolu, která běží v cloudu.

Pro zajímavost – Cloud Shell běží na Linuxu. Ale podle toho, jestli dáváte přednost prostředí Linuxu nebo Windows, můžete si vybrat ze dvou prostředí: Bash nebo PowerShell.

Cloud Shell je přístupný odkudkoli. Ke Cloud Shellu se dostanete nejen z portálu, ale také z [shell.azure.com](https://shell.azure.com/), mobilní aplikace Azure nebo z Visual Studio Code. Panel na pravé straně je terminál Cloud Shellu, který můžete používat během tohoto cvičení.

Cloud Shell obsahuje oblíbené nástroje a textové editory. Tady je stručný pohled na `az`, `jq` a `sqlcmd` – tři nástroje, které použijete k popisovanému aktuálnímu úkolu.

- `az` se také označuje jako Azure CLI. Je to rozhraní příkazového řádku pro práci s prostředky Azure. Použijete ho k získání informací o databázi, včetně připojovacího řetězce.
- `jq` je analyzátor příkazového řádku ve formátu JSON. Výstup z příkazů `az` předáte tomuto nástroji k extrakci důležitých polí z výstupu JSON.
- `sqlcmd` umožňuje spouštět příkazy na SQL Serveru. `sqlcmd` použijete k vytvoření interaktivní relace s databází Azure SQL Database.

## <a name="get-information-about-your-azure-sql-database"></a>Získání informací o Azure SQL Database

Než se k databázi připojíte, je vhodné ověřit, jestli existuje a je online.

Tady pomocí nástroje `az` vypíšete svoje databáze a zobrazíte některé informace o databázi **Logistics** (Logistika), včetně její maximální velikosti a stavu.

1. Příkazy `az`, které spustíte, vyžadují název skupiny prostředků a název vašeho logického serveru Azure SQL. Abyste si ušetřili psaní, spusťte tento příkaz `azure configure` a určete tyto názvy jako výchozí hodnoty.
    Název `<server-name>` nahraďte názvem vašeho logického serveru Azure SQL. Všimněte si, že v závislosti na okně, ve kterém na portálu jste, se název může zobrazovat jako plně kvalifikovaný název domény (servername.database.windows.net), ale vy potřebujete jenom logický název bez přípony .database.windows.net.

    ```azurecli
    az configure --defaults group=<rgn>[sandbox resource group name]</rgn> sql-server=<server-name>
    ```

1. Spuštěním příkazu `az sql db list` vypište všechny databáze na vašem logickém serveru Azure SQL.

    ```azurecli
    az sql db list
    ```
    Jako výstup uvidíte velký blok ve formátu JSON.

1. Protože chceme jenom názvy databází, spusťte tento příkaz znovu, ale tentokrát přesměrujte výstup do `jq`, aby se vypsala jenom pole názvů.
   
     ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    
    Měli byste vidět tento výstup.
    
    ```json
    [
      {
        "name": "Logistics"
      },
      {
        "name": "master"
      }
    ]
    ```

    **Logistics** (Logistika) je vaše databáze. Hlavní databáze **master** (jako SQL Server) obsahuje serverová metadata jako přihlašovací účty a nastavení systémové konfigurace.

1. Spusťte tento příkaz `az sql db show`, abyste získali podrobnosti o databázi **Logistics**.

    ```azurecli
    az sql db show --name Logistics
    ```

    Jako předtím uvidíte jako výstup velký blok ve formátu JSON.

1. Spusťte tento příkaz znovu, ale tentokrát přesměrujte výstup do `jq`, abyste výstup omezili jenom na název, maximální velikost a stav databáze **Logistics**.

    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```

    Uvidíte, že databáze je online a může pojmout přibližně 2 GB dat.

    ```json
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a>Připojení k databázi

Teď když už o vaší databázi něco víte, připojíme se k ní pomocí nástroje `sqlcmd`, vytvoříme tabulku obsahující informace o řidičích dopravy a provedeme pár základních operací CRUD.

CRUD označuje operace **create** (vytvoření), **read** (čtení), **update** (aktualizace) a **delete** (odstranění). Tyto pojmy označují operace, které provedete s daty tabulky, a jedná se o čtyři základní operace, které potřebujete pro svoji aplikaci. Teď je vhodná doba, abyste si ověřili, že umíte provést každou z nich.

1. Spusťte tento příkaz `az sql db show-connection-string`, abyste získali připojovací řetězec k databázi **Logistics** ve formátu, který umí nástroj `sqlcmd` použít.

    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```

    Výstup bude vypadat nějak takto.

    ```output
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```

1. Spuštěním příkazu `sqlcmd` z výstupu předchozího kroku vytvořte interaktivní relaci. Odeberte uvozovky a nahraďte `<username>` a `<password>` uživatelským jménem a heslem, které jste zadali při vytváření databáze. Tady je příklad.

    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```

    > [!TIP]
    > Dejte heslo do uvozovek, aby znak & a jiné speciální znaky, které jsou součástí hesla, nebyly interpretované jako instrukce pro zpracování.

1. V relaci `sqlcmd` vytvořte tabulku s názvem `Drivers` (Řidiči).

    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255));
    GO
    ```

    Tabulka obsahuje čtyři sloupce: jedinečný identifikátor, příjmení a jméno řidiče a město, odkud řidič pochází.

    > [!NOTE]
    > Jazyk, který tady vidíte, je Transact-SQL neboli T-SQL.

1. Ověřte, že tabulka `Drivers` existuje.

    ```sql
    SELECT name FROM sys.tables;
    GO
    ```

    Měli byste vidět tento výstup.

    ```output
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers

    (1 rows affected)
    ```

1. Spusťte tento příkaz T-SQL `INSERT`, abyste do tabulky přidali ukázkový řádek. Je to operace **create** (vytvoření).

    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Zirne', 'Laura', 'Springfield');
    GO
    ```

    Pokud byla operace úspěšná, uvidíte toto.

    ```output
    (1 rows affected)
    ```

1. Spusťte tento příkaz T-SQL `SELECT`, abyste vypsali sloupce `DriverID` a `OriginCity` ze všech řádků v tabulce. Je to operace **read** (čtení).

    ```sql
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    Uvidíte jeden výsledek s `DriverID` a `OriginCity` z řádku, který jste vytvořili v předchozím kroku.

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Springfield

    (1 rows affected)
    ```

1. Spusťte tento příkaz T-SQL `UPDATE`, abyste u řidiče, který má v poli `DriverID` hodnotu 123, změnili město původu z hodnoty „Springfield“ na „Boston“. Je to operace **update** (aktualizace).

    ```sql
    UPDATE Drivers SET OriginCity='Boston' WHERE DriverID=123;
    GO
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    Měl by se zobrazit následující výstup. Všimněte si, jak `OriginCity` odráží aktualizaci na Boston.

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Boston

    (1 rows affected)
    ```

1. Spusťte tento příkaz T-SQL `DELETE`, abyste záznam odstranili. Je to operace **delete** (odstranění).

    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```

    ```output
    (1 rows affected)
    ```

1. Spusťte tento příkaz T-SQL `SELECT`, abyste ověřili, že je tabulka `Drivers` prázdná.

    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```

    Uvidíte, že tabulka neobsahuje žádné řádky.

    ```output
    -----------
              0

    (1 rows affected)
    ```

Když teď rozumíte tomu, jak se pracuje s databází Azure SQL Database pomocí Cloud Shellu, můžete získat připojovací řetězec pro svůj oblíbený SQL nástroj pro správu &ndash;, ať už to je SQL Server Management Studio, Visual Studio, nebo něco jiného.

Cloud Shell umožňuje snadno přistupovat k prostředkům Azure a pracovat s nimi. Protože je Cloud Shell založený na prohlížeči, máte k němu přístup ve Windows, macOS i Linuxu – v podstatě v libovolném systému s webovým prohlížečem.

Získali jste některé praktické zkušenosti se získáváním informací o databázi Azure SQL Database pomocí příkazů Azure CLI. Jako bonus jste si procvičili dovednosti s T-SQL.

Nakonec to uzavřeme a podíváme se, jak databázi zlikvidovat.
