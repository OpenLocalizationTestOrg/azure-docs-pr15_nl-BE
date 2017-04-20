<properties
   pageTitle="PowerShell-cmdlets gebruiken met RemoteApp-Azure | Microsoft Azure"
   description="Informatie over het gebruik van Windows PowerShell-cmdlets in Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Windows PowerShell-cmdlets gebruiken met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

 U kunt de Azure RemoteApp-PowerShell-cmdlets beheren en onderhouden van uw collecties. Gebruik de volgende informatie aan de slag.

## <a name="get-the-cmdlets"></a>De cmdlets ophalen 
-------------
Download eerst de Azure Powershell-cmdlets [hier](http://go.microsoft.com/?linkid=9811175)de cmdlets zijn opgenomen in het RemoteApp. 

Bekijk de [Azure RemoteApp cmdlet helpen](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Azure-cmdlets voor het gebruik van uw abonnement configureren
------------------
Volg [deze handleiding](../powershell-install-configure.md) zodat u de cmdlets tegen uw abonnement Azure gebruiken kunt.

Deze stappen kunt u snel aan de slag:

1.  Download en installeer de [Azure PowerShell-cmdlets](http://go.microsoft.com/?linkid=9811175).
2.  Start Microsoft Azure PowerShell.
3.  **Toevoegen-AzureAccount** voor het verifiëren van uw abonnement op Azure worden uitgevoerd. Wanneer dat wordt gevraagd, voert u de gebruikersnaam en het wachtwoord die u gebruikt voor aanmelding bij Azure portal.  
4.  **Get AzureSubscription** als u de abonnementen die zijn gekoppeld aan uw gebruikersaccount uitgevoerd. 
5.  **Selecteer AzureSubscription** uitgevoerd en geef de naam van abonnement of -ID te gebruiken in de console met PowerShell.

Gefeliciteerd, uw console Azure PowerShell is geconfigureerd en klaar voor gebruik. Houd er rekening mee dat u moet herhaalt stap 2 tot en met 5 als u start de de console Azure PowerShell.  

## <a name="create-a-cloud-collection"></a>Een collectie cloud maken
--------------------
Het is eenvoudig, voert u de volgende opdracht:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

De bovenstaande opdracht wordt automatisch gepubliceerd toepassingen van Microsoft Office 365 (Excel, OneNote, Outlook, PowerPoint, Visio en Word).

Maken van een siteverzameling kan duren 30 minuten of langer duren. Deze opdracht retourneert daarom een tracerings-ID kunt u als volgt:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Nadat u de collectie is voltooid, kunt u gebruikers toevoegen aan de collectie met de volgende opdracht:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

En u bent klaar! Deze gebruiker moet verbinding kunnen maken met de toepassing gebruikt de Azure RemoteApp-client gevonden [hier](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Beschikbare cmdlets
Er zijn een heleboel andere opdrachten die we hebben, de documentatie voor deze zal binnenkort worden komen:

Basic collectie RemoteApp-cmdlets: 

- Nieuwe AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Set AzureRemoteAppCollection
- Update AzureRemoteAppCollection
- Verwijderen AzureRemoteAppCollection
- Toevoegen-AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Verwijderen AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Verbinding verbreken AzureRemoteAppSession
- Aanroepen van AzureRemoteAppSessionLogoff
- Verzenden-AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Publiceren AzureRemoteAppProgram
- Publicatie AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

RemoteApp-virtueel netwerk cmdlets:

- Nieuwe AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Set AzureRemoteAppVNet
- Verwijderen AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Get - AzureRemoteAppVpnDeviceConfigScript
- Beginwaarden AzureRemoteAppVpnSharedKey

RemoteApp-sjabloon afbeelding cmdlets:

- Nieuwe AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Naam AzureRemoteAppTemplateImage
- Verwijderen AzureRemoteAppTemplateImage

Andere RemoteApp-cmdlets:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Set AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
