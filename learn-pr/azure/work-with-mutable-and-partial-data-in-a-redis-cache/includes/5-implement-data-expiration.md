Tady přidáte k datům ve službě Azure Cache for Redis čas vypršení platnosti.

## <a name="add-an-expiration-time"></a>Přidání času vypršení platnosti

V předchozím cvičení jsme skončili s následujícím kódem v souboru **Program.cs**.

```csharp
bool transactionResult = false;

using (RedisClient redisClient = new RedisClient(redisConnectionString))
using (var transaction = redisClient.CreateTransaction())
{
    //Add multiple operations to the transaction
    transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
    transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

    //Commit and get result of transaction
    transactionResult = transaction.Commit();
}

if(transactionResult)
{
    Console.WriteLine("Transaction committed");
}
else
{
    Console.WriteLine("Transaction failed to commit");
}
```

Přidejme k **MyKey1** i **MyKey2** vypršení platnosti po 15 sekundách.

Před potvrzením transakce přidejte následující kód:

```csharp
//Add an expiration time
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
```

V tomto kódu je metoda **Expire** součástí **RedisNativeClient**. Chcete-li tuto metodu použít, musíte náš objekt nejprve přetypovat.

## <a name="verify-the-expiration"></a>Ověření vypršení platnosti

Když jsme nyní přidali kód pro vypršení platnosti našich dat, pojďme spustit program a zkontrolovat, zda budou data z Azure Cache for Redis odebrána.

1. Spusťte program.

    ```bash
    dotnet run
    ```

1. Vraťte se do konzoly Azure Cache for Redis na webu Azure Portal.

1. Abyste ověřili, že tam data stále jsou, zadejte následující příkaz:

    ```console
    get MyKey1
    ```

1. Po 15 sekundách zadejte příkaz znovu. Měli byste vidět, že tam data už nejsou.

    ![Snímek obrazovky konzoly Azure Cache for Redis zobrazující hodnotu nil pro MyKey1](../media/6-redis-console-data-expiration.png)