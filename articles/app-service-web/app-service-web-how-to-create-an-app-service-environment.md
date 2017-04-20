<properties 
    pageTitle="Het maken van een App-omgeving" 
    description="Beschrijving van stroom maken voor omgevingen met app-service" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Het maken van een App-omgeving #

### <a name="overview"></a>Overzicht ###

App-Service-omgevingen (ASE) zijn een optie Premium service van Azure App-Service biedt een verbeterde functionaliteit die niet beschikbaar is in de huurder met meerdere stempels.  De functie ASE implementeert in feite de App Azure Service in een virtueel netwerk van de klant.  Krijgen een beter inzicht in de mogelijkheden aangeboden door App serviceomgevingen gelezen het, [Wat is een App-omgeving] [ WhatisASE] documentatie.

### <a name="before-you-create-your-ase"></a>Voordat u uw ASE ###

Het is belangrijk te weten van de dingen die u niet wijzigen.  De aspecten die u over uw ASE niet wijzigen nadat deze is gemaakt, zijn:

- Locatie
- Abonnement
- Resourcegroep
- VNet gebruikt
- Subnet gebruikt 
- De grootte van het subnet

Wanneer een VNet verzamelen en geven een subnet, zorg ervoor dat is groot genoeg voor alle toekomstige groei.  

### <a name="creating-an-app-service-environment"></a>Maken van een App-omgeving ###

Er zijn twee manieren toegang krijgen tot het maken van de ASE UI.  Het kan worden gevonden door te zoeken op de markt Azure ***App-omgeving*** of door te gaan door middel van nieuw -> Web + Mobile -> App-omgeving.  Een ASE maken:

1. Geef de naam van de ASE.  De naam die is opgegeven voor de ASE wordt gebruikt voor de toepassingen die zijn gemaakt in de ASE.  Naam van het subdomein zou zijn als de naam van de ASE appsvcenvdemo is. *appsvcenvdemo.p.azurewebsites.net*.  Als u dus een toepassing met de naam *mytestapp* en vervolgens het adresseerbare op *mytestapp.appsvcenvdemo.p.azurewebsites.net*worden gemaakt.  Witte ruimte kunt u niet gebruiken in de naam van de ASE.  Als u hoofdletters in de naam gebruikt, de naam van het domein worden de totale kleine versie van die naam.  Als u een ILB vervolgens uw naam ASE wordt niet gebruikt in een subdomein, maar in plaats daarvan uitdrukkelijk wordt vermeld bij het maken van ASE

    ![][1]

2. Selecteer uw abonnement.  Het abonnement voor uw ASE gebruikt is ook een die met alle toepassingen in de desbetreffende ASE worden gemaakt.  U kunt uw ASE in een VNet dat in een ander abonnement plaatsen

3. Selecteren of een nieuwe bronnengroep opgeven.  De resourcegroep die wordt gebruikt voor de ASE moeten hetzelfde die wordt gebruikt voor uw VNet.  Als u een bestaande VNet worden om aan te geven die de VNet van de selectie van de resource voor de ASE bijgewerkt.

    ![][2]

4. Controleer uw selecties virtueel netwerk en de locatie.  U kunt een nieuwe VNet maken of Selecteer een bestaande VNet.  Als u een nieuwe VNet selecteren en vervolgens kunt u een naam en locatie opgeven. De nieuwe VNet hebben de 192.268.250.0/23 adres bereik en een subnet genaamd **standaard** die is gedefinieerd als 192.168.250.0/24.  U kunt ook gewoon een reeds bestaande klassieke of Resource Manager VNet selecteren.  VIP Type bepaald als de ASE rechtstreeks toegankelijk vanuit internet (extern) of als een interne Load Balancer (ILB) wordt gebruikt.  Voor meer informatie hierover lezen [met behulp van een interne taakverdeling met een App-omgeving][ILBASE].  Als u een type VIP extern kunt u hoeveel externe IP-adressen het systeem wordt gemaakt met oog voor IPSSL selecteren.  Als u interne moet u opgeven van het subdomein dat uw ASE wordt gebruikt.  ASEs kan worden geïmplementeerd in de virtuele netwerken die gebruikmaken van *een* openbaar adresbereiken, *of* RFC1918-adresruimten (dat wil zeggen particuliere adressen).  Om een virtueel netwerk met een bereik van openbare adressen gebruikt, moet u de VNet tijdig te maken.  Wanneer u een bestaande VNet moet u een nieuw subnet maken tijdens het maken van ASE.  **U kunt een vooraf gemaakte subnet niet gebruiken in de portal.  Als u uw ASE met behulp van een resource manager-sjabloon maakt, kunt u een ASE met een reeds bestaande subnet.**  Voor het maken van een ASE van het sjabloongebruik van een informatie, [het maken van een App-omgeving van sjabloon] [ ILBAseTemplate] en hier [een ILB App-omgeving van sjabloon maken][ASEfromTemplate].

### <a name="details"></a>Details ###

Een ASE is gemaakt met 2 Front-Ends en 2 werknemers.  De Front-Ends fungeren als de HTTP/HTTPS-eindpunten en verkeer worden verzonden naar de werknemers die de rollen die host zijn van uw toepassingen.   U kunt het aantal na het maken van ASE aanpassen en kunt zelfs automatisch schalen regels op deze resourcegroepen instellen.  Voor meer informatie over handmatig schalen, beheer en bewaking van een App-omgeving gaat u hier: [het configureren van een App-omgeving][ASEConfig] 

Alleen de ene ASE kan bestaan in het subnet gebruikt door de ASE.  Het subnet kan niet worden gebruikt voor iets anders dan de ASE

### <a name="after-app-service-environment-creation"></a>Na het maken van App-omgeving ###

Na het maken van ASE aanpassen:

- Hoeveelheid van de Front-Ends (minimum: 2)
- Aantal werknemers (minimum: 2)
- Het aantal IP-adressen beschikbaar voor SSL-IP
- Resource tekengrootten in de Front-Ends of werknemers te berekenen (P2-Front-End minimale grootte is)


Er zijn meer details rond handmatig schalen, beheer en controle van de milieus voor App-Service hier: [het configureren van een App-omgeving][ASEConfig] 

Voor meer informatie over autoscaling is hier een handleiding: [automatisch schalen voor een App-omgeving configureren][ASEAutoscale]

Er zijn extra afhankelijkheden die niet beschikbaar voor aanpassing van de database en opslag zijn.  Deze worden verwerkt door Azure en worden geleverd met het systeem.  De opslag systeem ondersteunt maximaal 500 GB voor de hele App-omgeving en de database wordt aangepast met Azure, zo nodig door de schaal van het systeem.


## <a name="getting-started"></a>Aan de slag
Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Zie [Inleiding App Service-omgevingen] aan de slag met App Service-omgevingen[WhatisASE]

Zie voor meer informatie over het platform Azure App Service [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
