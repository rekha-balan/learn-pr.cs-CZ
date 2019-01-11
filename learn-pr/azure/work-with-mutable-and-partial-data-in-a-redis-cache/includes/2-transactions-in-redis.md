Někdy je třeba zaručit, že se provede více operací najednou. Ve vaší aplikaci rychlých zpráv mohou uživatelé odesílat jednotlivé obrázky, samostatné textové zprávy nebo obrázkové a textové zprávy najednou. Když se uživatel rozhodne odeslat obrázek a textovou zprávu najednou, musíte zajistit, že je ostatní členové skupiny obdrží ve stejný okamžik. Je to důležité, protože pokud by obrázek a textová zpráva nepřišly najednou, není vyloučené, že by se mezi nimi mohla doručit jiná zpráva. To může činit celou konverzaci matoucí.

Zde se podíváme na způsob vytvoření transakce v Azure Cache for Redis, aby se zajistilo provedení více operací najednou.

## <a name="creating-and-running-transactions"></a>Vytváření a spouštění transakcí

Transakce v Redis fungují tak, že se více příkazů zařadí do fronty, aby se provedly jako skupina. Při provádění transakce je garantováno, že se příkazy zařazené v její frontě provedou, aniž by se mezi ně vměstnaly jiné příkazy z jiných klientů.

Blok transakce začnete vytvářet zadáním příkazu `MULTI`. Další příkazy se zařadí do fronty a neprovedou se okamžitě. Spuštěním příkazu `EXEC` se všechny příkazy ve frontě provedou jako transakční jednotka. Pokud se rozhodnete, že chcete otevřenou transakci při zařazování příkazů do fronty přerušit, zavřete příkazem `DISCARD` blok transakce, aniž by se spustil _jakýkoli_ z příkaz zařazený do fronty.

Transakce Redis nepodporují koncept vrácení kroků zpět. Pokud do fronty v bloku transakce zařadíte příkaz s nesprávnou syntaxí, zůstane blok otevřený, ale automaticky se zahodí, pokud se ho pokusíte provést pomocí `EXEC`. Příkazy v transakci, které _během_ provádění (po vyvolání `EXEC`) selžou, nezpůsobí přerušení nebo vrácení transakce – Redis i v tomto případě spustí všechny příkazy a bude transakci považovat za úspěšně dokončenou.

## <a name="redis-transactions-with-servicestackredis"></a>Transakce Redis pomocí knihovny ServiceStack.Redis

**ServiceStack.Redis** je klientská knihovna jazyka C# pro interakci s Azure Cache for Redis.

Transakce v ServiceStack.Redis se vytvářejí voláním `IRedisClient.CreateTransaction()`. Vrácený objekt `IRedisTransaction` může obsahovat více příkazů, které jsou v něm zařazené do fronty pomocí `QueueCommand()`. Objekt transakce se provede tím, že se u něho vyvolá `Commit()`.

Objekty `IRedisTransaction` jsou na jedno použití a automaticky vydají příkaz `DISCARD`, pokud jsou odstraněny před vyvoláním příkazu `Commit()`. Tato funkce funguje dobře s bloky `using` jazyka C#: pokud z nějakého důvodu transakci neodešlete, máte jistotu, že se automaticky zahodí, aby bylo možné dál používat připojení Redis.

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a>Vytvoření transakce pomocí jazyka C# a klienta ServiceStack.Redis

Zde je příklad použití ServiceStack.Redis k vytvoření transakce, která může odeslat zprávu zahrnující adresu URL obrázku a obsah textové zprávy.

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    return transactionResult;
}
```

Transakce zajišťují, že se více operací provede najednou, aniž by se mezi ně vměstnaly operace z jiných klientů. Transakci vytvoříte pomocí příkazu `MULTI`. Se ServiceStack.Redis použijete metodu `CreateTransaction()`.