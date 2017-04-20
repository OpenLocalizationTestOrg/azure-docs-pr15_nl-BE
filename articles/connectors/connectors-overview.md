<properties
    pageTitle="Overzicht van de logica Apps connectoren | Microsoft Azure"
    description="Overzicht van verbindingslijnen die kan worden gebruikt in een app logica"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Met behulp van verbindingslijnen in een app logica

Snelle toegang tot gebeurtenissen, gegevens en acties bieden connectors verschillende services, protocollen en platforms.  De volledige lijst van verbindingslijnen die logica Apps ondersteunt kunt [hier vinden](apis-list.md).  Verbindingslijnen kunnen worden gebruikt als een trigger of een actie in een app logica en mogelijk een geconfigureerde *verbinding* te gebruiken (bijvoorbeeld: machtigen een Twitter-account te openen en te boeken voor jou).

## <a name="basics"></a>Grondbeginselen

Verbindingslijnen zijn gehoste services toegankelijk als onderdeel van een app logica te integreren met andere services, zoals Dynamics, Azure, televergaderingen, [en meer](apis-list.md).  Ze worden gedistribueerd en beheerd door Microsoft, zodat u uw workflows integratie met schaal, doorvoer en beveiliging afgehandeld.  U kunt een verbindingslijn toevoegt aan een logica app zoeken en selecteer een handeling van de connector of trigger onder **weergeven Microsoft beheerde API's**.

![Actiemenu trigger selecteren][1]

Elke actie van de connector of de trigger heeft een set eigenschappen configureren.  U kunt op de knoppen info voor meer informatie over de actie op of verwijzen naar de documentatie [voor meer informatie](apis-list.md).

Als u integreren met een service of een API die niet wilt nog een verbindingslijn, kunt u ook logica apps via een [aangepaste verbindingslijn uitbreiden](../app-service-logic/app-service-logic-create-api-app.md) of belt gewoon rechtstreeks naar de service via een protocol zoals HTTP.

## <a name="triggers"></a>Triggers

Sommige verbindingslijnen hebben een trigger, wat betekent dat een gebeurtenis van deze verbindingslijn wordt gestart een app logica en gegevens doorgeeft als onderdeel van de trigger.  Een trigger is altijd de eerste stap in een app logica.  Populaire triggers zijn bewerkingen, zoals:
 
 * Herhaling - elk uur wordt uitgevoerd
 * Als een HTTP-aanvraag wordt ontvangen
 * Wanneer een item wordt toegevoegd aan een wachtrij
 * Wanneer u een e-mailbericht is ontvangen
 
Sommige triggers treedt op het moment dat een gebeurtenis door middel van een kennisgeving aan de toepassing van de logica plaatsvindt en andere moet u een interval van het terugkeerpatroon op hoe vaak de logica app de service voor een gebeurtenis (tot elke 15 seconden controleert) geconfigureerd.  

Nadat een gebeurtenis is ontvangen, de logica app uitvoeren wordt geactiveerd en de acties in de werkstroom wordt gestart.  Ook mogelijk toegang te krijgen tot alle gegevens van de trigger in de werkstroom (bijvoorbeeld de trigger 'op een nieuwe tweet' wordt geaccepteerd door de tweet in de run).

## <a name="actions"></a>Acties

De meeste verbindingslijnen hebben een of meer acties die kunnen worden uitgevoerd als onderdeel van de workflow.  Acties zijn de stappen die hebben plaatsgevonden nadat de uitvoering van een trigger is gestart.  Als u wilt toevoegen van een actie klikt u op de knop **Nieuwe stap** en zoek naar de connector u wilt gebruiken.  Zodra geselecteerd (en na het configureren van [verbindingen](#connections) die nodig zijn) ziet u de actie-kaart die u kunt configureren.  U kunt gegevens selecteren uit de vorige stappen door te klikken op een van de tokens voor uitvoer of indien nodig in een andere configuratie invoeren.

![De actie van een connector configureren][2]

## <a name="connections"></a>Verbindingen

De meeste verbindingslijnen moeten u een *verbinding* configureren voordat u de verbindingslijn kunt gebruiken.  Een *verbinding* is een aanmelding of verbinding-configuratie nodig voor toegang tot de connector.  Betekent het aanmelden bij de service (zoals Office 365, televergaderingen of GitHub) waar het toegangstoken kan worden gecodeerd en veilig opgeslagen in een geheime Azure archief voor verbindingslijnen die OAuth gebruiken, een verbinding maken.  Andere connectors (zoals FTP en SQL) vereist verbinding met de configuratie zoals serveradres, gebruikersnaam en wachtwoord.  Deze configuratie verbindingsgegevens worden ook gecodeerd en veilig opgeslagen.  Verbindingen kunnen toegang krijgen tot de service voor zolang de service kunnen worden.  We kunnen blijven het toegangstoken voor onbepaalde tijd vernieuwen voor Azure Active Directory OAuth-verbindingen (zoals Office 365 en Dynamics).  Andere services kunnen grenzen plaatsen op hoe lang een token kunt gebruiken zonder dat deze worden vernieuwd.  Bepaalde acties zoals het wijzigen van een wachtwoord, worden in het algemeen alle toegangstokens ongeldig.  

Verbindingen kunnen worden bekeken en beheerd in Azure, klikt u op **Bladeren** en selecteer **API-verbindingen**.  Uit de bron-API-verbindingen kunt u weergeven, bewerken, bijwerken of opnieuw toestaan dat alle verbindingen die u hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen

- [Maak uw eerste app in logica](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Meer informatie over algemene gebruikt en voorbeelden van logica apps](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Aan de slag met enterprise Integratietriggers en acties](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png