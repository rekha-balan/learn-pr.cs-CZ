Úspěšnost firmy poskytující služby často přímo souvisí se smlouvami o úrovni služeb, které má tato firma uzavřené se svými zákazníky. Vaši zákazníci očekávají, že služby, které poskytujete, budou vždy dostupné a že jejich data budou v bezpečí. To je něco, co Microsoft bere velmi vážně. Azure poskytuje nástroje, které můžete použít ke správě dostupnosti, zabezpečení dat a monitorování, takže víte, že vaše služby jsou vašim zákazníkům vždy dostupné.

Správa virtuálního počítače Azure se neomezuje na správu operačního systému nebo softwaru, který se na něm provozuje. Pomáhá zjistit, jaké služby poskytované Azure zajišťují dostupnost služeb a podporu automatizace. Tyto služby vám pomohou naplánovat strategii provozní kontinuity a zotavení po havárii.

Zde si ukážeme službu Azure, která zlepšuje dostupnost virtuálních počítačů, zefektivňuje úlohy jejich správy a udržuje data virtuálních počítačů zálohovaná a v bezpečí. Začněme definováním dostupnosti.

## <a name="what-is-availability"></a>Co je dostupnost?

Dostupnost je procento času, kdy je služba dostupná k používání.

Předpokládejme, že máte nějaký web a chcete, aby zákazníci měli kdykoli přístup k informacím. Vaše očekávání je 100% dostupnost týkající se přístupu k webu.

### <a name="why-do-i-need-to-think-about-availability-when-using-azure"></a>Proč při používání Azure musíte uvažovat o dostupnosti?

Virtuální počítače Azure běží na fyzických serverech umístěných v datových centrech Azure. U většiny fyzických zařízení se může stát, že dojde k selhání. Pokud selže fyzický server, selžou rovněž virtuální počítače hostované na tomto serveru. V takovém případě přesune Azure virtuální počítač automaticky na funkční hostitelský server. Tato samoopravná migrace ale může trvat několik minut, během kterých nebudou aplikace hostované na tomto virtuálním počítači dostupné.

Virtuální počítače mohou být také ovlivněny pravidelnými aktualizacemi iniciovanými samotnou platformou Azure. Mezi tyto události údržby patří aktualizace softwaru a upgrady hardwaru, které jsou potřebné ke zvýšení spolehlivosti a výkonu platformy. Tyto události obvykle nemají na virtuální počítače hosta žádný dopad, ale někdy se virtuální počítače restartují kvůli dokončení aktualizace nebo upgradu.

> [!NOTE]
> Microsoft operační systém ani software virtuálních počítačů automaticky neaktualizuje. Je to na vaší zodpovědnosti a máte nad tím úplnou kontrolu. U základního hostitele softwaru a hardwaru se ale pravidelně instalují opravy, aby byla trvale zajištěna spolehlivost a vysoký výkon.

Abyste zajistili poskytování služeb bez přerušení a vyhnuli se kritickému prvku způsobujícímu selhání, doporučuje se nasadit alespoň dvě instance každého virtuálního počítače. Tato funkce se označuje jako _skupina dostupnosti_.

### <a name="what-is-an-availability-set"></a>Co je skupina dostupnosti?

**Skupina dostupnosti** je logická funkce, která zajišťuje, že související virtuální počítače ve skupině jsou nasazené tak, aby jim všem najednou nehrozilo selhání a aby se neupgradovaly všechny najednou při upgradu hostitelského operačního systému v datovém centru. Virtuální počítače ve skupině dostupnosti by měly provádět stejnou sadu funkcí a mít nainstalovaný stejný software.

> [!TIP]
> Pro víceinstanční virtuální počítače nasazené ve skupině dostupnosti nabízí Microsoft smlouvu o úrovni služeb (SLA) s 99,95% externím připojením. To znamená, že aby tato smlouva SLA platila, musí existovat alespoň dvě instance virtuálního počítače nasazené ve skupině dostupnosti. 

