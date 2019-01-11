Všechny nás zajímá, co si zákazníci myslí o naší značce, našem produktu a naší zprávě. Jak se jejich názory mění v průběhu času? Určité vodítko nám může poskytnout, když rozpoznáme mínění v tom, co píší. Analýza mínění pomáhá zodpovědět tuto otázku: *Co naši zákazníci skutečně chtějí?* Používá se k analýze tweetů a dalšího obsahu sociálních médií, hodnocení zákazníků a e-mailů.

![Mínění se extrahuje z textu a zobrazuje na měřidle od negativního po pozitivní.](../media/sentiment-analysis.png)

 Oblíbeným přístupem k analýze mínění je natrénovat modely strojového učení, které rozpoznávají mínění. Tento proces je však složitý. Zahrnuje nutnost mít kvalitní a označená trénovací data, vytváření funkcí na základě těchto dat, trénování klasifikátoru a následné použití klasifikátoru k odhadu mínění v nových textech. Ne každá společnost má dostatek financí a odborných znalostí, aby mohla investovat do vytváření řešení AI od nuly. Microsoft a další společnosti naštěstí můžou investovat do nejmodernějšího výzkumu v těchto oblastech, a také to dělají. Jako vývojáři získáváme výhody vyplývající z jejich poznatků prostřednictvím rozhraní API, sad SDK a platforem, které poskytují. Příkladem takové nabídky jsou služby Microsoft Cognitive Services.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

Služby Microsoft Cognitive Services se skládají ze sady rozhraní API, sad SDK a služeb. Cílem je pomoct vývojářům vytvářet inteligentnější, poutavější a zjistitelnější aplikace.

Služby Azure Cognitive Services nabízejí inteligentní algoritmy zpracování obrazu, řeči, jazyka, vědomostí a vyhledávání. Pokud se chcete podívat, co nabízíme, projděte si [Adresář služeb Cognitive Services](https://azure.microsoft.com/services/cognitive-services/directory/). Každou službu si můžete vyzkoušet zdarma. Když se rozhodnete integrovat jednu nebo několik z těchto služeb do svých aplikací, zaregistrujete si placené předplatné. Služba, kterou používáme v celém tomto modulu, je Analýza textu, takže si o ní něco povíme.

## <a name="text-analytics-api"></a>Analýza textu

Rozhraní API pro analýzu textu je služba Cognitive Services určená k tomu, aby vám umožnila extrahovat informace z textu. Pomocí této služby můžete identifikovat jazyk, rozpoznat mínění, extrahovat klíčové fráze a detekovat v textu dobře známé entity. 

V této lekci se seznámíme s částí tohoto rozhraní API určenou k analýze mínění. Na pozadí služba využívá klasifikační algoritmus strojového učení, který generuje skóre mínění v rozsahu 0 až 1. Skóre blížící se 1 značí pozitivní mínění, zatímco skóre blížící se 0 značí negativní mínění. Skóre blížící se 0,5 značí žádné nebo neutrální mínění. Nemusíte se starat o podrobnosti implementace tohoto algoritmu. Můžete se zaměřit na využívání služby jejím voláním z vaší aplikace. Jak si za chvíli ukážeme, sestavíte požadavek **POST**, odešlete ho do koncového bodu `/sentiment` a obdržíte odpověď JSON obsahující *skóre mínění*.

Zpočátku budeme s rozhraním API pro analýzu textu experimentovat s použitím online konzoly pro testování rozhraní API. Jakmile se s rozhraním API seznámíme, použijeme ho ve scénáři k rozpoznávání mínění ve zprávách, abychom je mohli roztřídit pro další zpracování.