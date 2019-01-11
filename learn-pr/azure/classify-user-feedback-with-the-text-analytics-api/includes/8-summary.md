Azure Cognitive Services je bohatá sada inteligentních služeb, pomocí kterých můžete obohatit svoje aplikace. Rozhraní API pro analýzu textu má několik operací, které převedou text na smysluplné informace. Tuto službu jsme použili k analýze mínění v textové zpětné vazbě od zákazníků. Vytvořili jsme řešení hostované ve službě Azure Functions, které tyto textové zprávy rozřazuje do různých kbelíků neboli front pro další zpracování.

Jakmile zjistíte, jak volat rozhraní REST API, můžete tyto inteligentní služby snadno integrovat do svých řešení. Všechny fungují na podobném principu:

- Registrujte se a získejte přístupový klíč.
- Prozkoumejte možnosti konzoly pro testování rozhraní API.
- Formulujte žádosti s použitím přístupového klíče a správné oblasti.
- Odesílejte ze svého řešení požadavky POST a parsováním odpovědí získejte potřebné informace.

Tyto inteligentní funkce jsme přidali do logiky bez serveru vytvořené ve službě Azure Functions. Tyto služby můžete jednoduše volat i z jiných typů aplikací. Existuje řada klientských knihoven, kurzů a rychlých startů, které vám pomůžou začít.

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>Návrhy na další vylepšení našeho řešení

Podívejte se na několik nápadů, které je vhodné zvážit, pokud se rozhodnete rozšířit naše řešení.

- Otestujte řešení s použitím dalších příkladů textu. Rozhodněte, jestli jsme vhodně nastavili prahové hodnoty pro kategorizaci skóre mínění na pozitivní, negativní a neutrální.
- Zvažte možnost přidání další funkce do vaší aplikace funkcí, která bude číst zprávy z fronty [!INCLUDE [negative-q](./q-name-negative.md)] a volat rozhraní API pro analýzu textu za účelem vyhledání klíčových frází v textu.
- Naše vstupní fronta obsahuje nezpracovanou textovou zpětnou vazbu. V reálném světě bychom ke zpětné vazbě přidružili nějakou formu ID uživatele, například e-mailovou adresu, číslo účtu atd. Vylepšete položky vstupní fronty tak, aby to byly dokumenty JSON obsahující pole s ID a text. Následně při práci s textovou zprávou používejte toto ID.
- V současné době je v našem řešení pevně zakódovaná podpora angličtiny. Představte si, jaké změny byste udělali, abyste umožnili podporu zpracování textu ve všech jazycích, které podporuje rozhraní API pro analýzu textu.
- Pokud znáte Logic Apps, použijte odkaz na integrovaný konektor pro analýzu textu v části Další materiály.

Když teď víte, jak volat jedno z těchto rozhraní API služeb Cognitive Services, podívejte se na některé další služby a rozmyslete si, jak byste je mohli využít ve svých řešeních.

## <a name="further-reading"></a>Další materiály

- [Přehled analýzy textu](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Ukázka analýzy textu](https://azure.microsoft.com/services/cognitive-services/text-analytics/)
- [Rozpoznávání mínění v rozhraní API pro analýzu textu](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Dokumentace ke službám Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)

- [Logic Apps – konektor pro analýzu textu](https://docs.microsoft.com/connectors/cognitiveservicestextanalytics/)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
