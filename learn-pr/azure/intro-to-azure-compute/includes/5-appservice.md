:::row:::
  :::column:::
    ![Obrázek představující Azure App Service](../media/5-appservice.png)
  :::column-end:::
  :::column span="3":::
Azure App Service umožňuje vytvářet a hostovat webové aplikace, úlohy na pozadí, mobilní back-endy a rozhraní RESTful API v programovacím jazyce podle vašeho výběru, aniž by bylo potřeba zabývat se správou infrastruktury. Nabízí automatické škálování a vysokou dostupnost, podporuje systémy Windows a Linux a umožňuje automatizované nasazení z Githubu, Azure DevOps nebo libovolného úložiště Git pro podporu modelu průběžného nasazování.
  :::column-end:::
:::row-end:::

Toto řešení typu platforma jako služba (PaaS) umožňuje zaměřit se na logiku webů a rozhraní API, zatímco Azure zajistí infrastrukturu pro spouštění a škálování webových aplikací. 

## <a name="app-service-costs"></a>Náklady na službu App Service

Platíte za výpočetní prostředky Azure, které vaše aplikace využívá při zpracovávání žádostí, a to na základě vámi zvoleného plánu služby App Service. Plán služby App Service určuje, kolik hardwaru je určeno pro vašeho hostitele – například jestli jde o vyhrazený, nebo sdílený hardware a kolik paměti je pro něho rezervováno. K dispozici je i úroveň _Free_, pomocí které můžete hostovat malé weby se slabým provozem.

## <a name="types-of-web-apps"></a>Typy webových aplikací

Pomocí služby Azure App Service může hostovat webové aplikace obvyklých typů, mezi které patří:

- Web Apps
- API Apps
- WebJobs
- Mobile Apps

Azure App Service provádí většinu infrastrukturních rozhodnutí souvisejících s hostováním webových aplikací: nasazování a správa jsou integrované přímo do platformy, koncové body je možné zabezpečit, weby lze rychle škálovat pro zvládnutí vysokého přenosového zatížení a integrované vyrovnávání zatížení a služba Traffic Manager zajišťují vysokou dostupnost. Aplikace všech těchto typů jsou hostované ve stejné infrastruktuře a tyto výhody sdílejí. Díky tomu je App Service ideální volbou pro hostování aplikací orientovaných na web.

### <a name="web-apps"></a>Web Apps

App Service zahrnuje plnou podporu pro hostování webových aplikací na bázi jazyka ASP.NET, ASP.NET Core, Javy, Ruby, Node.js, PHP nebo Python. Jako hostitelský operační systém můžete zvolit Windows nebo Linux. 

### <a name="api-apps"></a>API Apps

Velmi podobně jako při hostování webu můžete sestavovat webová rozhraní API založená na protokolu REST s využitím vámi zvoleného jazyka a architektury. Získáte kompletní podporu pro Swagger a možnost rozhraní API zabalit a publikovat na webu Azure Marketplace. Vyprodukované aplikace je možné využívat z libovolného klienta na bázi HTTP(s).

### <a name="web-jobs"></a>Web Jobs

Funkce WebJobs umožňuje spustit program (.exe, Java, PHP, Python nebo Node.js) nebo skript (.cmd, .bat, PowerShell nebo Bash) ve stejném kontextu, v jakém je webová aplikace, aplikace API nebo mobilní aplikace. Je možné je naplánovat nebo spouštět na základě aktivační události. Často se používají ke spouštění úloh na pozadí v rámci logiky aplikace.

### <a name="mobile-apps"></a>Mobile Apps

Funkce Mobile Apps služby Azure App Service umožňuje rychle sestavit back-end aplikací pro iOS a Android. Pomocí několika kliknutí na webu Azure Portal můžete:

- Ukládat data mobilních aplikací do cloudové databáze SQL
- Ověřovat zákazníky prostřednictvím rozšířených sociálních sítí, jako jsou MSA, Google, Twitter a Facebook
- Odesílat nabízená oznámení
- Spouštět vlastní back-endovou logiku v C# nebo Node.js

Na straně mobilní aplikace je k dispozici podpora sady SDK pro nativní aplikace typu iOS, Android, Xamarin a React.