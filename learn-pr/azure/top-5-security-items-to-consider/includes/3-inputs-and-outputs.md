Nejrozšířenější bezpečnostní slabinou aplikací v dnešní době je, že aplikace nezpracovává správně vstup přijímaný z externích zdrojů, zejména _uživatelský vstup_. Vždy byste měli každý vstup důkladně zkontrolovat a před jeho použitím ověřit, že je platný. Pokud tak neučiníte, může to mít za následek ztrátu nebo odhalení dat, eskalaci oprávnění nebo dokonce spuštění škodlivého kódu na počítačích jiných uživatelů.

Smutné přitom je, že se jedná o problém s jednoduchým řešením. Probereme tu, jak zacházet s daty – při jejich přijetí, při zobrazení na obrazovce a při ukládání pro pozdější použití.

## <a name="why-do-we-need-to-validate-our-input"></a>Proč potřebujeme náš vstup ověřovat?

Představte si, že vytváříte rozhraní, které umožní uživateli vytvořit na vašem webu účet. Mezi data profilu patří jméno, e-mail a přezdívka, která se bude zobrazovat všem, kdo web navštíví. Co když nový uživatel vytvoří profil a zadá přezdívku, která bude obsahovat nějaké příkazy SQL? Co když náš zákeřný uživatel zadá například něco jako tohle:

```sql
Eve'); DROP TABLE Users;--
```

Pokud jen slepě vložíte tuto hodnotu do databáze, mohlo by to potenciálně pozměnit příkaz SQL a vést ke spuštění příkazů, které rozhodně spustit nechceme! Popsaná situace se označuje jako „útok prostřednictvím injektáže SQL“ (SQL Injection) a je to jen jeden z _mnoha_ typů zneužití bezpečnostních slabin, kterými je možné na vás zaútočit, když neošetřujete vstupy správě. Jak to tedy můžeme napravit? V této lekci se naučíte, kdy je nutné ověřit vstup, jakým způsobem kódovat výstup a jak vytvářet parametrizované dotazy (které eliminují výše popsanou možnost zneužití). To jsou tři hlavní obranné techniky proti škodlivému vstupu zadanému do vašich aplikací.

## <a name="when-do-i-need-to-validate-input"></a>Když je potřeba vstup ověřit?

Odpověď je, že _vždy_. Musíte ověřovat **každý** vstup pro vaši aplikaci. To zahrnuje parametry v adrese URL, vstup od uživatele, data z databáze, data z rozhraní API a všechno, co je předáváno v nezašifrované podobě a čím by uživatel mohl potenciálně manipulovat. Vždy používejte přístup založený na seznamu povolených položek. To znamená, že přijímáte jen známé fungující vstupy. Nepoužívejte přístup založený na seznamu zakázaných položek, který obsahuje známé škodlivé vstupy. Je totiž prakticky nemožné sestavit úplný seznam všech potenciálně nebezpečných vstupů.  Provádějte toto na serveru, ne straně klienta (nebo jak na straně klienta, tak i na straně serveru), aby bylo zajištěno, že vaši obranu nejde obejít. Považujte **VŠECHNA** data za nedůvěryhodná a ochráníte se před většinou běžných bezpečnostních slabin webových aplikací.

Pokud používáte ASP.NET, poskytuje tato architektura [skvělou podporu pro ověřování vstupu](https://docs.microsoft.com/aspnet/web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites) na straně klienta i serveru.

Pokud používáte jinou webovou architekturu, najdete skvělé techniky k provádění ověření vstupu v [návodu OWASP pro ověřování vstupů (OWASP Input Validation Cheatsheet)](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet).


## <a name="always-use-parameterized-queries"></a>Vždy používejte parametrizované dotazy

Databáze SQL se běžně používají k ukládání dat, například informací o profilu.  Nikdy nedělejte to, že byste vytvářeli vložené příkazy SQL nebo jiné databázové dotazy za běhu v kódu a posílali je přímo do databáze. To si totiž přímo koleduje o malér, jak jsme viděli výše.

Nikdy například **nedělejte tohle** (označuje se to vložený kód SQL):

```csharp
string userName = ... // receive input from the user BEWARE!
...
string query = "SELECT *  FROM  [dbo].[users] WHERE userName = '" + userName + "'";
```

V uvedeném příkladu pospojujeme dohromady textové řetězce, abychom vytvořili dotaz, přičemž použijeme vstup od uživatele a generujeme dynamický dotaz SQL na vyhledání uživatele. Opět, pokud se uživatel se zlými úmysly dovtípí, že děláme právě tohle, nebo bude jen _zkoušet_ různé styly vstupu a sledovat, jestli se neobjeví nějaké slabiny, může to pro nás skončit katastrofálně. Místo toho použijte parametrizované příkazy SQL nebo uložené procedury, jako například:

```sql
-- Lookup a user
CREATE PROCEDURE sp_findUser
(
@UserName varchar(50)
)

SELECT *  FROM  [dbo].[users] WHERE userName = @UserName
```

Pomocí této metody můžete bezpečně vyvolat proceduru ze svého kódu a předat jí řetězec `userName`, aniž byste se museli strachovat, že by mohl být použit jako součást příkazu SQL.

## <a name="always-encode-your-output"></a>Vždy kódujte výstup

Jakýkoli výstup, který prezentujete vizuálně nebo do souboru, by měl být vždy kódován a řádně uvozen řídicími znaky. To vás může ochránit v případě, že při sanitizačním průchodu něco uniklo, nebo když kód nedopatřením vygeneruje něco, co by šlo zneužít. Tím zajistíte, že se vše zobrazí jako _výstup_ a nebude nedopatřením interpretováno jako něco, co by se mělo spustit. Toto je další velmi běžná technika útoku označovaná jako „skriptování mezi weby“ (Cross-Site Scripting, XSS).

Jelikož se jedná o tak běžný požadavek, je to jedna z oblasti, kde technologie ASP.NET rovnou udělá tuto práci za vás. Ve výchozím nastavení je veškerý výstup již kódován. Pokud používáte jinou webovou architekturu, podívejte se na své možnosti pro kódování výstupu na webech v [návodu OWASP pro ochranu před XSS (OWASP XSS Prevention Cheatsheet)](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet).

## <a name="summary"></a>Shrnutí

Sanitizace a ověřování vstupů je nutný předpoklad k zajištění toho, že vaše vstupní hodnoty budou platné a bude je možné bezpečně používat a ukládat. Většina moderních webových architektur nabízí integrované funkce, které umožňují automatizovat některé z těchto úloh. Můžete zkontrolovat dokumentaci k vaší preferované architektuře a zjistit, jaké funkce nabízí. I když jsou webové aplikace nejčastějším terčem útoků, mějte na paměti, že jiné typy aplikací mohou být stejně zranitelné. Nemyslete si, že jste v bezpečí jen proto, že vaše nová aplikace je počítačová aplikace. Vždy je třeba správně zpracovat vstup uživatele, aby někdo nemohl pomocí vaší aplikace poškodit vaše data nebo pověst vaší společnosti.