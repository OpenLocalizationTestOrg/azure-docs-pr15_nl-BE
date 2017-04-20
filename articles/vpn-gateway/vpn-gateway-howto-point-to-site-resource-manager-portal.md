<properties 
   pageTitle="Configureren van een Point-to-Site VPN-gateway verbinding met virtuele netwerk via het implementatiemodel Resource Manager en de Azure portal | Microsoft Azure"
   description="Veilig verbinding maken met het virtuele netwerk Azure door het maken van een Point-to-Site VPN-gateway verbinding met behulp van bronbeheer en de Azure portal."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Een punt-naar-Site verbinding configureren met een VNet met behulp van de Portal Azure

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Een punt-naar-Site (P2S)-configuratie kunt u een beveiligde verbinding maken vanaf een werkstation met een virtueel netwerk. Een verbinding P2S is handig wanneer u wilt verbinding maken met uw VNet vanaf een externe locatie, zoals thuis of een vergadering, of als u alleen een paar-clients die verbinding moeten maken met een virtueel netwerk. 

Punt-tot-Site verbindingen hoeven niet een VPN-apparaat of een publieke IP-adres om te werken. Een VPN-verbinding wordt vastgesteld door de verbinding van de clientcomputer wordt gestart. Zie voor meer informatie over verbindingen met Point-to-Site [VPN-Gateway Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) en [Planning en ontwerp](vpn-gateway-plan-design.md).

Dit artikel helpt u bij het maken van een VNet met een punt-naar-Site verbinding in het implementatiemodel voor Resource Manager met behulp van de portal Azure.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Van implementatiemodellen en methoden voor P2S verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In de volgende tabel ziet u de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Basiswerkstroom 

![Punt-naar-Site-diagram] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punt-tot-site")

### <a name="example"></a>Voorbeelden van waarden

- **Naam: VNet1**
- **Adresruimte: 192.168.0.0/16**<br>In dit voorbeeld gebruiken we slechts één adresruimte. U kunt meer dan één adresruimte hebben voor uw VNet.
- **Subnetnaam: FrontEnd**
- **Subnet-adresbereik: 192.168.1.0/24**
- **Abonnement:** Als u meer dan één abonnement hebt, controleert u of u gebruikt de juiste is.
- **Resourcegroep: TestRG**
- **Locatie: Oost-VS**
- **GatewaySubnet: 192.168.200.0/24**
- **Virtueel netwerk gateway naam: VNet1GW**
- **Gateway-type: VPN**
- **VPN-type: op basis van een Route**
- **Openbare IP-adres: VNet1GWpip**
- **Verbindingstype: punt-naar-site**
- **Client-adresgroep: 172.16.201.0/24**<br>VPN-clients die verbinding maken met de VNet via deze verbinding punt-naar-Site wordt een IP-adres uit de adresgroep van de client.

## <a name="before-beginning"></a>Voordat u begint

- Controleer of u een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>Deel 1: een virtueel netwerk maken

