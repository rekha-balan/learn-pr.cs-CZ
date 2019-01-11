:::row:::
  :::column:::
    ![Obrázek představující kontejnery Azure](../media/4-azure-containers.png)
  :::column-end:::
  :::column span="3":::
Kontejnery jsou skvělou volbou, pokud chcete na jednom virtuálním počítači spustit více instancí aplikace. Instance se spouštějí, zastavují a škálují podle potřeby prostřednictvím orchestrátoru kontejnerů.
  :::column-end:::
:::row-end:::

Kontejnery jsou ze své podstaty jednoduché a jejich vytváření, škálování a zastavování probíhá dynamicky. Toto řešení umožňuje rychle reagovat na změny v poptávce nebo na selhání.

Další výhodou kontejnerů je možnost spouštět na jednom hostiteli virtuálních počítačů více izolovaných aplikací. Vzhledem k tomu, že kontejnery jsou zabezpečené a izolované, není nutné mít pro každou aplikaci samostatný virtuální počítač.

#### <a name="vms-versus-containers"></a>Virtuální počítače versus kontejnery

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuaq]

## <a name="containers-in-azure"></a>Kontejnery v Azure

Azure podporuje kontejnery Dockeru a nabízí několik možností jejich správy.

- Azure Container Instances (ACI)
- Azure Kubernetes Service (AKS)

### <a name="azure-container-instances"></a>Azure Container Instances

Služba Azure Container Instances (ACI) nabízí nejrychlejší a nejjednodušší způsob spuštění kontejneru v Azure. Nemusíte spravovat žádné virtuální počítače ani konfigurovat dodatečné služby. Je to nabídka PaaS, která umožňuje nahrát kontejnery a spouštět je přímo. 

### <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Automatizace a správa velkého množství kontejnerů a interakce s nimi se souhrnně označuje jako orchestrace. Azure Kubernetes Service (AKS) je kompletní orchestrační služba pro kontejnery s distribuovanými architekturami s více kontejnery. 

#### <a name="what-is-kubernetes"></a>Co je Kubernetes?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuX]

### <a name="using-containers-in-your-solutions"></a>Použití kontejnerů ve vašich řešeních

Kontejnery se často používají k vytváření řešení s využitím _architektury mikroslužeb_. Ta spočívá v rozdělení řešení na menší, nezávislé části. Můžete například rozčlenit web na kontejnery hostující front-end, respektive back-end, a třetí použít pro úložiště. To vám umožní umístit části aplikace do logických oddílů, které lze nezávisle na sobě udržovat, škálovat a aktualizovat.

#### <a name="what-is-a-microservice"></a>Co je mikroslužba?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yual]

Představte si, že back-end vašeho webu dosáhl plné kapacity, ale u front-endu a úložiště ještě nedochází k přetížení. Lepšího výkonu můžete dosáhnout škálováním samotného back-endu nebo se můžete rozhodnout pro použití jiné služby úložiště. Dalším řešením je dokonce i nahrazení kontejneru úložiště, aniž by to mělo vliv na zbytek aplikace.