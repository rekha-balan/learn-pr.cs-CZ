Většina organizací má různé požadavky na data hostovaná v cloudu. Například ukládání data do konkrétní oblasti nebo potřeba samostatné fakturace pro různé kategorie dat. Účty úložiště Azure vám umožňují tyto druhy zásad formalizovat a použít je na vaše data v Azure.

Předpokládejme, že pracujete u výrobce čokolády, který vyrábí přísady na pečení jako například kakaový prášek nebo čokoládové lupínky. Své produkty nabízíte obchodům s potravinami, které je prodávají zákazníkům.

Vaše recepty a výrobní procesy jsou obchodním tajemstvím. Tabulky, dokumenty a instruktážní videa zaznamenávající tyto informace jsou pro vaši firmu životně důležité a vyžadují geograficky redundantní úložiště. K těmto datům se primárně přistupuje z vaší hlavní továrny, takže byste je chtěli uložit v blízkém datacentru. Výdaje za toto úložiště je potřeba účtovat výrobnímu oddělení.

Máte také prodejce, kteří vytváří recepty a videa o pečení, aby tyto produkty propagovali zákazníkům. U těchto dat je spíš než redundance nebo umístění prioritou hlavně nízká cena. Toto úložiště se musí fakturovat prodejnímu týmu.

V tomto článku uvidíte, jak tyto druhy obchodních požadavků zpracovat vytvořením několika účtů Azure Storage. Každý účet úložiště bude mít nastavení odpovídající obsaženým datům.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

 - Rozhodnout se, kolik účtů úložiště pro svůj projekt potřebujete.
 - Určit odpovídající nastavení pro každý účet úložiště.
 - Vytvořit účet úložiště pomocí webu Azure Portal