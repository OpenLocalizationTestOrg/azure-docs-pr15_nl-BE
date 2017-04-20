<properties
   pageTitle="Logica Apps in de ruimten gegevensgateway installeren | Microsoft Azure"
   description="Informatie over het installeren van de gateway van de gegevens in de lokalen voor gebruik in een app logica."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>De gateway van de gegevens in de lokalen voor logica Apps installeren

## <a name="installation-and-configuration"></a>Installatie en configuratie

### <a name="prerequisites"></a>Vereisten

Minimum:

* 4.5 .NET framework
* 64-bits versie van Windows 7 of Windows Server 2008 R2 (of hoger)

Aanbevolen:

* 8 core CPU
* 8 GB geheugen
* 64-bits versie van Windows 2012 R2 (of hoger)

Verwante overwegingen:

* U kunt een gateway niet installeren op een domeincontroller.
* U moet niet een gateway installeren op een computer, die een laptop die kan worden uitgeschakeld, asleep, of niet verbonden met het Internet, omdat de gateway kan niet worden uitgevoerd onder een van deze omstandigheden. Bovendien gateway prestaties kan ten koste gaan via een draadloos netwerk.

### <a name="install-a-gateway"></a>Een gateway installeren

U kunt het [installatieprogramma voor de op het bedrijf gegevensgateway hier](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409)opvragen.

**On-premises gegevensgateway** opgeven als de modus, meld u aan met uw werk of school-account en een nieuwe gateway configureren of migreren, herstellen, of overnemen van een bestaande gateway.

* Typ een **naam** en een **herstelsleutel**gateway configureren, en klik op of klik op **configureren**.

    Geef een herstelsleutel die ten minste acht tekens bevat en op een veilige plaats bewaren. U moet deze sleutel op als u wilt migreren, herstellen of te nemen via de gateway.

* Als u wilt migreren, herstellen of overnemen van een bestaande gateway, bieden de herstelsleutel die is opgegeven bij het maken van de gateway.

### <a name="restart-the-gateway"></a>De gateway opnieuw starten

De gateway wordt uitgevoerd als een Windows-service in en net als bij elke andere Windows-service kunt u starten en stoppen op verschillende manieren. U kunt bijvoorbeeld open een opdrachtprompt met verhoogde bevoegdheden op de computer waarop de gateway wordt uitgevoerd en voer vervolgens een van deze opdrachten:

* Als u de service stopt, kunt u deze opdracht uitvoert:

    `net stop PBIEgwService`

* De service wilt starten, moet u deze opdracht uitvoert:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Een firewall of proxyserver configureren

