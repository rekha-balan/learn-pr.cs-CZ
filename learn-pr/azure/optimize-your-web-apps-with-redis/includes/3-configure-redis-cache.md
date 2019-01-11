Váš vývojový tým, který připravuje sportovní statistiky, rozhodl, že mezipamětí výrazně zlepší výkon dat požadovaných v nedávné době. Dalším krokem je vytvoření a konfigurace instance služby Azure Cache for Redis.

## <a name="create-and-configure-the-azure-cache-for-redis-instance"></a>Vytvoření a konfigurace instance Azure Cache for Redis

Mezipaměť Redis můžete vytvořit prostřednictvím webu Azure Portal, rozhraní Azure CLI nebo Azure PowerShellu. Existuje několik parametrů, ohledně kterých se budete muset rozhodnout, aby bylo možné správně nakonfigurovat mezipaměť pro vaše účely.

### <a name="name"></a>Název

Mezipaměť Redis bude vyžadovat globálně jedinečný název. Název musí být jedinečný v rámci Azure, protože se používá ke generování veřejné adresy URL k připojení a komunikaci se službou.

Musí to být řetězec znaků o délce 1 až 63 znaků, který obsahuje číslice, písmena a znak „-“. Název mezipaměti nesmí začínat ani končit znakem „-“ a nejsou platné ani po sobě jdoucí znaky „-“.

### <a name="resource-group"></a>Skupina prostředků

Azure Cache for Redis je spravovaný prostředek a vyžaduje vlastníka _skupiny prostředků_. Můžete buď vytvořit novou skupinu prostředků nebo použít existující skupinu v rámci předplatného.

### <a name="location"></a>Umístění

Výběrem oblasti Azure se budete muset rozhodnout, kde se bude mezipaměť Redis fyzicky nacházet. Instanci mezipaměti a svou aplikaci vždy umisťujte do stejné oblasti. Pokud byste se připojovali k mezipaměti v jiné oblasti, výrazně byste tím zvýšili latenci a snížili spolehlivost. Pokud se připojujete k mezipaměti mimo Azure, vyberte umístění v blízkosti spuštěné aplikace, která využívá data.

> [!IMPORTANT]
> Mezipaměť Redis umístěte co nejblíže k _příjemci_ dat.

### <a name="pricing-tier"></a>Cenová úroveň

Jak je uvedeno v poslední lekci, existují tři cenové úrovně, které jsou k dispozici pro Azure Cache for Redis.

- **Basic**: Základní mezipaměť ideální pro vývoj/testování. Je omezena na jeden server, 53 GB paměti a 20 000 připojení. Pro tuto úroveň služby neexistuje žádná smlouva SLA.
- **Standard**: Výrobní mezipaměť, která podporuje replikaci a zahrnuje smlouvu SLA na úrovni 99,99 %. Podporuje dva servery (hlavní/podřízený) a má stejná omezení paměti/připojení jako úroveň Basic.
- **Premium**: Podniková úroveň, která staví na úrovni Standard a zahrnuje podporu trvalého chodu, clusteringu a horizontálního navýšení mezipaměti. Toto je nejvýkonnější úroveň s až 530 GB paměti a 40 000 souběžnými připojeními.

Můžete ovládat dostupné množství paměti na jednotlivých úrovních – tato možnost se zvolí výběrem úrovně mezipaměti z možností C0–C6 základní/standardní úrovně nebo P0–P4 prémiové úrovně. Všechny podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cache/).

> [!TIP]
> K ostrému provozu systémů společnost Microsoft vždy doporučuje použít úroveň Standard nebo Premium. Úroveň Basic odpovídá systému s jedním uzlem bez replikace dat a smlouvy SLA. Jako mezipaměť použijte aspoň C1. Mezipaměti C0 jsou určené pro jednoduché vývojové/testovací scénáře, protože mají sdílené jádro procesoru a velice malou paměť.

Úroveň Premium umožňuje zachovat data jedním ze dvou způsobů, které nabízejí zotavení po havárii:

1. Při trvalosti RDB se pořizují pravidelné snímky, aby v případě selhání bylo možné mezipaměť znovu vytvořit ze snímku.

    ![Snímek obrazovky webu Azure Portal s možnostmi trvalosti RDB v nové instanci mezipaměti Redis](../media/3-redis-persistence-1.png)

2. Trvalost AOS ukládá každou operaci zápisu do protokolu, který se ukládá aspoň jednou za sekundu. Tím se vytvářejí větší soubory než v případě RDB, ale dochází k menší ztrátě dat.

    ![Snímek obrazovky webu Azure Portal s možnostmi trvalosti AOF v nové instanci mezipaměti Redis](../media/3-redis-persistence-2.png)

Existuje několik dalších nastavení, která jsou k dispozici pouze pro vrstvu **Premium**.

### <a name="virtual-network-support"></a>Podpora virtuální sítě

Pokud vytvoříte mezipaměť Redis úrovně Premium, můžete ji nasadit do virtuální sítě v cloudu. Vaše mezipaměť bude dostupná jenom ostatním virtuálním počítačům a aplikacím ve stejné virtuální síti. Tento postup poskytuje vyšší úroveň zabezpečení, když jsou služba i mezipaměť hostované v Azure, nebo jsou propojené prostřednictvím virtuální sítě Azure VPN.

