Předpokládejme, že potřebujete vybrat nástroj pro správu prostředků Azure používaných k testování vašeho systému správy vztahů se zákazníky (CRM). Testy vyžadují, abyste vytvořili skupiny prostředků a zřídili virtuální počítače.

Chcete řešení, se kterým by se správci snadno naučili pracovat, ale které je zároveň dostatečně výkonné, aby dokázalo automatizovat instalaci a nastavení více virtuálních počítačů nebo pomocí skriptů vytvořit kompletní prostředí aplikace. Existuje celá řada nástrojů a vy potřebujete najít ten, který bude pro vaše pracovníky a úlohy nejvhodnější.

## <a name="what-tools-are-available"></a>Jaké nástroje jsou k dispozici?
V Azure si můžete vybrat ze tří nástrojů pro správu:

- Azure Portal
- Azure CLI
- Azure PowerShell

Všechny tyto nástroje nabízejí přibližně stejnou úroveň kontroly. Všechno, co dokáže jeden z těchto nástrojů, dokážou i zbylé dva. Všechny tři jsou určené pro různé platformy a fungují ve Windows, macOS i Linuxu. Liší se syntaxí, požadavky na instalaci a tím, jestli podporují automatizaci.

V tomto článku si tyto tři možnosti popíšeme a uvedeme pár rad, jak se mezi nimi rozhodnout. 

## <a name="what-is-the-azure-portal"></a>Co je web Azure Portal?
Azure Portal je web, který umožňuje vytvářet, konfigurovat a měnit prostředky ve vašem předplatném Azure. Portál tvoří grafické uživatelské rozhraní, které usnadňuje hledání potřebných prostředků a provádění požadovaných změn. Zároveň vás pomocí průvodců a tipů provádí složitými úlohami správy.

Portál neposkytuje žádný způsob, jak automatizovat opakující se úlohy. Pokud například chcete nainstalovat 15 virtuálních počítačů, museli byste je vytvořit jeden po druhém pomocí průvodce. V případě složitých úloh to může být časově náročné a náchylné k chybám. 

## <a name="what-is-the-azure-cli"></a>Co je Azure CLI?
Azure CLI je program příkazového řádku pro různé platformy, který zajišťuje připojení k Azure a provádí příkazy správy na prostředcích Azure. Pokud byste například chtěli vytvořit virtuální počítač, použili byste příkaz podobný tomuto:

```azurecli
az vm create \
  --resource-group CrmTestingResourceGroup \
  --name CrmUnitTests \
  --image UbuntuLTS
  ...
```

Rozhraní Azure CLI je dostupné dvěma cestami: v prohlížeči prostřednictvím prostředí Azure Cloud Shell nebo jako místní instalace v systému Linux, Mac nebo Windows. V obou případech se dá použít interaktivně nebo pomocí skriptu. Při interaktivním použití nejprve spusťte prostředí, například `cmd.exe` ve Windows nebo Bash v Linuxu nebo macOS, a potom na příkazovém řádku daného prostředí zadejte příslušný příkaz. Pokud chcete automatizovat opakující se úlohy, sestavte příkazy do skriptu prostředí pomocí syntaxe skriptu pro dané prostředí a potom spusťte tento skript.

## <a name="what-is-azure-powershell"></a>Co je Azure PowerShell?
Azure PowerShell je modul, který přidáte do prostředí Windows PowerShell nebo PowerShell Core a který umožňuje připojení k předplatnému Azure a správu prostředků. Azure PowerShell vyžaduje prostředí PowerShell. PowerShell poskytuje služby, jako je okno prostředí, analýza příkazů a podobně. Azure PowerShell přidává příkazy specifické pro Azure.

Azure PowerShell třeba nabízí příkaz **New-AzVM**, který ve vašem předplatném Azure vytvoří virtuální počítač. Pokud ho chcete použít, spusťte aplikaci PowerShell a potom zadejte příkaz podobný tomuto:

