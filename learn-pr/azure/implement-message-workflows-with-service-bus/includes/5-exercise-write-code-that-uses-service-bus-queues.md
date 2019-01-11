Rozhodli jste se používat frontu služby Service Bus k výměně zpráv o jednotlivých prodejích mezi mobilní aplikací, kterou používají vaši pracovníci prodeje, a webovou službou hostovanou v Azure, která bude ukládat podrobnosti o každém prodeji v instanci služby Azure SQL Database.

Už jste implementovali nezbytné objekty ve vašem předplatném Azure. Teď chcete napsat kód, který bude zprávy do této fronty odesílat a zase je z ní načítat.

## <a name="clone-and-open-the-starter-application"></a>Klonování a otevření úvodní aplikace

V této lekci vytvoříte dvě konzolové aplikace. První aplikace umísťuje zprávy do fronty služby Service Bus a druhá je z ní načítá. Obě aplikace jsou součástí jednoho řešení .NET Core.

1. Začněte klonováním řešení: spusťte následující příkazy ve službě Cloud Shell:

```bash
cd ~
git clone https://github.com/MicrosoftDocs/mslearn-connect-services-together.git
```

2. Dále přejděte do složky úvodní aplikace a otevřete editor Cloud Shell.

```bash
cd mslearn-connect-services-together/implement-message-workflows-with-service-bus/src/start
code .
```

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Konfigurace připojovacího řetězce k oboru názvů služby Service Bus

Abyste umožnili přístup k oboru názvů služby Service Bus a mohla se používat fronta, musíte v konzolových aplikacích nakonfigurovat dva údaje:

* Koncový bod pro váš obor názvů
* Sdílený přístupový klíč pro ověřování

Obě tyto hodnoty se dají získat z webu Azure Portal v podobě úplného připojovacího řetězce.

> [!NOTE]
> Kvůli zjednodušení zakódujete připojovací řetězec u obou konzolových aplikací napevno v souboru **Program.cs**. V produkční aplikaci byste mohli k uložení připojovacího řetězce použít konfigurační soubor nebo i službu Azure Key Vault.

1. V CloudShellu spusťte následující příkaz a zobrazte primární připojovací řetězec pro váš obor názvů služby Service Bus. Nahraďte `<namespace-name>` názvem vašeho oboru názvů služby Service Bus.

    ```azurecli
    az servicebus namespace authorization-rule keys list \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name RootManageSharedAccessKey \
        --query primaryConnectionString \
        --output tsv \
        --namespace-name <namespace-name>
    ```

    Tento připojovací řetězec budete v průběhu tohoto modulu potřebovat víckrát, proto ho vložte do nějakého vhodného umístění.

1. Zkopírujte klíč ze služby Cloud Shell. V editoru otevřete **privatemessagesender/Program.cs** a vyhledejte následující řádek kódu:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    Vložte připojovací řetězec uvedený v uvozovkách. Stisknutím kombinace kláves <kbd>Ctrl+S</kbd> uložte soubor.

1. Opakujte předchozí krok v souboru **privatemessagereceiver/Program.cs** vložením stejné hodnoty připojovacího řetězce. Soubor uložte prostřednictvím nabídky „...“ nebo klávesové zkratky (<kbd>Ctrl+S</kbd> ve Windows a Linuxu, <kbd>Cmd+S</kbd> v macOS).

## <a name="write-code-that-sends-a-message-to-the-queue"></a>Napsání kódu, který odešle zprávu do fronty

Provedením následujících kroků dokončete komponentu, která odesílá zprávy o prodejích:

1. Otevřete **privatemessagesender/Program.cs** v editoru.

1. Vyhledejte metodu `SendSalesMessageAsync()`.

1. V této metodě najděte následující řádek kódu:

    ```C#
    // Create a queue client here
    ```

1. Abyste vytvořili klienta fronty, nahraďte tento řádek kódu následujícím kódem:

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. V bloku `try...catch` najděte následující řádek kódu:

    ```C#
    // Create and send a message here
    ```

