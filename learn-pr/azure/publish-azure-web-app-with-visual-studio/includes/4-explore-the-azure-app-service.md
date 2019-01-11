Vytvořili jste web a chcete ho nasadit do Azure. Teď musíme zvážit, které služby Azure využijeme, aby co nejlépe odpovídaly našim potřebám. App Service je vysoce škálovatelná služba pro hostování webů s automatickými opravami pro vaše aplikace.

V tomto článku se podíváme na to, jak pomocí sady Visual Studio publikovat webovou aplikaci ASP.NET Core do plánu služby Azure App Service.

## <a name="what-is-the-azure-app-service"></a>Co je Azure App Service?

Azure App Service je služba pro hostování webových aplikací, rozhraní REST API a mobilních back-endů. App Service podporuje kód napsaný v jazycích .NET, .NET Core, Java, Ruby, Node.js, PHP a Python. App Service je ideální pro většinu webů, zejména pokud nepotřebujete mít nad infrastrukturou hostování úplnou kontrolu.

## <a name="what-is-the-app-service-plan"></a>Co je plán služby App Service?

Plán služby App Service určuje výpočetní prostředky, které bude vaše aplikace používat, jejich umístění, kolik dalších prostředků může plán využívat a cenovou úroveň. Tyto výpočetní prostředky jsou obdobou serverové farmy běžného webhostingu. Ve stejném plánu služby App Service může běžet jedna nebo více aplikací.

Při nasazování aplikací můžete vytvořit plán služby App Service nebo můžete aplikace dál přidávat do již vytvořeného plánu.  Stejné výpočetní prostředky však aplikace sdílí v rámci stejného plánu služby App Service. Pokud chcete zjistit, jestli bude mít nová aplikace potřebné prostředky, musíte znát kapacitu existujícího plánu služby App Service a očekávanou zátěž nové aplikace. Přetížení plánu služby App Service může způsobit snížení výkonu nebo výpadek nejen nové aplikace, ale i stávajících aplikací.

Plán služby App Service můžete na webu Azure Portal předem definovat pomocí PowerShellu nebo Azure CLI nebo ho můžete nastavit při publikování aplikace v sadě Visual Studio.

Každý plán služby App Service definuje:

- Oblast (Západní USA, Východ USA atd.)
- Počet instancí virtuálních počítačů
- Velikost instancí virtuálních počítačů (malá, střední, velká)
- Cenovou úroveň (Free, Shared, Basic, Standard, Premium, Premium V2, V izolovaném prostředí)

## <a name="specify-the-region"></a>Určení oblasti

Při vytváření plánu služby App Service budete muset definovat oblast nebo umístění, kde bude tento plán hostovaný. Obvykle se volí oblast, která je geograficky nejblíž očekávaným zákazníkům.

## <a name="what-are-the-pricing-and-reliability-levels"></a>Jaké existují cenové úrovně a úrovně spolehlivosti?

**Sdílené výpočetní prostředky:** **Free** a **Shared** jsou dvě základní úrovně, ve kterých se aplikace spouštějí na stejném virtuálním počítači Azure, na kterém se spouští nejen vaše ostatní aplikace služby App Service, ale i aplikace ostatních zákazníků. Tyto úrovně přidělují kvóty procesoru každé aplikaci, která na sdílených prostředcích běží. Dále platí, že prostředky nejdou škálovat na více instancí.

Plány Free a Shared jsou nejvhodnější pro drobné osobní projekty s velmi omezenými požadavky na provoz, protože mají nastavený limit 165 MB odchozích dat na 24 hodin.

**Vyhrazené výpočetní prostředky:** V úrovních **Basic, Standard, Premium a Premium V2** se aplikace spouštějí na vyhrazených virtuálních počítačích Azure. Stejné výpočetní prostředky sdílejí jen aplikace ve stejném plánu služby App Service. Čím vyšší cenová úroveň, tím více instancí virtuálních počítačů je možné použít pro horizontální navýšení kapacity.

Plán Standard je nejvhodnější pro živé provozní úlohy, když zákazníkům publikujete obchodní aplikace.

Plány služeb Premium podporují vysokokapacitní webové aplikace, u kterých nechcete mít s vyhrazeným plánem (v izolovaném prostředí) spojené další náklady.

**V izolovaném prostředí:** V této úrovni se vyhrazené virtuální počítače Azure spouští ve vyhrazených virtuálních sítích Azure, kde kromě izolovaných výpočetních prostředků mají vaše aplikace také izolovanou síť. Tato úroveň nabízí maximální škálování na více instancí. Plán služby V izolovaném prostředí byste si vybrali pouze v případě, že máte specifické požadavky na nejvyšší úroveň zabezpečení a výkonu.

Aplikaci je vhodné do nového plánu služby App Service izolovat v těchto případech:

- Aplikace je náročná prostředky.
- Aplikaci chcete škálovat nezávisle na ostatních aplikacích v existujícím plánu.
- Aplikace potřebuje prostředky v jiné geografické oblasti.

Kapacitu plánu služby App Service je možné kdykoli vertikálně navýšit nebo snížit. Nejprve si můžete zvolit nižší cenovou úroveň a později kapacitu vertikálně navýšit, když budete potřebovat další funkce služby App Service.

## <a name="specify-the-resource-group"></a>Určení skupiny prostředků

Skupina prostředků je logický kontejner, do kterého se nasazují a ve kterém se spravují prostředky Azure, jako jsou například webové aplikace, databáze a účty úložiště. Jedná se o mechanismus uspořádání prostředků pro účely správy, monitorování a fakturace. Můžete použít existující skupinu prostředků nebo přímo v sadě Visual Studio vytvořit novou.  
