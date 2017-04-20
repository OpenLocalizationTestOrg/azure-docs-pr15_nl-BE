<properties
   pageTitle="Azure Resource Explorer | Microsoft Azure"
   description="Beschrijving van Azure Resource Explorer en hoe deze kan bekijken en bijwerken via Azure Resource Manager-implementaties worden gebruikt"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Azure Resource Explorer gebruiken om te bekijken en resources wijzigen
De [Azure Resource Explorer](https://resources.azure.com) is een geweldig hulpmiddel voor het kijken naar de bronnen die u al hebt gemaakt in uw abonnement. Met dit hulpprogramma kunt inzicht u in hoe de bronnen worden opgebouwd en de eigenschappen die zijn toegewezen aan elke resource. U kunt informatie over de REST API operations en PowerShell-cmdlets die beschikbaar voor een brontype zijn en geeft u opdrachten via de interface. Resource Explorer is met name handig bij het maken van sjablonen voor bronbeheer omdat Hiermee kunt u de eigenschappen voor bestaande bronnen.

De bron voor de Resource Explorer tool is beschikbaar op [github](https://github.com/projectkudu/ARMExplorer), biedt een waardevolle verwijzing als u nodig hebt voor het implementeren van soortgelijk gedrag in uw eigen toepassingen.

## <a name="view-resources"></a>Bronnen weergeven
Ga naar [https://resources.azure.com](https://resources.azure.com) en meld u aan met dezelfde referenties voor de [Azure-Portal gebruikt](https://portal.azure.com).

Eenmaal geladen, wordt de treeview aan de linkerkant kunt u inzoomen op uw abonnementen en resourcegroepen:

![TreeView](./media/resource-manager-resource-explorer/are-01-treeview.png)

Als u een lager niveau in een resourcegroep ziet u de providers die er bronnen in de groep zijn:

![providers](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Van daaruit kunt u boren in de resource-exemplaren starten. In het screenshot hieronder ziet u de `sltest` SQL Server-exemplaar in de structuurweergave. Aan de rechterkant ziet u informatie over de aanvragen voor REST-API die kunt u met deze resource. Door te gaan met het knooppunt voor een resource, wordt de Resource Explorer automatisch het GET-verzoek voor het ophalen van informatie over de resource. In het grote tekstgebied onder de URL ziet u de reactie van de API. 

Zoals u gewend bent aan sjablonen Resource Manager geraakt wordt de inhoud van de hoofdtekst bekend! De sectie **-Eigenschappen** van het antwoord komt overeen met de waarden die u in de sectie **Eigenschappen** van de sjabloon opgeeft.

![SQL server](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Resource Explorer kunt u inzoomen houden onderliggende bronnen voor SQL Server-Database verkennen, zijn er onderliggende bronnen voor bijvoorbeeld databases en firewall-regels.

Een database verkennen, zien we de eigenschappen van de database. In het screenshot hieronder kunt zien we dat de database `edition` is `Standard` en de `serviceLevelObjective` (of de databaselaag) is `S1`.

![SQL-database](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Resources wijzigen

Zodra u bent gegaan naar een bron, kunt u de knop bewerken om de JSON-inhoud worden bewerkt. Vervolgens kunt u Resource Explorer de JSON bewerken en verzenden van een PUT-aanvraag voor het wijzigen van de resource. De onderstaande afbeelding ziet u bijvoorbeeld de databaselaag gewijzigd in `S0`:

![database - aanvraag plaatsen](./media/resource-manager-resource-explorer/are-05-database-put.png)

Door te selecteren **zet** indient u de aanvraag. 

Zodra de aanvraag is ingediend verleent Resource Explorer opnieuw het GET-verzoek voor het vernieuwen van de status. In dit geval kunnen we zien dat de `requestedServiceObjectiveId` is bijgewerkt en verschilt van de `currentServiceObjectiveId` die aangeeft dat een bewerking schalen uitgevoerd wordt. Kunt u de knop GET om handmatig de status te vernieuwen.

![database - GET request2](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Acties uitvoeren voor resources

Het tabblad **Acties** kunt u zien en extra overige bewerkingen uitvoeren. Wanneer u een website resource hebt geselecteerd, bevat het tabblad acties een lange lijst van beschikbare bewerkingen, waarvan sommige worden hieronder weergegeven.

![Web - POST-aanvraag](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>Het aanroepen van de API via PowerShell
Het tabblad PowerShell in Resource Explorer ziet u de cmdlets gebruiken om te communiceren met de resource die u aan het verkennen bent. Afhankelijk van het type bron dat u hebt geselecteerd, de PowerShell-script weergegeven kan variëren van eenvoudige cmdlets (zoals `Get-AzureRmResource` en `Set-AzureRmResource`) tot meer gecompliceerde cmdlets (zoals het wisselen van "slots" op een website). 

![PowerShell](./media/resource-manager-resource-explorer/are-07-powershell.png)

Zie voor meer informatie op de Azure PowerShell-cmdlets [Gebruiken Azure PowerShell Azure Resource Manager](powershell-azure-resource-manager.md)

## <a name="summary"></a>Samenvatting
Als u werkt met Resource Manager, kan de Resource Explorer zeer nuttig gereedschap zijn. Het is een uitstekende manier om te zoeken naar manieren om PowerShell te zoeken en te wijzigen. Als u met de REST API werkt is een uitstekende manier om aan de slag en snel testen API-aanroepen voordat u begint met het schrijven van code. En als u sjablonen dat kan zijn een uitstekende manier schrijft om inzicht in de hiërarchie van de bron en voor de opslag van configuratie - zoeken kunt u een wijziging aanbrengt in de Portal en zoekt u de bijbehorende posten in de Resource Explorer!

Voor meer informatie Bekijk de [video met Scott Hanselman en David Ebbo Channel-9](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo)


