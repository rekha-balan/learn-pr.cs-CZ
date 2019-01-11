I když se přes síť můžeme připojit k databázi, neznamená to, že opravdu získáme přístup k samotným datům. Pomocí vícevrstvého přístupu lze zajistit, že přístup k datům budou mít jen ti uživatelé, kteří ho skutečně potřebují. Zde nachází své využití ověřování a autorizace.

## <a name="authentication"></a>Ověřování

Ověřování je proces kontroly identity. Tu může představovat uživatel, služba spuštěná v systému nebo samotný systém (například virtuální počítač). Během procesu ověřování se ujišťujeme, že osoba nebo systém jsou těmi, za koho se vydávají. Databáze SQL podporuje dva typy ověřování: ověřování SQL a ověřování služby Azure Active Directory.

### <a name="sql-authentication"></a>Ověřování SQL

Metoda ověřování SQL využívá uživatelské jméno a heslo. Uživatelské účty je možné vytvořit v hlavní databázi a udělit jim oprávnění ve všech databázích na serveru, nebo je lze vytvořit v samotné databázi (takové účty se nazývají uživatelé s omezením) a udělit jim přístup jen k této databázi. Když jste vytvářeli logický server databáze, zadali jste uživatelské jméno a heslo účtu „server admin“. Tyto přihlašovací údaje můžete použít k ověření a přihlášení k libovolné databázi na daném serveru jako vlastník databáze neboli „dbo“.

### <a name="azure-active-directory-authentication"></a>Ověřování služby Azure Active Directory

Tato metoda ověřování využívá identity spravované službou Azure Active Directory (AD) a je podporovaná u spravovaných a integrovaných domén. Ověřování služby Azure AD (integrované zabezpečení) používejte všude, kde to bude možné. Prostřednictvím ověřování služby Azure AD můžete centrálně spravovat identity uživatelů databází a další služby Microsoftu z jednoho umístění. Centrální správa ID poskytuje jediné místo pro správu uživatelů databáze a zjednodušuje správu oprávnění. Pokud chcete k ověřování použít Azure AD, musíte vytvořit jiného správce serveru, který se jmenuje „Azure AD admin“ a který smí spravovat uživatele a skupiny služby Azure AD. Tento správce může také provádět všechny operace jako běžný správce serveru.

## <a name="authorization"></a>Autorizace

Autorizace určuje, co může identita v rámci databáze Azure SQL Database dělat. Tyto možnosti jsou dané oprávněními udělenými přímo uživatelskému účtu a/nebo členstvím v databázových rolích. Databázové role se používají k seskupování oprávnění za účelem zjednodušení správy. Uživateli se přidáním do role udělí oprávnění, které taková role má. Součástí těchto oprávnění může být možnost přihlašování k databázi, čtení tabulky nebo třeba přidávání a odebírání sloupců v databázi. Doporučený postup je udělit uživatelům co nejmenší možná oprávnění. Proces udělení autorizace je pro uživatele SQL i Azure AD stejný.

Na níže uvedeném příkladu je účet správce serveru, pomocí kterého se připojujete, členem role db_owner. Tato role může s databází provádět všechny operace.

## <a name="authentication-and-authorization-in-practice"></a>Ověřování a autorizace v praxi

Pojďme si teď ukázat, jak nastavit uživatele a udělit mu přístup k databázi. V tomto případě používáme pro uživatele ověřování SQL, ale u ověřování pomocí služby Azure AD by se proces v zásadě nelišil.

### <a name="create-a-database-user"></a>Vytvoření uživatele databáze

Pojďme si vytvořit nového uživatele, kterému můžeme udělit přístup.

1. V Cloud Shellu na virtuálním počítači _appServer_ se znovu připojte k databázi jako uživatel s rolí správce (`ADMINUSER`).

    ```bash
    sqlcmd -S tcp:server<12345>.database.windows.net,1433 -d marketplaceDb -U <username> -P <password> -N -l 30
    ```

1. Spuštěním příkazu níže vytvořte nového uživatele. Bude se jednat o _uživatele s omezením_ a udělíte mu přístup jen k databázi _marketplace_. Upravte heslo dle svého uvážení – nezapomeňte si ho ale poznamenat, protože ho budeme potřebovat v dalším kroku.

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    GO
    ```

Pomocí těchto přihlašovacích údajů se uživatel může ověřit v databázi, nemá ale přístup k datům. Pojďme mu tedy přístup udělit.

### <a name="grant-permissions-to-a-user"></a>Udělení oprávnění uživateli

Uživateli teď vytvoříme členství v rolích `db_datareader` a `db_datawriter`, čímž mu pro databázi udělíme přístup ke čtení, respektive k zápisu. Současně ale budeme chtít, aby neměl přístup k tabulce s adresami.

1. Zůstaňte připojení k `sqlcmd` na virtuálním počítači _appServer_ a spuštěním následujícího příkazu T-SQL udělte nově vytvořenému uživateli role `db_datareader` a `db_datawriter`.

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    GO
    ```

1. Možnosti přístupu můžeme omezit ještě více. Pomocí operátoru DENY můžeme uživateli odepřít přístup k ostatním elementům v databázi. Spuštěním následujícího příkazu T-SQL odepřete uživateli _ApplicationUser_ možnost vybírat data z tabulky `SalesLT.Address`.

    ```sql
    DENY SELECT ON SalesLT.Address TO ApplicationUser;
    GO
    ```

Teď se přihlaste jako váš uživatel a podívejte se, jak všechno funguje.

1. Ve výzvě T-SQL ukončete relaci zadáním `exit`.

1. Znovu se přihlaste do databáze, ale tentokrát jako uživatel, kterého jste vytvořili.

    ```bash
    sqlcmd -S tcp:server<12345>.database.windows.net,1433 -d marketplaceDb -U ApplicationUser -P <password> -N -l 30
    ```

1. Spusťte následující dotaz. Ten načítá data z tabulky, k níž má uživatel povolen přístup.

    ```sql
    SELECT FirstName, LastName, EmailAddress, Phone FROM SalesLT.Customer;
    GO
    ```

    Měl by se zobrazit seznam zákazníků.

    ```output
    FirstName      LastName       EmailAddress                    Phone
    -------------- -------------- ------------------------------- ------------
    Orlando        Gee            orlando0@adventure-works.com    245-555-0173
    Keith          Harris         keith0@adventure-works.com      170-555-0127
    Donna          Carreras       donna0@adventure-works.com      279-555-0130
    Janet          Gates          janet1@adventure-works.com      710-555-0173
    ...
    ```

1. Teď se podívejme, co se stane při pokusu o dotazování na tabulku, k níž nemáme přístup.

    ```sql
    SELECT * FROM SalesLT.Address;
    GO
    ```

    Měla by se zobrazit zpráva s informací, že k této tabulce nemáte přístup.

    ```output
    Msg 229, Level 14, State 5, Server server-22942, Line 1
    The SELECT permission was denied on the object 'Address', database 'marketplace', schema 'SalesLT'.
    ```

Jak vidíte, i když jsme pro databázi udělili přístup ke čtení a zápisu, můžeme výslovným odepřením přístupu k tabulkám ještě více zabezpečit přístup uživatelů k datům. Pokud máte více uživatelů se stejným přístupem, můžete vytvořit vlastní role s vhodnými oprávněními a zjednodušit si tak správu.

Je důležité databázi správně zabezpečit a udělovat přístup jen tam, kde je to nutné. Azure SQL Database vám dává možnost plné kontroly nad ověřováním a autorizací identit za účelem přístupu k datům ve vaší databázi.