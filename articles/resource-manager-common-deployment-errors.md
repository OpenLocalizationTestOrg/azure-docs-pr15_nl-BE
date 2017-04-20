<properties
   pageTitle="Problemen oplossen met veelvoorkomende fouten in Azure-implementatie | Microsoft Azure"
   description="Beschrijving van algemene fouten oplossen wanneer u resources op Azure met Azure Resource Manager implementeert."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="Fout bij implementatie, azure-implementatie, implementeren op azure"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Problemen oplossen met veelvoorkomende fouten in Azure-implementatie met Azure Resource Manager

In dit onderwerp wordt beschreven hoe u kunt oplossen door sommige gemeenschappelijke Azure-implementatiefouten kunnen optreden. Als u meer informatie over wat er misgegaan met de implementatie is, eerst Zie [implementatie-bewerkingen weergeven](resource-manager-troubleshoot-deployments-portal.md) en teruggaan naar de in dit artikel voor hulp bij het oplossen van de fout. De foutcode die u ziet een lijst in de secties in dit onderwerp.

## <a name="invalid-template"></a>Ongeldige sjabloon

Deze fout kan leiden tot verschillende typen fouten. 

### <a name="syntax-error"></a>Syntaxisfout

Als er een foutbericht wordt weergegeven dat de validatie van de sjabloon kan niet, wellicht een probleem van de syntaxis in de sjabloon. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Deze fout is heel eenvoudig omdat sjabloonexpressies kunnen ingewikkeld zijn. De naamtoewijzing van de volgende voor een opslag-account bevat bijvoorbeeld één set haken, drie functies drie sets haakjes, één set enkele aanhalingstekens en één eigenschap:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Als u niet de bijbehorende syntaxis, maakt de sjabloon voor een waarde die anders is dan uw bedoeling.

Als u dit soort fouten ontvangt, zorgvuldig door de syntaxis van expressies. U kunt overwegen een JSON-editor als [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) of [Visual Studio-Code](resource-manager-vs-code.md), kunt u worden gewaarschuwd over syntaxisfouten. 

### <a name="incorrect-segment-lengths"></a>Onjuiste segmentlengte

Een andere ongeldige sjabloon-fout treedt op wanneer de naam van de resource zich niet in de juiste indeling.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Een niveau-toegangspuntbron moet één minder segment van de naam dan in het resourcetype. Elk segment wordt onderscheiden door een schuine streep. In het volgende voorbeeld wordt het type heeft twee segmenten en de naam bestaat uit één segment is een **geldige naam**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Maar in het volgende voorbeeld is **geen geldige naam** omdat deze hetzelfde nummer als het type van segmenten heeft.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Voor onderliggende bronnen hebben het type en de naam van hetzelfde aantal segmenten. Dit aantal segmenten is logisch omdat de volledige naam en het type van de onderliggende bevat de bovenliggende naam en type. Daarom heeft de volledige naam nog steeds minder één segment dan het volledige type. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

De segmenten rechts ophalen is lastig met Resource Manager-typen die worden toegepast op resource-providers. Bijvoorbeeld moet een resource vergrendeling toepassen op een website een type met vier segmenten. Daarom is de naam van de drie segmenten:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Index van de kopie wordt niet verwacht.

Deze **InvalidTemplate** -fout optreedt wanneer u het element **kopiëren** hebt toegepast op een deel van de sjabloon die geen ondersteuning biedt voor dit element. U kunt het element kopiëren alleen toepassen op een brontype. U kunt kopiëren op een eigenschap binnen een brontype toepassen. Bijvoorbeeld: u exemplaar toepassen op een virtuele machine, maar u deze niet vereffenen met de OS-schijven voor een virtuele machine. In sommige gevallen kunt u een onderliggende bron converteren naar een bovenliggende bron voor het maken van een lus kopiëren. Zie [meerdere instanties maken van bronnen in Azure Resource Manager](resource-group-create-multiple.md)voor meer informatie over het gebruik van de kopie.

### <a name="parameter-is-not-valid"></a>Parameter is niet geldig

