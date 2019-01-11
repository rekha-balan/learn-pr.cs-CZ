Azure nabízí tři hlavní způsoby nastavení virtuálních sítí:

- Virtuální sítě Azure
- Brány VPN Azure
- Azure ExpressRoute

Virtuální sítě Azure propojují prostředky (například virtuální počítače a škálovací sady virtuálních počítačů) ve stejné oblasti, aby spolu mohly komunikovat. Virtuální sítě Azure se také umí připojit k zadaným koncovým bodům služeb Azure, jako je Azure Storage, databáze a webové aplikace.

Brány Azure VPN Gateway umožňují komunikovat s místními klienty nebo sítěmi přes veřejný internet nebo propojit virtuální sítě v různých oblastech Azure. Pokud potřebujete vyhrazenou trasu s vysokým stupněm zabezpečení, použijte Azure ExpressRoute. Tato služba vytvoří privátní širokopásmové připojení k datacentrům Azure, které nabízí nejvyšší míru spolehlivosti a zabezpečení.

## <a name="cleanup"></a>Vyčištění

V interaktivních cvičeních jste v tomto modulu vytvořili dvě skupiny prostředků: `VpnGatewayDemo` a `vm-networks`. Odstraňte tyto skupiny prostředků, abyste vyčistili prostředky vytvořené při cvičeních.