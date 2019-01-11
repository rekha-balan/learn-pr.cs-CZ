## <a name="allow-azure-service-access"></a>Povolení přístupu ke službám Azure

Pokud se chcete připojit k vašemu serveru pomocí PowerShellu a nástroje `psql`, budete muset před tím, než začneme, povolit přístup ke službám Azure. Připomínáme, že přístup můžete povolit dvěma způsoby.

První možností je povolit možnost **Povolit přístup ke službám Azure**. Po povolení přístupu se vytvoří pravidlo brány firewall, i když toto pravidlo není uvedené v seznamu vlastních pravidel, která vytvoříte.

Druhou možností je vytvořit pravidlo brány firewall, které povolí přístup všem IP adresám. Toto pravidlo bude obsahovat IP adresu klienta s PowerShellem, ze kterého budete spouštět příkazy `psql`.

Budete také muset zakázat možnost **Vynutit připojení SSL**.

### <a name="create-a-firewall-rule"></a>Vytvoření pravidla brány firewall

1. Pomocí stejného účtu, kterým jste aktivovali sandbox, se přihlaste k webu [portál Microsoft Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Přejděte k prostředku serveru, pro který chcete vytvořit pravidlo brány firewall.

1. Vyberte možnost **Zabezpečení připojení**, aby se napravo otevřelo okno zabezpečení připojení.

    ![Snímek obrazovky portálu Microsoft Azure zobrazující část zabezpečení připojení okna prostředků databáze PostgreSQL](../media/6-db-security-settings.png)

Připomínáme, že chcete povolit přístup klientům PowerShellu s nástrojem `psql`.

Můžete provést jednu z těchto akcí:

- Nastavit možnost **Povolit přístup ke službám Azure** na **ZAPNUTO**.
- Nastavit možnost **Vynutit připojení SSL** na **ZAKÁZÁNO**.
- Kliknutím na tlačítko **Uložit** uložte provedené změny.

Případně můžete přidat pravidlo brány firewall, které povolí přístup všem IP adresám. Použijte následující hodnoty:

- Název pravidla: `AllowAll`
- Počáteční IP adresa: `0.0.0.0`
- Koncová IP adresa: `255.255.255.255`
- Nastavit možnost **Vynutit připojení SSL** na **ZAKÁZÁNO**.
- Kliknutím na tlačítko **Uložit** uložte provedené změny.

> [!Warning]
> Vytvořením tohoto pravidla brány firewall se povolí libovolné IP adrese na internetu, aby se zkusila připojit k serveru. V produkčních prostředích budete chtíti povolit přístup pouze konkrétním IP adresám.

### <a name="connect-to-the-database-with-psql"></a>Připojit se k databázi pomocí psql

1. Ve službě Azure Cloud Shell na pravé straně připojte `psql` k vašemu serveru pomocí následujícího příkazu. Nezapomeňte nahradit název serveru a jméno správce.

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
    ```

    Použijte hodnoty, které jste zvolili pro `server-name` a `admin-user`.

1. **postgres** je výchozí databáze správy, která se vytvoří pro každý server PostgreSQL. Zobrazí se výzva k zadání hesla, které jste zadali při vytváření serveru.

1. Po úspěšném připojení spusťte příkaz `\l`, který vypíše všechny databáze. Tento příkaz ve výsledku vrátí dvě nebo více výchozích databází.

1. Stisknutím klávesy `q` opustíte seznam.

1. Následujícím příkazem SQL vytvořte novou databázi:

    ```sql
    CREATE DATABASE "Adventureworks";
    ```

1. Spuštění příkazu psql `\c Adventureworks` se připojte k databázi.

1. Přidejte do databáze nějaká data. Použijte k tomu následující příkazy SQL, které přidají data do dvou tabulek:

    ```sql
    CREATE TABLE PEOPLE(NAME TEXT NOT NULL, AGE INT NOT NULL);
    INSERT INTO PEOPLE(NAME, AGE) VALUES ('Bob', 35);
    INSERT INTO PEOPLE(NAME, AGE) VALUES ('Sarah', 28);
    CREATE TABLE LOCATIONS(CITY TEXT NOT NULL, STATE TEXT NOT NULL);
    INSERT INTO LOCATIONS(CITY, STATE) VALUES ('New York', 'NY');
    INSERT INTO LOCATIONS(CITY, STATE) VALUES ('Flint', 'MI');
    ```

1. Pomocí následujících příkazů SQL načtěte data, která jste přidali:

    ```sql
    SELECT * FROM PEOPLE;
    SELECT * FROM LOCATIONS;
    ```

1. Můžete vyzkoušet i další příkazy psql.
    - `-?` pro získání nápovědy.
    - `\dt` pro výpis tabulek.

1. Jakmile skončíte s prováděním operací psql na vašem serveru, provedením příkazu `\q` ukončete psql.
