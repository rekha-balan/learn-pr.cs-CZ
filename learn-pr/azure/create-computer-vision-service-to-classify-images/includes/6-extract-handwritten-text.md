Viděli jsme, jak rozhraní API pro počítačové zpracování obrazu dokáže extrahovat tištěný text z obrázků. V tomto cvičení použijeme tuto službu k rozpoznání rukou psaného textu.

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a>Volání rozhraní API pro počítačové zpracování obrazu k extrakci rukou psaného textu

Operace `recognizeText` detekuje a extrahuje rukou psaný text z poznámek, dopisů, esejí, tabulí, formulářů a dalších zdrojů. Adresa URL požadavku má následující formát:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=<...>`

Všechna volání se musí provádět do oblasti, kde byl účet vytvořen.

Pokud je operace zadána, musí být parametr `mode` nastavený na `Handwritten` nebo `Printed`. Navíc se rozlišují velká a malá písmena. Pokud je parametr nastavený na `Handwritten` nebo není zadaný, provede se rozpoznávání textu psaného rukou. Pokud je parametr nastavený na `Printed`, provede se rozpoznávání tištěného textu. Doba potřebná k získání výsledků z tohoto volání závisí na rozsahu textu v obrázku.

V tomto příkladu:

- Vytiskneme hlavičky odpovědí do konzoly s použitím možnosti `-D` nástroje cUrl.
- Zkopírujeme hodnotu hlavičky `Operation-Location` z hlaviček, které dostaneme v odpovědi.
- Za několik sekund zkontrolujeme výsledky na adrese URL zadané prostřednictvím možnosti `Operation-Location`.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a>Rozpoznání a extrakce ručně psaného textu z obrázku

V tomto příkladu použijeme následující obrázek, ale stejný příkaz můžete vyzkoušet u adres URL jiných obrázků.

![Obrázek s ukázkou rukopisu na poznámce](../media/6-handwriting.jpg)

1. V Azure Cloud Shellu spusťte následující příkazy. Nahraďte v příkazu část `<region>` oblastí vašeho účtu služby Cognitive Services.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

Výše uvedené zadání vypíše do konzoly hlavičky této operace. Tady je příklad:

```azurecli
HTTP/1.1 202 Accepted
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Expires: -1
Operation-Location: https://westus2.api.cognitive.microsoft.com/vision/v2.0/textOperations/d0e9b397-4072-471c-ae61-7490bec8f077
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
apim-request-id: f5663487-03c6-4760-9be7-c9157fac10a1
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Date: Wed, 12 Sep 2018 19:22:00 GMT
```

Hlavička `Operation-Location` představuje místo, kam se výsledky po dokončení publikují.

2. Zkopírujte hodnotu hlavičky `Operation-Location`.
1. Spusťte v Azure Cloud Shellu následující příkaz, přičemž položku `"<Operation-Location>"` nahraďte hodnotou hlavičky **Operation-Location**, kterou jste zkopírovali v předchozím kroku.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

Až se operace dokončí, dostanete soubor JSON obsahující výsledek žádosti o rozpoznání rukopisu.

Další informace o operaci `recognizeText` najdete v referenční dokumentaci k [rozpoznávání rukou psaného textu](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200).