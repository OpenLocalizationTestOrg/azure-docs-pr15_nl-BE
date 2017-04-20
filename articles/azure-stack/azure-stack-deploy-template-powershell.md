<properties
    pageTitle="Sjablonen met PowerShell in Azure stapel implementeren | Microsoft Azure"
    description="Informatie over het implementeren van een virtuele machine met behulp van een sjabloon Resource Manager en PowerShell."
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
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Sjablonen in Azure Stack met PowerShell implementeren

Gebruik PowerShell Azure Resource Manager templates implementeren op de Stack Azure Implementatiemodel.  Resource Manager templates implementeren en alle bronnen voor uw toepassing in een enkele, gecoördineerde werking ter beschikking stellen.

## <a name="run-azurerm-powershell-cmdlets"></a>Uitvoeren van AzureRM PowerShell-cmdlets

In dit voorbeeld wordt uitvoeren u een script om een virtuele machine op Azure Stack Haalbaarheidstest implementeren met behulp van de sjabloon voor een Resource Manager.  Controleer voordat u verdergaat, dat u hebt [geïnstalleerd en geconfigureerd PowerShell](azure-stack-connect-powershell.md)  

De VHD gebruikt in dit voorbeeldsjabloon is een standaardafbeelding marketplace (WindowsServer-2012-R2-Datacenter).

1.  Ga naar <http://aka.ms/AzureStackGitHub>, zoekt u de sjabloon **101 eenvoudige-windows vm** en op te slaan op de volgende locatie: c:\\sjablonen\\azuredeploy-101-eenvoudig-windows-vm.json.

2.  Het volgende script voor implementatie in PowerShell, worden uitgevoerd. Vervangen door *gebruikersnaam* en *wachtwoord* voor uw gebruikersnaam en wachtwoord. Op latere wordt gebruikt, verhogen de waarde voor de parameter *$myNum* om te voorkomen dat uw implementatie worden overschreven.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  De Stack Azure portal opent, klikt u op **Bladeren**, klik op **virtuele machines**en zoek naar uw nieuwe virtuele machine (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Voorbeeld van de video: implementatie van hybride virtuele machines

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Volgende stappen

[Sjablonen met Visual Studio implementeren](azure-stack-deploy-template-visual-studio.md)
