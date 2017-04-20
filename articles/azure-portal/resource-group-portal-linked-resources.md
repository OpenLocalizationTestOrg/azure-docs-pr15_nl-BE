<properties 
    pageTitle="Verwante en gekoppelde bronnen in de galerie naast elkaar" 
    description="Meer informatie over verwante en gekoppelde bronnen die worden weergegeven in de galerie naast elkaar plaatsen van de portal Azure voorbeeld." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Verwante en gekoppelde bronnen in de galerie naast elkaar

De galerie naast elkaar kunt u tegels voor een bepaalde resource zoeken en sleept u deze naar uw huidige blade. De galerie naast elkaar gebruikt, kunt u beheer weergaven die bronnen omvatten. Voor een opgegeven resource zijn gerelateerde bronnen alle bronnen die dezelfde resourcegroep als middel voor het delen en alle bronnen die naar of uit de bron te koppelen.

## <a name="linked-resources-in-azure-resource-manager"></a>Gekoppelde bronnen in Azure Resource Manager

Koppelen is een functie van de Azure Resource Manager.  U kunt de relaties tussen bronnen declareren, zelfs als ze zich niet in dezelfde bronnengroep. Koppelen, heeft geen invloed op de uitvoering van uw resources, zonder invloed op de facturering en geen invloed op de toegang op basis van rollen.  Het is gewoon een mechanisme dat u gebruiken kunt voor het weergeven van relaties zodat middelen zoals de galerie tegel een uitgebreide management bieden kan ervaring.  De hulpprogramma's kunnen koppelingen met behulp van de API-koppelingen controleren en bieden aangepaste relatiebeheer ervaringen ook. 

## <a name="how-do-i-link-my-resources"></a>Hoe koppel ik mijn resources

Wanneer u resources via de portal of door het implementeren van een sjabloon via Azure PowerShell of Azure CLI maakt, worden voor sommige afhankelijke bronnen automatisch koppelingen gemaakt. Resources kunt u ook via programmacode koppelen met behulp van de [Gekoppelde bronnen REST API](https://msdn.microsoft.com/library/azure/mt238499.aspx) of het declareren van de relaties in de sjabloon. Zie voor een volledige bespreking van het werken met gekoppelde bronnen [resources koppelen in Azure Resource Manager](../resource-group-link-resources.md).

## <a name="next-steps"></a>Volgende stappen

- Als u een inleiding op het schrijven van sjablonen Azure Resource Manager, Zie [sjablonen ontwerpen](../resource-group-authoring-templates.md).
- Zie voor meer details over het maken van koppelingen tussen resources nu meteen, [resources koppelen in Azure Resource Manager](../resource-group-link-resources.md).
- Meer over het werken met resourcegroepen via de portal voorbeeld Zie [met behulp van de Portal Azure voorbeeld voor het beheren van uw resources Azure](resource-group-portal.md).
