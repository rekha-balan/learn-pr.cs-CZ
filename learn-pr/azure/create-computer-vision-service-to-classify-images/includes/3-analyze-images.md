Jako vedoucí vývojář ve společnosti Contoso Beverage Distribution jste zodpovědní za vytváření a správu podnikové aplikace, ve které kontaktní pracovníci distributorů skenují a nahrávají fotky regálů a polic, kam doplňují zboží. 

U všech obrázků chcete ověřit, že dodržují pravidla obsahu stanovená vaší společností. Společnost nechce mít na svých webech publikovaný nevhodný obsah. Vzhledem ke značnému pokroku v oblasti umělé inteligence se rozhodnete použít ve své aplikaci rozhraní API pro počítačové zpracování obrazu. 

Abyste mohli začít, vytvoříte ve svém předplatném Azure účet pro počítačové zpracování obrazu a spustíte testování funkce **analýzy**.

## <a name="calling-the-computer-vision-api-to-analyze-images"></a>Volání rozhraní API pro počítačové zpracování obrazu k analýze obrázků

Operace `analyze` extrahuje bohatou sadu vizuálních funkcí na základě obsahu obrázku. Adresa URL požadavku má následující formát:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=<...>&details=<...>&language=<...>`

Do služby se odesílají parametry `visualFeatures`, `details` a `languages`. Nastavte parametr `details` na hodnotu `Landmarks` nebo `Celebrities`. Usnadní vám to identifikaci orientačních bodů nebo známých osobností. Parametr `visualFeatures` určuje, jaký druh informací vám má služba vrátit. Možnost `Categories` kategorizuje obsah obrázků, například stromy, budovy a podobně. `Faces` identifikuje obličeje lidí a poskytne vám informace o jejich pohlaví a věku.

Pokud jde o obrázky, jejichž zpracování požadujete, mají veškeré operace v rozhraní API pro počítačové zpracování obrazu následující omezení:

- Podporované formáty obrázků: JPEG, PNG, GIF, BMP. 
- Velikost souboru obrázku musí být menší než 4 MB.
- Rozměry obrázku musí být nejméně 50 × 50.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a>Určení orientačních bodů na obrázku

Začněme voláním pro vyhledání orientačních bodů na obrázku. V tomto příkladu použijeme následující obrázek, ale stejný příkaz můžete vyzkoušet i u adres URL jiných obrázků. 

![Obrázek horského pásma s modrou oblohou nad sněhem pokrytými horami](../media/3-mountains.jpg)

V Azure Cloud Shellu proveďte následující příkaz. Nahraďte v příkazu část `<region>` oblastí vašeho účtu služby Cognitive Services.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

- Toto volání hledá orientační body na obrázku určeném adresou URL. Obrázek, který analyzujeme, je pro účely tohoto cvičení uložený v úložišti GitHub. 
- Volání také požádá službu, aby vrátila informace o kategoriích a popis obrázku. Popis se vrátí jako kompletní věta v angličtině. 
- Jak už víte, vyžaduje každé volání rozhraní API přístupový klíč. Ten je nastavený v záhlaví `Ocp-Apim-Subscription-Key` žádosti. 

> [!TIP]
> Výsledkem žádosti je popis obrázku v nezpracovaném formátu JSON v podobě `url`. Abychom vylepšili výstup JSON, přidali jsme na konec příkazu ` | jq '.'`.

Odpověď JSON z tohoto volání vrátí následující položky:

- Pole `categories` se seznamem všech zjištěných kategorií obrázků a skóre s hodnotou v rozmezí 0 až 1, které ukazuje, s jakou jistotou dokázala služba zařadit obrázek do dané kategorie.
- Záznam `description` obsahující pole se značkami nebo slovy vztahujícími se k obrázku.
- Záznam `captions` obsahující textové pole, které popisuje v angličtině, co je na obrázku. Můžete vidět, že text obsahuje také skóre určující jistotu. Toto skóre vám může pomoci při rozhodování, jak při této analýze postupovat dál.


## <a name="check-for-inappropriate-content-in-an-image"></a>Vyhledání nevhodného obsahu na obrázku

V tomto příkladu budeme analyzovat obrázek, jestli na něm není obsah pro dospělé. Skóre spolehlivosti vyhodnotí pravděpodobnost, že je na obrázku nevhodný obsah nebo obsah pro dospělé. 

V tomto příkladu použijeme následující obrázek, ale stejný příkaz můžete vyzkoušet i u adres URL jiných obrázků. 

![Obrázek usmívající se rodiny.](../media/3-people.png)

1. V Azure Cloud Shellu spusťte následující příkaz a nahraďte `<region>` v adrese URL.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

V tomto příkladu nastavíme pro `visualFeatures` hodnotu `Adult,Description`. 

Odpověď nám dává dvě skóre spolehlivosti, jedno pro nevhodný obsah a jedno pro obsah pro dospělé. S použitím těchto skóre a popisu obrázku a dalších vizuálních funkcí můžete začít označovat obrázky odeslané na váš server.

Příklady, které jsme v tomto cvičení vyzkoušeli, vám umožní si udělat představu o typu analýzy, kterou můžete provést s použitím operace `analyze`. Vyzkoušejte analýzu u různých obrázků a zkuste různé kombinace vizuálních funkcí, detailů a parametrů jazyka.

Další informace o operaci `analyze` najdete v referenční dokumentaci k analýze obrázků ([Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)).