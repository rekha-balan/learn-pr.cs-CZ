Mojifier je robot ze sady Slack, který rozpoznává tváře na obrázcích a pak je nahrazuje obrázky emoji, které odpovídají jejich emocím. Příklad:  

![Ukázka tváře převedené na emoji Na levé straně je obrázek mužské tváře se šipkou, která ukazuje na tuto tvář s překryvným emoji na pravé straně.](../media/example-mojify-image.png)

Tato funkce je navržená tak, aby fungovala ze Slacku jako vlastní příkaz. Příkaz můžete nazvat jakkoli, avšak tento modul na něj bude odkazovat pomocí názvu `mojify`. Příkaz můžete spustit zadáním `/mojify <image to mojify>`.

Mojifier pak provede následující:

  1. Vypočítá emoce všech osob na obrázku.
  2. Přiřadí emoce k obrázkům emoji.
  3. Nahradit tváře na obrázku obrázky emoji
  4. Odešle aktualizovaný obrázek do Slacku jako odpověď.

Výsledkem by měl být nový obrázek převedený na emoji, který se zobrazí ve Slacku, například takto: 

![Vyvolání aplikace Slack Mojifier za účelem přidání obrázku emoji do obrázku na adrese URL. Mojifier odeslal na adresu URL obrázek.](../media/8.slack-type-mojify.png)

Mojifier je napsaný pomocí jazyka TypeScript, [Azure Functions](https://azure.microsoft.com/services/functions?azure-portal=true) a [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services?azure-portal=true). Tyto prostředky můžete použít k vytvoření své vlastní verze aplikace Mojifier.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Klasifikovat emoce všech osob na obrázku
- Přiřadit emoce k emoji
- Nahradit tváře na obrázku obrázky emoji
- Odeslat aktualizovaný obrázek do Slacku jako odpověď.

## <a name="tools-youll-use"></a>Nástroje, které budete používat

### <a name="azure-cognitive-services"></a>Azure Cognitive Services

Azure Cognitive Services je sada rozhraní API vyšší úrovně, pomocí které můžete rychle přidávat pokročilé funkce umělé inteligence (AI) do aplikace. Pokud víte, jak vytvořit požadavek HTTP, pak byste měli být schopní Cognitive Services používat.

### <a name="azure-functions"></a>Azure Functions

Řešení Azure Functions umožňuje hostovat fragmenty kódu, které můžou odpovídat na události nebo na požadavky HTTP. Azure automaticky řeší problémy se škálováním a vy platíte jenom za prostředky, které využíváte. Podobně jako v kurzech Microsoft Learn uhradíme veškeré náklady ve výukovém prostředí za vás.

> [!NOTE]
> Celý kód Mojifieru je k dispozici v [GitHubu](https://github.com/MicrosoftDocs/mslearn-the-mojifier?azure-portal=true).
