Pokud chcete rozhraní API pro analýzu textu vidět v akci, můžeme pomocí integrované konzoly pro testování rozhraní API umístěné v online referenční dokumentaci vyzkoušet některá volání. Nejprve ale k těmto voláním budeme potřebovat přístupový klíč.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-access-key"></a>Vytvoření přístupového klíče

Každé volání rozhraní API pro analýzu textu vyžaduje klíč předplatného. Často se mu říká přístupový klíč a používá se k ověření, že máte k provedení volání přístup. K získání klíče použijeme web Azure Portal.

1. Pomocí stejného účtu, kterým jste aktivovali sandbox, se přihlaste k portálu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Klikněte na **Vytvořit prostředek**.

1. Do vyhledávacího pole **Hledat na Marketplace** zadejte spojení *analýza textu* a stiskněte klávesu ENTER.

1. Vyberte možnost **Analýza textu** ve výsledcích hledání a pak vyberte **vytvořit** tlačítko v pravém dolním rohu obrazovky.

1. Na stránce **Vytvořit**, která se otevře, zadejte do každého pole následující hodnoty.

    |Vlastnost  | Hodnota  | Popis  |
    |---------|---------|---------|
    |Název     |    MyTextAnalyticsAPIAccount     |  Název účtu služby Cognitive Services (Doporučujeme použít popisný název.) Platné znaky jsou `a-z`, `0-9` a `-`.    |
    |Předplatné     |  Předplatné Concierge    |   Předplatné, ve kterém jste vytvořili tento nový účet rozhraní API služby Cognitive Services s **rozhraním API pro analýzu textu**.      |
    |Umístění     |  *z rozevíracího seznamu zvolte oblast*       |  Vzhledem k tomu, že používáte bezplatný sandbox, vyberte z rozevíracího seznamu umístění, které je **také** v seznamu oblastí sandboxu níže.  |
    |Cenová úroveň     | **F0 Free**     |   Náklady na účet služby Cognitive Services závisí na skutečném využití a zvolených možnostech. Pro tyto účely doporučujeme zvolit bezplatnou úroveň.      |
    |Skupina prostředků     |  Vyberte **Použít existující** a zvolte <rgn>[název skupiny prostředků sandboxu]</rgn>       |  Pojmenujte novou skupinu prostředků, ve které chcete vytvořit svůj účet služby Cognitive Services s rozhraním API pro analýzu textu.       |

    ### <a name="sandbox-region-list"></a>Seznam oblastí sandboxu
    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    > [!TIP]
    > Zapamatujte si umístění, které jste vybrali při vytváření účtu služeb Cognitive Services pro analýzu textu. Použijete ho brzy k volání rozhraní API. 

1. V dolní části stránky vyberte **Vytvořit** a zahajte vytváření účtu. Počkejte na oznámení, že probíhá nasazování. Potom se objeví oznámení, že se účet do skupiny prostředků úspěšně nasadil.

    ![Oznámení o úspěšném nasazení s tlačítky Přejít k prostředku a Připnout na řídicí panel](../media/deploy-resource-group-success.PNG)

## <a name="get-the-access-key"></a>Získání přístupového klíče

Když už máme účet služby Cognitive Services vytvořený, pojďme najít přístupový klíč, abychom mohli začít volat rozhraní API.

1. V oznámení *Nasazení bylo úspěšné.* klikněte na tlačítko **Přejít k prostředku**. Tato akce otevře Rychlý start účtu.

1. Z nabídky vlevo nebo v části *Vezměte si svoje klíče* vyberte položku **Klíče**. Tím se otevře stránka **Spravovat klíče**.

1. Jeden z klíčů pomocí tlačítka pro kopírování zkopírujte.

    ![Uživatelské rozhraní pro správu klíčů zobrazující název účtu služby Cognitive Services a položky Klíč 1 a Klíč 2.](../media/manage-keys.PNG)

    > [!IMPORTANT]
    > Své přístupové klíče vždy chraňte a nikdy je s nikým nesdílejte.

1. Tento klíč budete potřebovat ve zbývající části tohoto modulu. Brzy ho použijeme k volání rozhraní API z konzoly pro testování a budeme ho používat i v dalších částech modulu.

## <a name="call-the-api-from-the-testing-console"></a>Volání rozhraní API z konzoly pro testování

Když teď máme klíč, můžeme přejít do konzoly pro testování a rozhraní API vyzkoušet.

1. Přejděte na následující adresu URL ve svém oblíbeném prohlížeči. Změňte `[location]` za umístění, které jste dříve vybrali při vytváření účtu služeb Cognitive Services pro analýzu textu. Například, pokud jste vytvořili účet v *eastus*, změňte v adrese URL `[location]` na `eastus`.

    ```bash
    https://[location].dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0
    ```

    Cílová stránka zobrazí nabídku na levé straně a obsah vpravo. V nabídce jsou uvedené metody POST, které můžete v rozhraní API pro analýzu textu volat. Těmito koncovými body jsou **Zjistit jazyk**, **Entity**, **Klíčové fráze** a **Mínění**. Pokud chcete jednu z těchto operací volat, potřebujeme nejprve provést několik kroků.

    - Vyberte požadovanou metodu volání.
    - Každému volání přidejte přístupový klíč, který jste si dříve v této lekci uložili.

