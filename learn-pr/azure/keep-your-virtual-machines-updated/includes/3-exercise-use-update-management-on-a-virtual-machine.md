 Váš nadřízený chce, abyste nastavili virtuální počítač, který bude sloužit jako webový prostředek místním médiím. Tento virtuální počítač musí mít maximální možnou ochranu, aby se zabránilo neoprávněnému přístupu. Jako součást bezpečnostního profilu chcete do virtuálního počítače implementovat Update Management, aby byl počítač stále aktuální a měl nainstalované nejnovější opravy zabezpečení. 

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Teď vytvoříte nový virtuální počítač, který bude sloužit jako webový server místním médiím.

1. Otevřete [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).
2. V levém navigačním podokně klikněte na **Vytvořit prostředek**.
3. V podokně **Nový** klikněte na **Virtuální počítač s Windows Serverem 2016**.
4. V podokně **Základy** zadejte níže uvedená data. Můžete vybrat vlastní uživatelské jméno a heslo. Způsob předplatného můžete změnit na Bezplatná zkušební verze nebo Průběžné platby. V umístění vyberte **USA – východ**. 
5. Vytvořte novou skupinu prostředků **RFD**.

![Vytvoření základního virtuálního počítače](../media/3-create-mediawebserver-basics-edited.png "Vytvoření základního virtuálního počítače")

6. V poli Velikost klikněte na **Změnit velikost**, zvolte **B2s** a klikněte na **Vybrat**.
7. V oddílu **PRAVIDLA PORTŮ PRO PŘÍCHOZÍ SPOJENÍ** stačí v poli **Veřejné příchozí porty** vybrat **Povolit vybrané porty**. Vyberte HTTP, HTTPS a RDP (viz níže).

![Výběr možnosti Veřejné příchozí porty](../media/3-public-inbound-ports-edited.png "Výběr možnosti Veřejné příchozí porty")

8. Klikněte na **Zkontrolovat + vytvořit** a pak klikněte na **Vytvořit**. Počkejte, až se virtuální počítač vytvoří. Pokud chcete sledovat průběh, klikněte na ikonu zvonku v pravém horním rohu portálu.

## <a name="onboard-update-manager-to-the-vm"></a>Nasazení Update Manageru do virtuálního počítače

Teď na vytvořeném virtuálním počítači aktivujete Update Manager.

1. V levém podokně klikněte na **Virtuální počítače**.
2. V podokně **Virtuální počítače** vyberte ze seznamu virtuální počítač. V tomto příkladu vyberte **MediaWebServer**.
3. V podokně MediaWebServer přejděte v seznamu na **Operace** a pak klikněte na **Správa aktualizací**.
4. V podokně **Správa aktualizací** zkontrolujte, že je vybraný přepínač **Povolit pro tento virtuální počítač**. Všimněte si, že se vytvořil výchozí **pracovní prostor Log Analytics** a **účet služby Automation**. Potvrďte zbývající výchozí hodnoty a klikněte na **Povolit**.
5. V levém horním rohu klikněte na zvonek Oznámení a počkejte, až nasazení skončí.
6. Po dokončení nasazení Update Managementu se zobrazí nabídka Správa aktualizací (viz níže).

![Dokončení nasazení Update Managementu](../media/3-update-management-deployment-complete-edited.png "Dokončení nasazení Update Managementu")

7. Počkejte aspoň 15 minut, až Update Management nakonfiguruje virtuální počítač.
8. Po dokončení konfigurace Update Managementu se zobrazí podokno Správa aktualizací (viz níže).

![Dokončení konfigurace Update Managementu](../media/3-update-management-vm-configured-edited.png "Dokončení konfigurace Update Managementu")

9. Všimněte si, že je dokončená **Kompatibilita**, čítač **Neúspěšná nasazení aktualizací** je nakonfigurovaný a že v tomto příkladu Update Management zjistil, že je pro Windows Server k dispozici kumulativní aktualizace. Napravo od oznámení o kumulativní aktualizaci pod položkou **ODKAZ NA INFORMACE** je odkaz na článek o této kumulativní aktualizaci ve znalostní bázi. 

## <a name="examine-hybrid-worker-groups"></a>Kontrola skupin hybridních pracovních procesů

1. V levém navigačním podokně klikněte na **Všechny prostředky**.
1. V podokně **Všechny prostředky** si prohlédněte sloupec **TYP**, najděte v něm typ prostředku **Účet služby Automation** a pak na něj klikněte.
1. V podokně Účet služby Automation se posuňte k oddílu **Automatizace procesu** a v něm klikněte na **Skupiny Hybrid Worker**.
1. V podokně **Skupiny Hybrid Worker** klikněte na kartu **Skupiny systémových Hybrid Workerů**.
1. Všimněte si vytvořeného virtuálního počítače, který je v seznamu (viz níže). 

![Skupina Hybrid Worker](../media/3-hybrid-worker-group.png "Skupina Hybrid Worker")

