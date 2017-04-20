<properties
    pageTitle="Met behulp van de verbindingslijn bestand in logica apps | Microsoft Azure App Service"
    description="Het maken en configureren van het bestand connector of API app en gebruiken in een logica in Azure App Service app"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Aan de slag met het bestand connector en toevoegen aan uw app logica
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld.

Verbinding maken met een bestandssysteem te uploaden, downloaden en meer tot uw bestanden op een hostcomputer. Logica apps kunnen worden geactiveerd op basis van verschillende gegevensbronnen en connectors bieden voor het ophalen en verwerken van gegevens. U kunt het bestand connector toevoegen aan workflow- en bedrijfsgegevens als onderdeel van deze werkstroom in een app logica. 

De hybride Connection Manager het bestand connector gebruikt voor hybride verbindingen naar de host file system.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Maken van een bestand-connector voor uw app logica ##
Voor het gebruik van het bestand connector moet u eerst een exemplaar van het bestand connector API app maken. Dit kan als volgt worden gedaan:

1.  Open de Azure Marketplace met + nieuwe optie aan de linkerkant van de Portal Azure.
2.  Zoek naar 'connector file'.
3.  Selecteer **Bestand-Connector (voorbeeld)** uit de lijst met zoekresultaten.
4.  Klik op de knop **maken**
5.  Configureer de connector bestand als volgt:  
![][1]

    - **Naam** - Geef een naam op voor uw bestand connector
    - **Pakketinstellingen**
        - **Hoofdmap** - pad naar de basismap opgeven op de hostcomputer. Bv. D:\FileConnectorTest
        - **Service Bus Connection String** - verbindingsreeks Bus Service bieden. Zorg ervoor dat de service bus-naamruimte van het type standaard en niet standaard toestaan voor het gebruik van de Service Bus relais.  Service Bus Relay wordt gebruikt om verbinding met de hybride Connection Manager.
    - **App serviceplan** - selecteren of maken van een App-serviceplan
    - **Prijzen laag** - Kies een laag prijzen voor connector
    - **Resourcegroep** - Selecteer of maak een groep waar de verbindingslijn moet zich bevinden
    - **Abonnement** - Kies een abonnement dat u maken wilt in deze connector
    - **Locatie** - kiest u de geografische locatie waar u wilt dat de connector worden geïmplementeerd

4. Klik op maken. De connector van een nieuw bestand wordt gemaakt

## <a name="configure-hybrid-connection-manager"></a>Hybride Connection Manager configureren ##
Zodra de API App-exemplaar is gemaakt, bladert u naar het dashboard.  U kunt dit doen door te klikken op Bladeren > API Apps > Selecteer uw bestand connector API App.  Hier moet de hybride Verbindingsbeheer worden geconfigureerd.
Voor meer informatie over het configureren en problemen op te lossen de hybride Verbindingsbeheer Zie [Verbindingsbeheer hybride].

## <a name="using-the-file-connector-in-your-logic-app"></a>Met behulp van de verbindingslijn bestand in uw app logica ##
Als uw app API is gemaakt, nu kunt u het bestand connector als een actie voor uw app logica. Hiervoor moet u:

1.  Maak een nieuwe logica app en kies dezelfde resourcegroep waaraan de verbindingslijn bestand heeft. Volg de aanwijzingen naar [een nieuwe logica app maken].

2.  Open 'Triggers en acties' binnen de gemaakte app logica de logica apps Designer openen en configureren van de stroom.

3.  De connector van het bestand weergegeven in de sectie 'API Apps in de resourcegroep' in de galerie op de rechterkant.

4.  U kunt het bestand connector API app neerzetten in de editor door te klikken op de connector' bestand'. connector bestand bevat één trigger en 4 acties:  
![][5]

6.  Elke één van deze worden bepaalde eigenschappen. De onderstaande afbeelding geeft een overzicht van de eigenschappen voor de trigger en actie-bestand:  
![][6]

7. Nadat deze zijn geconfigureerd, kan de Trigger en actie in de stroom worden gebruikt. Ook kunnen andere acties ook worden geconfigureerd.

> [AZURE.NOTE] De trigger bestand zal het bestand verwijderen nadat deze met succes wordt gelezen uit de map.

## <a name="file-connector-rest-apis"></a>Het bestand connector REST API 's ##
Voor het gebruik van de connector buiten een app logica, kunnen de REST API's die door de connector worden blootgesteld worden gebruikt. U kunt-de definities van deze API gebruik Bladeren Api App -> Weergave > bestand connector. Klik nu op de lens API definitie onder de sectie overzicht bekijken van alle API's die door deze connector:  
![][7]

Details van de API's vindt u op het [bestand connector definitie van API].

## <a name="do-more-with-your-connector"></a>Doe meer met uw connector
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een app logica. Zie [Wat zijn apps logica?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Als u aan de slag met Azure logica apps wilt voor het aanmelden voor een account met Azure, gaat u naar de [app proberen logica](https://tryappservice.azure.com/?appservice=logic), waar u direct een tijdelijk starter logica app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

De Swagger REST API reference op [verbindingslijnen en API-Naslaggids voor Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766)bekijken.

U kunt ook statistieken en beheer van zekerheid aan de verbindingslijn bekijken. Zie [beheren en controleren van uw ingebouwde API Apps en connector](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Maak een nieuwe logica app]: app-service-logic-create-a-logic-app.md
[Bestand connector API, definitie]: https://msdn.microsoft.com/library/dn936296.aspx
[Met behulp van de hybride Connection Manager]: app-service-logic-hybrid-connection-manager.md
