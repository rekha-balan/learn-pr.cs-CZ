Představte si, že vás správce zabezpečení vaší společnosti upozorní, že ve vaší síti bylo zjištěno potenciální porušení zabezpečení. Je tu podezření, že neautorizovaný uživatel mohl prostřednictvím škodlivých aktivit získat přístup k vaší databázi. Jak byste to ověřili? Víte, že podezřelé aktivity v databázi je nutné aktivně monitorovat, ale co můžete udělat, abyste nejen získali přehled o tom, co se děje ve vaší databázi, ale abyste výskytu škodlivých aktivit také zabránili?

Azure SQL Database obsahuje integrované funkce, které vám pomůžou sledovat, co se v databázi děje, budou provádět monitorování a v případě zjištění škodlivé aktivity vás na ni upozorní.

## <a name="azure-sql-database-auditing"></a>Auditování Azure SQL Database

Po povolení auditování se operace, ke kterým v databázi dochází, ukládají pro pozdější kontrolu nebo pro analýzu automatizovanými nástroji. Auditování se také používá pro správu dodržování předpisů nebo pochopení způsobu používání vaší databáze. Auditování se dále vyžaduje, pokud si v databázi Azure SQL přejete používat detekci hrozeb Azure.

Protokoly auditu se zapisují do doplňovacích objektů blob v účtu úložiště objektů blob v Azure, který určíte. Zásady auditu lze používat na úrovni serveru nebo na úrovni databáze. Po povolení můžete protokoly zobrazovat na webu Azure Portal nebo je odesílat do Log Analytics nebo do centra událostí pro účely dalšího zpracování a analýzy.

Podívejte se na postup nastavení auditování ve vašem systému.

1. Pomocí stejného účtu, kterým jste aktivovali sandbox, se přihlaste k webu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Na panelu hledání v horní části portálu vyhledejte **server<12345>** a pak tento server na portálu vyberte.

1. V nabídce vlevo v části **Zabezpečení** vyberte možnost **Auditování**.

1. Ve výchozím nastavení je auditování vypnuté. Pokud ho chcete na databázovém serveru povolit, klepněte na tlačítko **ZAPNUTO**.

1. Po výběru tlačítka ZAPNUTO zaškrtněte políčko **Úložiště** a pak klikněte na **Podrobnosti o úložišti**, abyste mohli definovat účet úložiště.

1. V dialogovém okně **Nastavení úložiště** můžete vybrat stávající účet úložiště nebo můžete pro ukládání auditů vytvořit nový účet úložiště. Účet úložiště musí být nakonfigurovaný na použití stejné oblasti jako váš server. V tomto případě budeme definovat nový účet úložiště. Klikněte na **Účet úložiště**. Otevře se dialogové okno **Vytvořit účet úložiště**. Dejte účtu úložiště název `server<12345>auditing`, přičemž `<12345>` nahraďte číslem z názvu vašeho logického serveru. U ostatních možností ponechejte jejich výchozí nastavení a vyberte **OK**. V dialogovém okně **Nastavení úložiště** ponechejte výchozí nastavení a klikněte na **OK**.

1. Kliknutím na tlačítko **Uložit** na panelu nástrojů uložíte změny a zapnete auditování na vašem databázovém serveru.

Teď vygenerujeme nějaké záznamy auditu a podíváme se, co můžete očekávat.

1. Přihlaste se znovu k databázi jako uživatel _ApplicationUser_.

    ```bash
    sqlcmd -S tcp:server<12345>.database.windows.net,1433 -d marketplaceDb -U ApplicationUser -P <password> -N -l 30
    ```

1. Spusťte následující dotaz.

    ```sql
    SELECT FirstName, LastName, EmailAddress, Phone FROM SalesLT.Customer;
    GO
    ```

1. Na portálu na vašem SQL serveru v nabídce vlevo vyberte **Databáze SQL** a vyberte databázi _marketplace_.

1. V nabídce vlevo v databázi _marketplace_ v části **Zabezpečení** vyberte **Auditování**.

1. Povolili jsme auditování na úrovni serveru, takže byste tady měli vidět, že je povolené. V horní nabídce vyberte **Zobrazit protokoly auditování**, aby se zobrazily protokoly.

