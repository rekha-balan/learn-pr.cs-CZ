Dejme tomu, že jste správcem systému v rostoucí firmě poskytující finanční služby. Analytici vytvářejí finanční modely, které investorům pomáhají doporučit nejlepší investice.

Tyto finanční modely se provozují na virtuálních počítačích v Azure. Model může běžet od několika minut až po několik hodin. Zpravidla vytvoříte nový virtuální počítač pro spuštění každého nového modelu, který potom, co analytik získá výsledky, odstraníte.

Podnikání v oblasti finančních služeb se vyznačuje rychlostí. Zjistili jste, že neustále nasazujete a odstraňujete cloudové prostředky. Zpočátku jste prostředky vytvářeli na webu Azure Portal a teď používáte k rozsáhlejší automatizaci Azure CLI a skripty. I když různá nasazení mají určité společné rysy, propojení virtuálních počítačů se síťovými a úložnými komponentami chvíli trvá. Na konci každého cyklu potřebujete odstranit jen komponenty související s tímto cyklem.

Jak dokážete udržet krok s tempem vašich analytiků? Šablony Azure Resource Manageru představují jeden ze způsobů, jak lze nasazení dále automatizovat.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu:

- Zjistíte, co je Azure Resource Manager a co obsahuje šablona Resource Manageru
- Nasadíte virtuální počítač pomocí Azure CLI a předem sestavené šablony Resource Manageru
- Přizpůsobíte šablonu Resource Manageru pro konfiguraci základního webového serveru na virtuálním počítači

## <a name="prerequisites"></a>Předpoklady

- Zkušenost s prostředím Bash a Azure CLI
- Zkušenost s prostředky a skupinami prostředků Azure