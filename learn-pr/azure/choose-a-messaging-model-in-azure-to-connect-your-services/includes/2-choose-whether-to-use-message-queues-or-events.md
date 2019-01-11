Předpokládejme, že plánujete architekturu distribuované aplikace pro sdílení hudby. Chcete zajistit, aby byla aplikace co nejvíce spolehlivá a škálovatelná, a hodláte pomocí technologií Azure sestavit robustní komunikační infrastrukturu.

Než si vyberete správnou technologii Azure, je potřeba pochopit, jaké druhy komunikace si budou jednotlivé komponenty aplikace vyměňovat. Pro každý typ komunikace můžete zvolit jinou technologii Azure.

První aspekt komunikace, který je potřeba pochopit, je to, jestli daná komunikace odesílá **zprávy**, nebo **události**. Jedná se o základní volbu, která vám pomůže vybrat vhodnou službu Azure.

#### <a name="communication-strategies-in-azure-apis"></a>Komunikační strategie v Azure (rozhraní API)

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuaw]

## <a name="what-is-a-message"></a>Co je zpráva?
V terminologii distribuovaných aplikací mají **zprávy** následující vlastnosti:

- Zpráva obsahuje nezpracovaná data vytvářená jednou komponentou, která bude spotřebovávat jiná komponenta.
- Zpráva obsahuje samotná data, ne odkazy na tato data.
- Odesílající komponenta očekává, že cílová komponenta zpracuje obsah zprávy určitým způsobem. Na provedení určité úlohy odesílající i přijímající komponentou může záviset integrita celého systému.

Můžeme si třeba představit, že uživatel pomocí mobilní aplikace pro sdílení hudby nahraje novou skladbu. Mobilní aplikace musí tuto skladbu odeslat do webového rozhraní API běžícího v Azure. Je potřeba odeslat samotný mediální soubor skladby, nejenom upozornění na přidání nové skladby. Mobilní aplikace očekává, že webové rozhraní API uloží novou skladbu do databáze a zpřístupní ji ostatním uživatelům. To je tedy příklad zprávy.

## <a name="what-is-an-event"></a>Co je událost?

**Události** jsou méně objemné než zprávy a nejčastěji se používají pro komunikaci s všesměrovým vysíláním. Komponenty odesílající událost se označují jako **vydavatelé** a příjemci jako **odběratelé**.

V případě událostí se většinou přijímající komponenty rozhodují, jaké komunikace je zajímají, a přihlašují se k jejich odběru. Odběr většinou spravuje některý zprostředkovatel, třeba Azure Event Grid nebo Azure Event Hubs. Když vydavatelé odešlou událost, zprostředkovatel ji přesměruje k odběratelům, kteří o ni mají zájem. To se označuje jako architektura publikování a odběru. Není to jediný způsob, jak zacházet s událostmi, ale je to způsob nejčastější.

Události mají následující vlastnosti:

- Událost je zjednodušené oznámení, které udává, že se něco stalo.
- Událost se může odeslat více příjemcům nebo žádnému příjemci.
- Události jsou často určené k větvení nebo mají pro každého vydavatele velký počet odběratelů.
- Vydavatel události nemá žádná očekávání ohledně toho, co má provést přijímající komponenta.
- Některé události jsou samostatné jednotky a nesouvisejí s ostatními událostmi. 
- Jiné události jsou součástí propojené a uspořádané řady.  

Předpokládejme například, že se nahrávání hudebního souboru dokončilo a nová skladba se přidala do databáze. Za účelem informování uživatelů o novém souboru musí webové rozhraní API informovat o novém souboru webový front-end a uživatele mobilní aplikace. Uživatelé si můžou vybrat, jestli si novou skladbu chtějí poslechnout. Počáteční oznámení proto neobsahuje samotný hudební soubor, ale jenom uživatele informuje o jeho existenci. Odesílatel nemá žádná konkrétní očekávání, že příjemci události podniknou v reakci na přijetí události nějaké kroky.

To je tedy příklad samostatné události.

## <a name="how-to-choose-messages-or-events"></a>Volba mezi zprávami a událostmi

Jedna aplikace bude pravděpodobně k některým účelům používat události a k jiným zase zprávy. Než se rozhodnete, je potřeba podrobit architekturu aplikace a všechny její případy použití analýze, abyste zjistili, za jakými účely budou muset její jednotlivé komponenty vzájemně komunikovat.

Události se pravděpodobně budou používat pro vysílání a jsou často dočasné, což znamená, že pokud není zrovna přihlášený k odběru žádný příjemce, nemusí komunikaci nikdo zpracovat. Zprávy se budou pravděpodobně používat v případech, kdy distribuovaná aplikace vyžaduje záruku zpracování komunikace.

U každé komunikace si položte tuto otázku: **Očekává odesílající komponenta, že cílová komponenta zpracuje komunikaci určitým způsobem?**

Pokud si odpovíte _kladně_, zvolte použití zprávy. Pokud je odpověď _záporná_, možná budete moct používat události.

Když pochopíte, jakým způsobem potřebují vaše komponenty komunikovat, snadno se pak rozhodnete, jak budou komunikovat. Začněme se zprávami.