### <a name="clustering-support"></a>Podpora clusteringu

S mezipamětí Redis úrovně Premium můžete implementovat clustering, který zajistí automatické dělení datové sady mezi více uzlů. Pokud chcete implementovat clustering, zadejte počet horizontálních oddílů (maximálně 10). Vynaložené náklady odpovídají nákladům původního uzlu krát počet horizontálních oddílů.

## <a name="accessing-the-redis-instance"></a>Přístup k instanci Redis

Redis podporuje sadu [známých příkazů](https://redis.io/commands). Příkaz je většinou ve tvaru `COMMAND parameter1 parameter2 parameter3`.

Tady jsou nejčastější příkazy, které můžete použít:

| Příkaz | Popis |
|---------|-------------|
| `ping` | Příkaz ping poslaný serveru. Vrátí „PONG“. |
| `set [key] [value]` | Nastaví v mezipaměti klíč/hodnotu. Při úspěchu vrátí „OK“. |
| `get [key]` | Získá hodnotu z mezipaměti. |
| `exists [key]` | Pokud **klíč** v mezipaměti existuje, vrátí 1. Pokud neexistuje, vrátí 0. |
| `type [key]` | Vrátí typ přidružený k hodnotě daného **klíče**. |
| `incr [key]` | Zvýšit hodnotu přidruženou ke **klíči** o 1. Hodnota musí být typu integer nebo double. Vrátí novou hodnotu. |
| `incrby [key] [amount]` | Zvýšit danou hodnotu přidruženou ke **klíči** o stanovený počet. Hodnota musí být typu integer nebo double. Vrátí novou hodnotu. |
| `del [key]` | Odstraní hodnotu přidruženou ke **klíči**. |
| `flushdb` | Odstraní z databáze _všechny_ klíče a hodnoty. |

Redis má nástroj příkazového řádku (**redis-cli**), ve kterém můžete přímo experimentovat s těmito příkazy. Tady je několik příkladů.

```output
> set somekey somevalue
OK
> get somekey
"somevalue"
> exists somekey
(string) 1
> del somekey
(string) 1
> exists somekey
(string) 0
```

Tady je příklad, jak pracovat s příkazy `INCR`. Jsou výhodné, protože nabízejí atomické přírůstky _v různých aplikacích_, které používají mezipaměť.

```output
> set counter 100
OK
> incr counter
(integer) 101
> incrby counter 50
(integer) 151
> type counter
(integer)
```

### <a name="adding-an-expiration-time-to-values"></a>Přidání doby platnosti hodnotám

Ukládání do mezipaměti je důležité, protože umožňuje uložit do paměti často používané hodnoty. Ale potřebujeme také mít možnost, ukončit platnost hodnot, pokud jsou zastaralé. V Redisu k tomu použijeme hodnotu _time to live_ (TTL) u klíče.

Jakmile hodnota TTL uplyne, klíč se automaticky odstraní úplně stejně jako příkazem DEL. Tady je několik poznámek k vypršení platnosti použitím hodnoty TTL.

- Vypršení platnosti můžete nastavit s přesností na sekundy nebo milisekundy.
- Rozlišení času vypršení platnosti je vždy 1 milisekunda.
- Informace o hodnotách s vypršenou platností se replikují na disk, kde se také uchovávají. Čas většinou uplyne, když je server Redis zastavený (to znamená, že Redis má uložené datum vypršení platnosti klíče).

Tady je příklad vypršení platnosti:

```output
> set counter 100
OK
> expire counter 5
(integer) 1
> get counter
100
... wait ...
> get counter
(nil)
```

## <a name="accessing-a-redis-cache-from-a-client"></a>Přístup klienta k mezipaměti Redis

Pro připojení k instanci Azure Cache for Redis budete potřebovat několik údajů. Pro službu mezipaměti potřebují klienti název hostitele, port a přístupový klíč. Tyto údaje můžete načíst z webu Azure Portal na stránce **Nastavení > Přístupové klíče**. 

- Název hostitele odpovídá veřejné internetové adrese vaší mezipaměti vytvořené z jejího názvu. Příklad: `sportsresults.redis.cache.windows.net`.

- Přístupový klíč funguje jako heslo vaší mezipaměti. Jsou vytvořené dva klíče: primární a sekundární. Můžete použít jeden nebo druhý. Dva jsou pro případ, že potřebujete primární klíč změnit. Všechny klienty můžete přepnout na sekundární klíč a pak primární klíč znovu vygenerovat. Tím zablokujete aplikace, které používají původní primární klíč. Microsoft doporučuje, abyste generování těchto klíčů pravidelně opakovali podobně, jako když generujete osobní hesla.

> [!WARNING]
> Přístupové klíče se považují za důvěrné informace, a proto s nimi zacházejte stejně jako s hesly. Každý, kdo má přístupový klíč, může ve vaší mezipaměti provádět jakékoli operace.
