<properties
    pageTitle="Distribueren van sjablonen op de opdrachtregel in Azure Stack | Microsoft Azure"
    description="Informatie over het distribueren van sjablonen uit in de ClientVM of na gebruik van de VPN-verbinding verbinding maken met Azure Stack met de cross-platform command line interface (CLI)."
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
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Distribueren van sjablonen in Azure stapel via de opdrachtregel

Gebruik de opdracht Azure Resource Manager templates implementeren op de Stack Azure Implementatiemodel. Azure Resource Manager sjablonen installeren en inrichten van de middelen voor de toepassing in een enkele, gecoördineerde werking.

## <a name="download-template"></a>Sjabloon downloaden        
Om te testen in een distributie met de CLI, download de bestanden azuredeploy.json en azuredeploy.parameters.json uit de [opslag account voorbeeldsjabloon maken](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Sjabloon distribueren
Ga naar de map waarin deze bestanden zijn gedownload en de volgende opdracht voor de implementatie van de sjabloon:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Met deze opdracht implementeert de sjabloon naar de resource groep **cliRG** in de standaardlocatie voor de Azure Stack Implementatiemodel.

## <a name="validate-template-deployment"></a>Voor sjabloonimplementatie valideren
Overzicht van deze resource en de opslag-account, kunt u de volgende opdrachten:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Volgende stappen

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
