<properties
   pageTitle="Technische vereisten voor het maken van een installatiekopie van de virtuele machine voor Azure Marketplace | Microsoft Azure"
   description="Inzicht in de vereisten voor het maken en implementeren van een image van de virtuele machine op de markt Azure voor anderen om te kopen."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Technische vereisten voor het maken van een installatiekopie van de virtuele machine voor Azure Marketplace
Het proces grondig voordat u begint te lezen en begrijpen waar en waarom elke stap wordt uitgevoerd. Zo veel mogelijk, u moet informatie over uw bedrijf en andere gegevens voorbereiden, benodigde hulpprogramma's downloaden en/of technische componenten maken voordat u begint met het proces voor het maken van het voorstel. Deze items moeten worden uit dit artikel te bekijken.  

## <a name="download-needed-tools--applications"></a>Benodigde tools en toepassingen downloaden
U moet de volgende items klaar voordat u begint met het proces:

- Afhankelijk van het besturingssysteem die u zijn gericht, het [Azure PowerShell-cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) of [Linux opdrachtregelinterface](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) installeren vanaf de pagina [Downloads Azure](https://azure.microsoft.com/downloads/) .
- Azure Opslagverkenner van CodePlex installeren.
- Downloaden en installeren van de certificering Test Tool voor Azure gecertificeerd:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). U moet een Windows-computer het hulpprogramma certificering uitvoeren. Als er niet een Windows-computer beschikbaar is, kunt u het hulpprogramma gebruikt een op Windows gebaseerde VM in Azure uitvoeren.

## <a name="platforms-supported"></a>Ondersteunde platforms
U kunt ontwikkelen VMs Azure is gebaseerd op Windows of Linux. Sommige elementen van het publicatieproces--zoals het maken van een Azure-compatibele virtuele harde schijf (VHD)--gebruiken verschillende hulpprogramma's en werk, afhankelijk van welk besturingssysteem u gebruikt:  

- Als u Linux gebruikt, Raadpleeg de sectie 'Een Azure-compatibele VHD (Linux-gebaseerde) maken' van de [virtuele machine afbeelding publishing guide](marketplace-publishing-vm-image-creation.md).
- Als u Windows gebruikt, Raadpleeg de sectie 'Maken een Azure-compatibele VHD (Windows-programma)' van de [virtuele machine afbeelding publishing guide](marketplace-publishing-vm-image-creation.md).

> [AZURE.NOTE] U moet toegang tot een Windows-machine op:
- Voer het validatieprogramma certificering.
- De VHD gedeelde toegang handtekening URL voor de indiening van de certificatie VHD maken.

## <a name="develop-your-vhd"></a>De VHD ontwikkelen
U kunt ontwikkelen Azure VHD's in de cloud of op locatie:

- Cloud-gebaseerde ontwikkeling betekent dat alle stappen van de ontwikkeling op afstand worden uitgevoerd op een VHD op Azure.
- Ontwikkeling van ruimten moet een VHD downloaden en ontwikkelen met behulp van lokalen, infrastructuur. Hoewel dit mogelijk is, niet aangeraden deze. Houd er rekening mee dat ontwikkelen voor Windows of SQL op gebouwen, moet u de licentiecodes betrokken op panden hebben. U kan opnemen of SQL Server installeren na het maken van een VM. Ook moet u uw voorstel baseren op een goedgekeurde SQL afbeelding uit de Azure portal. Als u op ruimten te ontwikkelen, moet u anders dan als u in de cloud ontwikkelt enkele stappen uitvoeren. U kunt relevante informatie vinden in [een VM op locatie afbeelding maken](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Volgende stappen
Nu dat u herzien van de vereisten en de vereiste taken uitgevoerd, kunt u gaan met het maken van uw virtuele machine afbeelding voorstel zoals wordt beschreven in de [handleiding voor virtuele machine afbeelding publishing](marketplace-publishing-vm-image-creation.md).

## <a name="see-also"></a>Zie ook
- [Aan de slag: het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md)
- [Een virtuele machine met Windows in Azure preview portal maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
