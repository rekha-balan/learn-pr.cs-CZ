Rozhodli jste se pomocí tématu Azure Service Bus distribuovat zprávy o výkonnosti prodeje ve vaší distribuované aplikaci pro prodejce. Aplikace používaná pracovníky prodeje na mobilních zařízeních bude posílat zprávy, které shrnují hodnoty prodeje za jednotlivé oblasti a časová období. Tyto zprávy se budou distribuovat do webových služeb, které jsou umístěné v provozních oblastech vaší společnosti, včetně Ameriky a Evropy.

Už jste implementovali potřebnou infrastrukturu ve svém předplatném Azure, včetně tématu a odběrů. Teď chcete napsat kód, který odesílá zprávy do tohoto tématu a načítá zprávy z odběru. Než začnete, budete muset zkontrolovat, že pracujete ve správném adresáři, a to provedením následujícího příkazu ve službě Cloud Shell:

```bash
cd ~/mslearn-connect-services-together/implement-message-workflows-with-service-bus/src/start
```

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Konfigurace připojovacího řetězce k oboru názvů služby Service Bus

Začněte konfigurací připojovacích řetězců v komponentách pro odesílání a příjem:

1. V editoru otevřete **performancemessagesender/Program.cs** a vyhledejte následující řádek kódu:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    Vložte připojovací řetězec mezi uvozovky a uložte soubor pomocí nabídky „...“ nebo klávesové zkratky (<kbd>Ctrl+S</kbd> ve Windows a Linuxu, <kbd>Cmd+S</kbd> v macOS).

1. Opakujte předchozí krok v souboru **performancemessagereceiver/Program.cs** vložením stejné hodnoty připojovacího řetězce a pak soubor uložte.

## <a name="write-code-that-sends-a-message-to-the-topic"></a>Napsání kódu, který odešle zprávu do tématu

Abyste dokončili komponentu, která odesílá zprávy o výkonnosti prodeje, postupujte podle těchto kroků:

1. Otevřete **performancemessagesender/Program.cs** v editoru.

1. Vyhledejte metodu `SendPerformanceMessageAsync()`.

1. V této metodě najděte následující řádek kódu:

    ```C#
    // Create a Topic Client here
    ```

1. Abyste vytvořili klienta tématu, nahraďte tento řádek kódu následujícím kódem:

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. V bloku `try...catch` najděte následující řádek kódu:

    ```C#
    // Create and send a message here
    ```

1. Abyste vytvořili a naformátovali zprávu pro frontu, nahraďte tento řádek kódu následujícím kódem:

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. Aby se zpráva zobrazila v konzole, přidejte na další řádek následující kód:

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. Abyste odeslali zprávu do fronty, přidejte na další řádek následující kód:

    ```C#
    await topicClient.SendAsync(message);
    ```

1. Najděte tento řádek kódu:

    ```C#
    // Close the connection to the topic here
    ```

1. Připojení ke službě Service Bus zavřete tak, že nahradíte tento řádek kódu následujícím kódem:

    ```C#
    await topicClient.CloseAsync();
    ```

1. Soubor uložte.

## <a name="send-a-message-to-the-topic"></a>Odeslání zprávy do tématu

Pokud chcete spustit komponentu, která odešle zprávu o prodeji, spusťte následující příkaz v Cloud Shellu:

```bash
dotnet run -p performancemessagesender
```

Když se program spustí, zobrazí se vám zprávy označující, že se odesílá zpráva. Pokaždé, když spustíte aplikaci, přidá se do tématu jedna zpráva a každý předplatitel dostane kopii.

Když skončíte, spusťte následující příkaz a podívejte se, kolik zpráv je v odběru pro Ameriku:

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

Pokud nahradíte `EuropeAndAfrica` za `Americas`, měli byste vidět, že oba odběry obsahují stejný počet zpráv.

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a>Napsání kódu, který načte zprávu z odběru tématu

Abyste dokončili komponentu, která načítá zprávy o výkonnosti prodeje, postupujte podle těchto kroků:

1. Otevřete **performancemessagereceiver/Program.cs** v editoru.

1. Vyhledejte metodu `MainAsync()`.

1. V této metodě najděte následující řádek kódu:

    ```C#
    // Create a subscription client here
    ```

1. Abyste vytvořili klienta odběru, nahraďte tento řádek následujícím kódem:

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. Vyhledejte metodu `RegisterMessageHandler()`.

1. Abyste nakonfigurovali možnosti zpracování zpráv, nahraďte veškerý kód v této metodě následujícím kódem:

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. Abyste registrovali obslužnou rutinu zpráv, přidejte na další řádek následující kód:

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. Vyhledejte metodu `ProcessMessagesAsync()`. Tuto metodu jste registrovali jako metodu, která zpracovává příchozí zprávy.

1. Abyste zobrazili příchozí zprávy v konzole, veškerý kód v rámci této metody nahraďte následujícím kódem:

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. Chcete-li odebrat přijatou zprávu z odběru, na další řádek přidejte následující kód:

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. Vraťte se k metodě `MainAsync()` a najděte následující řádek kódu:

    ```C#
    // Close the subscription here
    ```

1. Abyste ukončili připojení služby Service Bus, nahraďte tento kód následujícím kódem:

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. V nástroji Visual Studio Code zavřete všechna okna editoru a uložte všechny změněné soubory.

## <a name="retrieve-a-message-from-a-topic-subscription"></a>Načtení zprávy z odběru tématu

Provedením následujících kroků spusťte komponentu, která načítá zprávu o výkonnosti prodeje:

```bash
dotnet run -p performancemessagereceiver
```

Když program přestane zobrazovat oznámení, že přijímá zprávy, zastavte aplikaci stisknutím klávesy `Enter`. Potom spuštěním stejného příkazu jako předtím ověřte, že v odběru `Americas` nejsou žádné zbývající zprávy.

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

Pokud nahradíte `EuropeAndAfrica` za `Americas`, uvidíte, že počet zpráv se nezměnil. Aplikace načítala zprávy pouze z odběru `Americas`.
