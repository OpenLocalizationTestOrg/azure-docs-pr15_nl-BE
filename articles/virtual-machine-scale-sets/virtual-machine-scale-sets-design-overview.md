<properties
    pageTitle="Ontwerpen van virtuele Machine schaal ingesteld voor schaal | Microsoft Azure"
    description="Klik hier voor informatie over het ontwerpen van uw virtuele Machine schaal ingesteld voor schaal"
    keywords="Linux virtuele machine virtuele machine schaal instellen" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Ontwerpen van VM schaal ingesteld voor schaal

In dit onderwerp wordt besproken overwegingen voor virtuele Machine schaal Sets. Verwijzen naar de [Virtuele Machine schaal instellen-overzicht](virtual-machine-scale-sets-overview.md)voor meer informatie over wat virtuele Machine schaal zijn.


## <a name="storage"></a>Opslag

Een set schaal gebruikt opslag rekeningen voor het opslaan van de OS-schijven van het VMs in de set. U wordt aangeraden een verhouding van 20 VMs per account opslag of minder. Ook wordt aangeraden dat u verspreid over het alfabet het begin van de opslag-accountnamen. Daarmee helpt laden verspreid over verschillende interne systemen. Bijvoorbeeld in de volgende sjabloon gebruiken we de uniqueString functie van de sjabloon Resource Manager voor het genereren van voorvoegsel hashes die zijn voorafgegaan aan accountnamen opslag: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

Vanaf de versie van de API "30-03-2016", VM schaal wordt standaard ingesteld op "overprovisioning" VMs. Met overprovisioning ingeschakeld, de schaal werkelijk draaien om meer VMs, dan wordt u gevraagd voor instellen en vervolgens verwijdert u de extra VMs die van de laatste gesponnen. Overprovisioning verbetert provisioning succes te behalen. U niet worden gefactureerd voor deze extra VMs en ze worden niet meegerekend voor de quotumlimiet.

Terwijl overprovisioning provisioning succespercentages verbeteren, kan dit verwarrend zijn gedrag voor een toepassing die niet is ontworpen voor het verwerken van VMs verdwijnen onaangekondigde veroorzaken. Zorgen om overprovisioning uit te schakelen, hebt u de volgende tekenreeks in uw sjabloon: "overprovision": "false". Meer informatie vindt u in de [VM schaal instellen REST API-documentatie](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Als u overprovisioning uitschakelt, krijgt u weg met een grotere ratio van VMs per account opslag, maar gaan boven 40 wordt niet aanbevolen.


## <a name="limits"></a>Limieten
Een set schaal is gebaseerd op een aangepaste afbeelding (een door u gemaakt) moet alle OS schijf VHD in één opslag account maken. Als gevolg hiervan is het maximum aantal VMs in een schaal gebouwd op een aangepaste afbeelding aanbevolen 20. Als u overprovisioning uitschakelt, kunt u maximaal 40 gaan.

Een set schaal is gebaseerd op een besturingssysteemkopie is momenteel beperkt tot 100 VMs (het is raadzaam 5 opslag rekeningen voor deze schaal).

Voor VMs meer dan deze limieten is toegestaan, moet u meerdere sets met schaal implementeren, zoals in [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).