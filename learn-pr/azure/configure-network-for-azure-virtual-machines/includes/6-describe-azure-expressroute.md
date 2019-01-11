Vaše firma pracuje s velice citlivými daty a má velké množství informací, které bude ukládat v Azure. Proto existují určité obavy ohledně bezpečnosti a spolehlivosti připojení přes veřejný internet. Firma nechce hromadně migrovat do Azure, dokud nezíská prokazatelně vyšší úroveň připojení, zabezpečení a spolehlivosti.

Opustíme připojení, která používají internet, a přejdeme k vyhrazeným linkám směřujícím přímo do datacenter Azure.

## <a name="azure-expressroute"></a>Azure ExpressRoute

Microsoft Azure ExpressRoute umožňuje organizacím rozšířit jejich místní sítě do Microsoft Cloudu prostřednictvím privátního připojení implementovaného poskytovatelem připojení. Toto uspořádání znamená, že připojení k datacentrům Azure se neuskutečňuje po internetu, ale po vyhrazené lince. ExpressRoute také usnadňuje efektivní připojení k dalším cloudovým službám Microsoftu, jako je Office 365 a Dynamics 365.

Mezi výhody, které ExpressRoute poskytuje, patří:

- Vyšší rychlost 50 MB/s až 10 GB/s s dynamickým škálováním šířky pásma

- Nižší latence

- Větší spolehlivost díky integrovanému peeringu

- Vysoce bezpečné

ExpressRoute poskytuje řadu dalších výhod, například:

- Možnosti připojení ke všem podporovaným službám Azure

- Možnosti globálního připojení ke všem oblastem (vyžaduje doplněk Premium)

- Dynamické směrování přes protokol BGP (Border Gateway Protocol)

- Smlouvy o úrovni služeb (SLA) na dostupnost připojení

- Technologie QoS (Quality of Service) pro Skype pro firmy

K dispozici je také doplněk ExpressRoute Premium, který nabízí výhody, jako jsou zvýšené limity tras, globální připojení ke službám a vyšší počet linek virtuálních sítí na jeden okruh.

## <a name="expressroute-connectivity-models"></a>Modely připojení ExpressRoute

Připojení k ExpressRoute používají následující mechanismy:

- Síť IPVPN (typu any-to-any)

- Virtuální křížové připojení prostřednictvím ethernetové výměny

- Ethernetové připojení typu Point-to-Point

 Funkce a možnosti ExpressRoute jsou u všech výše uvedených modelů připojení identické.

### <a name="what-is-layer-3-connectivity"></a>Co je připojení vrstvy 3?

Microsoft používá k výměně tras mezi vaší místní sítí, vašimi instancemi v Azure a veřejnými adresami Microsoftu protokol dynamického směrování (BGP), který se považuje za oborový standard. S vaší sítí navážeme u různých profilů přenosu několik relací protokolu BGP.

### <a name="any-to-any-ipvpn-networks"></a>Sítě typu any-to-any (IPVPN)

Poskytovatelé sítě IPVPN obvykle poskytují připojení mezi pobočkami a podnikovým datacentrem přes spravovaná připojení vrstvy 3. Díky ExpressRoute se datacentra Azure jeví jako další pobočka.

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a>Virtuální křížové připojení prostřednictvím ethernetové výměny

Pokud se vaše organizace nachází na stejném místě jako zařízení cloudové výměny, můžete požadovat křížová připojení k Microsoft Cloudu prostřednictvím ethernetové výměny vašeho poskytovatele. Tato křížová připojení k Microsoft Cloudu se můžou provozovat na připojeních vrstvy 2 nebo spravovaných připojeních vrstvy 3 stejně jako v modelu sítí OSI.

### <a name="point-to-point-ethernet-connection"></a>Ethernetové připojení typu Point-to-Point

Ethernetové linky typu Point-to-Point můžou poskytovat připojení vrstvy 2 nebo spravovaná připojení vrstvy 3 mezi místními datacentry nebo pobočkami a Microsoft Cloudem.

## <a name="how-expressroute-works"></a>Jak ExpressRoute funguje

Azure ExpressRoute poskytuje připojení k Microsoft Cloudu s velkou šířkou pásma s využitím kombinace okruhů ExpressRoute a domén směrování.

### <a name="what-are-expressroute-circuits"></a>Co jsou okruhy ExpressRoute

