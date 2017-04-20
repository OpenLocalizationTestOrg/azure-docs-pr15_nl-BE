<properties
    pageTitle="Wat zijn VM schaal ingesteld? | Microsoft Azure"
    description="Informatie over VM schaal sets."
    keywords="Linux virtuele machine virtuele machine schaal instellen" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Wat zijn virtuele machine schaal ingesteld?

Virtuele Machine schaal Sets kunt u meerdere VMs beheren als een set. Op een hoog niveau hebben schaal stelt de volgende voor- en nadelen:

-Professionals:

1. Hoge beschikbaarheid. Elke set schaal brengt de VMs in een beschikbaarheid instellen met 5 fout met betrekking tot domeinen (FDs) en 5 Update domeinen (UDs) om beschikbaarheid te garanderen (Zie voor meer informatie over FDs en UDs, [VM beschikbaarheid](./virtual-machines-linux-manage-availability.md)). 
2. Eenvoudige integratie met Azure Load Balancer en App-Gateway.
3. Eenvoudige integratie met Azure automatisch schalen.
4. Vereenvoudigde implementatie, beheer en opschonen van VMs.
5. Ondersteuning voor algemene Windows en Linux flavors, evenals aangepaste images.

Nadelen:

1. Geen toevoegen gegevensschijven aan VM exemplaren in een schaal. In plaats daarvan moet gebruiken Blob Storage, Azure bestanden, Azure, tabellen of andere opslagoplossing.

## <a name="quick-create-using-azure-cli"></a>Snel tabellen maken met behulp van de CLI Azure

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Volgende stappen

Bekijk de [belangrijkste portaalpagina voor schaal sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/)voor algemene informatie.

Voor meer documentatie, bekijk de [van de belangrijkste documentatiepagina voor schaal wordt ingesteld](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Schaal sets met Resource Manager-sjablonen zoeken bijvoorbeeld 'vmss' in de [sjablonen van Azure Quickstart github repo](https://github.com/Azure/azure-quickstart-templates).

