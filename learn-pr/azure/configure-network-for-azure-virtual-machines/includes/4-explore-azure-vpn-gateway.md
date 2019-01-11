Pokud chcete integrovat místní prostředí do Azure, potřebujete mít možnost vytvořit šifrované připojení. K připojení můžete použít internet nebo vyhrazenou linku. Podíváme se na službu Azure VPN Gateway, která poskytuje koncový bod pro příchozí připojení z místních prostředí.

Nastavili jste virtuální síť Azure a potřebujete zajistit šifrování všech přenosů dat z Azure k vám nebo mezi virtuálními sítěmi Azure. Potřebujete také vědět, jak propojit virtuální sítě mezi oblastmi a předplatnými.

## <a name="what-is-a-vpn-gateway"></a>Co je VPN Gateway?

Azure VPN Gateway poskytuje koncový bod pro příchozí šifrovaná připojení z místních umístění do Azure po internetu. K posílání zašifrovaných dat mezi virtuálními sítěmi Azure také může používat vyhrazenou síť Microsoftu, která propojuje datacentra Azure v různých oblastech. Tato konfigurace umožňuje bezpečně propojit virtuální počítače a služby v různých oblastech.

Každá virtuální síť může mít pouze jednu bránu VPN. Všechna připojení k této bráně VPN sdílí dostupnou šířku pásma sítě.

V každé bráně virtuální sítě jsou dva nebo více virtuálních počítačů. Tyto virtuální počítače byly nasazeny do zvláštní podsítě, kterou zadáte a která se označuje jako _podsíť brány_. Obsahují směrovací tabulky pro připojení k jiným sítím a také určité služby brány. Tyto virtuální počítače a podsíť brány se podobají posílenému síťovému zařízení. Tyto virtuální počítače není potřeba přímo konfigurovat a do podsítě brány byste neměli nasazovat žádné další prostředky.

Vytvoření brány virtuální sítě může nějakou dobu trvat, takže je důležité ho správně naplánovat. Když vytvoříte bránu virtuální sítě, proces zřizování vygeneruje virtuální počítače brány a nasadí je do podsítě brány. Tyto virtuální počítače budou mít nastavení, která nakonfigurujete v bráně.

Klíčové nastavení je **_typ brány_**, který bude pro bránu sítě VPN typu vpn. K možnostem bran VPN patří:

- Připojení typu Network-to-Network přes tunelování VPN IPsec/IKE propojující brány VPN s dalšími bránami VPN.

- Tunelování VPN IPsec/IKE mezi různými místy pro připojení místních sítí k Azure prostřednictvím vyhrazených zařízení VPN a vytvoření připojení typu Site-to-Site.

- Připojení typu Point-to-Site přes protokol IKEv2 nebo SSTP pro připojení klientských počítačů k prostředkům v Azure.

Teď se podíváme na faktory, které musíte vzít v úvahu při plánování brány VPN.

## <a name="plan-a-vpn-gateway"></a>Plánování brány VPN

Při plánování brány VPN si můžete vybrat ze tří různých architektur:

- Připojení typu Point-to-Site přes internet
- Připojení typu Site-to-Site přes internet
- Připojení typu Site-to-Site přes vyhrazenou síť, jako je Azure ExpressRoute

### <a name="planning-factors"></a>S čím je potřeba počítat při plánování

Při plánování je potřeba vzít v úvahu následující skutečnosti:

- Propustnost – MB/s nebo GB/s
- Páteřní síť – Internet nebo privátní?
- Dostupnost veřejné (statické) IP adresy
- Kompatibilita zařízení VPN
- Více klientských připojení nebo propojení typu Site-to-Site?
- Typ brány sítě VPN
- Skladová položka služby Azure VPN Gateway

Následující tabulka shrnuje některé z těchto problémů s plánováním. Zbývající problémy budeme probírat později.

|                           |  Point-to-Site            | Site-to-Site                          |  ExpressRoute                 |
| -------------             | -------------             | -------------                         | ---------                     |
| Podporované služby Azure  | Cloudové služby a virtuální počítače    | Cloudové služby a virtuální počítače                | Všechny podporované služby        |
| Typická šířka pásma         | Závisí na skladové položce VPN Gateway    | Až 1 Gb/s s agregací         | 50 MB/s až 10 GB/s       |
| Podporované protokoly       | SSTP a IPSec            | IPsec                                 | Přímé připojení, sítě VLAN      |
| Směrování                   | RouteBased (dynamické)      | PolicyBased (statické) a RouteBased   | BGP                           |
| Odolnost připojení     | Aktivní-pasivní            | Aktivní-pasivní nebo aktivní-aktivní       | Aktivní-aktivní                 |
| Případ použití                  | Testování a vytváření prototypů   | Vývoj, testování a ostrý provoz v malém měřítku  | Nepostradatelné pro organizaci/cíle   |

### <a name="gateway-skus"></a>Skladové položky brány

Azure pro služby brány nabízí následující skladové položky:

