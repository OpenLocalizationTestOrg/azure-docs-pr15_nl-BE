<properties
   pageTitle="Met behulp van de verbindingslijn Azure Resource in logica apps | Microsoft Azure App Service"
   description="Het maken en de Azure Resource Connector of API app configureren en gebruiken in een logica in Azure App Service app"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Aan de slag met Azure Resource Connector en toevoegen aan uw app logica
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld.

Azure Resource Connector eenvoudig Azure om bronnen te beheren binnen uw logica app gebruiken.

## <a name="create-the-azure-resource-connector"></a>De Connector Azure Resource maken
Voor het gebruik van de app Azure Resource Connector API, moet u maakt eerst een exemplaar van het object. U kunt hiervoor een inline tijdens het maken van een app logica of door de app Azure Resource Manager Connector API van Azure Marketplace.

Om deze te configureren, hebt u nodig voor het instellen van een Service Principal met machtigingen om te doen wat het is dat u wilt doen in Azure. Alle oproepen komen op-rekening-van deze Service Principal die u hebt ingesteld. Hierdoor kunt u het bereik van de verbindingslijn om te gebruiken alleen precies wat u wilt doen en niets meer.

David Ebbo heeft [een geweldige blog](http://blog.davidebbo.com/2014/12/azure-service-principal.html) geschreven op dit instellen. Volg alle instructies bevat en u uw **Huurder-ID**, **De Client-ID** en **geheim**. Deze drie velden, plus de **Abonnement-ID**zijn wat zijn vereist voor het configureren van de Connector.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Met behulp van de verbindingslijn Azure Resource in logica apps designer
### <a name="trigger"></a>Trigger
Er zijn twee triggers die worden ondersteund in de Connector:

Naam | Beschrijving
---- | -----------
De gebeurtenis | Trigger wanneer een gebeurtenis op een bron in uw abonnement plaatsvindt.
Metric overschrijdt drempel |  Trigger wanneer een metric voldoet aan een bepaalde drempel.

### <a name="action"></a>Actie

Ook kunt u een groot aantal acties binnen uw abonnement Azure bieden:

U kunt voor **resourcegroepen** :

Naam | Beschrijving
---- | -----------
Lijst met resourcegroepen | Lijst van alle resourcegroepen in het abonnement.
Resourcegroep ophalen | Een resourcegroep ophalen met de id.
Resourcegroep maken | Maken of bijwerken van een resourcegroep.
Resourcegroep verwijderen | Een resourcegroep verwijderen.

Voor **Resources** , kunt u het volgende doen:

Naam | Beschrijving
---- | -----------
Lijst met bronnen | Lijst met bronnen in uw abonnement met verschillende soorten filters.
Bron | Een enkele bron door de resource-id ophalen
Maken of bijwerken van bron | Een bron maken of een bestaande bron bijwerken. U moet alle eigenschappen voor de resource opgeven.
Resource-actie |  Andere acties uitvoeren op een bron. U moet weten de naam van de actie en de nettolading die deze actie wordt (indien aanwezig).
Bron verwijderen | Een bron verwijderen.

U kunt voor **Resource-Providers** :

Naam | Beschrijving
---- | -----------
Lijst bron providers | Overzicht van de beschikbare resources providers in het abonnement.

U kunt voor de **Resource groep implementaties** :

Naam | Beschrijving
---- | -----------
Lijst implementaties | Overzicht van de implementaties in een resourcegroep.
Implementatie ophalen | De sjabloonimplementatie van een ophalen met de id.
Installatie maken | Een nieuwe resource groep implementatie door middel van een sjabloon maken.

U kunt voor **gebeurtenissen** over resources:

Naam | Beschrijving
---- | -----------
Gebeurtenissen ophalen | U gebeurtenissen in een abonnement of voor een resource.

Voor **metrische gegevens** over resources kunt u:

Naam | Beschrijving
---- | -----------
Parameters ophalen | Metric voor een resource-id ophalen

## <a name="do-more-with-your-connector"></a>Doe meer met uw connector
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke stroom met behulp van een app logica. Zie [Wat zijn apps logica?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Als u aan de slag met Azure logica apps wilt voor het aanmelden voor een account met Azure, gaat u naar de [app proberen logica](https://tryappservice.azure.com/?appservice=logic), waar u direct een tijdelijk starter logica app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

Weergeven van de Swagger REST API reference op [verbindingslijnen en API Reference apps](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
