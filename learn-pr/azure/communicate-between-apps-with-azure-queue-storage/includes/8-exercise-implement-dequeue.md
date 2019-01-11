Teď chceme aplikaci dokončit tím, že napíšeme kód, který přečte další zprávu ve frontě, zpracuje ji a pak ji z fronty odstraní. 

Tento kód umístíme do stejné aplikace a spustíme ho, když nepředáte žádné parametry. V našem scénáři zpravodajské služby bychom však tento kód ve skutečnosti umístili na servery střední vrstvy, kde by prováděl zpracování příběhů.

## <a name="dequeue-a-message"></a>Odstranění zprávy z fronty

Přidáme novou metodu, která z fronty načte další zprávu.

1. V editoru otevřete zdrojový soubor `Program.cs`.

1. Ve třídě `Program` vytvořte statickou metodu `ReceiveArticleAsync`, která nepřijímá žádné parametry a vrací `Task<string>`. Tuto metodu použijeme k přetáhnutí nového článku z fronty a jeho vrácení.
    - Pokračujte a přidejte k metodě klíčové slovo `async`, protože budeme používat některé asynchronní metody založené na objektu `Task`.

    ```csharp
    static async Task<string> ReceiveArticleAsync()
    {
    }

1. All of the setup code to get a `CloudQueue` will be identical to what we did in the last exercise. Code duplication is a bad habit, even in samples so go ahead and, refactor the code that obtains the `CloudQueue` to a new method named `GetQueue` and change the `SendArticleAsync` to use your new method.
     - Make sure to leave the code that _creates_ the queue in the `SendArticleAsync` method; remember only the **publisher** should create the queue.

    ```csharp
    const string ConnectionString = ...;
    // ...

    static CloudQueue GetQueue()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        return queueClient.GetQueueReference("newsqueue");
    }
    ```
    
1. V metodě `ReceiveArticleAsync` zavolejte novou metodu `GetQueue`, která načte odkaz na frontu a přiřadí ji do proměnné.

1. Potom pro objekt `CloudQueue` zavolejte metodu `ExistsAsync`, která vrátí informace o tom, jestli se vytvořila fronta. Pokud se pokusíme načíst zprávu z neexistující fronty, rozhraní API vyvolá výjimku.
    - Tato metoda je asynchronní, takže použijte klíčové slovo `await`, abyste získali návratovou hodnotu.
    - V metodě `ReceiveArticleAsync` už byste měli mít klíčové slovo `async`, ale pokud ne, teď ho přidejte.


1. Přidejte blok `if`, který používá návratovou hodnotu z metody `ExistsAsync`. Do tohoto bloku přidáme náš kód pro čtení hodnoty z fronty. Přidejte do metody poslední návratový řetězec, který značí, že se nepřečetla žádná hodnota. Vaše metoda by měla vypadat nějak takto:

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
    }

    return "<queue empty or not created>";
}
```

1. Zavolejte metodu `GetMessageAsync` pro objekt `CloudQueue`, abyste z fronty získali první zprávu `CloudQueueMessage`. Pokud je fronta prázdná, vrátí se hodnota `null`.

1. Pokud se vrátí jiná hodnota než null, k získání obsahu zprávy použijte vlastnost `AsString` objektu `CloudQueueMessage`.

1. Zavoláním metody `DeleteMessageAsync` pro objekt `CloudQueue` odstraňte zprávu z fronty.

Konečná implementace metody by měla vypadat přibližně takto:

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
        CloudQueueMessage retrievedArticle = await queue.GetMessageAsync();
        if (retrievedArticle != null)
        {
            string newsMessage = retrievedArticle.AsString;
            await queue.DeleteMessageAsync(retrievedArticle);
            return newsMessage;
        }
    }

    return "<queue empty or not created>";
}
```

## <a name="call-the-receivearticleasync-method"></a>Volání metody ReceiveArticleAsync

Nakonec přidáme podporu pro vyvolání nové metody. Provedeme to v případe, že do programu nepředáme žádné parametry.

1. Vyhledejte metodu `Main` a konkrétně blok `if`, který jste přidali dříve k vyhledání parametrů.

1. Přidejte podmínku `else` a zavolejte metodu `ReceiveArticleAsync`. 

1. Protože je asynchronní, pomocí klíčového slova `await` načtěte výsledek a vytiskněte ho do okna konzoly. Pokud jste aplikaci nepřevedli na C# 7.1, můžete hodnotu získat pomocí vlastnosti `Result` z vracející se úlohy.

Váš kód by měl vypadat přibližně takto:

```csharp
if (args.Length > 0)
{
    // ...
}
else
{
    string value = await ReceiveArticleAsync();
    Console.WriteLine($"Received {value}");
}
```

## <a name="execute-the-application"></a>Spuštění aplikace

Kód je teď hotový. Teď posílá a načítá zprávy. 

> [!WARNING]
> Než program sestavíte a spustíte, ujistěte se, že jste v online editoru uložili všechny soubory.

Pokud ho chcete otestovat, použijte příkaz `dotnet run` a předejte parametry, aby se odeslaly zprávy, nebo parametry vynechte, aby se přečetla jedna zpráva.

Pokud chcete otestovat případ, kdy fronta neexistuje, můžete pomocí Azure CLI frontu odstranit (společně se všemi daty). Nezapomeňte nahradit parametr `<connection-string>` (nebo nastavit proměnnou prostředí).

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

Při dalším přidání zprávy by se fronta měla znovu vytvořit.

> [!NOTE]
> K operaci odstranění ve skutečnosti dochází asynchronně. Pokud se nedokončila, může se vám při pokusu o opětovné vytvoření fronty zobrazit výjimka.