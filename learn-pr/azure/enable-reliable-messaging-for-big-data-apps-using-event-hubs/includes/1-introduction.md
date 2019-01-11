Aplikace pro zpracování velkých objemů dat by měly mít schopnost zpracovat zvýšené nároky na propustnost horizontálním navýšením kapacity, která zvládne vyšší objemy transakcí.

Předpokládejme, že pracujete v bance v oddělení platebních karet. Jste součástí týmu pro správu systému testování možných podvodů, který zjišťuje, jestli se mají jednotlivé transakce schválit nebo zamítnout. Tento systém přijímá proud transakcí a potřebuje je zpracovávat v reálném čase.

Zatížení systému může narůst během víkendů a svátků. Systém by měl efektivně a přesně zvládat zvýšené nároky na propustnost. Vzhledem k citlivé povaze transakcí může mít i nejmenší chyba obrovský dopad.

Služba Azure Event Hubs dokáže přijímat a zpracovávat velké počty transakcí. Může být také nakonfigurovaná tak, aby se v případě potřeby kvůli zvládnutí zvýšených nároků na propustnost dynamicky navýšila její kapacita.
V tomto modulu se dozvíte, jak připojit službu Event Hubs k vaší aplikaci a spolehlivě zpracovávat obrovské objemy transakcí.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu:

- Vytvoříte centrum událostí pomocí Azure CLI.
- Nakonfigurujete aplikace k odesílání nebo příjmu zpráv prostřednictvím centra událostí.
- Vyhodnotíte výkon centra událostí pomocí portálu Azure Portal.