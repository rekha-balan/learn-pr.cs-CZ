V distribuované aplikaci je potřeba některé zprávy odeslat jediné komponentě příjemce. Další zprávy budou muset dorazit do více než jednoho cíle.

Pojďme se podívat na to, co se stane, když uživatel zruší objednávku pizzy. Je to trochu jiné než při prvotním zadávání objednávky. V případě prvotní objednávky chceme počkat, než se zpracuje platba za objednávku, a teprve pak objednávka pokračuje k dalším krokům (například příprava a upečení pizzy v místní pizzerii). V případě operace stornování objednávky ale budeme najednou informovat jak pizzerii, *tak* zpracovatele platby. Tento přístup minimalizuje riziko, že se bude plýtvat ingrediencemi nebo časem řidiče, který by pizzu doručoval.

Aby byla zpráva odeslána do více komponent, použijeme téma služby Azure Service Bus.

## <a name="code-with-topics-vs-code-with-queues"></a>Kód s tématy a kód s frontami

Pokud chcete, aby se každá zpráva odeslaná doručila do všech komponent přihlášených k odběru, použijete témata. Psaní kódu, který využívá témata, je jedním ze způsobů, jak nahradit fronty. Použijete stejný balíček NuGet **Microsoft.Azure.ServiceBus**, nakonfigurujete připojovací řetězce a použijete asynchronní programovací vzory.

Místo třídy `QueueClient` ale pro odeslání použijete třídu `TopicClient` a pro příjem zpráv použijete třídu `SubscriptionClient`.

## <a name="setting-filters-on-subscriptions"></a>Nastavení filtrů pro odběry

Pokud chcete nastavit, aby se určité zprávy odeslané do tématu se doručily do konkrétních odběrů, můžete pro každý odběr v tématu nastavit filtry. V aplikaci pro objednávky pizzy například v našich pizzeriích běží aplikace pro Univerzální platformu Windows (UPW). Každá pizzerie se může přihlásit k odběru tématu OrderCancellation, ale musí si vyfiltrovat svoje vlastní ID restaurace (StoreID). Šetříme tak internetovou šířku pásma, protože neodesíláme zbytečné zprávy do vzdálených restaurací. Komponenta zpracování plateb se zároveň přihlásí k odběru všech našich stornovacích zpráv.

Filtry můžou být jednoho ze tří typů:

- **Logické filtry.** `TrueFilter` zajistí, že všechny zprávy odeslané do tématu se doručí do aktuálního odběru. `FalseFilter` zajistí, že se žádné zprávy nedoručí do aktuálního odběru. (Ve skutečnosti se tím zablokuje nebo vypne odběr.)
- **Filtry SQL.** Filtr SQL určuje podmínku pomocí stejné syntaxe jako klauzule `WHERE` v příkazu jazyka SQL. Odběratelům se doručí pouze zprávy, které při vyhodnocování pro daný odběr vrací hodnotu `True`.
- **Korelační filtry.** Korelační filtr obsahuje sadu podmínek, které jsou porovnávány s vlastnostmi jednotlivých zpráv. Pokud má vlastnost ve filtru stejnou hodnotu jako vlastnost ve zprávě, je považována za shodnou.

Pro náš filtr StoreID bychom *mohli* použít filtr SQL. Filtry SQL poskytují největší flexibilitu, ale jsou také výpočetně nejnáročnější a mohly by nám snížit propustnost služby Service Bus. V tomto případě místo toho zvolíme korelační filtr. 

## <a name="topicclient-example"></a>Příklad TopicClient

V jakékoli odesílající nebo přijímající komponentě byste měli přidat následující příkazy `using` do veškerých souborů kódu, které volají téma služby Service Bus:

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

Při odesílání zprávy začněte vytvořením nového objektu `TopicClient`, kterému předáte připojovací řetězec a název tématu:

```C#
topicClient = new TopicClient(TextAppConnectionString, "GroupMessageTopic");
```

Zprávu můžete do tématu odeslat zavoláním metody `TopicClient.SendAsync()` a předáním zprávy. Stejně jako u front musí být zpráva v podobě řetězce s kódováním UTF-8:

```C#
string message = "Cancel! I can't believe you use canned mushrooms!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await topicClient.SendAsync(encodedMessage);
```

Pro příjem zpráv musíte vytvořit objekt `SubscriptionClient`, nikoli objekt `TopicClient`, a předat mu připojovací řetězec, název tématu **a** název odběru:

```C#
subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, "GroupMessageTopic", "NorthAmerica");
```

Pak zaregistrujte popisovač zprávy – jde o asynchronní metodu v kódu, který zpracovává načtenou zprávu.

```C#
subscriptionClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

Z popisovače zprávy zavolejte metodu `SubscriptionClient.CompleteAsync()`, čímž zprávu odeberete z fronty:

```C#
await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
```