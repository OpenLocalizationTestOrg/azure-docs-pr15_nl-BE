<properties
   pageTitle="Maak een interne taakverdeling met behulp van een sjabloon in Resource Manager | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling met behulp van een sjabloon in Resource Manager"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-a-template"></a>Een interne taakverdeling met behulp van een sjabloon maken

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met behulp van klik te implementeren

De sjabloon monster beschikbaar in de openbare bibliotheek gebruikt een parameterbestand met de standaard waarden worden gebruikt voor het genereren van de hierboven beschreven scenario. Klik op **Deploy om Azure**, standaardparameterwaarden indien nodig vervangen voor de implementatie van deze sjabloon met Klik implementeren, volgt u [deze koppeling](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer)en volg de instructies in de portal.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon distribueren via PowerShell

Volg de onderstaande stappen voor de implementatie van de sjabloon die u hebt gedownload met behulp van PowerShell.

1. Als u Azure PowerShell nog nooit hebt gebruikt, Zie [installeren en configureren van Azure PowerShell](../../articles/powershell-install-configure.md) en volg de instructies helemaal naar het einde ondertekenen in Azure en selecteer uw abonnement.
2. Download het parameterbestand naar de lokale vaste schijf.
3. Het bestand bewerken en opslaan.
4. Voer de cmdlet **New-AzureRmResourceGroupDeployment** wilt maken van een groep met behulp van de sjabloon.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon distribueren met behulp van de CLI Azure

Volg de onderstaande stappen om de sjabloon implementeert met behulp van de CLI Azure.

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.
2. Voer de opdracht **azure config-modus** wilt overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

        azure config mode arm

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    New mode is arm

3. De parameterbestand openen en selecteert u de inhoud opslaan als een bestand op uw computer. Bijvoorbeeld: we het parameterbestand in *parameters.json*opgeslagen.

4. Voer de opdracht **implementatie azure groep maken** voor de implementatie van de nieuwe interne taakverdeling met behulp van de sjabloon en parameter bestanden u hebt gedownload en hierboven is gewijzigd. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Volgende stappen

[Configureer een load balancer distributie modus bron-IP-affiniteit](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)