Okruh ExpressRoute je logické propojení mezi vaší místní infrastrukturou a Microsoft Cloudem. Toto propojení implementuje poskytovatel připojení, i když některé organizace používají kvůli redundanci několik poskytovatelů připojení. Každý okruh má pevnou šířku pásma 50, 100, 200 nebo 500 Mb/s a 1 Gb/s nebo 10 Gb/s a každý z těchto okruhů se mapuje na poskytovatele připojení a peeringové umístění. Kromě toho platí pro každý okruh ExpressRoute výchozí kvóty a omezení.

Okruh Express Route neodpovídá síťovému připojení ani síťovému zařízení. Každý okruh má definovaný identifikátor GUID, který se označuje jako _služba_ nebo _klíč s-key_. Tento klíč s-key zajišťuje propojení mezi Microsoftem, vaším poskytovatelem připojení a vaší organizací, ale nejde o kryptografický tajný klíč. Každý klíč s-key je namapovaný na okruh Azure ExpressRoute ve vztahu 1:1.

Každý okruh může mít až tři peeringy, což jsou páry relací protokolu BGP nakonfigurované pro zajištění redundance. Jedná se o tyto peeringy:

- Privátní Azure
- Veřejný Azure
- Partnerský vztah Microsoftu

### <a name="routing-domains"></a>Domény směrování

Okruhy ExpressRoute se pak mapují na domény směrování, přičemž každý okruh ExpressRoute má několik domén směrování. Tyto domény jsou stejné jako tři výše uvedené peeringy. V konfiguraci aktivní-aktivní má každý pár směrovačů nakonfigurované všechny domény směrování stejně, a tak zajišťují vysokou dostupnost. Názvy veřejného a privátního peeringu Azure představují schémata přidělování IP adres.

#### <a name="azure-private-peering"></a>Soukromý partnerský vztah Azure

Privátní peering Azure se připojuje k výpočetním službám Azure, jako jsou virtuální počítače a cloudové služby nasazené prostřednictvím virtuální sítě. Co se týče zabezpečení, doména soukromého partnerského vztahu je jednoduše rozšířením místní sítě do Azure. Potom povolíte obousměrné připojení mezi touto sítí a jakoukoli virtuální sítí Azure, a tím zviditelníte IP adresy virtuálních počítačů Azure v rámci interní sítě.

> [!NOTE]
> K doméně privátního peeringu můžete připojit jen jednu virtuální síť.

#### <a name="azure-public-peering"></a>Veřejný partnerský vztah Azure

Veřejný partnerský vztah Azure umožňuje privátní připojení ke službám dostupným na veřejných IP adresách, jako jsou Azure Storage, databáze SQL Azure a webové služby Azure. Pomocí veřejného partnerského vztahu se můžete připojit k veřejným IP adresám těchto služeb bez směrování provozu po internetu. Připojení probíhá vždy z vaší sítě WAN do Azure, nikoli naopak. Zároveň se jedná o přístup typu „všechno nebo nic“, protože nemůžete vybrat služby, pro které chcete veřejný partnerský vztah povolit.

> [!NOTE]
> V případě služeb Azure PaaS se doporučuje místo veřejného partnerského vztahu použít partnerský vztah Microsoftu.

#### <a name="microsoft-peering"></a>Partnerský vztah Microsoftu

Partnerský vztah Microsoftu podporuje připojení ke cloudovým nabídkám SaaS, jako je Office 365 a Dynamics 365. Tato možnost partnerského vztahu zajišťuje obousměrné připojení mezi sítí WAN vaší společnosti a cloudovými službami Microsoftu.

### <a name="expressroute-health"></a>Stav ExpressRoute

Připojení ExpressRoute můžete – stejně jako většinu funkcí Microsoft Azure – monitorovat, abyste zajistili jejich uspokojivé fungování. Monitorování zahrnuje pokrytí následujících oblastí:

- Dostupnost
- Připojení k virtuálním sítím
- Využití šířky pásma

Klíčovým monitorovacím nástrojem je Network Performance Monitor, konkrétně NPM pro ExpressRoute.

Azure ExpressRoute se používá k vytvoření privátních spojení mezi datacentry Azure a místní infrastrukturou nebo infrastrukturou okolního prostředí. Připojení ExpressRoute neprocházejí veřejným internetem, jsou spolehlivější, rychlejší a mají nižší latenci než typická internetová připojení.
