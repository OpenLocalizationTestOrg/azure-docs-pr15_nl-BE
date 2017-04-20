<properties
    pageTitle="Verbinding maken met Azure Stack met PowerShell | Microsoft Azure"
    description="Informatie over het beheren van Azure Stack met PowerShell"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-powershell-and-connect-to-azure-stack"></a>PowerShell installeren en aansluiten op de stapel Azure
In deze handleiding doorlopen we de stappen voor het verbinden met Azure Stack met PowerShell. Zodra voltooid, kunt deze stappen u resources implementeren en beheren.

## <a name="install-azure-stack-powershell-cmdlets"></a>Azure Stack PowerShell cmdlets installeert

1.  AzureRM-cmdlets worden geïnstalleerd in de galerie met PowerShell. Open een PowerShell Console op MAS CON01 en de volgende opdracht een lijst opgevraagd van de opslagplaatsen van PowerShell beschikbaar om te beginnen:

        Get-PSRepository

      ![Screenshot resultaat van de lopende 4Get-PSRepository met PSGallery die worden vermeld](./media/azure-stack-connect-powershell/image1.png)

2.  Voer de volgende opdracht om de module AzureRM installeren:

        Install-Module -Name AzureRM -RequiredVersion 1.2.6 -Scope CurrentUser

    >[AZURE.NOTE] *-Scope CurrentUser* is optioneel. Als u meer dan de huidige gebruiker toegang hebben tot de modules wilt, gebruikt u een opdrachtprompt en laat uit de parameter *Scope* .

3.  Bevestig de installatie van modules AzureRM door de volgende opdrachten worden uitgevoerd:

        Get-Command -Module AzureRM.AzureStackAdmin

## <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack
Een module is beschikbaar voor download, dat zorgt voor de verbinding PowerShell met Azure Stack voor u configureren.  Ga naar [Azure Stack-hulpprogramma's](http://aka.ms/ConnectToAzureStackPS) voor de module en extra stappen. 

## <a name="retrieve-a-list-of-subscriptions"></a>Een lijst met abonnementen ophalen
In deze sectie controleert u PowerShell cmdlets worden uitgevoerd tegen Azure Stack ophalen en selecteert u een abonnement voor het gebruik.

Voer de volgende opdracht op te halen van een lijst van Azure Stack abonnementen aan uw account gekoppeld:

    Get-AzureRmSubscription


## <a name="next-steps"></a>Volgende stappen
[Sjablonen met PowerShell implementeren](azure-stack-deploy-template-powershell.md)

[Verbinding maken met Azure CLI](azure-stack-connect-cli.md)

[Sjablonen met Visual Studio implementeren](azure-stack-deploy-template-visual-studio.md)