```powershell
New-AzVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Nástroj Azure PowerShell je dostupný dvěma cestami: v prohlížeči prostřednictvím prostředí Azure Cloud Shell nebo jako místní instalace v systému Linux, Mac nebo Windows. V obou případech si můžete vybrat ze dvou režimů. Nástroj můžete použít buď v interaktivním režimu, ve kterém ručně zadáváte vždy jeden příkaz, nebo v režimu skriptování, ve kterém spustíte skript tvořený několika příkazy.

## <a name="how-to-choose-an-administrative-tool"></a>Výběr nástroje pro správu
Azure Portal, Azure CLI a Azure PowerShell jsou si v zásadě podobné, co se týče toho, jaké objekty Azure dokážou spravovat a jaké konfigurace v nich můžete vytvářet. Jsou také určené pro různé platformy. To znamená, že při vybírání je většinou potřeba zvážit několik dalších faktorů:

- **Automatizace**: Potřebujete automatizovat sadu složitých nebo opakujících se úloh? Nástroje Azure PowerShell a Azure CLI to podporují, ale portál ne.

- **Rychlost osvojování**: Potřebujete provést úlohu rychle, bez učení nových příkazů nebo syntaxe? Web Azure Portal nevyžaduje, abyste se učili syntaxi nebo si pamatovali příkazy. V nástrojích Azure PowerShell a Azure CLI potřebujete znát podrobnou syntaxi každého příkazu, který použijete.

- **Znalosti týmu**: Má už váš tým nějaké odborné znalosti? Váš tým třeba mohl ke správě Windows používat PowerShell. V takovém případě si rychle osvojí i práci s nástrojem Azure PowerShell.

## <a name="example"></a>Příklad
Připomínáme, že vybíráte nástroj pro správu, abyste mohli vytvořit testovací prostředí pro aplikaci CRM. Vaši správci budou potřebovat provést dvě konkrétní úlohy v Azure:

1. Vytvořit jednu skupinu prostředků pro každou kategorii testování (jednotka, integrace a přijetí)
2. V každé skupině prostředků vytvořit před každým kolem testování několik virtuálních počítačů

K vytvoření skupin prostředků je vhodné použít web Azure Portal. Jsou to jednorázové úlohy, takže je nepotřebujete provádět pomocí skriptů.

Při výběru nejlepšího nástroje pro vytvoření virtuálních počítačů je už ale rozhodování náročnější. Potřebujete jich vytvořit několik a k tomu opakovaně, pravděpodobně několikrát týdně. To znamená, že budete potřebovat automatizaci, takže web Azure Portal pro vás není vhodným řešením. V takovém případě vyhoví vašim požadavkům Azure PowerShell nebo Azure CLI. Pokud už členové vašeho týmu znají prostředí PowerShell, zřejmě nejvhodnější pro něj bude nástroj Azure PowerShell. Nástroj Azure PowerShell je dostupný pro operační systémy, které váš tým správců používá, podporuje automatizaci a váš tým by si ho měl rychle osvojit.

Většina správců se s Azure poprvé setká prostřednictvím webu Azure Portal. Je to skvělý výchozí bod, protože poskytuje čisté, dobře strukturované grafické rozhraní, má ale jenom omezené možnosti automatizace. Pokud potřebujete automatizaci, máte v Azure dvě možnosti: Azure PowerShell pro správce, kteří mají zkušenosti s prostředím PowerShell, a Azure CLI pro všechny ostatní.

V praxi je obvyklé, že firmy mají směsici jednorázových a opakujících se úloh. To znamená, že běžně používají jak portál, tak řešení založené na skriptech. V našem příkladu s aplikací CRM doporučujeme vytvořit skupiny prostředků pomocí portálu a automatizovat vytváření virtuálních počítačů pomocí PowerShellu.

Zbytek tohoto modulu se věnuje instalaci a použití nástroje Azure PowerShell.
