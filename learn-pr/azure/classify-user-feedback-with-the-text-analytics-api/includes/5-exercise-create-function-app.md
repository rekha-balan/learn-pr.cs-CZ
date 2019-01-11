 Aplikace funkcí poskytuje kontext pro správu a spouštění vašich funkcí. Nyní vytvoříme aplikaci funkcí a potom k ní přidáme funkce.

## <a name="create-a-function-app-to-host-our-function"></a>Vytvoření aplikace Function App k hostování funkce

1. Pomocí stejného účtu, kterým jste aktivovali sandbox, se přihlaste k webu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. V levém horním rohu webu Azure Portal vyberte tlačítko **Vytvořit prostředek** a pak vyberte **Výpočty** > **Serverless Function App**.

1. Zadejte nastavení aplikace funkcí uvedené v následující tabulce.

    | Nastavení      | Hodnota  | Popis                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název aplikace** | Globálně jedinečný název | Název identifikující novou aplikaci funkcí. Platné znaky jsou `a-z`, `0-9` a `-`.  |
    | **Předplatné** | **Předplatné Concierge** | Předplatné, pod kterým je tato nová aplikace funkcí vytvořena. |
    | **Skupina prostředků**|  **<rgn>[název sandboxové skupiny prostředků]</rgn>** | Název skupiny prostředků, ve které chcete aplikaci funkcí.<br/><br/>Vyberte možnost **Použít existující** a použijte skupinu prostředků z posledního cvičení. Tímto způsobem zůstanou všechny prostředky vytvořené v tomto modulu pohromadě. |
    | **OS** | Windows | Operační systém hostující aplikaci funkcí  |
    | **Plán hostování** |   Plán Consumption | Plán hostování, který určuje způsob přidělování prostředků aplikaci Function App. Ve výchozím **plánu Consumption** se prostředky přidávají dynamicky podle požadavků příslušných funkcí. U tohoto hostování [bez serveru](https://azure.microsoft.com/overview/serverless-computing/) platíte jenom za dobu, kdy jsou funkce spuštěné.   |
    | **Umístění** | Vyberte dříve použité umístění. | Vyberte oblast ve své blízkosti nebo v blízkosti jiných služeb, které vaše funkce využívají.<br/><br/>Vyberte stejnou oblast, kterou jste použili při vytváření účtu rozhraní API pro analýzu textu v posledním cvičení. |
    | **Zásobník modulu runtime** | JavaScript | Ukázkový kód v tomto modulu je napsaný v JavaScriptu.  |
    | **Storage** |  Globálně jedinečný název |  Název nového účtu úložiště, který bude aplikace funkcí používat. Názvy účtů úložiště musí mít délku 3 až 24 znaků a můžou obsahovat jenom číslice a malá písmena. Toto dialogové okno vyplní pole jedinečným názvem odvozeným od názvu aplikace. Můžete však použít i jiný název nebo dokonce existující účet. |

1. Klikněte na možnost **Vytvořit** a zřiďte a nasaďte aplikaci funkcí.

1. Vyberte ikonu oznámení v pravém horním rohu portálu a sledujte, kdy se objeví zpráva, že **nasazení probíhá**.

1. Nasazení může chvíli trvat. Zůstaňte v centru oznámení a čekejte na zprávu **Nasazení bylo úspěšné**.

1. Po nasazení aplikace funkcí přejděte na portálu na **Všechny prostředky**. Aplikace funkcí bude uvedena s typem **App Service** a s názvem, který jste jí dali. Vyberte aplikaci funkcí ze seznamu a tím ji otevřete.

    Blahopřejeme! Vytvořili jste a nasadili aplikaci funkcí.

> [!TIP]
> Máte potíže při hledání vaší aplikace funkcí na portálu? Zkuste [přidat aplikace funkcí do svých oblíbených položek na webu Azure Portal](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

## <a name="create-a-function-to-execute-our-logic"></a>Vytvoření funkce k provedení logiky

Když teď máme aplikaci funkcí, je načase vytvořit funkci. Funkce se aktivuje pomocí triggeru. V tomto modulu budeme používat Trigger fronty. Modul runtime se bude dotazovat fronty a tuto funkci spustí ke zpracování nové zprávy.

1. Klikněte na tlačítko Přidat (**+**) vedle **Funkce**. Tato akce spustí proces vytváření funkce.

1. Na stránce **Azure Functions pro JavaScript – Začínáme** vyberte **Na portálu** a potom **Pokračovat**.

1. V kroku **Vytvořit funkci** vyberte **Další šablony...** a pak vyberte **Dokončit a zobrazit šablony**.

1. V seznamu všech šablon dostupných pro tuto aplikaci funkcí vyberte **Trigger fronty** .

1. Pokud se zobrazí zpráva **Rozšíření nejsou nainstalovaná**, vyberte **Nainstalovat**. Instalace závislostí může trvat několik minut, proto buďte trpěliví. Než budete pokračovat, počkejte, až se instalace dokončí.

1. Do dialogového okna **Nová funkce**, které se zobrazí, zadejte následující hodnoty.

    |Vlastnost  |Hodnota  |
    |---------|---------|
    |Název     |   **discover-sentiment-function**      |
    |Název fronty     |   **new-feedback-q**      |
    |Připojení k účtu úložiště        |  **AzureWebJobsStorage**       |

1. Vyberte možnost **Vytvořit** a spusťte proces vytváření funkce.

1. Funkce se ve vybraném jazyce vytvoří pomocí šablony funkce Trigger fronty. Přestože v tomto modulu budeme funkci implementovat v JavaScriptu, vy si můžete vytvořit funkci v libovolném [podporovaném jazyce](https://docs.microsoft.com/azure/azure-functions/supported-languages).

Když se proces vytvoření dokončí, na webu Azure Portal se otevře editor kódu a načte stránku *index.js*. Do tohoto souboru s kódem budeme zapisovat logiku funkce.

## <a name="try-it-out"></a>Vyzkoušení

Pojďme otestovat, co zatím máme. Zatím jsme nenapsali žádný kód, takže budeme testovat, zda všechno, co jsme nakonfigurovali, běží.

1. V horní části editoru kódu klikněte na **Run** (Spustit).

1. Sledujete kartu **Logs** (Protokoly), která se otevře v dolní části obrazovky. Pokud všechno funguje podle plánu, uvidíte zprávu, která se bude podobat následující zprávě.
    ![Snímek obrazovky zprávy s odpovědí o úspěšném volání funkce](../media/func-default-run.PNG)

    Tlačítko **Run** (Spustit) spustilo funkci a předalo *ukázková data fronty* (výchozí text z okna požadavku **Test**) naší funkci.

Dobrá práce! Úspěšně jste do aplikace funkcí přidali funkci Trigger fronty a otestovali jste, že všechno funguje podle očekávání. V dalším cvičení přidáme této funkci další funkce.

Pojďme se krátce podívat na druhý soubor funkce – konfigurační soubor *function.json*. Konfigurační data z tohoto souboru jsou zobrazená v následujícím výpisu JSON.

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "new-feedback-q",
      "connection": "AzureWebJobsDashboard"
    }
  ],
  "disabled": false
}
```

Jak vidíte, tato funkce má vazbu na trigger typu `queueTrigger` s název **myQueueItem**. Když do fronty přijde nová zpráva, kterou jsme pojmenovali **new-feedback-q**, zavolá se naše funkce. Na novou zprávu odkazujeme pomocí parametru vazby myQueueItem. Vazby se za nás postarají o spoustu náročných operací.

V dalším kroku přidáme kód, který bude volat službu rozhraní API pro analýzu textu.

> [!TIP]
> Když na webu Azure Portal rozbalíte vpravo na panelu funkcí nabídku **Zobrazit soubory**, uvidíte soubory index.js a function.json.

Toto cvičení bylo celé o zařízení infrastruktury Azure Functions. Máme fungující funkci hostovanou v aplikaci funkcí, která se spustí, když do naší fronty přijde nová zpráva s názvem [!INCLUDE [input-q](./q-name-input.md)]. To nejzábavnější teprve přijde v dalším cvičení, ve kterém přidáme kód, který bude volat službu Azure Cognitive Services, aby provedla analýzu mínění.