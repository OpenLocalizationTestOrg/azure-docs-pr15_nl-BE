<properties
    pageTitle="Azure Resource Manager-sjablonen gebruiken in Azure Stack (huurder ontwikkelaars) | Microsoft Azure"
    description="Informatie over het Azure Resource Manager-sjablonen gebruiken in Azure stapel te implementeren en inrichten van alle bronnen voor uw toepassing in een enkele, gecoördineerde werking."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Resource Manager-sjablonen gebruiken in Azure stapel

Azure Resource Manager sjablonen installeren en inrichten van alle bronnen voor uw toepassing in een enkele, gecoördineerde werking. U definieert de middelen voor de toepassing en hoe deze worden geïmplementeerd.  U kunt ook sjablonen als u wijzigingen aanbrengen in de resources in de resourcegroep wilt implementeren.

Deze sjablonen kunnen worden geïmplementeerd met de portal Stack van Microsoft Azure, PowerShell de opdrachtregel en Visual Studio.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

De volgende sjablonen zijn beschikbaar op [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-non-high-availability"></a>Implementatie van SharePoint (niet voor hoge beschikbaarheid)

Gebruik de extensie PowerShell DSC voor het maken van een farm met SharePoint 2013 die het volgende bevat:

-   Een virtueel netwerk

-   Drie accounts voor opslag

-   Twee externe Netwerktaakverdeling

-   Een VM geconfigureerd als een domeincontroller voor een nieuw forest met één domein

-   Een VM geconfigureerd als een zelfstandige server met SQL Server 2014

-   Een VM farm configureren een machine met één SharePoint 2013

## <a name="deploy-ad-non-high-availability"></a>Implementeren AD (niet voor hoge beschikbaarheid)

Gebruik de extensie PowerShell DSC voor het maken van een AD-domeincontroller-server die het volgende bevat:

-   Een virtueel netwerk

-   Één account voor opslag

-   Een externe taakverdeling

-   Een VM geconfigureerd als een domeincontroller voor een nieuw forest met één domein

## <a name="deploy-adsql-non-high-availability"></a>Implementeren AD/SQL (niet voor hoge beschikbaarheid)

Gebruik de extensie PowerShell DSC voor het maken van een zelfstandige server is SQL Server 2014 die het volgende bevat:

-   Een virtueel netwerk

-   Twee opslag rekeningen

-   Een externe taakverdeling

-   Een VM geconfigureerd als een domeincontroller voor een nieuw forest met één domein

-   Een VM geconfigureerd als een zelfstandige server met SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Gebruik de extensie PowerShell DSC een bestaande virtuele machine lokale Configuration Manager (LCM) configureren en registreren op een Azure Account DSC Pull-automatiseringsserver.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Een virtuele machine maken van een installatiekopie van de gebruiker

Een virtuele machine van een aangepaste installatiekopie maken. Deze sjabloon implementeert ook een virtueel netwerk (met DNS) en openbare IP-adres een netwerkinterface.

## <a name="simple-vm"></a>Eenvoudige VM

Een eenvoudige Windows VM met een virtueel netwerk (met DNS), openbaar IP-adres en een netwerk-interface implementeren.

## <a name="cancel-a-running-template-deployment"></a>Een sjabloonimplementatie van de waarop annuleren

Om te annuleren een actieve sjabloonimplementatie, gebruikt u de `Stop-AzureRmResourceGroupDeployment` PowerShell-cmdlet.


## <a name="next-steps"></a>Volgende stappen

[Sjablonen met de portal implementeren](azure-stack-deploy-template-portal.md)

[Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md)

