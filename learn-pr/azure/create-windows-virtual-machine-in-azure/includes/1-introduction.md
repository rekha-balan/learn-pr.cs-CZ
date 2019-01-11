Představte si, že pracujete pro společnost, která zpracovává videodata a analyzuje v nich vzorce. Vytváříte prototyp nové platformy, která bude zpracovávat videa z dopravních kamer, analyzovat trendy a poskytovat užitečná data pro zlepšení provozu a stavu silnic. 

Abyste své algoritmy zlepšili, dohodli jste se s několika novými městy na shromažďování dat z jejich dopravních kamer. Ne všechna videodata jsou však ve stejném formátu a k dekódování dat v řadě z těchto formátů existují kodeky pouze pro Windows. Z tohoto důvodu jste se rozhodli k počátečnímu zpracování dat využít virtuální počítače a pak data odesílat do Azure Functions, které už budou zpracovávat standardní formát. Tento přístup vám umožní dynamicky přijímat nové formáty dat bez nutnosti zastavit celý systém.

Azure poskytuje robustní řešení hostování virtuálních počítačů, které může splnit vaše požadavky. Pojďme zjistit, jak v Azure vytvořit virtuální počítače s Windows a jak s nimi pracovat.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Pochopit možnosti dostupné pro virtuální počítače v Azure.
- Vytvořit virtuální počítače s Windows pomocí webu Azure Portal.
- Připojit se ke spuštěnému virtuálnímu počítači s Windows pomocí Vzdálené plochy
- Nainstalovat software a změnit konfiguraci sítě na virtuálním počítači na webu Azure Portal.

## <a name="prerequisites"></a>Požadavky

- Základní porozumění Azure Virtual Machines z **Úvod k Azure Virtual Machines**
- Klient Vzdálené plochy