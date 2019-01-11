Než začneme zkoumat některé postupy spojené se SRE, bude dobré si zasadit do souvislostí pár myšlenek, se kterými jsme se seznámili v předchozí lekci. V této krátké lekci si povíme něco o historii SRE a souvislostech s provozními postupy, které možná znáte. Později se nám to bude velmi hodit, protože zasazené do kontextu tyto postupy dostanou hlubší smysl. Navíc, když se vás přátelé zeptají „Jak se SRE liší od...“, budete mít připravenou odpověď.

## <a name="history"></a>Historie

Velmi zhuštěná historie SRE začíná v roce 2003 ve společnosti Google. Ben Treynor, nyní Treynor Sloss, převzal vedení „Produkčního týmu“ společnosti Google (tehdy pouze sedm softwarových inženýrů) a přišel s myšlenkou, kterou popsal slovy „co se stane, když požádáte softwarového inženýra, aby navrhl provozní funkci“. Tady nám historie pomůže pochopit, proč lidem, kteří se s technikou SRE setkávají poprvé, přijde celá moc „softwarově inženýrská“. Přejala totiž z téhle oblasti hodnoty a nástroje, jako jsou například důležitost kódování nebo systémy kontroly zdrojového kódu jako základní nástroj. Původní a současná implementace SRE společnosti Google je velice dobře popsaná v jejich dvou knihách vydaných nakladatelstvím O'Reilly (viz lekce Začínáme).

S tím, jak lidé z Googlu přecházeli do jiných společností (a lidé ze společnosti mluvili o postupech SRE více veřejně), začala se SRE šířit do dalších organizací. SRE se rozšířila do nových organizací, které přijaly a přizpůsobily si principy SRE tak, aby vyhovovaly jejich místní kultuře. Proces rozšíření přinesl řadu různých implementací SRE v praxi. 

## <a name="devops-and-sre"></a>DevOps a SRE

Celé softwarové odvětví čelilo obdobným výzvám ohledně škálování, rychlost vývoje versus provozní stabilita a další záležitosti týkající se dodávání softwaru, které stály u zrodu hnutí SRE. Paralelní úsilí o řešení těchto problémů mimo Google (a několik tehdy větších společností) přineslo principy DevOps. 

Další kvalitní informace o DevOps najdete na stránkách [ https://docs.microsoft.com/azure/devops/learn/ ](https://docs.microsoft.com/azure/devops/learn/).

> [!NOTE]
> Tady je nutné poznamenat, že DevOps a SRE jsou dva různé paralelní přístupy k řešení stejných problémů. SRE není dalším vývojovým stupněm po DevOps. Technika SRE nebyla vytvořena jako „budoucnost DevOps“.

Rozdíly mezi SRE a DevOps jsou stále v praxi námětem diskuzí a sporů. Tady uvádíme pár rozdílů, o kterých panuje obecně shoda:

- SRE je inženýrská disciplína, která se zaměřuje na spolehlivost, zatímco DevOps je kulturní hnutí, které vyvstalo z potřeby spolupráce mezi jinak oddělenými odděleními vývoje a provozu (Development and Operations).
- O SRE by se dalo mluvit jako o určité roli na pracovišti („Dělám u nás SRE...“), což u DevOps moc nejde. Jeden člověk může dělat SRE, ale jeden člověk nemůže dělat DevOps.
- SRE je obecně direktivnější, DevOps záměrně naopak. Téměř univerzální přijetí kontinuální integrace a dodávání a principů Agile jsou v tomto ohledu nejblíž.

Tyto dva provozní postupy, DevOps a SRE, sdílejí zaujetí pro monitorování/pozorovatelnosti a automatizaci, i když patrně z různých důvodů. Proto je často snazší přijmout postupy a principy SRE v organizaci s existující praxí DevOps. Tento proces je třeba provést opatrně a promyšleně. Může (a měl by) být zaváděn postupně jeden krok po druhém. Není nutná náhlá změna všeho.

> [!WARNING]
> Pouhé prohození názvů pracovních pozic je strategie implementace, která je téměř vždy odsouzena k nezdaru. Nepřinese to výhody, které jinak může SRE nabídnout. V části Začínáme této lekci naleznete další zajímavé návrhy.

## <a name="conclusion"></a>Závěr

V této krátké lekci jsme se pokusili zasadit SRE a DevOps do trochu širšího kontextu. SRE a DevOps jsou dva blízké myšlenkové směry provozních postupů a v takovýchto intencích je nejlepší o nich uvažovat. 

Po krátkém ohlédnutí za tím, co stálo na počátku SRE, se přesuneme k některým základním principům.