1. Měl by se zobrazit jeden nebo více záznamů auditu, ve kterých **HLAVNÍ NÁZEV** je _ApplicationUser_ a **TYP UDÁLOSTI** je **BATCH COMPLETED** (Dávka dokončena). Jeden z nich by měl obsahovat podrobnosti dotazu, který jste právě spustili. Můžou se zobrazovat i další události, například úspěšná a neúspěšná ověření. Výběrem libovolného záznamu zobrazíte úplné podrobnosti příslušné události.

![Příklad znázorňující událost v protokolu auditu](../media/5-audit-log.png)

Tyto akce nakonfigurují audity na úrovni databázového serveru a budou platit pro všechny databáze na daném serveru. Auditování můžete nakonfigurovat i na úrovni databáze.

Podívejme se na další funkci, která tyto protokoly využívá ke zvýšení zabezpečení databáze.

## <a name="advanced-threat-protection-for-azure-sql-database"></a>Advanced Threat Protection pro Azure SQL Database

Systém Advanced Threat Protection analyzuje protokoly auditů, aby vyhledal potenciální problémy a hrozby ve vaší databázi. Zahrnuje funkce pro zjišťování a klasifikaci citlivých dat, odhalování a omezování možných ohrožení zabezpečení databáze a detekci neobvyklých aktivit, které by pro vaši databázi mohly znamenat hrozbu. Poskytuje centrální místo pro povolování a správu těchto možností. Rozšířenou ochranu před internetovými útoky můžete jedním kliknutím povolit pro celý databázový server tak, aby se používala pro všechny databáze na daném serveru.

### <a name="setup-and-configuration"></a>Nastavení a konfigurace

Povolme službu Advanced Threat Protection pro naši databázi. Advanced Threat Protection je nastavení na úrovni serveru, takže začneme odtud.

1. Na portálu přejděte na váš SQL server. Na panelu hledání v horní části portálu vyhledejte **server<12345>** a pak tento server vyberte.

1. V nabídce vlevo v části **Zabezpečení** vyberte možnost **Advanced Threat Protection**.

1. Kliknutím na tlačítko **ZAPNUTO** povolte službu Advanced Threat Protection.

1. Volitelně můžete definovat seznam e-mailových adres oddělených středníkem, na který se mají doručovat e-maily s oznámeními. Ve výchozím nastavení je vybraná možnost **E-mailová služba a spolusprávci**, aby se hrozby odesílaly správcům služby.

1. Volitelně můžete určit účet úložiště pro protokolování historie podezřelých dotazů, u kterých se zobrazily výstrahy. K povolení služby Advanced Threat Protection se to nevyžaduje, ale může to být užitečné pro sledování historie. Kliknutím na **Podrobnosti o úložišti** otevřete dialogové okno **Nastavení úložiště**.

1. V dialogovém okně **Výběr účtu úložiště** vyberte účet úložiště **server<12345>auditing**, který jste vytvořili v předchozí části. Ostatní výchozí nastavení ponechejte a klikněte na **OK**.

1. Nakonec vyberte **Typy detekce hrozeb** a krátce se na ně podívejte. Upřednostňovaná možnost je Vše, což je výchozí nastavení.

    Možnost **Vše** představuje následující hodnoty:
    - Hlášení o útocích prostřednictvím injektáže SQL
    - Hlášení o pravděpodobném ohrožení zabezpečení prostřednictvím injektáže SQL
    - Neobvyklá a nestandardní přihlášení klientů, která by mohla představovat hrozbu (například kvůli potenciálnímu útočníkovi získávajícímu přístup)

1. Kliknutím na tlačítko **Uložit** použijete tyto změny a povolíte službu Advanced Threat Protection na vašem serveru.

Je tu ještě jedno nastavení na úrovni databáze, které budeme chtít povolit, a to je Posouzení ohrožení zabezpečení.

1. Přejděte do databáze *marketplace*. Na panelu hledání v horní části portálu vyhledejte **marketplace** a pak tuto databázi na portálu vyberte.

1. V nabídce vlevo v části **Zabezpečení** vyberte možnost **Advanced Threat Protection**.

1. V poli **Posouzení ohrožení zabezpečení** by se měla zobrazit zpráva „Klikněte a nakonfigurujte účet úložiště pro ukládání výsledků skenování“. K povolení této funkce je potřeba provést uvedenou akci, a proto pokračujte touto konfigurací.

