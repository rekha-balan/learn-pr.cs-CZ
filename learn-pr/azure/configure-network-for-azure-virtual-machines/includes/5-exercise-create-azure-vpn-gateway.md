Klientům nebo webům ve svém prostředí chcete umožnit, aby se mohli připojit k Azure šifrovanými tunely ve veřejném internetu. V této lekci vytvoříte bránu VPN typu Point-to-Site a pak se k této bráně připojíte z klientského počítače. Z bezpečnostních důvodů použijete nativní připojení s ověřováním certifikátů Azure.

Budete postupovat následovně:

1. Vytvoříte bránu VPN typu RouteBased.

1. Nahrajete veřejný klíč kořenového certifikátu pro účely ověřování.

1. Vygenerujete z kořenového certifikátu klientský certifikát a pro účely ověřování pak tento klientský certifikát nainstalujete na všechny klientské počítače, které se k virtuální síti připojí.

1. Vytvoříte konfigurační soubory klienta VPN, které obsahují informace potřebné pro připojení klienta k virtuální síti.

## <a name="setup"></a>Nastavení

K dokončení tohoto modulu použijete Azure PowerShell na místním počítači s Windows 10.

Začněte tím, že nastavíte proměnné, které použijete k vytvoření virtuální sítě. Otevřete novou relaci PowerShellu a vytvořte následující proměnné:

```PowerShell
$VNetName  = "VNetData"
$FESubName = "FrontEnd"
$BESubName = "Backend"
$GWSubName = "GatewaySubnet"
$VNetPrefix1 = "192.168.0.0/16"
$VNetPrefix2 = "10.254.0.0/16"
$FESubPrefix = "192.168.1.0/24"
$BESubPrefix = "10.254.1.0/24"
$GWSubPrefix = "192.168.200.0/26"
$VPNClientAddressPool = "172.16.201.0/24"
$ResourceGroup = "VpnGatewayDemo"
$Location = "East US"
$GWName = "VNetDataGW"
$GWIPName = "VNetDataGWPIP"
$GWIPconfName = "gwipconf"
```

## <a name="configure-a-virtual-network"></a>Konfigurace virtuální sítě

1. Vytvořte skupinu prostředků.

    ```PowerShell
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
    ```

1. Vytvořte pro virtuální síť konfigurace podsítí. Ty mají názvy **FrontEnd, BackEnd** a **GatewaySubnet**. Všechny tyto podsítě existují v rámci předpony virtuální sítě.

    ```PowerShell
    $fesub = New-AzVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. Dále vytvořte virtuální síť. Použijte k tomu hodnoty podsítě a statický server DNS.

    ```PowerShell
    New-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroup -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. Teď zadejte proměnné právě vytvořené sítě.

    ```PowerShell
    $vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroup
    $subnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. Požádejte o dynamicky přidělovanou veřejnou IP adresu.

    ```PowerShell
    $pip = New-AzPublicIpAddress -Name $GWIPName -ResourceGroupName $ResourceGroup -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## <a name="create-the-vpn-gateway"></a>Vytvoření brány sítě VPN

Při vytváření této brány sítě VPN:

- GatewayType (Typ brány) musí být Vpn.
- VpnType (Typ sítě VPN) musí být RouteBased.

> [!NOTE]
> Upozorňujeme, že dokončení této části cvičení může trvat až 45 minut.

1. Pokud chcete vytvořit bránu VPN, spusťte následující příkaz a stiskněte Enter.

    ```PowerShell
    New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $ResourceGroup `
      -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
      -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. Počkejte na zobrazení výstupu příkazu.

## <a name="add-the-vpn-client-address-pool"></a>Přidání fondu adres klienta VPN

