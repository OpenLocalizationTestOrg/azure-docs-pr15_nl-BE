<properties
    pageTitle="Verbinding maken met computers en apparaten aan via de Gateway OMS OMS | Microsoft Azure"
    description="De OMS beheerde apparaten en Operations Manager bewaakt computers verbinden met de OMS-Gateway om gegevens te verzenden naar de OMS-service wanneer ze geen toegang tot het Internet."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>
<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>Verbinding maken met computers en apparaten aan via de Gateway OMS OMS

Dit document wordt beschreven hoe de OMS beheerde apparaten en computers System Center Operations Manager SCOM bewaakt gegevens naar de OMS-service verzenden kunnen wanneer ze geen toegang tot het Internet. De Gateway OMS kunt verzamelen van de gegevens en verzend het naar de OMS-service.

De gateway is een voorwaartse HTTP-proxy die HTTP tunneling met behulp van de opdracht HTTP-verbinding ondersteunt. De gateway kan verwerken 2.000 OMS gelijktijdig aangesloten apparaten als uitvoert op een 4-core CPU, 16 GB-server waarop Windows wordt uitgevoerd.

Als u bijvoorbeeld uw onderneming of organisatie groot mogelijk servers met netwerkverbindingen, maar mogelijk geen verbinding met het Internet. Een ander voorbeeld mogelijk veel punt van verkoop (POS) apparaten met geen middelen om ze rechtstreeks controle uit. En een ander voorbeeld: Operations Manager de OMS Gateway kunt gebruiken als een proxy-server. In deze voorbeelden kan de Gateway OMS gegevens overbrengen van de agents die op deze servers of OMS POS-apparaten zijn geïnstalleerd.

In plaats van elke afzonderlijke agent verzonden gegevens rechtstreeks aan OMS en vereisen directe Internet-verbinding, wordt in plaats daarvan alle gegevens van de agent verzonden via één computer met een internetverbinding. Deze computer is waar u installeren en gebruiken van de gateway. In dit scenario kunt u agents installeren op alle computers waarbij u gegevens wilt verzamelen. De gateway brengt vervolgens gegevens van de agents aan OMS rechtstreeks — de gateway van de gegevens die worden overgebracht niet analyseren.

Om te controleren de OMS-Gateway en analyseren van de prestaties of de gegevens voor de server is geïnstalleerd, moet u de OMS-agent installeren op de computer waarop de gateway is geïnstalleerd.

De gateway moet toegang hebben tot het Internet voor het uploaden van gegevens naar OMS. Elke agent moet ook naar de gateway verbinding met het netwerk hebben zodat agents automatisch van gegevens van en naar de gateway overbrengen kunnen. Voor de beste resultaten, installeer niet de gateway op een computer waarop ook een domeincontroller is.

Hier is een diagram waarin de overdracht van directe agents aan OMS.

