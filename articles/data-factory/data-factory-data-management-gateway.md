<properties 
    pageTitle="Data Management Gateway voor Data Factory | Microsoft Azure"
    description="Een gegevensgateway instellen om gegevens te verplaatsen tussen lokale en de cloud. Data Management Gateway in Azure Data Factory gebruiken om uw gegevens te verplaatsen." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="jingwang"/>

# <a name="data-management-gateway"></a>Data Management Gateway
De Data Management Gateway is een client-agent die u in uw omgeving op locatie installeren moet te kopiëren tussen wolk en op ruimten gegevensarchieven. Ondersteund door Data Factory gegevensopslagplaatsen op ruimten worden vermeld in de sectie [ondersteunde gegevensbronnen](data-factory-data-movement-activities.md##supported-data-stores) . 

> [AZURE.NOTE] Gateway ondersteunt op dit moment alleen de kopie en opgeslagen procedure activiteit in Data Factory. Het is niet mogelijk met behulp van de gateway van een aangepaste activiteit toegang tot gegevensbronnen op gebouwen. 

In dit artikel vormt een aanvulling op de procedure in de [gegevens verplaatsen tussen lokale en cloud opgeslagen gegevens](data-factory-move-data-between-onprem-and-cloud.md) artikel. In het overzicht maakt u een pijpleiding die de gateway gebruikt om gegevens te verplaatsen vanuit een SQL Server-database op de gebouwen aan een Azure blob. Dit artikel bevat gedetailleerde informatie over de de Data Management Gateway.   

## <a name="overview"></a>Overzicht

### <a name="capabilities-of-data-management-gateway"></a>Mogelijkheden van gegevensbeheer gateway
Data Management Gateway biedt de volgende mogelijkheden:

- Model gegevensbronnen op gebouwen en gegevensbronnen in dezelfde fabriek gegevens cloud en verplaatsen van gegevens.
- Een door één venster voor controle en beheer met inzicht in de status van de gateway van de bladeserver Data Factory hebben.
- Toegang tot gegevensbronnen voor bedrijfsruimten veilig beheren.
    - Geen wijzigingen nodig zijn om de firewall van het bedrijf. Gateway wordt alleen uitgaande HTTP-verbindingen voor het openen van internet.
    - Referenties voor de gegevensarchieven in ruimten met uw certificaat coderen.
- Efficiënt verplaatsen van gegevens: de gegevens worden overgedragen parallel aan periodieke robuust netwerkproblemen met automatische logica opnieuw.

### <a name="command-flow-and-data-flow"></a>Opdracht stroom en gegevensoverdracht
Wanneer u een activiteit kopiëren gegevens te kopiëren tussen het lokale en cloud, de activiteit een gateway gebruikt om gegevens uit de gegevensbron voor de lokalen overbrengen naar de cloud en vice versa.

Hier high-level data stroom voor en samenvatting van de stappen voor het kopiëren met gegevensgateway: ![gegevensoverdracht via gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1.  Gegevens-ontwikkelaar maakt een gateway voor een Azure Data Factory gebruikt in de [portal Azure](https://portal.azure.com) [PowerShell-Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx). 
2.  Gegevens ontwikkelaar maakt een gekoppelde service voor een gegevensarchief op de gebouwen door te geven van de gateway. Als onderdeel van het instellen van de gekoppelde service gebruikt gegevens ontwikkelaar de toepassing van de referenties van de instelling verificatietypen en referenties op te geven.  Het dialoogvenster referenties instellen toepassing communiceert met het gegevensarchief voor het testen van de verbinding en de gateway naar de referenties op te slaan.
3. Gateway codeert de referenties aan het certificaat is gekoppeld aan de gateway (geleverd door de ontwikkelaar van gegevens), voordat u de referenties in de cloud opslaat.
4. Data Factory service communiceert met de gateway voor planning en beheer van projecten via een besturingskanaal die gebruikmaakt van een gedeelde Azure service bus wachtrij. Wanneer een activiteit kopie taak worden gestart moet, wachtrijen Data Factory verzoek samen met referentie-informatie. Gateway weer uit de taak na de wachtrij polling.
5.  De gateway decodeert de referenties met hetzelfde certificaat en vervolgens verbinding maakt met het gegevensarchief in ruimten met goede verificatietype en referenties.
6.  De gateway kopieert gegevens van een winkel in ruimten naar een cloud-opslag of vice versa afhankelijk van de configuratie van de activiteit van de kopie in de pijplijn gegevens. Voor deze stap communiceert de gateway rechtstreeks met cloud-gebaseerde opslagservices zoals Azure Blob-opslag via een beveiligde (HTTPS) kanaal.

### <a name="considerations-for-using-gateway"></a>Overwegingen bij het gebruik van gateway
- Een enkel exemplaar van Data Management Gateway kan worden gebruikt voor meerdere gegevensbronnen die op gebouwen. Echter **een enkele gateway-instantie is gebonden aan slechts één Azure data factory genoemd** en kan niet worden gedeeld met een andere data factory genoemd.
- U kunt **slechts één exemplaar van Data Management Gateway** op één computer geïnstalleerd hebben. Stel dat er twee fabrieken van gegevens die nodig zijn voor toegang tot gegevensbronnen op de gebouwen, moet u gateways installeren op computers met twee op gebouwen. Met andere woorden, is een gateway gebonden aan een specifieke data factory
- De hebt **gateway niet hoeft te worden op dezelfde computer als de gegevensbron**. Echter minder gateway dichter naar de gegevensbron met de tijd voor de gateway verbinding maken met de gegevensbron. Het is raadzaam de gateway te installeren op een machine die verschilt van die als host op gebouwen-gegevensbron fungeert. Als de gateway en de gegevensbron op verschillende computers, de gateway niet zou concurreren voor resources met een gegevensbron.
- U kunt **meerdere gateways op verschillende computers verbinding maken met dezelfde voor-premises gegevensbron**hebben. U kan bijvoorbeeld twee gateways voor de twee fabrieken van gegevens maar dezelfde gegevensbron in ruimten met beide fabrieken van de gegevens wordt geregistreerd.
- Als u al een gateway is geïnstalleerd op uw computer in een scenario voor **Power BI** , installeren een **afzonderlijke gateway voor Azure Data Factory** op een andere computer.
- Gateway moet worden gebruikt, zelfs als u **ExpressRoute**.
- De gegevensbron te behandelen als een gegevensbron op gebouwen (die zich achter een firewall) zelfs als u **ExpressRoute**gebruikt. De gateway gebruiken om de verbinding tussen de service en de gegevensbron te maken.
- U moet **de gateway gebruiken** zelfs als het gegevensarchief in de cloud op een **Azure IaaS VM**. 

## <a name="installation"></a>Installatie

### <a name="prerequisites"></a>Vereisten
- **De ondersteunde besturingssystemen** zijn Windows 7, Windows 8 / 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Installatie van de Gateway voor het beheer van gegevens op een domeincontroller wordt momenteel niet ondersteund.
- .NET framework 4.5.1 of hoger is vereist. Als u gateway op een computer met Windows 7 installeert, installeert u .NET Framework 4.5 of hoger. Zie [Systeemvereisten voor .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) voor meer informatie. 
- De aanbevolen **configuratie** voor de gateway-machine is ten minste 2 GHz, 4 cores, 8 GB RAM en 80 GB schijf.
- Als de hostcomputer in de slaapstand, reageert de gateway niet op aanvragen van gegevens. Daarom een passende **energiebeheerschema** configureren op de computer voordat u de gateway. Als de computer is geconfigureerd voor de slaapstand, wordt een bericht gevraagd om de installatie van gateway.
- U moet een beheerder op de computer installeren en configureren van de Data Management Gateway is. U kunt extra gebruikers toevoegen aan de groep lokale **Gebruikers gegevens Gateway** Windows. De leden van deze groep kunnen het hulpprogramma Data Management Gateway Configuration Manager gebruiken voor het configureren van de gateway. 

Als u per ongeluk op een bepaalde frequentie kopie activiteit wordt uitgevoerd, volgt het gebruik van bronnen (CPU, geheugen) op de computer ook hetzelfde patroon met piek en niet-actieve tijd. Brongebruik is ook sterk afhankelijk van de hoeveelheid gegevens die wordt verplaatst. Wanneer meerdere kopie taken uitgevoerd worden, ziet u Resourcegebruik tijdens piekuren omhoog gaan. 

### <a name="installation-options"></a>Installatie-opties
Data Management Gateway kan worden geïnstalleerd op de volgende manieren: 

- Door het downloaden van een MSI installatiepakket van het [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  Het MSI-bestand kan ook worden gebruikt om te upgraden van bestaande Data Management vindt u de meest recente versie met alle instellingen blijven behouden.
- Door te klikken op **downloaden en installeren van gegevensgateway** onder handmatige SETUP of **installeren op deze computer rechtstreeks** onder snelle installatie. Zie [verplaatsen tussen lokale en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor stapsgewijze instructies over het gebruik van snelle installatie. De handmatige gaat u naar het download center.  De instructies voor het downloaden en installeren van de gateway van Downloadcentrum zijn in de volgende sectie. 

### <a name="installation-best-practices"></a>Aanbevolen procedures voor installatie:
1.  Energieschema op de hostcomputer voor de gateway zodanig configureren dat de computer de slaapstand niet activeren. Als de hostcomputer in de slaapstand, reageert de gateway niet op aanvragen van gegevens.
2.  Back-up van het certificaat dat is gekoppeld aan de gateway.

### <a name="install-gateway-from-download-center"></a>Gateway installeren vanuit het Downloadcentrum
1. Ga naar [de downloadpagina van Microsoft Data Management Gateway](https://www.microsoft.com/download/details.aspx?id=39717). 
2. Klik op **downloaden**, selecteert u de juiste versie (**32-bits** en **64-bits**) en klik op **volgende**. 
3. De **MSI** rechtstreeks uitvoeren of opslaan op de vaste schijf en worden uitgevoerd.
4. Klik op **de pagina** Selecteer een **taal** klikt u op **volgende**.
5. **Accepteren** van de gebruiksrechtovereenkomst en klik op **volgende**. 
6. Selecteer de **map** , de gateway installeren en klik op **volgende**. 
7. Klik op de pagina **Gereed voor installatie** op **installeren**. 
8. Klik op **Voltooien** om de installatie te voltooien.
9. De sleutel ophalen uit de Azure portal. Zie het volgende gedeelte voor stapsgewijze instructies. 
10. Voer de volgende stappen uit op de pagina **gateway registreren** van **Data Management Gateway Configuration Manager** op uw computer: 
    1. De sleutel in de tekst te plakken.
    2. Klik eventueel op de **gateway-sleutel wordt weergegeven** als de belangrijkste tekst wilt zien.
    3. Klik op **registreren**. 

### <a name="register-gateway-using-key"></a>Registreren met behulp van sleutel gateway

#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Als u een logische gateway al hebt gemaakt in de portal
Een gateway maken in de portal en u de sleutel van de bladeserver **configureren** , volg de stappen van de stapsgewijze instructies in het artikel [verplaatst gegevens tussen op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) .    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Als u de logische gateway al hebt gemaakt in de portal
1. Ga naar de **Data Factory** blade in Azure portal en klikt u op naast elkaar **Gekoppelde Services** .

    ![Data Factory-blade](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Selecteer de logische **gateway** die u hebt gemaakt in de portal in de blade **Gekoppelde Services** . 

    ![logische gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
2. Klik op **downloaden en installeren van de gegevensgateway**in het blad **Gegevensgateway** .

    ![Downloadkoppeling in de portal](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
3. Klik op **sleutel opnieuw**in de blade **configureren** . Klik op Ja in het waarschuwingsbericht wordt weergegeven na het zorgvuldig lezen.

    ![Sleutel opnieuw maken](media/data-factory-data-management-gateway/recreate-key-button.png)
4. Klik op de knop kopiëren naast de toets. De sleutel naar het Klembord gekopieerd.
    
    ![Kopieer sleutel](media/data-factory-data-management-gateway/copy-gateway-key.png) 

### <a name="system-tray-icons-notifications"></a>Pictogrammen in het systeemvak / meldingen
De volgende afbeelding ziet u de pictogrammen in systeemvak die u ziet. 

![pictogrammen in het systeemvak](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Als u de cursor op de system tray pictogram/bericht plaatst, ziet u details over de status van de gateway updatebewerking in een pop-upvenster.

### <a name="ports-and-firewall"></a>Poorten en firewall
Er zijn twee firewalls, moet u rekening houden met: **firewall van het bedrijf** waarop de centrale router van de organisatie en **Windows firewall** zijn geconfigureerd als een daemon op de lokale computer waarop de gateway is geïnstalleerd.  

![firewalls](./media/data-factory-data-management-gateway/firewalls.png)

Op het niveau van de firewall van het bedrijf, moet u de volgende domeinen en uitgaande poorten configureren:

| Domeinnamen | Poorten | Beschrijving |
| ------ | --------- | ------------ |
| *. servicebus.windows.net | 443, 80 | Listeners op Bus-Relay-Service via TCP (443 voor toegangsbeheer token aanschaf vereist) | 
| *. servicebus.windows.net | 9350-9354 5671 | Optionele service bus relay via TCP | 
| *. core.windows.net | 443 | HTTPS | 
| *. clouddatahub.net | 443 | HTTPS | 
| Graph.Windows.NET | 443 | HTTPS |
| Login.Windows.NET | 443 | HTTPS | 

Deze uitgaande poorten zijn normaal gesproken op niveau van windows firewall ingeschakeld. Als dat niet het geval is, kunt u de domeinen en poorten dienovereenkomstig op gateway-computer.

#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Gegevens uit een gegevensarchief bron kopiëren naar een gegevensarchief sink

Controleer of de firewall-regels correct zijn ingeschakeld op de firewall van het bedrijf, Windows firewall op de gateway-computer en de gegevens zelf worden opgeslagen. Inschakelen van deze regels kan de gateway verbinding maken met zowel bron en kunnen opvangen. Regels voor iedere gegevensopslag die bij het kopiëren van bestanden betrokken is inschakelen.

Bijvoorbeeld als u wilt kopiëren uit **een gegevensarchief op ruimten een sink Azure SQL-Database of een Azure SQL Data Warehouse opvangen**, doen de volgende stappen uit: 

- Uitgaande **TCP-** communicatie toestaan op poort **1433** voor Windows firewall en firewall van het bedrijf
- Configureer de firewallinstellingen van Azure SQL server het IP-adres van de gateway-computer toevoegen aan de lijst van toegestane IP-adressen. 


### <a name="proxy-server-considerations"></a>Overwegingen voor proxy server
Als uw zakelijke netwerkomgeving een proxyserver gebruikt voor toegang tot het internet, configureert u Data Management Gateway om de juiste proxy-instellingen gebruiken. U kunt de proxy instellen tijdens de fase van de eerste inschrijving. 

![De proxy instellen tijdens de registratie](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Gateway maakt gebruik van de proxy-server verbinding maken met de service cloud. Klik op koppeling **wijzigen** tijdens setup. U ziet het dialoogvenster **proxy-instellingen** .

![Set proxy met config-manager](media/data-factory-data-management-gateway/SetProxySettings.png)

Er zijn drie opties: 

- **Gebruik geen proxy**: Gateway een proxy niet expliciet gebruikt voor verbinding met de cloud services.
- **Systeem-proxy gebruiken**: Gateway maakt gebruik van de proxy-instelling die is geconfigureerd in de diahost.exe.config.  Als er geen proxyserver is geconfigureerd in diahost.exe.config, verbindt de gateway naar cloud-service rechtstreeks zonder proxy.
- **Aangepaste proxy gebruiken**: de HTTP-proxy-instelling voor de gateway in plaats van configuraties in diahost.exe.config configureren.  Adres en poort zijn vereist.  Gebruikersnaam en wachtwoord zijn optioneel afhankelijk van de instelling voor verificatie van de proxy.  Alle instellingen zijn gecodeerd met een referentiecertificaat van de gateway en lokaal opgeslagen op de hostcomputer voor de gateway.

De Host Data Management Gateway-Service automatisch opnieuw gestart nadat u de bijgewerkte proxy-instellingen opgeslagen. 

Nadat de gateway is geregistreerd, als u wilt weergeven of bijwerken van de proxy-instellingen, gebruikt u Data Management Gateway Configuration Manager. 

1. Data Management Gateway Configuration Manager starten.
2. Ga naar het tabblad **Instellingen** .
3. Klik op koppeling **wijzigen** in het gedeelte **HTTP Proxy** voor het starten van het dialoogvenster met **De HTTP-Proxy instellen** .  
4. Nadat u op **volgende** klikt, ziet u een waarschuwingsdialoogvenster waarin u wordt gevraagd uw toestemming voor de proxy-instellingen opslaan en de Gateway-Host-Service opnieuw starten.

U kunt bekijken en bijwerken van HTTP-proxy met Configuration Manager-hulpprogramma. 

![Set proxy met config-manager](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [AZURE.NOTE] Als u een proxy-server met NTLM-verificatie is Gateway Host-Service wordt uitgevoerd onder de domeinaccount. Als u het wachtwoord voor de domeinaccount later wijzigt, moet u configuratie-instellingen voor de service bijwerken en dienovereenkomstig opnieuw is opgestart. Vanwege deze vereiste, stellen we voor u een speciale account voor toegang tot de proxy-server waarvoor u het wachtwoord regelmatig bijwerken.

### <a name="configure-proxy-server-settings-in-diahostexeconfig"></a>Instellingen van proxyserver configureren in diahost.exe.config
Als u **de proxy-systeem gebruiken** voor de HTTP-proxy, gateway maakt gebruik van de proxy-instellingen in diahost.exe.config.  Als er geen proxyserver is opgegeven in diahost.exe.config, verbindt de gateway naar cloud-service rechtstreeks zonder proxy. De volgende procedure bevat instructies voor het bijwerken van het configuratiebestand. 

1.  In File Explorer, een veilige kopie van C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config back-up van het oorspronkelijke bestand te maken.
2.  Notepad.exe wordt uitgevoerd als beheerder starten en openen van bestand 'C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. U vindt het standaardlabel voor system.net zoals in de volgende code:

            <system.net>
                <defaultProxy useDefaultCredentials="true" />
            </system.net>   

    Vervolgens kunt u proxy server-gegevens toevoegen, zoals in het volgende voorbeeld wordt getoond:

            <system.net>
                  <defaultProxy enabled="true">
                        <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
                  </defaultProxy>
            </system.net>

    Aanvullende eigenschappen zijn toegestaan in de proxy-tag om de vereiste instellingen zoals scriptLocation. Raadpleeg de [proxy Element (netwerkinstellingen)](https://msdn.microsoft.com/library/sa91de1e.aspx) op de syntaxis.

            <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>

3. Het configuratiebestand opslaan in de oorspronkelijke locatie en de Data Management Gateway-Host-service de wijzigingen neemt opnieuw starten. De service opnieuw te starten: de toepassing services in het Configuratiescherm of vanaf de **Data Management Gateway Configuration Manager** gebruiken > Klik op de knop **-Service stoppen** en vervolgens klikt u op de **Service starten**. Als de service niet wordt gestart, is het waarschijnlijk dat een onjuiste syntaxis van XML-code is toegevoegd in het configuratiebestand van de toepassing die is bewerkt.     

Naast deze punten moet u ook controleren of dat Microsoft Azure is in de "witte" lijst van uw bedrijf. De lijst met geldige Microsoft Azure IP-adressen kan worden gedownload vanaf het [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mogelijke symptomen voor firewall en proxy server problemen
Als u op problemen overeen met de volgende procedures stuit, wordt het veroorzaakt door een onjuiste configuratie van de firewall of proxy server gateway verbinding kunnen maken met Data Factory blokkeert zichzelf verifiëren. Verwijzen naar de vorige sectie om ervoor te zorgen uw firewall en proxy-server correct zijn geconfigureerd.

1.  Wanneer u voor het registreren van de gateway, het volgende foutbericht: 'kan niet registreren van de gateway-sleutel. Voordat u probeert om te registreren om de sleutel gateway opnieuw bevestigen dat de Data Management Gateway verbonden is en de Host Data Management Gateway-Service is gestart."
2.  Wanneer u Configuratiebeheer opent, ziet u status als "Verbinding verbroken" of "Verbinding maken". Bij het bekijken van gebeurtenislogboeken van Windows, onder 'Logboeken' > 'Toepassingen en Services Logs' > 'Data Management-Gateway', u ziet foutberichten weergegeven zoals de volgende fout:`Unable to connect to the remote server` 
    `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Open poort 8050 voor referentie-codering 
De **Referenties van de instelling** maakt de binnenkomende poort **8050** relay referenties naar de gateway bij het instellen van een bedrijf op service in Azure portal gekoppeld. Tijdens de installatie van gateway standaard de Data Management Gateway installatie geopend op de gateway-computer.
 
Als u een firewall van derden gebruikt, kunt u de 8050-poort handmatig openen. Als u tijdens de installatie van gateway firewall probleem ondervindt, kunt u proberen met de volgende opdracht om de gateway installeren zonder het configureren van de firewall.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Als u niet de 8050-poort op de gateway-machine openen, gebruiken andere mechanismen dan de data store-referenties te configureren met behulp van de **Referenties van de instelling van** toepassing. U kunt bijvoorbeeld de PowerShell-cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) . Zie de sectie [referenties voor instelling en beveiliging](#set-credentials-and-securityy) op hoe de referenties voor het opslaan van gegevens kan worden ingesteld.

## <a name="update"></a>Update 
Data Management Gateway wordt standaard automatisch bijgewerkt wanneer er een nieuwere versie van de gateway beschikbaar is. De gateway wordt niet bijgewerkt totdat de geplande taken kunt uitvoeren. Geen verdere taken worden door de gateway worden verwerkt totdat de update is voltooid. Als de update mislukt, wordt gateway teruggedraaid naar de oude versie. 

De geplande update tijd ziet u op de volgende plaatsen:

- De gateway eigenschappen blade in Azure portal.
- Introductiepagina van de Data Management Gateway Configuration Manager
- Melding van System tray. 

Het tabblad Start van de Data Management Gateway Configuration Manager geeft de updateplanning en de laatste keer dat de gateway is geïnstalleerd/bijgewerkt. 

![Updates plannen](media/data-factory-data-management-gateway/UpdateSection.png)

U kunt de update onmiddellijk te installeren of wachten op de gateway worden automatisch bijgewerkt op het geplande tijdstip. Bijvoorbeeld, de volgende afbeelding ziet u de melding die wordt weergegeven in de Gateway Configuration Manager met de knop bijwerken waarop u klikken kunt om onmiddellijk te installeren. 

![Update DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

De melding in het systeemvak eruit zoals in de volgende afbeelding wordt getoond: 

![Systeembericht lade](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

U ziet dat de status van de bijwerkbewerking (handmatig of automatisch) in het systeemvak. Wanneer u Gateway Configuration Manager volgende keer start, ziet u een bericht in het systeemvak worden weergegeven dat de gateway en een koppeling naar [Wat is nieuw onderwerp](data-factory-gateway-release-notes.md)is bijgewerkt.

### <a name="to-disableenable-auto-update-feature"></a>Met de functie voor automatisch bijwerken inschakelen/uitschakelen
U kunt uitschakelen of inschakelen de functie auto-update door de volgende stappen uit te voeren: 

1. Start Windows PowerShell op de gateway-computer. 
2. Ga naar de map C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript.
3. Uitvoeren met de volgende opdracht om de auto-update functie uit te schakelen.   

        .\GatewayAutoUpdateToggle.ps1  -off

4. Om weer in te schakelen: 
    
        .\GatewayAutoUpdateToggle.ps1  -on  

## <a name="configuration-manager"></a>Configuratiebeheer 
Als u de gateway hebt geïnstalleerd, kunt u Data Management Gateway Configuration Manager op een van de volgende manieren starten: 

- Typ in **het zoekvenster** **Data Management Gateway** voor toegang tot dit hulpprogramma. 
- Uitvoeren van het uitvoerbare bestand **ConfigManager.exe** in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared** 
 
### <a name="home-page"></a>Startpagina
De introductiepagina kunt u de volgende acties uitvoeren: 

- De status weergeven van de gateway (verbonden met de service cloud enz.). 
- **Registreren** met behulp van een sleutel van de portal.
- **Stop** en start de **service Data Management Gateway-Host** op de gateway-computer.
- **Updates plannen** op een bepaald tijdstip van de dagen.
- De datum weergeven waarop de gateway **voor het laatst bijgewerkt is**. 

### <a name="settings-page"></a>Pagina-instellingen
De pagina-instellingen kunt u de volgende acties uitvoeren:

- Weergeven, wijzigen en exporteren van **certificaten** die door de gateway wordt gebruikt. Dit certificaat wordt gebruikt voor het coderen van gegevensbron referenties.
- **HTTPS-poort** wijzigen voor het eindpunt. De gateway een poort voor het instellen van de referenties van data source wordt geopend. 
- **De status** van het eindpunt
- Weergave **SSL-certificaat** wordt gebruikt voor SSL-communicatie tussen portal en de gateway naar referenties voor gegevensbronnen instellen.  

### <a name="diagnostics-page"></a>De pagina Diagnostische gegevens
Op de pagina Diagnostische gegevens kunt u de volgende acties uitvoeren:

- Uitgebreide **logboekregistratie**inschakelen, logboekbestanden weergeven in Logboeken en logboeken naar Microsoft verzonden als er een fout is.
- **Verbinding testen** met een gegevensbron.  

### <a name="help-page"></a>Help-pagina
De Help-pagina bevat de volgende informatie:  

- Korte beschrijving van de gateway
- Versienummer
- Koppelingen naar de on line help, privacyverklaring en gebruiksrechtovereenkomst.  

## <a name="troubleshooting"></a>Het oplossen van problemen

- U vindt gedetailleerde informatie in het vak gateway worden vastgelegd in de gebeurtenislogboeken van Windows. U vindt deze met behulp van Windows **Logboekinzage** onder **toepassings- en servicelogboeken** > **Data Management Gateway**. Bij het oplossen van problemen met de gateway, zoek foutgebeurtenissen niveau in de gebeurtenis viewer.
- Als de gateway niet meer werkt nadat u **het certificaat wijzigen**, start u de **Data Management Gateway Service** met het hulpprogramma Microsoft Data Management Gateway Configuration Manager of Services in het Configuratiescherm. Als u nog steeds een fout ziet, moet u wellicht de expliciete machtigingen voor de gebruiker gegevens Management Gateway service voor toegang tot het certificaat in het beheer van certificaten (certmgr.msc).  De standaard-gebruikersaccount voor de service is: **NT-Service\DIAHostService**. 
- Als de toepassing **Referentiebeheer** niet **coderen van** referenties wanneer u klikt op knop coderen in Data Factory-Editor, controleert u of u deze toepassing uitvoert op de **gateway-computer**. Als dit niet het geval is, de toepassing uitvoeren op de gateway-machine en probeer het coderen van referenties.  
- Als gegevensarchief verbinding of fouten met betrekking tot stuurprogramma wordt weergegeven, moet u de volgende stappen uitvoeren: 
    - **Data Management Gateway Configuration Manager** starten op de gateway-computer.
    - Ga naar het tabblad **Diagnostische gegevens**
    - Selecteer/Geef de gewenste waarden voor de velden in de groep **Test verbinding met een gegevensbron op ruimten is met deze gateway**
    - Klik op **verbinding testen** om te zien of u verbinding kunt maken met de gegevensbron voor de lokalen van de gateway-computer met behulp van de verbindingsgegevens en referenties. Als u de verbinding test nog steeds niet nadat u een stuurprogramma hebt geïnstalleerd, start opnieuw op de gateway voor het afhalen van de laatste wijziging.  

    ![Verbinding testen](./media/data-factory-data-management-gateway/TestConnection.png)

### <a name="send-gateway-logs-to-microsoft"></a>Gateway-logboeken naar Microsoft verzenden
Wanneer u contact opnemen met Microsoft Support voor hulp bij het oplossen van problemen met gateway, kan u voor het delen van uw gateway-logboeken worden gevraagd. De release van de gateway kunt u gemakkelijk delen vereist gateway Logboeken door middel van twee muisklikken Gateway Configuration Manager.   

1. Ga naar het tabblad **Diagnostische gegevens** van gateway configuration manager.
 
    ![Data Management Gateway - tabblad Diagnostische gegevens](media/data-factory-data-management-gateway/data-management-gateway-diagnostics-tab.png)
2. Klik op koppeling **verzenden logboeken** om te zien het volgende dialoogvenster: 

    ![Data Management Gateway - logboeken verzenden](media/data-factory-data-management-gateway/data-management-gateway-send-logs-dialog.png)
3. (optioneel) Klik op **weergave logboeken** de logboeken bekijken.
4. (optioneel) Klik op **privacy** voor de privacyverklaring van Microsoft online services. 
3. Als u tevreden bent met wat u gaat uploaden, klikt u op **verzenden logboeken** daadwerkelijk logboeken van de afgelopen zeven dagen naar Microsoft verzenden voor het oplossen van problemen. Hier ziet u de status van de bewerking verzenden logboeken zoals in de volgende afbeelding wordt getoond:

    ![Data Management Gateway - verzenden registreert status](media/data-factory-data-management-gateway/data-management-gateway-send-logs-status.png)
4. Zodra de bewerking voltooid is, wordt een dialoogvenster weergegeven zoals in de volgende afbeelding wordt getoond:
    
    ![Data Management Gateway - verzenden registreert status](media/data-factory-data-management-gateway/data-management-gateway-send-logs-result.png)
5. Noteren op de **lijst-ID** en deze delen met Microsoft Support. Lijst-ID wordt gebruikt om de gateway logboeken die u hebt geüpload voor het oplossen van problemen te vinden.  De lijst-ID wordt ook opgeslagen in de logboeken voor uw referentie.  U kunt vinden door te kijken naar de gebeurtenis-ID "25" en controleer de datum en tijd.
    
    ![Data Management Gateway - verzenden registreert een lijst-ID](media/data-factory-data-management-gateway/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Archief gateway aanmeldt gateway-host-computer
Er zijn bepaalde scenario's waarbij u problemen gateway en kunt u niet delen logboeken gateway rechtstreeks: 

- U handmatig de gateway installeren en registreren van de gateway;
- U probeert te registreren van de gateway met een sleutel geregenereerde op Configuratiebeheer; 
- U probeert te verzenden, logboeken en de gateway-host-service kan niet worden verbonden;

In dergelijke gevallen kan gateway Logboeken opslaan als een zip-bestand en deze wanneer u later contact opnemen met Microsoft support. Bijvoorbeeld als u een foutbericht ontvangt bij het registreren van de gateway als weergegeven in de volgende afbeelding:   

![Data Management Gateway - fout](media/data-factory-data-management-gateway/data-management-gateway-registration-error.png)

Klik op **archief gateway** logboeken koppeling om te archiveren en opslaan van Logboeken en het zip-bestand vervolgens delen met Microsoft support. 

![Data Management Gateway - logboeken archiveren](media/data-factory-data-management-gateway/data-management-gateway-archive-logs.png)

### <a name="gateway-is-online-with-limited-functionality"></a>Gateway is on line met beperkte functionaliteit 
Ziet u de status van de gateway als **on line met beperkte functionaliteit** voor een van de volgende redenen.

- Gateway verbinding geen met de cloud-service via service bus.
- Cloud-service kan geen verbinding maken via service bus-gateway.

Wanneer een gateway is online met beperkte functionaliteit, kunt u mogelijk niet gebruik van de Wizard fabriek kopie maken van gegevens van pijpleidingen voor het kopiëren van gegevens van gegevensopslag voor gebouwen.

Resolutie/oplossing voor dit probleem (online met beperkte functionaliteit) is gebaseerd op of de gateway geen verbinding met cloud-service of de andere manier maken. De volgende secties vindt u deze oplossingen. 

#### <a name="gateway-cannot-connect-to-cloud-service-through-service-bus"></a>Gateway geen verbinding maken met de cloud-service via bus service
Ga als volgt te werk als u weer on line de gateway: 

1. Uitgaande poorten 9350 9354 op zowel de Windows Firewall op computer gateway en Firewall van het bedrijf inschakelen. Zie [poorten en firewall](#ports-and-firewall) sectie voor details.
2. Proxy-instellingen configureren op de gateway. Zie de sectie [Overwegingen voor Proxy server](#proxy-server-considerations) voor details. 

Als tijdelijke oplossing Data Factory Editor in Azure portal (of) Visual Studio (of) Azure PowerShell te gebruiken.

#### <a name="error-cloud-service-cannot-connect-to-gateway-through-service-bus"></a>Fout: Cloud-service kan geen verbinding maken via service bus-gateway.
Ga als volgt te werk als u weer on line de gateway:
 
1. Uitgaande poorten 5671 en 9350-9354 op zowel de Windows Firewall op computer gateway en Firewall van het bedrijf inschakelen. Zie [poorten en firewall](#ports-and-firewall) sectie voor details.
2. Proxy-instellingen configureren op de gateway. Zie de sectie [Overwegingen voor Proxy server](#proxy-server-considerations) voor details.
3. Statische IP-beperking op de proxyserver te verwijderen. 

Als tijdelijke oplossing kunt u Data Factory Editor in Azure portal (of) Visual Studio (of) Azure PowerShell.
 
## <a name="move-gateway-from-a-machine-to-another"></a>Gateway van een computer naar de andere verplaatsen
Deze sectie bevat stappen voor bewegende gateway-client vanaf een computer naar een andere computer. 

2. Ga naar de **introductiepagina van Data Factory**in de portal en klik op de tegel **Gekoppelde Services** . 

    ![Gegevenskoppeling Gateways](./media/data-factory-data-management-gateway/DataGatewaysLink.png) 
3. Selecteer uw gateway in de sectie **GEGEVENSGATEWAYS** van de bladeserver **Gekoppelde Services** .
    
    ![Gekoppelde Services blade met gateway geselecteerd](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
4. Klik op **downloaden en installeren van de gegevensgateway**in het blad **gegevensgateway** .
    
    ![Gateway koppeling downloaden](./media/data-factory-data-management-gateway/DownloadGatewayLink.png) 
5. Klik op **downloaden en installeren van gegevensgateway**in de blade **configureren** en volgt u de instructies voor het installeren van de gegevensgateway op de computer. 

    ![Blade configureren](./media/data-factory-data-management-gateway/ConfigureBlade.png)
6. De **Microsoft Data Management Gateway Configuration Manager** open te houden. 
 
    ![Configuratiebeheer](./media/data-factory-data-management-gateway/ConfigurationManager.png) 
7. In het blad **configureren** in de portal, klik op **sleutel opnieuw maken** op de opdrachtbalk en klik op **Ja** om het waarschuwingsbericht. Klik op de **knop kopiëren** naast tekst die de sleutel naar het Klembord gekopieerd. De gateway op de oude computer uitvalt als spoedig u de sleutel opnieuw maken.  
    
    ![Sleutel opnieuw maken](./media/data-factory-data-management-gateway/RecreateKey.png)
     
8. De **sleutel** in het tekstvak op de pagina **Registreren Gateway** van de **Data Management Gateway Configuration Manager** op uw computer plakken. (optioneel) Klik op overzicht van de belangrijkste tekst selectievakje **gateway sleutel wordt weergegeven** . 
 
    ![Kopie sleutel en kassa](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
9. Klik op **registreren** om u te registreren van de gateway bij de service cloud.
10. Op het tabblad **Instellingen** en klik op **wijzigen** om hetzelfde certificaat dat werd gebruikt met de oude gateway selecteert, voert u het **wachtwoord**en klik op **Voltooien**. 
 
    ![Certificaat opgeven](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

    U kunt een certificaat exporteren uit de oude gateway door de volgende stappen uit te voeren: Data Management Gateway Configuration Manager op de oude computer starten, overschakelen naar het tabblad **certificaat** , klikt u op de knop **exporteren** en volg de instructies. 
10. Na de registratie van de gateway is voltooid ziet u de **registratie** ingesteld op **geregistreerde** en **Status** op **gestart** op de introductiepagina van de Gateway Configuration Manager. 

## <a name="encrypting-credentials"></a>Coderen van referenties 
Referenties wilt coderen in de Editor voor Factory, voer de volgende stappen uit:

1. Start de webbrowser op de **gateway-computer**, gaat u naar de [portal Azure](http://portal.azure.com). De fabriek gegevens indien nodig, open data factory genoemd in de blade **DATA FACTORY** zoekt en klik vervolgens op **auteur & implementeren** om Data Factory-Editor te starten.   
1. Klik op een bestaande **gekoppelde service** in de structuurweergave de JSON-definitie zien of een gekoppelde service waarvoor een Data Management Gateway maken (bijvoorbeeld: SQL Server of Oracle). 
2. Voer de naam van de gateway in de JSON-editor voor de eigenschap **gatewayName** . 
3. Servernaam invoeren voor de eigenschap **Data Source** in de **connectionString**.
4. Databasenaam invoeren voor de eigenschap **Initial Catalog** in de **connectionString**.    
5. Klik op de knop **coderen** op de opdrachtbalk die het klikken op Start-eenmaal **Referentiebeheer** toepassing. Hier ziet u het dialoogvenster **Referenties instellen** . 
    ![De dialoogvenster referenties instellen](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
6. Voer de volgende stappen uit in het dialoogvenster **Referenties instellen** :  
    1.  **Verificatie** dat u wilt dat de service Data Factory gebruiken om verbinding met de database te selecteren. 
    2.  Voer de naam van de gebruiker die toegang tot de database voor de instelling van de **gebruikersnaam heeft** . 
    3.  Wachtwoord voor de gebruiker voor het **wachtwoord** invoeren.  
    4.  Klik op **OK** om te coderen van referenties en het dialoogvenster te sluiten. 
5.  U ziet nu een **encryptedCredential** -eigenschap in de **connectionString** .      
        
            {
                "name": "SqlServerLinkedService",
                "properties": {
                    "type": "OnPremisesSqlServer",
                    "description": "",
                    "typeProperties": {
                        "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                        "gatewayName": "adftutorialgateway"
                    }
                }
            }

Als u toegang de portal vanaf een computer die verschilt van de gateway-computer tot, moet u ervoor zorgen dat de Manager van referenties toepassing verbinding met de gateway-computer maken kan. Als de toepassing niet kan van de gateway-machine bereiken, kunt niet u de referenties voor de gegevensbron instellen en testen van de verbinding met de gegevensbron.  

Wanneer u de **Referenties van de instelling van** toepassing, codeert de portal de referenties met het certificaat dat is opgegeven op het tabblad van het **certificaat** van de **Gateway Configuration Manager** op de gateway-computer. 

Als u een API-gebaseerde aanpak voor het coderen van de referenties zoekt, kunt u de PowerShell-cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) voor het coderen van referenties. Het certificaat wordt gebruikt voor de cmdlet die gateway wordt geconfigureerd voor het gebruik voor het coderen van de referenties. U toevoegen gecodeerde referenties aan het element **EncryptedCredential** van de **connectionString** in de JSON. U gebruikt de JSON met de cmdlet [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) of in de Editor voor Factory. 

    "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

Er is één meer aanpak voor het instellen van referenties met behulp van Data Factory-Editor. Als u een gekoppelde SQL-Server-service maken met behulp van de editor en u referenties als tekst zonder opmaak invoeren, worden de referenties gecodeerd met een certificaat waarin de Data Factory-service. Maakt geen gebruik van het certificaat dat de gateway is geconfigureerd voor gebruik. Deze benadering kan een beetje sneller in sommige gevallen zijn, is maar het minder veilig. Daarom is het raadzaam dat u deze methode alleen voor ontwikkelings-/ testfase volgen. 


## <a name="powershell-cmdlets"></a>PowerShell-cmdlets 
Deze sectie wordt beschreven hoe u kunt maken en registreren van een gateway met Azure PowerShell-cmdlets. 

1. **Azure PowerShell** starten in de beheerdersmodus. 
2. Log in op uw account Azure door de volgende opdracht wordt uitgevoerd en uw Azure referenties in te voeren. 

    Inloggen AzureRmAccount
2. Gebruik de cmdlet **New-AzureRmDataFactoryGateway** voor het maken van een logische gateway als volgt:

        $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>

    **Voorbeeld van de opdracht en de uitvoer**:


        PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

        Name              : MyGateway
        Description       : gateway for walkthrough
        Version           :
        Status            : NeedRegistration
        VersionStatus     : None
        CreateTime        : 9/28/2014 10:58:22
        RegisterTime      :
        LastConnectTime   :
        ExpiryTime        :
        ProvisioningState : Succeeded
        Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI

    
4. In Azure PowerShell, Ga naar de map: * *C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**. Uitvoeren van * *RegisterGateway.ps1* * die is gekoppeld aan de lokale variabele * *$Key** zoals in de volgende opdracht. Dit script registreert de agent van de client geïnstalleerd op uw computer met de logische gateway die u eerder hebt gemaakt.

        PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
        
        Agent registration is successful!

    Met de parameter IsRegisterOnRemoteMachine kunt u de gateway op een externe computer registreren. Voorbeeld:
        
        .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true

5. U kunt de cmdlet **Get-AzureRmDataFactoryGateway** gebruiken om de lijst met Gateways in uw fabriek gegevens. Wanneer de **Status** **on line**ziet, betekent dit dat uw gateway gereed voor gebruik.

        Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF

U kunt een gateway met de cmdlet **Verwijderen AzureRmDataFactoryGateway** verwijderen en werk de beschrijving voor een gateway met behulp van de **Set-AzureRmDataFactoryGateway** -cmdlets. Zie voor de syntaxis en andere details van deze cmdlets Data Factory Cmdlet referentie.  

### <a name="list-gateways-using-powershell"></a>Lijst met gateways met PowerShell

    Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup

### <a name="remove-gateway-using-powershell"></a>Gateway met PowerShell verwijderen
    
    Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force 


## <a name="next-steps"></a>Volgende stappen
- Zie [gegevens verplaatsen tussen lokale en cloud opgeslagen gegevens](data-factory-move-data-between-onprem-and-cloud.md) artikel. In het overzicht maakt u een pijpleiding die de gateway gebruikt om gegevens te verplaatsen vanuit een SQL Server-database op de gebouwen aan een Azure blob.  
