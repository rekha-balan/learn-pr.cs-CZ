Zálohování představuje poslední a nejúčinnější obrannou linii proti trvalé ztrátě dat. Efektivní strategie zálohování vyžaduje více než pouhé vytváření kopií dat. Je potřeba vzít v úvahu architekturu a infrastrukturu dat vaší aplikace. Vaše aplikace může spravovat různé druhy dat, která jsou různě důležitá a nacházejí se v různých systémech souborů, databázích a dalších úložných službách v cloudu i v místním prostředí. Použitím správných služeb a produktů zjednodušíte proces zálohování a zkrátíte dobu potřebnou k případnému obnovení zálohy.

## <a name="establish-backup-and-restoration-requirements"></a>Stanovení požadavků na zálohování a obnovení

Stejně jako u strategie zotavení po havárii vycházejí požadavky na zálohování z analýzy nákladů a výhod. Analýza dat vaší aplikace by se měla řídit relativní důležitostí různých kategorií dat, která aplikace spravuje, a také externími požadavky, jako jsou zákonné požadavky na uchovávání dat.

Pokud chcete stanovit požadavky na zálohování vaší aplikace, rozdělte její data do skupin podle následujících požadavků:

* Kolik dat tohoto typu si můžete dovolit ztratit (měřeno dobou trvání)
* Maximální doba potřebná k obnovení dat tohoto typu
* Požadavky na uchovávání záloh – jak dlouho mají být zálohy k dispozici a jakou mají mít frekvenci

Tyto faktory berou v úvahu také koncepce RPO (cíl bodu obnovení) a RTO (plánovaná doba obnovení). Doba přijatelné ztráty bude většinou přímo úměrná požadovaným intervalům záloh a cíli bodu obnovení. Maximální doba potřebná k obnovení odpovídá plánované době obnovení datové součásti vaší aplikace. Oba požadavky by se měly vyhodnocovat s ohledem na náklady na jejich zajištění. Každá organizace obvykle tvrdí, že si skutečně nemůže dovolit ztratit *žádná* data, ale často se to změní, když vezme v úvahu náklady potřebné ke splnění tohoto požadavku.

Zálohování bezpochyby hraje roli při zotavení po havárii, ale zálohy, obnovení a související scénáře přesahují rámec zotavení po havárii. Zálohy může být potřeba obnovit i v situacích, kdy nedošlo k havárii, včetně situací, kdy tolik nezáleží na plánované době obnovení (RTO) ani na cíli bodu obnovení (RPO). Pokud například dojde k poškození nebo odstranění malého množství dat, která jsou starší než váš interval zálohování, ale jinak nedojde k výpadku aplikace, smlouva o úrovni služeb (SLA) nemusí být ohrožená, protože po úspěšném obnovení zálohy se žádná data neztratí. Váš plán zotavení po havárii může, ale nemusí zahrnovat pokyny k provádění obnovení v situacích, kdy nedošlo k havárii.

> [!TIP]
> Nepleťte si *archivaci*, *replikaci* a *zálohování*. Archivace je ukládání dat pro dlouhodobé uchování a přístup pro čtení. Replikace je kopírování dat téměř v reálném čase mezi replikami kvůli podpoře vysoké dostupnosti. Používá se také v některých scénářích zotavení po havárii. Na vaše strategie ve všech třech těchto oblastech můžou mít vliv další požadavky, například zákony o uchovávání dat. Archivaci, replikaci a zálohování je třeba analyzovat a implementovat samostatně.

## <a name="azure-backup-and-restore-capabilities"></a>Možnosti zálohování a obnovení v Azure

Azure nabízí různé zálohovací služby a funkce určené pro různé scénáře, které se týkají dat v Azure i místních dat. Většina služeb Azure nabízí některý druh zálohovací funkce. V tomto článku se podíváme na několik nejoblíbenějších nabídek Azure týkajících se zálohování.

### <a name="azure-backup"></a>Azure Backup

Azure Backup je řada zálohovacích produktů, které zálohují data do trezorů služby Azure Recovery Services určených k ukládání a obnovení dat. Trezory služby Recovery Services jsou úložné prostředky Azure, které jsou určené k uchovávání záloh dat a konfigurací virtuálních počítačů, serverů, jednotlivých pracovních stanic a úloh.

