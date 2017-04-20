<properties 
    pageTitle="Resources verplaatsen naar een nieuwe resourcegroep | Microsoft Azure" 
    description="Azure Resource Manager gebruiken voor resources verplaatsen naar een nieuwe resourcegroep of -abonnement." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Resources verplaatsen naar een nieuwe resourcegroep of abonnement

In dit onderwerp wordt beschreven hoe u bronnen verplaatsen naar een nieuw abonnement of een nieuwe groep in het hetzelfde abonnement. U kunt de portal, PowerShell, Azure CLI of de REST API resource verplaatsen. De bewerkingen verplaatsen in dit onderwerp voor u beschikbaar zijn zonder de hulp van Azure support.

Meestal verplaatsen u bronnen wanneer u dat besluit:

- Voor facturatie, een resource nodig heeft om te wonen in een ander abonnement.
- Een bron deelt niet langer de levenscyclus van dezelfde als deze al eerder werd gegroepeerd met resources. U wilt verplaatsen naar een nieuwe resourcegroep, zodat u kunt deze bron afzonderlijk van de andere bronnen beheren.

Bij het verplaatsen van resources, worden zowel de groep als de doelgroep vergrendeld tijdens de bewerking. Schrijven en verwijderen van de groepen worden geblokkeerd totdat de verplaatsing is voltooid.

U kunt de locatie van de resource niet wijzigen. Door een resource alleen verplaatst u deze naar een nieuwe resourcegroep. De nieuwe resourcegroep kan een andere locatie hebt, maar dat de locatie van de resource niet gewijzigd.

> [AZURE.NOTE] Dit artikel wordt beschreven hoe u bronnen binnen een bestaande Azure account aanbieden. Als u uw Azure-account wilt (bijvoorbeeld een upgrade van pay-as-you-go vooraf betalen) bieden terwijl u werkt met uw bestaande resources wijzigen, Zie [schakeloptie Azure een abonnement op een ander voorstel](billing-how-to-switch-azure-offer.md). 

## <a name="checklist-before-moving-resources"></a>Controlelijst voor het verplaatsen van resources

Er zijn enkele belangrijke stappen uitvoeren voordat u een bron. Door te controleren of deze voorwaarden, kunt u fouten voorkomen.

