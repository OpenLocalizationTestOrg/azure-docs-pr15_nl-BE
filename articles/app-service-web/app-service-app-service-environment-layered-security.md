<properties 
    pageTitle="Gelaagde beveiligingsarchitectuur met App Service-omgevingen" 
    description="Het implementeren van een gelaagde beveiligingsarchitectuur met App Service-omgevingen." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Implementatie van een gelaagde beveiligingsarchitectuur met App Service-omgevingen

## <a name="overview"></a>Overzicht ##
 
Omdat App serviceomgevingen een geïsoleerde runtimeomgeving in een virtueel netwerk wordt geïmplementeerd, kunnen ontwikkelaars een gelaagde beveiligingsarchitectuur bieden verschillende niveaus van toegang tot het netwerk voor elke toepassingslaag van de fysieke-maken.

Een gemeenschappelijk streven is API back-ends van algemene toegang tot het Internet te verbergen en alleen toestaan API's om te worden aangeroepen door de upstream web apps.  [Netwerk-beveiligingsgroepen (NSGs)] [ NetworkSecurityGroups] kan worden gebruikt in App serviceomgevingen met subnetten openbare toegang tot API toepassingen te beperken.

Het onderstaande diagram ziet u een voorbeeld van de architectuur met een WebAPI op basis van app geïmplementeerd op een App-omgeving.  Drie afzonderlijke web app-exemplaren, wordt geïmplementeerd op drie afzonderlijke App serviceomgevingen, telefoongesprekken back-end WebAPI dezelfde App.

![Conceptuele architectuur][ConceptualArchitecture] 

De groene plustekens geven aan dat de beveiligingsgroep netwerk op het subnet met 'apiase' kunt u inkomende oproepen vanuit de upstream web apps, als ook oproepen van zichzelf.  De groep dezelfde netwerk weigert echter expliciet toegang tot algemene inkomend verkeer vanaf het Internet. 

De rest van dit onderwerp, doorloopt de stappen voor het configureren van de netwerk-beveiligingsgroep op het subnet met 'apiase'.

## <a name="determining-the-network-behavior"></a>De werking van het netwerk bepalen ##
U moet weten welke regels voor netwerk nodig zijn, bepalen welke clients in een netwerk kunnen worden bereikt van het App-omgeving met de API-app en welke clients worden geblokkeerd.

Sinds de [netwerk-beveiligingsgroepen (NSGs)] [ NetworkSecurityGroups] worden toegepast op subnetten, en App serviceomgevingen worden geïmplementeerd in subnetten, de regels van een NSG van toepassing op **alle** toepassingen die worden uitgevoerd op een App-omgeving.  Met behulp van de monster-architectuur voor dit artikel, zodra een netwerkgroep voor beveiliging wordt toegepast op het subnet met "apiase", alle toepassingen die worden uitgevoerd op de 'apiase' App-omgeving beschermd door dezelfde set regels. 

- **Bepalen de uitgaande IP-adres van de upstream bellers:**  Wat is het IP-adres of de adressen van de upstream-bellers?  U moet deze adressen expliciet toegang worden gegeven in de NSG.  Omdat aanroepen tussen App serviceomgevingen worden beschouwd als 'Internet' oproepen, houdt dit in dat de uitgaande IP-adres toegewezen aan elk van de drie upstream App serviceomgevingen moet toegang worden gegeven in de NSG voor het subnet "apiase".   Voor meer informatie over het bepalen van de uitgaande IP-adres voor de [Netwerkarchitectuur] , uitgevoerd in een omgeving van de Service App apps Zie[ NetworkArchitecture] artikel overzicht.
- **Moet de back-end API app zelf bellen?**  Een punt soms onderbelicht en subtiele is het scenario waarin de back-endtoepassing moet zichzelf.  Als een back-end API-toepassing op een App-omgeving zichzelf moet, wordt dit ook beschouwd als een oproep voor een 'Internet'.  In de architectuur van het monster moet hiervoor toegang van de uitgaande IP-adres van de "apiase" App-omgeving ook.

## <a name="setting-up-the-network-security-group"></a>De beveiligingsgroep netwerk instellen ##
Zodra het aantal uitgaande IP-adressen zijn bekend, is de volgende stap om een beveiligingsgroep netwerk samen te stellen.  Netwerk-beveiligingsgroepen kunnen worden gemaakt voor zowel Resource Manager virtuele netwerken als klassieke virtuele netwerken gebaseerde.  In de onderstaande voorbeelden maken en configureren van een NSG op een klassieke virtueel netwerk met Powershell.

De omgevingen bevinden zich in Zuid-centraal ons, voor de monster-architectuur, zodat een lege NSG in die regio wordt gemaakt:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Eerst een expliciete toestaan regel wordt toegevoegd voor de Azure infrastructuur zoals beschreven in het artikel over [het binnenkomende verkeer] [ InboundTraffic] voor omgevingen met App-Service.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Vervolgens twee regels worden toegevoegd aan de HTTP- en HTTPS-gesprekken van de eerste upstream App-omgeving ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Spoelen en Herhaal dit voor de tweede en derde upstream App serviceomgevingen ("fe2ase" en "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Ten slotte, toegang verlenen tot de uitgaande IP-adres van de back-end-API App-omgeving zodat terug in zichzelf kan worden aangeroepen.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Geen andere beveiligingsregels voor netwerken moeten worden geconfigureerd omdat elke NSG heeft een aantal standaardregels die binnenkomende toegang vanaf het Internet te blokkeren.

De volledige lijst met regels in de netwerkgroep worden hieronder weergegeven.  U ziet hoe de laatste regel die is gemarkeerd, blokkeert binnenkomende toegang van alle bellers dan die welke expliciet toegang is verleend.

![NSG configuratie][NSGConfiguration] 

De laatste stap is de NSG toepassen op het subnet waarin de 'apiase' App-omgeving.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Met de NSG toegepast op het subnet, alleen de drie upstream App serviceomgevingen en de App-omgeving met de API back-end, mag aanroepen in het milieu "apiase".


## <a name="additional-links-and-information"></a>Als u meer koppelingen en informatie ##
Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Informatie over [netwerk-beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md). 

Wat zijn [uitgaande IP-adressen] [ NetworkArchitecture] en App-Service.

[Netwerkpoorten] [ InboundTraffic] die wordt gebruikt door de App Service-omgevingen.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
