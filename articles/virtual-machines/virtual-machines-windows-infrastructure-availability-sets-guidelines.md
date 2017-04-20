<properties
    pageTitle="Beschikbaarheid stelt richtlijnen | Microsoft Azure"
    description="Informatie over de belangrijkste ontwerp en de implementatie richtlijnen voor de implementatie van beschikbaarheid Sets in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="availability-sets-guidelines"></a>Beschikbaarheid stelt richtlijnen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dit artikel is gericht op het inzicht van de vereiste stappen van de planning voor beschikbaarheid sets, zodat uw toepassingen blijft toegankelijk tijdens geplande of ongeplande gebeurtenissen.

## <a name="implementation-guidelines-for-availability-sets"></a>Implementatie van richtlijnen voor de beschikbaarheid van sets

Besluiten:

- Hoeveel sets van beschikbaarheid moet u voor de verschillende rollen en lagen in de infrastructuur van toepassing?

Taken:

- Definieer het aantal VMs in elke toepassingslaag die u nodig hebt.
- Bepaal of u moet het aantal aanpassen of bijwerken van domeinen voor uw toepassing moet worden gebruikt.
- Definiëren van de vereiste beschikbaarheid sets met uw naamgevingsconventie en wat VMs bevinden zich in deze. Een VM kan alleen worden opgenomen in de beschikbaarheid van een set. 

## <a name="availability-sets"></a>Beschikbaarheid van sets

In Azure, kan virtuele machines (VMs) worden geplaatst een logische groepering een set beschikbaarheid genoemd. Bij het maken van VMs binnen een set beschikbaarheid verdeelt de Azure platform de plaatsing van de VMs in de onderliggende infrastructuur. Moet er een gebeurtenis gepland onderhoud aan het Azure platform of een onderliggende hardware / infrastructuur veroorzaakt, het gebruik van beschikbaarheid sets zorgt ervoor dat ten minste één VM actief blijft.

Als de beste toepassingen moeten bevindt zich niet op een enkele VM. Een set beschikbaarheid met een enkele VM krijgen niet een bescherming van geplande of ongeplande gebeurtenissen binnen de Azure platform. De [SLA Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines) vereist twee of meer VMs binnen een beschikbaarheid ingesteld dat de verdeling van VMs in de onderliggende infrastructuur.

De onderliggende infrastructuur in Azure is onderverdeeld domeinen en domeinen veroorzaakt bij te werken. Deze domeinen zijn gedefinieerd door welke hosts gemeenschappelijke update cyclus of delen dezelfde fysieke infrastructuur zoals kracht en netwerken delen. Azure distribueert automatisch uw VMs binnen een beschikbaarheid ingesteld tussen domeinen te handhaven, beschikbaarheid en fouttolerantie. Afhankelijk van de grootte van uw toepassing en het aantal VMs binnen een set beschikbaarheid, kunt u het aantal domeinen die u wilt gebruiken. Meer informatie over het [beheren van beschikbaarheid en het gebruik van domeinen update en fouttolerantie](virtual-machines-windows-manage-availability.md).

Bij het ontwerpen van de infrastructuur van uw toepassing, moet u de lagen die u gebruikt ook plannen. Groep VMs die dienen hetzelfde doel in beschikbaarheid wordt ingesteld, zoals een beschikbaarheid instellen voor uw front-end VMs waarop IIS wordt uitgevoerd. Maak een afzonderlijke beschikbaarheid instellen voor uw back-end VMs waarop SQL Server wordt uitgevoerd. Het doel is om ervoor te zorgen dat elk onderdeel van uw toepassing wordt beschermd door een set beschikbaarheid en ten minste eenmaal exemplaar wordt altijd uitgevoerd.

Netwerktaakverdeling kunnen worden gebruikt voor elke toepassingslaag werkt samen met een set beschikbaarheid en verkeer kan altijd worden doorgestuurd naar een actief exemplaar. Zonder een taakverdeling de VMs kunnen blijven uitvoeren tijdens het onderhoud van geplande en ongeplande gebeurtenissen, maar uw eindgebruikers niet mogelijk deze op te lossen als de primaire VM niet beschikbaar is.


## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 