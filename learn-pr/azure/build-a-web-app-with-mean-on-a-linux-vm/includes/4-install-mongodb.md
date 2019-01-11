Řada aplikací vyžaduje databázi. Teď nainstalujete databázi MongoDB, která v názvu zásobníku MEAN zastupuje písmeno „M“. Toto oblíbené databázové řešení NoSQL je zdarma a typu Open Source. Na rozdíl od relačních databází, jako je SQL Server nebo MySQL, nevyžaduje databáze NoSQL předem definovanou strukturu dat.

MongoDB uchovává data v dokumentech podobných formátu JSON, které nevyžadují pevnou strukturu dat. S MongoDB pracujete pomocí dotazů a příkazů, které posíláte ve formátu JavaScript Object Notation neboli JSON.

## <a name="what-mongodb-editions-are-available"></a>Jaké edice MongoDB jsou k dispozici?

MongoDB je k dispozici ve dvou edicích:

- MongoDB Community Server
- MongoDB Enterprise Server

Tady si nainstalujete MongoDB Community Server. Později použijete MongoDB k ukládání informací o knihách.

## <a name="how-do-i-install-mongodb"></a>Jak nainstalovat MongoDB

MongoDB můžete nainstalovat na systémy Linux, macOS a Windows. Pro účely tohoto kurzu si MongoDB nainstalujte na Ubuntu. Použijte k tomu správce balíčků `apt` systému Ubuntu.

Postup instalace se liší podle toho, jaký máte operační systém. Pokud neznáte Ubuntu, můžete i tak číst dál, abyste si udělali představu, jak to celé funguje.

Později se můžete [podívat do instalační příručky](https://docs.mongodb.com/manual/administration/install-community?azure-portal=true), kde najdete další informace.

## <a name="install-mongodb"></a>Instalace MongoDB

Tady nainstalujete několika příkazy MongoDB. Součástí tohoto postupu je i registrace úložiště MongoDB, aby správce balíčků `apt` dokázal balíček najít.

> [!IMPORTANT]
> Tady budete pracovat s připojením SSH k virtuálnímu počítači s Ubuntu, který jste vytvořili v předchozí lekci.

1. Importujte šifrovací klíč úložiště MongoDB. Umožníte tak správci balíčků ověřit, že instalované balíčky jsou od společnosti MongoDB Inc.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    > [!NOTE]
    > Část `sudo` znamená, že chceme ke spuštění příkazu použít oprávnění správce.

1. Zaregistrujte úložiště MongoDB, aby správce balíčků našel podobné balíčky.

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. Aktualizujte databázi balíčků `apt`, abyste získali nejnovější informace o balíčku.

    ```bash
    sudo apt-get update
    ```

1. Nainstalujte balíček MongoDB.

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. Spusťte službu MongoDB.

    ```bash
    sudo service mongod start
    ```

1. Ověřte instalaci příkazem `mongod --version`.

    ```bash
    mongod --version
    ```

Pro další část nechte připojení SSH otevřené.