> [!NOTE]
> Služby Azure Backup i Azure Site Recovery používají jako úložiště trezory služby Azure Recovery Services. Služba Azure Backup je zálohovací řešení pro obecné účely. Služba Azure Site Recovery dokáže koordinovat replikaci a převzetí služeb při selhání a podporuje operace zotavení po havárii s nízkými hodnotami RPO a RTO.

Služba Azure Backup slouží jako obecné zálohovací řešení pro cloudové a místní pracovní postupy spouštěné na virtuálních počítačích nebo na fyzických serverech. Je navržená tak, aby sloužila jako okamžitá náhrada za tradiční zálohovací řešení. Místo archivních pásků nebo jiných fyzických médií uchovává data v Azure.

Službu Azure Backup používají k vytváření záloh čtyři různé produkty a služby:

* **Azure Backup Agent** je malá aplikace pro Windows, která zálohuje soubory, složky a stav systému virtuálního počítače nebo serveru s Windows, na kterém je nainstalovaná. Funguje podobně jako řada zákaznických řešení cloudového zálohování, ale vyžaduje konfiguraci trezoru Azure Recovery. Po jejím stažení a instalaci na server nebo virtuální počítač s Windows ji můžete nakonfigurovat tak, aby vytvářela zálohy až třikrát denně.
* **System Center Data Protection Manager** je robustní systém zálohování a obnovení na podnikové úrovni s řadou funkcí. Data Protection Manager je aplikace pro Windows Server, která dokáže zálohovat systémy souborů a virtuální počítače (s Windows a Linuxem), vytvářet úplné zálohy systémů fyzických serverů a zálohovat řadu serverových produktů Microsoftu, jako je SQL Server a Exchange, se sledováním aplikací. Data Protection Manager je součástí řady produktů System Center. Licencuje se a prodává se jako součást produktu System Center, ale považuje se za součást řady služeb Azure Backup, protože může ukládat zálohy do trezoru Azure Recovery.
* **Azure Backup Server** je podobný jako Data Protection Manager, ale licencuje se v rámci předplatného Azure a nevyžaduje licenci produktu System Center. Azure Backup Server podporuje stejné funkce jako Data Protection Manager kromě místního zálohování na pásky a integrace s ostatními produkty System Center.
* **Zálohování virtuálních počítačů Azure IaaS** je funkce zálohování a obnovení na klíč ve službě Azure Virtual Machines. Zálohování virtuálních počítačů podporuje každodenní zálohování virtuálních počítačů s Windows a Linuxem. Podporuje obnovení jednotlivých souborů, celých disků a kompletních virtuálních počítačů a dokáže také vytvářet zálohy, které jsou konzistentní s aplikacemi. Jednotlivé aplikace dostanou informaci o operacích zálohování, aby před pořízením snímku zajistily konzistentní stav svých prostředků v systému souborů.

![Ilustrace zobrazující aplikaci Azure Backup Agent nakonfigurovanou s trezorem služby Recovery Services a zálohováním virtuálních počítačů Azure Aplikace Backup Agent a funkce zálohování virtuálních počítačů Azure ukládají všechna data do trezoru služby Recovery Services.](../media/azure-backup.png)

Služba Azure Backup nabízí přidanou hodnotu a přispívá k úspěšné strategii zálohování a obnovení aplikací IaaS a místních aplikací prakticky jakékoliv velikosti a typu.

### <a name="azure-blob-storage"></a>Azure Blob Storage

Služba Azure Storage neobsahuje funkci automatizovaného zálohování, ale k zálohování všech druhů dat z různých zdrojů se běžně používají objekty blob. Řada služeb, které nabízejí zálohování, používá k ukládání dat objekty blob. Tyto objekty jsou častým cílem skriptů a nástrojů ve všech druzích scénářů zálohování.

Účty úložiště pro obecné účely v2 podporují tři různé úrovně úložiště objektů blob s různým výkonem a náklady. **Studená** úroveň úložiště nabízí pro většinu záloh nejlepší poměr ceny a výkonu. **Horká** úroveň úložiště nabízí nižší náklady na přístup, ale má vyšší náklady na úložiště. **Archivní** úroveň úložiště může být vhodná pro sekundární zálohy nebo zálohy dat, u kterých nepotřebujete rychlé obnovení. Náklady jsou nízké, ale o přístup musíte žádat až 15 hodin předem.

