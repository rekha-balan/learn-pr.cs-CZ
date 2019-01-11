Předpokládejme, že obchodní partneři vaší společnosti mají zásady zabezpečení, které vyžadují, aby se jejich obchodní data chránila pomocí silného šifrování. Používáte aplikaci B2B, která běží na vašich serverech Windows a ukládá data na datových discích těchto serverů. Teď když přecházíte do cloudu, musíte vašim obchodním partnerům ukázat, že k datům uloženým na vašich virtuálních počítačích Azure nemají přístup neoprávnění uživatelé, zařízení ani aplikace. Musíte rozhodnout, jakou strategii zvolit pro implementaci šifrování vašich dat B2B.

Vaše požadavky na audit určují, že vaše šifrovací klíče se musí spravovat interně a nemůže je spravovat žádná třetí strana. Je pro vás také důležité, aby se zachoval výkon i možnosti správy vašich serverů založených na Azure. Proto před implementací šifrování chcete mít jistotu, že to neovlivní výkon.

## <a name="what-is-encryption"></a>Co je šifrování?

Šifrování je převedení smysluplných informací na něco, co vypadá nesmyslně, jako je například náhodná posloupnost písmen a číslic. Proces šifrování využívá jako součást algoritmu, který vytváří šifrovaná data, určitou formu **klíče**. Klíč je také zapotřebí k dešifrování. Klíče mohou být **_symetrické_**, kdy se stejný klíč používá k šifrování i dešifrování, nebo **_asymetrické_**, kdy se používají odlišné klíče. Příkladem posledně jmenovaných klíčů je pár **veřejného a privátního** klíče používaný v digitálních certifikátech.

### <a name="symmetric-encryption"></a>Symetrické šifrování

Algoritmy, které používají symetrické klíče, jako je Standard AES (Advanced Encryption), jsou obvykle rychlejší než algoritmy pro veřejné klíče a často se používají pro ochranu rozsáhlých úložišť dat. Vzhledem k tomu, že se používá jenom jeden klíč, musí být nastavené postupy, které zabraňují tomu, aby tento klíč byl veřejně známý.

### <a name="asymmetric-encryption"></a>Asymetrické šifrování

U asymetrických algoritmů stačí z příslušného páru zabezpečit jenom privátní klíč. Veřejný klíč, jak jeho označení napovídá, může být přístupný komukoli, aniž by to ohrožovalo šifrovaná data. Nevýhodou algoritmů pro veřejné klíče ale je, že jsou mnohem pomalejší než symetrické algoritmy a nedají se použít pro šifrování velkých objemů dat.

## <a name="key-management"></a>Správa klíčů

V Azure může šifrovací klíče spravovat Microsoft nebo zákazník. Zájem o klíče spravované zákazníkem často mají organizace, které potřebují prokazovat dodržování předpisů HIPAA nebo jiných nařízení. Takové dodržování předpisů může vyžadovat, aby se protokoloval přístup ke klíčům a aby se pravidelně prováděly a zaznamenávaly změny klíčů.

## <a name="azure-disk-encryption-technologies"></a>Technologie šifrování disků Azure

Hlavní technologie ochrany disků založené na šifrování pro virtuální počítače Azure jsou tyto:

- Šifrování služby Storage (SSE)
- Azure Disk Encryption (ADE)

### <a name="storage-service-encryption"></a>Šifrování služby Storage

Šifrování služby Azure Storage (SSE, Storage Service Encryption) je šifrovací služba integrovaná do Azure, která se používá k ochraně neaktivních uložených dat. Platforma úložiště Azure automaticky šifruje data předtím, než je uloží do několika služeb úložiště, včetně služby Azure Managed Disks. Šifrování se standardně provádí pomocí 256bitového šifrování AES a spravuje ho správce účtu úložiště.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

ADE (Azure Disk Encryption) spravuje vlastník virtuálního počítače. Řídí šifrování disků virtuálních počítačů s Windows a Linuxem pomocí nástroje **BitLocker** na virtuálních počítačích s Windows a nástroje **DM-Crypt** na virtuálních počítačích s Linuxem. BitLocker Drive Encryption je funkce ochrany dat, která se integruje s operačním systémem a řeší hrozby spojené s krádeží dat nebo jejich únikem ze ztracených, odcizených nebo nesprávně vyřazených počítačů. Podobně v Linuxu šifruje DM-Crypt neaktivní uložená data před zápisem do úložiště.

ADE zajišťuje, aby všechna neaktivní uložená data na discích virtuálních počítačů byla zašifrovaná v úložišti Azure a vyžaduje se pro virtuální počítače zálohované do trezoru služby Site Recovery.

Při použití ADE se virtuální počítače spouštějí v rámci zásad a klíčů řízených zákazníkem. ADE je pro účely správy těchto klíčů a tajných kódů pro šifrování disků integrováno se službou Azure Key Vault.

> [!NOTE] 
> ADE nepodporuje šifrování virtuálních počítačů úrovně Basic a společně s ADE se nedá používat místní Služba správy klíčů (KMS).

## <a name="when-to-use-encryption"></a>Kdy šifrování použít?

Počítačová data jsou ohrožená během přenosu (napříč internetem nebo jinou sítí) a když jsou neaktivní (uložená v úložném zařízení). Při ochraně dat na discích virtuálních počítačů Azure jsou nejdůležitějším faktorem scénáře pro neaktivní data. Někdo například může stáhnout soubor virtuálního pevného disku (VHD) přidružený k vašemu virtuálnímu počítači Azure VM a uložit si ho do svého přenosného počítače. Pokud tento disk VHD není zašifrovaný, je jeho obsah potenciálně přístupný komukoli, kdo si může soubor VHD připojit ke svému počítači.

Na discích operačního systému (OS) se data jako třeba hesla šifrují automaticky, takže i když samotný disk VHD není zašifrovaný, není snadné k informacím tohoto typu získat přístup. Aplikace také mohou automaticky šifrovat svoje vlastní data. Pokud ale někdo se zlými úmysly získá přístup k datovému disku a disk samotný není zašifrovaný, může být i při takové ochraně v situaci, že bude moci využít libovolné známé slabiny v ochraně dat příslušné aplikace. Když je použité šifrování, není takové zneužití možné.

Šifrování služby Storage (SSE) je součástí Azure a jeho používání by nemělo mít znatelný dopad na vstupně-výstupní výkon disků virtuálních počítačů. Spravované disky s Šifrováním služby Storage jsou teď standardem a není žádný důvod to měnit. Azure Disk Encryption (ADE) využívá nástroje operačního systému virtuálního počítače (BitLocker a DM-Crypt), takže při šifrování nebo dešifrování disků virtuálního počítače musí část práce provést vlastní virtuální počítač. Vliv této další aktivity procesoru virtuálního počítače je s výjimkou určitých situací obvykle zanedbatelný. Pokud například máte aplikaci s vysokými nároky na procesor, stojí za úvahu nechat disk operačního systému nezašifrovaný a zajistit tak maximální výkon. V takových situacích můžete aplikační data ukládat na samostatný šifrovaný datový disk a získat tak potřebný výkon, aniž byste ohrozili zabezpečení.

Azure poskytuje dvě komplementární technologie šifrování, které se používají k zabezpečení disků virtuálních počítačů Azure. Tyto technologie, SSE a ADE, šifrují na různých úrovních a mají také různý účel. Obě využívají 256bitové šifrování AES. Použití obou technologií zajišťuje důkladnou ochranu proti neoprávněnému přístupu k vašemu úložišti Azure a konkrétním virtuálním pevným diskům.
