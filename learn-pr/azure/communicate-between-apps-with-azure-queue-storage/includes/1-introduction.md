Představte si, že pracujete pro velkou tiskovou agenturu, která upozorňuje na nejnovější zprávy. Naše společnost zaměstnává celosvětovou síť novinářů, kteří přes webový portál a mobilní aplikaci neustále posílají aktuální informace. Webová služba střední vrstvy pak tyto aktuální informace prostřednictvím několika kanálů publikuje online.

Zjistilo se však, že v případě celosvětově významných událostí v systému některá upozornění chybí. To je _obrovský_ problém, protože nás předbíhá konkurence! Pečlivě vás vybrali jako nejlepšího vývojáře ve společnosti, abyste problém identifikovali a opravili.

Střední vrstva nabízí dostatečnou kapacitu ke zpracování běžného zatížení. Při procházení protokolů na serveru jste ale zjistili, že došlo k přetížení systému, když se několik novinářů najednou pokusilo nahrát větší zprávy. Někteří autoři si stěžovali, že portál přestal reagovat, a jiní tvrdili, že své příběhy ztratili úplně. Všimli jste si přímé souvislosti mezi hlášenými problémy a nárůstem zatížení serverů střední vrstvy.

Je jasné, že potřebujete najít způsob, jak tyto nečekané špičky zvládat. Nechcete přidávat další instance webu a webové služby střední vrstvy, protože jsou nákladné a za normálních podmínek nadbytečné. Mohli bychom dynamicky aktivovat instance, ale to trvá dlouho a nastal by problém s čekáním na přechod nových serverů do režimu online.

K řešení tohoto problému můžete použít Azure Queue Storage. Fronta úložiště je vysoce výkonná vyrovnávací paměť zpráv, která může fungovat jako zprostředkovatel mezi front-endovými komponentami (producenti) a střední vrstvou (příjemci). 

Naše front-endové komponenty umístí zprávu o každém novém upozornění do fronty. Střední vrstva pak tyto zprávy z fronty načítá jednu po druhé a zpracovává je. Když je poptávka vysoká, může fronta narůstat, ale žádné příběhy se neztratí a aplikace nepřestane reagovat. Až poptávka klesne na normální úroveň, může webová služba dohánět resty a zpracovávat backlog fronty.

Pojďme zjistit, jak pomocí Azure Queue Storage zvládat vysokou poptávku a zvýšit odolnost v distribuovaných aplikacích.

## <a name="learning-objectives"></a>Cíle výuky

- Vytvořit účet Azure Storage, který podporuje fronty.
- Vytvořit frontu pomocí jazyka C# a klientské knihovny Azure Storage pro .NET.
- Přidávat, načítat a odebírat zprávy z fronty pomocí jazyka C# a klientské knihovny Azure Storage pro .NET.