1. De service moet de mogelijkheid om resources inschakelen. Zie de lijst hieronder voor meer informatie over welke [services kunnen bewegende bronnen](#services-that-enable-move).
1. De bron- en doeladressen abonnementen moeten bestaan binnen de dezelfde [huurder Active Directory](./active-directory/active-directory-howto-tenant.md). Als u wilt verplaatsen naar een nieuwe huurder, contact op met ondersteuning.
2. Abonnement op de bestemming moet worden geregistreerd voor de voorziening van de bron van de resource wordt verplaatst. Als dat niet het geval is, wordt er een foutbericht met de mededeling dat het **abonnement is niet geregistreerd voor een brontype**. U kan dit probleem zich voordoet wanneer een resource verplaatsen naar een nieuw abonnement maar dat abonnement is nooit met dat soort resource gebruikt. Meer informatie over het controleren van de registratiestatus en resource-providers registreren, Zie [Resource providers en typen](../resource-manager-supported-services.md#resource-providers-and-types).
4. Als u de App Service app verplaatst, kunt u [beperkingen voor App-Service](#app-service-limitations)hebt bekeken.
4. Als u de resources die zijn gekoppeld aan de Recovery Services verplaatst, kunt u de [Recovery Services beperkingen](#recovery-services-limitations) hebt bekeken
5. Als u resources via klassieke objectmodel geïmplementeerd verplaatst, kunt u [beperkingen voor klassieke implementatie](#classic-deployment-limitations)hebt gecontroleerd.

## <a name="when-to-call-support"></a>Bij het aanroepen van ondersteuning

De meeste bronnen via de self-service bewerkingen in dit onderwerp wordt weergegeven, kunt u verplaatsen. De self-service-bewerkingen te gebruiken:

- Resource Manager bronnen verplaatsen.
- De [klassieke implementatie beperkingen](#classic-deployment-limitations)de klassieke resources verplaatsen. 

Ondersteuning bellen wanneer u wilt:

- Resources verplaatsen naar een nieuwe Azure account (en de huurder voor Active Directory).
- Klassieke bronnen verplaatsen maar moeite hebben met de beperkingen.

## <a name="services-that-enable-move"></a>Services die een verplaatsen

Op dit moment zijn de diensten die het mogelijk verplaatsen naar een nieuwe resourcegroep en een abonnement:

- API-beheer
- App Service apps (web apps) - Zie [App Service beperkingen](#app-service-limitations)
- Automatisering
- Batch
- CDN
- Cloud Services - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
- Data Factory
- DNS
- DocumentDB
- HDInsight clusters
- IoT Hubs
- Sleutel kluis 
- Mediaservices
- Betrokkenheid bij de mobiele
- Melding Hubs
- Operationele inzichten
- Cache bestand Vgx.
- Scheduler
- Zoeken
- Bus service
- Opslag
- Opslag (klassiek) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
- Database van SQL server - database en server moet zich in dezelfde bronnengroep bevinden. Wanneer u een SQL-server hebt verplaatst, worden ook alle bijbehorende databases verplaatst.
- Virtuele Machines - echter kan niet verplaatsen naar een nieuw abonnement wanneer de certificaten zijn opgeslagen in een kluis met sleutel
- Virtuele Machines (klassiek) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
- Virtuele netwerken

## <a name="services-that-do-not-enable-move"></a>Services waarmee niet verplaatsen

De services die momenteel niet de mogelijkheid een resource verplaatsen zijn:

- Application Gateway
- Inzichten van toepassing
- Route Express
- Recovery Services vault - ook niet verplaatsen van de Compute-, netwerk- en bronnen die zijn gekoppeld aan de kluis Recovery Services, Zie [Recovery Services beperkingen](#recovery-services-limitations).
- Virtuele Machines met een certificaat dat is opgeslagen in een kluis met sleutel
- Schaal instellen van virtuele Machines
- Virtuele netwerken (klassiek) - Zie [klassieke implementatie beperkingen](#classic-deployment-limitations)
- VPN-Gateway

## <a name="app-service-limitations"></a>Beperkingen voor App-Service

Als u werkt met apps App-Service, kunt u niet alleen een App serviceplan verplaatsen. Om de App Service apps verplaatsen, zijn de opties:

- Het plan App Service en alle andere App-Service bronnen in die bronnengroep verplaatsen naar een nieuwe resourcegroep die geen App-servicebronnen. Deze eis betekent dat u zelfs de App-Service bronnen die niet gekoppeld aan het abonnement App zijn moet verplaatsen. 
- De apps verplaatsen naar een andere groep, maar alle App serviceplannen bewaren in de oorspronkelijke resourcegroep.

Als uw oorspronkelijke resourcegroep ook een bron van inzicht van de toepassing bevat, kunt u die bron niet verplaatsen omdat inzichten van toepassing wordt momenteel niet ingeschakeld voor de verplaatsing. Als u de bron van de inzichten van toepassing bij het verplaatsen van apps in App-Service, mislukt het hele verhuizing. Echter, de toepassing inzichten en App serviceplan hoeft te bevinden zich in dezelfde bronnengroep als de app voor de app goed te laten functioneren.

Als bijvoorbeeld de resourcegroep bevat:

- **Web-a** die is gekoppeld aan **een plan** en **app-inzichten-a**
- **web-b** gekoppeld aan het **plan-b** - en **app-inzichten-b is**

De opties zijn:

- Verplaatsen **web-a**, **een plan**, **web-b**en **plan b**
- Verplaats **een web** en **web-b**
- Verplaats **een web**
- Verplaatsen, **web-b**

Alle andere combinaties betrekking hebben op een brontype dat niet verplaatsen (toepassing inzichten) verplaatsen of verlaten achter een brontype dat niet kan worden achtergelaten bij het verplaatsen van een App serviceplan (type van de bron van het App-Service).

Als uw web app bevindt zich in een andere bronnengroep bevinden dan de App serviceplan, maar u wilt op een nieuwe bronnengroep te verplaatsen, moet u de verplaatsing in twee stappen uitvoeren. Bijvoorbeeld:

- **een web** bevindt zich in de **web-groep**
- **plan een** **plan groep** zich bevindt
- U wilt **een web** en **plan een** zich bevinden binnen een **gecombineerde groep**

U doet dit verplaatsen door bewerkingen twee afzonderlijk verplaatsen in de volgende volgorde:

1. Verplaats de **webpagina een** **plan -** groep
2. Verplaatst **een web** en **plan een** **gecombineerde**groep.

U kunt een servicecertificaat App verplaatsen naar een nieuwe resourcegroep of abonnement zonder problemen. Als uw web app een SSL-certificaat dat u extern ingekocht en geüpload naar de app bevat, moet u het certificaat verwijderen voordat u de web app. U kunt bijvoorbeeld de volgende stappen uitvoeren:

1. De geüploade certificaat verwijderen uit het web app.
2. Verplaatsen van de web app.
3. Het certificaat uploaden naar het web app.

## <a name="recovery-services-limitations"></a>Recovery Services beperkingen

Verplaatsen is niet ingeschakeld voor opslag, netwerk, of bronnen die worden gebruikt voor herstel met Azure Site herstel na noodgevallen instellen berekenen. 

Stel bijvoorbeeld dat u een replicatie van de machines op ruimten naar een opslag (Storage1) hebt ingesteld en u wilt de beveiligde machine om na een failover naar Azure als een virtuele machine (VM1) gekoppeld aan een virtueel netwerk (Network1). U kunt deze Azure bronnen - Storage1 VM1 en Network1 - niet verplaatsen, via de resourcegroepen in het hetzelfde abonnement of via abonnementen.

## <a name="classic-deployment-limitations"></a>Klassieke implementatie beperkingen

De opties voor het verplaatsen van resources via het klassieke objectmodel geïmplementeerd hangt ervan af of u de bronnen binnen een abonnement of naar een nieuw abonnement wilt verplaatsen. 

### <a name="same-subscription"></a>Hetzelfde abonnement

Bij het verplaatsen van resources uit een resourcegroep aan een andere resource in het hetzelfde abonnement, gelden de volgende beperkingen:

- Virtuele netwerken (klassiek) kunnen niet worden verplaatst.
- Virtuele machines (klassiek) moet worden verplaatst met de cloud-service. 
- Cloud-service kan alleen worden verplaatst wanneer de verplaatsing alle virtuele machines bevat.
- Alleen een cloud-service kan tegelijkertijd worden verplaatst.
- Slechts één account voor opslag (klassiek) kan tegelijkertijd worden verplaatst.
- Opslag-account (klassiek) kan niet worden verplaatst in dezelfde bewerking met een virtuele machine of een cloud-service.

Klassieke om bronnen te verplaatsen naar een nieuwe bronnengroep binnen het hetzelfde abonnement, de bewerkingen standaard verplaatsen via de [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli)of [REST API](#use-rest-api)te gebruiken. U de bewerkingen die u voor het verplaatsen van Resource Manager bronnen gebruikt.

### <a name="new-subscription"></a>Nieuw abonnement

Als u bronnen naar een nieuw abonnement, gelden de volgende beperkingen:

- Alle klassieke bronnen in het abonnement moeten worden verplaatst in dezelfde bewerking.
- Het abonnement doel mag geen andere klassieke bronnen bevatten.
- De verplaatsing kan alleen worden aangevraagd via een aparte REST API voor klassieke verplaatst. De standaardopdrachten Resource Manager verplaatsen, werken niet als klassieke bronnen verplaatsen naar een nieuw abonnement.

Klassieke om bronnen te verplaatsen naar een nieuw abonnement, moet u de overige bewerkingen die specifiek voor klassieke bronnen zijn gebruiken. Voer de volgende stappen klassieke om bronnen te verplaatsen naar een nieuw abonnement.

1. Controleer als het abonnement bron deel uitmaken van het verplaatsen van een cross-abonnement. Gebruik de volgende bewerking:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     Neem in het hoofdgedeelte van de aanvraag:

         {
           "role": "source"
         }

     Het antwoord voor de validatiebewerking is in de volgende notatie:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Controleer als het abonnement bestemming deel uitmaken van het verplaatsen van een cross-abonnement. Gebruik de volgende bewerking:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     Neem in het hoofdgedeelte van de aanvraag:

         {
           "role": "target"
         }

     Het antwoord wordt in dezelfde indeling als de validatie van het abonnement.

3. Als beide abonnementen gevalideerd worden, alle klassieke bronnen verplaatsen van een abonnement op een ander abonnement met de volgende bewerking:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    Neem in het hoofdgedeelte van de aanvraag:

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

De bewerking kan worden uitgevoerd gedurende enkele minuten. 

## <a name="use-portal"></a>Portal gebruiken

Selecteer de resourcegroep met deze bronnen om bronnen te verplaatsen naar een nieuwe resourcegroep in het **hetzelfde abonnement**, en selecteer vervolgens de knop **verplaatsen** .

![resources verplaatsen](./media/resource-group-move-resources/edit-rg-icon.png)

Of om bronnen te verplaatsen naar een **Nieuw abonnement**, selecteert u de resourcegroep met deze bronnen en selecteer vervolgens het bewerkingspictogram-abonnement.

![resources verplaatsen](./media/resource-group-move-resources/change-subscription.png)

Selecteer de resources te verplaatsen en de resourcegroep bestemming. Bevestigen dat u moet de scripts voor deze resources bijwerken en klik op **OK**. Als u het pictogram van het abonnement bewerken in de vorige stap hebt geselecteerd, moet u ook het abonnement bestemming.

![bestemming selecteren](./media/resource-group-move-resources/select-destination.png)

**Meldingen**ziet u dat de verplaatsing wordt uitgevoerd.

![verplaatsen status weergeven](./media/resource-group-move-resources/show-status.png)

Wanneer deze is voltooid, wordt u gewaarschuwd van het resultaat.

![resultaat verplaatsen](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>PowerShell gebruiken

Bestaande om bronnen te verplaatsen naar een andere groep of abonnement, gebruikt u de opdracht **Move AzureRmResource** .

Het eerste voorbeeld ziet u hoe een resource verplaatsen naar een nieuwe resourcegroep.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

Het tweede voorbeeld ziet u hoe meerdere resources verplaatsen naar een nieuwe resourcegroep.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Als u wilt verplaatsen naar een nieuw abonnement, bevatten een waarde voor de parameter **DestinationSubscriptionId** .

U wordt gevraagd te bevestigen dat u wilt verplaatsen van de opgegeven resources.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Azure CLI gebruiken

Bestaande om bronnen te verplaatsen naar een andere groep of abonnement, gebruikt u de opdracht **azure resource verplaatsen** . U moet de bron-id's van de bronnen te verplaatsen. U kunt een resource-id's met de volgende opdracht:

    azure resource list -g sourceGroup --json

Die als resultaat de volgende notatie:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

In het volgende voorbeeld ziet u hoe een account opslag verplaatsen naar een nieuwe resourcegroep. Met de parameter **-i** , bieden dat een door komma's gescheiden lijst van de resource-id's te verplaatsen.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
U wordt gevraagd te bevestigen dat u wilt verplaatsen van de opgegeven resource.

## <a name="use-rest-api"></a>REST API gebruiken

Bestaande om bronnen te verplaatsen naar een andere groep of abonnement, uitvoeren:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

In het hoofdgedeelte van de aanvraag geeft u de doelgroep van de bron en de bronnen te verplaatsen. Zie [bronnen verplaatsen](https://msdn.microsoft.com/library/azure/mt218710.aspx)voor meer informatie over de verplaatsing van de REST.


## <a name="next-steps"></a>Volgende stappen
- Zie informatie over PowerShell-cmdlets voor het beheren van uw abonnement, [Met behulp van Azure PowerShell met Resource Manager](powershell-azure-resource-manager.md).
- Zie informatie over Azure CLI-opdrachten voor het beheren van uw abonnement, [met behulp van de CLI Azure met Resource Manager](xplat-cli-azure-resource-manager.md).
- Zie meer informatie over portal-functies voor het beheer van uw abonnement, [met behulp van de portal Azure voor het beheren van bronnen](./azure-portal/resource-group-portal.md).
- Zie meer informatie over het toepassen van een logische indeling op uw resources, [met labels voor het ordenen van uw resources](resource-group-using-tags.md).
