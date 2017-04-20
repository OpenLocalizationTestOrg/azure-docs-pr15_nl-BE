<properties
   pageTitle="De identiteit van een werk of school in AAD maken | Microsoft Azure"
   description="Informatie over het maken van een werk of school identiteit in Azure Active Directory voor gebruik met uw Linux virtuele machines."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>De identiteit van een werk of op School te maken in Active Directory te gebruiken met Linux VMs Azure

Als u een persoonlijke account Azure gemaakt of een persoonlijke MSDN-abonnement hebt en de Azure-account om te profiteren van de kredieten voor MSDN Azure--gemaakt gebruikt u de identiteit van een *Microsoft-account* aan te maken. Veel handige functies van Azure-- [resource groep-sjablonen](../azure-resource-manager/resource-group-overview.md) is een voorbeeld--een werk of school account (een identiteit beheerd door Azure Active Directory) nodig om te werken. U kunt de instructies hieronder te maken van een nieuw werk of school account omdat gelukkig een van de beste dingen over je persoonlijke account Azure is dat wordt geleverd met een standaard Azure Active Directory-domein die u gebruiken kunt voor het maken van een nieuw werk of school-account die u kunt gebruiken met Azure functies die het nodig hebben.

Echter, recente wijzigingen kunnen u uw abonnement met elk type van het gebruik van Azure account beheren de `azure login` beschreven methode voor interactieve aanmelding [hier](../xplat-cli-connect.md). U kunt dit mechanisme gebruiken of u kunt de instructies die volgen. U kunt ook [maken van een werk of school identiteit in Azure Active Directory voor gebruik met Windows VMs](virtual-machines-windows-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
