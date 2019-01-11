Společnost First Up Consultants provádí čtvrtletní revize řízení přístupu na základě role (RBAC) pro účely auditování a odstraňování potíží. Víte, že se změny zaznamenávají do [protokolu aktivit v Azure](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Vedoucí vás vyzval, abyste vygenerovali sestavu změn přiřazení rolí a vlastních rolí za poslední měsíc.

## <a name="view-activity-logs"></a>Zobrazení protokolů aktivit

Nejjednodušší způsob, jak začít, je zobrazit si protokoly aktivit přes Azure Portal.

1. Klikněte na **Všechny služby** a pak vyhledejte **Protokol aktivit**.

    ![Snímek obrazovky webu Azure Portal s umístěním možnosti Protokol aktivit](../media/6-all-services-activity-log.png)

1. Kliknutím na **Protokol aktivit** otevřete protokol aktivit.

    ![Snímek obrazovky webu Azure Portal s protokoly aktivit](../media/6-activity-log-portal.png)

1. Nastavte filtr **Časový rozsah** na **Minulý měsíc**.

1. Přidejte filtr **Operace** zadejte **roli** – tím seznam vyfiltrujete.

1. Vyberte následující operace RBAC:

    - Vytvoření přiřazení role (roleAssignments)
    - Odstranění přiřazení role (roleAssignments)
    - Vytvoření nebo aktualizace definice vlastní role (roleDefinitions)
    - Odstranění definice vlastní role (roleDefinitions)

    ![Snímek obrazovky se seznamem filtru Operace a čtyřmi vybranými filtry](../media/6-operation-filter.png)

    Za okamžik se zobrazí všechny operace s přiřazeními a definicemi rolí za poslední měsíc. K dispozici je také odkaz pro stažení protokolu aktivit do souboru CSV.

1. Kliknutím na některou z operací zobrazíte podrobnosti záznamu aktivit.

    ![Snímek obrazovky s podrobnostmi záznamu aktivit](../media/6-activity-log-details.png)

V této lekci jste zjistili, jak pomocí protokolu aktivit Azure vytvořit seznam změn v RBAC na portálu a vygenerovat jednoduchou sestavu.