Neměnné úložiště objektů blob můžete nakonfigurovat tak, aby se po dobu určenou uživatelem nedalo vymazat ani upravit. Neměnné úložiště objektů blob je navržené primárně pro plnění přísných požadavků na některé typy dat, jako jsou finanční data. Je skvělou možností pro zajištění, aby zálohy byly chráněné proti náhodnému odstranění nebo úpravě.

### <a name="azure-sql-database"></a>Azure SQL Database

Součástí služby Azure SQL Database jsou komplexní funkce automatického zálohování, a to bez dalších poplatků. Úplné zálohy se vytvářejí jednou týdně, rozdílové zálohování probíhá každých 12 hodin a zálohy protokolů se vytváří každých pět minut. Zálohy vytvořené touto službou je možné použít k obnovení databáze do určitého časového okamžiku, a to i v případě jejího odstranění. K obnovení je možné použít Azure Portal, PowerShell nebo REST API. Šifrují se i zálohy databází zašifrovaných technologií transparentního šifrování dat, která je zapnutá automaticky.

Zálohování služby SQL Database probíhá na podnikové úrovni, je připravené pro ostrý provoz a automaticky zapnuté. Pokud pro aplikaci zvažujete různé možnosti databází, měli byste zálohování zahrnout do analýzy ceny a výkonu, protože je velkou výhodou této služby. Tuto výhodu mají všechny aplikace, které používají službu Azure SQL Database, a měly by ji mít ve svém plánu zotavení po havárii a v postupech zálohování/obnovení.

### <a name="azure-app-service"></a>Azure App Service

Webové aplikace hostované ve službě Azure App Service úrovně Standard a Premium podporují plánované zálohování na klíč i ruční zálohování. Zálohy zahrnují konfiguraci a obsah souborů, ale také obsah databází používaných aplikací. Podporují také jednoduché filtry pro vyloučení souborů. Operace obnovení můžou mít za cíl různé instance služby App Service. Záloha služby App Service tak umožňuje jednoduše přesunout obsah jedné aplikace do druhé.

Zálohy služby App Service jsou omezené celkem na 10 GB, včetně aplikačního a databázového obsahu. Jsou vhodným řešením pro nové aplikace, u kterých probíhá vývoj, a také pro malé aplikace. Vyspělejší aplikace většinou nebudou zálohování služby App Service využívat. Budou spíše spoléhat na odolnější postupy nasazení a vrácení zpět a na takové strategie ukládání, které nevyužívají diskové úložiště aplikace, případně na strategie vyhrazeného zálohování databází a na trvalá úložiště.

## <a name="verify-backups-and-test-restore-procedures"></a>Ověřování záloh a testování postupů obnovení

Žádný systém zálohování není úplný bez strategie ověřování záloh a testování postupů obnovení. I když použijete vyhrazenou zálohovací službu nebo produkt, měli byste mít zdokumentované a nacvičené postupy obnovení, abyste měli jistotu, že jim dobře rozumíte a že dokážou vrátit systém do očekávaného stavu.

Strategie ověřování záloh jsou různé a závisí na povaze vaší infrastruktury. Můžete zvážit různé techniky, jako je vytvoření nového nasazení aplikace, obnovení zálohy do této aplikace a porovnání stavu obou instancí. V řadě případů tím věrně napodobíte skutečné postupy zotavení po havárii. Stačí i jednoduše porovnat podmnožinu zálohovaných dat s živými daty hned po vytvoření zálohy. Běžnou součástí ověřování záloh je také pokus o obnovení starých záloh, abyste se ujistili, že jsou stále dostupné a funkční a že se systém zálohování nezměnil tak, že by mohly být nekompatibilní.

Jakákoliv strategie je lepší, než když při pokusu o zotavení po havárii zjistíte, že vaše zálohy jsou poškozené nebo neúplné.

Strategie zálohování a obnovení je důležitá, protože přispívá k tomu, aby bylo možné vaši architekturu zotavit v případě ztráty nebo poškození dat. Zkontrolujte svou architekturu a definujte své požadavky na zálohování a obnovení. Azure poskytuje různé služby a funkce určené k zálohování a obnovení jakékoli architektury.
