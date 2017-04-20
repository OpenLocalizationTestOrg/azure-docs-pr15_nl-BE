<properties 
   pageTitle="Een Point-to-Site VPN-gateway verbinding met virtuele netwerk met behulp van het implementatiemodel Resource Manager configureren | Microsoft Azure"
   description="Veilig verbinding maken met het virtuele netwerk Azure door een Point-to-Site VPN-gateway verbinding te maken."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Een punt-naar-Site verbinding configureren met een VNet met PowerShell

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Een punt-naar-Site (P2S)-configuratie kunt u een beveiligde verbinding maken vanaf een werkstation met een virtueel netwerk. Een verbinding P2S is handig wanneer u wilt verbinding maken met uw VNet vanaf een externe locatie, zoals thuis of een vergadering, of als u alleen een paar-clients die verbinding moeten maken met een virtueel netwerk. 

Punt-tot-Site verbindingen hoeven niet een VPN-apparaat of een publieke IP-adres om te werken. Een VPN-verbinding wordt vastgesteld door de verbinding van de clientcomputer wordt gestart. Zie voor meer informatie over verbindingen met Point-to-Site [VPN-Gateway Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) en [Planning en ontwerp](vpn-gateway-plan-design.md). 

Dit artikel helpt u bij het maken van een VNet met een punt-naar-Site verbinding in de Resource Manager implementatiemodel met PowerShell.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Van implementatiemodellen en methoden voor P2S verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In de volgende tabel ziet u de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Basiswerkstroom 

![Punt-naar-Site-diagram] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punt-tot-site")

In dit scenario maakt u een virtueel netwerk met een punt-naar-Site verbinding. De instructies kunt u ook certificaten die nodig voor deze configuratie zijn. Een P2S verbinding bestaat uit de volgende onderdelen: een VNet met een VPN-gateway, een root certificaat cer-bestand (openbare sleutel), een certificaat voor de client en de VPN-configuratie op de client. 

