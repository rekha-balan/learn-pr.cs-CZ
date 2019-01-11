Máte místní datacentrum, které plánujete zachovat, ale chcete využít Azure k přesměrování provozu ve špičce pomocí virtuálních počítačů hostovaných v Azure. Potřebujete zachovat stávající schéma přidělování IP adres a síťová zařízení a současně zajistit zabezpečení všech přenosů dat.

## <a name="what-is-azure-virtual-networking"></a>Co jsou virtuální sítě Azure?

**Virtuální sítě Azure** umožňují prostředkům Azure, jako jsou virtuální počítače, webové aplikace a databáze, komunikovat navzájem mezi sebou, s uživateli na internetu a s místními klientskými počítači. Síť Azure si můžete představit jako sadu prostředků, které spojují další prostředky Azure.

Virtuální sítě Azure poskytují klíčové možnosti sítě:

- Izolace a segmentace
- Internetová komunikace
- Komunikace mezi prostředky Azure
- Komunikace s místními prostředky
- Směrování síťového provozu
- Filtrování síťového provozu
- Propojení virtuálních sítí
 
#### <a name="network-configurations-for-virtual-machines"></a>Konfigurace sítě pro virtuální počítače

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEve]

### <a name="isolation-and-segmentation"></a>Izolace a segmentace

V Azure můžete vytvořit několik izolovaných virtuálních sítí. Při nastavování virtuální sítě definujete privátní adresní prostor IP (Internet Protocol) adres s využitím rozsahů veřejných nebo privátních IP adres. Tento adresní prostor IP adres pak můžete segmentovat na podsítě a každé podsíti přiřadit část definovaného adresního prostoru.

K zajištění překladu názvů můžete využít službu překladu názvů integrovanou v Azure, nebo můžete virtuální síť nakonfigurovat tak, aby používala interní nebo externí server DNS (Domain Name System).

### <a name="internet-communications"></a>Internetová komunikace

Virtuální počítač v Azure se může automaticky připojit k internetu. Příchozí připojení z internetu povolíte tak, že definujete veřejnou IP adresu nebo veřejný nástroj pro vyrovnávání zatížení. Kvůli správě virtuálního počítače se můžete připojit prostřednictvím Azure CLI, protokolu RDP (Remote Desktop Protocol) nebo prostřednictvím protokolu SSH (Secure Shell).

### <a name="communicate-between-azure-resources"></a>Komunikace mezi prostředky Azure

Mezi prostředky Azure chcete povolit bezpečnou vzájemnou komunikaci. Můžete to provést dvěma způsoby:

- **Virtuální sítě**

    Virtuální sítě můžou propojit nejen virtuální počítače, ale i další prostředky Azure, jako jsou prostředí služby App Service, Azure Kubernetes Service a škálovací sady virtuálních počítačů Azure.

- **Koncové body služby**

     Pomocí koncových bodů služby se můžete připojit k dalším typům prostředků Azure, jako jsou databáze SQL Azure a účty úložiště. Tento přístup vám umožní propojit s virtuálními sítěmi více prostředků Azure a tím zlepšit zabezpečení a zajistit optimální směrování mezi prostředky.

### <a name="communicate-with-on-premises-resources"></a>Komunikace s místními prostředky

Virtuální sítě Azure umožňují vzájemně propojit prostředky ve vašem místním prostředí a v rámci vašeho předplatného Azure a vytvořit tak síť zahrnující místní i cloudové prostředí. Tohoto připojení můžete dosáhnout třemi způsoby:

- **Virtuální soukromé sítě typu Point-to-Site**

   Tento přístup se podobá připojení počítače, který je mimo organizaci, prostřednictvím sítě VPN (Virtual Private Network) zpět k podnikové síti jen s tím rozdílem, že funguje v opačném směru. V tomto případě klientský počítač zahájí šifrované připojení VPN k Azure, které tento počítač připojí k virtuální síti Azure.

- **Sítě VPN typu Site-to-Site** Síť tohoto typu připojí místní zařízení nebo bránu VPN k bráně Azure VPN ve virtuální síti. Díky tomu zařízení v Azure vypadají, jako by byla v místní síti. Připojení je šifrované a funguje po internetu.

