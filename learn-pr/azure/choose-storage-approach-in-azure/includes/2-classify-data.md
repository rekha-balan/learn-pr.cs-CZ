Online maloobchod má různé typy dat. Pro každý typ může být vhodnější jiné řešení úložiště. 

Data aplikací můžou být klasifikovaná jedním ze tří způsobů. Můžou být: strukturovaná, částečně strukturovaná a nestrukturovaná. Tady se dozvíte, jak data klasifikovat, abyste si mohli zvolit vhodné řešení úložiště.

#### <a name="approaches-to-storing-data-in-the-cloud"></a>Přístupy k ukládání dat v cloudu

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuY]

## <a name="structured-data"></a>Strukturovaná data

Strukturovaná data se drží určitého schématu, což znamená, že všechna data mají stejná pole nebo vlastnosti. Strukturovaná data můžete ukládat do databázové tabulky, která má řádky a sloupce. Strukturovaná data jsou opatřená klíči, které určují, jak řádek v jedné tabulce souvisí s daty jiného řádku v druhé tabulce. Strukturovaná data se také označují jako relační data. Datové schéma definuje tabulku dat, pole v tabulce a jednoznačný vztah mezi nimi.

Strukturovaná data můžeme jednoduše zadávat, vyhledávat dotazy a analyzovat. Všechna tato data používají stejný formát.

Příklady strukturovaných dat:

- Data ze snímačů
- Finanční data

## <a name="semi-structured-data"></a>Částečně strukturovaná data

Částečně strukturovaná data nejsou tolik uspořádaná jako strukturovaná data a neukládají se ve formátu relačních dat, protože jejich pole přesně nezapadají do tabulek, řádků a sloupců. Částečně strukturovaná data obsahují značky, které vysvětlují organizaci a hierarchii dat. Částečně strukturovaná data se také označují jako nerelační data nebo data NoSQL.

#### <a name="what-is-nosql"></a>Co je NoSQL?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvd]

Příklady částečně strukturovaných dat:

- Páry klíč / hodnota
- Data grafu
- Soubory JSON
- Soubory XML

## <a name="unstructured-data"></a>Nestrukturovaná data

Obecně řečeno je uspořádání nestrukturovaných dat nejednoznačné. Nestrukturovaná data často představují soubory, jako jsou fotky nebo videa. Samotný videosoubor může mít celkovou strukturu, kterou tvoří částečně strukturovaná metadata, ale data, která tvoří samotné video, strukturovaná nejsou. Proto se fotky, videa a další podobné soubory klasifikují jako nestrukturovaná data.

Příklady nestrukturovaných dat:

- Multimediální soubory, jako jsou fotky, videa a zvukové soubory
- Soubory Office, například wordové dokumenty
- Textové soubory
- Soubory protokolu

Už znáte rozdíly mezi jednotlivými typy dat. Pojďme se teď podívat na datové sady používané v online aplikaci pro maloobchodní prodej a na jejich klasifikaci.

## <a name="product-catalog-data"></a>Data v katalogu produktů

Data v katalogu produktů v online aplikaci pro maloobchodní prodej jsou už ze své podstaty značně strukturovaná – každý produkt má číslo skladové položky, popis, množství, cenu, rozměry, barevné varianty, fotku a případně i video. Tato data jsou na první pohled relační, protože mají všechna stejnou strukturu. Jak ale budete postupně zavádět nové produkty nebo různé druhy produktů, pravděpodobně budete chtít časem přidávat další pole. Například nové tenisky, které prodáváte, podporují Bluetooth, aby mohly přenášet data ze snímačů v botách do tréninkové aplikace v telefonu uživatele. Zdá se, že tento trend bude stále rozšířenější, a proto chcete zákazníkům do budoucna umožnit, aby si mohli vyfiltrovat boty, které mají Bluetooth. Nechcete se vracet zpět a aktualizovat všechna data o už prodávaných botách přidáním vlastnosti týkající se Bluetooth – chcete ji přidávat jen k novým botám.

Po přidání této vlastnosti s údajem o podpoře Bluetooth už nebudou data o botách homogenní, protože jste do schématu vnesli rozdíly. Pokud jde o jedinou výjimku, kterou očekáváte, můžete se vrátit a normalizovat existující data tak, aby všechny produkty zahrnovaly pole uvádějící podporu Bluetooth, aby byla zachováno strukturované relační uspořádání. Pokud jde ale jenom o jedno z mnoha polí pro speciální prvky, která podle svého předpokladu budete v budoucnu zavádět, bude tato data potřeba klasifikovat jako částečně strukturovaná. Data jsou uspořádaná na základě značek, ale každý produkt v katalogu může obsahovat jedinečná pole.

Klasifikace dat: **Částečně strukturovaná**

## <a name="photos-and-videos"></a>Fotky a videa

Fotky a videa zobrazené na stránkách produktu představují nestrukturovaná data. Multimediální soubor sice může obsahovat metadata, samotný obsah souboru je ale nestrukturovaný.

Klasifikace dat: **Nestrukturovaná**

## <a name="business-data"></a>Firemní data

Obchodní analytici potřebují implementovat funkce business intelligence, aby mohli vyhodnocovat zásobovací kanály a kontrolovat prodejní data. Tyto operace vyžadují agregovaná data za několik měsíců, na která se spouštějí dotazy. Vzhledem k nutnosti agregace podobných dat musí být tato data strukturovaná, abyste mohli porovnávat jednotlivé měsíce.

Klasifikace dat: **Strukturovaná**

## <a name="summary"></a>Shrnutí

Data můžou být klasifikovaná jedním ze tří způsobů. Můžou být: strukturovaná, částečně strukturovaná a nestrukturovaná. Porozumění těmto rozdílům tak, abyste dokázali klasifikovat svoje vlastní data, vám pomůže zvolit správné řešení úložiště. 

Pojďme si to tedy shrnout. Strukturovaná data jsou uspořádaná způsobem, který umožňuje hladké vkládání do řádků a sloupců v tabulkách. Částečně strukturovaná data jsou také uspořádaná a mají jasné vlastnosti a hodnoty, ale existují v nich odlišnosti. Nestrukturovaná data nezapadají přesně do tabulek ani nemají schéma.