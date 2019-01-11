Azure IoT Central je plně spravované řešení IoT (Internet of Things), které usnadňuje propojení, monitorování a správu vašich globálních prostředků IoT.

Zde použijete scénář, ve kterém je vzdálený kávovar připojený k Azure IoT Central za účelem monitorování a správy problémů. Můžete monitorovat telemetrická data, jako je teplota vody a vlhkost, sledovat stav zařízení, nastavit optimální teplotu, zobrazit stav záruky a odesílat příkazy. V případě, že již záruka skončila a teploty vody je mimo očekávaný rozsah, odešle IoT Central e-mail oddělení údržby klienta, které pak bude problém dále řešit.

Začnete vytvořením zařízení v Azure IoT Central definujícím data a příkazy, které lze se zařízením IoT vyměňovat.

V tomto modulu se naučíte:
  - Vytvořit vlastní aplikaci Azure IoT Central
  - Vytvořit a definovat šablonu zařízení
  - Připojit simulátor kávovaru k vaší aplikaci v Azure IoT Central
  - Ověřit připojení a tok dat
  - Konfigurovat pravidla pro oznámení o údržbě
 
## <a name="sign-in-to-azure-iot-central"></a>Přihlášení k Azure IoT Central
V této lekci se přihlásíte k IoT Central, abyste vytvořili novou vlastní aplikaci. K dokončení tohoto modulu je dostačující 7denní zkušební verze. 

1. Přejděte na stránku [správce aplikací](https://aka.ms/iotcentral?azure-portal=true) Azure IoT Central. 

1. Na přihlašovací stránce zadejte e-mailovou adresu a heslo, které používáte pro přístup ke svému účtu Microsoft.

## <a name="create-a-new-custom-application"></a>Vytvoření nové vlastní aplikace

1. Pokud chcete vytvořit novou aplikaci Azure IoT Central, zvolte **New Application** (Nová aplikace). 

1. Na stránce **Create Application** (Vytvořit aplikaci): 
    * Jako platební plán zvolte plán **Free**.
    * Jako šablonu aplikace zvolte **Custom Application** (Vlastní aplikace).
    * Zvolte popisný název aplikace, například **Kávovar 01-A**.
    * (Volitelně) Upravte adresu URL – to se vyžaduje, pokud se název, který jste vybrali, už používá
    * Zvolte **Vytvořit**.
