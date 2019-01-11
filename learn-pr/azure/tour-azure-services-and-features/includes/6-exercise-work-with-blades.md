V této lekci se naučíte pracovat s okny v uživatelském rozhraní webu Azure Portal. Můžete použít buď svůj vlastní účet (pokud nějaký máte), nebo bezplatný sandbox (izolovaný prostor) Azure.

Pokud máte účet, přihlaste se k webu [Azure Portal](https://portal.azure.com?azure-portal=true) pomocí přihlašovacích údajů svého účtu.

V opačném případě si odkazem výše **aktivujte sandbox Azure** a přihlaste se k webu [Azure Portal pro sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) pomocí stejného účtu, se kterým jste si sandbox aktivovali.

## <a name="working-with-blades"></a>Práce s okny

Po přihlášení k webu Azure Portal ho můžete začít prozkoumávat. V těchto oddílech se seznámíte s prvkem uživatelského rozhraní „okna“ (rámečky v okně portálu Azure), ale nebudete reálně vytvářet žádné prostředky Azure.

1. Začněme ukázkou toho, jak se dá vytvořit prostředek. V levé části portálu klikněte na **Vytvořit prostředek**.

1. Zobrazí se okno s nadpisem **Nový** a na levé straně v něm bude seznam kategorií s popiskem Azure Marketplace. To je něco jako nabídka „Oblíbené kategorie“, ve které uvidíte některé z nejběžnějších kategorií. Pokud chcete, můžete tento seznam rozbalit a zobrazit všechno v okně **Marketplace** – stačí kliknout na odkaz **Zobrazit vše** vedle záhlaví. Pokud tak učiníte, do okna Nový se vrátíte kliknutím na ikonu X v pravém horním rohu všech oken, která jste otevřeli.

1. Výběrem některé z položek v seznamu Azure Marketplace zobrazíte na pravé straně okna **Nový** oblíbené služby pro danou kategorii. Tento seznam je podmnožinou celé řady výpočetních prostředků dostupných pro tuto kategorii. Jako na webu Azure Marketplace můžete i tady kliknout na odkaz **Zobrazit vše** a podívat se na obsáhlejší seznam. Více o tomto seznamu probereme v dalších částech.

1. Po návratu do okna **Nový** klikněte na **Začínáme** a měl by se na pravé straně okna zobrazit seznam obsahující služby, jako je virtuální počítač Windows Serveru 2016, virtuální počítač serveru Ubuntu, SQL Database a tak dále. Většina z těchto položek obsahuje přímo pod názvem odkaz **Kurz pro rychlý start**. Po kliknutí na tento odkaz se otevře na nové kartě prohlížeče dokumentace Microsoftu k rychlému startu pro danou položku, pokud ji potřebujete.

1. Volitelné: Klikněte na **Kurz pro rychlý start** pod položkou **Virtuální počítač s Windows Serverem 2016** a v novém okně prohlížeče si prohlédněte kurzy pro virtuální počítače s Windows. Až všechno dokončíte, zavřete tuto novou kartu a vraťte se na Azure Portal.

## <a name="viewing-resources"></a>Zobrazení prostředků

1. V části Azure Marketplace klikněte na **Compute** (Výpočty). Na pravé straně okna se zobrazí další výpočetní možnosti, jako jsou Red Hat Enterprise, Reserved VM Instances, Web App for Containers a další.

1. Napravo od **Doporučené** klikněte na **Zobrazit vše**. Zobrazí se úplný seznam dostupných služeb virtuálního počítače.

1. V části **Doporučené** klikněte na **Windows Server**. Vpravo se zobrazí okno **Windows Server**.

1. Napravo od ikony **Připnout** v záhlaví okna klikněte na ikonu **Maximalizovat** (vypadá jako jednoduchý prázdný čtvereček nebo políčko). Okno Windows Server teď zaplní celou obrazovku s výjimkou levého podokna. Projděte si seznam a prohlédněte si všechny dostupné image Windows Serveru.

1. Volitelně můžete kliknout na ikonu **Obnovit** (podobná ikoně Maximalizovat, jednoduchý prázdný čtvereček a na něm ještě jeden) a vrátíte se do předchozího zobrazení okna.

1. Kliknutím na symbol **X** v pravém horním rohu zavřete okno **Windows Server**. Nyní byste měli znovu vidět okno **Nový**.

## <a name="filtering-results"></a>Filtrování výsledků

1. Dalším způsobem, jak najít služby, je upřesnění seznamu pomocí filtrů a hledaných výrazů. V okně **Nový** jste si asi všimli vyhledávacího pole v horní části. Je to nejrychlejší způsob, jak filtrovat služby, které chcete zobrazit, ale toto hledání kontroluje každou službu Azure, aby získalo její výsledky. Pokud se chcete dozvědět, jak se filtruje po výběru kategorie, pokračujte k dalším krokům.

1. V okně **Compute** vidíte v horní části tyto různé možnosti, vyhledávací pole a nějaká rozevírací pole s popiskem.

1. Do vyhledávacího pole zadejte `virtual machine images` a stiskněte ENTER. Měli byste vidět filtrovaný seznam výpočetních služeb souvisejících s imagemi virtuálních počítačů.

1. Prozkoumejte dostupná pole s rozevíracími filtry kliknutím na šipku dolů (vypadá trochu jako dvojitá šipka) a vyzkoušejte některé z nich. Pokud jste spokojeni, přejděte k dalšímu kroku.

1. Klikněte na **X** na pravém konci vyhledávacího pole. Tím se vymaže hledaný výraz, ale neobnoví se žádné rozevírací filtry, které jste nastavili. Můžete je buď obnovit ručně nebo jednoduše zavřít okno **Compute** ikonou X v pravém horním rohu a znovu ho otevřít. Až si vyzkoušíte možnosti vyhledávání a filtrování, přejděte k dalšímu kroku.

1. Kliknutím na **X** v pravém horním rohu zavřete okno **Compute**.

1. Kliknutím na **X** v pravém horním rohu zavřete okno **Marketplace**. Teď znovu uvidíte okno **Nový**.

1. Kliknutím na **X** v pravém horním rohu zavřete okno **Nový**.

Zobrazí se výchozí řídicí panel.

## <a name="summary"></a>Shrnutí

V této lekci jste se připojili k webu Azure Portal, přihlásili se a zjistili, jak okna zobrazují informace uživatelského rozhraní. Jako příklad jste použili okno Nový. Řadu zásad, co jste se naučili, uplatníte v rámci celého uživatelského rozhraní webu Azure Portal. V dalším cvičení si projdete a nakonfigurujete další nastavení v Azure.
