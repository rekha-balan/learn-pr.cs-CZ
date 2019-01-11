Pojďme vytvořit instanci Azure Cache for Redis pro ukládání a vracení často používaných hodnot.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-redis-cache-in-azure"></a>Vytvoření mezipaměti Redis v Azure

1. Pomocí stejného účtu, kterým jste aktivovali sandbox, se přihlaste k webu [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Klikněte na **Vytvořit prostředek**, klikněte na **Databáze** a pak klikněte na **Redis Cache**.

    Na následujícím snímku obrazovky je vidět umístění Redis Cache v různých možnostech databázových prostředků na webu Azure Portal.

    ![Snímek obrazovky s možnostmi databáze na webu Azure Portal se zvýrazněnými možnostmi Vytvořit prostředek, Databáze a Redis Cache](../media/4-create-a-cache-1.png)

### <a name="configure-your-cache"></a>Konfigurace mezipaměti

U mezipaměti použijte následující nastavení.

1. **Název DNS:** Vytváření globálně jedinečných názvů, jako **ContosoSportsApp[nnn]**, kde `[nnn]` nahradí náhodná čísla.

1. **Předplatné:** Vyberte předplatné Concierge.

1. **Skupina prostředků:** Jako skupinu prostředků vyberte <rgn>[název skupiny prostředků sandboxu]</rgn>.

1. **Umístění:** Normálně byste vybrali umístění, které je blízko vašim zákazníkům, v tomto případě je to východní pobřeží USA.

    [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

5. **Cenová úroveň:** Vyberte **Basic C0**. Je to nejnižší úroveň, jakou můžete použít. V ostrém provozu budou aplikace pravděpodobně ukládat více dat a také používat některé funkce typu Premium, jako je clusterování, které vyžaduje výběr vyšší cenové úrovně.

1. Klikněte na **Vytvořit**. Azure za vás vytvoří a nasadí instanci služby Redis Cache.

    > [!IMPORTANT]
    > Prostředky Redis Cache budou obvykle velmi rychle vytvořeny a zobrazeny na webu Azure Portal, ale samotná mezipaměť nebude několik minut k dispozici. Následující kroky ukazují, jak zkontrolovat stav vaší mezipaměti.

## <a name="verify-your-data"></a>Ověření dat

K vydávání příkazů do instance mezipaměti Redis můžete po jejím vytvoření použít funkci **Konzola** na webu Azure Portal.

1. Vyhledejte mezipaměť Redis prostřednictvím rozevíracího okna **Oznámení** po dokončení nasazení nebo výběrem volby **Všechny prostředky** na levém bočním panelu a použitím pole filtru vlevo k výběru instancí Redis Cache. Případně můžete použít vyhledávací pole nahoře a zadat název mezipaměti.

1. Vyberte instanci služby Redis Cache.

1. Zkontrolujte hodnotu pole „Stav“. Mezipaměť není připravena, dokud je ve stavu „Spuštěno“. Než budete moci pokračovat, budete muset počkat několik minut.

1. Po spuštění do mezipaměti, klikněte na tlačítko `>_ Console` na panelu nástrojů **Přehled** pro vaši službu Redis Cache. Otevře se konzola Redis, která vám umožňuje zadat příkazy Redis nízké úrovně. Vyzkoušejte některé z následujících možností:

    ```console
    ping
    ```

    ```output
    PONG
    ```

    ```console
    set test one
    ```

    ```output
    OK
    ```

    ```console
    get test
    ```

    ```output
    "one"
    ```

Přepněte zpět panel **Přehled** buď pomocí navigačního panelu s popisem cesty v horní části panelu nebo pomocí posuvníku k posunutí zobrazení zpět na levou stranu.

## <a name="retrieve-the-access-keys-and-host-name"></a>Načtení přístupových klíčů a názvu hostitele

1. Vyberte **Nastavení** > **Přístupové klíče**.

1. Zkopírujte **Primární připojovací řetězec (StackExchange.Redis)** na bezpečné místo. Budete ho potřebovat v dalším cvičení.

    Tento klíč obsahuje váš primární klíč a název hostitele. Jde o kompletní připojovací řetězec, který můžete použít v nastavení vaší aplikace pro balíček **StackExchange.Redis**, který budeme používat.

V další části se naučíte některé příkazy, které můžete použít k dotazování se do mezipaměti.