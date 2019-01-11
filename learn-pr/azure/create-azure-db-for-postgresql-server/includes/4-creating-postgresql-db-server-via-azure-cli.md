Účelem této lekce je seznámit vás s kroky, které provedete v další lekci, až budete provádět cvičení pro vytvoření serveru. Pokud byste někde v další lekci nevěděli jak dál, vraťte se do této části.

## <a name="scenario"></a>Scénář

Předpokládejme, že používáte místní databázi PostgreSQL. Vaše společnost se teď chystá rozšířit funkce podpory zařízení, dostupnosti, sledování dat a zpracování přesunutím serveru do Azure. Budete zjišťovat, kolik úsilí bude potřeba, aby se vytváření serveru Azure Database for PostgreSQL automatizovalo.

Vytvoření jednoho serveru Azure Database for PostgreSQL pomocí webu Azure Portal je snadné. Vytváření více než jedné databáze a provozování průběžné údržby jenom pomocí portálu může být únavné. Když budete chtít automatizovat úkoly správy, použijete k vytváření skriptů rozhraní příkazového řádku Azure (Azure CLI).

Vytváření téměř libovolného prostředku v rámci Microsoft Azure je možné automatizovat pomocí rozhraní příkazového řádku Azure. V této lekci se dozvíte, jak automatizovat správu serverů Azure Database for PostgreSQL pomocí rozhraní příkazového řádku Azure.

## <a name="what-is-the-azure-cli"></a>Co je Azure CLI?

[Rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure?azure-portal=true) (Azure CLI) je prostředí příkazového řádku Microsoftu pro různé platformy určené ke správě prostředků Azure. Rozhraní příkazového řádku Azure můžete spustit v prohlížeči pomocí služby Azure Cloud Shell nebo si ho můžete nainstalovat místně na macOS, Linux nebo Windows. Rozhraní příkazového řádku Azure se spouští z místního příkazového řádku pomocí prostředí bash nebo PowerShell. Místně spuštěné rozhraní příkazového řádku Azure vyžaduje další nastavení. Ke spouštění příkazů rozhraní příkazového řádku Azure budeme používat Azure Cloud Shell.

## <a name="what-is-azure-cloud-shell"></a>Co je Azure Cloud Shell?

Azure Cloud Shell je prostředí fungující v prohlížeči, které je hostované v cloudu a umožňuje připojení k Azure pomocí ověřené relace. Spuštěním příkazů rozhraní příkazového řádku Azure můžete automatizovat správu serveru Azure Database for PostgreSQL. Ve službě Cloud Shell jsou předinstalované obvyklé nástroje rozhraní příkazového řádku Azure a jsou nakonfigurované pro použití s vaším účtem.

> [!NOTE]
> Cloud Shell vyžaduje prostředek úložiště Azure, aby se zachovaly všechny soubory, které při práci v Cloud Shellu vytvoříte. Při prvním spuštění Cloud Shell zobrazí výzvu k vytvoření skupiny prostředků, účtu úložiště a sdílené složky Azure Files pod vaším účtem. Jde o jednorázový krok a automaticky se přiřadí ke všem budoucím relacím Cloud Shellu.

## <a name="create-an-azure-database-for-postgresql-server-using-the-azure-cli"></a>Vytvoření serveru Azure Database for PostgreSQL pomocí Azure CLI

K vytvoření serveru Azure Database for PostgreSQL pomocí rozhraní příkazového řádku Azure (Azure CLI) použijete terminál Azure Cloud Shell napravo.

Nápověda k použití příkazů pro vytvoření serveru pomocí Azure CLI zobrazující všechny dostupné parametry vypadá jako v následujícím příkladu:

```azurecli
az postgres server create [-h] [--verbose] [--debug]
                            [--output {json,jsonc,table,tsv}]
                            [--query JMESPATH]
                            --resource-group RESOURCE_GROUP_NAME --name SERVER_NAME
                            --sku-name SKU_NAME [--location LOCATION]
                            --admin-user ADMINISTRATOR_LOGIN
                            [--admin-password ADMINISTRATOR_LOGIN_PASSWORD]
                            [--backup-retention BACKUP_RETENTION]
                            [--geo-redundant-backup GEO_REDUNDANT_BACKUP]
                            [--ssl-enforcement {Enabled,Disabled}]
                            [--storage-size STORAGE_MB]
                            [--tags [TAGS [TAGS ...]]]
                            [--version VERSION]
                            [--subscription _SUBSCRIPTION]

```

Volitelné parametry jsou uzavřené v závorkách. Podívejme se na některé běžné parametry podívat.

### <a name="parameters"></a>Parametry

Parametr `--resource-group <resource_group_name>` určuje skupinu prostředků, ve které se má vytvořit server.

Pro přihlášení k serveru a jeho databázím se vyžadují `admin-user` a `admin-password` serveru, které určíte. Tyto informace si zapamatujte nebo poznamenejte, ať je víte později při práci s novým serverem.

Parametr `--sku-name` slouží k zadání části cenové úrovně, v tomto případě výpočetních prostředků. Hodnota má formát podle konvence `{pricing tier}_{compute generation}_{vCores}`.

Příklady:

- `--sku-name B_Gen4_4` se mapuje na úroveň Basic 4. generace se 4 virtuálními jádry.
- `--sku-name GP_Gen5_32` se mapuje na úroveň pro obecné účely 5. generace se 32 virtuálními jádry.
- `--sku-name MO_Gen5_2` se mapuje na úroveň optimalizovanou pro paměť 5. generace se dvěma virtuálními jádry.

Vzpomeňte si, že v lekci o vytváření serveru pomocí portálu jsme probírali tři cenové úrovně.

Předpokládejme, že chcete použít výpočetní prostředky úrovně Basic 5. generace s jedním virtuálním jádrem. Pak zadáte parametr jako `--sku-name B_Gen5_1`.

Parametr `--storage-size` slouží k zadání části cenové úrovně. Pokud hodnotu nezadáte, použije se výchozích 5120 MB. Platné velikosti úložiště jsou v rozsahu od 5120 MB v přírůstcích po 1024 MB až do 1 048 576 MB.

Parametr `--backup-retention` se používá, když je třeba zadat období uchování záloh v dnech. Pokud hodnotu nezadáte, použije se výchozích sedm dnů.

Parametr `--version` se používá k určení hlavní verze PostgreSQL, kterou byste chtěli použít.

Seznámili jste se s postupem, jak vytvořit server Azure Database for PostgreSQL pomocí Azure CLI. V další lekci vytvoříte server Azure Database for PostgreSQL pomocí Azure CLI.