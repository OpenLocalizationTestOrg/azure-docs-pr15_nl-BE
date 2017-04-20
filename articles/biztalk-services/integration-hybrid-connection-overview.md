<properties
    pageTitle="Hybride verbindingen-overzicht | Microsoft Azure"
    description="Meer informatie over hybride verbindingen, beveiliging, TCP-poorten en ondersteunde configuraties. MAB, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Hybride verbindingen-overzicht
Inleiding tot hybride verbindingen, ondersteunde configuraties, en de vereiste TCP-poorten.


## <a name="what-is-a-hybrid-connection"></a>Wat is een hybride verbinding

Hybride verbindingen zijn een functie van Azure BizTalk-Services. Hybride verbindingen bieden een eenvoudige en handige manier verbinding maken met het Web Apps-functies in Azure App-Service (voorheen Websites) en de Mobile Apps in Azure App Service (voorheen Mobile Services) bronnen voor lokalen achter uw firewall.

![Hybride verbindingen][HCImage]

Voordelen van hybride verbindingen zijn:

- Web Apps en Apps voor mobiele toegang tot bestaande gegevens op ruimten en diensten veilig.
- Meerdere Web Apps of mobiele Apps kunt delen een hybride verbinding voor toegang tot een bron op gebouwen.
- Minimale TCP-poorten zijn vereist voor toegang tot uw netwerk.
- Toepassingen met behulp van hybride verbindingen via de hybride verbinding toegang krijgen tot de specifieke op gebouwen-resource die wordt gepubliceerd.
- Kan verbinding maken met elke bron in de lokalen die gebruikmaakt van een statisch TCP-poort, zoals SQL Server, MySQL, HTTP-Web-API's en de meeste aangepaste Web Services.

    > [AZURE.NOTE] TCP-services die gebruikmaken van dynamische poorten (bijvoorbeeld passieve FTP-modus of passieve modus Extended) worden momenteel niet ondersteund. LDAP wordt ook niet ondersteund. LDAP een statisch TCP-poort gebruikt, maar het kan ook gebruikmaken van het UDP. Als gevolg hiervan wordt niet ondersteund.

- Kan worden gebruikt met alle kaders die worden ondersteund door Web Apps (.NET, PHP, Java, Python, Node.js) en Mobile Apps (Node.js, .NET).
- Web Apps en Apps voor mobiele toegang tot bronnen op gebouwen op dezelfde manier als het Web of mobiele App bevindt zich op het lokale netwerk. Bijvoorbeeld kunnen hetzelfde connection string gebruikt op-pand ook worden gebruikt op Azure.


Hybride verbindingen bieden ook de beheerders van bedrijfssystemen met controle en inzicht in de hybride toepassingen, inclusief toegang tot bedrijfsnetwerken:

- Werken met instellingen voor Groepsbeleid, beheerders kunnen hybride verbindingen toestaan op het netwerk en ook de middelen die kunnen worden benaderd door toepassingen hybride aanwijzen.
- Controle- en gebeurtenislogbestand op het bedrijfsnetwerk verschaffen inzicht in de bronnen waartoe hybride verbindingen.


## <a name="example-scenarios"></a>Voorbeeldscenario 's

Hybride verbindingen ondersteunt de volgende combinaties van kader en de toepassing:

- .NET framework-toegang tot SQL Server
- .NET framework toegang tot HTTP/HTTPS-services met de WebClient
- Toegang tot SQL Server, MySQL, PHP
- Java-toegang tot SQL Server, MySQL en Oracle
- Java toegang tot HTTP/HTTPS-services

Als u toegang tot SQL-Server in de ruimten met hybride verbindingen, moet u rekening houden met het volgende:

- SQL Express benoemde instanties moet worden geconfigureerd om statische poorten te gebruiken. Standaard benoemde SQL Express instanties gebruik maken van dynamische poorten.
- SQL Express standaard-exemplaren een statische poort gebruiken, maar de TCP moet zijn ingeschakeld. TCP is niet standaard ingeschakeld.
- Wanneer u clusters of beschikbaarheidsgroepen, de `MultiSubnetFailover=true` modus wordt niet ondersteund.
- De `ApplicationIntent=ReadOnly` wordt momenteel niet ondersteund.
- SQL-verificatie mogelijk als de end-to-end-verificatie methode ondersteund door de toepassing Azure en de SQL-server op locatie.


