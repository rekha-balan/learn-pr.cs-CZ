Data nejsou vždy trvalá a jsou situace, kdy je chcete v určitém čase odstranit. Například v aplikaci rychlého zasílání zpráv můžou uživatelé nastavit zobrazovaný název skupinového chatu. Chcete nicméně, aby se po hodině tento název resetoval. To můžete provést napsáním vlastní logiky na straně serveru, která nastaví časovač na jednu hodinu a odstraní název. Azure Cache for Redis podporuje vypršení platnosti dat, což je funkce, která toto dělá automaticky, aniž byste museli psát dodatečnou logiku.

Tady se dozvíte o běžných příkazech Redis pro implementaci vypršení platnosti dat.

## <a name="what-is-data-expiration"></a>Co je vypršení platnosti dat?

Vypršení platnosti dat je funkce, která může automaticky odstranit klíč a hodnotu v mezipaměti po nastavené době.

## <a name="why-use-data-expiration"></a>Proč používat vypršení platnosti dat?

Vypršení platnosti dat se obvykle používá v situacích, kdy ukládáte krátkodobá a jednorázová data.  To je důležité, protože Azure Cache for Redis je databáze v paměti a nemáte tak k dispozici tolik paměti, kolik byste měli při ukládání na disk. Protože je vaše úložiště na Azure Cache for Redis omezené, je na místě zajistit, abyste ukládali pouze důležitá data. Pokud určitá data nepotřebujete mít po ruce dlouhou dobu, nastavte pro ně vypršení platnosti.

## <a name="how-to-use-data-expiration-in-azure-cache-for-redis"></a>Jak používat vypršení platnosti dat v Azure Cache for Redis

Existují různé příkazy pro implementaci a správu vypršení platnosti dat v Azure Cache for Redis:

- `EXPIRE`: Nastaví časový limit klíče v sekundách.
- `PEXPIRE`: Nastaví časový limit klíče v milisekundách.
- `EXPIREAT`: Nastaví časový limit klíče pomocí absolutního unixového časového razítka v sekundách.
- `PEXPIREAT`: Nastaví časový limit klíče pomocí absolutního unixového časového razítka v milisekundách.
- `TTL`: Vrátí zbývající čas klíče v sekundách.
- `PTTL`: Vrátí zbývající čas klíče v milisekundách.
- `PERSIST`: Nastaví, že platnost klíče nikdy nevyprší.

Nejběžnější je příkaz `EXPIRE`, který budeme v tomto modulu dále používat.

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a>Příklad vypršení platnosti dat při použití jazyka C# a ServiceStack.Redis

Upozorňujeme, že při použití klientské knihovny (například **ServiceStack.Redis**) není potřeba pamatovat si nízkoúrovňové příkazy Azure Cache for Redis. Místo toho můžeme použít jednoduché metody jazyka C#.

```csharp
public static void SetGroupChatName(string groupChatID, string chatName)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create a key for group chat display names
        string key = groupChatID + "displayName";

        //Set the group chat display name
        redisClient.SetValue(key, chatName);

        //Set the expiration for one hour
        redisClient.Expire(key, 3600);
    }
}
```

Azure Cache for Redis umožňuje odstranit data automaticky po určité době pomocí funkce vypršení platnosti dat. To je důležité, protože Azure Cache for Redis je databáze v paměti a nemáte tak k dispozici tolik paměti, kolik byste měli při ukládání na disk. Pokud ukládaná data nepotřebujete mít k dispozici po dlouhou dobu, nastavte pro ně vypršení platnosti.