Kromě veřejného webu používá hasičský sbor také web pro vnitřní obsah, jako jsou záznamy o výjezdech a o péči o pacienty. Tyto weby musí být co nejvíce zabezpečené.

Tady se naučíte, jak vyhodnotit připojení k agentovi a naplánovat pravidelné aktualizace.

## <a name="components-used-by-update-management"></a>Komponenty používané řešením Update Management

K vyhodnocení a nasazení aktualizací se používají následující konfigurace:

- Microsoft Monitoring Agent (MMA) pro Windows nebo Linux
- Konfigurace požadovaného stavu (DSC) PowerShellu pro Linux
- Funkce Hybrid Runbook Worker služby Automation
- Služby Microsoft Update nebo Windows Server Update Services pro počítače s Windows

## <a name="compliance-scan"></a>Kontrola kompatibility

Update Management provádí kontrolu kompatibility aktualizací. Ve výchozím nastavení se kontrola kompatibility provádí každých 12 hodin na počítači s Windows a každé 3 hodiny na počítači s Linuxem. Kromě plánovaných kontrol se kontrola kompatibility spustí do 15 minut při restartování agenta Microsoft Monitoring Agent (MMA), před instalací aktualizací a po jejich instalaci. Jakmile počítač zkontroluje kompatibilitu aktualizací, agent hromadně předá informace službě Azure Log Analytics.

Kromě plánovaných kontrol se kontrola kompatibility aktualizací spustí do 15 minut při restartování agenta Microsoft Monitoring Agent (MMA), před instalací aktualizací a po jejich instalaci.

Zobrazení aktuálních dat o spravovaných počítačích může trvat 30 minut nebo až 6 hodin.

## <a name="recurring-updates"></a>Pravidelné aktualizace

Můžete vytvořit plán pravidelného nasazení aktualizací. Při plánovaném nasazení můžete definovat, jaké cílové počítače obdrží aktualizace. Počítače můžete konkrétně určit nebo můžete vybrat skupinu počítačů, která vychází z prohledávání protokolů konkrétní sady počítačů. Můžete také zadat plán ke schválení a určit časové období, ve kterém se budou aktualizace instalovat.

Aktualizace se instalují po sadách runbook službou Azure Automation. Tyto sady runbook nemůžete zobrazit a nevyžadují konfiguraci. Při vytvoření nasazení aktualizací vytvoří nasazení aktualizací plán, který ve stanovený čas spustí hlavní runbook aktualizace pro zahrnuté počítače. Hlavní runbook spustí podřízený runbook na každém agentovi, který provádí instalaci požadovaných aktualizací.
