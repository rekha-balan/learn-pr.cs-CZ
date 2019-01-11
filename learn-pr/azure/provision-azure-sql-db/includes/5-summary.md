Teď je služba Azure SQL Database zprovozněná, takže ji můžete připojit k vašemu oblíbenému nástroji pro správu SQL Serveru a naplnit ji skutečnými daty.

Původně jste zvažovali, jestli vaši databázi provozovat v místním prostředí nebo v cloudu. Při použití služby Azure SQL Database stačí nakonfigurovat několik základních možností a máte plně funkční databázi SQL, kterou můžete připojit k vašim aplikacím.

Nemusíte se vůbec zabývat opravami infrastruktury nebo softwaru. Teď se můžete bez obav více soustředit na zprovoznění prototypu vaší aplikace pro dopravní logistiku a méně na správu databáze. A váš prototyp nebude ukázka určená k zahození. Azure SQL Database poskytuje zabezpečení a výkon na provozní úrovni.

Mějte na paměti, že každý logický server Azure SQL obsahuje jednu nebo více databází. Azure SQL Database nabízí dva cenové modely DTU a vCore, které vám pomůžou vyvážit náklady a výkon napříč všemi vašimi databázemi.

Pokud právě začínáte nebo chcete nakoupit jednoduché a předem nakonfigurované prostředky, zvolte model DTU. Pokud chcete mít větší kontrolu nad tím, jaké výpočetní prostředky a prostředky úložiště vytvoříte a za co platíte, zvolte model VCore.

Zahájení práce s databázemi vám usnadní Cloud Shell. Ze služby Cloud Shell máte přístup k rozhraní Azure CLI, které umožňuje získat informace o vašich prostředcích Azure. Cloud Shell poskytuje také řadu dalších běžných nástrojů, které vám pomůžou začít s novou databází hned pracovat, například `sqlcmd`.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>Další zdroje informací

Dokumentace obsahuje mnoho dalších informací včetně kurzů a ukázek. Zde je několik odkazů k tomu, čím jsme se tady zabývali:

- [Dokumentace ke službě Azure SQL Database](https://docs.microsoft.com/azure/sql-database/)
- [Nákupní modely a prostředky Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)
- [Logické servery Azure SQL Database a jejich správa](https://docs.microsoft.com/azure/sql-database/sql-database-logical-servers)
- [Pravidla brány firewall služeb Azure SQL Database a SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)

Další informace o službě Cloud Shell najdete v tématu [Přehled služby Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

Další informace o nástroji `sqlcmd` najdete v tématu [Nástroj sqlcmd](https://docs.microsoft.com/sql/tools/sqlcmd-utility?view=sql-server-2017).
