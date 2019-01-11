Kontaktní pracovníci distributorů skenují a nahrávají fotky regálů a polic, kam doplňují zboží. Jako vedoucí vývojář ve své společnosti jste zodpovědní za vytváření miniatur těchto fotek. Tyto miniatury se používají v online sestavách, které vytváříte pro prodejní tým. Nedávno si manažer prodeje stěžoval, že obrázky v sestavě jsou rozostřené a produkt na nich není uprostřed, takže skenování velké sestavy je obtížnější. Je na vás, abyste situaci zlepšili.

Rozhodnete se použít funkci generování miniatur rozhraní API pro počítačové zpracování obrazu. Možná to zvládne lépe než vaše funkce pro změnu velikosti.

Počítačové zpracování obrazu nejprve vygeneruje miniaturu ve vysokém rozlišení a pak analýzou objektů na obrázku určí oblasti zájmu (ROI). Počítačové zpracování obrazu pak obrázek ořízne, aby odpovídal požadavkům oblasti zájmu. Vytvořená miniatura může mít podle vašich potřeb jiný poměr stran než původní obrázek. Podívejte se, jak to funguje.

## <a name="calling-the-computer-vision-api-to-generate-a-thumbnail"></a>Volání rozhraní API pro počítačové zpracování obrazu k vygenerování miniatury

Operace `generateThumbnail` vytvoří obrázek miniatury, který bude mít vámi zadanou šířku a výšku. Ve výchozím nastavení služba analyzuje obrázek, identifikuje oblast zájmu (ROI) a na základě této oblasti zájmu vygeneruje souřadnice inteligentního oříznutí. Inteligentní oříznutí je nápomocné v případě, že zadáte jiný poměr stran, než má vstupní obrázek. Adresa URL požadavku má následující formát:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=<...>&height=<...>&smartCropping=<...>`

Rozhraní API je možné poskytnout různé parametry, aby se vygenerovala správná miniatura podle vašich potřeb. Parametry `width` a `height` jsou povinné. Řeknou rozhraní API, jakou velikost pro konkrétní obrázek potřebujete. Parametr `smartCropping` zajišťuje inteligentnější oříznutí díky analýze oblasti zájmu ve vašem obrázku, kterou by bylo vhodné v rámci miniatury zachovat. Když je inteligentní oříznutí povolené, zůstal by například na oříznuté profilové fotce obličej uživatele v rámečku obrázku i v případě, že by obrázek měl jiný poměr stran.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="generate-a-thumbnail"></a>Vytvoření miniatury

V tomto příkladu použijeme následující obrázek, ale stejný příkaz můžete vyzkoušet i u adres URL jiných obrázků.

![Obrázek roztomilého bílého psa sedícího v zelené trávě](../media/4-dog.png)

1. V Azure Cloud Shellu spusťte následující příkazy. Nahraďte v příkazu část `<region>` oblastí vašeho účtu služby Cognitive Services.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```

V tomto příkladu požádáme službu o vytvoření miniatury o velikosti 100 x 100. Inteligentní oříznutí je povolené. Úspěšná odpověď obsahuje binární kód miniatury obrázku, kterou zapíšeme do souboru s názvem thumbnail.jpg.

> [!CAUTION]
> Parametr `-o` přesměruje výstup do souboru. Tento soubor se vždy přepíše, takže pokud si chcete při zkoušení tohoto příkazu zachovat víc než jednu miniaturu, změňte název souboru.

## <a name="view-the-generated-thumbnail"></a>Zobrazení vygenerované miniatury

Vygenerovanou miniaturu najdete ve svém účtu úložiště Azure Cloud Shell. Soubor jsme pojmenovali **thumbnail.jpg**.

Cloud Shell ve službě Microsoft Learn nemůže stahovat soubory, ale když budete postupovat podle těchto pokynů, můžete si miniaturu stáhnout z webu Azure Portal.

1. Spusťte ve službě Azure Cloud Shell následující příkazy, abyste ověřili, že soubor **thumbnail.jpg** ve vaší domovské složce existuje.

    ```azurecli
    cd ~
    ls -l
    ```



1. Následujícím příkazem soubor `thumbnail.jpg` přesuňte do složky clouddrive.

    ```azurecli
    mv ~/thumbnail.jpg ~/clouddrive
    ```
1. Pomocí stejného účtu, kterým jste aktivovali sandbox, se přihlaste k portálu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).
1. Na panelu **Všechny prostředky** řídicího panelu Azure Portal vyberte účet úložiště, jehož název začíná na `cloudshell`.
1. Na panelu Účet úložiště vyberte **Průzkumník služby Storage**, potom vyberte **Sdílené složky** a nakonec vyberte sdílenou složku v kolekci s názvem začínajícím na **cloudshellfiles**.
1. Vyberte soubor *thumbnail.jpg* a potom z horní nabídky vyberte **Stáhnout**, abyste si obrázek zobrazili.

Operace `generateThumbnail` je výkonný generátor miniatur, který v miniatuře umí zachovat oblast zájmu (ROI) obrázku.

Další informace o operaci `generateThumbnails` najdete v referenční dokumentaci metody [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb).
