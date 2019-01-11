Váš web zaměřený na sportovní akce využívá databázi, která vrací data prováděním dotazů. Při vysokém zatížení, zejména při velkých sportovních akcích, se ale jeho výkon snižuje. V hostovaných prostředích se vyšší využití prostředků promítá do vyšších nákladů. Optimální a ekonomické fungování vašeho webu je možné zajistit ukládáním dat do mezipaměti.

## <a name="what-is-caching"></a>Co je ukládání do mezipaměti?

Ukládání do mezipaměti znamená ukládání často používaných dat do paměti nacházející se velmi blízko aplikace, která data využívá. Ukládání do mezipaměti se používá ke zvýšení výkonu a snížení zatížení vašich serverů. Použijeme mezipaměť Redis, pomocí které vytvoříme mezipaměť umístěnou v paměti. Ta dokáže zajistit vynikající latenci a potenciálně zlepšit výkon.

## <a name="what-is-a-redis-cache"></a>Co je mezipaměť Redis?

Mezipaměť Redis (**RE**mote **DI**ctionary **S**erver) je open source úložiště párů klíč-hodnota v paměti. Je to oblíbené řešení, protože je rychlé a umožňuje ukládání a manipulaci s běžnými typy dat, jako jsou řetězce, hodnoty hash a sady. Považuje se také obecně za řešení vhodné pro vývojáře, protože podporuje více jazyků, mimo jiné Python, C, C++, C#, Java a JavaScript.

## <a name="what-is-azure-cache-for-redis"></a>Co je Azure Cache for Redis?

Služba Azure Cache for Redis je založená na oblíbené opensourcové mezipaměti Redis. Umožňuje vám přístup do zabezpečené, vyhrazené mezipaměti Redis spravované Microsoftem. Mezipaměť vytvořená pomocí Azure Cache for Redis je přístupná ze všech aplikací v rámci Azure. Azure Cache for Redis se obvykle používá ke zlepšení výkonu systémů, které z velké části využívají back-endová úložiště dat.

Vaše data jsou uložená v mezipaměti umístěné v paměti na serveru Azure s mezipamětí Redis, takže je databáze nemusí načítat z disku. Vaše mezipaměť je také vysoce škálovatelná. Kdykoli můžete upravit její velikost a cenovou úroveň.

## <a name="what-type-of-data-can-be-stored-in-the-cache"></a>Jaký typ dat je možné ukládat do mezipaměti?

Redis podporuje celou řadu datových typů orientovaných na _binárně bezpečné_ řetězce. To znamená, že jako hodnotu můžete použít jakoukoli binární sekvenci, od řetězce jako „i-love-rocky-road“ až po obsah souboru obrázku. Platná hodnota je i prázdný řetězec.

- Binárně bezpečné řetězce (nejběžnější)
- Seznamy řetězců
- Neuspořádané sady řetězců
- Hodnoty hash
- Uspořádané sady řetězců
- Mapy řetězců

Každá datová hodnota je přiřazená ke _klíči_, pomocí kterého je možné danou hodnotu vyhledat v mezipaměti. Redis funguje nejlépe s menšími hodnotami (100 kB nebo méně), proto zvažte rozdělení větších dat do několika klíčů. Je možné ukládat i větší hodnoty (až do 500 MB), ale zvyšuje se tím latence sítě. Pokud v mezipaměti není nakonfigurované vypršení platnosti starých hodnot, může to také vést k problémům s ukládáním do mezipaměti a nedostatkem paměti.

## <a name="what-is-a-redis-key"></a>Co je klíč Redis?
Klíče Redis jsou také binárně bezpečné řetězce. Tady je několik pokynů k výběru klíčů:

- Vyhněte se dlouhým klíčům. Zabírají více paměti a jejich vyhledávání trvá déle, protože se musí porovnávat bajt po bajtu. Pokud chcete jako klíč použít binární objekt blob, vygenerujte místo toho jedinečnou hodnotu hash a použijte ji jako klíč. Maximální velikost klíče je 512 MB, ale tak velký klíč byste _nikdy_ neměli používat.
- Používejte klíče umožňující identifikaci dat. Například klíč „sport:football;date:2008-02-02“ by byl lepší než klíč „fb:8-2-2“. První klíč je čitelnější a jeho větší velikost je zanedbatelná. Najděte nejlepší poměr mezi velikostí a čitelností.
- Dodržujte konvence. Dobrým příkladem je „objekt:id“ jako v klíči „sport:football“. 

## <a name="how-is-data-stored-in-a-redis-cache"></a>Jak se data v mezipaměti Redis ukládají?

Data se v mezipaměti ukládají v _**uzlech**_ a _**clusterech**_.

**Uzly** představují místo, kde se vaše data v mezipaměti Redis ukládají.

**Clustery** jsou sady tří nebo více uzlů, mezi které je vaše datová sada rozdělena. Clustery je užitečné využívat proto, že vaše operace bude pokračovat, i pokud uzel selže nebo nebude schopen komunikovat se zbytkem clusteru.

## <a name="what-are-redis-caching-architectures"></a>Co jsou architektury ukládání do mezipaměti Redis?

Architektura ukládání do mezipaměti Redis představuje způsob, jak naše data v mezipaměti distribuujeme. Redis distribuuje data třemi hlavními způsoby:

1. **Jeden uzel**
1. **Více uzlů**
1. **V clusteru**

Architektury ukládání do mezipaměti Redis se dělí v rámci Azure podle úrovně:

### <a name="basic-cache"></a>Mezipaměť na úrovni Basic

Mezipaměť na úrovni Basic nabízí mezipaměť Redis s _**jedním uzlem**_. Celá datová sada se uloží v jednom uzlu. Tato úroveň je ideální pro vývoj a testování nebo pro méně náročné úlohy.

### <a name="standard-cache"></a>Mezipaměť na úrovni Standard

Mezipaměť na úrovni Standard vytvoří architektury s _**více uzly**_. Redis replikuje mezipaměť ve dvouuzlové konfiguraci primární/sekundární. Azure spravuje replikaci mezi dvěma uzly. Toto je mezipaměť připravená pro produkční prostředí s replikací typu hlavní/podřízený.

### <a name="premium-tier"></a>Úroveň Premium

Úroveň Premium zahrnuje funkce úrovně Standard, ale navíc umožňuje trvalé uchování dat, pořizování snímků a zálohování dat. Na této úrovni můžete vytvořit cluster Redis, který za účelem zvýšení dostupné paměti rozděluje data mezi více uzlů Redis. Úroveň Premium také podporuje službu Azure Virtual Network, abyste měli plnou kontrolu nad svými připojeními, podsítěmi, přidělováním IP adres a izolací sítě. Součástí této úrovně je také geografická replikace, abyste mohli zajistit, že budou vaše data blízko aplikace, která je využívá.

## <a name="summary"></a>Shrnutí

Databáze se skvěle hodí k ukládání velkých objemů dat, ale je s ní spojená latence při vyhledávání dat. Odešlete dotaz. Server tento dotaz interpretuje, vyhledá odpovídající data a vrátí je. Servery mají také omezení kapacity pro zpracování požadavků. Pokud je požadavků příliš mnoho, načítání dat se pravděpodobně zpomalí. Při ukládání do mezipaměti se často požadovaná data ukládají v paměti, aby je bylo možné vracet rychleji než při dotazování databáze, což by mělo snížit latenci a zvýšit výkon. Azure Cache for Redis vám umožňuje přístup do zabezpečené, vyhrazené a škálovatelné mezipaměti Redis spravované Microsoftem, která je hostována v Azure.
