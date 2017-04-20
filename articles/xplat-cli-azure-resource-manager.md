
<properties
    pageTitle="Beheren van resources met de CLI Azure | Microsoft Azure"
    description="Azure opdrachtregelinterface (CLI) gebruiken voor het beheren van Azure bronnen en groepen"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>De Azure CLI gebruiken voor het beheren van Azure resources en resourcegroepen


> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST-API](resource-manager-rest-api.md)


De Azure opdrachtregelinterface (CLI Azure) is een van de verschillende hulpmiddelen waarmee u kunt implementeren en beheren van bronnen met Resource Manager. In dit artikel worden algemene methoden voor het beheren van Azure resources en resourcegroepen met behulp van de CLI Azure in modus Resource Manager geïntroduceerd. Zie voor informatie over het implementeren van bronnen met behulp van de CLI, [resources, distribueren met sjablonen Resource Manager en Azure CLI](resource-group-template-deploy-cli.md). Ga naar de [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md)voor achtergrondinformatie over Azure resources en Resource Manager.

>[AZURE.NOTE] Azure resources met de Azure CLI beheren, moet u [de Azure CLI installeren](xplat-cli-install.md)en [aanmelden bij Azure](xplat-cli-connect.md) met behulp van de `azure login` opdracht. Zorg ervoor dat bronbeheer-modus is de CLI (uitvoeren `azure config mode arm`). Als u deze dingen hebt gedaan, bent u klaar om te gaan.



## <a name="get-resource-groups-and-resources"></a>Ophalen van bronnengroepen en bronnen

### <a name="resource-groups"></a>Resourcegroepen

Als u een lijst van alle resourcegroepen in uw abonnement en hun locaties, kunt u deze opdracht uitvoert.

    azure group list
    

### <a name="resources"></a>Bronnen
 U kunt alle bronnen in een groep, zoals een lijst met de naam *testRG*, met de volgende opdracht te gebruiken.

    azure resource list testRG

Gebruik voor een afzonderlijke resource in de groep weergeven, zoals een VM met de naam *MyUbuntuVM*, een opdracht als volgt uit.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
U ziet de parameter **Microsoft.Compute/virtualMachines** . Deze parameter geeft het type van de aangevraagde gegevens op resource.
    
>[AZURE.NOTE]Wanneer u de opdrachten **azure resource** dan met de opdracht **lijst** gebruikt, moet u de versie van de API van de resource met de parameter **-o** . Als u over de API-versie twijfelt, raadpleegt u het sjabloonbestand en het veld apiVersion vinden voor de resource. Zie voor meer informatie over API-versies in Resource Manager [Resource Manager providers, regio's, API-versies en schema's](resource-manager-supported-services.md).

Bij het bekijken van details van een bron, is het vaak handig gebruik van de `--json` parameter. Met deze parameter kunt u de uitvoer beter leesbaar, omdat sommige waarden geneste structuur of collecties. In het volgende voorbeeld ziet u de resultaten van de opdracht **show** als een JSON-document.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Als u wilt, de JSON gegevens opslaan in bestand met behulp van de &gt; teken de uitvoer naar een bestand. Bijvoorbeeld:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Tags

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Bronnen beheren


Als een resource bijvoorbeeld een opslag-account toevoegen aan een resourcegroep, uitvoeren van een opdracht als:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Naast het opgeven van de API-versie van de resource met de parameter **-o** , gebruik de parameter **-p** opgemaakte JSON string met alle vereiste of aanvullende eigenschappen doorgeven.
    
    
U verwijdert een bestaande bron zoals een bron van de virtuele machine, een opdracht als de volgende te gebruiken.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Bestaande om bronnen te verplaatsen naar een andere groep of abonnement, gebruikt u de opdracht **azure resource verplaatsen** . In het volgende voorbeeld ziet u hoe een bestand Vgx. Cache verplaatsen naar een nieuwe resourcegroep. Met de parameter **-i** , bieden dat een door komma's gescheiden lijst van de resource-id's te verplaatsen.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Toegang tot bronnen beheren

U kunt de CLI Azure maken en beheren van beleid om toegang tot bronnen Azure. Zie [beleid voor het beheren van bronnen en toegang gebruiken](resource-manager-policy.md)voor achtergrond over beleidsdefinities en het beleid toewijzen aan resources.

