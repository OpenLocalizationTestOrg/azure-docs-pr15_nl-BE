<properties
   pageTitle="Azure Active Directory Graph API | Microsoft Azure"
   description="Een overzicht en quickstart-gids voor de Graph API die programmatisch toegang tot Azure AD tot en met REST API eindpunten biedt."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API

> [AZURE.IMPORTANT] Azure AD Graph API-functionaliteit is ook beschikbaar via [Microsoft Graph](https://graph.microsoft.io/)een geünificeerde API die API's van andere Microsoft-services, zoals OneDrive, Outlook, OneNote, Planner en Office-grafiek, toegankelijk via een enkel eindpunt en met een één toegangstoken bevat.

Azure Active Directory Graph API biedt toegang op programmeerniveau tot Azure AD via eindpunten REST API. Toepassingen kunnen de Graph API gebruiken voor het uitvoeren van maken, lezen, bijwerken en verwijderen van bewerkingen van Active directory-gegevens en objecten (CRUD). De Graph API ondersteunt bijvoorbeeld de volgende veelvoorkomende bewerkingen voor een user-object:

- Maak een nieuwe gebruiker in een directory

- Krijg meer eigenschappen van een gebruiker, zoals de groepen

- De eigenschappen van een gebruiker, zoals hun locatie en telefoonnummer, bijwerken of wijzigen van hun wachtwoord.

- Controleer het groepslidmaatschap van een gebruiker voor toegang op basis van rollen

- Een gebruikersaccount uitschakelen of volledig verwijderen

Naast user-objecten, kunt u dezelfde bewerkingen op andere objecten, zoals groepen en toepassingen uitvoeren. De Graph API aanroepen op een map, wordt de toepassing met Azure Active Directory moet worden geregistreerd en worden geconfigureerd voor het toestaan van toegang tot de map. Dit wordt gewoonlijk bereikt door middel van een gebruiker of beheerder toestemming stroom.

Raadpleeg de [Handleiding van Graph API](active-directory-graph-api-quickstart.md)om te beginnen met de Azure Active Directory Graph API of de [interactieve grafiek API-documentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Functies

De Graph API biedt de volgende functies:

- **REST API eindpunten**: The Graph API is een RESTful service bestaat uit eindpunten die worden geopend met behulp van standaard HTTP-aanvragen. De Graph API ondersteunt XML- of Javascript Object Notation (JSON) inhoudstypen voor aanvragen en antwoorden. Zie [Azure AD Graph REST API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)voor meer informatie.

- **Verificatie met Azure AD**: elke aanvraag van de Graph API moet worden geverifieerd door een JSON Web Token (JWT) in de koptekst van de vergunning van het verzoek toe te voegen. Deze token wordt verkregen door het verzoek naar Azure AD token eindpunt en het leveren van geldige referenties. Kunt u de stroom OAuth 2.0-client referenties of de code van de vergunning verlenen stroom aan te schaffen van een token om aan te roepen van de grafiek. Voor meer informatie, [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Role-Based Authorization RBAC ()**: beveiligingsgroepen gebruikt voor het uitvoeren van RBAC in de Graph API. Als u bepalen wilt of een gebruiker toegang tot een specifieke bron heeft, kan de toepassing welke bewerking [Controleer lidmaatschap (transitief)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) geeft als waar of ONWAAR resultaat aanroepen.

- **Differentiële Query**: als u controleren of wijzigingen in een map tussen de twee perioden wilt zonder ervoor te moeten regelmatig query's naar de Graph API, kunt u een queryaanvraag voor een differentiële. Dit type aanvraag retourneert alleen de wijzigingen tussen de vorige differentiële queryaanvraag en de huidige aanvraag. Zie [differentiële Azure AD Graph API-Query](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query)voor meer informatie.

- **Directory-extensies**: als u een toepassing die u wilt lezen of schrijven van unieke eigenschappen voor directoryobjecten ontwikkelt, kunt u registreren en uitbreiding waarden met behulp van de Graph API gebruiken. Bijvoorbeeld, als uw toepassing een Skypenaam eigenschap voor elke gebruiker, kunt u de nieuwe eigenschap in Active directory registreren en zijn beschikbaar op elk gebruikersobject. Zie voor meer informatie [Azure AD Graph API Directory Schema-uitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Beveiligd door scopes machtiging**: AAD Graph API maakt machtiging bereiken die beveiligd/ingestemd toegang te krijgen tot gegevens AAD en ondersteunen een verscheidenheid aan app clienttypen, waaronder:
 - met een gebruikersinterface die zijn gegeven toegang tot gegevens via een vergunning van de aangemelde gebruiker (gedelegeerd) gedelegeerd
  - die met definieert toepassing-op rollen gebaseerde toegangscontrole zoals service/daemon-clients (app rollen)

    Zowel overgedragen als app rol machtiging scopes vertegenwoordigen een bevoegdheid die door de API van de grafiek weergegeven en kunnen worden aangevraagd door de client-toepassingen via registratie machtigingen voor [voorzieningen in de klassieke Azure portal](https://manage.windowsazure.com). Kunnen clients verifiëren de scopes machtiging verleend aan hen met een controle van het bereik ('scp') claim in het toegangstoken voor overgedragen machtigingen ontvangen en voor app-roltoegangsrechten aanspraak maken op de rollen ('rollen'). Meer informatie over [Azure AD Graph API machtiging bereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Scenario 's

De Graph API kunnen veel scenario's van toepassing. De volgende scenario's zijn de meest voorkomende:

- **Line of Business (één huurder) toepassing**: In dit scenario wordt een enterprise developer werkt voor een organisatie met een Office 365-abonnement. De ontwikkelaar bouwt een webtoepassing die samenwerkt met Azure AD taken uit te voeren die een licentie toewijzen aan een gebruiker. Deze taak vereist toegang tot de API van de grafiek, zodat de registers van de ontwikkelaar van de ene toepassing in Azure AD pachters en configureert u lees- en voor de Graph API schrijfmachtigingen. Vervolgens is de toepassing geconfigureerd voor gebruik van een eigen referenties of die van de gebruiker die momenteel aanmelden aan te schaffen van een token in de Graph API aanroepen.

- **Software als een Service-toepassing (met meerdere huurder)**: In dit scenario wordt een onafhankelijke softwareleverancier (ISV) gehoste meerdere huurder webtoepassing biedt functies voor het beheer van gebruikers voor andere organisaties die gebruikmaken van Azure AD ontwikkelt. Deze functies vereisen toegang tot directory-objecten en de toepassing moet de Graph API aan te roepen. De ontwikkelaar, registreert u de toepassing in Azure AD worden geconfigureerd om te vereisen lees- en schrijfmachtigingen voor de Graph API en wordt dat andere organisaties kunnen toestemming voor het gebruik van de toepassing in de map Externe toegang. Wanneer een gebruiker in een andere organisatie de toepassing voor de eerste keer verifieert, ze staan een dialoogvenster instemming met de machtigingen die de toepassing aanvraagt.  Verlenen van toestemming krijgt vervolgens de toepassing die machtigingen voor de Graph API in de map van de gebruiker gevraagd. Voor meer informatie over het kader van de toestemming, overzicht [van het kader staan](active-directory-integrating-applications.md).

## <a name="see-also"></a>Zie ook

[Azure AD Graph API handleiding](active-directory-graph-api-quickstart.md)

[AD Graph REST documentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory developer's guide](active-directory-developers-guide.md)
