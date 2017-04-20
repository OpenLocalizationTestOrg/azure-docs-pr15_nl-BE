<properties
    pageTitle="Lijst met beschikbare verbindingslijnen API Apps | Microsoft Azure App Service"
    description="Meer informatie over connectoren en API Apps in Azure App-Service"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Lijst van verbindingslijnen en API Apps te gebruiken in uw logica Apps
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld. Zie de [Lijst met nieuwe verbindingslijnen](../connectors/apis-list.md)voor de versie logica Apps algemene beschikbaarheid (GA).

Meer informatie over de beschikbare connectors en API Apps gemaakt door Microsoft te gebruiken in uw logica Apps.

Voor informatie en een lijst van wat opgenomen in elke laag Service is prijzen, Zie [Azure App serviceprijzen](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Ga naar [Probeer logica App](https://tryappservice.azure.com/?appservice=logic)aan de slag met Apps logica voor het aanmelden voor een account met Azure. U kunt direct een tijdelijk starter logica app in App-Service maken. Geen creditcard vereist; geen verplichtingen.

## <a name="core-connectors"></a>Core-Connectors
De volgende tabel worden de beschikbare connectors en API-Apps gemaakt door Microsoft die als Core Connectors:

Naam | Beschrijving
--- | ---
[Bing Translator](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Bing gebruiken om tekst te vertalen in een andere taal.
[HTTP](app-service-logic-connector-http.md) | De HTTP-Listener wordt geopend een eindpunt dat fungeert als een HTTP-server en luistert naar binnenkomende HTTP- of HTTPS-aanvragen. De HTTP-actie een API App niet nodig en binnen Apps logica wordt ondersteund.
[Toegestane vertraging](app-service-logic-connector-slack.md) | Verbinding maken met een toegestane vertraging en berichten met toegestane kanalen.


## <a name="enterprise-integration-connectors"></a>Enterprise Integration verbindingslijnen
De volgende tabel worden de beschikbare verbindingslijnen en API Apps gemaakt door Microsoft beschikbaar als Enterprise Integration Connectors:

Naam  | Beschrijving
------------- | -------------
[BizTalk-regels](app-service-logic-use-biztalk-rules.md) | BizTalk-regels gebruiken om te definiëren en beheren van de bedrijfslogica in een organisatie. Zakelijk beleid kunnen worden bijgewerkt zonder opnieuw te compileren of zonder de bijbehorende toepassingen opnieuw te implementeren.
[BizTalk-XPath Extractor](app-service-logic-xpath-extract.md) | Zoekt en haalt gegevens uit XML-inhoud op basis van een XPath dat u kiest.
[DB2-Connector](app-service-logic-connector-db2.md) | Verbindt een IBM DB2-database op gebouwen en een Windows-besturingssysteem wordt uitgevoerd op een Azure virtuele machine. Web API en OData-API kan worden toegewezen aan opdrachten Informix Structured Query Language. <br/><br/>Geen triggers. Acties zijn tabel selecteren, invoegen, bijwerken, verwijderen en aangepaste instructie<br/><br/>Deze connector bevat ook de Microsoft Client voor DRDA verbinding maken met een Informix-server via een TCP/IP-netwerk.
[Bestand](app-service-logic-connector-file.md) | Deze connector gebruikt, kunt u het bestandssysteem op de bedrijfsruimten of netwerk en volledig document in een andere taken, zoals het uploaden, verwijderen waarin bestanden en meer.
[Informix](app-service-logic-connector-informix.md) | Maakt verbinding met een IBM Informix-database, in gebouwen en op een Azure virtuele machine met een Windows-besturingssysteem. Web API en OData-API kan worden toegewezen aan opdrachten Informix Structured Query Language.<br/><br/>Geen triggers. Acties zijn tabel selecteren, invoegen, bijwerken, verwijderen en aangepaste-instructie.<br/><br/>Wanneer u op het bedrijf, kan VPN- of ExpressRoute Azure worden gebruikt. Deze connector bevat ook een Microsoft-Client voor DRDA verbinding maken met een Informix-server via een TCP/IP-netwerk.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Maakt verbinding met on-premises SQL Server of een Azure SQL-Database. U kunt maken, bijwerken, ophalen en verwijderen van gegevens in een SQL-databasetabel.
MQ | Maakt verbinding met IBM WebSphere MQ-Server versie 8, in gebouwen en op een Azure virtuele machine met een Windows-besturingssysteem. Wanneer u op het bedrijf, kan VPN- of ExpressRoute Azure worden gebruikt. De connector bevat tevens de Microsoft Client voor MQ.<br/><br/>Geen triggers. Geen acties.<br/><br/>**Opmerking** Op dit moment worden niet gebruikt met logica Apps.

## <a name="connectors-as-triggers"></a>Verbindingslijnen als Triggers
Verschillende connectors bieden triggers voor Apps logica. Deze triggers zijn twee typen:

1. Poll Triggers: Deze triggers gecontroleerd uw service op een opgegeven frequentie om te controleren of er nieuwe gegevens. Wanneer nieuwe gegevens beschikbaar is, wordt een nieuw exemplaar van uw App logica uitgevoerd met de gegevens als invoer. Om te voorkomen dat wordt verbruikt dezelfde gegevens meerdere keren de trigger kan opschonen gegevens die zijn gelezen en doorgegeven aan de toepassing van de logica. Voorbeelden van dergelijke verbindingslijnen zijn bestanden, SQL en Azure opslag.
2. Push-signalen: Luisteren deze triggers voor gegevens op een eindpunt of voor een gebeurtenis plaatsvindt. Vervolgens een nieuw exemplaar van een logica App activeren. Voorbeelden van dergelijke verbindingslijnen zijn HTTP-Listener en Twitter.

## <a name="connectors-as-actions"></a>Verbindingslijnen als acties
Verbindingslijnen kunnen ook worden gebruikt als acties binnen uw App logica. Acties zijn handig voor het opzoeken van gegevens binnen de logica App die kan vervolgens worden gebruikt in de uitvoering. U wilt bijvoorbeeld opzoeken van gegevens uit een SQL-database voor meer informatie over een klant bij het verwerken van een order. Of u wilt schrijven, bijwerken of verwijderen van gegevens in een bestemming. U kunt dit doen met de acties die door de verbindingslijnen. Acties toewijzen aan bewerkingen in API Apps (zoals gedefinieerd in de metagegevens van Swagger).

## <a name="create-your-own-connectors-and-api-apps"></a>Verbindingslijnen en API Apps maken
[Verbindingslijnen en API-Naslaggids voor Apps](http://aka.ms/appservicesconnectorreference)  
[Azure App Service API app triggers](../app-service-api/app-service-api-dotnet-triggers.md)  
[Logica App verwijzing](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Meer informatie over connectoren en API Apps
[Wat zijn verbindingslijnen en BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md)  
[Met behulp van de hybride Connection Manager in Azure App-Service](app-service-logic-hybrid-connection-manager.md)  
[De ingebouwde API Apps en de verbindingen controleren en beheren](app-service-logic-monitor-your-connectors.md)
