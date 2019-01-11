Naučili jste se lépe uspořádat prostředky do skupin a přidat k nim značky využitelné v sestavách fakturace a v monitorovacím řešení. Pomocí skupin a opatření prostředků značkami jste do stávajících firemních prostředků vnesli řád, jak ale zajistit, aby se i nové prostředky vytvářely podle pravidel? Pojďme se podívat, jak vám při vynucování standardů v prostředí Azure můžou pomoct zásady.

## <a name="what-is-azure-policy"></a>Co je Azure Policy?

Azure Policy je služba, která slouží k vytváření, přiřazování a správě zásad. Tyto zásady uplatňují a vynucují pravidla, která vaše prostředky mají dodržovat. Zásady můžou vynucovat pravidla u nově vytvářených prostředků a dají se vyhodnocovat u stávajících prostředků, takže získáte přehled o dodržování předpisů.

Pomocí zásady můžete vynucovat různé věci, třeba povolit vytváření pouze určitých typů prostředků nebo povolit jen prostředky v konkrétních oblastech Azure. V celém prostředí Azure můžete vynucovat zásady pro vytváření názvů. Můžete také vynutit, aby se k prostředkům přidávaly konkrétní značky. Pojďme se podívat, jak zásady fungují.

## <a name="create-a-policy"></a>Vytvoření zásady

Chtěli bychom zajistit, že všechny prostředky budou opatřené značkou **Oddělení**, a pokud tato značka neexistuje, zablokovat jejich vytvoření. K tomu budeme muset vytvořit novou definici zásady a pak ji přiřadit oboru. V tomto případě bude oborem naše skupina prostředků **msftvyuka-zakladni-infrastruktura-sp**. Zásady se dají vytvářet a přiřazovat na webu Azure Portal, v prostředí Azure PowerShell nebo v rozhraní příkazového řádku Azure. Projděme si postup vytvoření zásady na portálu.

### <a name="create-the-policy-definition"></a>Vytvoření definice zásady