Als u deze configuratie als oefening maakt, kunt u verwijzen naar de [voorbeelden van waarden](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. Voeg extra adresruimte en subnetten

U kunt extra adresruimte en subnetten toevoegen aan uw VNet eenmaal is gemaakt.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. een gateway-subnet maken

Voordat u het virtuele netwerk verbinding te maken met een gateway, moet u eerst de gateway subnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. Indien mogelijk is het meest geschikt voor het maken van een gateway-subnet met een CIDR-blok van /28 of /27 om voldoende IP-adressen ten behoeve van extra configuratievereisten voor toekomstige.

De schermafbeeldingen in dit gedeelte worden geleverd als een voorbeeld van de verwijzing. Zorg ervoor dat het adresbereik GatewaySubnet die overeenkomt met de vereiste waarden voor uw configuratie gebruiken.

#### <a name="to-create-a-gateway-subnet"></a>Een gateway-subnet maken

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. Geef een DNS-server (optioneel)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Deel 2: een virtueel netwerkgateway maken

Punt-tot-site-verbindingen vereist de volgende instellingen:

- Gateway-type: VPN
- VPN-type: op basis van een Route

### <a name="to-create-a-virtual-network-gateway"></a>Voor het maken van een virtueel netwerkgateway

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Deel 3 - certificaten

Certificaten worden gebruikt door Azure voor de verificatie van VPN-clients voor Point-to-Site VPN-verbindingen. U exporteren een certificaat met openbare gegevens (niet de persoonlijke sleutel) als een Base64-gecodeerd x.509-cer-bestand van een basiscertificaat dat wordt gegenereerd door een bedrijfsoplossing certificaat of een zelf-ondertekend basiscertificaat. U het certificaat voor openbare gegevens importeren uit het basiscertificaat naar Azure. Bovendien moet u een clientcertificaat uit het basiscertificaat voor clients te genereren. Elke client die verbinding met het virtuele netwerk via een P2S wil moet beschikken over een clientcertificaat geïnstalleerd die is gegenereerd op basis van het basiscertificaat.

### <a name="getcer"></a>1. het cer-bestand voor het basiscertificaat ophalen

Als u een bedrijfsoplossing, kunt u uw bestaande certificaatketen. Als u een enterprise-oplossing CA niet gebruikt, kunt u een hoofdmap zelf-ondertekend certificaat maken. Een methode voor het maken van een zelfondertekend certificaat is makecert.

- Als u een certificaat bedrijfssysteem, krijgen het cer-bestand voor het basiscertificaat dat u wilt gebruiken. 

- Als u een bedrijfsoplossing voor het certificaat niet gebruikt, moet u een basis zelfondertekend certificaat te genereren. Voor Windows 10 stappen, kunt u verwijzen naar [werken met zelfondertekende basiscertificaten voor punt-naar-Site-configuraties](vpn-gateway-certificates-point-to-site.md).

1. Als u wilt een cer-bestand ophalen van een certificaat, **certmgr.msc** openen en zoek het basiscertificaat. Met de rechtermuisknop op het certificaat zelfondertekend root, klik op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik op **volgende**in de Wizard, selecteert u **Nee, de persoonlijke sleutel niet exporteren**en klik op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** selecteren **Base-64 encoded X.509 (. (CER).** Klik vervolgens op **volgende**. 

4. Op het **te exporteren bestand**, **Blader** naar de locatie waarnaar u het certificaat exporteren. Naam voor **de naam van**het certificaatbestand. Klik vervolgens op **volgende**.

5. Klik op **Voltooien** om het exporteren van het certificaat.

### <a name="generateclientcert"></a>2. een certificaat genereren

Kunt u een uniek certificaat genereren voor elke client die verbinding maakt of kunt u hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid om een enkel certificaat intrekken indien nodig. Anders als iedereen hetzelfde certificaat gebruikt en u vindt dat u wilt intrekken van het certificaat voor een bepaalde klant, moet u genereren en nieuwe certificaten installeren voor alle clients die gebruikmaken van dat certificaat worden geverifieerd.

- Als u een bedrijfsoplossing certificaat genereren een clientcertificaat met algemene waarde indeling van de 'name@yourdomain.com', in plaats van de indeling 'domeinnaam\gebruikersnaam'. 

- Als u een zelf-ondertekend certificaat gebruikt, Zie [werken met zelfondertekende basiscertificaten voor punt-naar-Site-configuraties](vpn-gateway-certificates-point-to-site.md) voor het genereren van een clientcertificaat.

### <a name="exportclientcert"></a>3. het clientcertificaat exporteren

Een clientcertificaat is vereist voor verificatie. Na het genereren van het clientcertificaat exporteren. Het clientcertificaat exporteren zal later worden geïnstalleerd op elke clientcomputer.

1. Als u wilt een certificaat exporteert, kunt u *certmgr.msc*. Met de rechtermuisknop op het certificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**.
2. Dit certificaat met de persoonlijke sleutel exporteren. Dit is een *PFX* -bestand. Zorg ervoor dat te nemen of het wachtwoord die u hebt ingesteld voor dit certificaat (key).

## <a name="addresspool"></a>Deel 4 - de client-adresgroep toevoegen

1. Zodra de gateway virtueel netwerk is gemaakt, gaat u naar de sectie **Instellingen** van het virtuele netwerk gateway blade. Klik op configuratie **punt-naar-site in de sectie **Instellingen voor** ** voor het openen van de bladeserver **configuratie** .

    ![Wijs site blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "Wijs site blade")

2. **Adresgroep** wordt de groep IP-adressen waarmee clients die verbinding maken met een IP-adres ontvangt. De adresgroep toevoegen en klik vervolgens op **Opslaan**.

    ![client-adresgroep] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "client-adresgroep")

## <a name="uploadfile"></a>Deel 5 - het root certificaat cer-bestand uploaden

Nadat u de gateway hebt gemaakt, kunt u het cer-bestand voor een vertrouwd basiscertificaat kunt uploaden naar Azure. U kunt de basiscertificaten van maximaal 20 bestanden uploaden. U uploaden de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Zodra het cer-bestand is geüpload, Azure worden gebruikt voor het verifiëren van clients die verbinding met het virtuele netwerk maken.

1. Ga naar de **punt-naar-site-configuratie** blade. .Cer-bestanden voegt u in de sectie van het **basiscertificaat** van deze blade.

    ![Wijs site blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "Wijs site blade")

2. Zorg ervoor dat u het basiscertificaat geëxporteerd als een Base64-gecodeerd x.509 (.cer)-bestand. U moet in deze indeling exporteren, zodat u kunt het certificaat met een teksteditor openen.
3. Open het certificaat met een teksteditor, zoals Kladblok. Kopieert alleen de volgende sectie:

    ![certificaatgegevens] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "certificaatgegevens")

4. Plak de certificaatgegevens in de sectie **Openbare gegevens van het certificaat** van de portal. Plaatst u de naam van het certificaat in **de naamruimte** en klik vervolgens op **Opslaan**. U kunt maximaal 20 vertrouwde basiscertificaten toevoegen.

    ![certificaat uploaden] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "certificaat uploaden")

## <a name="clientconfig"></a>Deel 6 - Download en installeer het pakket voor VPN-clients configureren

