<properties
   pageTitle="Instellen van PowerShell een VM maken voor de markt | Microsoft Azure"
   description="Instructies voor het instellen van Azure PowerShell en deze als een optioneel proces doorlopen als u wilt implementeren op VM-images maken en verkopen op de Marketplace Azure"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Azure PowerShell instellen voor het maken van een aanbieding voor Azure Marketplace
Zie voor gedetailleerde informatie over het instellen van PowerShell in Azure, [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Een eenvoudige aanpak wordt met de methode certificaat, gedownload en importeert een certificaat voor verificatie nodig. Gebruik de cmdlet **Get-AzurePublishSettingsFile** om de benodigde certificaten. Sla het bestand als u wordt gevraagd. Om het certificaat te importeren in een PowerShell-sessie, gebruikt u de cmdlet **Import-en AzurePublishSettingsFile** .

Voor het configureren en opslaan van de algemene instellingen voor het abonnement op Microsoft Azure voor de PowerShell-sessie, gebruikt u de cmdlets **Set AzureSubscription** en **Selecteer AzureSubscription** :

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

De eerste opdracht wordt een standaardaccount voor opslag aan het abonnement (nodig voor sommige VM provisioning bewerkingen).  De tweede kunt u de huidige versie (die wordt herkend door andere cmdlets) het abonnement.

## <a name="see-also"></a>Zie ook
- [Aan de slag: het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md)
- [Een installatiekopie maken voor virtuele machine voor de markt](marketplace-publishing-vm-image-creation.md)