Als de sjabloon worden de toegestane waarden voor een parameter en u een waarde opgeeft die niet in een van deze waarden is, wordt een foutbericht met de volgende fout:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Dubbel controleren de toegestane waarden in de sjabloon en bieden een tijdens de implementatie.

## <a name="not-found-or-resource-not-found"></a>Niet gevonden (of bron niet gevonden)

Als uw sjabloon de naam van een resource dat kan worden opgelost bevat, wordt een foutbericht zoals:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Als u probeert de ontbrekende bron in de sjabloon te implementeren, moet u controleren of u moet een afhankelijkheid toevoegen. Resource Manager optimaliseert de implementatie door het maken van bronnen in parallel, indien mogelijk. Als een resource moet worden geïmplementeerd na een andere resource, moet u het element **dependsOn** gebruiken in de sjabloon voor het maken van een afhankelijkheid van de andere bron. Bijvoorbeeld bij het implementeren van een web app, moet de App serviceplan aanwezig zijn. Als u hebt opgegeven dat de web app is afhankelijk van het abonnement App, maakt Resource Manager beide bronnen op hetzelfde moment. U ontvangt een foutbericht waarin wordt aangegeven dat de App Service plan bron niet vinden, omdat het bestaat nog niet wanneer u probeert een eigenschap instellen op het web app. U kunt deze fout voorkomen door de afhankelijkheid in de web app.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Deze fout wordt ook zien wanneer de resource in een andere bronnengroep bevinden dan de versie die wordt ingezet bestaat voor. In dat geval de [resourceId functie](resource-group-template-functions.md#resourceid) gebruiken om de volledige naam van de resource.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Als u de [referentie](resource-group-template-functions.md#referenc) - of [listKeys](resource-group-template-functions.md#listkeys) -functies gebruiken met een resource dat kan worden opgelost probeert, wordt het volgende foutbericht weergegeven:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Zoekt u een expressie met de functie **verwijst** . Controleer of de waarden van de parameters juist zijn.

## <a name="storage-account-already-exists-or-already-taken"></a>Opslag-account bestaat al (of al)

Voor opslag rekeningen, moet u een naam voor de resource die is uniek voor Azure opgeven. Als u een unieke naam niet opgeeft, wordt een fout als:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Door uw naamgevingsschema met het resultaat van de functie [uniqueString](resource-group-template-functions.md#uniquestring) aaneen te schakelen kunt u een unieke naam.

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Als u een opslag-account met dezelfde naam als een bestaande account voor de opslag te in uw abonnement implementeren, maar een andere locatie, u een foutbericht wordt weergegeven dat de opslag account bestaat al op een andere locatie. Verwijder de bestaande account voor opslag of bieden dezelfde locatie als de bestaande account voor opslag.

## <a name="account-name-invalid"></a>Ongeldige accountnaam

Wanneer u probeert een opslag-account een naam te geven met verboden tekens ziet u de fout **AccountNameInvalid** . Namen van opslag moeten tussen 3 en 24 tekens bevatten en gebruik alleen cijfers en kleine letters.

## <a name="no-registered-provider-found"></a>Geen geregistreerde provider gevonden

Bij het implementeren van resources, wordt de volgende foutcode en het bericht:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Deze fout wordt weergegeven om een van drie redenen:

1. Locatie niet ondersteund voor het resourcetype
2. API versie wordt niet ondersteund voor het resourcetype
3. De resource-provider is niet geregistreerd voor uw abonnement

Het foutbericht geeft u suggesties voor de ondersteunde locaties en API-versies. U kunt de sjabloon op een van de voorgestelde waarden wijzigen. De meeste providers zijn ingeschreven, automatisch door de Azure portal of de opdrachtregelinterface die u gebruikt, maar niet alle. Als u een bepaalde resource provider voordat u niet hebt gebruikt, moet u mogelijk registreren die provider. U kunt meer informatie over providers via PowerShell of Azure CLI bron vinden.

### <a name="powershell"></a>PowerShell

Gebruik **Get AzureRmResourceProvider**overzicht van de status van uw inschrijving.

    Get-AzureRmResourceProvider -ListAvailable

**Register AzureRmResourceProvider** gebruiken voor het registreren van een provider, en geef de naam van de resource-provider die u wilt registreren.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Als u de ondersteunde locaties voor een bepaald type resource, gebruikt u:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Als u de ondersteunde versies van de API voor een bepaald type resource, gebruikt u:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure CLI

Als u wilt zien of de provider is geregistreerd, gebruikt u de `azure provider list` opdracht.

    azure provider list

Voor het registreren van een resource-provider, gebruikt u de `azure provider register` opdracht en geef de *naamruimte* te registreren.

    azure provider register Microsoft.Cdn

Overzicht van de ondersteunde locaties en API-versies voor een resource-provider, gebruikt u:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>De bewerking is niet toegestaan.

U hebt problemen implementatie meer dan een quota per resourcegroep, abonnementen, rekeningen en andere scopes kan worden. Uw abonnement kan bijvoorbeeld zodanig te beperken het aantal kernen voor een regio. Als u een virtuele machine met meer cores dan het toegestane bedrag implementeren probeert, foutbericht er een met de mededeling dat het quotum is overschreden.
Zie voor volledige quota informatie [Azure abonnement service grenzen, quota's en beperkingen](azure-subscription-service-limits.md).

Om te controleren van uw abonnement quota voor cores, kunt u de `azure vm list-usage` in de CLI Azure opdracht. In het volgende voorbeeld ziet u dat de core quota voor een gratis proefperiode 4 is:

    azure vm list-usage

Die wordt geretourneerd:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Als u een sjabloon die wordt gemaakt van meer dan vier cores in de regio West VS implementeert, krijgt u een implementatiefout die eruit als ziet:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Of in PowerShell, gebruikt u de cmdlet **Get-AzureRmVMUsage** .

    Get-AzureRmVMUsage

Die wordt geretourneerd:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

In deze gevallen moet u gaat u naar de portal en het bestand een probleem op als u wilt verhogen van het quotum voor de regio waarin u wilt implementeren.

> [AZURE.NOTE] Denk eraan dat voor resourcegroepen, het quotum voor elke afzonderlijke regio, niet voor het hele abonnement. Als u implementeren 30 cores in West VS wilt, hebt u 30 Resource Manager cores in West VS vragen. Als u implementeren 30 cores in een van de gebieden waaraan u toegang hebt moet, vraagt u 30 Resource Manager cores in alle regio's.

## <a name="invalid-content-link"></a>Ongeldige koppeling naar inhoud

Wanneer u het foutbericht ontvangt:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

U hebt waarschijnlijk geprobeerd te koppelen aan een geneste sjabloon is niet beschikbaar. Controleer de URI die u hebt opgegeven voor de geneste sjabloon. Als de sjabloon in een opslag-account bestaat, moet u dat de URI is toegankelijk. Wellicht moet u een token SAS doorgeven. Zie [gekoppelde sjablonen met Azure Resource Manager gebruiken](resource-group-linked-templates.md)voor meer informatie.

## <a name="authorization-failed"></a>Verificatie mislukt

U mogelijk een foutbericht tijdens de installatie omdat de account of de service principal willen implementeren de resources geen toegang tot deze acties uit te voeren. Azure Active Directory kunt u of de beheerder om te bepalen welke identiteiten toegang tot bronnen met een grote mate van precisie. Bijvoorbeeld, als uw account is toegewezen aan de rol van lezer, bent u niet kunnen maken van bronnen. In dat geval wordt er een foutbericht met de mededeling dat de verificatie is mislukt.

Voor meer informatie over rollen gebaseerd toegangsbeheer Zie [Toegangsbeheer Azure Role-Based](./active-directory/role-based-access-control-configure.md).

Naast de op rollen gebaseerde toegangscontrole kunnen uw implementatie acties worden beperkt door beleid op het abonnement. Via het beleid, kan de beheerder van de verdragen op alle middelen die worden ingezet in het abonnement afdwingen. Een beheerder kan bijvoorbeeld vereisen dat een bepaald label voor een brontype opgeeft. Als u voldoen niet aan de vereisten, wordt een fout tijdens de implementatie. Zie [Beleid voor netwerkbronnen beheren en toegang beheren](resource-manager-policy.md)voor meer informatie over het beleid.

## <a name="troubleshooting-virtual-machines"></a>Het oplossen van virtuele machines

| Fout | Artikelen |
| -------- | ----------- |
| Aangepast script extensie fouten | [Windows VM extensie storingen](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />of<br />[Linux VM extensie storingen](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Installatiekopie van het besturingssysteem fouten inrichten | [Nieuwe Windows VM-fouten](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />of<br />[Nieuwe Linux VM-fouten](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Fouten | [Windows VM fouten](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />of<br />[Linux VM fouten](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Secure Shell (SSH) fouten wanneer u probeert verbinding te maken | [Secure Shell verbindingen met Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Verbinding maken met de toepassing die wordt uitgevoerd op de VM fouten | [Toepassing die wordt uitgevoerd op een Windows VM](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />of<br />[Toepassing die wordt uitgevoerd op een Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Verbinding met extern bureaublad-fouten | [Verbindingen met extern bureaublad voor Windows VM](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Fouten opgelost door opnieuw distribueren | [Nieuwe Azure knooppunt voor virtuele Machine opnieuw lanceren](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Cloud-service fouten | [Cloud service problemen op te lossen](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Andere services oplossen

De volgende tabel is niet een volledige lijst met onderwerpen over probleemoplossing voor Azure. In plaats daarvan de nadruk op kwesties in verband met de implementatie of het configureren van bronnen. Als u het oplossen van problemen met een resource runtime-help, Zie de documentatie voor die Azure service.

| Service | Artikel |
| -------- | -------- |
| Automatisering | [Tips voor probleemoplossing voor veelvoorkomende fouten in Azure automatisering](./automation/automation-troubleshooting-automation-errors.md) |
| Azure Stack | [Probleemoplossing Microsoft Azure Stack](./azure-stack/azure-stack-troubleshooting.md) |
| Azure Stack | [Web Apps en Azure-Stack](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Data Factory | [Problemen met Data Factory](./data-factory/data-factory-troubleshoot.md) |
| Service-Fabric | [Algemene problemen bij het implementeren van services op Azure Service Fabric](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Site herstellen | [Bewaken en problemen oplossen met de beveiliging voor virtuele machines en fysieke servers](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Opslag | [Controleren, opsporen en oplossen van problemen met Microsoft Azure opslag](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [Oplossen van problemen bij de implementatie StorSimple apparaat](./storsimple/storsimple-troubleshoot-deployment.md) |
| SQL-Database | [Problemen met verbinding met Azure SQL-Database](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL datawarehouse | [Het datawarehouse Azure SQL oplossen](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Weten wanneer een implementatie gereed is

Wanneer alle providers weer met succes van de implementatie, Azure Resource Manager succes rapporten over een implementatie. Echter, dit bericht betekent niet noodzakelijkerwijs dat de resourcegroep ' actief is en gereed is voor uw gebruikers." Bijvoorbeeld wilt een distributie downloaden, upgrades, wacht op bronnen van niet-sjabloon of complexe schrifttypen installeren. Resource Manager weet niet wanneer deze taken worden voltooid omdat ze niet activiteiten waarmee een provider worden bijgehouden. In deze gevallen kan het enige tijd voordat uw resources gereed voor gebruik in werkelijkheid zijn zijn. Als gevolg hiervan moet u verwacht dat de implementatiestatus komt na enige tijd voordat de installatie kan worden gebruikt.

U kunt voorkomen dat Azure bevestiging implementatie, echter door het maken van een aangepast script voor de aangepaste sjabloon. Het script weet hoe om te controleren de volledige implementatie voor het hele systeem gereedheid en retourneert met succes alleen als gebruikers interactief met de gehele implementatie werken kunnen. Als u ervoor wilt zorgen dat is de uitbreiding het laatst wilt uitvoeren, gebruikt u de eigenschap **dependsOn** in uw sjabloon.

## <a name="next-steps"></a>Volgende stappen

- Zie informatie over het controleren van acties [controleverrichtingen met Resource Manager](resource-group-audit.md).
- Zie meer informatie over acties om de fouten tijdens de implementatie, [bewerkingen weergeven-implementatie](resource-manager-troubleshoot-deployments-portal.md).
