<properties 
    pageTitle="Logica Apps prijsstructuur | Microsoft Azure" 
    description="Details over de prijzen werking in logica Apps" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Logica Apps prijsstructuur

Azure logica Apps kunt u schalen en uitvoeren van workflows voor integratie in de cloud.  Hieronder vindt u meer informatie over de facturering en prijsstelling plannen voor Apps logica.

## <a name="consumption-pricing"></a>Consumptie prijzen

Nieuwe logica Apps gebruiken een plan voor consumptie. Met de logica Apps verbruik pricing model betaalt je alleen voor wat u gebruikt.  De snelheid van de logica Apps zijn niet bij het gebruik van een plan voor consumptie.
Alle acties die worden uitgevoerd in een sessie van een logica app-exemplaar zijn gemeten.

### <a name="what-are-action-executions"></a>Wat zijn actie uitvoeringen?

Elke stap in een definitie van de logica app is een actie.  Dit geldt ook voor triggers, control flow stappen als voorwaarden, scopes, for each-lussen totdat lussen voor verbindingslijnen en aanroepen naar native acties doen.
Triggers zijn alleen met speciale acties die zijn ontworpen om een nieuwe instantie van een app logica wanneer een bepaalde gebeurtenis plaatsvindt.  Er zijn een aantal verschillende bewerkingen voor triggers kunnen beïnvloeden hoe de app logica wordt gemeten.

-   **Polling-trigger** – deze trigger voortdurend controleert of een eindpunt totdat er een bericht dat voldoet aan de criteria ontvangt voor het maken van een nieuw exemplaar van een app logica.  De polling-interval kan worden geconfigureerd in de trigger in de ontwerpfunctie voor Apps logica.  Elke polling-verzoek, wordt zelfs als er een nieuw exemplaar van een app logica niet gemaakt tellen als een actie worden uitgevoerd.

-   **De trigger Webhook** – deze trigger wacht op een client een aanvraag voor een bepaald eindpunt verzenden.  Elke aanvraag die wordt verzonden naar het eindpunt webhook telt als een actie worden uitgevoerd. De aanvraag en de HTTP-Webhook trigger zijn beide webhook triggers.

-   **Terugkeerpatroon trigger** – deze trigger maakt een nieuw exemplaar van de logica app op basis van het interval van het terugkeerpatroon in de trigger is geconfigureerd.  Bijvoorbeeld kan een trigger herhaling worden geconfigureerd om te worden uitgevoerd om de 3 dagen of zelfs elke minuut.

Trigger uitvoeringen kunnen in de blade logica Apps bron in het gedeelte Geschiedenis van de Trigger bekijken.

Alle acties die zijn uitgevoerd, of ze waren dat geslaagde of mislukte worden gemeten als een actie worden uitgevoerd.  Acties die zijn overgeslagen omdat een voorwaarde niet wordt voldaan of acties die niet worden uitgevoerd omdat de app logica beëindigd vóór voltooiing worden niet meegeteld als actie uitvoeringen.

Acties uitgevoerd in lussen worden geteld per iteratie van de lus.  Een enkele actie in een voor elke lus doorlopen een lijst van 10 items worden geteld als het aantal items in de lijst (10) vermenigvuldigd met het aantal acties in de lus (1) plus één voor de opening van de lus die in dit voorbeeld zijn zou (10 * 1) + 1 = 11 actie uitvoeringen.

Logica Apps die zijn uitgeschakeld, kan geen nieuwe exemplaren gemaakt en daarom op het moment dat ze zijn uitgeschakeld zal niet krijgen in rekening gebracht.  Zijn zich ervan bewust dat na het uitschakelen van een app logica het even de tijd voor de exemplaren stilleggen duurt voordat volledig wordt uitgeschakeld.

## <a name="app-service-plans"></a>App-en serviceplannen

Serviceplannen voor App niet meer vereist voor het maken van een App logica.  U kunt ook verwijzen naar een App Service plannen met een bestaande logica app.  Logica apps die eerder zijn gemaakt met een serviceplan App blijft werken als voorheen, afhankelijk van het plan gekozen, zal krijgen vertraagde nadat een aantal dagelijkse uitvoeringen worden overschreden en wordt niet in rekening worden gebracht met de meter actie worden uitgevoerd.

App serviceplannen en hun dagelijkse toegestane actie uitvoeringen:

| |Vrij/gedeeld/Basic|Standaard|Premium|
|---|---|---|---|
|Actie uitvoeringen per dag| 200|10.000|50.000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Omzetten van consumptie prijzen App Service Plan

Als u verwijst naar een App Service plannen voor een consumptie logica App, kunt u eenvoudig [uitvoeren van de onderstaande PowerShell script](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Zorg ervoor dat u de [Azure PowerShell tools](https://github.com/Azure/azure-powershell) geïnstalleerd hebben.

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Converteren van App Service plannen voor consumptie prijzen

Wijzigen van een App logica met een App Service Plan gekoppeld aan een model van consumptie de verwijzing naar de App Service plannen in de definitie van de logica App verwijderen.  Dit u kunt doen met een aanroep van een PowerShell-cmdlet:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Prijzen

Zie [Logica Apps prijzen](https://azure.microsoft.com/pricing/details/logic-apps/)voor prijsdetails.

## <a name="next-steps"></a>Volgende stappen

- [Een overzicht van de logica Apps][whatis]
- [Maak uw eerste app in logica][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

