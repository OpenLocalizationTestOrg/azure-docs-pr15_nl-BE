<properties
   pageTitle="Bewerkingen met Azure CLI implementatie weergeven | Microsoft Azure"
   description="Hierin wordt beschreven hoe de Azure CLI gebruiken voor het detecteren van problemen van de implementatie van de Resource Manager."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Implementatie bewerkingen weergeven met Azure CLI

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Als u een fout ontvangen hebt bij de distributie van resources in Azure, wilt u mogelijk meer details over de implementatie van bewerkingen die zijn uitgevoerd. Azure CLI bevat opdrachten waarmee u kunt zoeken naar fouten en mogelijke oplossingen bepalen.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sommige fouten kunt u voorkomen door het valideren van uw sjabloon en infrastructuur voor implementatie. U kunt zich ook aanmelden aanvullende aanvraag en respons informatie tijdens de implementatie die handig is hoger voor het oplossen van problemen. Zie meer informatie over het valideren van en informatie over de aanvraag en het antwoord van de logboeken, [een bronnengroep met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Gebruik van controlelogboeken oplossen

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Overzicht van fouten in een implementatie, gebruik de volgende stappen uit:

1. Overzicht van de controlelogboeken, voert u de opdracht **groep azure logboek weergeven** . U kunt opnemen de **--laatste implementatie** optie voor het logboek voor de implementatie van de meest recente ophaalt.

        azure group log show ExampleGroup --last-deployment

2. De opdracht **show azure groep logboek** geeft als resultaat een grote hoeveelheid informatie. Voor het oplossen van problemen wilt meestal u zich richten op activiteiten die zijn mislukt. Het volgende script wordt de optie **--json** en het hulpprogramma [jq](https://stedolan.github.io/jq/) JSON naar het logboek zoeken naar fouten bij implementatie.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    U kunt zien dat **Eigenschappen** bevat informatie in json over de mislukte bewerking.

    U kunt de **--uitgebreide** en **vv -** opties voor meer informatie van de logs.  Gebruik de **--verbose** optie voor het weergeven van de stappen die de bewerkingen doorlopen op `stdout`. Gebruik de **vv -** optie voor de geschiedenis van een volledige aanvraag. De berichten bevatten vaak belangrijke aanwijzingen over de oorzaak van storingen.

3. Om u te richten op het statusbericht voor mislukte posten, gebruik de volgende opdracht:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Implementatie-bewerkingen gebruiken voor het oplossen van problemen met

1. De algemene status van een distributie met de opdracht **show azure groep implementatie** krijgt. In het volgende voorbeeld is de implementatie mislukt.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Als het bericht wordt weergegeven voor mislukte bewerkingen voor een implementatie gebruiken:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Volgende stappen

- Zie [veelvoorkomende fouten bij het implementeren van Azure Azure Resource Manager bronnen oplossen](resource-manager-common-deployment-errors.md)voor hulp bij het oplossen van fouten in specifieke implementatie.
- Zie informatie over het gebruik van de controlelogboeken om te controleren van andere soorten acties, [bewerkingen met Resource Manager controleren](resource-group-audit.md).
- Zie [een bronnengroep met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md)voor het valideren van uw implementatie voordat deze wordt uitgevoerd.
