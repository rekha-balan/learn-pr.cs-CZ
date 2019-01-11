Než se dáte do nastavování a psaní kódu, vyplatí se odpovědět na několik obecných otázek.

## <a name="how-do-you-map-an-emotion-to-an-emoji"></a>Jak namapujete emoce na emoji?

Představte si, že by existovaly jenom dvě emoce, strach a štěstí, s hodnotami v rozsahu od 0 do 1. Každou tvář byste pak mohli vykreslit ve 2D _emocionálním prostoru_ na základě emocí uživatele, například takto:

![Jednoduchý graf. Štěstí je na ose y, strach je na ose x a ukázkový obrázek tváře člověka z úvodní lekce je vykreslený v prázdném místě přibližně v 1/4 svisle a ve 3/4 vodorovně.](../media/graph-1.jpg)

Můžete to použít k nalezení emocionálního bodu pro každé emoji, stejně jako jste to udělali pro tvář. Po vykreslení emoji ve 2D emocionálním prostoru můžete zjistit, která emoji je nejblíže k emocím v obrázku.

![Aktualizovaná verze předchozího grafu ukazuje různé emoji vykreslené v různých bodech na stupnici štěstí-strach. Je to třeba usmívající se emoji se srdíčky v očích, která je na ose štěstí výš a na ose strachu blíž, nervózní emoji, která je na ose strachu dál a na ose štěstí níž, a plačící emoji, která je na ose štěstí níž a na ose strachu dál.](../media/graph-2.png)

Tato kalkulace se nazývá _euklidovská vzdálenost_. Nepoužívá se jenom ve 2D prostoru, ale spíše v 8D emocionálním prostoru, ve kterém se měří vztek, opovržení, znechucení, strach, štěstí, neutrálnost, smutek a překvapení.