![directe agent-diagram](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Hier is een diagram waarin de gegevensstroom van Operations Manager aan OMS.

![Operations Manager-diagram](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>De OMS-Gateway installeren

Deze Gateway installeren vervangt eerdere versies van de Gateway (logboek Analytics doorstuurserver) is geïnstalleerd.

Voorwaarden: .net Framework 4.5, Windows Server 2012 R2 SP1 en hoger

1. De meest recente versie van de Gateway OMS downloaden vanaf het [Microsoft Download Center](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi).
2. U start de installatie, dubbelklik op **OMS Gateway.msi**.
3. Op de pagina Welkom **volgende**.  
    ![De wizard Setup gateway](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. Selecteer **ik ga akkoord met de voorwaarden in de licentie-overeenkomst** akkoord gaan met de gebruiksrechtovereenkomst en klik op **volgende**op de pagina Gebruiksrechtovereenkomst.
5. Op de poort en proxy adres pagina:
    1. Typ het TCP-poortnummer voor de gateway moet worden gebruikt. Setup wordt dit poortnummer van Windows firewall geopend. De standaardwaarde is 8080.
    Het geldige bereik van het poortnummer is 1-65535. Als de invoer niet in dit bereik valt, wordt een foutbericht weergegeven.
    2. Als de server waarop de gateway is geïnstalleerd, moet het gebruik van een proxy, typ desgewenst de proxy-adres waar de gateway nodig om verbinding te maken. Bijvoorbeeld `http://myorgname.corp.contoso.com:80` als veld leeg is, de gateway probeert te rechtstreeks verbinding met het Internet. Anders wordt verbinding de gateway met de proxy. Als de proxyserver verificatie vereist is, typt u uw gebruikersnaam en wachtwoord.
        ![Gateway Wizard proxyconfiguratie](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Klik op **volgende**
6. Als u niet Microsoft Updates is ingeschakeld, verschijnt de Microsoft Update-pagina waar u kunt Microsoft Updates inschakelen. Maak een selectie en klik vervolgens op **volgende**. Ga anders verder met de volgende stap.
7. Laat de standaard map **%ProgramFiles%\OMS Gateway** of typ de locatie waar u gateway installeren en klik op **volgende**op de pagina van de doelmap.
8. Op de pagina gereed voor installatie, klikt u op **installeren**. Beheer van een gebruikersaccount mogelijk aanvraagtoestemming installeren wordt weergegeven. Als dit het geval is, klikt u op **Ja**.
9. Nadat Setup is voltooid, klikt u op **Voltooien**. U kunt controleren of de service wordt uitgevoerd door het openen van de module services.msc in en controleer of **OMS Gateway** wordt weergegeven in de lijst met services.  
    ![Services – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Een agent installeren op apparaten

Zie [verbinding maken met Windows-computers aanmelden Analytics](log-analytics-windows-agents.md) voor meer informatie over het installeren van rechtstreeks verbonden agenten als nodig. Het artikel wordt beschreven hoe u de agent een Setup-wizard kunt installeren met behulp van de opdrachtregel.

## <a name="configure-oms-agents"></a>OMS-agents configureren

Zie [de proxy- en firewall instellingen configureren met de Microsoft Agent controleren](log-analytics-proxy-firewall.md) voor meer informatie over het configureren van een agent voor het gebruik van een proxy-server in dit geval is is de gateway.

Operations Manager agenten verzenden bepaalde gegevens zoals Operations Manager waarschuwingen, configuratie bepalen exemplaar ruimte en capaciteitsgegevens, via de Server. Andere grote hoeveelheden gegevens, zoals Logboeken, prestaties en beveiliging van IIS worden rechtstreeks naar de OMS-Gateway verzonden. Zie [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md) voor een volledige lijst met gegevens die worden verzonden via elk kanaal.

>[AZURE.NOTE]
Als u van plan bent de Gateway gebruikt met Netwerktaakverdeling Zie [Netwerktaakverdeling desgewenst configureren](#optionally-configure-network-load-balancing).

## <a name="configure-a-scom-proxy-server"></a>Een proxyserver SCOM configureren

U configureert Operations Manager om de gateway te fungeren als een proxy-server toevoegen. Als u de proxyconfiguratie, wordt de proxyconfiguratie automatisch toegepast op de agenten die rapporteren aan de Operations Manager.

De Gateway voor de ondersteuning van Operations Manager, hebt u nodig hebt:

- Microsoft Monitoring Agent (agent-versie **8.0.10900.0** of hoger) op de Gateway-server geïnstalleerd en geconfigureerd voor het Kantoorbeheersysteem werkruimten die u wilt communiceren.
- De gateway moet Internet-verbinding hebt of verbinding met een proxyserver die is.

### <a name="to-configure-scom-for-the-gateway"></a>SCOM voor de gateway configureren

1. Open de console Operations Manager en **Operations Management Suite**, klik op **verbinding** en klik op **Proxy-Server configureren**:  
    ![Operations Manager-proxyserver configureren](./media/log-analytics-oms-gateway/scom01.png)
2. Selecteer **een proxyserver toegang te krijgen tot het Operations Management Suite** en typ vervolgens het IP-adres van de Gateway OMS-server. Ervoor te zorgen dat u met begint het `http://` prefix:  
    ![Operations Manager-adres van de proxy server](./media/log-analytics-oms-gateway/scom02.png)
3. Klik op **Voltooien**. De Operations Manager server is verbonden met uw werkruimte OMS.

## <a name="configure-network-load-balancing"></a>Netwerktaakverdeling configureren

U kunt de gateway voor maximale beschikbaarheid gebruiken door het maken van een cluster voor netwerktaakverdeling configureren. De cluster beheert het netwerkverkeer door aangevraagde verbindingen van de Agents Microsoft controle omleiden via de knooppunten van de agents. Als een Gateway-server uitvalt, wordt het verkeer omgeleid naar andere knooppunten.

1. Open beheer van netwerktaakverdeling en een cluster maken.
2. Met de rechtermuisknop op het cluster voordat u gateways toevoegt en selecteer **Eigenschappen van Cluster.** Configureer het cluster heeft zijn eigen IP-adres:  
    ![Beheer van netwerktaakverdeling: IP-adressen van Cluster](./media/log-analytics-oms-gateway/nlb01.png)
3. OMS-gatewayserver verbinding met de Microsoft Monitoring Agent is geïnstalleerd, met de rechtermuisknop op IP-adres van het cluster en klik op **Host aan Cluster toevoegen**.  
    ![Netwerk laden Balancing Manager – toevoegen Host aan Cluster](./media/log-analytics-oms-gateway/nlb02.png)
4. Geef het IP-adres van de Gateway van de server waarmee u verbinding wilt maken:  
    ![Network Load Balancing Manager – Host aan Cluster toevoegen: verbinding maken](./media/log-analytics-oms-gateway/nlb03.png)
5. Op computers die geen internetverbinding hebt, moet het IP-adres van het cluster gebruiken wanneer u de **Microsoft Monitoring Agenteigenschappen**configureren:  
    ![Microsoft Agenteigenschappen: Proxy-instellingen controleren](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Configureren voor automatisering hybride werknemers

Als u automation hybride werknemers in uw omgeving hebt, u volgt de handmatige, tijdelijke oplossingen voor het configureren van de Gateway voor de ondersteuning van deze.

In de volgende stappen moet u weten de Azure regio waar de automatisering account zich bevindt. Zoek de locatie:

1. Log in om de [Azure portal](https://portal.azure.com/).
2. Selecteer de service Azure automatisering.
3. Selecteer de juiste Azure automatisering.
4. De regio onder **locatie**weergeven.  
    ![Azure portal – locatie automatisering](./media/log-analytics-oms-gateway/location.png)

De volgende tabellen gebruikt voor het identificeren van de URL voor elke locatie:

**Project runtime gegevens URL 's**

| **locatie** | **URL** |
| --- | --- |
| Centrale Noord-Amerikaanse | ncus-jobruntimedata-Prod.-su1.azure-automation.net |
| West-Europa | We-jobruntimedata-Prod.-su1.azure-automation.net |
| Zuid-centraal-Verenigde Staten | scus-jobruntimedata-Prod.-su1.azure-automation.net |
| Oost-Verenigde Staten | eus-jobruntimedata-Prod.-su1.azure-automation.net |
| Centraal-Canada | CC-jobruntimedata-Prod.-su1.azure-automation.net |
| Noord-Europa | ne-jobruntimedata-Prod.-su1.azure-automation.net |
| Zuid-Oost-Azië | zee-jobruntimedata-Prod.-su1.azure-automation.net |
| Centraal India | CID-jobruntimedata-Prod.-su1.azure-automation.net |
| Japan | jpe-jobruntimedata-Prod.-su1.azure-automation.net |
| Australië | ASE-jobruntimedata-Prod.-su1.azure-automation.net |

**Agent-service URL 's**

| **locatie** | **URL** |
| --- | --- |
| Centrale Noord-Amerikaanse | ncus-agentservice-Prod.-1.azure-automation.net |
| West-Europa | Wij-agentservice-Prod.-1.azure-automation.net |
| Zuid-centraal-Verenigde Staten | scus-agentservice-Prod.-1.azure-automation.net |
| Oost-Verenigde Staten | eus2-agentservice-Prod.-1.azure-automation.net |
| Centraal-Canada | CC-agentservice-Prod.-1.azure-automation.net |
| Noord-Europa | ne-agentservice-Prod.-1.azure-automation.net |
| Zuid-Oost-Azië | zee-agentservice-Prod.-1.azure-automation.net |
| Centraal India | CID-agentservice-Prod.-1.azure-automation.net |
| Japan | jpe-agentservice-Prod.-1.azure-automation.net |
| Australië | ASE-agentservice-Prod.-1.azure-automation.net |

Als uw computer is geregistreerd als een hybride werknemer automatisch voor u herstellen met behulp van de oplossing voor het beheer van updates, moet u deze stappen gebruiken:

1. De taakgegevens Runtime service URL's toevoegen aan de lijst toegestane Host op de OMS-Gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. De OMS Gateway-Service opnieuw starten met behulp van de volgende PowerShell-cmdlet:`Restart-Service OMSGatewayService`

Als uw computer op-ingescheept naar Azure automatisering met behulp van de cmdlet hybride werknemer registratie, voer deze stappen uit:

1. De URL van de agent registratie toevoegen aan de lijst toegestane Host op de OMS-Gateway. Bijvoorbeeld:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. De taakgegevens Runtime service URL's toevoegen aan de lijst toegestane Host op de OMS-Gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. De OMS Gateway-Service opnieuw starten.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Handig PowerShell-cmdlets

Cmdlets kunt u taken uitvoeren die nodig zijn voor het bijwerken van de configuratie-instellingen voor de OMS-Gateway. Voordat u ze kunt gebruiken, moet u:

1. De Gateway OMS (MSI) te installeren.
2. Open het venster PowerShell.
3. Typ de volgende opdracht voor het importeren van de module:`Import-Module OMSGateway`
4. Als er geen fout is opgetreden in de vorige stap, de module is geïmporteerd en de cmdlets kunnen worden gebruikt. Type`Get-Module OMSGateway`
5. Nadat u wijzigingen met de cmdlets aanbrengt, zorg ervoor dat u de Gateway-service opnieuw starten.

Als u een foutbericht in stap 3, wordt de module is niet geïmporteerd. De fout kan optreden als PowerShell kan de module te vinden is. Deze kunt u vinden in het installatiepad van de Gateway: C:\Program File\Microsoft OMS Gateway\PowerShell.

| **Cmdlet** | **Parameters** | **Beschrijving** | **Voorbeelden** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Sleutel (vereist) <br> Waarde | Hiermee wijzigt u de configuratie van de service | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Sleutel | Deze eigenschap haalt de configuratie van de service | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Adres <br> Gebruikersnaam <br> Wachtwoord | Adres (en referentie) van relay (upstream) proxy instellen | 1. een antwoord-proxy en de referentie instellen:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. Stel een antwoord proxy die niet nodig voor verificatie:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. Schakel het antwoord proxy-instellingen, dat wil zeggen, hoeft niet een antwoord-proxy:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | Met deze eigenschap wordt het adres van de relay (upstream) proxy | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | Host (vereist) | Voegt de host aan de lijst met toegestane | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`t | Host (vereist) | Hiermee verwijdert u de host uit de lijst met toegestane | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | Met deze eigenschap wordt de momenteel toegestane host (alleen lokaal geconfigureerde host zijn toegestaan, neem niet automatisch gedownload toegestane hosts) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | Onderwerp (vereist) | Wordt toegevoegd aan de lijst met toegestane certificaat | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | Onderwerp (vereist) | De certificaathouder client verwijdert uit de lijst met toegestane | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`e |   | Deze eigenschap haalt de client momenteel toegestane certificaat onderwerpen (uitsluitend de lokaal geconfigureerde toegestane onderwerpen, neem niet automatisch gedownload toegestane onderwerpen) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Problemen oplossen met

Het is raadzaam dat u de OMS-agent installeren op computers waarop de gateway is geïnstalleerd. Vervolgens kunt u de agent voor het verzamelen van de gebeurtenissen die zijn vastgelegd door de gateway.

![Logboeken-OMS Gateway logboek](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS Gateway gebeurtenis-id's en beschrijvingen**

De volgende tabel ziet u de gebeurtenis-id's en omschrijvingen voor OMS Gateway gebeurtenissen.

| **ID** | **Beschrijving** |
| --- | --- |
| 400 | Een toepassingsfout waarbij beschikt niet over een specifieke ID |
| 401 | Onjuiste configuratie. Bijvoorbeeld: listenPort = "text" in plaats van een geheel getal |
| 402 | Uitzondering bij het parseren van TLS handshake berichten |
| 403 | Fout bij het netwerk. Bijvoorbeeld: kan geen verbinding met de doelserver |
| 100 | Algemene informatie |
| 101 | Service is gestart. |
| 102 | Service is gestopt. |
| 103 | Een HTTP-verbinding maken met opdracht ontvangen van client |
| 104 | Niet een HTTP-verbinding maken met opdracht |
| 105 | Doelserver zich niet in de lijst met toegestane of de bestemming wordt niet beveiligde poort (443) <br> <br> Zorg ervoor dat de MMA-agent op de Gateway-server en de communicatie met de Gateway worden verbonden met dezelfde werkruimte logboek Analytics.|
| 105 | TcpConnection fout: ongeldig clientcertificaat: CN = Gateway <br><br> Ervoor te zorgen dat: <br> <br> & #149; U gebruikt een Gateway met het versienummer 1.0.395.0 of hoger. <br> & #149; De MMA-agent op de Gateway-server en de communicatie met de Gateway zijn verbonden met dezelfde werkruimte logboek Analytics. |
| 106 | De reden dat de TLS-sessie verdachte en afgewezen |
| 107 | De TLS-sessie is gecontroleerd. |

**Prestatiemeteritems voor het verzamelen van**

De volgende tabel ziet de prestatiemeteritems beschikbaar voor de Gateway OMS. U kunt de items met de functie Prestatiemeter kunt toevoegen.

| **Naam** | **Beschrijving** |
| --- | --- |
| OMS Gateway/actieve clientverbinding | Aantal actieve verbindingen van clients netwerk (TCP) |
| OMS Gateway/aantal | Aantal fouten |
| OMS Gateway/aangesloten Client | Het aantal aangesloten clients |
| OMS-Gateway of afkeuring tellen | Het aantal afwijzingen door een validatiefout TLS |

![Prestatiemeteritems OMS-Gateway](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>Hulp

Wanneer u bent ingelogd op de portal Azure, maakt u een verzoek om bijstand met de Gateway OMS of andere Azure service of functie van een service.
Klik op het symbool van het vraagteken in de rechterbovenhoek van het portaal voor het aanvragen van Hulp op en klik vervolgens op **nieuw ondersteuningsverzoek**. Voltooi het nieuwe support aanvraagformulier.

![Nieuw ondersteuningsverzoek](./media/log-analytics-oms-gateway/support.png)

U kunt ook feedback over OMS of Analytics logboek op het [Feedbackforum Microsoft Azure](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Volgende stappen

- [Toevoegen-gegevensbronnen](log-analytics-data-sources.md) om gegevens van de aangesloten bronnen in uw werkruimte OMS verzamelen en opslaan in de bibliotheek OMS.
