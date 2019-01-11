Web Azure Portal je skvělý nástroj k provádění samostatných úloh a zobrazení rychlého přehledu o stavu prostředků. Ale v případě úloh, které se musí opakovat každý den nebo i každou hodinu, vám může použití příkazového řádku a sady ověřených příkazů nebo skriptů pomoct dokončit práci rychleji a vyhnout se chybám.

Předpokládejme, že pracujete ve společnosti, která vyvíjí aplikace v Azure Web Apps. Jsou to aplikace hostované v Azure se všemi výhodami automaticky nakonfigurovaného zabezpečení, vyrovnávání zatížení, správy a tak dále. Aktuálně testujete webovou aplikaci, která na základě celé řady vstupů z různých databází a jiných zdrojů dat generuje prognózy prodeje. Vaši vývojáři používají počítače s Windows, Linuxem a macOS a pro každodenní sestavení aplikací používají úložiště GitHub.

Jako součást testování chcete porovnat výkon aplikací u různých zdrojů dat a různých typů datových připojení. Všimli jsme si, že když váš vývojový tým používá k vytvoření nové testovací instance aplikace web Azure Portal, nepoužívá vždy přesně stejné parametry. Tento problém chcete vyřešit pomocí sady příkazů standardního nasazení, kterou je možné v případě potřeby automatizovat, aby na všech počítačích používaných softwarovým týmem probíhal každý test aplikace stejně.

V tomto modulu uvidíte, jak pomocí Azure CLI spravovat prostředky Azure.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Nainstalovat Azure CLI na Linux, macOS a/nebo Windows
- Připojit se pomocí Azure CLI k předplatnému Azure
- Vytvořit prostředky Azure pomocí Azure CLI

## <a name="prerequisites"></a>Požadavky

- Zkušenost s rozhraním příkazového řádku (například PowerShell nebo Bash)
- Znalost základních konceptů Azure (například skupin prostředků)
- Zkušenosti se správou prostředků Azure prostřednictvím webu Azure Portal