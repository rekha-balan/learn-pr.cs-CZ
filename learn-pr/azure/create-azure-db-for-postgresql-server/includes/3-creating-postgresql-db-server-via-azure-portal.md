Předpokládejme, že momentálně používáte místní relační databázi PostgreSQL s flexibilními typy dat a podporou geoprostorových funkcí. Vaše společnost se chystá expandovat, což vyžaduje škálování databáze. Vaším úkolem je najít optimální nabídku databáze hostované v cloudu jako alternativu k investici do dalšího hardwaru. Rozhodli jste se použít server služby Azure Database for PostgreSQL.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Co je server Azure Database for PostgreSQL?

Server PostgreSQL je centrální bod pro správu jedné nebo více databází. Služba PostgreSQL v Azure je spravovaný prostředek, který poskytuje záruku výkonu a přístup a funkce na úrovni serveru.

Server **Azure Database for PostgreSQL** je _prostředek_ nadřazený databázi. _Prostředek_ je spravovatelná položka, která je k dispozici prostřednictvím Azure. Vytvoření tohoto prostředku vám umožní konfigurovat instanci vašeho serveru.

### <a name="what-is-an-azure-database-for-postgresql-server-resource"></a>Co je prostředek serveru Azure Database for PostgreSQL?

Prostředek serveru Azure Database for PostgreSQL je kontejner, který trvale a zásadně ovlivňuje server a databáze. Pokud dojde k odstranění prostředku serveru, odstraní se také všechny databáze. Mějte na paměti, že veškeré prostředky náležící do nadřazeného objektu jsou hostovány ve stejné oblasti.

Název prostředku serveru se používá k definici názvu koncového bodu serveru. Pokud je název prostředku například **mypgsqlserver**, potom název serveru bude **mypgsqlserver.postgres.database.azure.com**.

Prostředek serveru dále poskytuje __obor připojení__ pro zásady správy, které se používají pro jeho databázi. Jedná se například o přihlašování, bránu firewall, uživatele, role a konfiguraci.

Stejně jako open source verze PostgreSQL je tento server dostupný v několika verzích a umožňuje instalaci rozšíření. Vy si vyberete verzi serveru, která se má nainstalovat.

> [!NOTE]
> Rozšíření umožňují sdružení více objektů SQL do jednoho balíčku, který se může načíst nebo odebrat jedním příkazem. Příkladem rozšíření je `chkpass`. Toto rozšíření poskytuje datový typ pro automaticky šifrovaná hesla.

## <a name="pricing-tiers"></a>Cenové úrovně

Azure Database for PostgreSQL nabízí možnost výběru cenových úrovní na základě parametrů, jako jsou výpočetní výkon a úložiště. Další podrobnosti najdete v tématu věnovaném [cenovým úrovním Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers?azure-portal=true).

## <a name="steps-to-create-an-azure-database-for-postgresql-server"></a>Postup vytvoření serveru Azure Database for PostgreSQL

Server Azure Database for PostgreSQL se obvykle vytváří pomocí webu Azure Portal. Podívejme se na kroky, které byste provedli. Toto není cvičení. Jde spíše o seznámení se s příslušnými kroky, pokud se rozhodnete používat portál Azure Portal.

Nejdříve se přihlásíte k webu Azure Portal a potom kliknete na **Vytvořit prostředek**.

Vyberete **Databáze** a **Azure Database for PostgreSQL**. K vyhledání této kategorie také můžete použít funkci **vyhledávání**.

Portál zobrazí obrazovku konfigurace serveru PostgreSQL, která se také označuje jako okno. Tady provedete své volby.

Každé z položek v okně musíte přiřadit hodnotu. Pojďme se tedy na ně podívat.

### <a name="server-name"></a>Název serveru

Dříve jsme se zmínili, že vytvoříte prostředek serveru. Název serveru je položka, která tento prostředek specifikuje. Proto musíte pro server zvolit jedinečný název. Tento název může obsahovat jenom malá písmena, číslice a spojovníky.

Řekněme, že budete chtít server pojmenovat _Adventure Works Tracking_. Potom byste měli název nastavit jako `adventure-works-tracking`. Pokud se pokusíte vytvořit server s názvem, který už existuje, zobrazí se vám chyba.

### <a name="subscription"></a>Předplatné

Pole předplatného se používá pro fakturaci. Pokud máte k dispozici více předplatných, vyberte jedno konkrétní předplatné.

### <a name="resource-group"></a>Skupina prostředků

Skupinu prostředků použijete ke správě všech prostředků souvisejících se serverem. Můžete vytvořit novou skupinu prostředků nebo znovu použít již existující skupinu prostředků.

### <a name="source"></a>Zdroj

Server můžete buď vytvořit úplně od začátku výběrem výchozí možnosti _Prázdné_, nebo z existující zálohy. Možnost **zálohování** vám poskytne příležitost obnovit geografickou zálohu existujícího serveru Azure Database for PostgreSQL.

### <a name="server-admin-login-name"></a>Přihlašovací jméno správce serveru

Vytvořte uživatele správce serveru. Zvolte přihlašovací jméno, které se použije jako přihlašovací jméno správce nového serveru. Přihlašovací jméno správce nemůže být azure_superuser, azure_pg_admin, admin, administrator, root, guest ani public. Nemůže začínat na pg_. Toto jméno si zapamatujte nebo poznamenejte pro pozdější použití.

### <a name="password"></a>Heslo

Zvolte heslo pro výše uvedené přihlašovací jméno správce. Heslo musí obsahovat znaky ze tří následujících kategorií:

- Velká písmena anglické abecedy
- Malá písmena anglické abecedy
- Číslice (0 – 9)
- Jiné než alfanumerické znaky (!, $, #, % atd.)

Toto heslo si zapamatujte nebo poznamenejte pro pozdější použití.

### <a name="confirm-password"></a>Potvrzení hesla

Pro potvrzení heslo znovu zadejte.

### <a name="location"></a>Umístění

Možností umístění lze určit, kde se server fyzicky vytvoří. Vyberte zeměpisné umístění, které je k vám nejblíže. V reálném světě by to mělo být umístění, které je nejblíže k většině vašich uživatelů.

### <a name="version"></a>Verze

Můžete určit verzi PostgreSQL, která se má použít. Microsoft se zaměřuje na podporu aktuální verze a dvou předchozích verzí PostgreSQL. Vyberte verzi, která odpovídá vašemu produkčnímu prostředí.

> [!NOTE]
> Další informace najdete v následujících zdrojích:
> - [Podporované verze databáze PostgreSQL](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions?azure-portal=true)
> - [Zásady správy verzí](https://www.postgresql.org/support/versioning/?azure-portal=true)

### <a name="pricing-tier"></a>Cenová úroveň

Vyberte takovou cenovou úroveň, která bude podporovat úlohy vašeho serveru. Nezapomeňte, že můžete vybírat z různých úrovní. Jak jste mohli vidět, každá z těchto úrovní vám umožní nakonfigurovat možnosti výpočetního výkonu a úložiště. 

Teď už zbývá jen zkontrolovat vámi zadané hodnoty, poznamenat si položky, které můžete potřebovat později, a kliknutím na tlačítko **Vytvořit** server vytvořit.

Nasazení serveru trvá několik minut. Jakmile se nasazení dokončí, zobrazí se vám oznámení. Z tohoto oznámení můžete přejít na nově vytvořený server.