Bijvoorbeeld het volgende beleid voor het weigeren van alle aanvragen waarbij locatie geen West VS of North Central ons is definiëren en opslaan op het beleid definitie bestand policy.json:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Voer de opdracht **maken van de definitie van beleid** :

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Deze opdracht geeft u de volgende uitvoer.

    + Beleid definitie MyPolicy gegevens gemaakt: PolicyName: gegevens voor MyPolicy: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    gegevens: PolicyType: aangepaste gegevens: DisplayName: undefined gegevens: Beschrijving: niet-gedefinieerde gegevens: PolicyRule: veld locatie =, = [westus, northcentralus], in effect = weigeren

 Een beleid voor het bereik dat u wilt gebruiken de **PolicyDefinitionId** die wordt geretourneerd uit de vorige opdracht wilt toewijzen. In het volgende voorbeeld wordt dit bereik is het abonnement, maar u kunt het bereik aan resourcegroepen of afzonderlijke resources:

    Azure beleidstoewijzing maken MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

U kunt ophalen, wijzigen of verwijderen van beleidsdefinities met behulp van de opdrachten van de **definitie van beleid weergeven**, **definitie van beleid**en **beleid definitie verwijderen** .

U kunt ook ophalen, wijzigen of verwijderen van toewijzingen van beleid met behulp van de opdrachten **beleidstoewijzing weergeven**, **toewijzing van beleid**en **toewijzing van beleid verwijderen** .


## <a name="export-a-resource-group-as-a-template"></a>Een resourcegroep exporteren als een sjabloon

Voor een bestaande resourcegroep, kunt u de sjabloon Resource Manager voor de resourcegroep bekijken. De sjabloon exporteren biedt twee voordelen:

1. Omdat alle infrastructuur is gedefinieerd in de sjabloon kunt u eenvoudig de toekomstige implementaties van de oplossing automatiseren.

2. U kunt vertrouwd zijn met de sjabloonsyntaxis van de worden door te kijken naar de JSON waarmee uw oplossing.

Met behulp van de CLI Azure, kunt u exporteren van een sjabloon met de huidige status van de resourcegroep, of de sjabloon die is gebruikt voor een bepaalde implementatie van downloaden.

* **De sjabloon voor een resourcegroep exporteren** - dit is handig wanneer u aanbrengt in een resourcegroep wijzigingen en moet de JSON representatie van de huidige toestand ophalen. De gegenereerde sjabloon bevat echter alleen een minimum aantal parameters en geen variabelen. De meeste van de waarden in de sjabloon zijn hardcoded. Voordat u de gegenereerde sjabloon implementeert, wilt u mogelijk meer waarden converteren naar parameters, zodat u de implementatie voor verschillende omgevingen kan aanpassen.

    Uitvoeren om de sjabloon voor een resourcegroep exporteren naar een lokale map, de `azure group export` opdracht, zoals in het volgende voorbeeld wordt getoond. (Vervangen door een lokale map voor uw besturingssysteemomgeving).

        azure group export testRG ~/azure/templates/

* **Download de sjabloon voor een bepaalde implementatie van** --dit is handig als u nodig hebt om de werkelijke sjabloon die is gebruikt voor het implementeren van resources weer te geven. De sjabloon bevat alle parameters en variabelen die zijn gedefinieerd voor de oorspronkelijke implementatie. Echter als iemand in uw organisatie wijzigingen in de resourcegroep buiten de definitie in de sjabloon aangebracht, deze sjabloon niet staat voor de huidige status van de resourcegroep.

    Uitvoeren om te downloaden van de sjabloon voor een bepaalde implementatie van een lokale map gebruikt, de `azure group deployment template download` opdracht. Bijvoorbeeld:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Sjabloon exporteren in het voorbeeld is en niet alle typen momenteel ondersteuning voor het exporteren van een sjabloon. Bij het exporteren van een sjabloon, wordt er een fout gemeld dat sommige bronnen zijn niet geëxporteerd. Indien nodig, handmatig deze bronnen in de sjabloon te definiëren nadat het is gedownload.



## <a name="next-steps"></a>Volgende stappen

* Zie voor details van de implementatie en oplossen van implementatiefouten met de CLI Azure, [implementatie bewerkingen weergeven met Azure CLI](resource-manager-troubleshoot-deployments-cli.md).
* Als u de CLI gebruiken wilt voor het instellen van een toepassing of het script toegang krijgen tot bronnen, Zie [Azure CLI gebruiken voor het maken van een service principal toegang krijgen tot bronnen](resource-group-authenticate-service-principal-cli.md).


