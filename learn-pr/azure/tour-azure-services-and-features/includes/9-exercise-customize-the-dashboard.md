Řídicí panely jsou flexibilním nástrojem pro správu různých aspektů služeb Azure prostřednictvím webu Azure Portal. Nabízejí praktický způsob, jak monitorovat stav služeb. Protože se dají sdílet, pomohou vám zajistit, aby všichni členové vašeho týmu viděli stejná data a měli přehled o stavu kritických komponent. Pojďme vytvořit nový řídicí panel a přidat do něj nějaké dlaždice.

## <a name="create-a-new-dashboard"></a>Vytvoření nového řídicího panelu

1. V levém navigačním panelu webu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) vyberte **Řídicí panel** a v horní části klikněte na tlačítko **Nový řídicí panel**.

1. V poli s textem **Můj řídicí panel** změňte název na **Customer Dashboard**.

## <a name="add-and-configure-the-clock-tile"></a>Přidání a konfigurace dlaždice hodin

1. Přetáhněte hodiny z galerie dlaždic do pracovního prostoru. Umístěte je do dostupného prostoru v pravém horním rohu.

1. V okně **Upravit hodiny** změňte umístění na **Tichomoří (USA a Kanada)**.

1. U položky **Formát času** klikněte na **24 hodin**.

1. Klikněte na **Hotovo**.

1. Zopakujte předchozí čtyři kroky, ale jako umístění vyberte **Východ (USA a Kanada)**. Teď byste měli mít dvoje hodiny, z nichž jedny ukazují čas na západním pobřeží a druhé na východním pobřeží.

## <a name="resize-a-tile"></a>Změna velikosti dlaždice

1. V podokně **Galerie dlaždic** klikněte na **Všechny prostředky** a potom přetáhněte požadovanou dlaždici do levého horního rohu pracovního prostoru nového řídicího panelu.

1. Klikněte na dlaždici, klikněte pravým tlačítkem na tři tečky a potom klikněte na **6 x 6**.

1. Klikněte na šedý roh v pravém dolním rohu dlaždice a změňte velikost dlaždice na 3,5 svisle krát 6 vodorovně. Všimněte si, že jakmile se změnou velikosti skončíte, dlaždice se změní na 4 × 6.

1. V galerii dlaždic klikněte na dlaždici **Skupiny prostředků** a přetáhněte ji do pracovního prostoru. Umístěte ji pod dlaždici **Všechny prostředky**.

1. V galerii dlaždic klikněte na dlaždici **Service Health** a přetáhněte ji do pracovního prostoru. Umístěte ji vpravo vedle dlaždice **Všechny prostředky**.

1. Pokračujte přidáním následujících dlaždic a měňte jejich velikost tak, aby se vešly do prostoru:

    - Nápověda a podpora
    - Rychlé úkoly
    - Marketplace
    - Co je nového

1. Po přidání těchto dlaždic klikněte na **Přizpůsobení dokončeno**. Měl by se zobrazit řídicí panel **Customer Dashboard**.

## <a name="clone-a-dashboard"></a>Klonování řídicího panelu

Pro některé další zákazníky chcete vytvořit velmi podobný řídicí panel.

1. Klikněte na tlačítko **Klonovat**.

1. Přejmenujte řídicí panel z **Klon řídicího panelu Customer Dashboard** na **Azure AD Admin Dashboard**.

1. Na dlaždici **Skupiny prostředků** klikněte na ikonu odpadkového koše a tuto dlaždici odstraňte.

1. Z galerie dlaždic přidejte tyto dlaždice:

    - Identita organizace
    - Uživatelé a skupiny
    - Shrnutí aktivity uživatele
    - Vítejte v centru pro správu Azure AD

1. Podle potřeby dlaždice přemístěte a potom klikněte na **Přizpůsobení dokončeno**.

## <a name="share-a-dashboard"></a>Sdílení řídicího panelu

Teď chcete řídicí panel zpřístupnit jiným uživatelům. Za tímto účelem proveďte následující kroky:

1. Na řídicím panelu Azure AD Admin Dashboard klikněte v horní části na tlačítko **Sdílet**.

1. Na panelu **Sdílení a řízení přístupu**, který se zobrazí, zrušte zaškrtnutí políčka **Publikovat do skupiny prostředků dashboards** a v rozevíracím seznamu **Skupina prostředků** zvolte skupinu prostředků <rgn>[název skupiny prostředků sandboxu]</rgn>.

1. Klikněte na **Publikovat** a pak okno **Sdílení a řízení přístupu** zavřete.

1. Pomocí rozevíracího seznamu v horní části přejděte na řídicí panel **Customer Dashboard**.

    Všimněte si, že se na dlaždici **Všechny prostředky** objevil sdílený prostředek řídicího panelu.

1. Pokud chcete sdílet řídicí panel Customer Dashboard, zopakujte kroky 1 až 3.

## <a name="edit-a-dashboardjson-file"></a>Úprava souboru JSON řídicího panelu

Pokud se chcete dozvědět, jak stáhnout a upravit soubor řídicího panelu, postupujte následovně:

1. Klikněte na **Stáhnout**.

