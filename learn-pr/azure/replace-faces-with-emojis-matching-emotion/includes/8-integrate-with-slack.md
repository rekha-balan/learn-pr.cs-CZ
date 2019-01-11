Vytvořili jste všechny požadované Azure Functions a nasadili je! Pojďme propojit Azure Functions se Slackem a vytvořit slackový příkaz lomítko, který zavolá funkci Azure Functions a zobrazí výsledný obrázek s emoji v okně Slacku. 

## <a name="create-a-slack-app"></a>Vytvoření slackové aplikace

Příkaz lomítko existuje jako součást aplikace Slack známé jako Slack bot. 

1. [Vytvořte slackovou aplikaci.](https://api.slack.com/apps/new?azure-portal=true)
2. Zvolte **název aplikace**, přidružte ho k **pracovnímu prostoru**, který jste vytvořili na začátku tohoto modulu, a pak vyberte **Create App** (Vytvořit aplikaci).

    ![Vytvoření slackové aplikace](../media/8.create-slack-app.png)

3. Přejděte do slackové aplikace a vytvořte příkaz lomítko. Vyberte položku nabídky `slash Commands` (Příkazy lomítko).

    ![Obrazovka vytvoření slackové aplikace. Dialogové okno žádá o název aplikace a vývojového pracovního prostoru Slacku.](../media/8.slash-commands.png)

4. Vyberte **Create New Command** (Vytvořit nový příkaz).

    - Do pole **Command** (Příkaz) zadejte libovolný název pro váš příkaz lomítko.
    - Do pole **Request URL** (Adresa URL požadavku) zadejte veřejnou adresu URL aplikace funkcí Azure. Pro náš `mojifier-slack-function-app` použitý v předchozí části bude naše veřejná adresa URL aplikace funkcí `https://mojifier-slack-function-app.azurewebsites.net`. Připojte k adrese URL `/api/RespondToSlackCommand`.
    - Přidejte **Short Description** (Krátký popis) a **Usage Hint** (Nápovědu k použití).

    Vyberte **Save** (Uložit).

    ![Příkaz lomítko](../media/8.create-slash-command.png)

5. Pokud to bylo úspěšné, příkaz lomítko se zobrazí v seznamu **Slash Commands** (Příkazy lomítko).

    ![Příkaz lomítko se zobrazí v seznamu všech příkazů lomítko. V tomto seznamu je pouze mojifier.](../media/8.create-slash-commands-success.png)

6. Aby to fungovalo, bude nutné nainstalovat do pracovního prostoru slackovou aplikaci. V nabídce vyberte **Basic Information** (Základní informace).

7. Rozbalte **Install your app to your workspace** (Instalace aplikace do pracovního prostoru) a vyberte **Install app to workspace** (Nainstalovat aplikaci do pracovního prostoru).

   ![Možnost Install app to workspace najdete v nabídce Building apps for Slack (Vytváření aplikací pro Slack) na stránce rozhraní Slack API.](../media/8.install-app-to-workspace.png)

8. Pokud se vám zobrazí výzva, abyste **autorizovali** aplikaci, autorizujte ji.

   ![Vkládaná obrazovka pro ověření aplikace do pracovního prostoru je zobrazena buď s možnostmi pro autorizaci, nebo zrušení.](../media/8.authenticate-slack-app.png)

## <a name="try-it-out"></a>Vyzkoušení

Vytvořili jste a propojili příkaz lomítko do nasazené služby Azure Functions. Teď ho můžete otestovat.

1. Otevřete svůj pracovní prostor Slacku.
2. Otevřete okno chatu a zadejte `/mojify`. Pokud pojmenujete aplikaci jinak, zadejte místo toho příslušný příkaz.
3. Pokud všechno funguje správně, měla by se zobrazit možnost `mojify`.

   ![Nabídka příkazu lomítko se zobrazila nad textem. Název aplikace se zobrazuje jako Mojifier a příkaz jako „/mojify images of people's faces“ s popisem, který uvádí, že nahrazuje obličeje lidí za emoji.](../media/8.slack-check-mojify.png)

4. Zadejte `/mojify` a přidejte adresu URL obrázku.

   ![Slackový příkaz je spuštěný a pro uvedenou adresu URL obrázku provedl náhradu. Emoji se zobrazuje přímo před obličejem člověka na obrázku.](../media/8.slack-type-mojify.png)