> [!Tip]
> Abychom si to usnadnili, použili jsme balíček npm [euclidean-distance](https://www.npmjs.com/package/euclidean-distance?azure-portal=true).

## <a name="how-do-you-calculate-the-emotion-of-a-face"></a>Jak z výrazu tváře vypočítáte emoce?

### <a name="the-face-api"></a>Rozhraní API pro rozpoznávání tváře

Výpočet emocí je překvapivě jedna z nejpřístupnějších částí aplikace. [Rozhraní API pro rozpoznávání tváře](https://azure.microsoft.com/services/cognitive-services/face?azure-portal=true) služby Azure Cognitive Services přijímá na vstupu obrázek a vrací informace o obrázku, včetně toho, jestli byla rozpoznána tvář, a včetně jejího umístění. Pokud je to požadováno, také vypočítá a vrátí emoce obličejů, například takto:

```json
{
  "anger": 0.572,
  "contempt": 0.025,
  "disgust": 0.242,
  "fear": 0.001,
  "happiness": 0.014,
  "neutral": 0.111,
  "sadness": 0.033,
  "surprise": 0.003
}
```

Vezměte si například tento obrázek:

![Detailní obrázek tváře ženy. Žena má hnědé vlasy, brýle, usmívá se a dívá se přímo na pozorovatele.](../media/example-face.jpg)

Když chcete tento obrázek zpracovat, odešlete do koncového bodu rozhraní API požadavek POST, například takovýto:

 https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion

Obrázek uvedete v těle požadavku:

```json
{
  "url": "<path-to-image>"
}
```

> [!Note]
> Rozhraní API nevrací emoce standardně. Je potřeba explicitně zadat parametr dotazu `returnFaceAttributes=emotion`.

Rozhraní API se ověřuje pomocí tajného kódu. Tento klíč je nutné odeslat s hlavičkou.

```
Ocp-Apim-Subscription-Key: <your-subscription-key>
```

Rozhraní API s výše uvedenými parametry dotazu by vrátilo odpověď JSON podobnou této:

```json
[
  {
    "faceRectangle": {
      "top": 207,
      "left": 198,
      "width": 229,
      "height": 229
    },
    "faceAttributes": {
      "emotion": {
        "anger": 0.001,
        "contempt": 0.014,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.306,
        "neutral": 0.675,
        "sadness": 0.003,
        "surprise": 0.001
      }
    }
  }
]
```

Vrátí pole výsledků – jeden výsledek pro každou tvář rozpoznanou na obrázku. Každý výsledek obsahuje velikost a umístění tváře jako `faceRectangle` a emoce vyjádřené číslem od 0 do 1 jako `faceAttributes`.

Volání rozhraní API pro rozpoznávání tváře jsou zabaleny do funkcí usnadnění v [kódu Mojifieru](https://github.com/MicrosoftDocs/mslearn-the-mojifier/blob/master/shared/faceapi/index.ts?azure-portal=true)

### <a name="emotivepoint-class"></a>Třída EmotivePoint

Když se pozorně podíváte na třídu `EmotivePoint` v souboru [shared/emotivePoint.ts](https://github.com/MicrosoftDocs/mslearn-the-mojifier/blob/master/shared/models/emotivePoint.ts?azure-portal=true), všimnete si několika věcí.

Konstruktor přebírá jako vstup objekt obsahující informace o emocích a ukládá je jako místní členské proměnné, například takto:

```typescript
 constructor({
    anger,
    contempt,
    disgust,
    fear,
    happiness,
    neutral,
    sadness,
    surprise
  }) {
    this.anger = anger;
    this.contempt = contempt;
    this.disgust = disgust;
    this.fear = fear;
    this.happiness = happiness;
    this.neutral = neutral;
    this.sadness = sadness;
    this.surprise = surprise;
  }
```

Obsahuje také funkci `distance`, kterou můžeme použít k výpočtu euklidovské vzdálenosti mezi dvěma emocionálními body, například takto:

```typescript
  distance(other) {
    let myPoint = this.toArray();
    let otherPoint = other.toArray();
    return distance(myPoint, otherPoint);
  }
```

Pomocí těchto informací můžete vytvořit dva emocionální body a vypočítat vzdálenost mezi nimi, například takto:

```typescript
let a = new EmotivePoint({
  /* ... */
});
let b = new EmotivePoint({
  /* ... */
});
let distance = a.distance(b);
```

### <a name="face-class"></a>Třída Face

Další pomocnou třídou je třída `Face`, která kombinuje několik různých vlastností, včetně emocionálního bodu `EmotivePoint` tváře a třídy `Rect`, což je obdélník definující ohraničení tváře.

Pokud si prohlédnete konstruktor třídy `Face` v souboru [shared/face.ts](https://github.com/MicrosoftDocs/mslearn-the-mojifier/blob/master/shared/models/faces.ts?azure-portal=true), uvidíte tento řádek kódu:

```typescript
this.moji = this.chooseMoji(this.emotivePoint);
```

- `emotivePoint` je samotný emocionální bod tváře.
- Funkce `chooseMoji` vrací odpovídající emoji na základě emocionálního bodu `emotivePoint` tváře.

```typescript
  chooseMoji(point) {
    let closestMoji = null;
    let closestDistance = Number.MAX_VALUE;
    for (let moji of MOJIS) {
      let emoPoint = moji.emotiveValues;
      let distance = emoPoint.distance(point);
      if (distance < closestDistance) {
        closestMoji = moji;
        closestDistance = distance;
      }
    }
    return closestMoji;
  }
```

- `MOJIS` je seznam emocionálních souřadnic všech emoji. Ty jsou generovány ze sady [proxy obrázků](https://github.com/MicrosoftDocs/mslearn-the-mojifier/blob/master/shared/proxy-images?azure-portal=true) přibližující emoce každé emoji. Jako volitelný krok můžete později v tomto modulu vygenerovat vlastní sadu proxy obrázků a nakalibrovat sadu emocionálních souřadnic emoji.
- Funkce `chooseMoji` vypočítá vzdálenost mezi tímto obličejem a všemi emoji a vrátí tu nejbližší.
