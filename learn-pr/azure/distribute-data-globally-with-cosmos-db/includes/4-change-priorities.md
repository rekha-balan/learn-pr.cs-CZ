Když replikujete data do několika oblastí, můžete využít řešení automatického převzetí služeb při selhání, které poskytuje služba Azure Cosmos DB. Automatické převzetí služeb při selhání je funkce, která připadá v úvahu, když dojde k havárii nebo jiné události, která převede jednu z vašich oblastí čtení nebo zápisu do režimu offline a přesměrovává požadavky z offline oblasti do další oblasti s nejvyšší prioritou. 

Pro váš online web s oblečením, který jste právě replikovali do oblastí Západní USA 2, Východní USA a Japonsko – východ, můžete určit priority tak, že pokud Východní USA přejde do offline režimu, můžete přesměrovat čtení do oblasti Západní USA 2 místo do oblasti Japonsko – východ, aby se omezila latence.

V této lekci získáte informace o tom, jak převzetí služeb při selhání funguje a jak nastavit prioritu pro oblasti, ve kterých se replikovala data vaší společnosti.

## <a name="failover-basics"></a>Základní informace o převzetí služeb při selhání

Ve výjimečném případě regionálního výpadku Azure nebo výpadku datového centra služba Azure Cosmos DB automaticky aktivuje převzetí služeb všech účtů Azure Cosmos DB přítomných při selhání v ovlivněné oblasti.

**Co se stane, když má oblast čtení výpadek?**

Účty Azure Cosmos DB s oblastí čtení v jedné z ovlivněných oblastí se automaticky odpojí od oblasti zápisu a označí se jako offline. Sady Cosmos DB SDK implementují protokol zjišťování oblastí, který jim umožňuje automaticky zjistit, že je oblast k dispozici, a přesměrovávat volání čtení na další dostupnou oblast v seznamu upřednostňovaných oblastí. Pokud není žádná oblast ze seznamu upřednostňovaných oblastí k dispozici, volání se automaticky vrátí na aktuální oblast zápisu. K řešení regionálních převzetí služeb při selhání není nutné v kódu aplikace nic měnit. V celém tomto procesu je zaručené, že Azure Cosmos DB dodrží konzistenci.

Jakmile se ovlivněná oblast zotaví z výpadku, Azure Cosmos DB automaticky obnoví všechny své účty v dané oblasti. Účty Azure Cosmos DB, které mají oblast čtení v ovlivněné oblasti, se pak automaticky synchronizují s aktuální oblastí zápisu a převedou se do režimu online. Sady Azure Cosmos DB SDK zjišťují dostupnost nové oblasti a vyhodnocují, jestli se oblast má vybrat jako aktuální oblast čtení podle upřednostňovaného seznamu oblastí nakonfigurovaného danou aplikací. Následná čtení se přesměrují na zotavenou oblast bez toho, aby se musel nějak změnit kód aplikace.

**Co se stane, když má oblast zápisu výpadek?**

Pokud je ovlivněnou oblastí aktuální oblast zápisu a pro účty Azure Cosmos DB je povolené automatické převzetí služeb při selhání, oblast se automaticky označí jako offline. Pak se pro ovlivněný účet Azure Cosmos DB automaticky povýší alternativní oblast na oblast zápisu.

Během automatických převzetí služeb při selhání služba Azure Cosmos DB automaticky volí další oblast zápisu pro daný účet Azure Cosmos DB podle zadaného pořadí priorit. Aplikace můžou pomocí vlastnosti WriteEndpoint třídy DocumentClient zjišťovat změny v oblasti zápisu.

Jakmile se ovlivněná oblast zotaví z výpadku, Cosmos DB automaticky obnoví všechny své účty v dané oblasti.

Teď pojďme upravit oblast čtení pro vaši databázi.

## <a name="set-read-region-priorities"></a>Nastavení priorit oblasti čtení

1. Na portálu Azure Portal na obrazovce **Globální replikace dat** klikněte na **Automatické převzetí služeb při selhání**. Automatické převzetí služeb při selhání se povolí jen v případě, že se databáze už replikovala do více než jedné oblasti.
2. Na obrazovce **Automatické převzetí služeb při selhání** změňte možnost **Povolit automatické převzetí služeb při selhání** na **Zapnuto**.
3. V části **Oblasti čtení** klikněte do levé části řádku **Východní USA** a přetáhněte řádek na nejvyšší pozici.
4. Klikněte do levé části řádku **Východní Japonsko** a přetáhněte ho na druhou pozici.
5. Klikněte na **OK**.

    ![Změna oblasti čtení na portálu](../media/4-change-priorities/change-read-priorities.gif)

## <a name="summary"></a>Shrnutí

V této lekci jste zjistili, co nabízí automatické převzetí služeb při selhání, jak ho můžete využít k ochraně před nepředvídatelnými výpadky a jak upravit priority oblastí čtení.