We gebruiken de volgende waarden voor deze configuratie. We stellen de variabelen in sectie [1](#declare) van het artikel. U kunt de stappen gebruiken als een overzicht en gebruik de waarden niet wijzigen of wijzigen zodat ze overeenkomen met uw omgeving. 

### <a name="example"></a>Voorbeelden van waarden

- **Naam: VNet1**
- **Adresruimte: 192.168.0.0/16** en **10.254.0.0/16**<br>In dit voorbeeld gebruiken we meer dan één adresruimte om te illustreren dat deze configuratie met meerdere adresruimten werkt. Meerdere adresruimten zijn echter niet vereist voor deze configuratie.
- **Subnetnaam: FrontEnd**
    - **Subnet-adresbereik: 192.168.1.0/24**
- **Subnetnaam: back-end**
    - **Subnet-adresbereik: 10.254.1.0/24**
- **Subnetnaam: GatewaySubnet**<br>De naam van het Subnet *GatewaySubnet* is verplicht voor de VPN-gateway om te werken.
    - **Subnet-adresbereik: 192.168.200.0/24** 
- **VPN-client-adresgroep: 172.16.201.0/24**<br>VPN-clients die verbinding maken met de VNet via deze verbinding punt-naar-Site wordt een IP-adres uit de adresgroep van de VPN-client.
- **Abonnement:** Als u meer dan één abonnement hebt, controleert u of u gebruikt de juiste is.
- **Resourcegroep: TestRG**
- **Locatie: Oost-VS**
- **DNS-Server: IP-adres** van de DNS-server die u wilt gebruiken voor naamomzetting.
- **GW-naam: Vnet1GW**
- **Openbare IP-naam: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Voordat u begint

- Controleer of u een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
    
- Installeer de nieuwste versie van de Azure Resource Manager PowerShell-cmdlets. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets. Als u werkt met PowerShell voor deze configuratie, zorg dat u als beheerder uitvoert. 

## <a name="declare"></a>Deel 1 - Log in en variabelen instellen

In deze sectie, log in en declareert u de waarden voor deze configuratie. De opgegeven waarden worden gebruikt in de voorbeeldscripts. Wijzig de waarden aan uw eigen omgeving. Of u kunt de gedeclareerde waarden en doorloop de stappen als een oefening.

1. In de console PowerShell aanmelden bij uw account Azure. Deze cmdlet wordt u gevraagd de aanmeldingsreferenties voor de Account Azure. Na het aanmelden, wordt het gedownload uw accountinstellingen zodat ze beschikbaar voor Azure PowerShell zijn.

        Login-AzureRmAccount 

2. Een lijst van uw abonnementen Azure ophalen.

        Get-AzureRmSubscription

3. Geef het abonnement dat u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Declareer de variabelen die u wilt gebruiken. Gebruik het volgende voorbeeld, vervangen door de waarden voor uw eigen wanneer dat nodig is.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Deel 2 - een VNet configureren 

1. Een resourcegroep maken.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. De subnet-configuraties voor het virtuele netwerk, ze benoemen *front-end*en *back-end* *GatewaySubnet*maken. Deze voorvoegsels moeten deel uitmaken van de VNet-adresruimte die u hebt gedeclareerd.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Het virtuele netwerk maken. De opgegeven DNS-server moet een DNS-server die de namen van de resources die u aansluit op kunt oplossen. In dit voorbeeld hebben we een openbaar IP-adres gebruikt. Zorg ervoor dat uw eigen waarden.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Geef de variabelen voor het virtuele netwerk dat u hebt gemaakt.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Een dynamisch toegewezen, openbaar IP-adres aanvragen. Dit IP-adres is nodig voor de gateway goed te laten werken. U wordt later de gateway voor het gateway-IP-configuratie.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Deel 3 - certificaten

Certificaten worden gebruikt door Azure voor de verificatie van VPN-clients voor Point-to-Site VPN-verbindingen. U exporteren een certificaat met openbare gegevens (niet de persoonlijke sleutel) als een Base64-gecodeerd x.509-cer-bestand van een basiscertificaat dat wordt gegenereerd door een bedrijfsoplossing certificaat of een zelf-ondertekend basiscertificaat. U het certificaat voor openbare gegevens importeren uit het basiscertificaat naar Azure. Bovendien moet u een clientcertificaat uit het basiscertificaat voor clients te genereren. Elke client die verbinding met het virtuele netwerk via een P2S wil moet beschikken over een clientcertificaat geïnstalleerd die is gegenereerd op basis van het basiscertificaat.

### <a name="cer"></a>1. het cer-bestand voor het basiscertificaat ophalen

U moet het certificaat voor openbare gegevens ophalen voor het basiscertificaat dat u wilt gebruiken.

- Als u een certificaat bedrijfssysteem, krijgen het cer-bestand voor het basiscertificaat dat u wilt gebruiken. 

- Als u een bedrijfsoplossing voor het certificaat niet gebruikt, moet u een basis zelfondertekend certificaat te genereren. Voor Windows 10 stappen, kunt u verwijzen naar [werken met zelfondertekende basiscertificaten voor punt-naar-Site-configuraties](vpn-gateway-certificates-point-to-site.md).


1. Als u wilt een cer-bestand ophalen van een certificaat, **certmgr.msc** openen en zoek het basiscertificaat. Met de rechtermuisknop op het certificaat zelfondertekend root, klik op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik op **volgende**in de Wizard, selecteert u **Nee, de persoonlijke sleutel niet exporteren**en klik op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** selecteren **Base-64 encoded X.509 (. (CER).** Klik vervolgens op **volgende**. 

4. Op het **te exporteren bestand**, **Blader** naar de locatie waarnaar u het certificaat exporteren. Naam voor **de naam van**het certificaatbestand. Klik vervolgens op **volgende**.

5. Klik op **Voltooien** om het exporteren van het certificaat.

### <a name="generate"></a>2. het clientcertificaat genereren

Vervolgens genereert de clientcertificaten. Kunt u een uniek certificaat genereren voor elke client die verbinding maakt of kunt u hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid om een enkel certificaat intrekken indien nodig. Anders als iedereen hetzelfde certificaat gebruikt en u vindt dat u wilt intrekken van het certificaat voor een bepaalde klant, moet u genereren en nieuwe certificaten installeren voor alle clients die gebruikmaken van het certificaat worden geverifieerd. Clientcertificaten worden geïnstalleerd op elke clientcomputer die verderop in deze oefening.

- Als u een bedrijfsoplossing certificaat genereren een clientcertificaat met algemene waarde indeling van de 'name@yourdomain.com', in plaats van 'Domein\gebruikersnaam' van de NetBIOS-notatie. 

- Als u een zelf-ondertekend certificaat gebruikt, Zie [werken met zelfondertekende basiscertificaten voor punt-naar-Site-configuraties](vpn-gateway-certificates-point-to-site.md) voor het genereren van een clientcertificaat.

### <a name="exportclientcert"></a>3. het clientcertificaat exporteren

Een clientcertificaat is vereist voor verificatie. Na het genereren van het clientcertificaat exporteren. Het clientcertificaat exporteren zal later worden geïnstalleerd op elke clientcomputer.

1. Als u wilt een certificaat exporteert, kunt u *certmgr.msc*. Met de rechtermuisknop op het certificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**.
2. Dit certificaat met de persoonlijke sleutel exporteren. Dit is een *PFX* -bestand. Zorg ervoor dat te nemen of het wachtwoord die u hebt ingesteld voor dit certificaat (key).

### <a name="upload"></a>4. het root certificaat cer-bestand uploaden

De variabele voor de certificaatnaam van uw, de waarde vervangen door uw eigen declareren:

        $P2SRootCertName = "Mycertificatename.cer"

Een certificaat met openbare gegevens voor het basiscertificaat toevoegen aan Azure. U kunt de basiscertificaten van maximaal 20 bestanden uploaden. U uploaden de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Zodra het cer-bestand is geüpload, Azure worden gebruikt voor het verifiëren van clients die verbinding met het virtuele netwerk maken. 

Het pad naar het bestand te vervangen door uw eigen en voert u de cmdlets.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Deel 4: de VPN-gateway maken

Configureren en de gateway virtueel netwerk maken voor uw VNet. De *-GatewayType* moet **Vpn** en de *VpnType -* **RouteBased**moet worden. Dit kan maximaal 45 minuten duren.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Deel 5 – de VPN-client configuration downloaden

Clients die verbinding maken met Azure met P2S moeten hebben zowel een certificaat als een VPN-client configuratiepakket geïnstalleerd. VPN-client configuratiepakketten zijn beschikbaar voor Windows-clients. Het pakket voor VPN-clients bevat informatie voor het configureren van de VPN-client-software die is ingebouwd in Windows en is specifiek voor de VPN waarmee u verbinding wilt maken. Extra software het pakket niet geïnstalleerd. Raadpleeg de [Veelgestelde vragen over het VPN-Gateway](vpn-gateway-vpn-faq.md#point-to-site-connections) voor meer informatie.

1. Nadat u de gateway hebt gemaakt, kunt u het configuratiepakket client downloaden. In dit voorbeeld downloadt het pakket voor de 64-bits clients. Als u downloaden van de 32-bits client wilt, vervangen door "Amd64" 'x86'. U kunt ook downloaden van de VPN-client met behulp van de portal Azure.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. De PowerShell-cmdlet retourneert een URL-koppeling. Dit is een voorbeeld van wat de geretourneerde URL ziet er als volgt:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Kopieer en plak de koppeling die wordt geretourneerd aan een webbrowser te downloaden van het pakket. Vervolgens installeert u het pakket op de clientcomputer. Als u een pop-up SmartScreen, klik op **meer info**, dan **toch gewoon uitvoeren** om het pakket te installeren.

4. Ga naar de **Netwerkinstellingen** op de clientcomputer en op **VPN**. Ziet u de verbinding weergegeven. Het geeft de naam van het virtuele netwerk waarmee zij verbinding maken met en het ziet er vergelijkbaar met het volgende voorbeeld: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN-client")


## <a name="clientcertificate"></a>Deel 6 - het clientcertificaat installeren

Elke clientcomputer moet beschikken over een clientcertificaat om te verifiëren. Wanneer u het certificaat installeert, moet u het wachtwoord dat is gemaakt nadat het certificaat is geëxporteerd.

1. Het .pfx-bestand naar de clientcomputer kopiëren.
2. Dubbelklik op het pfx-bestand om het te installeren. Locatie van de installatie niet wijzigen.

## <a name="connect"></a>Deel 7 - verbinding maken met Azure

1. Om verbinding met uw VNet, klikt u op de clientcomputer te navigeren naar de VPN-verbindingen en naar de VPN-verbinding die u hebt gemaakt. Dit is dezelfde naam als het virtuele netwerk genoemd. Klik op **verbinden**. Een pop-upbericht kan weergegeven dat verwijst naar het gebruik van het certificaat. Als dit gebeurt, klikt u op **Doorgaan** om meer bevoegdheden te gebruiken. 

2. Klik op de statuspagina van **verbinding** op **verbinden** om de verbinding te starten. Als u een **Certificaat selecteren** scherm ziet, controleert u of het clientcertificaat weer die u wilt gebruiken om verbinding te maken. Als dat niet het geval is, gebruikt u de pijl omlaag om het juiste certificaat selecteert en klik vervolgens op **OK**.

    ![VPN-clientverbinding] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-clientverbinding")

3. Je moet nu verbinding maken.

    ![Verbinding maken] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Verbinding maken")

## <a name="verify"></a>Deel 8 – Controleer uw verbinding

1. Om te controleren dat uw VPN-verbinding actief is, opent u een opdrachtprompt en voer *ipconfig/all*.

2. De resultaten bekijken. U ziet het IP-adres dat u hebt ontvangen is een van de adressen binnen de adresgroep van Point-to-Site VPN-Client die u hebt opgegeven in de configuratie. De resultaten moet iets ongeveer als volgt uit:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="addremovecert"></a>Toevoegen of verwijderen van een vertrouwd basiscertificaat

Certificaten worden gebruikt voor de verificatie van VPN-clients voor Point-to-Site VPN-verbindingen. De volgende stappen leest u toevoegen en verwijderen van basiscertificaten. Wanneer u een bestand met Base64 gecodeerde X.509 (.cer) naar Azure toevoegt, geeft u aan dat Azure vertrouwt het basiscertificaat dat het bestand vertegenwoordigt. 

U kunt toevoegen of verwijderen van vertrouwde basiscertificaten met PowerShell of in de portal Azure. Als u dit doen met de Azure portal wilt, gaat u naar de **virtueel netwerkgateway > Instellingen > configuratie punt-naar-site > Root certificaten**. De volgende stappen kunt u doorlopen deze taken met PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat toevoegen

U kunt maximaal 20 vertrouwde certificaten cer-bestanden toevoegen aan Azure. Volg onderstaande stappen voor het toevoegen van een basiscertificaat.

1. Maken en voorbereiden van de nieuwe basiscertificaat dat u aan de Azure toevoegen wilt. De openbare sleutel exporteren als een Base64-gecodeerd X.509 (. (CER) en met een teksteditor openen. Kopieer alleen de onderstaande sectie. 
 
    Kopieer de waarden, zoals in het volgende voorbeeld wordt getoond:

    ![certificaat] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certificaat")
    
2. Geef de naam van het certificaat en belangrijke informatie als een variabele. De informatie vervangen door uw eigen, zoals in het volgende voorbeeld:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Het nieuwe hoofdcertificaat toevoegen. U kunt slechts één certificaat tegelijk toevoegen.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. U kunt controleren of het nieuwe certificaat op de juiste wijze is toegevoegd met behulp van de volgende cmdlet.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Een vertrouwd basiscertificaat verwijderen

U kunt vertrouwd basiscertificaat verwijderen uit Azure. Als u een vertrouwd certificaat verwijdert, is de certificaten die zijn gegenereerd op basis van het basiscertificaat niet langer verbinding maken met Azure via Point-to-Site. Als u wilt dat clients verbinding kunnen maken, moeten ze een nieuwe clientcertificaat installeren dat wordt gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.

1. Als u een vertrouwd basiscertificaat, wilt wijzigen in het volgende voorbeeld:

    De variabelen declareren.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Het certificaat verwijderen.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. De volgende cmdlet gebruikt om te controleren of het certificaat is verwijderd. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>De lijst met ingetrokken certificaten beheren

U kunt certificaten intrekken. De lijst met ingetrokken certificaten kunt u selectief weigeren punt-naar-Site verbinding op basis van afzonderlijke clientcertificaten. Als u een cer root-certificaat uit Azure verwijderen, trekt deze u de toegang voor alle clientcertificaten gegenereerd/ondertekend door het basiscertificaat ingetrokken. Als u wilt intrekken, een bepaald clientcertificaat niet in de hoofdmap, kunt u dat doen. Op die manier de certificaten die zijn gegenereerd op basis van het basiscertificaat nog steeds wordt zijn geldig.

De gangbare praktijk is met het basiscertificaat toegang op de niveaus team of organisatie, terwijl ingetrokken clientcertificaten voor fijnmazig toegangsbeheer voor afzonderlijke gebruikers beheren.

### <a name="revoke-a-client-certificate"></a>Een certificaat intrekken

1. Krijg de vingerafdruk van het clientcertificaat in te trekken.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. De vingerafdruk toevoegen aan de lijst met ingetrokken vingerafdruk.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Controleren of de vingerafdruk is toegevoegd aan de lijst met ingetrokken certificaten. U moet één vingerafdruk per keer toevoegen.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Een clientcertificaat herstellen

U kunt een clientcertificaat herstellen door de vingerafdruk te verwijderen uit de lijst met ingetrokken certificaten.

1.  De vingerafdruk verwijderen uit de lijst met ingetrokken client certificaatvingerafdruk.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Controleer als de vingerafdruk is verwijderd uit de lijst met ingetrokken.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Volgende stappen

U kunt een virtuele machine toevoegen aan het virtuele netwerk. Zie [virtuele machines maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.