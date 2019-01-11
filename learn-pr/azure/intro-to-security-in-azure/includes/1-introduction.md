Každý systém, architekturu a aplikaci je potřeba navrhnout s ohledem na zabezpečení. Riskujete zkrátka příliš mnoho. Útok DoS (odepření služby) by vám například zabránil v podnikání. Poškození vzhledu webu by poškodilo vaši pověst. A v nejhorším by mohlo dojít k porušení zabezpečení dat, protože to by mohlo vést ke ztrátě tvrdě vybudované důvěry a způsobit tak významné osobní a finanční škody. Jako správci, vývojáři a správci IT musíme všichni pracovat na tom, abychom zajistili zabezpečení našich systémů.

Řekněme, že pracujete ve společnosti s názvem Contoso Shipping. Jste průkopníky doručování zásilek prostřednictvím dronů ve venkovských oblastech, ale řidiči nákladních automobilů používají pro doručování ve městech mobilní aplikace. Právě se zabýváte přesouváním velké části infrastruktury Contoso Shipping do cloudu, abyste maximalizovali efektivitu, a několika fyzických serverů z firemního datacentra do virtuálních počítačů Azure. Váš tým plánuje vytvořit hybridní řešení s několika servery, které zůstanou jako místní, budete proto potřebovat zabezpečené a vysoce kvalitní propojení mezi novými virtuálními počítači a existující sítí.

![Obrázek ukazující koncepci zabezpečení s ochranou umístěnou mezi místní a cloudovou sítí](../media/1-heading.png)

Součástí provozu společnosti Contoso Shipping je navíc několik zařízení, která jsou mimo síť. V dronech používáte síťová čidla, která posílají data do Azure Event Hubs, a doručovatelé používají mobilní aplikace pro navigaci a zaznamenávají podpisy při převzetí zásilek. Tato zařízení a aplikace musí být bezpečně ověřeny, než mohou data odeslat nebo přijmout.

Jak tedy data zabezpečit?

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Jak se s Azure sdílí odpovědnost za zabezpečení.
- Jak správa identit poskytuje ochranu i mimo vaši síť
- Jak možnosti šifrování integrované v Azure můžou chránit vaše data
- Jak můžete chránit svoji síť a virtuální sítě

## <a name="prerequisites"></a>Požadavky  

Žádné