1. Abyste vytvořili a naformátovali zprávu pro frontu, nahraďte tento řádek kódu následujícím kódem:

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. Aby se zpráva zobrazila v konzole, přidejte na další řádek následující kód:

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. Abyste odeslali zprávu do fronty, přidejte na další řádek následující kód:

    ```C#
    await queueClient.SendAsync(message);
    ```

1. Najděte tento řádek kódu:

    ```C#
    // Close the connection to the queue here
    ```

1. Nahrazením tohoto řádku kódu následujícím kódem ukončete připojení ke službě Service Bus:

    ```C#
    await queueClient.CloseAsync();
    ```

1. Soubor uložte.

## <a name="send-a-message-to-the-queue"></a>Odeslání zprávy do fronty

Pokud chcete spustit komponentu, která odešle zprávu o prodeji, spusťte následující příkaz v Cloud Shellu:

```bash
dotnet run -p privatemessagesender
```

> [!NOTE]
> Aplikacím spuštěným v rámci tohoto cvičení může chvíli trvat, než se spustí, protože `dotnet` musí obnovit balíčky ze vzdálených zdrojů a sestavit aplikace při jejich prvním spuštění.

Když se program spustí, zobrazí se vám zprávy označující, že se odesílá zpráva. Pokaždé, když spustíte aplikaci, přidá se do fronty jedna zpráva.

Když skončíte, spusťte následující příkaz a podívejte se, kolik zpráv je ve frontě:

```azurecli
az servicebus queue show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name salesmessages \
    --query messageCount \
    --namespace-name <namespace-name>
```

## <a name="write-code-that-receives-a-message-from-the-queue"></a>Napsání kódu, který přijme zprávu z fronty

1. Otevřete **privatemessagereceiver/Program.cs** v editoru

1. Vyhledejte metodu `ReceiveSalesMessageAsync()`.

1. V této metodě najděte následující řádek kódu:

    ```C#
    // Create a queue client here
    ```

1. Abyste vytvořili klienta fronty, nahraďte tento řádek následujícím kódem:

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
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
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. Vyhledejte metodu `ProcessMessagesAsync()`. Tuto metodu jste registrovali jako metodu, která zpracovává příchozí zprávy.

1. Aby se příchozí zprávy zobrazily v konzole, nahraďte veškerý kód v této metodě následujícím kódem:

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. Abyste odebrali přijatou zprávu z fronty, přidejte na další řádek následující kód:

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. Vraťte se k metodě `ReceiveSalesMessageAsync()` a vyhledejte následující řádek kódu:

    ```C#
    // Close the queue here
    ```

1. Nahrazením tohoto řádku následujícím kódem zavřete připojení služby Service Bus:

    ```C#
    await queueClient.CloseAsync();
    ```

1. Soubor uložte.

## <a name="retrieve-a-message-from-the-queue"></a>Načtení zprávy z fronty

Chcete-li spustit komponentu, která odešle zprávu o prodeji, spusťte tento příkaz ve službě Azure Cloud Shell:

```bash
dotnet run -p privatemessagereceiver
```

Až uvidíte, že zpráva je přijatá a zobrazená v konzole, stiskněte `Enter` a aplikaci zastavte. Potom spusťte stejný příkaz jako předtím a potvrďte, že všechny zprávy byly odebrány z fronty:

```azurecli
az servicebus queue show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name salesmessages \
    --query messageCount \
    --namespace-name <namespace-name>
```

Tím se zobrazí `0`. pokud byly odebrány všechny zprávy.

Napsali jste kód, který odešle zprávu o jednotlivém prodeji do fronty služby Service Bus. U aplikace distribuované prodejcům byste měli tento kód napsat do mobilní aplikace, kterou budou pracovníci prodeje používat na zařízeních.

Také jste napsali kód, který přijme zprávu z fronty služby Service Bus. U aplikace distribuované prodejcům byste měli tento kód napsat do webové služby, která běží v Azure a zpracovává přijaté zprávy.
