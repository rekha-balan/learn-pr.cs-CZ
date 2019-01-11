Globální společnost, která pořádá automobilové závody, si vás najala kvůli modernizaci celé monitorovací a webové platformy. Rozhodli jste se nahradit stávající linuxové servery cloudovou infrastrukturou, která využívá nejnovějších trendů v architektuře. Část systému poběží na bezserverové platformě Azure a bude využívat Azure Functions ke zpracování závodních dat v reálném čase a bude odesílat statistiky, závodní data a další relevantní části analyzovaných informací do clusterů databází. Společnost chce zachovat stávající web, který byl přepracován teprve loni, ale chce ho připojit k tomuto modernímu datovému streamu.

Web běží na Apache s Linuxem, a protože je už v provozu, rozhodnete se ho přesunout přímo do Azure s využitím virtuálního počítače Azure. Tím zajistíte, že web bude mít přístup k datům s minimálním úsilím na vaší straně.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Pochopit možnosti dostupné pro virtuální počítače v Azure
- Vytvořit virtuální počítač s Linuxem pomocí webu Azure Portal
- Připojit se ke spuštěnému virtuálnímu počítači s Linuxem pomocí SSH
- Nainstalovat software a změnit na virtuálním počítači konfiguraci sítě prostřednictvím webu Azure Portal