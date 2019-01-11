Úspěšně jste vytvořili webovou aplikaci a publikovali ji do Azure. Co se však stane, když chcete provádět změny? Visual Studio si pamatuje, kam jste aplikaci publikovali, takže k aktualizaci a změně aplikace stačí dvě kliknutí.

## <a name="explore-the-project-structure"></a>Prozkoumání struktury projektu

V sadě Visual Studio jsme vytvořili webovou aplikaci využívající ASP.NET a teď je potřeba web upravit a přizpůsobit. Pojďme prozkoumat strukturu projektu, abychom viděli, co pro nás Visual Studio vytvořilo.

### <a name="dependencies"></a>Dependencies (Závislosti)

Složka Dependencies zahrnuje vnitřní informace rozhraní ASP.NET, díky kterým je možné aplikaci zprovoznit. Pokud neplánujete přidat konkrétní balíčky třetích stran, nemusíte se touto složkou příliš zabývat.

### <a name="properties"></a>Vlastnosti

Složka Properties obsahuje konfigurační data pro umístění, ve kterém webovou aplikaci hostujete. Když teď rozbalíte složku **PublishProfiles**, měli byste vidět adresu URL webu Alpine Ski Hill. Každý profil publikování je soubor .xml obsahující informace o publikování konfigurace, jako je například adresa Azure, kterou sada Visual Studio používá k nahrání souborů.

### <a name="wwwroot"></a>wwwroot

Soubor wwwroot obsahuje všechny statické prostředky vašeho webu, jako jsou soubory .css, .js, .lib a obrázky. V této složce budete pracovat, když budete chtít změnit styl webu nebo přidat další funkce.

### <a name="pages"></a>Pages (Stránky)

Složka **Pages** obsahuje šablony _**syntaxe Razor**_ pro stránky vašeho webu.
Razor je syntaxe vytvořená kolem jazyka HTML, která má speciální konvence pro zobrazení dat a provádění logiky na webu.

Každou stránku webu reprezentují dva soubory s kódy:

- Prvním souborem je `.cshtml`, což je soubor označení syntaxe Razor. Tento soubor obsahuje zobrazovaný kód HTML a část logiky jazyka C#.

- Druhým souborem je `.cs`, což je kód na pozadí jazyka C# obsahující třídu `PageModel`. Tento soubor umožňuje zachytávat požadavky HTTP a částečně je zpracovat, než se do souboru Razor odešlou jakákoli data.

### <a name="appsettingjson"></a>appsetting.json

Toto je konfigurační soubor pro ASP.NET.

### <a name="bundleconfigjson"></a>bundleconfig.json

Soubor bundleconfig.json je předběžné zpracování konfigurace. Tento soubor při publikování zmenšuje vaše soubory .css a .js.

### <a name="programcs-and-startupcs"></a>Program.cs a Startup.cs

Soubory Program.cs a Startup.cs na webu konfigurují a spouští webového hostitele.

## <a name="introduction-to-razor-templates"></a>Úvod do šablon Razor

Na webu budeme chtít provést nějaké základní změny. K tomu budete potřebovat základní znalosti toho, jak využít šablon syntaxe Razor k přizpůsobení webové aplikace.

## <a name="what-is-razor"></a>Co je syntaxe Razor?

Razor je syntaxe rozhraní ASP.NET, která se používá k vytváření dynamických webových stránek pomocí jazyka C#. Když server přečte stránku syntaxe Razor, tak se ještě před vykreslením kódu HTML nejprve spustí kód jazyka C#. To vám umožňuje rychle generovat dynamický obsah.

Syntaxe Razor používá direktivy `@`, kterými rozhraní ASP.NET sděluje, jak stránku zpracovat.

Podívejte se třeba na kód na stránce `Contact.cshtml`:

```aspx-csharp
@page
@model ContactModel
@{
    ViewData["Title"] = "Contact";
}
<h2>@ViewData["Title"]</h2>
<h3>@Model.Message</h3>
...
```

- Direktiva `@page` sděluje rozhraní ASP.NET, aby se tento soubor zpracoval jako stránka syntaxe Razor.
- Direktiva `@model` sděluje rozhraní ASP.NET, aby tuto stránku syntaxe Razor svázalo s třídou jazyka C# s názvem `ContactModel`.

V syntaxi Razor se symbol `@` používá také k přechodu mezi kódem a HTML. Když se podíváte na výše uvedený fragment kódu, všimněte si `@{ ... }`. Toto je **blok kódu syntaxe Razor**. Jde o kód, který se _provede, ale nevykreslí_.

K vykreslení výstupu příkazu kódu používáme před výrazem jazyka C# symbol `@`. Ve výše uvedeném bloku kódu najdete dva takové příklady ve značkách `<h2>` a `<h3>`.

Vytvoření a publikování webu jsou pouze prvními kroky k vytvoření dobrého webu. Jakmile začnete přidávat obsah, budete muset web aktualizovat. Po prvním publikování webu do Azure, ho můžete kdykoli aktualizovat.