1. V dialogovém okně **Výběr účtu úložiště** vyberte účet úložiště **server<12345>auditing**, který jste vytvořili v předchozí části.

1. Můžete také zapnout **pravidelné opakované kontroly** a nakonfigurovat Posouzení ohrožení zabezpečení tak, aby se spouštěly automatické kontroly jednou týdně. Souhrn výsledků kontrol se odešle na e-mailové adresy, které zadáte. V tomto případě zde ponecháme nastavení **VYPNUTO**. Kliknutím nahoře na **Uložit** toto nastavení uložíte a povolíte Posouzení ohrožení zabezpečení.

1. Na hlavním panelu Posouzení ohrožení zabezpečení klikněte na **Naskenovaný soubor** a spusťte kontrolu vaší databáze. Po dokončení se zobrazí doporučení pro zvýšení zabezpečení vaší databáze.

Po povolení služby Advanced Threat Protection se budou zobrazovat podrobnosti a výsledky na úrovni databáze.

Při zjištění ohrožení zabezpečení budete dostávat oznámení. E-mail bude obsahovat stručné informace o tom, co se stalo a jaké akce je třeba provést.

![Příklad upozornění ze služby Advanced Threat Protection](../media/5-email-with-warning.png)

### <a name="data-discovery--classification"></a>Zjišťování a klasifikace dat

Klikněte na panel **Zjišťování a klasifikace dat**.

Panel Zjišťování a klasifikace dat ukazuje sloupce v tabulkách, které mají být chráněny. Některé z těchto sloupců mohou obsahovat citlivé informace nebo mohou být v některých zemích nebo oblastech považovány za klasifikované.

![Zjišťování a klasifikace dat](../media/5-data-discovery-and-classification.png)

Pokud je potřeba pro některé ze sloupců nakonfigurovat ochranu, zobrazí se zpráva. Tato zpráva by měla mít zhruba tento formát: *Zjistili jsme 10 sloupců s doporučeními klasifikace*. Kliknutím na text můžete tato doporučení zobrazit.

Kliknutím na znaménko zaškrtnutí vedle sloupce nebo výběrem zaškrtávacího políčka vlevo od záhlaví schématu vyberte sloupce, které chcete klasifikovat. Výběrem možnosti Přijmout vybraná doporučení tato doporučení klasifikace použijete.

Pak upravte sloupce a pak definujte typ informací a označení citlivosti pro databázi. Kliknutím na tlačítko Uložit změny uložte.

Když se vám podaří doporučení úspěšně zvládnout, neměla by se zobrazovat žádná aktivní doporučení.

### <a name="vulnerability-assessment"></a>Posouzení ohrožení zabezpečení

Klikněte na panel **Posouzení ohrožení zabezpečení**.

![Řídicí panel Posouzení ohrožení zabezpečení](../media/5-vulnerability-assessment-dashboard.png)

Posouzení ohrožení zabezpečení obsahuje seznam problémů s konfigurací vaší databáze a související rizika. Například na výše uvedeném obrázku můžete vidět, že je potřeba nastavit bránu firewall na úrovni serveru.

Po kliknutí na panel Posouzení ohrožení zabezpečení si můžete prohlédnout úplný seznam ohrožení zabezpečení. Tady můžete kliknout na každé jednotlivé ohrožení zabezpečení.

Na stránce ohrožení zabezpečení se zobrazí podrobnosti, jako je například úroveň rizika, které databáze se problém týká, popis ohrožení zabezpečení a doporučený postup k odstranění problému. Proveďte nápravu, kterou problém nebo problémy vyřešíte. Ujistěte se, že jste vyřešili všechna ohrožení zabezpečení.

### <a name="threat-detection"></a>Detekce hrozeb

Klikněte na panel **Detekce hrozeb**.

Tento panel zobrazuje seznam zjištěných hrozeb. V tomto seznamu můžete například vidět jeden potenciální útok prostřednictvím injektáže SQL.

![Detekce hrozeb](../media/5-threat-detection-dashboard.png)

 Případné problémy vyřešte podle doporučení. U problémů, jako jsou například upozornění na injektáže SQL, se budete moct podívat na dotaz a zpětně dohledat, kde se v kódu tento dotaz provedl. Jakmile ho najdete, kód přepište, abyste problém odstranili.