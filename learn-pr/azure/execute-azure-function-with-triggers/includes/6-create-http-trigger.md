V této lekci vytvoříme funkci Azure, která přijme požadavek HTTP pomocí jediného řetězce. Tato funkce vrátí řetězec zpět volajícímu s vyjádřením o úspěchu či neúspěchu. Budeme dále pracovat s funkcí z předchozího cvičení.

## <a name="create-an-http-trigger"></a>Vytvoření triggeru HTTP

Budeme dál používat existující aplikaci Azure Functions a přidáme do ní trigger HTTP.

1. Zkontrolujte, že jste přihlášeni k portálu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) pomocí stejného účtu, kterým jste aktivovali izolovaný prostor (sandbox).

1. Přejděte na obrazovku **Všechny prostředky** a vyberte vaši aplikaci funkcí.

1. Klikněte na tlačítko Přidat (**+**) vedle **Funkce**. Tato akce spustí proces vytváření funkce.

1. V seznamu všech šablon dostupných pro tuto aplikaci funkcí vyberte **Trigger HTTP** .

1. V dialogovém okně **Nová funkce** zvolte název funkce a v rozevíracím seznamu **Úroveň autorizace** vyberte *Anonymní*.
1. Funkci vytvoříte výběrem možnosti **Vytvořit**. 

1. Prohlédněte si automaticky vygenerovaný kód, abyste získali představu o tom, co se děje. Parametr *req* představuje příchozí požadavek a obsahuje parametr *name*. Zkontrolujeme, zda parametr *name* má nějakou hodnotu. Pokud ano, vrátíme pozdrav. V opačném případě vrátíme chybovou zprávu.

## <a name="get-your-function-url"></a>Získání adresy URL funkce

Když jsme vytvořili trigger HTTP, pojďme získat adresu URL funkce, abychom mohli vytvořit žádost.

1. Vyberte trigger HTTP, abyste otevřeli obrazovku s kódem.

1. Napravo od **Spustit** vyberte **Získat adresu URL funkce**.

1. Vyberte **Kopírovat**, pak zavřete místní adresu URL funkce.

## <a name="issue-a-get-request-to-your-http-trigger"></a>Vydání žádosti GET pro trigger HTTP

Adresu URL funkce máme teď zkopírovanou do schránky. Pojďme vydat žádost GET, abychom zjistili, zda získáme odpověď.

1. Ve webovém prohlížeči otevřete novou kartu.

1. Vložte adresu URL do adresního řádku.

1. Přidejte parametr řetězce dotazu *name* s vaším jménem, například `.../api/HttpTriggerCSharp1?name=Jesse`.

1. Stisknutím klávesy <kbd>ENTER</kbd> odešlete žádost.
