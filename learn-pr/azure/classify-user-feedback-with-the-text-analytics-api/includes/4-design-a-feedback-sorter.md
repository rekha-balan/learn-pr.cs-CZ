Pojďme naše znalosti analýzy textu převést do praxe ve funkčním řešení. Naše řešení se zaměří na analýzu mínění v textových dokumentech. Pojďme si nastavit kontext a popsat problém, který chceme vyřešit.

## <a name="manage-customer-feedback-more-efficiently"></a>Efektivnější správa názorů zákazníků

V sociálních sítích se aktivně diskutuje o produktu vaší společnosti. Vaše e-mailová adresa pro shromažďování názorů je také hojně využívaná a zákazníci se ochotně dělí o své názory na váš produkt.

Podobně jako u jiných začínajících společností je vaší mantrou, že nasloucháte názorům svých zákazníků. Úspěch vašeho produktu ale způsobil, že o tomto slibu se teď snáze mluví, než se dodržuje. Je to sice příjemný problém, ale pořád problém.

Váš tým už nedokáže příval názorů pořádně zpracovat. Potřebuje pomoc s utříděním názorů, aby se jednotlivé věci mohly řešit co nejefektivněji. Jako vedoucího vývojáře v organizaci vás požádali o vytvoření řešení.

Podívejme se na některé požadavky na nejvyšší úrovni:

|Požadavek  | Podrobnosti  |
|---------|---------|
|Zařaďte názory do kategorií, abychom na ně mohli reagovat.     |   Ne všechny názory jsou rovnocenné. Něco jsou nadšené souhlasy. Další názor může být ostrou kritikou od frustrovaného zákazníka. V jiných případech třeba ani nedokážete určit, co vlastně zákazník chce. <br/><br/>Názory by nám ale pomohlo zařadit do kategorií, kdybychom měli aspoň indikaci o jejich náladě nebo tónu.     |
|Řešení by mělo být postavené tak, aby šlo podle požadavků vertikálně navýšit nebo snížit kapacitu.    |   Jsme začínající společnost. Pevné náklady se těžko ospravedlňují a zatím jsme ještě nerozpoznali přesný vzorec toho, jak často a za jakých okolností názory přicházejí. Budeme potřebovat řešení, které dokáže zvládnout špičky aktivity, ale v klidnějších obdobích na něj budou co nejnižší náklady. <br/><br/> V tomto případě je vhodným kandidátem architektura bez serveru účtovaná podle využití.     |
| Vytvořte minimální schůdný produkt, ale s možností dalších úprav.    | Dnes chceme hlavně zařadit názory do kategorií, aby se naše omezené zdroje mohly věnovat těm, na kterých záleží. Pokud je zákazník frustrovaný, chceme se to okamžitě dozvědět a začít s ním komunikovat. V budoucnu toto řešení vylepšíme dalšími funkcemi. Jedním z nápadů na novou funkci je zkoumat klíčové fráze v názorech a zjišťovat problematické body před tím, než u našich zákazníků dosáhnou kritické míry. Dalším nápadem je automatizace odpovědí zákazníkům, jejichž názory jsou pozitivní nebo neutrální. I když se jim naše produkty líbí, chceme jim dát vědět, že jejich názorům stále nasloucháme. <br/><br/>Tady je vhodné řešení, které nabízí architekturu plug-and-play. Mohli bychom třeba využít fronty jako obdobu továrního pásu. Provedete jednu úlohu a pak výsledek umístíte do fronty pro další část systému, která si ho vyzvedne a zpracuje.   |
|Doručte řešení rychle.     |   Tohle jsme už všichni slyšeli! Nezapomeňte, že řešení má být minimální schůdný produkt, který ale chceme rychle otestovat. Pokud chcete rychle doručit kvalitní řešení, je vhodné psát co nejméně kódu. <br/><br/> Když využijete rozhraní API pro analýzu textu, nemusíte trénovat model pro analýzu mínění. Použití služby Azure Functions a deklarativních vazeb na fronty sníží množství kódu, který budete muset napsat. Řešení bez serveru také znamená, že si nemusíme dělat starosti se správou serveru.   |

Naše navrhované řešení jednotlivých požadavků v předchozí tabulce nabízí náznak toho, jak se požadavky dají namapovat na řešení. Teď se pojďme podívat, jak by mohlo vypadat řešení založené na Azure.

## <a name="a-solution-based-on-azure-functions-azure-queue-storage-and-text-analytics-api"></a>Řešení založené na službách Azure Functions a Azure Queue Storage a na rozhraní API pro analýzu textu

V následujícím diagramu je návrh řešení. Používá tři základní součásti Azure: Azure Queue Storage, Azure Functions a Azure Cognitive Services.

![Koncepční diagram architektury pro třídění názorů](../media/proposed-solution.PNG)

Základní myšlenkou je, že textové dokumenty obsahující názory uživatelů se umístí do fronty, kterou jsme v předchozím diagramu pojmenovali *new-feedback-q*. Přijetí zprávy, která obsahuje textový dokument, do fronty aktivujte neboli spustí provádění funkce. Funkce přečte zprávy obsahující nové dokumenty ze vstupní fronty a odešle je k analýze do rozhraní API pro analýzu textu. Na základě výsledků, které rozhraní API vrátí, se nová zpráva obsahující dokument umístí do výstupní fronty pro další zpracování.

Výsledek, který získáme pro každý dokument, je skóre mínění (tónu a nálady). Výstupní fronty se používají k ukládání názorů roztříděných na pozitivní, neutrální a negativní. Doufejme, že fronta pro negativní názory bude pořád prázdná! :-) Jakmile jednotlivé příchozí názory rozdělíme do výstupních front podle jejich vyznění, dokážete si jistě představit přidání logiky, která by se zprávami v jednotlivých frontách prováděla další akce.

Podívejme se na vývojový diagram, který znázorní, co by logika funkce měla dělat.

![Vývojový diagram logiky v rámci funkce služby Azure Functions, která třídí textové dokumenty do výstupních front podle jejich vyznění](../media/flow.PNG)

Naše logika je jako směrovač. Převezme textový vstup a nasměruje ho do výstupní fronty na základě zjištěného skóre mínění textu. Řešení je závislé na rozhraní API pro analýzu textu. Logika se může zdát triviální, ale tato funkce eliminuje potřebu, aby názory ručně analyzovali členové týmu.

## <a name="steps-to-implement-our-solution"></a>Postup implementace našeho řešení

Pokud chcete implementovat řešení popsané v tomto modulu, proveďte následující kroky.

1. Vytvořte aplikaci funkcí pro hostování řešení.

1. Vygenerujte skóre mínění v příchozích názorech pomocí rozhraní API pro analýzu textu. Využijeme náš přístupový klíč z předchozího cvičení a napíšeme kód pro odesílání požadavků.

1. Odešlete názory do front pro zpracování podle skóre mínění.

Pojďme se pustit dál do vytvoření naší aplikace funkcí.