1. Spusťte následující příkaz a stiskněte Enter.

    ```PowerShell
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $ResourceGroup -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. Počkejte na zobrazení výstupu příkazu.

## <a name="generate-a-client-certificate"></a>Vygenerování klientského certifikátu

Pro síťovou infrastrukturu vytvořenou v Azure potřebujeme na místním počítači vytvořit certifikát podepsaný klientem. U většiny operačních systémů je postup stejný. Ukážeme si, jak vygenerovat klientský certifikát ve Windows 10. Použijeme PowerShell s modulem Azure PowerShell a nástrojem **Správce certifikátů**, který je ve Windows.

1. Nejprve vytvoříme kořenový certifikát podepsaný jeho držitelem. Spusťte následující příkaz:

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. Dále vygenerujte klientský certifikát podepsaný vaším novým kořenovým certifikátem.

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

Když máme vygenerované certifikáty, potřebujeme exportovat veřejný klíč našeho kořenového certifikátu.

1. Otevřete Správce certifikátů tím, že v PowerShellu spustíte `certmgr`.

1. Přejděte do složky **Personal** > **Certificates**. Najděte certifikát **P2SRootCert**, klikněte na něj pravým tlačítkem a vyberte ze seznamu **Všechny úkoly** > **Exportovat...**

1. V Průvodci exportem certifikátu klikněte na **Další**.

1. Ujistěte se, že je vybraná možnost **Ne, neexportovat privátní klíč**, a klikněte na **Další**.

1. Na stránce **Formát souboru pro export** ověřte, že je vybraná možnost **X.509, kódování Base-64 (CER)**, a klikněte na **Další**.

1. Na stránce **Soubor pro export** přejděte v poli **Název souboru** k umístění, které si zapamatujete, uložte soubor jako **P2SRootCert.cer** a klikněte na Další.

1. Na stránce **Dokončení Průvodce exportem certifikátu** klikněte na **Dokončit**.

1. V okně zprávy **Průvodce exportem certifikátu** klikněte na **OK**.

## <a name="upload-the-root-certificate-public-key-information"></a>Nahrání informací o veřejném klíči kořenového certifikátu

1. V okně PowerShellu spusťte následující příkaz, který deklaruje proměnnou pro název certifikátu:

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. Zástupný text `<cert-path>` nahraďte umístěním, kam budete kořenový certifikát exportovat, a spusťte následující příkaz:

    ```PowerShell
    $filePathForCert = "<cert-path>\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. Po nastavení názvu skupiny nahrajte certifikát do Azure následujícím příkazem.

    ```PowerShell
    Add-AzVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname $GWName -ResourceGroupName $ResourceGroup -PublicCertData $CertBase64
    ```

    Teď Azure pozná, že jde o důvěryhodný kořenový certifikát vaší virtuální sítě.

## <a name="configure-the-native-vpn-client"></a>Konfigurace nativního klienta VPN

1. Spuštěním následujícího příkazu vytvořte konfigurační soubory klienta VPN ve formátu ZIP.

    ```PowerShell
    $profile = New-AzVpnClientConfiguration -ResourceGroupName $ResourceGroup -Name $GWName -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. Zkopírujte adresu URL vrácenou ve výstupu tohoto příkazu a vložte ji do prohlížeče. V prohlížeči by se měl začít stahovat soubor .ZIP. Extrahujte obsah archivu a uložte ho na vhodné místo.

    > [!NOTE]
    > Některé prohlížeče se napřed pokusí zablokovat stažení souboru ZIP, protože může být nebezpečné. Pokus o zablokování musíte v prohlížeči potlačit, abyste mohli extrahovat obsah archivu.

1. V extrahované složce přejděte do složky **WindowsAmd64** (u 64bitových počítačů s Windows) nebo do složky **WindowsX86** (u 32bitových počítačů).

    > [!NOTE]
    > Pokud chcete síť VPN nakonfigurovat na počítači s jiným systémem než Windows, použijte certifikát a soubory pro nastavení ze složky **Generic**.

1. Poklikejte na soubor **VpnClientSetup{architecture}.exe**, kde parametr `{architecture}` odpovídá vaší architektuře.

1. Na obrazovce **Systém Windows ochránil váš počítač** klikněte na **Další informace** a pak klikněte na **Přesto spustit**.

1. V dialogovém okně **Řízení uživatelských účtů** klikněte na **Ano**.

1. V dialogovém okně **VNetData** klikněte na **Ano**.

## <a name="connect-to-azure"></a>Připojení k Azure

1. Stiskněte klávesu Windows, zadejte **Nastavení** a stiskněte Enter.

1. V okně **Nastavení** klikněte na **Síť a internet**.

1. V levém podokně klikněte na **VPN**.

1. V pravém podokně klikněte na **VNetData** a pak klikněte na **Připojit**.

1. V okně VNetData klikněte na **Připojit**.

1. V dalším okně VNetData klikněte na **Pokračovat**.

1. V okně zprávy **Řízení uživatelských účtů** klikněte na **Ano**.

> [!NOTE]
> Pokud tento postup nefunguje, možná bude potřeba restartovat počítač.

## <a name="verify-your-connection"></a>Ověření připojení

1. Na novém příkazovém řádku ve Windows spusťte příkaz `IPCONFIG /ALL`.

1. Zkopírujte IP adresu v části PPP adapter VNetData (Adaptér protokolu PPP VNetData) nebo si ji zapište.

1. Zkontrolujte, že se tato IP adresa nachází v **rozsahu VPNClientAddressPool 172.16.201.0/24**.

1. Úspěšně jste se připojili k bráně Azure VPN Gateway.

Právě jste nastavili bránu VPN, která klientovi umožňuje používat šifrované připojení k virtuální síti v Azure. Tento přístup je ideální pro klientské počítače a menší připojení typu Site-to-Site.