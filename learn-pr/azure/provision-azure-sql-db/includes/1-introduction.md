Správa dat je důležitou součástí všech firem. Relační databáze a konkrétně Microsoft SQL Server patří už desítky let mezi nejběžnější nástroje pro zpracování dat. 

Pokud chceme data spravovat prostřednictvím cloudu, _stačí_, když k hostování vlastních instancí Microsoft SQL Serveru využijeme virtuální počítače Azure. Někdy jde o vhodné řešení, ale Azure nabízí další způsob, který je často mnohem snazší a efektivnější z hlediska nákladů. Azure SQL Database je služba typu platforma jako služba (PaaS), což znamená, že se sami budete muset starat o menší infrastrukturu a údržbu.

Pro lepší porozumění si vezměme tento scénář: Jste vedoucím vývojového týmu v dopravní logistické společnosti s názvem Contoso Transport.

Dopravní průmysl vyžaduje těsnou koordinaci mezi všemi zúčastněnými stranami: plánovači, dispečery, řidiči a dokonce i zákazníky.

Váš současný proces zahrnuje hromady papírových formulářů a hodiny na telefonu, abyste koordinovali zásilky. Zjišťujete, že na formulářích často chybí podpisy a dispečeři jsou často nedostupní. Kvůli těmto zdržením zůstávají řidiči nevyužiti a dochází k následnému zpoždění důležitých zásilek. Spokojenost zákazníků a dlouhodobé obchodní vztahy jsou pro celkový zisk vaší firmy zásadní.

Váš tým se rozhodne přejít od papírových formulářů a telefonických hovorů k digitálním dokumentům a online komunikaci. Přechod do digitální sféry umožní všem koordinovat a sledovat časy zásilek přes webový prohlížeč nebo mobilní aplikaci.

Chcete rychle vyvinout prototyp, abyste ho mohli sdílet se svým týmem. Váš prototyp bude zahrnovat databázi, ve které se budou ukládat informace o řidičích, zákaznících a objednávkách. Prototyp bude sloužit jako základ pro produkční aplikaci. Proto technologická rozhodnutí, která uděláte teď, se promítnou v úrovni služeb poskytovaných vaším týmem.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Proč je Azure SQL Database vhodnou volbou pro provoz relační databáze
- Které možnosti konfigurace a cen jsou u Azure SQL Database k dispozici
- Jak vytvořit Azure SQL Database na portálu
- Jak používat Azure Cloud Shell pro připojení k databázi Azure SQL, přidání tabulky a práci s daty