## <a name="security-and-ports"></a>Beveiliging en poorten

Hybride verbindingen gebruiken gedeelde toegang handtekening (SAS) vergunning voor het beveiligen van de verbindingen tussen de Azure toepassingen en Verbindingsbeheer in lokalen hybride hybride-verbinding. Afzonderlijke verbindingssleutels worden gemaakt voor de toepassing en Verbindingsbeheer hybride op gebouwen. Deze verbindingssleutels kunnen worden overgebracht en onafhankelijk van elkaar worden ingetrokken.

Hybride verbindingen bieden voor naadloze en veilige distributie van de sleutels tot de toepassingen en Verbindingsbeheer hybride op gebouwen.

Zie [hybride verbindingen maken en beheren](integration-hybrid-connection-create-manage.md).

*Vergunning van toepassing is gescheiden van de hybride-verbinding*. De juiste autorisatie methode kan worden gebruikt. De methode van de vergunning is afhankelijk van de end-to-end machtigingsmethoden ondersteund tussen de Azure cloud en de onderdelen van gebouwen. Bijvoorbeeld, uw Azure toepassing toegang heeft tot een SQL-Server op locatie. In dit scenario, SQL-verificatie worden de autorisatie-methode die wordt ondersteund in end-to-end.

#### <a name="tcp-ports"></a>TCP-poorten
Hybride verbindingen vereist alleen uitgaande TCP of HTTP-verbinding hebben met uw particuliere netwerk. U hoeft niet alle poorten van de firewall openen of wijzigen van uw netwerkconfiguratie omtrek als u wilt dat alle binnenkomende verbindingen in uw netwerk.

De volgende TCP-poorten worden gebruikt door hybride verbindingen:

Poort | Waarom u deze nodig hebt
--- | ---
9350 - 9354 | Deze poorten worden gebruikt voor gegevensoverdracht. De manager van de relay Service Bus sondes poort 9350 als TCP-verbinding beschikbaar is. Als deze beschikbaar is vervolgens wordt ervan uitgegaan dat poort 9352 ook beschikbaar is. Verkeer gaat via poort 9352. <br/><br/>Uitgaande verbindingen op deze poorten toestaan.
5671 | Wanneer poort 9352 wordt gebruikt voor gegevensverkeer, 5671 poort gebruikt als het besturingskanaal. <br/><br/>Uitgaande verbindingen op deze poort toegestaan.
80, 443 | Deze poorten worden gebruikt voor bepaalde gegevensaanvragen naar Azure. Als u poorten 9352 en 5671 niet bruikbaar zijn, zijn *vervolgens* de poorten 80 en 443 de terugval poorten gebruikt voor de gegevensoverdracht en het besturingskanaal.<br/><br/>Uitgaande verbindingen op deze poorten toestaan. <br/><br/>**Opmerking** Het is niet aanbevolen deze als de terugval poorten te gebruiken in plaats van de TCP-poorten. Het HTTP/WebSocket wordt gebruikt in plaats van native TCP protocol voor gegevenskanalen. Dit kan leiden tot lagere prestaties.



## <a name="next-steps"></a>Volgende stappen

[Hybride verbindingen maken en beheren](integration-hybrid-connection-create-manage.md)<br/>
[Verbinding met de bron van een On-Premises Azure Web Apps](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Verbinding maken met SQL-Server voor ruimten vanuit een Azure web app.](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Zie ook

[REST-API voor het beheren van Services op Microsoft Azure BizTalk-](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk-Services: edities grafiek](biztalk-editions-feature-chart.md)<br/>
[Een BizTalk-Service met Azure portal maken](biztalk-provision-services.md)<br/>
[BizTalk-Services: Tabbladen voor het Dashboard, de Monitor en de schaal](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
