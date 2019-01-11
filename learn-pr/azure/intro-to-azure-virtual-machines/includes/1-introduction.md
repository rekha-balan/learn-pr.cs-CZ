Řekněme, že pracujete pro společnost zabývající se lékařským výzkumem a jste zodpovědní za správu místních serverů. Na serverech, které spravujete, běží veškerá infrastruktura společnosti od webových serverů po databáze. Hardware ale stárne a přestává se mu dařit držet krok s některými novými aplikacemi pro analýzu dat, které jsou v něm nasazené.

Jednou z možností je upgradovat veškerý hardware, což není vhodné z několika důvodů:

1. Servery jsou fyzicky rozmístěny po celém světě s minimálním počtem zaměstnanců v jednotlivých oblastech. Rádi bychom centralizovali upgrade na naši domovskou pobočku.

1. Společnost provozuje software pro analýzu vlastních dat na několika verzích a edicích Windows a Linuxu, v některých případech ve zvláštních konfiguracích, kterým nikdo úplně nerozumí. Před přesunem práce potřebujeme způsob, jak otestovat celé nasazení a vyzkoušet různé konfigurace, abychom měli jistotu, že vše funguje.

1. Podnikání je úspěšné a společnost se rychle rozrůstá. Je pravděpodobné, že zatížení interních serverů, zejména databází, bude i nadále stoupat. Abychom nárůst zvládli, budeme muset v budoucnosti nakupovat nebo vytvořit plán škálovatelnosti.

Z těchto důvodů se rozhodnete, že je čas prozkoumat cloud a podívat se, jestli může pomoct vyřešit problémy se zatížením a škálováním. Vzhledem k tomu, že máte spoustu smíšených serverů a vlastního softwaru, je vhodné zkusit přesouvat servery jeden po druhém do Azure pomocí Azure Virtual Machines.

Azure Virtual Machines je jedním z několika typů škálovatelných výpočetních prostředků, které Azure nabízí. S Virtual Machines máte úplnou kontrolu nad konfigurací a můžete nainstalovat, cokoli potřebujete k provedení práce. V případě potřeby škálování nebo rozšíření datového centra není nutné kupovat nový hardware. Azure navíc poskytuje další služby pro monitorování, zabezpečení a správu aktualizací a oprav operačního systému.

Podíváme se, o čem je potřeba rozhodnout před vytvořením virtuálního počítače, jaké jsou možnosti vytvoření a správy virtuálních počítačů a jaké služby a rozšíření můžete použít k jejich správě.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Sestavit kontrolní seznam pro vytvoření virtuálního počítače
- Popsat možnosti pro vytvoření a správu virtuálních počítačů
- Popsat další služby dostupné ke správě virtuálních počítačů
