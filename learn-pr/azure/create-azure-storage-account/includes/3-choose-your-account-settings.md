Nastavení účtu úložiště, které jsme už probrali, platí pro datové služby v rámci účtu. V tomto tématu se budeme zabývat třemi nastaveními, které platí pro samotný účet, ne pro data uložená v něm:

- Název
- Model nasazení
- Druh účtu

Tato nastavení ovlivňují způsob správy účtu a náklady na služby v něm.

## <a name="name"></a>Název

Každý účet úložiště má svůj název. Název musí být globálně jedinečný v rámci Azure, musí obsahovat pouze číslice a malá písmena a musí být dlouhý 3 až 24 znaků.

## <a name="deployment-model"></a>Model nasazení

_Model nasazení_ je systém, který Azure používá k organizaci prostředků. Model definuje rozhraní API, které používáte k vytváření, konfiguraci a správě těchto prostředků. Azure nabízí dva modely nasazení:

- **Resource Manager**: Aktuální model, který používá rozhraní API Azure Resource Manageru.
- **Klasické**: Starší verze nabídky, která používá rozhraní API Azure Service Managementu.

Rozhodnutí, který model vybrat, je obvykle jednoduché, protože většina prostředků Azure funguje pouze s Resource Managerem. Účty úložiště, virtuální počítače a virtuální sítě ale podporují oba modely. Při vytváření účtu úložiště se tedy musíte rozhodnout pro jeden či druhý.

Klíčovým rozdílem mezi těmito dvěma modely je podpora seskupení. Model Resource Manager přidává koncept _skupiny prostředků_, který není v klasickém modelu k dispozici. Skupina prostředků vám umožňuje nasadit a spravovat kolekci prostředků jako jednu jednotku.

Microsoft doporučuje u všech nových prostředků používat **Resource Manager**.

## <a name="account-kind"></a>Druh účtu

_Druh_ účtu úložiště je sada zásad, která určuje, jaké datové služby může váš účet obsahovat a kolik za tyto služby zaplatíte. Existují tři druhy účtů úložiště:

- **StorageV2 (obecné účely v2)**: aktuální nabídka, která podporuje všechny druhy úložišť a všechny nejnovější funkce.
- **Storage (obecné účely v1)**: Starší verze, která podporuje všechny druhy úložišť, ale nemusí podporovat všechny funkce.
- **Blob Storage**: Starší verze, která umožňuje pouze blokovat a připojovat objekty blob.

Microsoft doporučuje u nových účtů úložiště používat možnost pro **obecné účely v2**.

Z tohoto pravidla existuje několik výjimek. Například ceny za transakce jsou ve verzi pro obecné účely v1 nižší, takže pokud to odpovídá vašemu typickému zatížení, mohli byste mírně snížit náklady.

Obecným doporučením je zvolit model nasazení **Správce prostředků** a u všech účtů úložiště zvolit druh účtu **StorageV2 (pro obecné účely v2)**. Ostatní možnosti stále existují hlavně proto, aby existující prostředky mohly dále fungovat. U nových prostředků existuje jenom málo důvodů, proč zvažovat ostatní možnosti.