<properties
    pageTitle="Richtlijnen voor virtuele Machines Windows | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerp en de implementatie van de richtlijnen voor de implementatie van windows virtuele machines in Azure"
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

# <a name="virtual-machines-guidelines"></a>Richtlijnen voor virtuele machines

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dit artikel is gericht op het inzicht van de vereiste stappen voor het maken en beheren van virtuele machines (VMs) in uw omgeving Azure van de planning.

## <a name="implementation-guidelines-for-vms"></a>Implementatie van richtlijnen voor VMs
Besluiten:

- Hoeveel VMs moet u voor de verschillende lagen en componenten van uw infrastructuur?
- Welke CPU en geheugenbronnen in beslag heeft elke VM nodig en wat zijn de opslagvereisten?

Taken:

- De werklast voor uw toepassing en de middelen het VMs vereist definiëren.
- Hiermee lijnt u de systeembronnen voor elke VM met de juiste VM grootte en opslag.
- Definieer de resourcegroepen voor de verschillende niveaus en onderdelen van uw infrastructuur.
- De VM-naamgevingsregels definiëren.
- Maak uw VMs met de Azure PowerShell, web portal, of met sjablonen Resource Manager.

## <a name="virtual-machines"></a>Virtuele machines

Een van de belangrijkste onderdelen in uw omgeving Azure is waarschijnlijk VMs. Dit is waar het uitvoeren van uw toepassingen, databases, verificatieservices, enz.

Het is belangrijk om te begrijpen van de [verschillende formaten van VM](virtual-machines-windows-sizes.md) correct inschatten van de omgeving van prestaties en kosten perspectief. Als u uw VMs nog niet voldoende CPU-kernen of geheugen, daaronder de prestaties van uw toepassing ongeacht hoe goed is ontworpen en ontwikkeld. De voorgestelde werkbelasting bekijken voor elke reeks VM als uitgangspunt als u welke grootte VM besluit te gebruiken voor elk onderdeel in uw infrastructuur. Na de implementatie kunt u [de grootte wijzigen van een VM](https://azure.microsoft.com/blog/resize-virtual-machines/) .

Opslag speelt een belangrijke rol in de VM-prestaties. Kunt u standaardopslag die normale draaiende schijven gebruikt, of de premie opslag voor hoge i/o-belasting en topprestaties die gebruikmaakt van SSD-schijven. Als met de VM-grootte, er worden kosten overwegingen bij het selecteren van het opslagmedium. U kunt de [opslag infrastructuur richtlijnen artikel](virtual-machines-windows-infrastructure-storage-solutions-guidelines.md) informatie over het ontwerpen van de juiste opslag voor optimale prestaties van uw VMs lezen.


## <a name="resource-groups"></a>Resourcegroepen
Onderdelen zoals VMs logisch gegroepeerd voor eenvoudig beheer en onderhoud met [Azure resourcegroepen](../azure-resource-manager/resource-group-overview.md). Met behulp van de resourcegroepen kunt u maken, beheren en controleren van de resources die in een bepaalde toepassing. U kunt ook implementeren [op rollen gebaseerde toegangscontrole](../active-directory/role-based-access-control-what-is.md) om toegang tot anderen binnen uw team alleen de bronnen die zij nodig hebben. Tijd voor de planning van de bronnengroepen en roltoewijzingen in beslag nemen. Er zijn verschillende benaderingen daadwerkelijk ontwerpen en implementeren van resourcegroepen, dus Lees de [resource groepen richtlijnen artikel](virtual-machines-windows-infrastructure-resource-groups-guidelines.md) om te begrijpen hoe het best aan het bouwen van uw VMs.


## <a name="templates"></a>Sjablonen 
U kunt sjablonen, gedefinieerd door een declaratieve JSON-bestanden, maken uw VMs samenstellen. Sjablonen meestal bouwen ook de vereiste opslag, netwerken, netwerkinterfaces, IP-adressen, enz. met het VMs zelf. Sjablonen maakt u consistente en reproduceerbare omgevingen voor ontwikkeling en testen van doeleinden productieomgevingen gemakkelijk gerepliceerd en vice versa. U kunt meer lezen over het [opstellen en gebruiken van sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment) te begrijpen hoe u ze kunt gebruiken voor het maken en implementeren van uw VMs.


## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 