| Skladová položka              |  Tunely S2S/Network-to-Network | Připojení P2S  |  Srovnávací test agregované propustnosti   | Použití pro                         |
| -------------    | -------------             | -------------    | ---------                         | ---------                       |
| Basic            | Max. 10                    | Max. 128          | 100 Mb/s                          | Vývoj, testování nebo testování konceptu                    |
| VpnGw1           | Max. 30                    | Max. 128          | 650 Mb/s                          | Produkční nebo důležité úlohy   |
| VpnGw2           | Max. 30                    | Max. 128          | 1 Gb/s                            | Produkční nebo důležité úlohy   |
| VpnGw3           | Max. 30                    | Max. 128          | 1,25 Gb/s                          | Produkční nebo důležité úlohy   |

> [!Note]
> Je důležité vybrat správnou skladovou položku. Pokud bránu VPN vytvoříte pomocí nesprávné skladové položky, budete ji muset vyřadit a bránu znovu vytvořit, což může být časově náročné.

## <a name="workflow"></a>Pracovní postup

Při návrhu strategie připojení ke cloudu s využitím virtuálních privátních sítí v Azure byste měli použít následující pracovní postup:

1. Navrhněte topologii připojení, která uvádí adresní prostor pro všechny připojující se sítě.

1. Vytvořte virtuální síť Azure.

1. Vytvořte pro virtuální síť bránu sítě VPN.

1. Podle potřeby vytvořte a nakonfigurujte připojení k místním sítím nebo jiným virtuálním sítím.

1. V případě potřeby vytvořte a nakonfigurujte připojení typu Point-to-Site pro vaši bránu VPN Azure.

### <a name="design-considerations"></a>Co vzít v úvahu při návrhu

Při návrhu bran VPN pro propojení virtuálních sítí je potřeba zvážit následující faktory:

- Podsítě se nesmí překrývat.

    Je důležité, aby podsíť v jedné lokalitě neobsahovala stejný adresní prostor jako podsíť v jiné lokalitě.

- IP adresy musí být jedinečné.

    Nemůžete mít dva hostitele se stejnou IP adresou v různých lokalitách, protože nebude možné směrovat provoz mezi těmito dvěma hostiteli a připojení typu Network-to-Network selže.

- Brány VPN potřebují svou podsíť s názvem **GatewaySubnet**.

    Brána musí mít tento název, aby fungovala, a nesmí obsahovat žádné jiné prostředky.

### <a name="create-an-azure-virtual-network"></a>Vytvoření virtuální sítě Azure

Před vytvořením brány VPN je potřeba vytvořit virtuální síť Azure.

### <a name="create-a-vpn-gateway"></a>Vytvoření brány VPN

Typ brány VPN, kterou vytvoříte, bude záviset na vaší architektuře. Dostupné možnosti:

- RouteBased

    Zařízení VPN založená na směrování využívají selektory provozu typu Any-to-Any (zástupný znak) a umožňují směrovacím a předávacím tabulkám směrovat provoz do různých tunelů IPsec. Připojení založená na směrování obvykle využívají platformy směrovačů, kde je každý tunel IPsec modelovaný jako síťové rozhraní nebo virtuální rozhraní tunelu (VTI).

- PolicyBased

    Zařízení VPN založená na zásadách používají k definování způsobu šifrování a dešifrování provozu procházejícího tunely IPsec kombinace předpon z obou sítí. Připojení založené na zásadách obvykle využívá zařízení brány firewall, která provádějí filtrování paketů. Šifrování a dešifrování tunelů IPsec se přidává do modulu filtrování a zpracování paketů.

## <a name="set-up-a-vpn-gateway"></a>Nastavení brány VPN

Kroky, které je potřeba provést, budou záviset na typu instalované brány VPN. Například k vytvoření brány VPN typu Point-to-Site pomocí webu Azure Portal bude potřeba provést následující kroky:

1. Vytvoření virtuální sítě

2. Přidání podsítě brány

3. Určení serveru DNS (volitelné)

4. Vytvoření brány virtuální sítě

5. Vygenerování certifikátů

6. Přidání fondu adres klienta

7. Konfigurace typu tunelu

8. Konfigurace typu ověřování

9. Nahrání dat veřejného klíče kořenového certifikátu

10. Instalace exportovaného klientského certifikátu

11. Vygenerování a instalace konfiguračního balíčku klienta VPN

12. Připojení k Azure

Vzhledem k tomu, že brány VPN Azure umožňují několik způsobů konfigurace s různými možnostmi, není možné v tomto kurzu probrat všechna nastavení. Další informace najdete v části Další zdroje.

## <a name="configure-the-gateway"></a>Konfigurace brány

Vytvořenou bránu bude potřeba nakonfigurovat.  Budete muset nakonfigurovat různá nastavení, jako je název, umístění, server DNS atd. Podrobněji se jim budeme věnovat ve cvičení.

Brány Azure VPN Gateway jsou součástí virtuálních sítí Azure. Umožňují připojení typu Point-to-Site, Site-to-Site nebo Network-to-Network. Brány VPN Azure umožňují připojit jednotlivé klientské počítače k prostředkům v Azure, rozšířit místní sítě do Azure nebo propojit virtuální sítě v různých oblastech a předplatných.
