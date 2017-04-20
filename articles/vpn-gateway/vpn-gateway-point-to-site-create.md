<properties
   pageTitle="Configureren van een Point-to-Site VPN-gateway verbinding met een virtueel netwerk met de klassieke portal van Azure | Microsoft Azure"
   description="Veilig verbinding maken met het virtuele netwerk Azure door een Point-to-Site VPN-gateway verbinding te maken."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Een punt-naar-Site verbinding configureren met een VNet met de klassieke portal

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassiek - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Classic - klassieke Portal](vpn-gateway-point-to-site-create.md)

Een punt-naar-Site (P2S)-configuratie kunt u een beveiligde verbinding maken vanaf een werkstation met een virtueel netwerk. Een verbinding P2S is handig wanneer u wilt verbinding maken met uw VNet vanaf een externe locatie, zoals thuis of een vergadering, of als u alleen een paar-clients die verbinding moeten maken met een virtueel netwerk.

Dit artikel helpt u bij het maken van een VNet met een punt-naar-Site verbinding in het **klassieke deployment-model** met het **klassieke portal**.

Punt-tot-Site verbindingen hoeven niet een VPN-apparaat of een publieke IP-adres om te werken. Een VPN-verbinding wordt vastgesteld door de verbinding van de clientcomputer wordt gestart. Zie voor meer informatie over verbindingen met Point-to-Site [VPN-Gateway Veelgestelde vragen](vpn-gateway-vpn-faq.md#point-to-site-connections) en [Planning en ontwerp](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Van implementatiemodellen en methoden voor P2S verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In de volgende tabel ziet u de twee implementatiemodellen en beschikbare implementatiemethoden voor P2S configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Basiswerkstroom

![Punt-naar-Site-diagram] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "punt-tot-site")
 
Volgt doorlopen u de stappen voor het maken van een beveiligde verbinding punt-naar-Site met een virtueel netwerk. 

De configuratie voor een punt-naar-Site verbinding is onderverdeeld in vier secties. De volgorde waarin u elk van deze secties configureren is belangrijk. Geen stappen overslaan of Vooruit springen.

- **Afdeling 1** Een virtueel netwerk en VPN-gateway maken.
- **Afdeling 2** De certificaten gebruikt voor verificatie maken en uploaden.
- **Afdeling 3** Exporteren en installeren van uw certificaten.
- **Deel 4** Configureer de VPN-client.

## <a name="vnetvpn"></a>Sectie 1 - een virtueel netwerk en een VPN-gateway maken

### <a name="part-1-create-a-virtual-network"></a>Deel 1: Een virtueel netwerk maken

