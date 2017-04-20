<properties
    pageTitle="Eindpunt-typen verkeer Manager | Microsoft Azure"
    description="In dit artikel worden verschillende soorten eindpunten die kunnen worden gebruikt met Azure verkeer Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Beheer van netwerkverkeer eindpunten

Microsoft Azure verkeer Manager kunt u bepalen hoe netwerkverkeer wordt verdeeld met toepassing installaties uitgevoerd in verschillende datacentra. U elke implementatie van toepassingen als een 'eindpunt' Manager in het verkeer. Bij het beheer van verkeer een DNS-aanvraag ontvangt, worden gekozen een eindpunt beschikbaar om in de DNS-antwoord te retourneren. Beheer van verkeer wordt de keuze gebaseerd op de huidige Eindpuntstatus van het en de methode voor het routeren van verkeer. Voor meer informatie, Zie [het beheer van verkeer](traffic-manager-how-traffic-manager-works.md).

Er zijn drie typen worden ondersteund door verkeer Manager eindpunt:

- **Azure eindpunten** worden gebruikt voor services die worden gehost in Azure.
- **Externe eindpunten** worden gebruikt voor services die worden gehost buiten Azure, in ruimten of bij een andere hostingprovider.
- **Geneste eindpunten** worden gebruikt voor het verkeer profielen maken meer flexibele verkeer routing maatregelen ter ondersteuning van de behoeften van grotere, complexe implementaties te combineren.

Er is geen beperking op hoe de eindpunten van de verschillende typen worden gecombineerd in één profiel verkeer Manager. Elk profiel bevat een mix van eindpunt typen.

De volgende secties beschrijven elk type eindpunt in meer diepte.

## <a name="azure-endpoints"></a>Azure eindpunten

Azure eindpunten worden gebruikt voor producten op basis van Azure services Manager in het verkeer. De volgende resourcetypen Azure worden ondersteund:

- 'Klassieke' VMs van IaaS en PaaS cloud services.
- Web Apps
- PublicIPAddress middelen (die kan worden verbonden met VMs rechtstreeks of via een taakverdeling Azure). De publicIpAddress moet een DNS-naam toegewezen om te worden gebruikt in een serviceprofiel verkeer hebben.

PublicIPAddress bronnen zijn bronnen Azure Resource Manager. Ze bestaan niet in het klassieke implementatiemodel. Ze zijn dus alleen ondersteund in verkeer van Manager Azure Resource Manager ervaringen. Het andere eindpunt-typen worden ondersteund via zowel Resource Manager en de klassieke implementatiemodel.

