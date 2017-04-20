<properties
   pageTitle="Met behulp van Windows-installatiekopieën van clients voor dev/Testscenario's | Microsoft Azure"
   description="Voordelen van Visual Studio-abonnement gebruiken voor het implementeren van Windows 7/8/10 in Azure voor dev/Testscenario's"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Met behulp van Windows-client voor dev/Testscenario's in Azure

Windows 7, Windows 8, kunt u of Windows 10 in Azure voor dev/Testscenario's mits dat een juiste Visual Studio (voorheen MSDN)-abonnement hebt. In dit artikel worden de vereisten in aanmerking komen voor Windows-client uitgevoerd in Azure en het gebruik van de Azure-afbeeldingen.


## <a name="subscription-eligibility"></a>Abonnement in aanmerking komen
Actieve Visual Studio abonnees (mensen die een abonnementslicentie voor Visual Studio hebben aangeschaft) kunnen Windows-client gebruiken voor ontwikkel- en testdoeleinden. Windows-client kan worden gebruikt op uw eigen hardware en Azure virtuele machines die worden uitgevoerd in elk type abonnement Azure. Windows-client kan niet worden geïmplementeerd op op Azure worden gebruikt voor normale productie of gebruikt door mensen die geen actieve Visual Studio-abonnees.

Voor uw gemak, hebben we bepaalde Windows 10 afbeeldingen beschikbaar gesteld uit de galerie Azure binnen de [in aanmerking komende dev/test biedt](#eligible-offers). Binnen elk type aanbieding Visual Studio-abonnees kunnen ook [adequaat voorbereiden en maken](virtual-machines-windows-prepare-for-upload-vhd-image.md) een 64-bits Windows 7, Windows 8-of image van Windows 10 en vervolgens [uploaden naar Azure](virtual-machines-windows-upload-image.md). Het gebruik blijft beperkt tot dev/test door actieve Visual Studio-abonnees.


## <a name="eligible-offers"></a>In aanmerking komende aanbiedingen
De volgende tabel ziet de offerte-ID's die in aanmerking komen voor de implementatie van Windows 10 door de galerie Azure. De Windows 10-afbeeldingen zijn alleen zichtbaar voor de volgende aanbiedingen. Visual Studio-abonnees die u moeten de Windows-client uitvoeren in een ander voorstel, moet u [goed voorbereiden](virtual-machines-windows-prepare-for-upload-vhd-image.md) en maken een 64-bits Windows 7, Windows 8 of Windows 10 afbeelding en [vervolgens uploaden naar Azure](virtual-machines-windows-upload-image.md).

| Naam van het voorstel | Aanbiedingsnummer | Installatiekopieën van clients beschikbaar |
|:-----------|:------------:|:-----------------------:|
| [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10 |
| [Visual Studio Enterprise (MPN)-abonnees](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10 |
| [Visual Studio Professional-abonnees](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10 |
| [Visual Studio Test Professional-abonnees](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10 |
| [Premie voor Visual Studio met MSDN (voordeel)](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10 |
| [Visual Studio Enterprise abonnees](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10 |
| [Abonnees van Visual Studio Enterprise (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Enterprise Dev/Test](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10 |


## <a name="check-your-azure-subscription"></a>Controleer uw abonnement Azure
Als u uw aanbieding ID niet weet, kunt u deze via de portal Azure of de Account portal.

De aanbieding abonnements-ID wordt vermeld op het 'Abonnementen' blade van de Azure portal:

![Details over voorstel-ID van het portal voor Azure](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

U kunt ook de aanbod-ID op het [tabblad 'Abonnementen'](http://account.windowsazure.com/Subscriptions) van de portal Azure Account bekijken:

![Details over voorstel-ID uit de Azure Account portal](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Volgende stappen
U kunt nu uw VMs met [PowerShell](virtual-machines-windows-ps-create.md) [Resource Manager sjablonen](virtual-machines-windows-ps-template.md)of [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)implementeren.
