Vytváření skriptů pro správu je výkonný způsob, jak optimalizovat pracovní postup. Můžete automatizovat časté opakující se úlohy a jakmile se skript ověří, poběží neustále a pravděpodobně sníží počet chyb.

Předpokládejme, že pracujete ve firmě, která k testování softwaru řízení vztahů se zákazníky (CRM) používá virtuální počítače Azure. Tyto virtuální počítače se vytváří z imagí, které obsahují webový front-end, webovou službu implementující obchodní logiku a databázi SQL.

Na jediném virtuálním počítači jste provedli několik cyklů testů, ale všimli jste si, že změny v databázi a konfiguračních souborech můžou způsobit nekonzistentní výsledky. V jednom případě vytvořila chyba záznam telefonního hovoru bez odpovídajícího zákazníka z databáze. Osamocený záznam způsoboval selhání následujících testů integrace i po opravení chyby. Tento problém chcete vyřešit použitím nového nasazení virtuálního počítače u každého testovacího cyklu. Vytvoření virtuálního počítače chcete automatizovat, protože k němu bude v průběhu týdne docházet mnohokrát. 

Zde uvidíte, jak pomocí Azure PowerShellu spravovat prostředky Azure. Prostředí Azure PowerShell interaktivně použijete k jednorázovým úlohám i psaní skriptů k automatizaci opakovaných úloh. 

## <a name="learning-objectives"></a>Cíle výuky
V tomto modulu se naučíte:

- Rozhodnout, zda je prostředí Azure PowerShell tím nejvhodnějším nástrojem pro vaše úkoly správy Azure
- Nainstalovat Azure PowerShell na Linux, macOS a Windows
- Připojit se k předplatnému Azure prostřednictvím Azure PowerShellu
- Vytvořit prostředky Azure pomocí Azure PowerShellu

## <a name="prerequisites"></a>Požadavky

- Zkušenost s rozhraním příkazového řádku (například PowerShell nebo Bash)
- Znalosti základních konceptů Azure (například skupin prostředků) a virtuálních počítačů
- Zkušenosti se správou prostředků Azure prostřednictvím webu Azure Portal
