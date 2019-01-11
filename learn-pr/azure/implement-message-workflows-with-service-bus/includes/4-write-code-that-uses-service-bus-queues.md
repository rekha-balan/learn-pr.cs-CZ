Distribuované aplikace využívají fronty, například fronty Service Bus, jako dočasná úložiště pro zprávy, které čekají na doručení do cílové komponenty. K odesílání a příjmu zpráv prostřednictvím fronty musíte napsat kód pro zdrojovou i cílovou komponentu.

Představme si třeba aplikaci Contoso Slices. Zákazník zadá objednávku prostřednictvím webu nebo mobilní aplikace. Protože weby a mobilní aplikace běží na zařízeních zákazníků, není fakticky nijak omezený počet objednávek, které můžou přijít najednou. Tím, že mobilní aplikace a web vkládají objednávky do fronty, můžeme povolit, aby back-endová komponenta (webová aplikace) zpracovávala objednávky z této fronty vlastním tempem.

Aplikace Contoso Slices ve skutečnosti provádí při zpracování nové objednávky několik kroků. Všechny kroky jsou závislé na počáteční autorizační platbě, a proto jsme se rozhodli použít frontu. První úlohou naší přijímající komponenty bude zpracování platby.

V mobilní aplikaci a na webu musí společnost Contoso napsat kód, který přidá zprávu do fronty. V back-endové webové aplikaci vytvoří kód, který bude přebírat zprávy z fronty.

Tady se dozvíte, jak tento kód napsat.

## <a name="the-microsoftazureservicebus-nuget-package"></a>Balíček NuGet Microsoft.Azure.ServiceBus

Microsoft chce psaní kódu, který odesílá a přijímá zprávy přes Service Bus, usnadnit, a proto poskytuje knihovnu tříd .NET, které můžete použít v libovolném jazyce rozhraní .NET Framework pro interakci s frontou, tématem nebo přenosem přes Service Bus. Tuto knihovnu můžete do své aplikace zahrnout přidáním balíčku NuGet **Microsoft.Azure.ServiceBus**.

Nejdůležitější třídou pro fronty v této knihovně je `QueueClient`. Nejprve musíte vytvořit instance této třídy v odesílající i přijímající komponentě.

## <a name="connection-strings-and-keys"></a>Připojovací řetězce a klíče

Pro připojení k frontě v oboru názvů služby Service Bus potřebují zdrojové i cílové komponenty dva údaje:

- Umístění oboru názvů služby Service Bus, označované také jako **koncový bod**. Umístění se zadává jako plně kvalifikovaný název domény v rámci domény **servicebus.windows.net**. Příklad: **pizzaService.servicebus.windows.net**.
- Přístupový klíč. Service Bus omezuje přístup k frontám, tématům a přenosům tím, že vyžaduje přístupový klíč.

Oba tyto údaje jsou k dispozici v objektu `QueueClient` ve formě připojovacího řetězce. Správný připojovací řetězec pro váš obor názvů můžete získat přes Azure Portal.

## <a name="calling-methods-asynchronously"></a>Asynchronní volání metod

Fronta v Azure se může nacházet tisíce kilometrů od odesílající a přijímající komponenty. I když jsou fyzicky blízko, můžou pomalá připojení a kolize při využití šířky pásma způsobovat prodlevy, když komponenta volá metodu ve frontě. Z tohoto důvodu klientská knihovna služby Service Bus poskytuje pro interakci s frontami metody `async`. Pomocí těchto metod zabráníte zablokování vlákna při čekání na dokončení volání.

Při odesílání zprávy do fronty například použijte metodu `QueueClient.SendAsync()` s klíčovým slovem `await`.

## <a name="write-code-that-sends-to-queues"></a>Psaní kódu pro odesílání do front

V jakékoli odesílající nebo přijímající komponentě byste měli přidat následující příkazy `using` do veškerých souborů kódu, které volají frontu služby Service Bus:

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

Pak vytvořte nový objekt `QueueClient` a předejte mu připojovací řetězec a název fronty:

```C#
queueClient = new QueueClient(TextAppConnectionString, "PrivateMessageQueue");
```

Zprávu můžete do fronty odeslat zavoláním metody `QueueClient.SendAsync()` a předáním zprávy v podobě řetězce s kódováním UTF-8:

```C#
string message = "Sure would like a large pepperoni!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await queueClient.SendAsync(encodedMessage);
```

## <a name="receive-messages-from-the-queue"></a>Přijetí zpráv z fronty

Pokud chcete přijímat zprávy, musíte nejprve zaregistrovat popisovač zprávy – jedná se o metodu ve vašem kódu, který se vyvolá, když je ve frontě k dispozici zpráva.

```C#
queueClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

Proveďte zpracování. Pak z popisovače zprávy zavolejte metodu `QueueClient.CompleteAsync()`, která zprávu odeberete z fronty:

```C#
await queueClient.CompleteAsync(message.SystemProperties.LockToken);
```