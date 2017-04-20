<properties
   pageTitle="Bewerkingen met PowerShell implementatie weergeven | Microsoft Azure"
   description="Hierin wordt beschreven hoe de Azure PowerShell gebruiken voor het detecteren van problemen van de implementatie van de Resource Manager."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Implementatie bewerkingen weergeven met Azure PowerShell

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

U kunt de bewerkingen voor een implementatie van de Azure PowerShell weergeven. Kan zijn dat u graag de bewerkingen weergeven wanneer heb je een fout tijdens de implementatie zodat dit artikel richt zich op het weergeven van bewerkingen die zijn mislukt. PowerShell biedt cmdlets waarmee u kunt eenvoudig zoeken de fouten en mogelijke oplossingen bepalen.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sommige fouten kunt u voorkomen door het valideren van uw sjabloon en voorafgaand aan de distributie-infrastructuur. U kunt zich ook aanmelden aanvullende aanvraag en respons informatie tijdens de implementatie die handig is hoger voor het oplossen van problemen. Zie meer informatie over het valideren van en informatie over de aanvraag en het antwoord van de logboeken, [een bronnengroep met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Implementatie-bewerkingen gebruiken voor het oplossen van problemen met

1. Als u de algemene status van een distributie, gebruikt u de opdracht **Get-AzureRmResourceGroupDeployment** . U kunt de resultaten filteren voor alleen die implementaties die zijn mislukt.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Die de mislukte implementaties resulteert in de volgende notatie:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Elke distributie bestaat meestal uit meerdere bewerkingen, met elke bewerking die een stap in het implementatieproces. Om te ontdekken wat er is misgegaan bij een implementatie, moet u meestal informatie over de implementatie van bewerkingen. Hier ziet u de status van de bewerkingen met **Get-AzureRmResourceGroupDeploymentOperation**.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Die als resultaat meerdere bewerkingen met elkaar in de volgende notatie:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Als u meer informatie over mislukte bewerkingen, haalt de eigenschappen van transacties met de status **mislukt** mee.

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Dat het resultaat van de mislukte bewerkingen met elkaar in de volgende notatie:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Opmerking De tracerings-ID voor de bewerking. U gebruikt die in de volgende stap te richten op een bepaalde bewerking.

4. Als u het statusbericht van een bepaalde bewerking is mislukt, gebruikt u de volgende opdracht:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Die wordt geretourneerd:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Gebruik van controlelogboeken oplossen

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Overzicht van fouten in een implementatie, gebruik de volgende stappen uit:

1. Voer de opdracht **Get-AzureRmLog** voor het ophalen van items in het logboek. U kunt de parameters **ResourceGroup** en **Status** retourneert alleen de gebeurtenissen die voor één brongroep is mislukt. Als u geen waarde voor een begin- en tijd opgeeft, worden het laatste uur gegeven.
Bijvoorbeeld voor het ophalen van de mislukte bewerkingen voor het afgelopen uur uitgevoerd:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    U kunt een bepaalde tijdsduur opgeven. In het volgende voorbeeld, zullen we voor mislukte acties voor de laatste dag. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Of u kunt een exacte begin- en eindtijd voor mislukte acties instellen:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Als deze opdracht geeft te veel vermeldingen en eigenschappen, kunt u zich concentreren uw controle-inspanningen door de eigenschap **Properties** ophalen. We nemen we ook de parameter **DetailedOutput** om te zien de foutberichten worden weergegeven.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Die eigenschappen van de logboekvermeldingen resulteert in de volgende notatie:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Op basis van deze resultaten, we gaan richten op het tweede element. U kunt de resultaten verfijnen door te kijken naar het statusbericht voor dat item.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Die wordt geretourneerd:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Volgende stappen

- Zie [veelvoorkomende fouten bij het implementeren van Azure Azure Resource Manager bronnen oplossen](resource-manager-common-deployment-errors.md)voor hulp bij het oplossen van fouten in specifieke implementatie.
- Zie informatie over het gebruik van de controlelogboeken om te controleren van andere soorten acties, [bewerkingen met Resource Manager controleren](resource-group-audit.md).
- Zie [een bronnengroep met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md)voor het valideren van uw implementatie voordat deze wordt uitgevoerd.

