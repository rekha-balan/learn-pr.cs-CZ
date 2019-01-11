Moderní aplikace se často skládají z několika součástí běžících na různých počítačích a zařízeních, které jsou umístěné na různých místech světa. Mezi těmito součástmi jsou složité sítě s různou spolehlivostí a rychlostí. Základní otázkou u těchto distribuovaných aplikací je to, jak mezi součástmi spolehlivě komunikovat.

Předpokládejme, že jste cloudovým vývojářem pro Contoso Slices, globální řetězec zajišťující rozvážku pizzy. Váš zaměstnavatel upgraduje technologie tak, aby uživatelé mohli zadávat objednávky z webu nebo mobilních aplikací. Tyto objednávky budou odesílány do  upřednostňovaných pizzerií, kde pak příslušní zaměstnanci pizzu připraví. Při jednotlivých úkonech, jako je vyválení těsta, vložení pizzy do pece, zabalení do krabice a naložení do rozvážkového auta, se odesílají aktualizace do mobilní aplikace daného uživatele. Uživatelé dokonce dostanou aktualizované informace o tom, kde se pizza při rozvážce na cestě k nim právě nachází.

Řetězec Contoso Slices již dříve vytvořil systém online objednávek, který okamžitě ukládal data objednávek do databáze SQL Serveru. Každá pizzerie musela pamatovat na to, aby ručně aktualizovala stránku webových objednávek, jinak by nezjistila, že má nové objednávky. Kromě toho systém během špiček, kdy se obvykle objednává pizza, například při živě vysílaných sportovních akcích, často kolaboval, takže nezřídka docházelo k výjimkám a vypršení časového limitu. Předchozí systém navíc neměl centrální zpracování plateb ani žádný způsob, jakým by uživateli posílal aktualizovaný stav.

Na tento nový, náročnější projekt si v Contoso najali cloudového architekta a mají v plánu využívat oddělenou architekturu.

V tomto modulu si ukážeme, jak vám služba Azure Service Bus může pomoct vytvořit aplikaci, která zůstává spolehlivá i během špičky. Uvidíme také, jak služba Azure Service Bus usnadňuje přidávání funkcí do našich aplikací. Průběžně budeme psát kód v jazyce C# nezbytný pro tyto lekce. Tady uvidíte, jak používat témata a fronty služby Azure Service Bus v distribuované architektuře, aby bylo možné zajistit spolehlivou komunikaci i v době vysoké poptávky. Také budete psát kód v C#, který bude komunikovat přes službu Service Bus.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Rozhodnout, jestli se mají používat fronty, témata nebo přenosy služby Service Bus ke komunikaci v distribuované aplikaci
- Nakonfigurovat obor názvů služby Azure Service Bus v rámci předplatného Azure
- Vytvořit **téma** služby Service Bus a používat ho k odesílání a příjmu zpráv
- Vytvořit **frontu** služby Service Bus a používat ji k odesílání a příjmu zpráv

## <a name="prerequisites"></a>Požadavky

Žádné