Jakmile zjistíte, s jakým druhem dat budete pracovat (strukturovaná, částečně strukturovaná nebo nestrukturovaná), musíte v dalším kroku určit, jak budete tato data používat. Třeba jako online prodejce víte, že zákazníci potřebují rychlý přístup k datům o produktech. Podnikoví uživatelé zase potřebují spouštět složité analytické dotazy. Když se začnete těmito požadavky podrobněji zabývat a přihlédnete přitom ke způsobu, jakým třídíte data, můžete začít plánovat řešení datového úložiště.

V tomto článku projdeme některé z otázek, které byste si měli položit, když stanovujete, co budete s daty dělat.

## <a name="operations-and-latency"></a>Operace a latence

Jaké hlavní operace budete provádět s jednotlivými typy dat a jaké jsou požadavky na výkon?

Položte si tyto otázky:

* Budete vyhledávat jednoduše podle ID?
* Potřebujete zadávat dotazy do databáze na jedno nebo více polí?
* Kolik vytvářecích, aktualizačních a odstraňovacích operací předpokládáte?
* Potřebujete spouštět složité analytické dotazy?
* Jak musí být tyto operace rychlé?

Pojďme projít jednotlivé datové sady. Probereme požadavky s přihlédnutím k uvedeným otázkám.

## <a name="product-catalog-data"></a>Data v katalogu produktů

Nejvyšší prioritou u dat v katalogu produktů ve scénáři online maloobchodního prodeje jsou potřeby zákazníka. Zákazníci budou zadávat dotazy do katalogu produktů. Budou třeba hledat veškerou pánskou obuv, potom pánskou obuv v akci a nakonec pánskou obuv v akci určité velikosti. Potřeby zákazníka můžou vyžadovat velké množství operací čtení spolu s možností dotazování na určitá pole.

Když zákazníci zadávají objednávky, musí aplikace také aktualizovat údaje o počtech produktů. Aktualizační operace musí probíhat stejně rychle jako operace čtení, aby si uživatelé nedávali do košíku zboží, které už je vyprodané. Důsledkem bude nejen velké množství operací čtení, ale bude nutný také vyšší počet operací zápisu dat do katalogu produktů. Nezapomeňte určit priority pro všechny uživatele databáze, nejen ty primární.

## <a name="photos-and-videos"></a>Fotky a videa

Pro fotky a videa, které se zobrazují na stránkách produktu, platí jiné požadavky. Potřebujeme je načítat a zobrazovat na webu stejně rychle jako data z katalogu produktů, ale nepotřebujeme je vyhledávat samostatnými dotazy. Můžete raději použít výsledky dotazu na produkt a zahrnout ID videa nebo adresu URL jako vlastnost v datech produktu. Fotky a videa tedy stačí načítat jenom podle jejich ID.

Kromě toho uživatelé nebudou provádět aktualizace stávajících fotek nebo videí. Můžou ale přidávat nové snímky jako součást svých recenzí produktů. Mohli by třeba nahrát snímek sebe sama v nových botách. Zaměstnanci také budou nahrávat a odstraňovat fotky produktů od dodavatelů, ale tato aktualizace nemusí být tak rychlá jako ostatní aktualizace dat o produktech. 

Jinak řečeno, dotazy na fotky a videa můžou obsahovat jenom ID, které stačí k vrácení celého souboru, ale vytváření a aktualizace těchto souborů nebudou tak časté a budou mít menší prioritu.  

## <a name="business-data"></a>Firemní data

U firemních dat probíhají všechny analýzy na základě historických dat. V rámci analýz se nebudou žádná původní data aktualizovat, takže firemní data jsou jen pro čtení. Uživatelé neočekávají, že se komplexní analýzy budou spouštět okamžitě, takže určitá latence při zobrazení výsledků je v pořádku. Kromě toho budou firemní data uložená ve více datových sadách, přičemž uživatelé budou mít u každé z nich odlišná přístupová oprávnění k zápisu. Firemní analytici ale budou moct číst ze všech databází.

## <a name="summary"></a>Shrnutí

Při rozhodování o tom, jaké řešení úložiště použít, zvažte, jak se budou data používat. Jak často se bude k datům přistupovat? Jsou vaše data jen pro čtení? Hraje důležitou roli to, jak dlouho se bude dotaz zpracovávat? Odpovědi na tyto otázky vám pomůžou při rozhodování o nejlepším řešení úložiště pro vaše data.