Mobilní aplikace je spuštěná a vytvořila se prvotní verze funkce Azure. V této lekci z mobilní aplikace zavoláte funkci Azure a předáte jí polohu uživatele a seznam telefonních čísel, na která chce uživatel odeslat zprávy SMS.

## <a name="calling-the-azure-function-from-the-mobile-app"></a>Volání funkce Azure z mobilní aplikace

1. Otevřete třídu `MainViewModel`.

1. Do této třídy přidejte privátní pole `HttpClient` s názvem `client`. Bude potřeba přidat odkaz na obor názvů `System.Net.Http`.

    ```cs
    HttpClient client = new HttpClient();
    ```

1. Přidejte konstantní pole pro základní adresu URL funkce. Nastavte tuto hodnotu na adresu, na které naslouchá místní modul runtime Azure Functions. Po nasazení funkce do Azure můžete tuto konstantu změnit na adresu URL Azure.

    ```cs
    const string baseUrl = "http://localhost:7071";
    ```

1. V metodě `SendLocation` po zjištění polohy vytvořte novou instanci `PostData` s použitím polohy a seznamu telefonních čísel zadaných uživatelem. Bude potřeba přidat direktivu Using pro obor názvů `ImHere.Data`.

    ```cs
    PostData postData = new PostData
    {
        Latitude = location.Latitude,
        Longitude = location.Longitude,
        ToNumbers = PhoneNumbers.Split('\n')
    };
    ```

    > [!NOTE]
    > Předpokladem je, že jsou telefonní čísla zadaná ve správném formátu, tedy na samostatných řádcích v ovládacím prvku `Editor`. V aplikaci určené do produkčního prostředí by se ověřovalo, že se zadalo alespoň jedno telefonní číslo ve správném formátu.    
 

1. Nejjednodušší způsob, jak serializovat `PostData` do formátu JSON, je použít balíček NuGet Newtonsoft.json. Přidejte tento balíček NuGet do projektu `ImHere` stejným způsobem jako Xamarin.Essentials v předchozí lekci.

1. Pomocí statické třídy `JsonConvert` serializujte `PostData` do řetězce `string`. Bude potřeba přidat direktivu Using pro obor názvů `Newtonsoft.Json`. Zakódujte tento řetězec do třídy `StringContent`, aby bylo možné ho předat do funkce Azure ve formátu JSON.

    ```cs
    string data = JsonConvert.SerializeObject(postData);
    StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
    ```

1. Odešlete tato data do funkce a získejte zpět výsledek.

   ```cs
    HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                        content);
   ```

   K funkcím Azure se přistupuje přes adresu `/api/<function name>`, takže za předpokladu, že místní modul runtime Functions zvolil port 7071, bude funkce `SendLocation` přístupná na adrese `http://localhost:7071/api/SendLocation`.

1. V závislosti na výsledku zobrazte v uživatelském rozhraní zprávu.

    ```cs
    if (result.IsSuccessStatusCode)
        Message = "Location sent successfully";
    else
        Message = $"Error - {result.ReasonPhrase}";
    ```

Níže vidíte celý kód nových polí a metody `SendLocation`.

```cs
HttpClient client = new HttpClient();
const string baseUrl = "http://localhost:7071";

async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";

        PostData postData = new PostData
        {
            Latitude = location.Latitude,
            Longitude = location.Longitude,
            ToNumbers = PhoneNumbers.Split('\n')
        };

        string data = JsonConvert.SerializeObject(postData);
        StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
        HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                            content);

        if (result.IsSuccessStatusCode)
            Message = "Location sent successfully";
        else
            Message = $"Error - {result.ReasonPhrase}";
    }
}
```

## <a name="testing-it-out"></a>Otestování

1. Ujistěte se, že je funkce Azure stále místně spuštěná a že port odpovídá metodě `SendLocation`.

1. Nastavte aplikaci pro UPW jako spouštěnou aplikaci a spusťte ji. Klikněte na tlačítko **Send Location** (Odeslat umístění). V okně konzoly modulu runtime Functions se zobrazí výstup s volanou funkcí a protokolováním ukazujícím vygenerovanou adresu URL.

    ![Výstup volané funkce](../media/6-function-called.png)

1. Pokud chcete otestovat generování adresy URL, vložte ji z konzoly do prohlížeče. Měla by se zobrazit vaše aktuální poloha.

    > [!TIP]
    > Umístění, které uvidíte, je umístění, ve kterém je aplikace spuštěná, takže bude v blízkosti datového centra, ze kterého běží virtuální počítač. Pokud byla tato aplikace spuštěna na místním zařízení, mělo by se zobrazit vaše umístění.

## <a name="summary"></a>Shrnutí

V této lekci jste zjistili, jak z mobilní aplikace zavolat funkci Azure. Tímto voláním se předala poloha uživatele a telefonní čísla zadaná uživatelem ve formátu JSON. V další lekci vytvoříte vazbu mezi funkcí Azure a platformou Twilio, abyste tuto polohu mohli odeslat ve zprávě SMS.