1. Pokud ještě nemáte v prohlížeči otevřený web [Azure Portal](https://portal.azure.com/?azure-portal=true), otevřete ho. Do vyhledávacího pole v horním navigačním panelu zadejte **Policy** a vyberte službu **Policy**.

1. V části **Vytváření ** v nabídce vlevo vyberte **Definice**.

1. Zobrazí se seznam integrovaných zásad, které můžete použít. V tomto případě si vytvoříme vlastní zásadu. Klikněte na tlačítko **+ Definice zásady** v horní nabídce.

1. Zobrazí se dialogové okno **Nová definice zásady**. **Umístění definice** nastavte kliknutím na modrou ikonu **…** (tři tečky). Vyberte předplatné, ve kterém se zásada má uložit. Mělo by jít o předplatné, kde už je umístěna skupina prostředků. Klikněte na **Vybrat**.

1. V dialogovém okně **Nová definice zásady** v poli **Název** zásadu pojmenujte **Vynucení značky u prostředku**.

1. Do pole **Popis** zadejte **Tato zásada vynucuje existenci značky u prostředku.**

1. V seznamu **Kategorie** vyberte **Použít existující** a pak vyberte kategorii **Obecné**.

1. Z pole **Pravidlo zásad** odstraňte veškerý text a vložte do něj následující kód JSON.

    ```json
    {
      "mode": "indexed",
      "policyRule": {
        "if": {
          "field": "[concat('tags[', parameters('tagName'), ']')]",
          "exists": "false"
        },
        "then": {
          "effect": "deny"
        }
      },
      "parameters": {
        "tagName": {
          "type": "String",
          "metadata": {
            "displayName": "Tag Name",
            "description": "Name of the tag, such as 'environment'"
          }
        }
      }
    }
    ```

    Definice zásady by měla vypadat následovně. Kliknutím na **Uložit** ji uložte.

    ![Obrázek portálu znázorňující dialogové okno Nová definice zásady](../media/4-policy-definition.PNG)

### <a name="create-a-policy-assignment"></a>Vytvoření přiřazení zásady

Zásadu jsme vytvořili, ale zatím jsme ji nezačali uplatňovat. Abychom ji aktivovali, potřebujeme vytvořit přiřazení. V našem případě zásadu přiřadíme oboru naší skupiny prostředků **msftvyuka-zakladni-infrastruktura-sp**, takže bude platit pro všechno, co se ve skupině nachází.

1. V podokně zásady v části **Vytváření** na levé straně vyberte **Přiřazení**.

1. Nahoře vyberte **Přiřadit zásadu**.

1. V podokně **Přiřazení zásad** přiřadíme naši zásadu skupině prostředků. **Obor** vyberte kliknutím na modrou ikonu **…** (tři tečky). Vyberte své předplatné a skupinu prostředků **msftvyuka-zakladni-infrastruktura-sp** a klikněte na tlačítko **Vybrat**.

1. Pro zadání **Definice zásady** klikněte na modrou ikonu **…** (tři tečky). V rozevíracím seznamu **Typ** vyberte **Vlastní**, vyberte zásadu **Vynucení značky u prostředku**, kterou jste vytvořili, a klikněte na **Vybrat**.

1. V části **Parametry** jako **Název značky** zadejte **Oddělení**. Kliknutím na **Přiřadit** zásadu přiřaďte.

### <a name="test-out-the-policy"></a>Otestování zásady

Teď, když jsme naši zásadu přiřadili skupině prostředků, by se neměl zdařit žádný pokus vytvořit prostředek bez značky **Oddělení**. Pojďme to vyzkoušet.

1. V levém horním rohu portálu vyberte **+Vytvořit prostředek**.

1. Vyhledejte **Účet úložiště** a z výsledků vyberte **Účet úložiště – objekt blob, soubor, tabulka, fronta**. Klikněte na **Vytvořit**.

1. Vyberte své předplatné a skupinu prostředků **msftvyuka-zakladni-infrastruktura-sp**.

1. Do pole **Název účtu úložiště** zadejte libovolný název, pamatujte ale na to, že musí být globálně jedinečný.

1. U ostatních možností ponechejte výchozí nastavení a vyberte **Zkontrolovat a vytvořit**.

    Vytvoření prostředku se nepodaří ověřit, protože jsme mu nepřiřadili značku **Oddělení**.

    ![Obrázek portálu zobrazující porušení zásady](../media/4-policy-violation.PNG)

    Opravme porušení zásady, abychom mohli účet úložiště úspěšně nasadit.

1. V horní části podokna **Vytvoření účtu úložiště** vyberte **Značky**.

1. Přidejte do seznamu značku **Oddělení: Finance**.

    ![Obrázek portálu znázorňující novou značku Oddělení](../media/4-add-department-tag.PNG)

1. Znovu klikněte na **Zkontrolovat a vytvořit**. Ověření by teď mělo proběhnout úspěšné, a když kliknete na **Vytvořit**, účet úložiště se vytvoří.

## <a name="use-policies-to-enforce-standards"></a>Použití zásad k vynucování standardů

Viděli jsme, jak se dá pomocí zásad zajistit, aby prostředky vždy měly značky, které je pomáhají uspořádat. Zásady ale můžou být výhodné i v dalších směrech.

Dovolují třeba omezit, do kterých oblastí Azure se můžou nasazovat prostředky. V organizacích, které podléhají přísným regulacím nebo právním předpisům omezujícím umístění dat, pomáhají zásady zajistit, že prostředky nebudou zřízeny v geografických oblastech, kde by to bylo v rozporu s požadavky předpisů.

Zásady můžeme využít i k omezení velikosti virtuálních počítačů, které je povoleno nasazovat. Může být užitečné povolit v produkčním předplatném větší velikosti virtuálních počítačů, ale zároveň se postarat o minimalizaci nákladů v předplatných pro vývojové prostředí. Když v těchto předplatných pomocí zásady zakážete velké virtuální počítače, zajistíte tak, že se ve vývojovém prostředí nebudou nasazovat.

Zásady se dají využít také k vynucování zásad pro vytváření názvů. Pokud organizace používá konkrétní standardizované zásady vytváření názvů, pomůže uplatnění zásad prosazujících tyto standardy zajistit konzistentní pojmenování všech prostředků Azure.

## <a name="summary"></a>Shrnutí

Zásady jsou flexibilní a najdou spoustu různých uplatnění. Jsou dalším nástrojem, který můžete využít ke zlepšení celkového uspořádání prostředků Azure.