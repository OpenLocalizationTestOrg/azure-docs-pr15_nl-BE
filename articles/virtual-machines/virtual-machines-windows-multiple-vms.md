<properties
    pageTitle="Meerdere virtuele machines maken | Microsoft Azure"
    description="Opties voor het maken van meerdere virtuele machines in Windows"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Meerdere Azure virtuele machines maken

Er zijn veel scenario's waarin u moet maken van een groot aantal vergelijkbare virtuele machines (VMs). Enkele voorbeelden zijn high performance computing (HPC), grootschalige gegevensanalyse, schaalbare en vaak staatloze middenlaag of back-end-servers (zoals webservers) en gedistribueerde databases.

Dit artikel worden de beschikbare opties voor het maken van meerdere VMs in Azure. Deze opties verder dan de eenvoudige gevallen waarin u een reeks van VMs handmatig maken. Niet wilt maken veel VMs, schalen de processen die u gewoonlijk gebruikt en als u meer dan een handvol VMs maken.

Een manier voor het maken van veel soortgelijke VMs is constructie voor de Azure Resource Manager van een _resource wordt_gebruikt.

## <a name="resource-loops"></a>Resource-lussen

Resource-lussen zijn een syntactische steno in Azure Resource Manager-sjablonen. Lussen resource kunnen u een reeks informatiebronnen op vergelijkbare wijze geconfigureerde maken in een lus. Resource-lussen kunt u meerdere accounts voor opslag, netwerkinterfaces of virtuele machines maken. Voor meer informatie over lussen resource verwijzen naar het [VMs maken in de beschikbaarheid wordt ingesteld met behulp van resource-lussen](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>De uitdagingen van de schaal

Hoewel resource lussen gemakkelijker te bouwen van een cloud-infrastructuur met schaal en produceren een beknoptere sjablonen, blijven bepaalde uitdagingen. Als u met een lus voor de resource 100 virtuele machines maken, moet u bijvoorbeeld correleren network interface controllers (NIC's) met bijbehorende VMs en opslag rekeningen. Omdat het aantal VMs waarschijnlijk anders zijn dan het aantal accounts voor opslag, hebt u een andere resource lus formaten, te behandelen. Dit zijn problemen opgelost, maar de complexiteit aanzienlijk kan met schaal.

Een andere uitdaging doet zich voor wanneer u een infrastructuur die schaalbaar is elastically. U kunt bijvoorbeeld een automatisch schalen infrastructuur die automatisch vergroot of verkleint u het aantal VMs in reactie op de werkbelasting. VMs voorzien niet van een geïntegreerde mechanisme om te variëren in aantal (geschaald uitbreiden en schaal). Als u de schaal door het verwijderen van VMs, is het moeilijk om een hoge beschikbaarheid garanderen door ervoor te zorgen dat VMs zijn verdeeld over de update en het probleem.

Ten slotte, wanneer u een resource lus, meerdere aanroepen voor het maken van bronnen gaat u naar het onderliggende weefsel. Wanneer meerdere aanroepen van dergelijke bronnen maakt, heeft Azure impliciete gelegenheid te verbeteren bij dit ontwerp en implementatie van betrouwbaarheid en prestaties te optimaliseren. Dit is waar _virtuele machine schaal stelt_ komen.

## <a name="virtual-machine-scale-sets"></a>Virtuele machine schaal sets

Virtuele machine schaal sets zijn een middel Azure berekenen te implementeren en beheren van een reeks identiek VMs. Geconfigureerd met alle VMs dezelfde, VM schaal sets eenvoudig zijn schalen en schalen uit. Wijzigt u het nummer van het VMs in de set. U kunt ook sets, VM schalen automatisch schalen op basis van de vereisten van de werklast.

Voor toepassingen die u wilt schalen Compute resources uit- en schaal bewerkingen impliciet in evenwicht zijn tussen probleem- en domeinen.

Een set VM schaal heeft in plaats van meerdere bronnen, zoals netwerkinterfacekaarten en VMs correleren, netwerk, opslag, virtuele machine en extensie-eigenschappen die u centraal kunt configureren.

Zie de [virtuele machine schaal stelt de productpagina](https://azure.microsoft.com/services/virtual-machine-scale-sets/)voor een inleiding op VM schaal wordt. Ga naar de [virtuele machines schaal stelt de documentatie](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)voor meer gedetailleerde informatie.
