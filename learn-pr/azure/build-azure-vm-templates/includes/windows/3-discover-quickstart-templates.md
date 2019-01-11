Vzpomeňte si, že finanční modely analytiků se provozují na virtuálních počítačích Azure. Pokud chcete nasazení ještě více automatizovat, můžete od příkazů a skriptů Azure CLI přejít k šablonám Resource Manageru.

Než začnete, mohlo by vás zajímat, jaké jsou existující šablony, ze kterých byste se mohli něco naučit a na kterých byste mohli stavět.

Zde zjistíte, co je šablona Azure pro rychlý start a jaké předem sestavené šablony můžete hned začít používat.

> [!TIP]
> Preferujete Linux, nebo chcete vyzkoušet něco nového? Pokud chcete nasadit linuxový virtuální počítač, vyberte na začátku této stránky **Linux**.

## <a name="what-are-azure-quickstart-templates"></a>Co jsou šablony Azure pro rychlý start?

Šablony Azure pro rychlý start jsou šablony Resource Manageru pocházející od komunity Azure. Šablony pro rychlý start jsou dostupné na GitHubu.

Mnohé šablony poskytují vše, co potřebujete k nasazení svého řešení. Jiné mohou sloužit jako výchozí bod pro vaši šablonu. V obou případech se můžete studiem těchto šablon naučit, jak co nejlépe vytvářet a strukturovat své vlastní šablony.

## <a name="discover-whats-on-the-quickstart-template-gallery"></a>Prozkoumání galerie šablon pro rychlý start

Řekněme, že chcete najít šablonu Resource Manageru se základní konfigurací virtuálního počítače &ndash; takovou, která obsahuje virtuální počítač, základní nastavení sítě a úložiště.

1. Začněte tím, že přejdete do [galerie šablon pro rychlý start](https://azure.microsoft.com/resources/templates?azure-portal=true) a zjistíte, co je k dispozici.

    Uvidíte množství oblíbených a nedávno aktualizovaných šablon. Tyto šablony fungují s prostředky Azure a oblíbenými softwarovými balíčky.

    ![Část webové stránky s galerií šablon Azure pro rychlý start](../../media/3-gallery-homepage.png)

1. Dejme tomu, že narazíte na šablonu [Nasazení jednoduchého virtuálního počítače s Windows](https://azure.microsoft.com/resources/templates/101-vm-simple-windows?azure-portal=true).

    ![Stránka galerie s šablonou virtuálního počítače s Windows](../../media/3-gallery-page-windows.png)

    Zdá se, že tento název vyjadřuje přesně to, co potřebujete. Podívejme se ale blíže, co tato šablona provádí.

    Tlačítko **Nasadit do Azure** umožňuje nasadit tuto šablonu přímo přes Azure Portal. To tady ale neuděláte. Místo toho použijete k nasazení této šablony Azure CLI z prostředí Cloud Shell.

1. Kliknutím na **Procházet na GitHubu** přejděte na zdrojový kód šablony na GitHubu.

    Uvidíte toto.

    ![Soubor README na GitHubu pro šablonu Resource Manageru](../../media/3-github-page-windows.png)

    Tlačítko **Nasadit do Azure** umožňuje nasadit tuto šablonu přímo přes Azure Portal, stejně jako jste to viděli na stránce galerie.

1. Kliknutím na **Vizualizovat** přejděte na vizualizér Azure Resource Manageru.

    Zobrazí se prostředky, které jsou součástí nasazení, včetně virtuálního počítače, účtu úložiště a síťových prostředků.

    Pomocí myši můžete tyto prostředky uspořádat. Rolovacím kolečkem myši můžete také přiblížit a oddálit zobrazení.

    ![Vizualizér Azure Resource Manageru znázorňující prostředky Azure](../../media/3-armviz-windows.png)

1. Klikněte na prostředek **Virtuální počítač** s názvem **SimpleWinVM**.

    Zobrazí se zdrojový kód, který definuje prostředek virtuálního počítače.

    ![Vizualizér Azure Resource Manageru znázorňující zdrojový kód šablony](../../media/3-armviz-vm-windows.png)

    Za chvíli budete mít více času na prozkoumání zdrojového kódu. Teď si ho ale jen v rychlosti prohlédněte.

    Uvidíte, že:

    * Typ prostředku je `Microsoft.Compute/virtualMachines`.
    * Jeho umístění, neboli oblast Azure, pochází z parametru šablony s názvem `location`.
    * Velikost virtuálního počítače je **Standard_A2**.
    * Název počítače se načítá z proměnné šablony a jeho uživatelské jméno a heslo se načítají z parametrů šablony.

V praxi si můžete prohlédnout soubor **README.md** na GitHubu a důkladnějším prozkoumáním zdrojového kódu zjistit, jestli tato šablona vyhovuje vašim potřebám.

Prozatím vypadá tato šablona slibně. V další části se posunete dopředu a tuto šablonu nasadíte.
