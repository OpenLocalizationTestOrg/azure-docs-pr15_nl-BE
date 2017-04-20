<properties
   pageTitle="Een Internet facing taakverdeling in Resource Manager, met behulp van een sjabloon maken | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding taakverdeling in Resource Manager, met behulp van een sjabloon"
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

# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Een internetverbinding taakverdeling met behulp van een sjabloon maken

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op het implementatiemodel Resource Manager. U kunt ook [informatie over het maken van een internetverbinding met klassieke implementatiemodel taakverdeling](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met behulp van klik te implementeren

De sjabloon monster beschikbaar in de openbare bibliotheek gebruikt een parameterbestand met de standaard waarden worden gebruikt voor het genereren van de hierboven beschreven scenario. Klik op **Deploy om Azure**, standaardparameterwaarden indien nodig vervangen voor de implementatie van deze sjabloon met Klik implementeren, volgt u [deze koppeling](http://go.microsoft.com/fwlink/?LinkId=544801)en volg de instructies in de portal.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon distribueren via PowerShell

Volg de onderstaande stappen voor de implementatie van de sjabloon die u hebt gedownload met behulp van PowerShell.

1. Als u Azure PowerShell nog nooit hebt gebruikt, Zie [installeren en configureren van Azure PowerShell](../../articles/powershell-install-configure.md) en volg de instructies helemaal naar het einde ondertekenen in Azure en selecteer uw abonnement.

2. Voer de cmdlet **New-AzureRmResourceGroupDeployment** wilt maken van een groep met behulp van de sjabloon.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon distribueren met behulp van de CLI Azure

Volg de onderstaande stappen om de sjabloon implementeert met behulp van de CLI Azure.

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.
2. Voer de opdracht **azure config-modus** wilt overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

        azure config mode arm

    Dit is de verwachte output van de bovenstaande opdracht:

        info:    New mode is arm

3. Vanuit uw browser, Ga naar [De sjabloon Quickstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), Kopieer de inhoud van het bestand json en plak in een nieuw bestand op uw computer. In dit scenario zou u de waarden onder kopiëren naar een bestand met de naam **c:\lb\azuredeploy.parameters.json**.
4. Voer de cmdlet **implementatie azure groep maken** voor de implementatie van de nieuwe taakverdeling met behulp van de sjabloon en parameter bestanden u hebt gedownload en hierboven is gewijzigd. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
