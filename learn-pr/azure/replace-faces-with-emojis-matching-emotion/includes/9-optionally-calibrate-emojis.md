Obrázek emoji nemůžete předat do rozhraní API pro rozpoznávání tváře, abyste zjistili jeho emoce, protože se nejedná o lidskou tvář. Pro každé emoji proto potřebujete souřadnice emocí.

V tomto modulu jsme použili velmi jednoduchou metodu generování souřadnic emocí. Tato metoda

- Používá obrázek jedné skutečné tváře s výrazem, který se přibližuje výrazu obrázku emoji. Tento obrázek označujeme jako proxy obrázek.
- Předá tento obrázek do rozhraní Azure API pro rozpoznávání tváře.
- Použije výsledek této operace jako souřadnice emocí pro příslušný obrázek emoji.

V této lekci si ukážeme, jak můžete použít vlastní proxy obrázky a překalibrovat souřadnice emocí u obrázku emoji.

Jako rozšíření byste mohli také navrhnout přísnější metodu tohoto mapování, například tím, že byste k natrénování použili velký počet sad obrázků.

## <a name="proxy-images"></a>Proxy obrázky

Seznam proxy obrázků pro každý obrázek emoji najdete ve složce `shared/proxy-images` v ukázkách kódu.

![Sada spárovaných obrázků některých poradců pro cloudový vývoj s různými expresivními výrazy vedle odpovídajících obrázků převedených na emoji.](../media/team.jpg)

Tyto proxy obrázky byly předány do rozhraní API pro rozpoznávání tváře, aby byly vygenerovány body emocí, které pak byly přiřazeny k jednotlivým obrázkům emoji. Výsledky dodané sady proxy obrázků si můžete prohlédnout v poli `MOJIS` v souboru `shared/models/mojis.ts` ve zdrojovém kódu.

## <a name="create-your-own-proxy-images-for-emojis"></a>Vytvoření vlastních proxy obrázků pro emoji

>[!NOTE]
> Pokud chcete, můžete použít své vlastní obrázky. Může být zábavné převést ve Slacku sebe nebo kolegy z práce na emoji.

Pokud chcete použít vlastní obrázky, vyfoťte sami sebe, jak napodobujete jednotlivé obrázky emoji ve složce `shared/proxy-images`, a nahraďte původní sadu obrázků.

## <a name="create-an-azure-function-to-calibrate-your-proxy-images"></a>Vytvoření funkce Azure ke kalibrování vlastních proxy obrázků

Stejným způsobem, jakým jste postupovali u funkcí `MojifyImage` a `RespondToSlackCommand`, vytvořte další funkci s názvem `Calibrate`.

Soubor index.js nahraďte souborem index.ts a do tohoto souboru zkopírujte následující kód:

```typescript
import { EmotivePoint } from "../shared/models/emotivePoint";
import { Face } from "../shared/models/faces";
import * as FaceApi from "../shared/faceapi";
import * as emojiLookup from "emoji-dictionary";

const EMOJIS_TO_TRAIN = [
  "☺️",
  "🤓",
  "😃",
  "😆",
  "😉",
  "😍",
  "😎",
  "😐",
  "😕",
  "😖",
  "😘",
  "😜",
  "😝",
  "😠",
  "😧",
  "😩",
  "😬",
  "😭",
  "😱",
  "😳",
  "😴"
];

async function getCalibrationArrayString(context) {
  let str = "[";

  for (let emoji of EMOJIS_TO_TRAIN) {
    context.log(`Processing ${emoji}`);
    // Given an emoji like 😴 returns the word version, like 'sleepy'
    let emojiName = emojiLookup.getName(emoji);
    let emotion = await FaceApi.getEmotionFromLocalProxyImage(context, emojiName);
    let point = new EmotivePoint(emotion);
    let face = new Face(point, null);
    str += `{
        emotiveValues: new EmotivePoint({
            anger: ${emotion.anger},
            contempt: ${emotion.contempt},
            disgust: ${emotion.disgust},
            fear: ${emotion.fear},
            happiness: ${emotion.happiness},
            neutral: ${emotion.neutral},
            sadness: ${emotion.sadness},
            surprise: ${emotion.surprise}
        }),
        emojiIcon: "${emoji}"
      },`;
  }
  str += "]";
  return str;
}

export async function index(context, req) {
    context.log(`Calibrate HTTP trigger`);

    const array = await getCalibrationArrayString(context);
    const body = {MOJIS: array};

    context.res = {
      status: 200,
      headers: {
        "Content-Type": "application/json"
      },
      body: body
    };
}
```

## <a name="try-it-out"></a>Vyzkoušet

A teď ta nejzábavnější část. U každého obrázku ve složce `shared/proxy-images` použijeme rozhraní API pro rozpoznávání tváře, aby v emocionálním prostoru vypočítalo emocionální bod příslušného obrázku emoji.

Zkontrolujte, že aplikace funkcí běží – spusťte ji z nabídky ladění nebo z terminálu.
```bash
func host start
```

Spusťte novou funkci `Calibrate` tím, že se připojíte k http://localhost:7071/api/Calibrate.

> [!NOTE]
> Rozhraní API pro rozpoznávání tváře omezuje rychlost, jakou může být voláno. Pokud dojde k překročení limitu rychlosti, počká kód 30 sekund a zkusí to znovu. Protože funkce kalibrace provádí volání v rychlém sledu, může dojít k dosažení tohoto limitu. V takovém případě uvidíte, že provedení funkce kalibrace trvá jednu nebo dvě minuty.

Výstup příkazu se zobrazí v okně prohlížeče jako pole souboru json s body emocí. Můžete vyzkoušet různé proxy obrázky a zjistit, jak se mění body emocí. Toto pole můžete také zkopírovat zpět do souboru `shared/models/mojis.ts` a znovu nasadit aplikaci funkcí, abyste v příkazu `/mojify` ve Slacku používali vlastní proxy obrázky.
