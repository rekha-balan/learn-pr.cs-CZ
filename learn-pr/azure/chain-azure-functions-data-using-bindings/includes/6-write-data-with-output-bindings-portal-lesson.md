Tak jako u vstupních vazeb existují i různé druhy výstupních vazeb. Ne všechny typy ale podporují vstup a výstup. Používají se, kdykoli chcete odesílat nebo ukládat data. Podívejme se teď na typy, které podporují výstupní vazby, a kdy je použít.

## <a name="output-binding-types"></a>Typy výstupních vazeb

- **Blob Storage** – Můžete používat výstupní vazbu blob k zápisu objektů blob.

- **Cosmos DB** – Výstupní vazba Azure Cosmos DB umožňuje zapsat nový dokument do databáze Azure Cosmos DB pomocí rozhraní SQL API.

- **Event Hubs** – Pomocí výstupní vazby služby Event Hubs můžete zapisovat události do streamu událostí. Musíte mít oprávnění k odesílání do centra událostí, abyste do něj události mohli zapisovat.

- **HTTP** – Výstupní vazbu HTTP použijte k tomu, abyste mohli odpovědět odesílateli požadavku HTTP. Tato vazba vyžaduje aktivační událost protokolu HTTP a umožňuje přizpůsobit odpověď přidruženou k požadavku aktivační události. Můžete ji použít také pro připojení k webhookům.

- **Microsoft Graph** – Výstupní vazby Microsoft Graphu umožňují zapisovat do souborů na OneDrivu, upravovat excelová data a odesílat e-maily prostřednictvím Outlooku.

- **Mobile Apps** – Výstupní vazba Mobile Apps zapisuje nový záznam do tabulky Mobile Apps.

- **Notification Hubs** – Pomocí výstupních vazeb Notification Hubs můžete posílat nabízená oznámení.

- **Queue Storage** – Výstupní vazbu Azure Queue Storage můžete použít k zapisování zpráv do fronty.

- **SendGrid** – Pomocí vazeb SendGrid můžete odesílat e-maily.

- **Service Bus** – Výstupní vazby Azure Service Bus můžete používat k odesílání zpráv fronty nebo tématu.

- **Table storage** – Výstupní vazbu služby Azure Table Storage můžete použít k zapisování do tabulky v účtu služby Azure Storage.

- **Twilio** – Odesílání textových zpráv prostřednictvím platformy Twilio.

Pokud chcete vytvořit vazbu jako výstup, je nutné definovat `direction` jako `out`. Parametry pro každý typ vazby se mohou lišit.

## <a name="combining-input-and-output-bindings"></a>Kombinování vstupní a výstupní vazby 

Pro jedinou funkci je možné použít více vazeb. To vám umožní definovat jak vstupní, tak výstupní vazbu, přičemž vstup a výstup mohou mít dokonce stejný typ vazby.