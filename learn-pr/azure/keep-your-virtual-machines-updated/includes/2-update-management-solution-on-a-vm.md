Vedoucí IT vašeho hasičského sboru udržuje neveřejný web, který používají místní média. K aktualizaci obsahu mediálního webu používá mobilní zařízení, aby místní média měla informace o aktuálních událostech. Tento web musí být co nejvíce zabezpečený, aby se zabránilo zveřejnění nesprávných informací v médiích. Jako správce chcete zlepšit zabezpečení webu, a proto ho budete udržovat aktuální, aby měl nainstalované nejnovější aktualizace.

A tady přichází ke slovu řešení Update Management pro Azure.

## <a name="update-management-overview"></a>Přehled Update Managementu

 Řešení Update Management umožňuje spravovat a instalovat aktualizace a opravy operačního systému na virtuální počítače s Windows a Linuxem nasazené v Azure, místně nebo u jiných poskytovatelů cloudových služeb. Můžete vyhodnotit stav dostupných aktualizací na počítačích a řídit postup instalace požadovaných aktualizací serverů.  

 Řešení Update Management má několik výhod:
  1. Virtuální počítače nevyžadují agenty ani další konfiguraci.
  1. Aktualizace můžete spouštět i bez přihlášení k virtuálnímu počítači. Při instalaci aktualizací také nemusíte vytvářet hesla.
  1. Řešení Update Management zobrazí seznam chybějících aktualizací a nabídne přehledné informace o neúspěšném nasazení.

Update Management můžete nativně nasadit na počítače v různých předplatných stejného tenanta. Pokud chcete spravovat počítače, které jsou v různých tenantech, musíte je zavést jako počítače mimo Azure.

## <a name="supported-operating-systems"></a>Podporované operační systémy

Řešení Update Management podporuje Windows a Linux. Konkrétně podporuje tyto systémy:
 - Windows Server (2008 nebo novější)
 - CentOS 6 (x86/X64) a CentOS 7
 - Red Hat Enterprise 6 (x86/x64) a 7 (x64)
 - SUSE Linux Enterprise Server 11 (x86/x64) a 12 (x64)
 - Ubuntu 14.04 LTS, 16.04 LTS a 18.04 (x86/x64)

V tomto modulu budeme pracovat s virtuálním počítačem s Windows Serverem 2016 nasazeným v Azure.
 
## <a name="components-used-by-update-management"></a>Komponenty používané řešením Update Management

K vyhodnocení a nasazení aktualizací se používají následující konfigurace:

- Microsoft Monitoring Agent (MMA) pro Windows nebo Linux
- Konfigurace požadovaného stavu (DSC) PowerShellu pro Linux
- Funkce Hybrid Runbook Worker služby Automation
- Služby Microsoft Update nebo Windows Server Update Services pro počítače s Windows

Následující diagramy znázorňují principy chování a toku dat. Ukazují, jak toto řešení vyhodnocuje všechny připojené počítače s Windows Serverem a Linuxem v pracovním prostoru a používá pro ně aktualizace zabezpečení.

![Znázornění principů toku dat](../media/2-conceptual-view-data-flow50.png "Znázornění principů toku dat")

### <a name="hybrid-worker-groups"></a>Skupiny Hybrid Worker

 Počítače s Windows, které jsou přímo připojené k pracovnímu prostoru Log Analytics, se automaticky nakonfigurují jako Hybrid Runbook Worker, aby podporovaly sady runbook, které jsou součástí tohoto řešení. Každý počítač s Windows, který je spravovaný v tomto řešení, je v podokně se seznamem skupin Hybrid worker uvedený jako skupina systémových Hybrid Workerů účtu Automation. Řešení používají tento způsob označování počítačů: Název hostitele FQDN_GUID.

## <a name="operations-manager-management-packs"></a>Sady Management Pack Operations Manageru

Pokud je vaše skupina pro správu System Center Operations Manageru připojená k pracovnímu prostoru Log Analytics, nainstalují se do Operations Manageru následující sady Management Pack. Tyto sady Management Pack se po přidání do řešení nainstalují také přímo na připojené počítače s Windows. Sady Management Pack nemusíte konfigurovat ani spravovat:
- Microsoft System Center Advisor Update Assessment Intelligence Pack 
- Microsoft.IntelligencePack.UpdateAssessment.Configuration
- Update Deployment MP (Management Pack pro nasazení aktualizací)
