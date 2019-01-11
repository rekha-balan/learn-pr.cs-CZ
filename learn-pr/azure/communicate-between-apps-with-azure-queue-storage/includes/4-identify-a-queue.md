Když teď máme účet úložiště, podívejme se, jak pracovat s frontou, kterou bude obsahovat.

K získání přístupu k frontě potřebujete tři údaje:

 1. Název účtu úložiště
 2. Název fronty
 3. Ověřovací token

Tyto údaje používají obě aplikace, které komunikují s frontou (webový front-end, který přidává zprávy, a prostřední vrstva, která je zpracovává).

## <a name="queue-identity"></a>Identita fronty

Každý fronta má název, který přiřadíte během vytváření. Název musí být jedinečný v rámci účtu úložiště, ale nemusí být jedinečný globálně (na rozdíl od názvu účtu úložiště).

Frontu jednoznačně identifikuje kombinace názvu vašeho účtu úložiště a názvu vaší fronty.

## <a name="access-authorization"></a>Autorizace přístupu

Každá žádost o přístup do fronty se musí autorizovat. Existuje několik možností, ze kterých si můžete zvolit.

| Typ ověřování | Popis |
|--------------------|-------------|
| **Azure Active Directory** | Můžete použít ověřování na základě role a identifikovat konkrétní klienty podle přihlašovacích údajů AAD. |
| **Sdílený klíč** | Někdy se označuje jako **klíč účtu** a jedná se o šifrovaný podpis klíče přidružený k účtu úložiště. Každý účet úložiště má dva tyto klíče, které je možné předávat v jednotlivých požadavcích k ověření přístupu. Tento přístup se podobá použití kořenového hesla – poskytuje _úplný přístup_ k účtu úložiště. |
| **Sdílený přístupový podpis** | Sdílený přístupový podpis (SAS) je vygenerovaný identifikátor URI, který klientům uděluje omezený přístup k objektům ve vašem účtu úložiště. Přístup můžete omezit na konkrétní prostředky, oprávnění a rozsah dat, aby se přístup po určité době automaticky vypnul.  |

> [!NOTE]
> My použijeme ověřování klíčem účtu, protože se jedná o nejjednodušší způsob, jak začít pracovat s frontami. V produkčních aplikacích se však doporučuje používat sdílený přístupový podpis (SAS) nebo Azure Active Directory (AAD).

### <a name="retrieving-the-account-key"></a>Načtení klíče účtu
 
Svůj klíč účtu najdete v části **Nastavení > Přístupové klíče** vašeho účtu úložiště na webu Azure Portal. Případně ho můžete načíst přes příkazový řádek:

```azurecli
az storage account keys list ...
```

```powershell
Get-AzStorageAccountKey ...
```

## <a name="accessing-queues"></a>Přístup k frontám

K frontě můžete přistupovat pomocí rozhraní REST API. Použijete přitom adresu URL, která je kombinací zvoleného názvu účtu úložiště, domény `queue.core.windows.net` a cesty k frontě, se kterou chcete pracovat. Například: `http://<storage account>.queue.core.windows.net/<queue name>`. Součástí každého požadavku musí být hlavička `Authorization`. Její hodnota může být libovolný ze tří typů ověřování.

### <a name="using-the-azure-storage-client-library-for-net"></a>Použití klientské knihovny Azure Storage pro .NET

Klientská knihovna Azure Storage pro .NET je knihovna poskytovaná společností Microsoft, která za vás formuluje požadavky REST a parsuje odpovědi REST. To významně snižuje množství kódu, který musíte napsat. Přístup pomocí klientské knihovny stále vyžaduje stejné informace (název účtu úložiště, název fronty a klíč účtu), tyto informace jsou ale uspořádané odlišně.

Klientská knihovna používá k navázání připojení **připojovací řetězec**. Svůj připojovací řetězec najdete v části **Nastavení** vašeho účtu úložiště na webu Azure Portal nebo ho můžete získat přes Azure CLI a PowerShell.

Připojovací řetězec je řetězec, který kombinuje název účtu úložiště a klíč účtu a který musí aplikace znát, aby mohla získat přístup k účtu úložiště. Má přibližně takovýto formát:

```csharp
string connectionString = "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net"
```

> [!WARNING]
> Hodnota tohoto řetězce by měla být uložená na bezpečném místě, protože kdokoli s přístupem k tomuto připojovacímu řetězci by mohl s frontou manipulovat.

Všimněte si, že připojovací řetězec neobsahuje název fronty. Název fronty zadáte v kódu při navazování připojení k frontě.

Teď získáme připojovací řetězec z Azure a nastavíme novou aplikaci, která ho bude používat.