1. Otevřete Průzkumníka Windows a přejděte do složky Stažené soubory.

1. Najděte soubor *Customer Dashboard.json* a dvakrát na něj klikněte.

1. V editoru souborů vyhledejte text *ClockPart*.

1. U prvního výskytu položky ClockPart změňte předchozí hodnotu **rowSpan** na 1.

1. U druhého výskytu položky ClockPart také změňte předchozí hodnotu **rowSpan** na 1.

1. Při druhém výskytu položky ClockPart změňte hodnotu Y z 2 na 1.

1. Soubor *Customer Dashboard.json* uložte a zavřete editor kódu.

1. Na řídicím panelu Azure klikněte na **Nahrát**.

1. V dialogovém okně **Otevřít** přejděte do složky Stažené soubory a dvakrát klikněte na soubor *Customer Dashboard.json*.

    Všimněte si, že se velikost hodin změnila na výšku jednoho řádku a spodní hodiny se posunuly o řádek výš.

## <a name="select-a-shared-dashboard"></a>Výběr sdíleného řídicího panelu

Uvědomili jste si, že se vám menší hodiny nelíbí, a chcete se vrátit k dřívější sdílené verzi řídicího panelu Customer Dashboard? Provést to můžete buď tak, že upravíte soubor a znovu ho nahrajete, nebo získáte přístup ke sdílené verzi. Za tímto účelem proveďte následující kroky:

1. Klikněte na šipku dolů vedle řídicího panelu **Customer Dashboard**.

1. Klikněte na **Procházet všechny řídicí panely**.

1. V okně **Všechny řídicí panely** v části **TYP** vyberte **Sdílené řídicí panely**.

1. Klikněte na řídicí panel **Customer Dashboard**.

1. Zavřete okno **Všechny řídicí panely**.

    Všimněte si, že se obnovila původní velikost hodin.

## <a name="switch-to-full-screen"></a>Přepnutí na celou obrazovku

1. Klikněte na šipku dolů vedle řídicího panelu **Customer Dashboard**. 

    Všimněte si, že existuje ještě druhý řídicí panel Customer Dashboard, tentokrát neoznačený symbolem sdílení. Klikněte na tuto verzi řídicího panelu Customer Dashboard a hodiny se zase zmenší.

1. Přepněte zpátky na sdílený řídicí panel Customer Dashboard.

1. Klikněte na tlačítko **Celá obrazovka**. 

    Všimněte si, že všechny nabídky a panely prohlížeče zmizely.

1. Kliknutím na **Ukončit režim celé obrazovky** se vrátíte na běžnou obrazovku.

## <a name="unshare-a-dashboard"></a>Zrušení sdílení řídicího panelu

Pokud chcete zařídit, aby už sdílený řídicí panel nebylo možné vybrat, můžete _zrušit jeho sdílení_. Pokud chcete sdílení řídicího panelu zrušit, proveďte následující kroky:

1. Klikněte na tlačítko **Zrušit sdílení**. Zobrazí se okno **Sdílení a řízení přístupu**.

1. Klikněte na tlačítko **Zrušit publikování**.

1. V potvrzovacím okně klikněte na **OK**.

1. Klikněte na šipku dolů vedle řídicího panelu **Customer Dashboard**.

1. Klikněte na **Procházet všechny řídicí panely**.

1. V okně **Všechny řídicí panely** v části **TYP** vyberte **Sdílené řídicí panely**.

    Všimněte si, že se řídicí panel **Customer Dashboard** už nezobrazuje v seznamu dostupných řídicích panelů.

1. Zavřete okno **Všechny řídicí panely**.

## <a name="delete-a-dashboard"></a>Odstranění řídicího panelu

1. Ujistěte se, že je řídicí panel **Azure AD Admin** vybraný.

1. Klikněte na tlačítko **Odstranit**.

1. V okně zprávy **Potvrzení** potvrďte zaškrtnutím políčka, že tento řídicí panel už nebude viditelný, a pak klikněte na **OK**.

## <a name="reset-a-dashboard"></a>Resetování řídicího panelu

1. Ujistěte se, že je řídicí panel **Customer Dashboard** vybraný.

1. Klikněte na **Upravit**.

1. Klikněte pravým tlačítkem na pracovní prostor a klikněte na **Resetovat do výchozího stavu**.

1. V okně zprávy **Resetovat řídicí panel do výchozího stavu** klikněte na **Ano**.

    Všimněte si, že se na řídicím panelu Customer Dashboard obnovily výchozí dlaždice.

1. Klikněte na **Přizpůsobení dokončeno**.

1. Klikněte na své jméno v pravém horním rohu portálu.

1. Klikněte na **Odhlásit**.

1. Zavřete prohlížeč.

## <a name="summary"></a>Shrnutí

Vytvořili jste řídicí panely, upravili je, sdíleli, změnili je pomocí souborů **JSON**, zrušili jejich sdílení a nakonec jste je resetovali do výchozího stavu. Teď už určitě chápete, jak výkonné nástroje řídicí panely představují a jak je můžete použít k vytváření efektivních rozhraní pro různé role v organizaci.
