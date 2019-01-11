Představte si situaci, kdy oblíbený kadeřnický salon má opakovaný problém: zákazníci často promeškají domluvené návštěvy. Návštěvy se objednávají na určené časy. Pokud zákazník promešká návštěvu, přijde salon o peníze. Salon vás (vývojáře softwaru) kontaktuje, abyste tento problém vyřešili. Rozhodnete se problém odstranit posíláním textových zpráv s připomenutím. Textové zprávy by se mohly odeslat při naplánování nebo změně schůzky a každé ráno pošlete všem zákazníkům textovou zprávu s informacemi o schůzce, která se v daný den uskuteční.

Potřebujete vytvořit službu, kterou lze snadno plánovat, aktualizovat a škálovat. Rozhodnete se tento problém vyřešit pomocí aplikace funkcí. Jak implementovat logiku posílání textových zpráv, už víte. Teď potřebujete zjistit, jak zprávu odeslat v určitém čase nebo když nastane konkrétní událost. Služba Azure Functions naštěstí podporuje funkci nazývanou _triggery_ (aktivační události). Triggery určují, jak se funkce Azure spouštějí.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:
- Zjistit, jaký trigger nejlépe vyhovuje vašim firemním potřebám
- Vytvořit trigger časovače k vyvolání funkce podle konzistentního plánu
- Vytvořit trigger HTTP k vyvolání funkce při přijetí požadavku HTTP
- Vytvořit trigger objektu blob k vyvolání funkce při vytvoření nebo aktualizaci objektu v Azure Storage