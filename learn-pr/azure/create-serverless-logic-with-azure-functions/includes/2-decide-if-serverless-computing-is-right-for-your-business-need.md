Abyste se mohli lépe rozhodnout, jestli je bezserverová architektura pro vás tím pravým, podíváme se, co to vlastně je.

## <a name="what-is-serverless-compute"></a>Co je výpočetní prostředí bez serveru?

Výpočetní prostředí bez serveru se dá označit pojmem Funkce jako služba (FaaS), nebo se dá považovat za mikroslužbu hostovanou na cloudové platformě. Vaše obchodní logika se spouští jako funkce a vy nemusíte ručně zřizovat nebo škálovat infrastrukturu. Infrastrukturu spravuje poskytovatel cloudu. Aplikace automaticky horizontálně navyšuje nebo vertikálně snižuje kapacitu podle zatížení. Azure nabízí několik způsobů, jak tento druh architektury vytvořit. Dvěma nejběžnějšími přístupy jsou Azure Logic Apps a Azure Functions, na které se v tomto modulu zaměříme.

## <a name="what-is-azure-functions"></a>Co je Azure Functions?

Azure Functions je platforma pro aplikace bez serveru. Umožňuje vývojářům hostovat obchodní logiku, kterou je možné spouštět bez zřizování infrastruktury. Služba Azure Functions poskytuje vnitřní škálovatelnost a účtuje pouze používané prostředky. Kód funkce můžete psát v jazyce, který si sami zvolíte, včetně C#, F# a JavaScriptu. Součástí je také podpora pro NuGet a NPM, takže v obchodní logice můžete používat oblíbené knihovny.

## <a name="benefits-of-a-serverless-compute-solution"></a>Výhody řešení bezserverové architektury

Výpočetní prostředí bez serveru je skvělou volbou pro hostování kódu obchodní logiky v cloudu. Díky nabídkám bezserverové architektury, jako je třeba služba Azure Functions, můžete psát vlastní obchodní logiku ve vámi vybraném jazyce. Získáte automatické škálování, nemusíte spravovat žádné servery a platíte za to, co jste použili, nikoli za rezervovaný čas. Zde jsou některé další charakteristiky bezserverového řešení, které byste měli zvážit.

### <a name="avoids-over-allocation-of-infrastructure"></a>Vyhnete se předimenzování infrastruktury

Předpokládejme, že jste zřizovali servery na virtuálních počítačích a konfigurovali je tak, aby měly dostatek prostředků pro zpracování zatížení ve špičce. Pokud je zatížení malé, pravděpodobně platíte za infrastrukturu, kterou nevyužíváte. Výpočetní prostředí bez serveru pomáhá řešit problém předimenzování automatickým vertikálním škálováním nahoru i dolů a platit budete jen v případě, že vaše funkce provádí výpočetní úlohy.

### <a name="stateless-logic"></a>Bezstavová logika

Bezstavové funkce jsou skvělými kandidáty pro výpočetní prostředí bez serveru; instance funkcí jsou vytvářeny a rušeny podle potřeby. Pokud je vyžadováno uchování stavu, může být uložen v přidružené službě úložiště.

### <a name="event-driven"></a>Řízení událostmi

Funkce jsou _řízené událostmi_. To znamená, že se funkce spouštějí jenom v reakci na určitou událost (označovanou jako aktivační událost) – třeba přijetí požadavku HTTP nebo přidání zprávy do fronty. Aktivační událost můžete nakonfigurovat jako součást definice funkce. Tento přístup váš kód zjednodušuje, protože umožňuje deklarovat, odkud data pocházejí (vazba triggeru/vstupu) a kam putují (výstupní vazba). Není nutné psát kód pro zpracování front, objektů blob, center atd. Můžete se soustředit čistě na obchodní logiku.

### <a name="functions-can-be-used-in-traditional-compute-environments"></a>Funkce můžete používat v tradičních výpočetních prostředích

Funkce jsou klíčovou komponentou bezserverové architektury. Jsou ale také obecnou výpočetní platformou pro spouštění libovolného typu kódu. Pokud by se potřeby aplikace změnily, můžete svůj projekt použít a nasadit ho v jiném než bezserverovém prostředí, které umožňuje flexibilně spravovat škálování, spouštění ve virtuálních sítích a dokonce úplnou izolaci vašich funkcí.

## <a name="drawbacks-of-a-serverless-compute-solution"></a>Nevýhody řešení bezserverové architektury

Výpočetní prostředí bez serveru není vždy tím nejvhodnějším řešením pro hostování vaší obchodní logiky. Tady je několik charakteristik funkcí, které mohou ovlivnit vaše rozhodnutí, zda vaše služby hostovat ve výpočetním prostředí bez serveru.

### <a name="execution-time"></a>Doba spouštění

Funkce mají ve výchozím nastavení časový limit 5 minut. Tento časový limit je možné prodloužit na maximálních 10 minut. Pokud vaše funkce vyžaduje k provedení potřebné akce více než 10 minut, je třeba ji hostovat na virtuálním počítači. Pokud se navíc vaše služba aktivuje v reakci na požadavek HTTP a vrací hodnotu v odpovědi HTTP, je časový limit omezen na 2,5 minuty. Navíc je tu také možnost nazvaná **Durable Functions**, která umožňuje orchestraci spuštění více funkcí bez jakéhokoli časového limitu.

### <a name="execution-frequency"></a>Frekvence spouštění

Druhým charakteristickým znakem je frekvence spouštění. Pokud očekáváte, že funkce bude současně používána více klienty, doporučuje se odhadnout využití a propočítat náklady na používání funkcí. Může se stát, že hostování služby na virtuálním počítači bude levnější.

Při škálování je možné vytvořit jen jednu instanci aplikace funkcí každých 10 sekund, až do celkového počtu instancí 200. Mějte na paměti, že každá instance může obsloužit více souběžných spuštění, takže provoz zpracovatelný jednou instancí není nijak principiálně omezen. Různé druhy aktivačních událostí mají různé požadavky na škálování, proto prozkoumejte vaše možnosti a seznamte se s jejich omezeními.
