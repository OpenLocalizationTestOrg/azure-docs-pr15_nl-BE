<properties 
    pageTitle="Met behulp van de hybride Connection Manager | Microsoft Azure" 
    description="Installeren en configureren van de hybride Connection Manager en verbinding maken met verbindingslijnen in logica Apps voor gebouwen" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Verbinding maken met voor bedrijfsruimten verbindingslijnen met behulp van de hybride Connection Manager

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld. Een gateway logica Apps algemene beschikbaarheid (GA) gebruikt voor verbindingen voor gebouwen. Lees meer over de nieuwe [gateway](app-service-logic-gateway-connection.md) en [Logica Apps GA](https://azure.microsoft.com/documentation/services/logic-apps/).

Logica Apps gebruikt voor het gebruik van een systeem van gebouwen, de hybride Connection Manager. Sommige verbindingslijnen kunnen verbinden met een systeem van gebouwen, zoals SQL Server, SAP en SharePoint. 

De hybride Connection Manager (HCM) is een klik-eenmaal installer is geïnstalleerd op een IIS-server in uw netwerk, achter uw firewall. Met behulp van een relay Azure Service Bus HCM wordt geverifieerd door het systeem op locatie met de Connector in Azure. 

> [AZURE.NOTE] Hybride Verbindingsbeheer is alleen vereist als u verbinding met een bron van lokalen achter uw firewall maakt. Als u niet een verbinding naar een systeem op locatie, moet u niet de hybride Connection Manager.

U moet om te beginnen:

- Azure Service Bus relay naamruimte SAS verbindingsreeks. Zie de [Serviceprijzen Bus](https://azure.microsoft.com/pricing/details/service-bus/) om te bepalen welke laag bevat relais.
- Over lokalen systeem-in informatie, inclusief gebruikersnaam en wachtwoord. Bijvoorbeeld, als u verbinding met een SQL-Server op locatie maakt, moet u de SQL Server-aanmeldingsaccount en het wachtwoord.
- Op locatie gegevens, met inbegrip van de naam en de server poort. Bijvoorbeeld als u verbinding met een SQL-Server op locatie maakt, moet u de naam van de SQL-Server en TCP-poortnummer.

## <a name="get-the-service-bus-connection-string"></a>De verbindingsreeks Service Bus ophalen

Kopiëren in de Azure portal de root Bus Service SAS-verbindingsreeks. Deze verbindingsreeks verbindt uw Azure connector met uw systeem op locatie. 

1. Selecteer uw Service Bus-naamruimte en selecteer **Verbindingsgegevens**in [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885):

    ![][SB_ConnectInfo]

2. Kopieer de SAS-verbindingsreeks:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>De hybride Verbindingsbeheer installeren

1. Selecteer de verbindingslijn die u hebt gemaakt in de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Om deze te openen, kunt u selecteren op **Bladeren**, **API Apps**selecteren en selecteert u de verbindingslijn of API App. 
<br/><br/>
De instelling is **onvolledig**bij **Hybride verbindingen**:
<br/>
![][2] 

2. **Hybride verbinding**selecteren. De Service Bus verbindingsreeks die u eerder hebt ingevoerd, wordt weergegeven.
3. De **primaire configuratietekenreeks**kopiëren:
<br/>
![][PrimaryConfigString]

4. U kunt onder **On-Premises hybride Verbindingsbeheer**de hybride Connection Manager downloaden of installeren rechtstreeks vanaf de portal. 
<br/><br/>
Om te installeren direct vanaf de portal gaat naar de IIS-server op lokalen, bladert u naar de portal en selecteer **downloaden en configureren**.
<br/><br/>
De hybride Verbindingsbeheer te downloaden, gaat u naar de IIS-server op locatie en gaat u naar de **ClickOnce-toepassing** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). De installatie wordt automatisch gestart, zodat u deze kunt uitvoeren.

5. De **Primaire configuratietekenreeks** u eerder hebt geplakt (in stap 3) invoeren in het venster **Instellingen voor Listener** en selecteer **installeren**.

Wanneer de installatie is voltooid, de volgende worden weergegeven:
<br/>
![][3] 

Wanneer u opnieuw aan op de connector gaat, is de verbindingsstatus hybride **verbonden**. Mogelijk hebt u de connector te sluiten en opnieuw openen:
<br/>
![][4] 

> [AZURE.NOTE] Als u wilt overschakelen naar de secundaire connection string, de hybride Connection setup opnieuw uitvoeren en voer de **Secundaire configuratietekenreeks**.


## <a name="tcp-ports-and-security"></a>TCP-poorten en beveiliging

Wanneer u een hybride verbinding maakt, wordt een website gemaakt op de IIS-server lokale op gebouwen. De IIS-server kan zich in een DMZ. De eisen van de TCP-poort op de IIS-server zijn:

TCP-poort | Waarom
--- | ---
 | Er zijn geen inkomende TCP-poorten nodig.
9350 - 9354 | Deze poorten worden gebruikt voor gegevensoverdracht. De manager van de relay Service Bus sondes poort 9350 als TCP-verbinding beschikbaar is. Als deze beschikbaar is vervolgens wordt ervan uitgegaan dat poort 9352 ook beschikbaar is. Verkeer gaat via poort 9352. <br/><br/>Uitgaande verbindingen op deze poorten toestaan.
5671 | Wanneer poort 9352 wordt gebruikt voor gegevensverkeer, 5671 poort wordt gebruikt als de het besturingskanaal. <br/><br/>Uitgaande verbindingen op deze poort toegestaan. 
80, 443 | Als poorten 9352 en 5671 niet bruikbaar zijn, worden *vervolgens* de poorten 80 en 443 de terugval poorten gebruikt voor de gegevensoverdracht en het besturingskanaal.<br/><br/>Uitgaande verbindingen op deze poorten toestaan.
Op prem systeempoort | Open de poort die wordt gebruikt door het systeem op het systeem op locatie. SQL Server gebruikt bijvoorbeeld meestal poort 1433. Open dit TCP-poort.

[Achter een Firewall met Bus Service hosting](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Het oplossen van problemen

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>Het oplossen van problemen in gebouwen

1. Bevestig op de IIS-server de rol van het web IIS is geïnstalleerd en alle IIS-services worden gestart.
2. Bevestig de hybride Connection Manager is geïnstalleerd en wordt uitgevoerd op de IIS-server:
 - In IIS-beheer (inetmgr), de website van ***MicrosoftAzureBizTalkHybridListener*** moet worden vermeld en worden uitgevoerd. 
 - Deze website maakt gebruik van de ***HybridListenerAppPool*** die wordt uitgevoerd als *de gebruiker met lokale ingebouwde netwerkserviceaccount* . Deze AppPool moet ook worden gestart.
3. Bevestig dat de connector is geïnstalleerd en wordt uitgevoerd op de IIS-server: 
 - Een website is gemaakt voor de verbindingslijn. Bijvoorbeeld, als u een SQL-connector hebt gemaakt, is er een website van ***MicrosoftSqlConnector_nnn*** . In IIS-beheer (inetmgr), bevestig deze website is vermeld en wordt gestart. 
 - Deze website maakt gebruik van een eigen IIS-toepassingen met de naam ***HybridAppPoolnnn***. Deze AppPool wordt uitgevoerd als *de gebruiker met lokale ingebouwde netwerkserviceaccount* . Deze website en AppPool moeten beide worden gestart. 
 - Bladeren in de lokale-connector. Bijvoorbeeld, als uw website connector poort 6569 gebruikt, gaat u naar http://localhost:6569. Een standaarddocument niet is geconfigureerd om een `HTTP Error 403.14 - Forbidden error` wordt verwacht.
4. Bevestig de TCP-poorten vindt u in dit onderwerp worden geopend in uw firewall.
5. Bekijk de bron- of doel-systeem:
 - Sommige systemen in gebouwen vereist extra afhankelijkheidsbestanden. Als u verbinding met SAP voor gebouwen maakt, moeten er bijvoorbeeld extra SAP bestanden worden geïnstalleerd op de IIS-server.
 - Controleer de verbinding met het systeem met de aanmeldingsaccount. De TCP-poort die wordt gebruikt door het systeem moet zo zijn geopend, zoals poort 1433 voor SQL Server. De aanmeldingsaccount die u hebt ingevoerd in de portal Azure moet toegang hebben tot het systeem.
6. Controleer de gebeurtenislogboeken op fouten op de IIS-server. 
7. Opruimen en de hybride Connection Manager te installeren: 
 - In IIS, moet u handmatig de connector-website en de bijbehorende groep van toepassingen verwijderen. 
 - Hybride Verbindingsbeheer opnieuw uit te voeren en bevestigt dat u het juiste **Primaire configuratietekenreeks** invoert voor de verbindingslijn.



### <a name="in-the-azure-classic-portal"></a>In de klassieke Azure portal

1. Controleer dat de naamruimte Service Bus heeft een **actieve** status.
2. Wanneer u de connector maakt, voert u de verbindingsreeks Service Bus SAS. Geef geen verbindingsreeks van de ACS.


## <a name="faq"></a>FAQ

**Vraag**: Er zijn twee hybride verbinding Managers. Wat is het verschil? 

**Antwoord**: is de [Hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) -technologie hoofdzakelijk door Web-Apps (voorheen websites) en Mobile Apps (voorheen mobiele services gebruikt wordt) verbinding maken met op gebouwen. Deze hybride verbindingen Manager is de eigen [Instellingen](../biztalk-services/integration-hybrid-connection-create-manage.md) en gebruikt een Azure BizTalk-Service (achter de schermen). TCP- en HTTP-protocollen ondersteunt.

Met Azure App Service-connectors hebben we ook een hybride versie van Verbindingsbeheer.  Deze hybride Verbindingsbeheer wordt *niet* gebruikt een Azure BizTalk-Service (achter de schermen) en ondersteunt meer dan de protocollen TCP en HTTP. Zie de [verbindingslijnen en API Apps lijst](app-service-logic-connectors-list.md).

Beide Azure Service Bus met verbinding maken met het systeem op locatie.

**Vraag**: wanneer ik een aangepaste API App maakt, kan ik met App Service hybride Connection Manager verbinding maken met op ruimten? 

**Antwoord**: niet in de traditionele zin. U kunt een ingebouwde connector te gebruiken, App Service hybride Connection Manager verbinding maken met het systeem op locatie configureren. Vervolgens gebruikt u deze connector met uw aangepaste API App, eventueel met behulp van een App logica. Op dit moment kunt ontwikkelen of maak uw eigen hybride App API (zoals de SQL-connector of de connector bestand).

Als uw aangepaste API een poort TCP of HTTP gebruikt, kunt u [Hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en de hybride Connection Manager. In dit scenario wordt wordt een Azure BizTalk-Service gebruikt. [Verbinden met SQL Server in de ruimten van een web app](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) kan helpen.  


## <a name="read-more"></a>Lees meer

[De logica Apps controleren](app-service-logic-monitor-your-logic-apps.md)<br/>
[Prijzen van Bus-service](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
