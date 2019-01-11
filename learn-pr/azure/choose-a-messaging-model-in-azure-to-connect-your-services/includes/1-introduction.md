Mnoho aplikací se skládá z programů, které běží na několika různých počítačích nebo zařízeních. V těchto distribuovaných aplikacích je nutné posílat zprávy mezi komponentami v různých sítích a na velké vzdálenosti. Dokonce i volně propojené architektury na stejném serveru nebo ve stejném datacentru vyžadují mechanismy pro komunikaci komponent. Spolehlivé zasílání zpráv je často kritický problém.

Představte si, že pracujete v softwarové společnosti, která vyvíjí aplikaci pro sdílení hudby. Hudebníci mohou nahrávat vytvořenou hudbu do vaší platformy pomocí webového front-endu nebo mobilní aplikace. Můžou také poslouchat práci ostatních členů a komentovat ji. Aplikace se skládá z webu, který běží u vašeho poskytovatele internetových služeb, mobilní aplikace, která běží na mobilních zařízeních uživatelů, webového rozhraní API, které běží v Azure, a služby Azure SQL Database, kde jsou uložená data.

Zjistili jste, že v době, kdy přichází nejvíce požadavků, se některé hudební soubory úspěšně nenahrají a některé komentáře se nezveřejní. Z testování vyplývá, že k těmto problémům dochází kvůli ztrátě zpráv mezi front-endovými komponentami a webovým rozhraním API. K řešení těchto problémů chcete použít jednu nebo více následujících technologií: Fronty služby Azure Storage, Azure Event Hubs, Azure Event Grid a Azure Service Bus.

Tady se dozvíte, jak pro každou komunikační úlohu v distribuované aplikaci vybrat správnou technologii zasílání zpráv v Azure.

## <a name="learning-objectives"></a>Cíle výuky
V tomto modulu se naučíte:

- Popsat události a zprávy, které použijete k řešení problémů v distribuované aplikaci.
- Určit scénáře, kdy je nejvhodnější technologií zasílání zpráv v aplikaci fronta služby Storage.
- Určit scénáře, kdy je nejvhodnější technologií zasílání zpráv v aplikaci služba Event Grid.
- Určit scénáře, kdy je nejvhodnější technologií zasílání zpráv v aplikaci služba Event Hubs.
- Určit scénáře, kdy je nejvhodnější technologií zasílání zpráv v aplikaci služba Service Bus.