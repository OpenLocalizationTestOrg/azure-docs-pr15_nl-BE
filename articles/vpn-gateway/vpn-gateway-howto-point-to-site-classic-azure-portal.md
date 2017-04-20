<properties
   pageTitle="Configureren van een Point-to-Site VPN-gateway verbinding met een virtueel netwerk met behulp van de portal Azure van Azure | Microsoft Azure"
   description="Veilig verbinding maken met het virtuele netwerk Azure door het maken van een Point-to-Site VPN-gateway verbinding met de Azure portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Een punt-naar-Site verbinding configureren met een VNet met de Azure portal

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Dit artikel helpt u bij het maken van een VNet met een punt-naar-Site verbinding in de klassieke implementatiemodel met behulp van de portal Azure. Een punt-naar-Site (P2S)-configuratie kunt u een beveiligde verbinding maken vanaf een werkstation met een virtueel netwerk. Een verbinding P2S is handig wanneer u verbinding maken met uw VNet vanaf een externe locatie wilt, zoals thuis of een vergadering. Of, als u alleen een paar-clients die verbinding moeten maken met een virtueel netwerk.

Punt-tot-Site verbindingen hoeven niet een VPN-apparaat of een publieke IP-adres om te werken. Een VPN-verbinding wordt vastgesteld door de verbinding van de clientcomputer wordt gestart. Zie de [Veelgestelde vragen over het VPN-Gateway](vpn-gateway-vpn-faq.md#point-to-site-connections) en [Over VPN-Gateway](vpn-gateway-about-vpngateways.md#point-to-site)voor meer informatie over verbindingen met punt-naar-Site.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Van implementatiemodellen en methoden voor P2S verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In de volgende tabel ziet u de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Basiswerkstroom 

![Punt-naar-Site-diagram] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punt-tot-site")


In de volgende secties kunnen u door de stappen voor het maken van een beveiligde verbinding punt-naar-Site met een virtueel netwerk. 

1. Een virtueel netwerk en een VPN-gateway maken
2. Certificaten genereren
3. Het cer-bestand uploaden
4. De VPN-client configuratiepakket genereren
5. De clientcomputer configureren
6. Verbinding maken met Azure

### <a name="example-settings"></a>Van de voorbeeldinstellingen

U kunt de voorbeeldinstellingen van het volgende:

- **Naam: VNet1**
- **Adresruimte: 192.168.0.0/16**
- **Subnetnaam: FrontEnd**
- **Subnet-adresbereik: 192.168.1.0/24**
- **Abonnement:** Als u meer dan één abonnement hebt, controleert u of u gebruikt de juiste is.
- **Resourcegroep: TestRG**
- **Locatie: Oost-VS**
- **Verbindingstype: punt-naar-site**
- **Client-adresruimte: 172.16.201.0/24**. VPN-clients die verbinding maken met de VNet via deze verbinding punt-naar-Site ontvangt een IP-adres van de opgegeven groep.
- **GatewaySubnet: 192.168.200.0/24**. Het Gateway-subnet moet de naam 'GatewaySubnet' gebruiken.
- **Formaat:** Selecteer de gateway SKU die u wilt gebruiken.
- **Routering Type: dynamisch**

## <a name="vnetvpn"></a>Sectie 1 - een virtueel netwerk en een VPN-gateway maken

### <a name="createvnet"></a>Deel 1: Een virtueel netwerk maken

Als u nog niet een virtueel netwerk, maken. Screenshots zijn beschikbaar als voorbeelden. Zorg ervoor dat de waarden vervangen door uw eigen. Een VNet maken met behulp van de portal Azure, gebruik de volgende stappen uit: 

1. Vanuit een browser, Ga naar de [Azure portal](http://portal.azure.com) en meld u zonodig met uw Azure-account.

2. Klik op **Nieuw**. Typ in het veld **Zoeken de marketplace** "Virtuele netwerk". **Virtueel netwerk** vinden in de resulterende lijst en klik op om het **Virtuele netwerk** blade.

    ![Zoeken naar een virtueel netwerk blade] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Zoeken naar een virtueel netwerk blade")

3. Aan de onderkant van de blade virtuele netwerk in de lijst **Selecteer een implementatiemodel** **klassieke**selecteren en klik op **maken**.

    ![Selecteer het implementatiemodel] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Selecteer het implementatiemodel")

4. Op het blad **virtueel netwerk maken** door de VNet-instellingen te configureren. In deze blade voegt u uw eerste adresruimte en een bereik met één subnet. Als u klaar bent met het maken van de VNet, kunt u teruggaan en voeg extra subnetten en adresruimten.

    ![Maken virtueel netwerk blade] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Maken virtueel netwerk blade")

5. Controleer of het **abonnement** is de juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.

6. Klik op **resourcegroep** en hetzij een bestaande resourcegroep selecteren of een nieuw typt u een naam voor uw nieuwe resourcegroep te maken. Als u een nieuwe groep maken wilt, benoemen de resourcegroep, afhankelijk van uw van geplande configuratiewaarden. Ga naar [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md#resource-groups)voor meer informatie over resourcegroepen.

7. Selecteer vervolgens de **locatie** -instellingen voor uw VNet. De locatie bepaalt u waar de resources die u op deze VNet implementeert wordt geplaatst.

8. **Pincode voor dashboard** selecteren als u uw VNet gemakkelijk terugvinden op het dashboard en klik vervolgens op **maken**.
    
    ![Pincode voor dashboard] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Pincode voor dashboard")


9. Nadat u op maken klikt, ziet u een tegel op uw dashboard dat de vorderingen van uw VNet weerspiegelt. De tegel wordt gewijzigd wanneer de VNet wordt gemaakt.

    ![Maken virtueel netwerk naast elkaar] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Maken virtueel netwerk naast elkaar")

10. Nadat u het virtuele netwerk maakt, kunt u het IP-adres van een DNS-server toevoegen voor het verwerken van naamomzetting. De instellingen voor het virtuele netwerk openen, klikt u op DNS-servers en voeg het IP-adres van de DNS-server die u wilt gebruiken. Deze instelling maakt een nieuwe DNS-server. Zorg ervoor dat een DNS-server die uw resources met communiceren kunnen toevoegen.

Als het virtuele netwerk is gemaakt, wordt er **dat gemaakt** weergegeven onder **Status** op de pagina netwerken in de klassieke Azure portal.

### <a name="gateway"></a>Deel 2: Gateway subnet en een dynamische routering gateway maken

In deze stap maakt u een gateway-subnet en gateway zijn een dynamische routering. In Azure portal voor het implementatiemodel klassiek kunt maken van het subnet van de gateway en de gateway worden uitgevoerd via de blades met dezelfde configuratie.

1. Ga naar het virtuele netwerk waarvoor wilt u een gateway maken in de portal.

2. Klik op het blad voor het virtuele netwerk, op het blad **Overzicht** in de sectie VPN-verbindingen op **Gateway**.

    ![Klik hier om een gateway maken] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Klik hier om een gateway maken")


3. Selecteer **punt-naar-site op het blad **Nieuwe VPN-verbinding** **.

    ![Het type verbinding P2S] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "Het type verbinding P2S")

4. Voor **Client-adresruimte**, Voeg het IP-adresbereik. Dit is het bereik waarop de VPN-clients een IP-adres wordt ontvangen wanneer verbinding wordt gemaakt. Het bereik automatisch gevuld verwijderen en toevoegen van uw eigen.

    ![Adresruimte] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Adresruimte")

5. Schakel het selectievakje **direct gateway maken** .

    ![Direct-gateway maken] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Direct-gateway maken")

6. Klik op **optionele gateway configuration** om de **configuratie van de standaardgateway** blade openen.

    ![Klik op optionele gateway configureren] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Klik op optionele gateway configureren")

7. Klik op **Subnet configureren instellingen nodig** om toe te voegen van het **subnet van de gateway**. Tijdens het maken van een subnet gateway /29 zo klein mogelijk is, wordt aangeraden dat u maakt een grotere subnet met meer adressen door ten minste /28 of /27 te selecteren. Hierdoor wordt voor voldoende adressen voor eventuele extra configuraties kunt u in de toekomst.

    >[AZURE.IMPORTANT] Bij het werken met gateway subnetten, Vermijd een beveiligingsgroep netwerk (NSG) naar het subnet van de gateway koppelen. Koppelen van een beveiligingsgroep netwerk op dit subnet, is het mogelijk dat uw VPN-gateway niet meer werkt zoals verwacht. Zie voor meer informatie over beveiligingsgroepen netwerk [Wat is een netwerk-beveiligingsgroep?](../articles/virtual-network/virtual-networks-nsg.md)

    ![GatewaySubnet toevoegen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "GatewaySubnet toevoegen")

8. Selecteer de **grootte**van de gateway. Dit is de gateway SKU die u gebruiken wilt voor het maken van uw gateway virtueel netwerk. In de portal is de SKU standaard **Basic**. Zie voor meer informatie over SKU's gateway, [Over VPN-Gateway-instellingen](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![grootte van de gateway](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Selecteer het **Type Routing** voor uw gateway. P2S configuraties vereisen een **dynamische** routering type. Klik op **OK** als u klaar bent met het configureren van deze blade.

    ![Routetype configureren] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Routetype configureren")

10. Klik op **OK** onder aan het blad om te beginnen met het maken van uw van virtuele netwerkgateway op de bladeserver **Nieuwe VPN-verbinding** . Dit kan maximaal 45 minuten duren. 


## <a name="generatecerts"></a>Afdeling 2 - certificaten

Certificaten worden gebruikt door Azure voor de verificatie van VPN-clients voor Point-to-Site VPN-verbindingen. U exporteren een certificaat met openbare gegevens (niet de persoonlijke sleutel) als een Base64-gecodeerd x.509-cer-bestand van een basiscertificaat dat wordt gegenereerd door een bedrijfsoplossing certificaat of een zelf-ondertekend basiscertificaat. U het certificaat voor openbare gegevens importeren uit het basiscertificaat naar Azure. Bovendien moet u een clientcertificaat uit het basiscertificaat voor clients te genereren. Elke client die verbinding met het virtuele netwerk via een P2S wil moet beschikken over een clientcertificaat geïnstalleerd die is gegenereerd op basis van het basiscertificaat.

### <a name="cer"></a>Deel 1: Het cer-bestand voor het basiscertificaat ophalen


Als u een bedrijfsoplossing, kunt u uw bestaande certificaatketen. Als u een enterprise-oplossing CA niet gebruikt, kunt u een hoofdmap zelf-ondertekend certificaat maken. Een methode voor het maken van een zelfondertekend certificaat is makecert.

- Als u een certificaat bedrijfssysteem, krijgen het cer-bestand voor het basiscertificaat dat u wilt gebruiken. 

- Als u een bedrijfsoplossing voor het certificaat niet gebruikt, moet u een basis zelfondertekend certificaat te genereren. Voor Windows 10 stappen, kunt u verwijzen naar [werken met zelfondertekende basiscertificaten voor punt-naar-Site-configuraties](vpn-gateway-certificates-point-to-site.md).

1. Als u wilt een cer-bestand ophalen van een certificaat, **certmgr.msc** openen en zoek het basiscertificaat. Met de rechtermuisknop op het certificaat zelfondertekend root, klik op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik op **volgende**in de Wizard, selecteert u **Nee, de persoonlijke sleutel niet exporteren**en klik op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** selecteren **Base-64 encoded X.509 (. (CER).** Klik vervolgens op **volgende**. 

4. Op het **te exporteren bestand**, **Blader** naar de locatie waarnaar u het certificaat exporteren. Naam voor **de naam van**het certificaatbestand. Klik vervolgens op **volgende**.

5. Klik op **Voltooien** om het exporteren van het certificaat.


### <a name="genclientcert"></a>Deel 2: Een certificaat genereren

Kunt u een uniek certificaat genereren voor elke client die verbinding maakt of kunt u hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid om een enkel certificaat intrekken indien nodig. Anders als iedereen hetzelfde certificaat gebruikt en u vindt dat u wilt intrekken van het certificaat voor een bepaalde klant, moet u genereren en nieuwe certificaten installeren voor alle clients die gebruikmaken van dat certificaat worden geverifieerd.

- Als u een bedrijfsoplossing certificaat genereren een clientcertificaat met algemene waarde indeling van de 'name@yourdomain.com', in plaats van de indeling 'domeinnaam\gebruikersnaam'. 

- Als u een zelf-ondertekend certificaat gebruikt, Zie [werken met zelfondertekende basiscertificaten voor punt-naar-Site-configuraties](vpn-gateway-certificates-point-to-site.md) voor het genereren van een clientcertificaat.

### <a name="exportclientcert"></a>Deel 3: Uitvoer het clientcertificaat

Een certificaat installeren op elke computer die u wilt verbinden met het virtuele netwerk. Een clientcertificaat is vereist voor verificatie. U kunt automatiseren met het installeren van het clientcertificaat of kunt u deze handmatig installeren. De volgende stappen leest u exporteren en het certificaat handmatig installeren.

1. Als u wilt een certificaat exporteert, kunt u *certmgr.msc*. Met de rechtermuisknop op het certificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**.
2. Dit certificaat met de persoonlijke sleutel exporteren. Dit is een *PFX* -bestand. Zorg ervoor dat te nemen of het wachtwoord die u hebt ingesteld voor dit certificaat (key).

## <a name="upload"></a>Afdeling 3 - het root certificaat cer-bestand uploaden

Nadat u de gateway hebt gemaakt, kunt u het cer-bestand voor een vertrouwd basiscertificaat kunt uploaden naar Azure. U kunt de basiscertificaten van maximaal 20 bestanden uploaden. U uploaden de persoonlijke sleutel voor het basiscertificaat niet naar Azure. Zodra het cer-bestand is geüpload, Azure worden gebruikt voor het verifiëren van clients die verbinding met het virtuele netwerk maken.

1. Klik op de afbeelding **clients** openen de **Point-to-site VPN-verbinding op de **VPN-verbindingen** -sectie van het blad voor de VNet,** blade.

    ![Clients] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Clients")

2. Op de **punt-naar-site verbinding** blade, klik op **certificaten beheren** om de bladeserver **certificaten** te openen.<br>

    ![Certificaten blade](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Klik op **uploaden** om te openen de blade **certificaat uploaden** op de bladeserver **certificaten** .<br>

    ![Certificaten blade uploaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Klik op de afbeelding van de map te bladeren naar het cer-bestand. Selecteer het bestand en klik vervolgens op **OK**. Vernieuw de pagina om het geüploade certificaat weergegeven op het blad van **certificaten** .

    ![Certificaat uploaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Sectie 4 – het configuratiepakket VPN-client genereren

Voor verbinding met het virtuele netwerk, moet u ook een VPN-client configureren. De client-computer vereist een clientcertificaat en het juiste configuratiepakket voor VPN-client voor de verbinding.

Het pakket voor VPN-clients bevat configuratiegegevens voor het configureren van de VPN-clientsoftware ingebouwd in Windows. Extra software het pakket niet geïnstalleerd. De instellingen zijn specifiek voor het virtuele netwerk waarmee u verbinding wilt maken. Zie voor een lijst met client-besturingssystemen die worden ondersteund, de verbindingen met [punt-naar-Site](vpn-gateway-vpn-faq.md#point-to-site-connections) gedeelte van de VPN-Gateway Veelgestelde vragen. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>Voor het genereren van het pakket voor VPN-clients configureren

1. Klik op de client-afbeelding te openen de **Point-to-site VPN-verbinding in de Azure portal, in het blad **Overzicht** voor uw VNet, **VPN-verbindingen**,** blade.
2. Aan de bovenkant van de **Point-to-site VPN verbinding** blade, klikt u op het pakket dat overeenkomt met het client-besturingssysteem waarop deze worden geïnstalleerd:

 - Selecteer de **VPN-Client (64-bits)**voor 64-bits-clients.
 - Voor 32-bits clients, selecteert u de **VPN-Client (32-bits)**.

    ![Configuratie voor VPN-client downloaden](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Er verschijnt een Azure genereren van de VPN-client configuratiepakket voor het virtuele netwerk. Het pakket wordt gegenereerd na een paar minuten en wordt er een bericht weergegeven op de lokale computer dat het pakket is gedownload. Het configuratiebestand opslaan. U installeert dit op elke clientcomputer die verbinding maakt met het virtuele netwerk met P2S.


## <a name="clientconfiguration"></a>Hoofdstuk 5 - de clientcomputer configureren

### <a name="part-1-install-the-client-certificate"></a>Deel 1: Installeer het certificaat voor de client

Elke clientcomputer moet beschikken over een clientcertificaat om te verifiëren. Wanneer u het certificaat installeert, moet u het wachtwoord dat is gemaakt nadat het certificaat is geëxporteerd.

1. Het .pfx-bestand naar de clientcomputer kopiëren.
2. Dubbelklik op het pfx-bestand om het te installeren. Locatie van de installatie niet wijzigen.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Deel 2: Installeer het pakket voor VPN-clients configureren

Op voorwaarde dat de versie overeenkomt met de architectuur van de client, kunt u het hetzelfde pakket VPN-client configureren op elke clientcomputer.

1. Het configuratiebestand lokaal op de computer die u wilt verbinden met het virtuele netwerk en dubbelklik op het .exe-bestand kopiëren. 

2. Zodra het pakket is geïnstalleerd, kunt u de VPN-verbinding te starten. Het configuratiepakket is niet ondertekend door Microsoft. U wilt ondertekenen van het pakket met handtekening service van uw organisatie of uzelf met [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)ondertekenen. Het is OK om het pakket te gebruiken zonder te melden. Echter, als het pakket niet is ondertekend, een waarschuwing weergegeven wanneer u het pakket installeert.

3. Ga naar de **Netwerkinstellingen** op de clientcomputer en op **VPN**. Ziet u de verbinding weergegeven. De naam van het virtuele netwerk ziet dat deze verbinding met maken en zal uitzien: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN-client")

## <a name="connect"></a>Sectie 6 - verbinding maken met Azure

### <a name="connect-to-your-vnet"></a>Verbinding maken met uw VNet

1. Om verbinding met uw VNet, klikt u op de clientcomputer te navigeren naar de VPN-verbindingen en naar de VPN-verbinding die u hebt gemaakt. Dit is dezelfde naam als het virtuele netwerk genoemd. Klik op **verbinden**. Een pop-upbericht kan weergegeven dat verwijst naar het gebruik van het certificaat. Als dit gebeurt, klikt u op **Doorgaan** om meer bevoegdheden te gebruiken. 

2. Klik op de statuspagina van **verbinding** op **verbinden** om de verbinding te starten. Als u een **Certificaat selecteren** scherm ziet, controleert u of het clientcertificaat weer die u wilt gebruiken om verbinding te maken. Als dat niet het geval is, gebruikt u de pijl omlaag om het juiste certificaat selecteert en klik vervolgens op **OK**.

    ![Verbinding maken] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "VPN-clientverbinding")

3. Je moet nu verbinding maken.

    ![Established verbinding] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Verbinding maken")

### <a name="verify-the-vpn-connection"></a>Controleer of de VPN-verbinding

1. Om te controleren dat uw VPN-verbinding actief is, opent u een opdrachtprompt en voer *ipconfig/all*.
2. De resultaten bekijken. U ziet het IP-adres dat u hebt ontvangen is een van de adressen binnen het adresbereik van de punt-naar-Site verbinding die u hebt opgegeven tijdens het maken van uw VNet. De resultaten moet iets ongeveer als volgt uit:

Voorbeeld:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Volgende stappen

U kunt virtuele machines toevoegen aan het virtuele netwerk. Zie [een aangepaste virtuele machine maken](../virtual-machines/virtual-machines-windows-classic-createportal.md).