Als u de eindpunten Azure, verkeer worden gedetecteerd wanneer een VM 'Klassieke' IaaS, cloud-service of een Web App wordt gestopt en gestart. Deze status wordt weerspiegeld in de Eindpuntstatus. Zie [verkeer Manager eindpunt controleren](traffic-manager-monitoring.md#endpoint-and-profile-status) voor meer informatie. Wanneer de onderliggende service is gestopt, voert verkeer Manager geen eindpunt veterinairrechtelijke controles of directe verkeer naar het eindpunt. Geen verkeer Manager facturering gebeurt voor de instantie gestopt. Wanneer de service opnieuw wordt gestart, komt facturatie cv's en het eindpunt in aanmerking voor verkeer. Deze detectie niet van toepassing op de eindpunten van de PublicIpAddress.

## <a name="external-endpoints"></a>Externe eindpunten

Externe eindpunten worden gebruikt voor services buiten Azure. Bijvoorbeeld een service die zijn ondergebracht in ruimten of met een andere provider. Externe eindpunten kunnen afzonderlijk worden gebruikt of in combinatie met Azure eindpunten in hetzelfde verkeer Manager profiel. Azure eindpunten met externe eindpunten te combineren, kunt verschillende scenario's:

- Gebruiken in een actieve of actieve-passieve failover-model, Azure voor vergrote redundantie voor een bestaande op-premises toepassing.
- Uitbreiden om de latentie van toepassing voor gebruikers over de hele wereld, een bestaande toepassing op ruimten met extra geografische locaties in Azure. Zie voor meer informatie [verkeer Manager 'Performance' verkeersroutering](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Azure gebruiken om extra capaciteit voor een bestaande op-premises-toepassing voortdurend of als 'burst cloud-oplossing om te voldoen aan een Prikker in de vraag.

In bepaalde gevallen is het nuttig om externe eindpunten gebruiken om te verwijzen naar Azure services (Zie de [Veelgestelde vragen](#faq)voor voorbeelden). In dit geval worden gezondheidscontroles gefactureerd tegen het tarief van Azure eindpunten, niet de snelheid van de externe eindpunten. Echter, in tegenstelling tot Azure-eindpunten als u stopt of verwijdert de onderliggende service Serverstatus controleren facturering totdat u het eindpunt in verkeer Manager verwijderen of uitschakelen.

## <a name="nested-endpoints"></a>Geneste eindpunten

Geneste eindpunten combineren meerdere verkeer profielen maken flexibele regelingen verkeer Routering en ondersteuning van de behoeften van grotere, complexe implementaties. Met geneste eindpunten, een profiel 'child' als een eindpunt aan een 'bovenliggende' profiel toegevoegd. De onderliggende en bovenliggende profielen kunnen andere eindpunten van elk type, met inbegrip van andere geneste profielen bevatten. Zie [geneste verkeer profielen](traffic-manager-nested-profiles.md)voor meer informatie.

## <a name="web-apps-as-endpoints"></a>Web Apps als eindpunten

Sommige extra overwegingen zijn van toepassing bij het configureren van Web Apps als eindpunten Manager in het verkeer:

1. Alleen Web Apps bij de 'Standaard' SKU of hoger komen in aanmerking voor gebruik met verkeer Manager. Toevoegen van een Web App van een lagere SKU niet. Downgraden van de SKU van een bestaand Web App resulteert in verkeer-beheer niet meer verkeer naar dat Web App verzenden.

2. Wanneer een eindpunt een HTTP-aanvraag ontvangt, worden de kop 'host' in het verzoek om te bepalen welke Web App moet de aanvraag service. De host-header bevat de DNS-naam gebruikt voor het initiëren van de aanvraag, bijvoorbeeld contosoapp.azurewebsites.net. Gebruik een andere DNS-naam met uw Web App, moet de DNS-naam worden geregistreerd als een aangepaste domeinnaam voor de App. Als u het eindpunt van een Web App toevoegt als een eindpunt in Azure, is de DNS-naam van de Manager van verkeer profiel automatisch geregistreerd voor de App. Deze registratie wordt automatisch verwijderd als het eindpunt wordt verwijderd.

3. Elk serviceprofiel verkeer kan hooguit één Web App eindpunt hebben uit elke regio Azure. Om voor deze beperking, kunt u een Web App configureren als een externe endpoint. Zie de [Veelgestelde vragen](#faq)voor meer informatie.

## <a name="enabling-and-disabling-endpoints"></a>In- en uitschakelen van de eindpunten

Een eindpunt in verkeer Manager uitschakelen kan handig zijn om verkeer tijdelijk verwijderen uit een eindpunt dat zich in de onderhoudsmodus of opnieuw worden geïmplementeerd. Wanneer het eindpunt opnieuw wordt uitgevoerd, kan het zijn weer ingeschakeld.

Eindpunten kunnen worden ingeschakeld en uitgeschakeld via de portal verkeer Manager PowerShell, CLI of REST API, die allemaal in het model Klassiek deployment en Resource Manager worden ondersteund.

>[AZURE.NOTE] Het uitschakelen van een Azure eindpunt heeft niets te maken met de status van de implementatie in Azure. Een Azure service (zoals een VM of Web App blijft actief en kan ontvangen van verkeer, zelfs wanneer in beheer van netwerkverkeer uitgeschakeld. Verkeer kan worden opgelost en rechtstreeks naar het service-exemplaar niet via de DNS-naam van de Manager van verkeer profiel. Zie [de werking van verkeer Manager](traffic-manager-how-traffic-manager-works.md)voor meer informatie.

De huidige beschikbaarheid van elk eindpunt verkeer ontvangt, is afhankelijk van de volgende factoren:

- De profiel-status (ingeschakeld/uitgeschakeld)
- De Eindpuntstatus (ingeschakeld/uitgeschakeld)
- De resultaten van de gezondheid worden gecontroleerd dat eindpunt

Zie [verkeer Manager eindpunt controleren](traffic-manager-monitoring.md#endpoint-and-profile-status)voor meer informatie.

>[AZURE.NOTE] Omdat verkeer Manager op het niveau van DNS werkt, is het geen invloed op bestaande verbindingen naar elk eindpunt. Wanneer u een eindpunt niet beschikbaar is, stuurt verkeer Manager nieuwe verbindingen met een andere beschikbare eindpunt. De host achter het eindpunt uitgeschakelde of beschadigde blijft echter mogelijk verkeer via bestaande verbindingen ontvangen totdat deze sessies worden beëindigd. Toepassingen te beperken van de sessieduur voor verkeer uit bestaande verbindingen.

Als alle eindpunten in een profiel zijn uitgeschakeld of als het profiel zelf is uitgeschakeld, Manager verkeer vervolgens een reactie 'NXDOMAIN' naar een nieuwe DNS-query verzendt.

## <a name="faq"></a>FAQ

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kan ik verkeer Manager gebruiken met eindpunten van meerdere abonnementen?

Eindpunten van meerdere abonnementen is niet mogelijk met Azure Web Apps. Azure Web Apps moet elke aangepaste domeinnaam met Web Apps wordt alleen gebruikt binnen een enkel abonnement. Het is niet Web Apps uit meerdere abonnementen met dezelfde domeinnaam gebruiken.

Voor andere typen eindpunt is het beheer van verkeer kunt met eindpunten die uit meer dan één abonnement. Het beheer van verkeer configureren, is afhankelijk van of u het model Klassiek deployment of Resource Manager ervaring gebruikt.

- Resource Manager eindpunten van een abonnement kunnen toevoegen aan verkeer Manager, zolang de persoon die het verkeer Manager profiel configureren lezen toegang tot het eindpunt heeft. Deze machtigingen kunnen worden toegekend met behulp van [toegangsbeheer Azure Resource Manager op rollen gebaseerde (RBAC)](../active-directory/role-based-access-control-configure.md).
- In de interface van het model Klassiek deployment vereist Manager verkeer Cloud-Services of Web Apps geconfigureerd als Azure eindpunten bevinden zich in het abonnement op hetzelfde als het verkeer Manager profiel. Cloud Service-endpoints in andere abonnementen kunnen naar verkeer Manager worden toegevoegd als 'externe' eindpunten. Deze externe eindpunten worden als Azure eindpunten in plaats van het externe tarief gefactureerd.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kan ik met Cloud Service 'Tijdelijke' sleuven verkeer Manager gebruiken?

Ja. Cloud Service sleuven fasering kan Manager in het verkeer worden geconfigureerd als externe eindpunten. Gezondheidscontroles zijn nog steeds worden gebracht tegen het tarief van de eindpunten Azure. Omdat het type van de externe endpoint gebruikt wordt, zijn wijzigingen in de onderliggende service niet opgenomen automatisch. Niet met externe eindpunten detecteren beheer van verkeer wanneer de Cloud-Service is gestopt of verwijderd. Daarom blijft de Manager verkeer facturering voor gezondheidscontroles totdat het eindpunt is uitgeschakeld of verwijderd.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Verkeer Manager biedt ondersteuning voor IPv6-eindpunten?

Beheer van netwerkverkeer biedt momenteel geen IPv6-addressible naamservers. Beheer van netwerkverkeer kan nog steeds worden gebruikt door IPv6-clients verbinding maken met een IPv6-eindpunten. Een client maakt DNS-aanvragen niet rechtstreeks naar verkeer Manager. In plaats daarvan gebruikt de client een recursieve DNS-service. Alleen IPv6-client verzendt aanvragen naar de recursieve DNS-service via IPv6. De recursieve service dan kunt u contact opnemen met de naamservers van het beheer van verkeer met IPv4.

Beheer van verkeer reageert met de DNS-naam van het eindpunt. Een eindpunt van de IPv6-ondersteuning, moet een wijzen het IPv6-adres de naam van het DNS DNS AAAA-record bestaan. Gezondheidscontroles verkeer Manager alleen ondersteuning voor IPv4-adressen. De service moet een IPv4-eindpunt op dezelfde DNS-naam.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kan ik met meer dan één Web App in dezelfde regio Manager verkeer gebruiken?

Beheer van verkeer wordt meestal gebruikt om verkeer naar toepassingen die worden gebruikt in verschillende regio's. Het kan echter ook worden gebruikt wanneer een toepassing meer dan één implementatie heeft in dezelfde regio. De eindpunten van het beheer van verkeer Azure staan niet meer dan één eindpunt van het Web App van Azure hetzelfde gebied moet worden toegevoegd aan hetzelfde verkeer Manager profiel.

De volgende stappen bieden een oplossing voor deze beperking:

1. Controleer of de eindpunten in een ander web app 'schaaleenheden zijn'. Een domeinnaam moet worden toegewezen aan één site in een bepaalde schaaleenheid. Daarom kan niet twee Web Apps in dezelfde schaaleenheid verkeer Manager profiel delen.
2. Uw aangepaste domeinnaam als een aangepaste hostnaam aan elke Web App toevoegen. Elke Web App moet zich in een andere schaaleenheid. Alle Web Apps moet deel uitmaken van het hetzelfde abonnement.
3. Één (en slechts één) eindpunt van Web App toevoegen aan uw profiel Manager verkeer als een eindpunt Azure.
4. Elk extra Web App eindpunt toevoegen aan uw serviceprofiel verkeer als een eindpunt van de externe. Externe eindpunten kunnen alleen worden toegevoegd met behulp van het implementatiemodel Resource Manager.
5. Een CNAME DNS-record maken in uw aangepaste domein die naar de DNS-naam van de Manager van verkeer profiel verwijst (<>.... trafficmanager.net).
6. Toegang tot uw site via de domeinnaam niet verkeer profiel DNS-naam van de Manager van ijdelheid.

## <a name="next-steps"></a>Volgende stappen

- Informatie over [de werking van beheer van netwerkverkeer](traffic-manager-how-traffic-manager-works.md).
- Informatie over beheer van netwerkverkeer [controleren eindpunt en een automatische failover](traffic-manager-monitoring.md).
- Informatie over het beheer van verkeer [methoden voor het doorsturen van verkeer](traffic-manager-routing-methods.md).