Skupiny dostupnosti se vytvářejí na webu Azure Portal v oddíle pro zotavení po havárii. Můžete je vytvořit také pomocí šablon Resource Manageru, skriptování nebo nástrojů rozhraní API. Při umísťování virtuálních počítačů do skupiny dostupnosti zaručuje Azure jejich rozložení mezi **domény selhání** a **aktualizační domény**.

#### <a name="what-is-a-fault-domain"></a>Co je doména selhání?

Doména selhání je logická skupina hardwaru v Azure, která sdílí společné napájení a síťový přepínač. Můžete si ji představit jako stojan v místním datovém centru. První dva virtuální počítače ve skupině dostupnosti budou zřízeny ve dvou různých stojanech, takže pokud u stojanu selže napájení nebo síť, ovlivní to jen jeden virtuální počítač. Domény selhání jsou také definované pro spravované disky připojené k virtuálním počítačům.

![Ilustrace zobrazující dvě domény selhání se dvěma virtuálními počítači v každé z nich Dva horní virtuální počítače z každé domény selhání hostují Internetovou informační službu a jsou součástí společné skupiny dostupnosti. Další dva virtuální počítače v každé doméně hostují databázi SQL a jsou součástí další skupiny dostupnosti.](../media/5-fault-domains.png)

#### <a name="what-is-an-update-domain"></a>Co je aktualizační doména?

Aktualizační doména je logická skupina hardwaru, u které může současně probíhat údržba nebo restartování. Azure automaticky umísťuje skupiny dostupnosti do aktualizačních domén, aby se minimalizoval dopad, když platforma Azure zavádí změny hostitelského operačního systému. Azure pak zpracovává aktualizační domény postupně jednu po druhé.

Skupiny dostupnosti představují výkonnou funkci, která zajišťuje, aby služby běžící na virtuálních počítačích byly vždy dostupné zákazníkům. Nejsou ale odolné vůči lidskému selhání. Co když se stane něco s daty nebo softwarem běžícím na samotném virtuálním počítači? Kvůli tomu se budeme muset podívat na jiné možnosti zotavení po havárii a zálohování.

## <a name="failover-across-locations"></a>Převzetí služeb při selhání mezi lokalitami

Svou infrastrukturu můžete rovněž replikovat mezi lokality a ošetřit tak oblastní převzetí služeb při selhání. **Azure Site Recovery** replikuje úlohy z primární lokality do sekundární lokality. Pokud v primární lokalitě dojde k výpadku, může při selhání převzít služby sekundární lokalita. Toto převzetí služeb při selhání umožňuje uživatelům používat vaše aplikace bez přerušení. Jakmile je primární lokalita provozuschopná, můžete jí služby znovu předat. Principem Azure Site Recovery je replikace virtuálních nebo fyzických počítačů a zachování dostupnosti při výpadku.

I když má Site Recovery spoustu atraktivních technických funkcí, má nejméně dvě významné obchodní výhody:

1. Služba Site Recovery umožňuje použít Azure jako cíl pro zotavení, čímž eliminuje náklady a úsilí spojené s udržováním sekundárního fyzického datového centra.

2. Díky Site Recovery je neuvěřitelně snadné otestovat postupy zotavení po havárii bez dopadu na produkční prostředí. To usnadňuje testování plánovaných nebo neplánovaných převzetí služeb při selhání. Pokud jste převzetí služeb při selhání nikdy nevyzkoušeli, nemáte dobrý plán zotavení po havárii.

Plány zotavení, které pomocí Site Recovery vytvoříte, mohou být tak jednoduché nebo složité, jak vyžaduje vaše situace. Mohou zahrnovat vlastní powershellové skripty, runbooky Azure Automation nebo ruční zásah. Tyto plány zotavení můžete využít k replikaci úloh do Azure, snadnému umožnění nových příležitostí k migraci, dočasnému posílení v době špiček nebo k vývoji a testování nových aplikací.

Azure Site Recovery spolupracuje s prostředky Azure nebo s Hyper-V, VMware a fyzickými servery ve vaší místní infrastruktuře a může být klíčovou součástí organizace pro strategii provozní kontinuity a zotavení po havárii. Zajišťuje totiž replikaci, převzetí služeb při selhání a zotavení úloh a aplikací, když primární lokalita selže.