- **Azure ExpressRoute**

    V prostředích, ve kterých potřebujete větší šířku pásma a vyšší úroveň zabezpečení, je nejlepším řešením Azure ExpressRoute. Azure ExpressRoute poskytuje vyhrazené privátní připojení k Azure, které nepoužívá internet.

### <a name="route-network-traffic"></a>Směrování síťového provozu

Azure bude automaticky směrovat provoz mezi podsítěmi v jakékoli připojené virtuální nebo místní síti a internetem. Směrování můžete řídit a můžete také přepsat nastavení následujícím způsobem:

- **Směrovací tabulky**

    Směrovací tabulka umožňuje definovat pravidla směrování provozu. Můžete vytvářet vlastní směrovací tabulky řídící způsob směrování paketů mezi podsítěmi.

- **Border Gateway Protocol**

    Protokol BGP (Border Gateway Protocol) ve spolupráci s branami Azure VPN nebo ExpressRoute šíří místní trasy protokolu BGP do virtuálních sítí Azure.

### <a name="filter-network-traffic"></a>Filtrování síťového provozu

Virtuální sítě Azure umožňují filtrovat provoz mezi podsítěmi pomocí následujících postupů:

- **Skupiny zabezpečení sítě**

    Skupina zabezpečení sítě je prostředek Azure, který může obsahovat několik pravidel zabezpečení pro příchozí a odchozí provoz. Tato pravidla můžete definovat tak, aby povolovala nebo blokovala provoz na základě faktorů, jako jsou zdrojová a cílová IP adresa, port a protokol.

- **Síťová virtuální zařízení**

    Síťové virtuální zařízení je specializovaný virtuální počítač, který je možné přirovnat k posílenému síťovému zařízení. Síťové virtuální zařízení provádí konkrétní síťovou funkci, jako je provozování brány firewall nebo optimalizace sítě WAN.

## <a name="connect-virtual-networks"></a>Propojení virtuálních sítí

Virtuální sítě mezi sebou můžete propojit pomocí _partnerského vztahu_ virtuálních sítí. Partnerský vztah umožňuje vzájemnou komunikaci prostředků v jednotlivých virtuálních sítích. Tyto virtuální sítě můžou být v různých oblastech, takže můžete prostřednictvím Azure vytvořit globální propojenou síť.

## <a name="azure-virtual-network-settings"></a>Nastavení virtuální sítě Azure

Virtuální sítě Azure můžete vytvářet a konfigurovat na webu Azure Portal, pomocí Azure PowerShellu na místním počítači nebo pomocí služby Azure Cloud Shell.

### <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě

Při vytváření virtuální sítě Azure konfigurujete několik základních nastavení. Můžete konfigurovat pokročilá nastavení, jako je více podsítí, ochrana před útoky DDoS (Distributed Denial of Service) a koncové body služby.

![Snímek obrazovky webu Azure Portal s příkladem polí v okně Vytvořit virtuální síť](../media/2-create-virtual-network.PNG)

U základní virtuální sítě budete konfigurovat následující nastavení:

- **Název sítě**

    Tento název musí být jedinečný v rámci vašeho předplatného, ale nemusí být jedinečný globálně. Použijte popisný název, který se dobře pamatuje a odlišuje síť od ostatních virtuálních sítí.

- **Adresní prostor**

    Při nastavování virtuální sítě definujete interní adresní prostor ve formátu CIDR (Classless Interdomain Routing). Tento adresní prostor musí být jedinečný jak v rámci předplatného, tak v rámci ostatních sítí, ke kterým se připojujete.

    Předpokládejme, že jste pro svou první virtuální síť zvolili adresní prostor 10.0.0.0/24. Adresy definované v tomto adresním prostoru jsou v rozsahu 10.0.0.1–10.0.0.254. Potom vytvořte druhou virtuální síť a zvolte jí adresní prostor 10.1.0.0./8. Tento adresní prostor definuje rozsah adres 10.0.0.1–10.255.255.254. Některé adresy se překrývají, a proto se nedají použít v obou virtuálních sítích.

    Adresu 10.0.0.0/16 můžete použít v rozsahu adres 10.0.0.1–10.0.255.254 a adresu 10.1.0.0/16 zase můžete použít v rozsahu adres 10.1.0.1–10.1.255.254. Tyto adresní prostory můžete přiřadit virtuálním sítím, protože se adresy nepřekrývají.

    > [!NOTE]
    > Adresní prostory můžete přidat i po vytvoření virtuální sítě.

