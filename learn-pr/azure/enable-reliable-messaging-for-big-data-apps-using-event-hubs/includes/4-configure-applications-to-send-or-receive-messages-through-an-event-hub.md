Po vytvoření a nakonfigurování centra událostí bude potřeba nakonfigurovat aplikace na posílání a příjem datových streamů událostí.

Například v řešení pro zpracování plateb se použije některá podoba odesílací aplikace, která získá údaje o platební kartě zákazníka, a přijímací aplikace, která ověří platnost platební karty.

Konfigurace aplikace Java se sice liší od konfigurace aplikace .NET, ale platí obecné principy, které umožňují aplikacím připojit se k centru událostí, aby mohly úspěšně posílat a přijímat zprávy. Postup úpravy konfiguračních textových souborů v jazyce Java se liší od přípravy aplikace .NET v sadě Visual Studio, ale principy jsou stejné.

## <a name="what-are-the-minimum-event-hub-application-requirements"></a>Jaké jsou minimální požadavky na použití aplikace s centrem událostí?

Pokud chcete nakonfigurovat aplikaci, aby posílala zprávy do centra událostí, musíte poskytnout následující informace, aby aplikace mohla vytvořit přihlašovací údaje pro připojení:

- Obor názvů centra událostí
- Název centra událostí
- Název zásady sdíleného přístupu
- Sdílený primární přístupový klíč

Pokud chcete nakonfigurovat aplikaci, aby přijímala zprávy z centra událostí, zadejte následující informace, aby aplikace mohla vytvořit přihlašovací údaje pro připojení:

- Obor názvů centra událostí
- Název centra událostí
- Název zásady sdíleného přístupu
- Sdílený primární přístupový klíč
- Název účtu úložiště
- Připojovací řetězec účtu úložiště
- Název kontejneru účtu úložiště

Pokud máte přijímací aplikaci, která ukládá zprávy do Azure Blob Storage, budete také muset napřed nakonfigurovat účet úložiště.

## <a name="azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a>Příkazy Azure CLI pro vytvoření účtu úložiště pro obecné účely úrovně Standard

Azure CLI nabízí sadu příkazů, které můžete použít k vytvoření a správě účtu úložiště. Budeme s nimi pracovat v další lekci, ale tady je základní souhrn příkazů. 

> [!TIP]
> Existuje několik modulů MS Learn, které se týkají účtů úložiště, počínaje modulem **Seznámení se službou Azure Storage**.

| Příkaz | Popis |
|---------|-------------|
| `storage account create` | Vytvoření účtu úložiště pro obecné účely verze 2 |
| `storage account key list` | Načtení klíče účtu úložiště |
| `storage account show-connection-string` | Načtení připojovacího řetězce pro účet Azure Storage |
| `storage container create` | Vytvoří nový kontejner v účtu úložiště. |

## <a name="shell-command-for-cloning-an-application-github-repository"></a>Příkaz Shell ke klonování úložiště GitHub aplikace

Git je nástroj na spolupráci, který používá distribuovaný model řízení verzí. Je navržený pro spolupráci na softwarových a dokumentačních projektech. Klienti Gitu jsou k dispozici pro různé platformy včetně Windows. Příkazový řádek Gitu je součástí služby Azure Bash Cloud Shell. GitHub je webová hostingová služba pro úložiště Git. 

Pokud máte aplikaci, která je hostovaná jako projekt v GitHubu, můžete vytvořit místní kopii projektu tím, že naklonujete jeho úložiště příkazem **git clone**. Provedeme to v další lekci.

## <a name="editing-files-in-the-cloud-shell"></a>Upravování souborů v Cloud Shellu

Pomocí některého z předdefinovaných editorů v Cloud Shellu můžete upravit všechny soubory, které tvoří aplikaci, a přidat váš obor názvů centra událostí, název centra událostí, název zásad sdíleného přístupu a primární klíč. 

Cloud Shell podporuje editory **nano**, **vim** a **emacs** a také editor podobný Visual Studio Codu s názvem **code**. Stačí zadat název požadovaného editoru a ten se v prostředí spustí. V další lekci použijeme editor **code**.

## <a name="summary"></a>Shrnutí

V odesílací a přijímací aplikaci musí být nakonfigurované konkrétní informace o prostředí centra událostí. Účet úložiště vytvoříte, pokud přijímací aplikace ukládá zprávy do služby Blob Storage. Pokud je aplikace hostovaná v GitHubu, musíte ji klonovat do místního adresáře. K přidání vašeho oboru názvů do aplikace slouží textové editory, jako je **nano**.