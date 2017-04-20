<properties
    pageTitle="Fout bij het toewijzen van Cloud Service Troubleshooting | Microsoft Azure"
    description="Fout bij toewijzing van het oplossen van problemen wanneer u Services in Azure Cloud"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Fout bij toewijzing van het oplossen van problemen wanneer u Services in Azure Cloud

## <a name="summary"></a>Samenvatting
Wanneer u exemplaren met een Cloud Service implementeren of toevoegen van nieuwe webpagina of nieuw werknemer rol exemplaren, wijst Microsoft Azure compute-bronnen. Soms verschijnt er fouten bij het uitvoeren van deze bewerkingen nog voordat u de abonnementen Azure bereikt. In dit artikel wordt uitgelegd van de oorzaken van een aantal van de voorkomende fouten en stelt mogelijke oplossing voor problemen. De gegevens zijn mogelijk ook handig bij het plannen van de implementatie van uw diensten.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Achtergrond: de werking van toewijzing
De servers in datacenters Azure worden gepartitioneerd in clusters. Een nieuwe aanvraag voor cloud service toewijzing wordt uitgevoerd in meerdere clusters. Wanneer het eerste exemplaar wordt geïmplementeerd naar een cloud-service (in de gefaseerde installatie of productie), die service cloud wordt vastgemaakt aan een cluster. Eventuele verdere implementaties voor de service cloud gebeurt in hetzelfde cluster. In dit artikel verwijzen we naar dit als 'vastgemaakt aan een cluster'. Afbeelding 1 hieronder illustreert het geval van een normale verdeling die wordt uitgevoerd in meerdere clusters; Diagram 2 ziet u het geval van een toewijzing die is vastgemaakt aan het Cluster 2 want dat is waar de bestaande CS_1 voor Cloud-Service wordt gehost.

![Toewijzing van Diagram](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Waarom de fout bij de toewijzing gebeurt
Wanneer een aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, is er een hogere kans op vrije bronnen vinden omdat de beschikbare resourcegroep beperkt tot een cluster is niet. Voorts als uw aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, maar de soort resource die u hebt aangevraagd wordt niet ondersteund door de cluster, uw aanvraag niet zelfs als het cluster gratis resource heeft. Diagram 3 hieronder ziet u het geval waarin een vastgezette toewijzing mislukt, omdat het cluster enige kandidaat geen vrije bronnen heeft. Afbeelding 4 illustreert het geval waarin een vastgezette toewijzing mislukt, omdat de grootte van de aangevraagde VM biedt geen ondersteuning voor het cluster enige kandidaat, hoewel het cluster vrije bronnen heeft.

![Fout bij het toewijzen van vaste](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Fout bij het toewijzen van cloud-services oplossen
### <a name="error-message"></a>Foutbericht
Mogelijk ziet u het volgende foutbericht weergegeven:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Veelvoorkomende problemen
Hier vindt u de algemene toewijzing van scenario's die ertoe leiden een aanvraag voor geheugentoewijzing dat te worden vastgemaakt aan een cluster.

- Implementeert voor Staging-sleuf - als een cloud-service een implementatie in een sleuf heeft, is vervolgens de hele cloud-service vastgemaakt aan een specifiek cluster.  Dit betekent dat als een implementatie al in de productie-sleuf bestaat, vervolgens een nieuwe tijdelijke implementatie kan alleen worden toegewezen in hetzelfde cluster als de productie-sleuf. Als het cluster capaciteit nadert, mislukt de aanvraag.

- Schalen - nieuwe exemplaren toe te voegen aan een bestaande cloud-service moet in hetzelfde cluster toewijzen.  Kleine schaal aanvragen kan meestal worden toegewezen, maar niet altijd. Als het cluster capaciteit nadert, mislukt de aanvraag.

- Affiniteit groep - een nieuwe installatie met een lege cloud-service kan worden toegewezen door de stof in een cluster in die regio, tenzij de service cloud is vastgemaakt aan een groep affiniteit. Implementaties dezelfde groep affiniteit wordt geprobeerd in hetzelfde cluster. Als het cluster capaciteit nadert, mislukt de aanvraag.

- Affiniteit groep vNet - oudere virtuele netwerken zijn gebonden aan affiniteit groepen in plaats van de regio's en services in deze virtuele netwerken cloud zou worden vastgemaakt aan het cluster affiniteit groep. Implementaties op dit type virtuele netwerk wordt geprobeerd op het cluster vastgemaakt. Als het cluster capaciteit nadert, mislukt de aanvraag.

## <a name="solutions"></a>Oplossingen

1. Implementeren naar een nieuwe wolk service - deze oplossing is waarschijnlijk meest succesvolle omdat het platform te kiezen uit alle clusters in die regio.

    - De werklast voor een nieuwe wolk service implementeren  

    - De CNAME- of een record als verkeer naar de nieuwe wolk service bijwerken

    - Zodra de oude site nul verkeer gaat is, kunt u de oude cloud-service verwijderen. Deze oplossing moet nul uitvaltijd oplopen.

2. Verwijderen van productie en staging-sleuven: deze oplossing zal uw bestaande DNS-naam, maar behouden uitvaltijd voor uw toepassing, wordt.

    - De productie en klaarzetten van sleuven van een bestaande cloud-service, zodat de service cloud leeg is, verwijderen en vervolgens

    - Maak een nieuwe distributie in de bestaande cloud-service. Deze wordt toewijzing voor alle clusters in de regio opnieuw geprobeerd. Controleer dat de cloud-service is niet gebonden aan een groep affiniteit.

3. Gereserveerde IP - deze oplossing behoudt u uw bestaande IP adres, maar wordt leiden tot uitvaltijd voor uw toepassing.  

    - Een optie voor uw bestaande implementatie met Powershell maken

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Volg #2 boven en zorg dat de optie nieuwe opgeven in de CSCFG van de service.

4. Verwijder groep affiniteit voor nieuwe implementaties - affiniteit groepen niet langer worden aanbevolen. Stappen voor #1 hierboven om een nieuwe wolk service implementeren. Controleer de cloud-service is niet in een groep affiniteit.

5. Converteren naar een regionale virtueel netwerk - Zie [migreren van affiniteit groepen met regionale virtueel netwerk (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