1. Log in op de [Azure klassieke portal](https://manage.windowsazure.com/). De klassieke portal, niet de Azure portal gebruikt. Op dit moment geen u met behulp van de portal Azure P2S verbinding kunt maken.

2. Klik op **Nieuw**in de linkerbenedenhoek van het scherm. Klik op **Netwerkservices**en klik op **Virtual Network**in het navigatiedeelvenster. Klik op **Aangepaste maken** om te beginnen met de configuratiewizard.

3. Voer de volgende gegevens op de pagina **Virtuele netwerkdetails** en klik op de volgende pijl in de rechterbenedenhoek.
    - **Naam**: de naam van het virtuele netwerk. Bijvoorbeeld 'VNet1'. Dit is de naam die u verwijzen kunt naar u VMs om deze VNet te implementeren.
    - **Locatie**: de locatie is afhankelijk van de fysieke locatie (regio), waar u uw bronnen (VMs) zich bevinden. Als u wilt dat de VMs die u op deze virtuele netwerk op fysiek bevindt zich in de Oost-Amerikaanse implementeert, selecteert u die locatie in. De regio die is gekoppeld aan het virtuele netwerk nadat u deze hebt gemaakt kunt u niet wijzigen.

4. Voer de volgende gegevens op de pagina **DNS-Servers en VPN-verbindingen** en klik vervolgens op de pijl volgende gemarkeerd in de rechterbenedenhoek.
    - **DNS-Servers**: Voer de naam van de DNS-server en IP-adres of Selecteer een eerder geregistreerde DNS-server in het snelmenu. Deze instelling maakt een DNS-server. U kunt de DNS-servers opgeven die u wilt gebruiken voor het herleiden van namen voor deze virtuele netwerk. Als u wilt dat de service voor naamomzetting Azure standaard gebruiken, laat u deze sectie leeg.
    - **VPN configureren punt-naar-Site**: Schakel het selectievakje in.

5. Geef op de pagina **Verbinding punt-naar-Site** , het IP-adresbereik waaruit VPN-clients een IP-adres wanneer verbinding ontvangt. Er zijn een paar regels met betrekking tot de adresbereiken die u kunt opgeven. Het is belangrijk om te controleren dat het bereik dat u opgeeft met een van de bereiken die zich op uw netwerk in de lokalen niet overlappen.

6. Voer de volgende informatie en klik vervolgens op de pijl volgende gemarkeerd.
 - **Address Space**: het eerste IP-en CIDR (aantal adressen).
 - **Toevoegen-adresruimte**: adresruimte alleen toevoegen als nodig is voor het netwerkontwerp van uw.

7. Geef het bereik van adressen die u wilt gebruiken voor het virtuele netwerk op de pagina **Virtuele netwerk adresruimten** . Dit zijn de dynamische IP-adressen (SPANNINGSDIPS) die wordt toegewezen aan de VMs en andere rol exemplaren die u met dit virtuele netwerk implementeert.<br><br>Het is vooral belangrijk voor een bereik selecteren dat niet overlapt met een van de bereiken die worden gebruikt voor het netwerk op gebouwen. U moet samenwerken met uw netwerkbeheerder die moet kunnen fungeren van een bereik van IP-adressen van uw space op ruimten netwerk adres kunt gebruiken voor het virtuele netwerk.

8. Voer de volgende informatie en klik op het vinkje om te beginnen met het maken van het virtuele netwerk.
 - **Address Space**: interne IP-adresbereik dat u gebruiken voor deze virtuele netwerk wilt, inclusief begin-IP- en tellen. Het is belangrijk dat u een bereik selecteren dat niet overlapt met een van de bereiken die worden gebruikt voor het netwerk op gebouwen. 
 - **Subnet toevoegen**: extra subnetten zijn niet vereist, maar u kunt een apart subnet maken voor VMs met statische SPANNINGSDIPS. Of u kunt uw VMs hebben in een subnet dat losstaat van de andere exemplaren van de rol.
 - **Gateway-subnet toevoegen**: het gateway-subnet is vereist voor een Point-to-Site VPN. Hiermee voegt het subnet van de gateway. Het gateway-subnet wordt alleen gebruikt voor het virtuele netwerkgateway.

9. Als het virtuele netwerk is gemaakt, wordt er **dat gemaakt** weergegeven onder **Status** op de pagina netwerken in de klassieke Azure portal. Zodra het virtuele netwerk is gemaakt, kunt u uw dynamische routering gateway.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Deel 2: Een dynamische routering gateway maken

Het type gateway moet worden geconfigureerd als dynamische. Statische routering gateways werken niet met deze functie.

1. Klik op het virtuele netwerk die u hebt gemaakt en navigeer naar de pagina van het **Dashboard** in de Azure klassieke portal op de pagina **netwerken** .

2. Klik op **Gateway maken**, te vinden op de onderkant van de pagina van het **Dashboard** . Er verschijnt een bericht waarin wordt gevraagd **wilt u een gateway voor virtuele netwerk "VNet1"**. Klik op **Ja** om te beginnen met het maken van de gateway. Het kan voor de gateway voor het maken van ongeveer 15 minuten duren.

## <a name="generate"></a>Afdeling 2 - genereren en uploaden van certificaten

Certificaten worden gebruikt voor de verificatie van VPN-clients voor Point-to-Site VPN-verbindingen. U kunt een basiscertificaat dat wordt gegenereerd door een bedrijfsoplossing certificaat of kunt u een zelfondertekend certificaat. U kunt maximaal 20 basiscertificaten uploaden naar Azure. Zodra het cer-bestand is geüpload, kunt Azure de informatie in het verifiëren van clients die beschikken over een clientcertificaat dat is geïnstalleerd. Dit certificaat moet worden gegenereerd vanuit hetzelfde certificaat met het cer-bestand.

In deze sectie doen u het volgende:

- Het cer-bestand voor een basiscertificaat ophalen. Dit is een zelf-ondertekend certificaat of kunt u uw certificaat bedrijfssysteem.
- Het cer-bestand uploaden naar Azure.
- Genereren van clientcertificaten.

### <a name="root"></a>Deel 1: Het cer-bestand voor het basiscertificaat ophalen

Als u een certificaat bedrijfssysteem, krijgen het cer-bestand voor het basiscertificaat dat u wilt gebruiken. In [deel 3](#createclientcert), kunt u clientcertificaten genereren uit het basiscertificaat.

Als u een bedrijfsoplossing voor het certificaat niet gebruikt, moet u een basis zelfondertekend certificaat te genereren. Voor Windows 10 stappen, kunt u verwijzen naar [werken met zelfondertekende basiscertificaten voor punt-naar-Site-configuraties](vpn-gateway-certificates-point-to-site.md). Het artikel begeleidt u bij makecert met behulp van een zelfondertekend certificaat te genereren en vervolgens het cer-bestand exporteren.

### <a name="upload"></a>Deel 2: Upload certificaatbestand voor het basiscertificaat voor Azure klassieke portal

Een vertrouwd certificaat toevoegen aan Azure. Wanneer u een bestand met Base64 gecodeerde X.509 (.cer) naar Azure toevoegt, geeft u aan dat Azure vertrouwt het basiscertificaat dat het bestand vertegenwoordigt.

1. Klik op **uploaden van een basiscertificaat**in de Azure klassieke portal op de pagina **certificaten** voor het virtuele netwerk.

2. Op de pagina **Certificaat uploaden** voor het basiscertificaat cer bladeren en klik op het vinkje.

### <a name="createclientcert"></a>Deel 3: Een certificaat genereren

Vervolgens genereert de clientcertificaten. Kunt u een uniek certificaat genereren voor elke client die verbinding maakt of kunt u hetzelfde certificaat gebruiken op meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is de mogelijkheid om een enkel certificaat intrekken indien nodig. Anders als iedereen hetzelfde certificaat gebruikt en u vindt dat u wilt intrekken van het certificaat voor een bepaalde klant, moet u genereren en nieuwe certificaten installeren voor alle clients die gebruikmaken van het certificaat worden geverifieerd.

- Als u een bedrijfsoplossing certificaat genereren een clientcertificaat met algemene waarde indeling van de 'name@yourdomain.com', in plaats van 'Domein\gebruikersnaam' van de NetBIOS-notatie. 

- Als u een zelf-ondertekend certificaat gebruikt, Zie [werken met zelfondertekende basiscertificaten voor punt-naar-Site-configuraties](vpn-gateway-certificates-point-to-site.md) voor het genereren van een clientcertificaat.

## <a name="installclientcert"></a>Afdeling 3 - uitvoer en het clientcertificaat installeren

Een certificaat installeren op elke computer die u wilt verbinden met het virtuele netwerk. Een clientcertificaat is vereist voor verificatie. U kunt automatiseren met het installeren van het clientcertificaat of u kunt handmatig installeren. De volgende stappen leest u exporteren en het certificaat handmatig installeren.

1. Als u wilt een certificaat exporteert, kunt u *certmgr.msc*. Met de rechtermuisknop op het certificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**.
2. Dit certificaat met de persoonlijke sleutel exporteren. Dit is een *PFX* -bestand. Zorg ervoor dat te nemen of het wachtwoord die u hebt ingesteld voor dit certificaat (key).
3. Het *.pfx* -bestand naar de clientcomputer kopiëren. Dubbelklik op het *PFX* -bestand om het te installeren op de clientcomputer. Voer het wachtwoord wanneer aangevraagd. Locatie van de installatie niet wijzigen.

## <a name="vpnclientconfig"></a>Sectie 4 - de VPN-client configureren

Voor verbinding met het virtuele netwerk, moet u ook een VPN-client configureren. De client vereist een clientcertificaat en juiste configuratie van de VPN-client voor de verbinding. Configureren van een VPN-client, voeren de volgende stappen in volgorde.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Deel 1: Het configuratiepakket VPN-client maken

1. Ga naar het menu kijkje in de rechterhoek in de Azure klassieke portal op de **Dashboard** -pagina voor het virtuele netwerk. Zie voor een lijst met client-besturingssystemen die worden ondersteund, de verbindingen met [punt-naar-Site](vpn-gateway-vpn-faq.md#point-to-site-connections) gedeelte van de VPN-Gateway Veelgestelde vragen. Het pakket voor VPN-clients bevat configuratiegegevens voor het configureren van de VPN-clientsoftware ingebouwd in Windows. Extra software het pakket niet geïnstalleerd. De instellingen zijn specifiek voor het virtuele netwerk waarmee u verbinding wilt maken.<br><br>Selecteer het downloadpakket dat overeenkomt met het client-besturingssysteem waarop deze worden geïnstalleerd:
 - Voor 32-bits clients, selecteert u **de 32-bits Client VPN downloaden**.
 - Selecteer **de 64-bits Client VPN downloaden**voor 64-bits-clients.

2. Het duurt een paar minuten om uw clientpakket te maken. Zodra het pakket is voltooid, kunt u het bestand kunt downloaden. Het *.exe* -bestand dat u downloadt kan veilig worden opgeslagen op uw lokale computer.

3. Nadat u genereren en de VPN-client-pakket van de klassieke Azure portal downloaden, kunt u het clientpakket installeren op de clientcomputer die u wilt verbinden met het virtuele netwerk. Als u van plan bent om het pakket voor VPN-clients installeren op meerdere clientcomputers, moet elke ze ook beschikken over een clientcertificaat dat is geïnstalleerd.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Deel 2: Installeer het pakket met VPN-configuratie op de client

1. Het configuratiebestand lokaal op de computer die u wilt verbinden met het virtuele netwerk en dubbelklik op het .exe-bestand kopiëren. 

2. Zodra het pakket is geïnstalleerd, kunt u de VPN-verbinding te starten. Het configuratiepakket is niet ondertekend door Microsoft. U wilt ondertekenen van het pakket met handtekening service van uw organisatie of uzelf met [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)ondertekenen. Het is OK om het pakket te gebruiken zonder te melden. Echter, als het pakket niet is ondertekend, een waarschuwing weergegeven wanneer u het pakket installeert.

3. Ga naar de **Netwerkinstellingen** op de clientcomputer en op **VPN**. Ziet u de verbinding weergegeven. De naam van het virtuele netwerk weergegeven dat verbinding met en zal er ongeveer zo: 

    ![VPN-client] (./media/vpn-gateway-point-to-site-create/vpn.png "VPN-client")


### <a name="part-3-connect-to-azure"></a>Deel 3: Verbinding maken met Azure

1. Om verbinding met uw VNet, klikt u op de clientcomputer te navigeren naar de VPN-verbindingen en naar de VPN-verbinding die u hebt gemaakt. Dit is dezelfde naam als het virtuele netwerk genoemd. Klik op **verbinden**. Een pop-upbericht kan weergegeven dat verwijst naar het gebruik van het certificaat. Als dit gebeurt, klikt u op **Doorgaan** om meer bevoegdheden te gebruiken. 

2. Klik op de statuspagina van **verbinding** op **verbinden** om de verbinding te starten. Als u een **Certificaat selecteren** scherm ziet, controleert u of het clientcertificaat weer die u wilt gebruiken om verbinding te maken. Als dat niet het geval is, gebruikt u de pijl omlaag om het juiste certificaat selecteert en klik vervolgens op **OK**.

    ![VPN-client 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN-clientverbinding")

3. Je moet nu verbinding maken.

    ![VPN-client 3] (./media/vpn-gateway-point-to-site-create/connected.png "VPN-clientverbinding 2")

### <a name="part-4-verify-the-vpn-connection"></a>Deel 4: Controleer of de VPN-verbinding

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

Als u meer informatie over virtuele netwerken wilt, Zie de pagina [Virtuele documentatie van uw netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) .
