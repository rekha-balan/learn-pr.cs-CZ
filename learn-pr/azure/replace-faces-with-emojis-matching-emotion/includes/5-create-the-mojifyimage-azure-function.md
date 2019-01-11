Když jste teď vytvořili projekt funkcí Azure a minimální funkci Azure v JavaScriptu, budete tuto funkci Azure vyvíjet v jazyce TypeScript. Konečná funkce načte adresu URL obrázku ze svých parametrů dotazu, pomocí rozhraní API pro rozpoznávání tváře najde tváře a jejich emoce na obrázku a následně nahradí tváře odpovídajícími emoji. Vrátí složený obrázek jako nezpracovaná data obrázku.

## <a name="convert-to-typescript"></a>Převedení na TypeScript

Rozšíření funkce Azure Functions vytvořilo funkci jazyka JavaScript. Musíme ji převést do jazyka TypeScript.

1. Soubor `index.js` přejmenujte na `index.ts`.

    > [!NOTE]
    > Pokud spustíte příkaz `npm run build` a s ním související příkaz `tsc -w`, pak se soubor `index.ts` okamžitě zkompiluje do souboru `index.js`. Zároveň by se měl vytvořit soubor `index.js.map`. Pokud se soubory `index.js` a `index.js.map` nevytvoří automaticky, zkontrolujte, jestli běží příkaz `npm run build` a že se ve výstupu nezobrazují žádné chyby.

1. Nahraďte kód v souboru `index.ts` následujícím kódem:

    ```typescript
    export async function index(context, req) {
      context.log("ReplyWithMojifiedImage HTTP trigger");
      context.res.body = "Hello!";
    }
    ```

    > [!NOTE]
    > K zaznamenávání výstupu do konzoly používáme v Azure Functions soubor `context.log` místo souboru `console.log`.

## <a name="test-your-function"></a>Otestování funkce

1. Ujistěte se, že je spuštěná místní aplikace funkcí Azure, a to pomocí jedné z metod, které jste použili dříve: buď použijte příkaz `func host start`, nebo ji spusťte z nabídky ladění pomocí úlohy **Attach to JavaScript Function** (Připojit k funkci v JavaScriptu).

    Pokud se aplikace funkcí spustila správně, mělo by okno výstupu zobrazovat podobný výstup:

    ```
    Http Functions:
            MojifyImage: http://localhost:7071/api/MojifyImage
    ```

1. Přejděte na adresu URL v prohlížeči. Pokud všechno funguje správně, mělo by se v prohlížeči zobrazit `Hello!`.

    Pokud vidíte `Hello`, znamená to, že jste trigger funkce převedli z TypeScriptu do JavaScriptu.

## <a name="configure-your-face-api-environment-variables"></a>Konfigurace proměnných prostředí vašeho rozhraní API pro rozpoznávání tváře

Aby byl váš kód lépe zabezpečený, neměli byste v něm kódovat soukromá data napevno. Místo toho pro ukládání adresy URL rozhraní API pro rozpoznávání tváře a klíče používáme proměnné prostředí.

Při vytváření projektu Azure Functions se vytvoří soubor s názvem `local.settings.json`. Tento soubor se nachází v souboru `.gitignore`, takže se nevrací se změnami do správy zdrojového kódu. Slouží jako místo pro ukládání citlivé, případně místní konfigurace, kterou nechcete zveřejňovat. Vše v objektu `Values` se zpřístupní kódu Node.js jako proměnná prostředí.

1. Otevřete `local.settings.json` v editoru Visual Studio Code. Ve výchozím nastavení má soubor tento obsah:

    ```json
    {
      "IsEncrypted": false,
      "Values": {
        "AzureWebJobsStorage": "",
        "FUNCTIONS_WORKER_RUNTIME": "node"
      }
    }
    ```

