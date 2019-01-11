> [!NOTE]
> Po spuštění testovacího prostředí budete potřebovat uživatelské jméno a heslo, která se nacházejí na kartě **Prostředky** vedle pokynů.

Ve společnosti First Up Consultants vám byl udělen přístup ke skupině prostředků pro marketingový tým. Chcete se seznámit s portálem Azure Portal a podívat se, jaké role jsou aktuálně přiřazené.

## <a name="list-role-assignments-for-yourself"></a>Výpis rolí přiřazených vám

Následujícím postupem si zobrazíte role, které jsou aktuálně přiřazené vám.

1. V pravém horním rohu webu Azure Portal klikněte na svoje uživatelské jméno a otevřete nabídku.

1. Zkontrolujte, že jste přihlášeni jako **LabAdmin-_XXXXXXX_**. Pokud ne, odhlaste se a přihlaste se pomocí uživatelského jména a hesla na kartě **Prostředky**.

1. Kliknutím na tlačítko se třemi tečkami (**...**) si zobrazte další odkazy.

    ![Snímek obrazovky s nabídkou se zvýrazněnou položkou Moje oprávnění](../media/4-my-permissions-menu.png)

1. Kliknutím na **Moje oprávnění** otevřete podokno Moje oprávnění.

    ![Snímek obrazovky s podoknem Moje oprávnění](../media/4-my-permissions-pane.png)

    V podokně Moje oprávnění se zobrazují role, které vám byly přiřazeny, a obor. Váš výpis bude vypadat jinak.

## <a name="list-role-assignments-for-a-resource-group"></a>Výpis přiřazení rolí pro skupinu prostředků

Následujícím postupem si zobrazíte role, které jsou přiřazené v oboru skupiny prostředků.

1. V navigačním seznamu klikněte na **Skupiny prostředků**.

   ![Snímek obrazovky webu Azure Portal znázorňující okno Skupiny prostředků](../media/4-resource-groups.png)

1. Vyhledejte skupinu prostředků s názvem **FirstUpConsultantsRG1-_XXXXXXX_** a klikněte na ni.

1. Klikněte na **Řízení přístupu (IAM)**.

   ![Snímek obrazovky znázorňující umístění možnosti Řízení přístupu (IAM) v okně vybrané skupiny prostředků](../media/4-resource-group-access-control.png)

1. Klikněte na kartu **Přiřazení rolí**.

    Můžete vidět, kdo má přístup k této skupině prostředků. Všimněte si, že některé role mají obor nastavený na **Tento prostředek**, zatímco jiné mají nastavení **(zděděno)** z nadřazeného oboru.

   ![Snímek obrazovky znázorňující řízení přístupu pro zvolenou skupinu prostředků s vybranou kartou Přiřazení rolí](../media/4-resource-group-role-assignment.png)

## <a name="list-roles"></a>Výpis rolí

Jak jste se dozvěděli v předchozí jednotce, role je kolekce oprávnění. Azure má více než 70 předdefinovaných rolí, které můžete použít ve svých přiřazeních rolí. Výpis rolí zobrazíte následujícím postupem.

- V horní části podokna klikněte na kartu **Role** – tím si zobrazíte seznam všech předdefinovaných a vlastních rolí.

   Vidíte i počet uživatelů a skupin přiřazených k jednotlivým rolím.

   ![Snímek obrazovky zobrazující seznam rolí a uživatele a skupiny přiřazené k jednotlivým rolím](../media/4-roles-list.png)

V této lekci jste se dozvěděli, jak si přes Azure Portal zobrazit výpis rolí, které máte přiřazené. Také jste zjistili, jak si zobrazit výpis přiřazení rolí pro skupinu prostředků.