- **Předplatné**

    Platí jen v případě, že máte více předplatných, ze kterých si můžete vybrat.

- **Skupina prostředků**

    Stejně jako jakýkoli jiný prostředek Azure musí i virtuální síť existovat ve skupině prostředků. Můžete vybrat existující skupinu prostředků nebo vytvořit novou.

- **Umístění**

    Vyberte umístění, ve kterém má virtuální síť existovat.

- **Podsíť**

    V rozsahu adres každé virtuální sítě můžete vytvořit jednu nebo několik podsítí, které rozdělí adresní prostor virtuální sítě. Směrování mezi podsítěmi pak bude záviset na výchozích trasách provozu, případně můžete definovat vlastní trasy. Alternativně můžete definovat jednu podsíť, která bude zahrnovat celý adresní prostor virtuální sítě.

    > [!NOTE]
    > Názvy podsítí musí začínat písmenem nebo číslicí, končit písmenem, číslicí nebo podtržítkem a můžou obsahovat jenom písmena, číslice, podtržítka, tečky nebo spojovníky.

- **Ochrana před útoky DDoS (Distributed Denial of Service)**

    Můžete vybrat ochranu před útoky DDoS úrovně Basic nebo Standard. Ochrana před útoky DDoS úrovně Standard je prémiová služba. Další informace o ochraně před útoky DDoS úrovně Standard najdete v článku o [službě Azure DDoS Protection Standard](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

- **Koncové body služby**

    Tady povolíte koncové body služby a pak v seznamu vyberete, které koncové body služby Azure chcete povolit. Mezi možnosti patří Azure Cosmos DB, Azure Service Bus, Azure Key Vault atd.

Jakmile tato nastavení nakonfigurujete, klikněte na tlačítko **Vytvořit**.

### <a name="define-additional-settings"></a>Definování dalších nastavení

Po vytvoření virtuální sítě můžete definovat další nastavení. Mezi ně patří:

- **Skupina zabezpečení sítě**

    Skupiny zabezpečení sítě obsahují pravidla zabezpečení umožňující filtrovat typ síťového provozu, který může přicházet do podsítí virtuálních sítí a síťových rozhraní a odcházet z nich. Skupinu zabezpečení sítě vytvoříte odděleně a pak ji přidružíte k virtuální síti.

- **Směrovací tabulka**

    Azure pro každou podsíť v rámci virtuální sítě Azure automaticky vytvoří směrovací tabulku a přidá do ní výchozí systémové trasy. Můžete však přidat vlastní směrovací tabulky a upravit tak provoz mezi virtuálními sítěmi.

Můžete také pozměnit koncové body služby.

![Snímek obrazovky webu Azure Portal znázorňující příklad okna pro úpravu nastavení virtuální sítě](../media/2-virtual-network-additional-settings.PNG)

### <a name="configure-virtual-networks"></a>Konfigurace virtuálních sítí

Po vytvoření virtuální sítě můžete jakékoli další nastavení změnit v okně Virtuální sítě na webu Azure Portal. Alternativně můžete změny provést pomocí příkazů PowerShellu nebo příkazů ve službě Cloud Shell.

![Snímek obrazovky webu Azure Portal znázorňující příklad okna pro konfiguraci virtuální sítě](../media/2-configure-virtual-network.PNG)

V dalších podoknech pak můžete nastavení zkontrolovat a změnit.
K těmto nastavením patří:

- Adresní prostory: K počáteční definici můžete přidat další adresní prostory.

- Připojená zařízení: K připojení počítačů použijte virtuální síť.

- Podsítě: Můžete přidat další podsítě.

- Partnerské vztahy: Virtuální sítě můžete propojit do uspořádáních partnerských vztahů.

Virtuální sítě můžete také monitorovat a odstraňovat jejich potíže. Případně můžete vytvořit automatizační skript, který vygeneruje aktuální virtuální síť.

Virtuální sítě představují výkonný a vysoce konfigurovatelný mechanismus propojení entit v Azure. Prostředky Azure můžete propojit mezi sebou nebo s místními prostředky. Síťový provoz můžete izolovat, filtrovat a směrovat a Azure vám v případě potřeby pomůže zvýšit zabezpečení.