Zie voor meer informatie over hoe u informatie voor uw gateway proxy [proxy-instellingen configureren](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

U kunt controleren of uw firewall of proxy, dat verbindingen blokkeert mogelijk met de volgende opdracht uitvoeren vanaf een PowerShell-prompt. Hierdoor wordt de verbinding met de Azure Service Bus getest. Dit is alleen de netwerkverbinding gecontroleerd en heeft niets te maken met de cloud server-service of de gateway. Het helpt om te bepalen of uw computer daadwerkelijk kom uit met het internet.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Het resultaat moeten uitzien in het volgende voorbeeld. Als **TcpTestSucceeded** de waarde true is, wordt u mogelijk geblokkeerd door een firewall.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Als u volledig wilt, vult u de **computernaam** en het **poortnummer** waarden die worden vermeld onder [poorten configureren](#configure-ports) verderop in dit onderwerp.

De firewall ook mogelijk de verbindingen blokkeert de Azure Service Bus in die de Azure datacenters. Als dit het geval is, zult u "witte" lijst (blokkering opheffen) alle IP-adressen voor uw regio voor de datacenters. U kunt een lijst met [hier Azure IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Poorten configureren

De gateway maakt een uitgaande verbinding Azure Service bus. Deze communiceert op uitgaande poorten: TCP-443 (standaard), 5671, 5672, 9350 tot 9354. De gateway nodig niet poorten voor binnenkomend verkeer.

Meer informatie over [hybride oplossingen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMEINNAMEN | UITGAANDE POORTEN | BESCHRIJVING |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | HTTPS |
| *. login.windows.net | 443 | HTTPS |
| *. servicebus.windows.net |5671 5672 | Geavanceerde Message Queuing-Protocol (AMQP) |
| *. servicebus.windows.net | 443, 9350 9354 | Listeners op Bus-Relay-Service via TCP (443 voor toegangsbeheer token aanschaf vereist) |
| *. frontend.clouddatahub.net | 443 | HTTPS |
| *. core.windows.net | 443 | HTTPS |
| Login.microsoftonline.com | 443 | HTTPS |
| *. msftncsi.com | 443 | Internet-verbindingen te testen als de gateway onbereikbaar voor de Power BI-service is gebruikt. |

Als u op de witte lijst IP-adressen in plaats van de domeinen, kunt u downloaden en gebruiken de [lijst Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653). In sommige gevallen wordt de Azure Service Bus-verbindingen worden gemaakt met het IP-adres in plaats van de FQDN-namen.

### <a name="sign-in-account"></a>Account

Gebruikers zich aanmelden met een een werk of school account. Dit is de rekening van uw organisatie. Als u aangemeld voor een Office 365-aanbod en niet uw e-mailadres van werkelijke hoeveelheid werk opgeeft, het lijkt jeff@contoso.onmicrosoft.com. Uw account binnen een cloud-service is opgeslagen in een huurder in Azure Active Directory (AAD). In de meeste gevallen van uw account AAD UPN komt overeen met het e-mailadres.

### <a name="windows-service-account"></a>Windows Service-account

De gegevens op het bedrijf gateway is geconfigureerd voor gebruik van NT SERVICE\PBIEgwService voor de aanmeldingsreferenties van het Windows-service. Standaard heeft het recht van het logboek op als een service. Dit is in het kader van de computer waarop u de gateway installeert.

Dit is niet de account waarmee verbinding met gegevensbronnen in ruimten of het werk of school-account waarmee u zich aanmeldt cloud services.

##<a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="general"></a>Algemeen

**Vraag**: welke gegevensbronnen ondersteuning biedt voor de gateway?<br/>
**Antwoord**: op dit schrijven, SQL Server.

**Vraag**: moet ik een gateway voor gegevensbronnen in de cloud, zoals SQL Azure? <br/>
**Antwoord**: Nee. Een gateway verbinding met gegevensbronnen alleen voor bedrijven.

**Vraag**: Wat is de werkelijke Windows-service genoemd?<br/>
**Antwoord**: In diensten, de gateway Power BI Enterprise Gateway Service genoemd.

**Vraag**: zijn er geen inkomende verbindingen met de gateway van de cloud? <br/>
**Antwoord**: Nee. De gateway wordt gebruikt voor uitgaande verbindingen naar Azure Service Bus.

**Vraag**: Wat gebeurt er als ik uitgaande verbindingen blokkeren? Wat heb ik nodig om te openen <br/>
**Antwoord**: Zie de poorten en de hosts die de gateway wordt gebruikt.


**Vraag**: de gateway hoeft te worden geïnstalleerd op dezelfde computer als de gegevensbron? <br/>
**Antwoord**: Nee. De gateway verbinding met de gegevensbron met behulp van de informatie die is geleverd. De gateway beschouwen als een clienttoepassing in deze zin. Het moet net kunnen verbinding maken met de naam van de server die is opgegeven.


**Vraag**: Wat is de wachttijd voor het uitvoeren van query's naar een gegevensbron van de gateway? Wat is de beste architectuur? <br/>
**Antwoord**: om netwerkvertraging, de gateway installeren zo dicht mogelijk bij de gegevensbron mogelijk. Als u de gateway op de eigenlijke gegevensbron installeren kunt, zal het de geïntroduceerd Latentie minimaliseren. U kunt ook de datacenters. Als uw service met behulp van het datacenter VS West en SQL Server gehost in een Azure VM hebt, wilt u zult ook Azure VM in westelijk Verenigde Staten is. Dit wordt Latentie minimaliseren en egress toeslagen op de Azure VM voorkomen.


**Vraag**: zijn er vereisten voor bandbreedte van het netwerk? <br/>
**Antwoord**: het wordt aanbevolen om goede doorvoer voor de netwerkverbinding. Elke omgeving anders is en de hoeveelheid verzonden gegevens heeft invloed op de resultaten. ExpressRoute gebruik kan helpen een doorvoersnelheid tussen lokale en de Azure gegevenscentra teneinde te waarborgen.

Om te meten wat de doorvoer is kunt u het hulpprogramma derden Azure snelheid Test app.


**Vraag**: kan de gateway service voor Windows worden uitgevoerd met een Azure Active Directory-account? <br/>
**Antwoord**: Nee. De Windows-service moet een geldige Windows-account hebben. Standaard wordt uitgevoerd met de Service-SID NT SERVICE\PBIEgwService.


**Vraag**: hoe worden verzonden naar de cloud? <br/>
**Antwoord**: dit wordt gedaan in de Azure Service Bus. Zie voor meer informatie hoe het werkt.


**Vraag**: waar zijn mijn referenties opgeslagen? <br/>
**Antwoord**: de referenties die u voor een gegevensbron invoert zijn opgeslagen, versleuteld in de cloud gatewayservice. De referenties zijn gedecodeerd op de gateway op gebouwen.

### <a name="high-availabilitydisaster-recovery"></a>Hoge beschikbaarheid en herstel in noodgevallen

**Vraag**: zijn er plannen voor een hoge beschikbaarheid scenario's met de gateway inschakelen? <br/>
**Antwoord**: dit is op de routekaart, maar er nog geen een tijdlijn.


**Vraag**: welke opties zijn beschikbaar voor noodherstel? <br/>
**Antwoord**: U kunt de herstelsleutel terugzetten of verplaatsen van een gateway. Geef de herstelsleutel tijdens de installatie van de gateway.


**Vraag**: Wat is het voordeel van de herstelsleutel? <br/>
**Antwoord**: het biedt een manier om te migreren of de gateway-instellingen herstellen na een ramp.

### <a name="troubleshooting"></a>Het oplossen van problemen

**Vraag**: waar zijn de logboeken voor gateway? <br/>
**Antwoord**: Zie's verderop in dit onderwerp.


**Vraag**: hoe kan ik zien welke query's worden verzonden naar de gegevensbron in de lokalen? <br/>
**Antwoord**: U kunt query traceren, waaronder de query's worden verzonden. U deze terug naar de oorspronkelijke waarde gedaan bij het oplossen van problemen. Query tracering ingeschakeld laat, wordt de logboeken groter.

U kunt ook extra met de gegevensbron voor tracering query's bekijken. Zo kunt u uitgebreide gebeurtenissen of SQL Profiler voor SQL Server en Analysis Services.

## <a name="how-the-gateway-works"></a>De werking van de gateway

Wanneer een gebruiker communiceert met een element dat verbonden met een gegevensbron op ruimten:

1. De cloud-service maakt u een query, samen met de gecodeerde referenties voor de gegevensbron en de query te verzenden naar de wachtrij voor de gateway worden verwerkt.
1. De service de query analyseert en duwt de aanvraag naar de Azure Service Bus.
1. De gateway op ruimten gegevens opgevraagd Azure Service Bus voor aanvragen in behandeling.
1. De gateway wordt de query decodeert de referenties en verbinding maakt met de gegevensbron(nen) referenties.
1. De gateway stuurt de query voor de gegevensbron voor de uitvoering.
1. De resultaten uit de gegevensbron verzonden naar de gateway en vervolgens naar de cloud-service. De resultaten worden vervolgens gebruikt door de service.

## <a name="troubleshooting"></a>Het oplossen van problemen

### <a name="update-to-the-latest-version"></a>Bijwerken naar de nieuwste versie

Veel problemen kan het oppervlak als de gateway-versie verouderd is.  Het is algemeen verstandig om ervoor te zorgen dat u zich op de meest recente versie.  Als u nog niet de gateway voor een maand of langer bijgewerkt, kunt u rekening houden met de meest recente versie van de gateway installeren en bekijken als u het probleem kunt reproduceren.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Fout: Kan gebruiker niet toevoegen aan groep. (-2147463168 PBIEgwService Prestatielogboekgebruikers)

U kunt deze fout kan optreden als u probeert de gateway installeren op een domeincontroller, wordt niet ondersteund. U moet de gateway op een computer die geen domeincontroller implementeren.

## <a name="tools"></a>Hulpprogramma 's

### <a name="collecting-logs-from-the-gateway-configurator"></a>Verzamelen van Logboeken van de gateway-configurator

U kunt meerdere logboeken voor de gateway verzamelen. Begint altijd met de logboeken!

#### <a name="installer-logs"></a>Installer-logboekbestanden

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Configuratielogboeken

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Enterprise gateway service, Logboeken

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Gebeurtenislogboeken

De logboeken Data Management Gateway en PowerBIGateway zijn onder de **toepassing Logboeken en Services**aanwezig.

### <a name="fiddler-trace"></a>Fiddler traceren

[Fiddler](http://www.telerik.com/fiddler) is een gratis hulpprogramma van Telerik die HTTP-verkeer controleert.  U kunt de achterkant zien en weer met de Power BI-service vanaf de clientcomputer. Dit kan fouten en andere gerelateerde gegevens worden weergegeven.

## <a name="next-steps"></a>Volgende stappen
- [Een op locatie verbinding maken met logica Apps](app-service-logic-gateway-connection.md)
- [Enterprise integration-functies](app-service-logic-enterprise-integration-overview.md)
- [Logica Apps verbindingslijnen](../connectors/apis-list.md)