Hasičský sbor nedávno přesunul celou svou infrastrukturu do Azure. Používáte řadu virtuálních počítačů, které fungují jako weby nebo plní e-mailové funkce. Dostali jste požadavek udržovat tyto virtuální počítače aktuální, aby na nich byly nejnovější opravy a aktualizace zabezpečení. Rozhodnete se pro všechny virtuální počítače v podniku zavést řešení Update Management. 

V následujícím cvičení zkontrolujete připojení klientů ke službě Log Analytics a naučíte se plánovat nasazení aktualizací.

## <a name="review-agent-connectivity-to-log-analytics"></a>Kontrola připojení agenta ke službě Log Analytics

Na webu Azure Portal proveďte následující kroky, abyste vyhodnotili, jestli funguje připojení agenta ke službě Log Analytics. Nejprve se přihlaste k webu [Azure Portal pro Sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true). Použijte stejný účet, kterým jste aktivovali sandbox.

1. Na portálu v podokně vlevo klikněte na možnost **Virtuální počítače** a pak klikněte na nově vytvořený virtuální počítač.
2. Klikněte na možnost nabídky **Přehled**.
3. Na stránce virtuálního počítače si poznamenejte **veřejnou IP adresu** (viz níže).

![Veřejná IP adresa](../media/5-public-ip-address-edited.png "Veřejná IP adresa")

4. V místním počítači klikněte na ikonu Windows, zadejte **Připojení ke vzdálené ploše** a pak klikněte na aplikaci **Připojení ke vzdálené ploše**.
5. V aplikaci **Připojení ke vzdálené ploše** zadejte veřejnou IP adresu do pole **Počítač** a pak klikněte na **Připojit**.
6. V dialogovém okně **Zadejte své přihlašovací údaje** zadejte heslo, které jste zadali při vytvoření virtuálního počítače, a klikněte na **OK**.
7. V dialogovém okně s upozorněním na certifikát klikněte na **Ano**.
8. Na vzdáleném počítači klikněte na ikonu Windows a pak klikněte na dlaždici **Ovládací panely**.
9. V ovládacích panelech otevřete službu **Microsoft Monitoring Agent** a pak klikněte na kartu **Azure Log Analytics (OMS)**.
10. Všimněte si, že agent zobrazí zprávu, že **Microsoft Monitoring Agent se úspěšně připojil ke službě Log Analytics** (viz níže).

![Microsoft Monitoring Agent](../media/5-microsoft-monitoring-agent.png "Microsoft Monitoring Agent")

10. Kliknutím na **OK** zavřete okno s **vlastnostmi Microsoft Monitoring Agenta**.
11. V okně **Všechny položky Ovládacích panelů** klikněte na **Nástroje pro správu**.
12. V okně **Nástroje pro správu** poklikejte na **Prohlížeč událostí**.
13. Rozbalte položku **Protokoly aplikací a služeb**, klikněte na **Operations Manager** a pak maximalizujte okno **Prohlížeče událostí**.
14. V zobrazení **Operations Manager** klikněte na nadpis sloupce **ID události**, aby se seznam seřadil podle ID událostí.
15. Všimněte si událostí s ID 3000 a 5002. Tyto události znamenají, že se počítač zaregistroval do pracovního prostoru služby Log Analytics a přijímá konfiguraci. Událost s ID 5002 je vidět níže.

![ID události 5002](../media/5-event-id-5002.png "ID události 5002")

16. Zavřete Prohlížeč událostí a všechna další otevřená okna.
17. Ukončete aplikaci Připojení ke vzdálené ploše.

## <a name="schedule-update-deployments"></a>Plánování nasazení aktualizací

Tady se dozvíte, jak naplánovat aktualizace virtuálního počítače.

1. V podokně **MediaWebServer - Update management** klikněte na kartu **Naplánovat nasazení aktualizace**. 
2. Do pole **Název** zadejte **Důležité aktualizace a aktualizace zabezpečení**.
3. V rozevíracím seznamu **Klasifikace aktualizací** zaškrtněte jenom **Důležité aktualizace** a **Aktualizace zabezpečení**.
4. V poli **Nastavení plánu** nastavte v poli **Spuštění** o hodinu vyšší čas.
5. V poli **Opakování** klikněte na **Opakující se**.
5. V poli **Opakovat každých** nakonfigurujte aktualizaci, aby probíhala jednou týdně v neděli (viz níže) a pak klikněte na **OK**.

![Týdenní konfigurace](../media/5-configure-recurring-schedule-edited.png "Týdenní konfigurace")

6. V podokně **Nové nasazení aktualizace** klikněte na **Vytvořit**.