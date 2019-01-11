Při použití služby Event Hubs je zásadní monitorovat centrum událostí a zajistit, aby fungovalo a pracovalo podle očekávání.

Pokračujme v příkladu bankovnictví a předpokládejme, že jste nasadili službu Azure Event Hubs a nakonfigurovali aplikace odesílatele a příjemce. Vaše aplikace jsou připravené na testování řešení pro zpracování plateb. Aplikace odesílatele shromažďuje data platebních karet zákazníků a aplikace příjemce ověřuje, jestli je platební karta platná. Vzhledem k citlivé povaze podnikání vašeho zaměstnavatele je nezbytné, aby zpracování plateb bylo robustní a spolehlivé, i když bude dočasně nedostupné.

Centrum událostí musíte vyhodnotit a otestovat, jestli zpracovává data podle očekávání. Metriky, které jsou dostupné ve službě Event Hubs, vám umožní zjistit, jestli centrum událostí funguje správně.

## <a name="how-do-you-use-the-azure-portal-to-view-your-event-hub-activity"></a>Jak pomocí portálu Azure Portal zobrazíte aktivitu centra událostí?

Na stránce Azure Portal > Přehled se zobrazují počty zpráv. Tyto počty zpráv představují data (události) přijaté a odeslané centrem událostí. Můžete zvolit časové měřítko pro zobrazení těchto událostí.

![Snímek obrazovky portálu Microsoft Azure zobrazuje obor názvů centra událostí s počtem zpráv.](../media/6-view-messages.png)

## <a name="how-can-you-test-event-hub-resilience"></a>Jak můžete testovat odolnost centra událostí?

Služba Azure Event Hubs přijme zprávy z aplikace odesílatele, i když není dostupná. Zprávy přijaté během tohoto období se úspěšně přenesou, až bude centrum událostí dostupné.

Abyste tuto funkci otestovali, můžete pomocí portálu Azure Portal zakázat centrum událostí.

Až centrum událostí znovu povolíte, můžete aplikaci příjemce znovu spustit a pomocí metriky služby Event Hubs pro váš obor názvů zkontrolovat, jestli byly všechny zprávy odesílatele úspěšně přenesené a přijaté.

K dalším užitečným metrikám dostupným ve službě Event Hubs patří:

- Omezené požadavky: Počet žádostí, které byly omezeny, protože bylo překročeno použití jednotek propustnosti.
- Aktivní připojení: Počet aktivních připojení u oboru názvů nebo centra událostí.
- Příchozí/odchozí bajty: Počet bajtů odeslaných do nebo přijatých ze služby Event Hubs za zadané období.

## <a name="summary"></a>Shrnutí

Azure Portal nabízí počty zpráv a další metriky, které můžete použít ke kontrole stavu služby Event Hubs.