Clients die verbinding maken met Azure met P2S moeten een certificaat voor de client en een VPN-client configuratiepakket geïnstalleerd hebben. VPN-client configuratiepakketten zijn beschikbaar voor Windows-clients. 

De VPN-client-pakket bevat informatie voor het configureren van de VPN-client-software die is ingebouwd in Windows. De configuratie is specifiek voor de VPN-verbinding die u tot stand wilt brengen. Extra software het pakket niet geïnstalleerd. Raadpleeg de [Veelgestelde vragen over het VPN-Gateway](vpn-gateway-vpn-faq.md#point-to-site-connections) voor meer informatie.

1. In de configuratie **punt-naar-site** blade, klik op **Download VPN-client** als u wilt openen de blade **downloaden VPN-client** .

    ![VPN-client downloaden] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "VPN-client downloaden")

2. Selecteer het juiste pakket voor de client en klik vervolgens op **downloaden**. Selecteer **AMD64**voor 64-bits-clients. Voor 32-bits clients, selecteert u **x86**.

3. Het pakket installeren op de clientcomputer. Als u een pop-up SmartScreen, klik op **meer info**, dan **toch gewoon uitvoeren** om het pakket te installeren.

4. Ga naar de **Netwerkinstellingen** op de clientcomputer en op **VPN**. Ziet u de verbinding weergegeven. Het geeft de naam van het virtuele netwerk waarmee zij verbinding maken met en het ziet er vergelijkbaar met het volgende voorbeeld: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN-client")

## <a name="installclientcert"></a>Deel 7 - het clientcertificaat installeren

Elke clientcomputer moet beschikken over een clientcertificaat om te verifiëren. Wanneer u het certificaat installeert, moet u het wachtwoord dat is gemaakt nadat het certificaat is geëxporteerd.

1. Het .pfx-bestand naar de clientcomputer kopiëren.
2. Dubbelklik op het pfx-bestand om het te installeren. Locatie van de installatie niet wijzigen.

## <a name="connect"></a>Deel 8 - verbinding maken met Azure

1. Om verbinding met uw VNet, klikt u op de clientcomputer te navigeren naar de VPN-verbindingen en naar de VPN-verbinding die u hebt gemaakt. Dit is dezelfde naam als het virtuele netwerk genoemd. Klik op **verbinden**. Een pop-upbericht kan weergegeven dat verwijst naar het gebruik van het certificaat. Als dit gebeurt, klikt u op **Doorgaan** om meer bevoegdheden te gebruiken. 

2. Klik op de statuspagina van **verbinding** op **verbinden** om de verbinding te starten. Als u een **Certificaat selecteren** scherm ziet, controleert u of het clientcertificaat weer die u wilt gebruiken om verbinding te maken. Als dat niet het geval is, gebruikt u de pijl omlaag om het juiste certificaat selecteert en klik vervolgens op **OK**.

    ![VPN-client 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-clientverbinding")

3. Je moet nu verbinding maken.

    ![VPN-client 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN-clientverbinding 2")

## <a name="verify"></a>Deel 9 - de verbinding controleren

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

## <a name="add"></a>Toevoegen of verwijderen van vertrouwde basiscertificaten

U kunt vertrouwd basiscertificaat verwijderen uit Azure. Als u een vertrouwd certificaat verwijdert, is de certificaten die zijn gegenereerd op basis van het basiscertificaat niet langer verbinding maken met Azure via Point-to-Site. Als u wilt dat clients verbinding kunnen maken, moeten ze een nieuwe clientcertificaat installeren dat wordt gegenereerd op basis van een certificaat dat wordt vertrouwd in Azure.

U kunt de lijst met ingetrokken certificaten in de configuratie **punt-naar-site beheren** blade. Dit is het blad dat u voor het [uploaden van een vertrouwd basiscertificaat gebruikt](#uploadfile).

## <a name="revokeclient"></a>De lijst met ingetrokken certificaten beheren

U kunt certificaten intrekken. De lijst met ingetrokken certificaten kunt u selectief weigeren punt-naar-Site verbinding op basis van afzonderlijke clientcertificaten. Als u een cer root-certificaat uit Azure verwijderen, trekt deze u de toegang voor alle clientcertificaten gegenereerd/ondertekend door het basiscertificaat ingetrokken. Als u wilt intrekken, een bepaald clientcertificaat niet in de hoofdmap, kunt u dat doen. Op die manier de certificaten die zijn gegenereerd op basis van het basiscertificaat nog steeds wordt zijn geldig. 

De gangbare praktijk is met het basiscertificaat toegang op de niveaus team of organisatie, terwijl ingetrokken clientcertificaten voor fijnmazig toegangsbeheer voor afzonderlijke gebruikers beheren.

U kunt de lijst met ingetrokken certificaten in de configuratie **punt-naar-site beheren** blade. Dit is het blad dat u voor het [uploaden van een vertrouwd basiscertificaat gebruikt](#uploadfile).


## <a name="next-steps"></a>Volgende stappen

U kunt een virtuele machine toevoegen aan het virtuele netwerk. Zie [virtuele machines maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.