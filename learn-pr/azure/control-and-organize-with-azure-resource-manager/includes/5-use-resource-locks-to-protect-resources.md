Váš nadřízený nedávno v rozhovoru zmínil, že se několikrát stalo, že někdo omylem odstranil důležité prostředky Azure. Prostředí Azure nebylo uspořádané a dobrý úmysl vyčistit ho od nepoužívaných prostředků vyústil v nechtěné odstranění. Vy jste zaslechli něco o tom, že v Azure existují zámky prostředků. Řeknete nadřízenému, že ty by podle vás mohly podobným incidentům do budoucna pomoct zabránit. Pojďme se podívat, jak se zámky prostředků k vyřešení takového problému používají.

## <a name="what-are-resource-locks"></a>Co jsou zámky prostředků?

Zámky prostředků jsou nastavení, které se dá uplatnit u libovolného prostředku a zablokovat tak možnost jeho úprav nebo odstranění. Zámek prostředku můžete nastavit na hodnotu **Odstranění** nebo **Jen pro čtení**. Se zámkem odstranění budou u prostředku povoleny všechny operace, ale zablokuje se možnost jeho odstranění. Zámek **Jen pro čtení** umožní provádět s prostředkem jen aktivity čtení a zablokuje veškeré jeho úpravy nebo odstranění. Zámky prostředků se dají použít u předplatných, skupin prostředků i jednotlivých prostředků a pokud se použijí na vyšší úrovni, dědí se.

> [!NOTE]
> Použití zámku **Jen pro čtení** může vést k neočekávaným výsledkům, protože některé operace, které bychom na první pohled pokládali za operace čtení, ve skutečnosti vyžadují další akce. Například umístění zámku Jen pro čtení na účet úložiště zabrání uživatelům v pořizování výpisů klíčů. Operace výpisu klíčů se zpracovává prostřednictvím požadavku POST, protože vrácené klíče jsou k dispozici pro operace zápisu.

Když se použije zámek prostředku, je k provedení této aktivity potřeba ho nejprve odebrat. Vložení dalšího kroku před tím, než se akce u prostředku povolí, pomáhá chránit prostředky před nechtěnými akcemi a správcům zabrání udělat něco, co ve skutečnosti udělat nechtěli. Zámky prostředků platí bez ohledu na přístupová oprávnění na základě rolí. I v případě, že jste vlastníkem prostředku, musíte před provedením blokované aktivity odebrat zámek.

Pojďme se podívat, jak zámek prostředku funguje v akci.

## <a name="create-a-resource-lock"></a>Vytvoření zámku prostředku

Vraťme se k naší skupině prostředků **msftvyuka-zakladni-infrastruktura-sp**. Máme v ní teď dvě virtuální sítě a účet úložiště. Tyto prostředky pokládáme za klíčové součásti našeho prostředí Azure a chceme zajistit, aby je nikdo omylem neodstranil. Použijeme tedy na skupinu prostředků zámek, který zabrání odstranění skupiny i prostředků, které jsou v ní obsažené.

1. Pokud ještě nemáte v prohlížeči otevřený web [Azure Portal](https://portal.azure.com/?azure-portal=true), otevřete ho. Do vyhledávacího pole v horním navigačním panelu zadejte dotaz **msftvyuka-zakladni-infrastruktura-sp** a klikněte na skupinu prostředků.

1. V části **Nastavení** v nabídce vlevo vyberte **Zámky**. Měli byste vidět, že prostředek aktuálně nemá žádný zámek. Pojďme ho přidat.

1. Klikněte na **+ Přidat**. Zámek pojmenujte **BlokovatOdstranění** a jako **Typ zámku** vyberte **Odstranění**. Klikněte na **OK**.

    ![Obrázek portálu znázorňující konfiguraci nového zámku prostředku](../media/5-add-lock.PNG)

    Teď je skupina prostředků opatřena zámkem, který brání jejímu odstranění a zdědí ho také všechny prostředky ve skupině. Zkusme odstranit jednu z virtuálních sítí a uvidíme, co se stane.

1. Přejděte zpátky na **Přehled**a kliknutím na **msftvyuka-vnet1** prostředek zobrazte.

1. V podokně **Přehled** klikněte u sítě **msftvyuka-vnet1** na tlačítko **Odstranit** nahoře a potvrďte odstranění kliknutím na **Ano**. Měla by se zobrazit chyba s informacemi o tom, že prostředek je opatřen zámkem, který brání jeho odstranění.

    ![Obrázek chyby, která ukazuje, že odstranění prostředku je blokováno](../media/5-delete-error.PNG)

1. V části **Nastavení** v nabídce vlevo vyberte **Zámky**. Měli byste vidět, že prostředek **msftvyuka-vnet1** má zámek, který zdědil ze skupiny prostředků.

1. Přejděte zpátky do skupiny prostředků **msftvyuka-zakladni-infrastruktura-sp** a otevřete podokno **Zámky**. Teď pojďme zámek odebrat, abychom mohli systém vyčistit. Klikněte na ikonu **…** u zámku **BlokovatOdstranění** a vyberte **Odstranit**.

## <a name="using-resource-locks-in-practice"></a>Použití zámků prostředků v praxi

Probrali jsme, jak může zámek ochránit prostředky před náhodným odstraněním. Abychom mohli virtuální síť odstranit, museli jsme zámek odebrat. Takto strukturovaná akce pomáhá ujistit se, že daný prostředek opravdu chcete odstranit nebo změnit.

Zámky prostředků využijte k ochraně klíčových součástí Azure, jejichž odebrání nebo změna by mohla mít rozsáhlý dopad. K příkladům patří okruhy ExpressRoute a virtuální sítě, důležité databáze a řadiče domény. Posuďte svoje prostředky a použijte zámky u těch, kde je žádoucí mít další vrstvu ochrany před nechtěnými akcemi.

## <a name="summary"></a>Shrnutí

Viděli jsme, jak můžete využít ve svém prostředí zámky k ochraně důležitých prostředků. Zámky prostředků jsou další nástroj ve vašem arzenálu možností, jak ovládat a uspořádat prostředky Azure.