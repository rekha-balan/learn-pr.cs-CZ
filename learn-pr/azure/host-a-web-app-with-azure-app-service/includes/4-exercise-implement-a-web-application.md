V této lekci použijete nástroje pro vývojáře k vytvoření kódu počáteční webové aplikace.

## <a name="create-a-new-web-project"></a>Vytvoření projektu nového webu

::: zone pivot="csharp"

Jedním z nejdůležitějších nástrojů rozhraní příkazového řádku .NET CLI je nástroj příkazového řádku `dotnet`. Pomocí tohoto příkazu vytvoříte nový projekt webové aplikace využívající ASP.NET Core.

Ve službě Cloud Shell na pravé straně vytvořte novou aplikaci ASP.NET Core MVC. Pojmenujte ji BestBikeApp.

```bash
dotnet new mvc --name BestBikeApp
```

Tento příkaz vytvoří novou složku s názvem BestBikeApp, do které se bude ukládat tento projekt. Přejděte do ní příkazem `cd`, a potom sestavte a spusťte aplikaci, abyste ověřili, že je kompletní.

```bash
cd BestBikeApp
dotnet run
```

Mělo by se vám zobrazit přibližně toto:

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

Výstup popisuje situaci po spuštění aplikace: aplikace je spuštěna a naslouchá na portu 5000 (přes HTTPS).

Pokud bychom aplikaci spouštěli na vašem vlastním počítači, mohli bychom v prohlížeči otevřít http://localhost:5000. Abychom aplikaci zpřístupnili i z umístění mimo vlastní počítač, budeme potřebovat nasadit aplikaci někam s veřejným koncovým bodem. Ideální je k tomu dříve vytvořená instance App Service.

::: zone-end

::: zone pivot="node"

K vytvoření úvodní webové aplikace Node.js použijeme správce balíčků Node Package Manager (NPM) a základní javascriptový kód, který spustí vlastní zpracování webové stránky.

Když tyto příkazy spustíte v Cloud Shellu, vytvoříte nový soubor `package.json`, který popíše naši aplikaci Node.js.

```console
cd ~
mkdir helloworld
cd helloworld
npm init
```

Správce balíčků NPM vás vyzve k řadě odpovědí. Použijte následující odpovědi. Pokud chcete potvrdit výchozí odpověď, stiskněte Enter.

| Otázka | Odpověď |
|----------|--------|
| package name | helloworld |
| version | _default_ |
| description | Jednoduchá aplikace Hello World |
| entry point | _default_ |
| test command | _default_ |
| git repository | _default_ |
| keywords | _default_ |
| author | vaše jméno |
| license | cokoli, co použijete, třeba MIT, ISC apod. |

Tím v aktuální složce vytvoříte nový soubor `package.json`. Pokud v okně terminálu zadáte `ls`, měl by se zobrazit v aktuální složce. Ke spuštění logiky webu potřebujeme javascriptový soubor. Vzhledem k tomu, že jde o základní příklad, budete potřebovat jenom jeden soubor `index.js`. K vytvoření souboru použijte v terminálu následující příklad:

```bash
touch index.js
``` 

Teď je potřeba v obou souborech provést několik úprav. V terminálu zadejte následující příkaz, který otevře interaktivní editor.

```console
code .
```

Vyberte soubor `package.json` a v oddílu `scripts` proveďte následující úpravy, aby se ke spuštění webové aplikace použil Node.js. Můžete také odebrat záznam `main`.

```json
{
  "name": "helloworld",
  ...
  "scripts": {
    "start": "node index.js"
  },
  ...
}
```

Soubor uložte.

> [!IMPORTANT]
> Pokaždé, když v editoru vložíte do souboru kód nebo ho změníte, nezapomeňte ho uložit. Použijte nabídku „...“ nebo klávesovou zkratku (ve Windows a Linuxu <kbd>Ctrl+S</kbd>, v macOS <kbd>Command+S</kbd>).

Přepněte na soubor `index.js` a přidejte do něj následující obsah. Jde o jednoduchý program node, který pokaždé odpoví „Hello World!“ – bez ohledu na požadavek GET adresovaný serveru.

```javascript
var http = require('http');

var server = http.createServer(function(request, response) {

    response.writeHead(200, { "Content-Type": "text/html" });
    response.end("<html><h1>Hello World!</h1></html>");

});

var port = process.env.PORT || 1337;
server.listen(port);

console.log("Server running at http://localhost:%d", port);
```

Uložte soubor a ukončete editor. K ukončení editoru použijte nabídku „...“ v pravém horním rohu nebo klávesovou zkratku <kbd>Ctrl+Q</kbd>.

Teď můžeme zabalit soubory, které chceme nasadit do Azure. Potřebujeme vytvořit archiv ZIP celého projektu. V okně terminálu zadejte následující příkazy, kterými vytvoříte soubor ZIP:

```bash
zip -r helloworld.zip .
```

Až příkaz doběhne, zadejte `ls`. Zobrazí se soubor s názvem `helloworld.zip`. Tento balíček webové aplikace nasadíme ve službě App Service.

::: zone-end

::: zone pivot="java"

K vytvoření počáteční webové aplikace použijeme Maven. Jde o běžně používaný nástroj na správu a vytváření projektů pro aplikace Java. Součástí nástroje Maven je funkce *archetypes*, která rychle vytvoří počáteční kód pro různé druhy aplikací. K vygenerování kódu jednoduché webové aplikace, která na domovské stránce zobrazí nápis „Hello World!“, použijeme šablonu `maven-archetype-webapp` .

Teď spusťte v Cloud Shellu tyto příkazy, které vytvoří novou webovou aplikaci:

```console
cd ~
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DinteractiveMode=false -DarchetypeArtifactId=maven-archetype-webapp
```

A teď spusťte tyto příkazy, kterými přejdete do nového adresáře aplikace „helloworld“ a zabalíte aplikaci pro nasazení:

```console
cd helloworld
mvn package
```

Až příkaz doběhne, pokud jste přešli do adresáře `target` a spustili jste příkaz `ls`, zobrazí se soubor s názvem `helloworld.war`. Tento balíček webové aplikace nasadíme do App Service.

::: zone-end