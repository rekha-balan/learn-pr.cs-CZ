Předpokládejme, že pracujete jako architekt sítě v nadnárodní farmaceutické společnosti, která sekvenuje geny, a vytváří tak vlastní vzorce léčivých přípravků, na které se vztahuje obchodní tajemství. Tyto vzorce se používají ve výrobních závodech po celém světě. Společnost chce implementovat Microsoft Azure a přenést část funkcí, které se týkají sekvenace genů, na cloudové virtuální počítače. Výsledky sekvenace genů musí být dostupné v různých oblastech po celém světě. V současnosti probíhá zpracování dat v místních datacentrech, která jsou v jednotlivých oblastech propojená virtuální privátní sítí (VPN).

Dostali jste za úkol vyhodnotit, jak Microsoft Azure implementuje sítě, a zjistit, jestli má dostatečně zabezpečený přenos dat. Zabezpečený přenos dat se požaduje jak mezi místním datacentrem a Microsoft Azure, tak mezi jednotlivými oblastmi Microsoft Azure. Použijete technologie Azure Virtual Network, Azure VPN Gateway a Azure ExpressRoute.

## <a name="learning-objectives"></a>Cíle výuky

Na konci tohoto modulu budete umět:

- Vytvořit Azure Virtual Network.
- Vytvořit bránu Azure VPN Gateway.
- Identifikovat funkce a výhody služby Azure ExpressRoute.

## <a name="prerequisites"></a>Předpoklady

[!include[](prerequisites.md)]

> [!IMPORTANT]
> K cvičením v tomto modulu potřebujete plné předplatné Azure. Cvičení jsou volitelná, a nejsou k dokončení tohoto modulu potřeba. Pokud se zúčastníte interaktivních cvičení, která jsou v tomto modulu, budou se vám v předplatném Azure používaném k jejich dokončení účtovat poplatky.  Abyste tyto poplatky minimalizovali, vymažte co nejdříve vytvořené prostředky. Pokyny k vyčištění najdete v závěrečné lekci.