1. **Nahraďte** obsah tohoto souboru proměnnými rozhraní API pro rozpoznávání tváře (tj. odeberte ze souboru výchozí nastavení). Nahraďte hodnoty `<face-api-url>` a `<face-api-key>` proměnnými, které jste si uložili při vytváření prostředku Azure rozhraní API pro rozpoznávání tváře.

    ```json
    {
        "Values": {
          "FACE_API_URL": "<face-api-url>",
          "FACE_API_KEY": "<face-api-key>"
        }
    }
    ```

Proměnné `FACE_API_URL` a `FACE_API_KEY` budou dostupné pro Node.js jako proměnné prostředí.

Proměnné se používají k [volání rozhraní API pro rozpoznávání tváře](https://github.com/MicrosoftDocs/mslearn-the-mojifier/blob/master/shared/faceapi/index.ts).

## <a name="write-the-mojifyimage-function"></a>Vytvoření funkce MojifyImage

1. Nahraďte kód v souboru `MojifyImage/index.ts` následujícím kódem:

    ```typescript
    import Jimp = require("jimp");
    import * as path from "path";
    import * as FaceApi from "../shared/faceapi";

    async function createMojifiedImage(context, imageUrl, faces) {
      // TODO
    }

    export async function index(context, req) {
      context.log(`ReplyWithMojifiedImage HTTP trigger`);
      context.res.body = "Hello!";
    }
    ```

    V této lekci budete vyvíjet funkce `createMojifiedImage` a `index`.

1. Zavolejte rozhraní API pro rozpoznávání tváře s obrázkem a získejte odpověď.

    Tělo funkce `index(context, req)` v souboru `index.ts` nahraďte následujícím kódem:

    ```typescript
    const { imageUrl } = req.query;

    if (!imageUrl) {
      let message = `imageUrl is required`;
      context.log(message);
      context.res = { status: 400, body: message }
    } else {
      context.log(`Called with imageUrl: "${imageUrl}"`);

      // Get the response from the faceAPI
      try {
        let faces = await FaceApi.getFaces(context, imageUrl);
        context.log(faces);
        context.res = { status: 200, body: faces }
      } catch (err) {
        let message = `There was an error processing this image: ${err.message}`;
        context.log(message);
        context.res = { status: 400, body: message };
      }
    }
    ```

1. Spusťte tuto funkci ve vašem prohlížeči.

    Měli byste vidět odpověď JSON, kterou vrací rozhraní API pro rozpoznávání tváře při předání obrázku: `http://localhost:7071/api/MojifyImage?imageUrl=<image>`.

    Projděte si kód v souboru [`shared/faceapi/index.ts`](https://github.com/MicrosoftDocs/mslearn-the-mojifier/blob/master/shared/faceapi/index.tsazure-portal=true) a podívejte se, jak se odpověď z rozhraní API pro rozpoznávání tváře převádí na instanci pole objektů `Face`. Každý objekt `Face` obsahuje souřadnice emocí, umístění tváře na obrázku, odpovídající emoji a ikonu emoji.

1. Vytvořte „mojifikovaný“ obrázek.

    Vyplněním funkce TypeScript `createMojifiedImage(context, imageUrl, faces)` vytvoříte složený obrázek skládající se z původního obrázku spolu s jeho odpovídajícími emoji. Tato funkce využívá balíček npm `Jimp`, opensourcovou knihovnu pro manipulaci s obrázky.

    Nahraďte řádek `TODO` v této funkci následujícím kódem:

    ```typescript
    let sourceImage = await Jimp.read(imageUrl);
    // Create a composite image, we will "append" to this composite an emoji image for each face found
    let compositeImage = sourceImage;

    if (faces.length == 0) {
      throw new Error(`No faces found in image`);
    }

    for (let face of faces) {
      const mojiName = face.mojiName;
      const faceHeight = face.faceRectangle.height;
      const faceWidth = face.faceRectangle.width;
      const faceTop = face.faceRectangle.top;
      const faceLeft = face.faceRectangle.left;

      // Load the emoji from disk
      let mojiPath = path.resolve(
        __dirname,
        "../shared/emojis/" + mojiName + ".png"
      );
      let emojiImage = await Jimp.read(mojiPath);

      // Resize the emoji to fit the face
      emojiImage.resize(faceWidth, faceHeight);

      // Compose the emoji on the image
      compositeImage = compositeImage.composite(emojiImage, faceLeft, faceTop);
    }

    try {
      return await compositeImage.getBufferAsync(Jimp.MIME_JPEG);
    } catch (error) {
      context.log(`There was an error adding the emoji to the image: ${error}`);
      throw new Error(error);
    }
    ```

    Podívejme se na tento kód v jednotlivých krocích.

    - K načtení obrázku pomocí knihovny Jimp použijete funkci `Jimp.read`.

        ```typescript
        let sourceImage = await Jimp.read(imageUrl);
        ```

    - V umístění `shared/emojis` je adresář souborů png pro všechny emoji.

    - Pro každou tvář na obrázku
        - Načte se obrázek emoji.

            ```typescript
            let mojiPath = path.resolve(
                __dirname,
                "../shared/emojis/" + mojiName + ".png"
            );
            let emojiImage = await Jimp.read(mojiPath);
            ```

        - Změní se velikost obrázku emoji, aby odpovídala velikosti tváři na obrázku.

            ```typescript
            emojiImage.resize(faceWidth, faceHeight);
            ```

        - Vytvoří se složený obrázek z původního obrázku a emoji se změněnou velikostí a umístěného na tvář na obrázku.

            ```typescript
            compositeImage = compositeImage.composite(emojiImage, faceLeft, faceTop);
            ```

    - Vrátí se složený obrázek jako vyrovnávací paměť.

        ```typescript
        try {
            return await compositeImage.getBufferAsync(Jimp.MIME_JPEG);
        } catch (error) {
            context.log(`There was an error adding the emoji to the image: ${error}`);
            throw new Error(error);
        }
        ```

1. Nastavte návratovou odpověď protokolu HTTP ve funkci `index`.

    Přidejte následující kód do horní části typescriptové funkce `index(context, req)` jako konfiguraci odpovědi HTTP:

    ```typescript
    context.res = {
        status: 200,
        headers: {
          "Content-Type": "image/jpeg"
        },
        isRaw: true
    };
    ```

1. Přidejte volání funkce `createMojifiedImage` z funkce `index` nahrazením bloku `try` následujícím kódem:

    ```typescript
    try {
      let faces = await FaceApi.getFaces(context, imageUrl);
      if (faces) {
        let buffer = await createMojifiedImage(context, imageUrl, faces);
        context.res.body = buffer;
        context.log(`Posted reply with mojified image`);
      } else {
        context.res = { status: 400, body: `Could not process image: ${imageUrl}` };
      }
    } catch (err) {
      let message = `There was an error processing this image: ${err.message}`;
      context.log(message);
      context.res = { status: 400, body: message };
    }
    ```

## <a name="try-it-out"></a>Vyzkoušení

1. Ujistěte se, že je spuštěná místní aplikace funkcí Azure, pomocí jedné z metod, které jste dříve používali: buď použijte příkaz `func host start`, nebo ji spusťte z nabídky ladění pomocí úlohy **Attach to JavaScript Function** (Připojit k funkci v JavaScriptu).

    Pokud se aplikace funkcí spustila správně, mělo by okno výstupu zobrazovat podobný výstup:

    ```
    Http Functions:
            MojifyImage: http://localhost:7071/api/MojifyImage
    ```

1. Přejděte na adresu URL v prohlížeči. Nezapomeňte předat adresu URL obrázku prostřednictvím parametru dotazu `imageUrl`.

1. Měla by se vrátit „mojifikovaná“ verze obrázku.

    Právě jste napsali funkci Azure v jazyce TypeScript pro „mojifikaci“ obrázku! 👏👏👏

    ![„Mojifikovaný“ obrázek](../media/4.mojified-image.png)
