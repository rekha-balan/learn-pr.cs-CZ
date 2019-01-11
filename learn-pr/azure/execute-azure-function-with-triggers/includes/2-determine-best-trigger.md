Aplikace funkce Azure neprovede svoji činnost, dokud se nějak neaktivuje její spuštění. Můžeme například vytvořit funkci Azure, která před nějakou událostí odesílá našim zákazníkům textovou zprávu s připomenutím. Pokud pro tuto funkci neurčíme, kdy se má spustit, naši zákazníci žádnou zprávu nedostanou. 

Budeme se teď zabývat triggery na vysoké úrovni a prozkoumáme nejběžnější typy triggerů.

## <a name="what-is-a-trigger"></a>Co je trigger?

Trigger je objekt, který definuje, jak se funkce Azure vyvolává. Pokud například chcete, aby se funkce spouštěla každých 10 minut, můžete použít trigger typu časovač.

Každá funkce musí mít přiřazený právě jeden trigger. Pokud chcete spouštět logiku, která používá více podmínek, je nutné vytvořit více funkcí, které sdílejí stejný základní kód funkce.

V tomto modulu se budeme zabývat triggery typu **časovač**, **HTTP** a **objekt blob**.

## <a name="types-of-triggers"></a>Typy triggerů

Služba Azure Functions podporuje širokou škálu typů triggerů. Tady jsou některé z nejběžnějších typů:

| Typ | Účel |
| --- | --- |
| **Časovač** | Spustí funkci v nastaveném intervalu. |
| **HTTP** | Spustí funkci po přijetí požadavku HTTP. |
| **Objekt blob** | Spustí funkci po nahrání souboru do úložiště objektů blob v Azure nebo po jeho aktualizaci. |
| **Fronta** | Spustí funkci po přidání zprávy do fronty ve službě Azure Storage. |
| **Databáze Cosmos** | Spustí funkci po změně dokumentu v kolekci. |
| **Centrum událostí** | Spustí funkci po přijetí nové události v centru událostí. |

## <a name="what-is-a-binding"></a>Co je vazba?

Vazba je propojení na data v rámci funkce. Vazby jsou volitelné a může se jednat o vstupní nebo výstupní vazby. Vstupní vazba jsou data, která funkce přijímá. Výstupní vazba jsou data, která funkce odesílá.

Na rozdíl od triggeru může mít funkce více vstupních a výstupních vazeb.

V dalším cvičení spustíme funkci podle plánu pomocí triggeru časovače.