<properties
    pageTitle="Azure woordenlijst - woordenboek Azure | Microsoft Azure"
    description="De verklarende woordenlijst van Azure cloud terminologie op de Azure platform inzicht te gebruiken. Deze korte Azure woordenlijst bevat definities voor algemene termen die cloud voor Azure."
    keywords="Woordenlijst Azure, cloud terminologie, Azure woordenlijst, terminologie, definities, cloud voorwaarden"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Verklarende woordenlijst van Microsoft Azure: een woordenlijst met termen op de Azure platform cloud

De verklarende woordenlijst van Microsoft Azure is een korte woordenlijst van de terminologie voor de Azure platform cloud.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Servicedefinities en andere voorwaarden wolk zoeken

* Collega's Zie voor definities van Azure services en hun AWS [Microsoft Azure en Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

* Wolk termen Zie [Cloud computing voorwaarden](https://azure.microsoft.com/overview/cloud-computing-dictionary/)voor algemene industrie.

De Azure verklarende woordenlijst met de bovenstaande twee verwijzingen biedt een end-to-end-taxonomie voor Azure en de cloud-industrie.  

## <a name="azure-glossary-list"></a>Lijst van Azure-verklarende woordenlijst


### <a name="account"></a>account  
Werk of school of persoonlijke Microsoft-account die wordt gebruikt voor toegang tot en beheer van een Azure-abonnement.  
Zie ook [hoe Azure abonnementen zijn gekoppeld aan Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>beschikbaarheid instellen  
Een collectie van virtuele machines die samen worden beheerd voor toepassing redundantie en betrouwbaarheid. Het gebruik van een set beschikbaarheid zorgt ervoor dat tijdens een geplande of niet-gepland onderhoud-gebeurtenis ten minste één virtuele machine beschikbaar.  
Zie ook [de beschikbaarheid van Windows virtuele machines beheren](./virtual-machines/virtual-machines-windows-manage-availability.md) of [beheren van de beschikbaarheid van Linux virtuele machines](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Model klassieke Azure-implementatie  
Een van de twee [implementatiemodellen](resource-manager-deployment-model.md) voor het implementeren van bronnen in Azure (het nieuwe model is Azure Resource Manager). Sommige Azure bronnen kunnen worden ingezet in één model of de andere, terwijl anderen in beide modellen kunnen worden geïmplementeerd. Richtlijnen voor details van de afzonderlijke Azure resources die model(s) van een resource kan worden geïmplementeerd met.


### <a name="cli"></a>Azure opdrachtregelinterface (CLI)  
Een [opdrachtregelinterface](xplat-cli-install.md) die Azure services beheren vanaf Windows, OSX en Linux pc's kunnen worden gebruikt.


### <a name="powershell"></a>Azure PowerShell  
Een [opdrachtregelinterface](powershell-install-configure.md) Azure services via de opdrachtregel beheren vanaf Windows-pc's. Bepaalde services of functies kunnen alleen via PowerShell of de CLI worden beheerd. Richtlijnen voor elke afzonderlijke Azure resourcedetails die model(s) van een resource kan worden geïmplementeerd met.   
Zie ook [het installeren en configureren van Azure PowerShell](powershell-install-configure.md)


### <a name="arm-model"></a>Azure implementatiemodel voor bronbeheer  
Een van de twee [implementatiemodellen](resource-manager-deployment-model.md) voor het implementeren van resources in Microsoft Azure (de andere is het klassieke implementatiemodel). Sommige Azure bronnen kunnen worden ingezet in één model of de andere, terwijl anderen in beide modellen kunnen worden geïmplementeerd. Richtlijnen voor details van de afzonderlijke Azure resources die model(s) van een resource kan worden geïmplementeerd met.


### <a name="fault-domain"></a>Fout met betrekking tot domein  
De collectie van virtuele machines in een beschikbaarheid die kan mogelijk niet op hetzelfde moment. Een voorbeeld is een groep computers in een rack die een gemeenschappelijke bron- en stopcontact delen. De virtuele machines in een set beschikbaarheid worden automatisch in Azure gescheiden in meerdere domeinen voor fouttolerantie.  
Zie ook [de beschikbaarheid van Windows virtuele machines beheren](./virtual-machines/virtual-machines-windows-manage-availability.md) of [beheren van de beschikbaarheid van Linux virtuele machines](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>geo  
Een gedefinieerde rand voor hand vestigingsplaats gegevens doorgaans met twee of meer regio's. De grenzen kunnen binnen of buiten de nationale grenzen en worden beïnvloed door de btw-verordening. Elke geo heeft ten minste één regio. Voorbeelden van geos zijn Azië en Japan. *Geografie*ook genoemd.  
Zie ook [Azure regio 's](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>geo-replicatie  
Het proces van BLOB's, tabellen en wachtrijen binnen een paar regionale worden automatisch gerepliceerd.  
Zie ook [Actieve Geo-replicatie voor Azure SQL-Database](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>afbeelding  
Een bestand waarin het besturingssysteem en de configuratie van toepassingen die kan worden gebruikt voor het maken van een willekeurig aantal virtuele machines. In Azure er zijn twee typen afbeeldingen: VM afbeelding en de installatiekopie van het besturingssysteem. Een VM-afbeelding bevat een besturingssysteem en alle schijven die zijn aangesloten op een virtuele machine wanneer de installatiekopie is gemaakt. Een installatiekopie van het besturingssysteem bevat alleen een algemene besturingssysteem met geen data schijf-configuraties.  
Zie ook [navigeren en selecteer Windows virtuele machine afbeeldingen in Azure PowerShell of de CLI](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>limieten  
Het aantal resources dat kunnen worden gemaakt of de benchmark voor prestaties die kan worden bereikt. Grenzen zijn meestal gekoppeld aan abonnementen, services en oplossingen.  
Zie ook [Azure abonnement service grenzen, quota's en beperkingen](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>taakverdeling  
Een bron die binnenkomend verkeer tussen computers in een netwerk distribueert. In Azure taakverdeling virtuele machines die zijn gedefinieerd in een set load balancer-verkeer. Een [belasting voor documentconversies](./load-balancer/load-balancer-overview.md) kan worden verbonden met internet kan, of interne.  


### <a name="offer"></a>aanbieding  
De prijzen, verantwoording en bijbehorende voorwaarden van toepassing op een Azure-abonnement.  
Zie de [detailpagina bieden Azure](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>Portal  
Beveiligde webportal te implementeren en beheren van Azure services gebruikt.  Er zijn twee portals: de [Azure portal](http://portal.azure.com/) en de [klassieke portal](http://manage.windowsazure.com/). Sommige services zijn beschikbaar in zowel portals, terwijl anderen alleen beschikbaar in het ene of het andere zijn. De [Azure portal beschikbaarheidstabel](https://azure.microsoft.com/features/azure-portal/availability/) wordt aangegeven welke services in welke portal beschikbaar zijn.  


### <a name="region"></a>regio  
Een gebied binnen een geo dat snijpunt geen nationale grenzen en bevat een of meer datacenters. Prijzen, regionale diensten en type aanbieding beschikbaar op het regioniveau van de. Een regio is doorgaans gekoppeld aan een ander gebied van enkele honderden mijlen weg, vormen een combinatie van een regionale kan zijn. Regionale paren kunnen worden gebruikt als een mechanisme voor noodherstel en scenario's voor hoge beschikbaarheid. Ook genoemd in het algemeen als *locatie*.  
Zie ook [Azure regio 's](best-practices-availability-paired-regions.md)


### <a name="resource"></a>resource  
Een item dat deel uitmaakt van uw oplossing Azure. Elke Azure service kunt u verschillende typen bronnen, zoals databases of virtuele machines te implementeren.   
Zie ook [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>resourcegroep  
Een container in de bronnenbeheerder die in het bezit van de verwante bronnen voor een toepassing. De resourcegroep kan bevatten alle resources voor een toepassing of alleen de resources die logisch zijn gegroepeerd. U kunt bepalen hoe u resources toewijzen aan resourcegroepen op basis van wat het meest zinvol voor uw organisatie.  
Zie ook [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>Sjabloon voor bronbeheer  
Een JSON-bestand waarmee een of meer bronnen van Azure declaratief worden gedefinieerd en die de afhankelijkheden tussen de gebruikte middelen definieert. De sjabloon kan worden gebruikt voor de implementatie van de bronnen consistent en herhaaldelijk.  
Zie ook [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>bronnen  
Een service die levert de bronnen die u kunt implementeren en beheren door bronbeheer. Elke resource provider biedt activiteiten voor het werken met de resources die zijn geïmplementeerd. Resource-providers zijn toegankelijk via de portal Azure, Azure PowerShell en verschillende programming SDK's.  
Zie ook [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>rol  
Een middel voor toegangsbeheer die kunnen worden toegewezen aan gebruikers, groepen en services. Rollen kunnen acties, zoals maken, beheren en lezen op Azure bronnen uit te voeren.  
Zie ook [RBAC: ingebouwde functies](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>serviceovereenkomst (SLA)  
De overeenkomst die de beschrijving van Microsofts toezeggingen voor beschikbaarheid en connectiviteit. Elke Azure service heeft een specifieke SLA.  
Zie ook [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>opslag account  
Een opslag-account waarmee u toegang tot de services Azure Blob, wachtrij, tabel en bestand in Azure opslag. Uw account opslag biedt een unieke naamruimte voor uw opslag Azure-gegevensobjecten.  
Zie ook [over Azure opslag rekeningen](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>abonnement  
De klant overeenkomst met Microsoft waarmee deze Azure services te verkrijgen. De abonnementsprijs en de bijbehorende voorwaarden worden beheerst door het aanbod gekozen voor het abonnement. Zie [Microsoft Online-abonnement-overeenkomst](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Zie ook [hoe Azure abonnementen zijn gekoppeld aan Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>label  
Een indexing term waarmee u resources groeperen op uw vereisten voor het beheer of facturering. U kunt labels gebruiken als u een complexe verzameling van bronnengroepen en bronnen hebt en u moet deze activa op de manier die het handigst visualiseren. U kunt bijvoorbeeld resources die een vergelijkbare rol hebben in uw organisatie of deel uitmaken van dezelfde afdeling labelen.  
Zie ook [werken met labels voor het ordenen van uw resources Azure](resource-group-using-tags.md)


### <a name="update-domain"></a>domein bijwerken  
De collectie van virtuele machines in een set beschikbaarheid die op hetzelfde moment worden bijgewerkt. Virtuele machines in hetzelfde domein update opnieuw zijn opgestart samen tijdens gepland onderhoud. Azure meer dan één update domein wordt nooit opnieuw opgestart tegelijk. Ook wel genoemd een domein upgraden.  
Zie ook [de beschikbaarheid van Windows virtuele machines beheren](./virtual-machines/virtual-machines-windows-manage-availability.md) of [beheren van de beschikbaarheid van Linux virtuele machines](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>virtuele machine  
De software-implementatie van een fysieke computer met een besturingssysteem. Meerdere virtuele machines kunnen gelijktijdig worden uitgevoerd op dezelfde hardware. In Azure zijn virtuele machines beschikbaar in allerlei formaten.  
Zie ook de [documentatie van de virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>uitbreiding van de virtuele machine  
Een bron die gedragingen of functies waarmee een andere programma's werken of bieden de mogelijkheid voor u om te communiceren met een actief computer implementeert. U kunt bijvoorbeeld de extensie VM toegang opnieuw instellen of RAS-waarden op een Azure virtuele machine wijzigen.  
Zie ook [over de virtuele machine-extensies en -functies (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) of [virtuele machine uitbreidingen en functies (Linux)](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>virtueel netwerk  
Een netwerk biedt connectiviteit tussen uw Azure bronnen die gescheiden is van alle andere huurders Azure. Deze kan netwerkgebruikers in ruimten met [meerdere opties](./vpn-gateway/vpn-gateway-cross-premises-options.md)en andere Azure virtuele netwerken via een [VPN-Gateway van Azure](./vpn-gateway/vpn-gateway-about-vpngateways.md) worden aangesloten. U kunt volledig bepalen de blokken van IP-adres, DNS-instellingen voor beveiligingsbeleid en routetabellen binnen dit netwerk.  
Zie ook [virtueel netwerk-overzicht](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Zie ook**  
- [Aan de slag met Azure](https://azure.microsoft.com/get-started/)
- [Wolk resource center](https://azure.microsoft.com/resources/)  
- [Azure voor de zakelijke toepassing](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure in uw datacenter](https://azure.microsoft.com/overview/business-apps-on-azure/) 