1. Z levé nabídky vybere **Mínění**. Tato volba otevře na pravé straně dokumentaci Mínění. Jak je vidět v dokumentaci, provedeme volání REST v následujícím formátu.  

    `https://[location].api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

     `[location]` se nahradí umístěním, které jste vybrali při vytváření účtu pro analýzu textu.  
Do hlavičky **ocp-Apim-Subscription-Key** předáme klíč předplatného nebo přístupový klíč.

## <a name="make-some-api-calls"></a>Volání rozhraní API

1. Tlačítkem **Otevřete testovací konzolu rozhraní API** otevřete živou, interaktivní konzolu rozhraní API.

1. Do pole s označením **Ocp-Apim-Subscription-Key** vložte přístupový klíč, který jste si předtím uložili. Všimněte si, že se klíč automaticky zapíše do okna požadavku HTTP jako hodnota hlavičky.

1. Přejděte do dolní části stránky a klikněte na **Odeslat**.

    Podívejme se podrobněji na části této obrazovky.

    V části uživatelského rozhraní Hlavičky nastavíme v hlavičce požadavku přístupový klíč nebo klíč předplatného.

    ![Snímek obrazovky s částí Hlavičky](../media/2-marker.PNG)

    Vedle se nachází část Text požadavku, ve které je pole **dokumentů**. Každý dokument v poli má tři vlastnosti. Těmi vlastnostmi jsou *jazyk*, *id* a *text*. Vlastnost *id* je v tomto příkladu číslo, ale nemusí to být jen číslo, důležité je, aby byla v poli dokumentů jedinečná. V tomto příkladu máte dokumenty napsané ve třech různých jazycích. Funkce Mínění v rozhraní API pro analýzu textu podporuje přes 15 jazyků. Další informace najdete v tématu o [podporovaných jazycích v rozhraní API pro analýzu textu](https://docs.microsoft.com//azure/cognitive-services/text-analytics/text-analytics-supported-languages). Maximální velikost jednoho dokumentu je 5 000 znaků a jeden požadavek může mít až 1 000 dokumentů.

    ![Snímek obrazovky s částí Text požadavku](../media/3-marker.PNG)

    V další části můžete vidět hotový požadavek včetně hlaviček a adresy URL požadavku. V tomto příkladu si můžete všimnout, že se požadavky směrují na adresu URL, která začíná `westus`. Adresa URL se liší v závislosti na umístění, které jste vybrali při vytváření účtu pro analýzu textu.

    ![Snímek obrazovky s částí číslo čtyři zobrazující adresu URL požadavku](../media/4-marker.PNG)
    ![Snímek obrazovky s částí číslo pět zobrazující podrobnosti požadavku HTTP](../media/5-marker.PNG)

    Pak máme informace o odpovědi. V tomto příkladu vidíme, že byl požadavek úspěšný a vrátil se kód `200`. Můžeme si také všimnout, že doba odezvy byla 38 ms.

    ![Snímek obrazovky s částí číslo pět zobrazující stav odpovědi](../media/6-marker.PNG)

    A konečně vidíme odpověď na požadavek. Odpověď obsahuje přehled rozhraní API pro analýzu textu o dokumentech. Vrátí se nám pole dokumentů bez původního textu. U každého pole získáme *id* a *skóre*. Rozhraní API vrací číselné skóre v rozsahu 0 až 1. Skóre blížící se 1 značí pozitivní mínění, zatímco skóre blížící se 0 značí negativní mínění. Skóre s hodnotou 0,5 značí žádné nebo neutrální mínění. V tomto příkladu máme dva poměrně pozitivní dokumenty a jeden negativní dokument.

    ![Snímek obrazovky s částí číslo pět zobrazující obsah odpovědi](../media/7-marker.PNG)

Blahopřejeme! Provedli jste první volání rozhraní API pro analýzu textu, aniž byste museli napsat jediný řádek kódu. Nebojte se zůstat v konzole a vyzkoušet další volání. Tady máte několik návrhů:

- Změňte dokumenty v části číslo 2 a zjistěte, co rozhraní API vrátí.
- Použijte stejný klíč předplatného a vyzkoušejte jiné metody: **Zjistit jazyk**, **Entity** nebo **Klíčové fráze**.
- Zkuste pomocí svého předplatného provést volání z jiné oblasti a sledujte, co se stane.

Konzola pro testování rozhraní API je skvělým způsobem, jak možnosti tohoto rozhraní API prozkoumat. Když jste si to teď vyzkoušeli, pojďme tuto zkušenost převést do scénáře z reálného světa.