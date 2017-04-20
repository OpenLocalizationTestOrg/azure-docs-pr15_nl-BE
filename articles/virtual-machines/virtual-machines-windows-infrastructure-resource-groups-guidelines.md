<properties
    pageTitle="Resource Groups richtlijnen | Microsoft Azure"
    description="Informatie over de belangrijkste ontwerp en de implementatie richtlijnen voor de implementatie van resourcegroepen in Azure infrastructuurservices."
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

# <a name="azure-resource-group-guidelines"></a>Azure resource groep richtlijnen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dit artikel is gericht op het logisch bouwen van uw omgeving en alle onderdelen in groepen resources groeperen.


## <a name="implementation-guidelines-for-resource-groups"></a>Implementatie van richtlijnen voor resourcegroepen

Besluiten:

- Gaat u bouwen van resourcegroepen door de kernonderdelen van infrastructuur of implementatie van complete toepassingen?
- Moet u de toegang beperken tot de brongroepen met op rollen gebaseerde toegangscontrole?

Taken:

- Definiëren wat core onderdelen van de infrastructuur en de toegewezen resourcegroepen u nodig hebt.
- Lees hoe u sjablonen voor een consistente en reproduceerbare implementaties Resource Manager implementeert.
- Bepalen wat u nodig hebt voor het beheren van toegang aan resourcegroepen rollen toegang.
- Maak de bronnengroepen gebruiken de naamgevingsconventie. U kunt de Azure PowerShell of de portal.


## <a name="resource-groups"></a>Resourcegroepen

In Azure groepeert u logisch gerelateerde bronnen, zoals accounts voor opslag en virtuele netwerken, virtuele machines (VMs) te implementeren, beheren en onderhouden ze als een eenheid. Deze benadering maakt het gemakkelijker te implementeren toepassingen terwijl de verwante bronnen samen vanuit een management perspectief of anderen toegang verlenen tot die groep van bronnen. Lees het [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)voor een vollediger begrip van bronnengroepen.

Een belangrijke functie voor resourcegroepen is de mogelijkheid om te bouwen van uw omgeving met behulp van sjablonen. Een sjabloon is gewoon een JSON-bestand die verklaart de opslag, netwerken, en resources te berekenen. Ook kunt u de bijbehorende aangepaste scripts of configuraties toe te passen. Met deze sjablonen kunt u consistente en reproduceerbare implementaties voor uw toepassingen. Deze benadering kunt u gemakkelijk bouwen van een omgeving in ontwikkeling en gebruik vervolgens dezelfde sjabloon voor het maken van een productie-implementatie, of vice versa. Voor een beter inzicht met behulp van sjablonen, Lees [de beschrijving van de sjabloon](../resource-manager-template-walkthrough.md) die u bij elke stap van het gebouw van een sjabloon helpt.

Er zijn twee verschillende benaderingen die u nemen kunt bij het ontwerpen van uw omgeving met resourcegroepen:

- Resourcegroepen voor elke distributie die de opslag rekeningen, virtuele netwerken en subnetten, VMs combineert, laden balancers, enz.
- Centrale resourcegroepen die uw core virtuele netwerken en subnetten of opslag accounts bevatten. De toepassingen zijn dan in hun eigen resourcegroepen die alleen VMs, Netwerktaakverdeling, netwerkinterfaces, enz bevatten.

Als u schalen, gecentraliseerde resourcegroepen voor uw virtuele netwerken en subnetten maken gemakkelijker te bouwen netwerkverbindingen tussen lokalen voor hybride connectiviteitsopties. De andere mogelijkheid is voor elke toepassing hebben hun eigen virtuele netwerk waarvoor u de configuratie- en onderhoudstaken.  [Op rollen gebaseerde toegangscontrole](../active-directory/role-based-access-control-what-is.md) bieden een granulaire manier toegang tot de resourcegroepen. Voor productietoepassingen kunt u bepalen welke gebruikers toegang bronnen tot mogelijk of voor de belangrijkste infrastructuur voor resources kunt u alleen infrastructuur technici werken beperken. Eigenaren van uw toepassing alleen hebben toegang tot de onderdelen binnen hun groep en niet de kern van het Azure-infrastructuur van uw omgeving. Bij het ontwerpen van uw omgeving, kunt u de gebruikers die toegang nodig tot de bronnen en de bronnengroepen dienovereenkomstig ontwerp. 


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 