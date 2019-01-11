
## <a name="what-is-microsoft-cognitive-services"></a>Co jsou Microsoft Cognitive Services?

Microsoft Cognitive Services je sada algoritmů strojového učení, která je dostupná v podobě služby a kterou může používat kdokoli. Místo toho, abychom tuto inteligentní funkci vytvářeli pro naše aplikace úplně od začátku, můžeme pro zpracování obrazu, řeč, jazyk, znalosti a hledání využít tyto služby. Každou službu si můžete vyzkoušet zdarma. Když se rozhodnete některou službu integrovat do své aplikace, zaregistrujete si placené předplatné. V tomto modulu se zaměříme na rozhraní API pro počítačové zpracování obrazu.

> [!TIP]
> Pokud se chcete podívat na seznam všech služeb tohoto typu, projděte si [Adresář služeb Cognitive Services](https://azure.microsoft.com/services/cognitive-services/directory/). 

## <a name="what-is-the-computer-vision-api"></a>Co je rozhraní API pro počítačové zpracování obrazu?

Rozhraní API pro počítačové zpracování obrazu poskytuje algoritmy pro zpracování obrázků a vracení analytických informací. Můžete pomocí něj například zjistit, jestli není na obrázku obsah pro dospělé, nebo ho můžete použít k vyhledání všech tváří na obrázku. Obsahuje také další funkce, jako je odhad dominantní a doplňkové barvy, kategorizace obsahu obrázků a popis obrázku celou větou v angličtině. Kromě toho také dokáže inteligentně generovat miniatury pro efektivní zobrazování velkých obrázků.

> [!TIP]
> Rozhraní API pro počítačové zpracování obrazu je dostupné v mnoha oblastech po celém světě. Informace o nejbližší dostupné oblasti najdete v tématu [Dostupné produkty podle oblastí](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

K čemu můžete rozhraní API pro počítačové zpracování obrazu použít:

- Analýza obrázků za účelem získání přehledu
- Extrahování tištěného textu z obrázků s využitím optického rozpoznávání znaků (OCR)
- Rozpoznání tištěného a rukou psaného textu na obrázcích
- Identifikace známých osobností a orientačních bodů
- Analýza videí 
- Vytvoření miniatury obrázku 

## <a name="how-to-call-the-computer-vision-api"></a>Jak volat rozhraní API pro počítačové zpracování obrazu

Počítačové zpracování obrazu můžete ve své aplikaci vyvolat pomocí našich klientských knihoven, případně můžete volat přímo rozhraní REST API. V tomto modulu budeme volat rozhraní REST API. Volání rozhraní:

1. Získání přístupového klíče API

    Přístupové klíče se vám přiřadí, když si zaregistrujete účet služby pro počítačové zpracování obrazu. Klíč se musí předávat v hlavičce **každého** požadavku. 

1. Volání rozhraní API metodou POST

    Naformátujte adresu URL následujícím způsobem: **oblast**.api.cognitive.microsoft.com/vision/v2.0/**prostředek**/**[parametry]** 

    - **oblast** – oblast, ve které jste vytvořili účet, například *westus*.
    - **prostředek** – prostředek počítačového zpracování obrazu, který voláte, například `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.

    Obrázek, který se má zpracovat, můžete zadat buď jako binární kód obrázku raw, nebo jako adresu URL obrázku.

    Hlavička požadavku musí obsahovat klíč předplatného, který poskytuje přístup k tomuto rozhraní API.

1. Parsování odpovědi

    Podrobnější informace, které rozhraní API pro počítačové zpracování o obrázku získalo, obsahuje odpověď v datové části kódu JSON.

V tomto modulu budeme pro všechna praktická cvičení v Azure CLI používat integrované Cloud Shell. Pojďme se na toto nastavení podívat trochu blíž.

## <a name="what-is-the-azure-cli"></a>Co je Azure CLI?

Azure CLI je nástroj příkazového řádku od Microsoftu pro správu prostředků Azure, který je určený pro různé platformy. Je k dispozici pro macOS, Linux a Windows a také v prohlížeči prostřednictvím služby [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> Aktuálně jsou k dispozici dvě verze nástroje Azure CLI: Azure CLI 1.0 a Azure CLI 2.0. My budeme používat Azure CLI 2.0, což je nejnovější a zároveň doporučená verze, pokud nepotřebujete spouštět starší skripty napsané pro verzi 1.0. Azure CLI 1.0 spustíte příkazem `azure` a Azure CLI 2.0 spustíte příkazem `az`.

## <a name="az-cognitiveservices-commands"></a>příkazy az cognitiveservices

Azure CLI zahrnuje příkaz `cognitiveservices` ke správě účtů služeb Cognitive Services v Azure. K provedení konkrétních úkolů použijeme různé podpříkazy. Nejčastější příkazy:

| Podpříkaz | Popis |
|-------------|-------------|
| `list` | Seznam dostupných účtů služeb Cognitive Services |
| `account show` | Získání podrobností o účtu služeb Azure Cognitive Services |
| `account create` | Vytvoření účtu Azure Cognitive Services |
| `account delete` | Odstranění účtu Azure Cognitive Services |
| `keys list` | Výpis klíčů účtu Azure Cognitive Services |

Pojďme vytvořit služby Cognitive Services pomocí Azure CLI.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a>Vytvoření účtu Cognitive Services

Potřebujeme přístupový klíč rozhraní API pro volání rozhraní API pro počítačové zpracování obrazu. K získání přístupových klíčů potřebujeme účet služeb Cognitive Services pro rozhraní API pro počítačové zpracování obrazu. K vytvoření účtu v našem předplatném použijeme `az cognitiveservices create`.

 K vytvoření účtu služeb Cognitive Services ve skupině prostředků slouží příkaz `az cognitiveservices create`.  Při volání tohoto příkazu je nutné zadat následujících pět parametrů.

> [!Tip]
> Většina příznaků pro parametry Azure CLI je možné zkrátit na jeden znak. Můžeme například místo `--location` použít `-l`. Delší podoba příznaků se používá pro přehlednost.

| Parametr | Popis |
|-----------|-------------|
| `resource-group` | Skupina prostředků, která bude vlastníkem účtu služeb Cognitive Services. V této interaktivní relaci sandboxu budete používat <rgn>[název skupiny prostředků sandboxu]</rgn> |
| `kind` | Název rozhraní API účtu služeb Cognitive Services |
| `name` | Název účtu služeb Cognitive Services |
| `sku` | Skladová položka účtu služeb Cognitive Services|
| `location` | Umístění (oblast), ze kterého chcete provádět volání do tohoto rozhraní API. Ze seznamu níže vyberte jedno z míst. |

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

V Azure Cloud Shellu proveďte následující příkaz. Nezapomeňte nahradit `[location]` místem ve vaší blízkosti.

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--location [location]
```

> [!NOTE]
> Místo, které jste vybrali, si zapamatujte. Všechna volání rozhraní API budete provádět z této oblasti.

Vytvořili jsme účet služeb Cognitive Services pro rozhraní API pro **počítačové zpracování obrazu**. Vybrali jsme skladovou jednotku *S1* a účet pojmenovali **ComputerVisionService**. Vlastníkem našeho účtu je skupina prostředků **<rgn>[název skupiny prostředků sandboxu]</rgn>** a rozhraní API vyvoláme z místa, které jsme nastavili v parametru `--location`. 

Jakmile příkaz dokončí vytváření účtu služeb Cognitive Services, získáte odpověď ve formátu JSON, která bude obsahovat vlastnost **provisioningState** nastavenou na hodnotu **Succeeded**.

## <a name="get-an-access-key"></a>Získání přístupového klíče

Jakmile budeme mít účet úspěšně vytvořený, můžeme pro něho načíst klíče předplatného neboli přístupové klíče.

1. V Azure Cloud Shellu spusťte následující příkaz:

    ```azurecli
    az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[sandbox resource group name]</rgn>
    ```
    
    Výše uvedený příkaz vrátí klíče přidružené k účtu služeb Cognitive Services s názvem **ComputerVisionService**, který vlastní daná skupina prostředků. Vrátí dva klíče – jeden je náhradní. Klíče jsou obtížně zapamatovatelné, takže první klíč uložíme v proměnné, kterou budeme používat pro všechna volání rozhraní API.

2.  V Azure Cloud Shellu spusťte následující příkaz:

    ```azurecli
    key=$(az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query key1 -o tsv)
    ```
    
    Azure CLI 2.0 pomocí argumentu `--query` provádí dotazy JMESPath nad výsledky příkazů. JMESPath je dotazovací jazyk pro formát JSON, který poskytuje možnost vybrat data z výstupu rozhraní příkazového řádku a prezentovat je. Tyto dotazy se spouštějí na výstupu JSON ještě před jakýmkoli jiným formátováním zobrazení.
    Argument `--query` podporují všechny příkazy v rozhraní příkazového řádku Azure. 
    
    V našem příkladu zadáme dotaz na seznam klíčů pro položku s názvem key1 a výsledek si necháme zobrazit ve formátu **tsv**. Tento formát odebere uvozovky kolem řetězcové hodnoty. Výsledek přiřadíme ke **klíči** proměnné.
    
    > [!IMPORTANT]
    > Tento klíč budeme používat v celém tomto modulu, takže jeho uložení do proměnné bude velmi užitečné. Pokud hodnotu ztratíte nebo se zruší nastavení proměnné, spusťte příkaz znovu, aby se opět nastavila.  

3. Pokud si chcete hodnotu našeho klíče zobrazit, spusťte v Azure Cloud Shellu následující příkaz:

    ```azurecli
    echo $key
    ```

Když teď máme účet a klíč, je čas provést určitá volání rozhraní API.