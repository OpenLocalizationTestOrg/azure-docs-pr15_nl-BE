<properties
    pageTitle="SQL Server-toepassing patronen op VMs | Microsoft Azure"
    description="Dit artikel heeft betrekking op patronen van toepassing voor SQL Server op Azure VMs. Biedt oplossing architecten en ontwikkelaars een basis voor goede toepassingsarchitectuur en ontwerp."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Patronen van toepassing en van ontwikkelingsstrategieën voor SQL Server in Azure virtuele Machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Samenvatting:
Om te bepalen welke toepassing patroon of patronen wilt gebruiken voor uw SQL Server-gebaseerde toepassingen in Azure omgeving is een belangrijke beslissing en moet een goed begrip van hoe SQL Server en elk onderdeel van de infrastructuur van Azure samen. Met SQL Server in Azure infrastructuurservices, kunt u gemakkelijk migreren, onderhouden en bewaken van uw bestaande SQL Server-toepassingen gebouwd bij Windows Server virtuele machines in Azure.

Het doel van dit artikel is bedoeld als oplossing voor architecten en ontwikkelaars een basis voor een goede toepassingsarchitectuur en ontwerp, dat zij bij de migratie van bestaande toepassingen op Azure en ontwikkelen van nieuwe toepassingen in Azure kunnen volgen.

Voor elk patroon toepassing vindt u een scenario op locatie, de respectieve cloud-compatibele oplossing en de bijbehorende technische aanbevelingen. Daarnaast wordt toegelicht Azure-specifieke ontwikkelingsstrategieën zodat u uw toepassingen correct kunt ontwerpen. Vanwege de vele mogelijke toepassing patronen, het raadzaam dat architecten en ontwikkelaars de meest geschikte patroon voor hun toepassingen en gebruikers moeten kiezen.

**Technische medewerkers:** Luis Carlos Vargas haring, Madhan Arumugam Ramakrishnan

**Technische revisoren:** Corey schuurmachines, Drew McDaniel, Narayan, Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow, Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Inleiding

Door tussen de onderdelen van de andere toepassingslagen op verschillende computers ook in afzonderlijke componenten kunt u tal van n-tier toepassingen ontwikkelen. Bijvoorbeeld, kunt u de clienttoepassing plaatsen en bedrijfsregels onderdelen in een machine, front-tier en laag voor gegevenstoegang in een andere machine en een reeks back-end-database in een andere computer. Dit soort structureren kunt isoleren elke laag van elkaar. Als u waar gegevens worden opgehaald wijzigen, hoeft u niet te wijzigen, maar de toepassing client- of alleen de lagen voor gegevenstoegang.

Een typische *n-tier* -toepassing bevat de presentatielaag, de business-laag en de gegevenslaag:


| Laag              | Beschrijving                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Presentatie** | De *presentatielaag* (web tier, tier front-end) is de laag waarin gebruikers interactie met een toepassing.                                                                      |
| **Business**     | De *business trapsgewijs* (middelste laag) is de laag die trapsgewijs van de presentatie en de gegevenslaag gebruiken om te communiceren met elkaar en bevat de belangrijkste functies van het systeem. |
| **Gegevens**         | De *gegevenslaag* is in feite de server waarin de gegevens worden van de toepassing (bijvoorbeeld een server waarop SQL Server wordt uitgevoerd).                                                             |

Toepassingslagen beschrijven de logische groeperingen van de functies en onderdelen in een toepassing. Overwegende dat de lagen beschrijven de fysieke verdeling van de functies en onderdelen op verschillende fysieke servers, computers, netwerken of externe locaties. De lagen van een toepassing kunnen zich bevinden op dezelfde fysieke computer (hetzelfde niveau) of kunnen worden verdeeld over verschillende computers (n-tier) en de onderdelen in elke laag communiceren met componenten in andere lagen via goed gedefinieerde interfaces. U kunt de term laag als verwijzingen naar de fysieke distributiepatronen zoals tweelagige drie lagen en n-lagige beschouwen. Een **patroon 2-tier-toepassing** bevat twee lagen: toepassingsserver en databaseserver. Er gebeurt de rechtstreekse communicatie tussen de toepassingsserver en de databaseserver. De application server bevat web-tier-en business-laag. In het **patroon 3-tier-toepassing**, zijn er drie lagen: webserver, toepassingsserver, die de business logica laag en/of business tier data access-componenten bevat, en de databaseserver. De communicatie tussen de webserver en de databaseserver gebeurt via de application server. Zie [Microsoft Application Architecture Guide](https://msdn.microsoft.com/library/ff650706.aspx)voor gedetailleerde informatie over toepassingslagen en lagen.

Voordat u begint met het lezen van dit artikel, hebt u kennis van de fundamentele concepten van SQL Server en Azure. Zie de [On line boekbestanden van SQL Server](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md) en [Azure.com](https://azure.microsoft.com/)voor informatie.

Dit artikel beschrijft verschillende patronen van toepassing die geschikt is voor uw eenvoudige toepassingen zoals de zeer complexe bedrijfstoepassingen kunnen worden. Voordat u met informatie over elk patroon, is het raadzaam dat moet u vertrouwd raken met de beschikbare gegevens storage-services in Azure, zoals [Opslag Azure](../storage/storage-introduction.md), [Azure SQL-Database](../sql-database/sql-database-technical-overview.md)en [SQL Server een Azure Virtual Machine](virtual-machines-windows-sql-server-iaas-overview.md). Als u wilt de beste beslissingen voor uw toepassingen te ontwerpen, te begrijpen wanneer duidelijk welke data storage-service gebruikt.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Kies SQL Server in een virtuele Machine Azure, wanneer:

- U moet een besturingselement voor SQL Server en Windows. Dit kan bijvoorbeeld de versie van SQL Server, speciale hotfixes, configuratie van prestaties, enz.

- U moet een volledige compatibiliteit met SQL Server op ruimten en verplaats de bestaande toepassingen naar Azure als-is.

- Azure SQL-Database ondersteunt niet alle functies die uw toepassing vereist, maar u wilt profiteren van de mogelijkheden van de Azure-omgeving. Het kan hierbij gaan de volgende gebieden:

    - **Grootte van de database**: op het moment dat dit artikel is bijgewerkt, ondersteunt SQL-Database een database van maximaal 1 TB aan gegevens. Als uw toepassing meer dan 1 TB aan gegevens vereist en u niet wilt dat om aangepaste sharding oplossingen te implementeren, het raadzaam dat u SQL Server in een Azure Virtual Machine gebruikt. Zie [Schalen van Azure SQL-Databases](https://msdn.microsoft.com/library/azure/dn495641.aspx) en [Azure SQL Database Service Tiers en prestaties](../sql-database/sql-database-service-tiers.md)voor de meest recente informatie.
    - **Dergelijke wetgeving**: gezondheidszorg klanten en onafhankelijke softwareleveranciers (ISV's) kunt [Azure SQL-Database](../sql-database/sql-database-technical-overview.md) [SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md) omdat SQL Server in een Azure Virtual Machine is gedekt door de HIPAA Business koppelen overeenkomst (BAA). Zie voor informatie over compatibiliteit, [Microsoft Azure Trust Center: naleving](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Instantieniveau functies**: op dit moment is de SQL-Database functies die buiten de database niet ondersteunt (zoals gekoppelde Servers Agent banen, FileStream, Service Broker, enz.). Zie [Azure SQL Database richtlijnen en beperkingen](https://msdn.microsoft.com/library/azure/ff394102.aspx)voor meer informatie.

## <a name="1-tier-simple-single-virtual-machine"></a>1 laag (eenvoudig): één virtuele machine

In dit patroon toepassing implementeert u uw SQL Server-toepassing en de database met een zelfstandige virtuele machine in Azure. De virtuele machine bevat uw client/webtoepassing, bedrijfsonderdelen gegevenstoegangslaag en de databaseserver. De presentatie en business data access code logisch worden gescheiden, maar fysiek aanwezig zijn in een machine met één server. De meeste klanten beginnen met het patroon van deze toepassing en vervolgens een schaalbaar uitbreiden door meer functies van de website of virtuele machines toe te voegen aan hun systeem.

Dit patroon van toepassing is handig wanneer:

- Wilt u een eenvoudige migratie naar Azure platform te beoordelen of het platform eisen van uw toepassing of niet antwoorden uit te voeren.

- U wilt alle de lagen die worden gehost in virtual machine in dezelfde Azure datacenter te verminderen van de wachttijd tussen lagen behouden.

- U wilt snel creëren van ontwikkeling en testen omgevingen voor korte perioden.

- U wilt uitvoeren van stress testen op verschillende niveaus van de werkbelasting, maar op hetzelfde moment u niet wilt om te hebben en veel fysieke machines voortdurend onderhouden.

In het volgende diagram ziet u een eenvoudige op-lokalen scenario en hoe u de oplossing cloud ingeschakeld in virtuele machines in Azure kunt implementeren.

![patroon 1-tier-toepassing](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Implementatie van de business-laag (bedrijfslogica en data access-componenten) op hetzelfde fysieke niveau als de presentatie laag kan prestaties toepassing, tenzij u moet een afzonderlijke laag omdat u de schaalbaarheid of beveiliging.

Aangezien dit een zeer algemene patroon beginnen, nuttig het volgende artikel voor migratie voor het verplaatsen van de gegevens voor uw SQL Server-VM: [een Database aan SQL Server op een Azure VM migratie](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3-tier (eenvoudig): meerdere virtuele machines

In dit patroon toepassing implementeert u een 3-tier-toepassing in Azure door elke toepassingslaag in een andere virtuele machine. Dit biedt een flexibele omgeving voor een eenvoudige scenario's opschaling en scale-out. Wanneer een virtuele machine uw client/webtoepassing bevat, een fungeert als host voor de onderdelen van uw business en fungeert als een databaseserver.

Dit patroon van toepassing is handig wanneer:

- Wilt u een migratie van databasetoepassingen van complexe Azure virtuele Machines uit te voeren.

- Wilt u andere lagen te worden ondergebracht in verschillende regio's. Zoals misschien u hebt gedeeld databases die worden gebruikt voor meerdere regio's voor rapportagedoeleinden.

- U wilt verplaatsen van bedrijfstoepassingen van platforms voor bedrijfsruimten gevirtualiseerde Azure virtuele Machines. Zie voor een gedetailleerde bespreking van bedrijfstoepassingen, [wat een bedrijfstoepassing is](https://msdn.microsoft.com/library/aa267045.aspx).

- U wilt snel creëren van ontwikkeling en testen omgevingen voor korte perioden.

- U wilt uitvoeren van stress testen op verschillende niveaus van de werkbelasting, maar op hetzelfde moment u niet wilt om te hebben en veel fysieke machines voortdurend onderhouden.

In het volgende diagram ziet u hoe u een eenvoudige 3-tier-toepassing kunt plaatsen in Azure door elke toepassingslaag in een andere virtuele machine.

![patroon 3-tier-toepassing](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

In dit patroon toepassing is slechts één virtuele machine (VM) in elke laag. Als er meerdere VMs in Azure, wordt u aangeraden een virtueel netwerk in te stellen. [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) maakt een vertrouwde beveiligingsgrens en kunnen ook VMs communiceren onderling via het particuliere IP-adres. Bovendien altijd voor zorgen dat alle Internet-verbindingen alleen gaat u naar de presentatielaag. Wanneer deze toepassing patroon te volgen, de regels voor groep netwerk toegang beheren Zie [externe toegang toestaan voor uw VM met de Azure portal](virtual-machines-windows-nsg-quickstart-portal.md)voor meer informatie.

In het diagram wordt een Internet-protocollen TCP, UDP, HTTP of HTTPS.

>[AZURE.NOTE] Het instellen van een virtueel netwerk in Azure is gratis. Echter, u in rekening worden gebracht voor de VPN-gateway die is verbonden met de op het bedrijf. Deze toeslag is gebaseerd op de hoeveelheid tijd die verbinding ingericht en beschikbaar is.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>niveaus 2 en 3-tier met presentatie laag schalen

In dit patroon toepassing implementeert u laag 2 of 3-tier databasetoepassing Azure virtuele Machines door elke toepassingslaag in een andere virtuele machine. Bovendien schaalt u de presentatielaag vanwege toegenomen aantal binnenkomende clientaanvragen.

Dit patroon van toepassing is handig wanneer:

- U wilt verplaatsen van bedrijfstoepassingen van platforms voor bedrijfsruimten gevirtualiseerde Azure virtuele Machines.

- U wilt schalen uit de presentatielaag vanwege toegenomen aantal binnenkomende clientaanvragen.

- U wilt snel creëren van ontwikkeling en testen omgevingen voor korte perioden.

- U wilt uitvoeren van stress testen op verschillende niveaus van de werkbelasting, maar op hetzelfde moment u niet wilt om te hebben en veel fysieke machines voortdurend onderhouden.

- Wilt u de eigenaar van een infrastructuur omgeving die omhoog en omlaag op aanvraag schalen kunt.

In het volgende diagram laat zien hoe kunt u de lagen in meerdere virtuele machines in Azure plaatsen door de presentatielaag vanwege toegenomen aantal binnenkomende clientaanvragen schalen. Zoals u in het diagram, is Azure Load Balancer verantwoordelijk voor verkeer distribueren over meerdere virtuele machines en ook om te bepalen welke webserver verbinding maken met. Met meerdere exemplaren van de webservers achter een load balancer zorgt voor de beschikbaarheid van de presentatielaag.

![Toepassing patroon - presentatie laag geschaald uitbreiden](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Aanbevolen procedures voor laag 2 of 3-tier n-lagige patronen met meerdere VMs in één laag

Het wordt aanbevolen dat u plaatst de virtuele machines die deel uitmaken van hetzelfde niveau in dezelfde cloud service en dezelfde de beschikbaarheid instellen. Plaats bijvoorbeeld een set van webservers in **CloudService1** en **AvailabilitySet1** en een reeks databaseservers in **CloudService2** en **AvailabilitySet2**. De beschikbaarheid in Azure instellen kunt u de knooppunten met hoge beschikbaarheid in een fout met betrekking tot afzonderlijke domeinen en domeinen upgraden.

Als u wilt gebruikmaken van meerdere VM exemplaren van een laag, moet u Azure taakverdeling configureren tussen lagen. Voor informatie over het configureren van taakverdeling in elke laag een taakverdeling eindpunt maken op elke laag van VMs afzonderlijk. Voor een specifieke laag, moet u eerst VMs maken in dezelfde cloud-service. Dit zorgt ervoor dat ze hetzelfde openbare virtuele IP-adres hebben. Maak vervolgens een eindpunt van een van de virtuele machines op die laag. Hetzelfde eindpunt vervolgens toewijzen aan de andere virtuele machines op dat niveau voor load balancing. U maakt een set verdeeld, verkeer verdelen over meerdere virtuele machines en waarmee ook de Load Balancer om te bepalen welk knooppunt verbinding maken wanneer u een back-end VM knooppunt uitvalt. Met meerdere exemplaren van de webservers achter een load balancer zorgt voor bijvoorbeeld de beschikbaarheid van de presentatielaag.

Beste, altijd voor zorgen dat alle internet-verbindingen eerst gaat u naar de presentatielaag. De presentatie laag toegang heeft tot de business-laag en vervolgens de niveaus business toegang heeft tot de gegevenslaag. Zie voor meer informatie over het toestaan van toegang tot de presentatie laag, [externe toegang toestaan voor de met behulp van de portal voor Azure VM](virtual-machines-windows-nsg-quickstart-portal.md).

Houd er rekening mee dat de taakverdeling in Azure werkt ongeveer zoals netwerktaakverdelers in een omgeving op gebouwen. Zie [Netwerktaakverdeling voor Azure infrastructuurservices](virtual-machines-windows-load-balance.md)voor meer informatie.

Daarnaast raden wij aan dat u van een particulier netwerk voor uw virtuele machines met behulp van Azure Virtual Network. Hierdoor kunnen ze communiceren onderling via het particuliere IP-adres. Zie [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)voor meer informatie.

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>niveaus 2 en 3-tier met zakelijke laag schalen

In dit patroon toepassing implementeert u laag 2 of 3-tier databasetoepassing Azure virtuele Machines door elke toepassingslaag in een andere virtuele machine. Bovendien kunt u de application serveronderdelen distribueren naar meerdere virtuele machines als gevolg van de complexiteit van de toepassing.

Dit patroon van toepassing is handig wanneer:

- U wilt verplaatsen van bedrijfstoepassingen van platforms voor bedrijfsruimten gevirtualiseerde Azure virtuele Machines.

- Wilt u de application serveronderdelen distribueren naar meerdere virtuele machines als gevolg van de complexiteit van de toepassing.

- Wilt u business logica zware op-premises LOB-(line-of-business) toepassingen Azure virtuele Machines te verplaatsen. LOB-toepassingen zijn een aantal essentiële toepassingen die cruciaal zijn voor het uitvoeren van een onderneming, zoals boekhouding, HR (human resources) salaris, toeleveringsbeheer en resourceplanning toepassingen.

- U wilt snel creëren van ontwikkeling en testen omgevingen voor korte perioden.

- U wilt uitvoeren van stress testen op verschillende niveaus van de werkbelasting, maar op hetzelfde moment u niet wilt om te hebben en veel fysieke machines voortdurend onderhouden.

- Wilt u de eigenaar van een infrastructuur omgeving die omhoog en omlaag op aanvraag schalen kunt.

In het volgende diagram ziet u een scenario op locatie en de oplossing van de wolk is ingeschakeld. In dit scenario kunt u de lagen in meerdere virtuele machines in Azure plaatsen door de schaal van de business-laag die de business logica laag en de data access-componenten bevat. Zoals u in het diagram, is Azure Load Balancer verantwoordelijk voor verkeer distribueren over meerdere virtuele machines en ook om te bepalen welke webserver verbinding maken met. Met meerdere exemplaren van de toepassingsservers achter een load balancer zorgt voor de beschikbaarheid van de business-laag. Zie [Aanbevolen procedures voor laag 2, 3-tier, of patronen van n-tier-toepassing die meerdere virtuele machines in één laag hebben](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier)voor meer informatie.

![Patroon met business tier schaal van toepassing](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>niveaus 2 en 3-tier met presentatie en zakelijke lagen schalen en HADR

In dit patroon toepassing implementeren u de presentatielaag (webserver) en de zakelijke lagen (application server) onderdelen om meerdere virtuele machines te distribueren laag 2 of 3-tier databasetoepassing Azure virtuele Machines. U kunt bovendien hersteloplossingen voor hoge beschikbaarheid en noodherstel implementeren voor uw databases in Azure virtuele machines.

Dit patroon van toepassing is handig wanneer:

- Wilt u bedrijfstoepassingen gevirtualiseerde platforms op ruimten naar Azure verplaatsen door de uitvoering van de beschikbaarheid van SQL Server en disaster recovery mogelijkheden.

- Wilt u de schaal van de presentatielaag en de zakelijke laag vanwege de grotere omvang van de binnenkomende clientaanvragen en de complexiteit van uw toepassing.

- U wilt snel creëren van ontwikkeling en testen omgevingen voor korte perioden.

- U wilt uitvoeren van stress testen op verschillende niveaus van de werkbelasting, maar op hetzelfde moment u niet wilt om te hebben en veel fysieke machines voortdurend onderhouden.

- Wilt u de eigenaar van een infrastructuur omgeving die omhoog en omlaag op aanvraag schalen kunt.

In het volgende diagram ziet u een scenario op locatie en de oplossing van de wolk is ingeschakeld. In dit scenario wordt u de schaal van de presentatielaag en de business tier onderdelen in meerdere virtuele machines in Azure. U kunt bovendien hoge beschikbaarheid en noodherstel (HADR) technieken voor SQL Server-databases implementeren in Azure.

Uitvoeren van meerdere exemplaren van een toepassing in verschillende zorg VMs ervoor dat u aanvragen voor taakverdeling tussen hen zijn. Als er meerdere virtuele machines, moet u om ervoor te zorgen dat alle uw VMs toegankelijk en worden uitgevoerd op één punt in de tijd zijn. Als u Netwerktaakverdeling configureert, Azure Load Balancer de gezondheid van de VMs en bijgehouden zorgt ervoor dat binnenkomende gesprekken naar de gezonde goed functionerende VM knooppunten correct. Zie [taakverdeling voor Azure infrastructuurservices](virtual-machines-windows-load-balance.md)voor meer informatie over het instellen van taakverdeling van de virtuele machines. Met meerdere exemplaren van web- en servers achter een load balancer zorgt voor de beschikbaarheid van de presentatie en de zakelijke lagen.

![Schalen en hoge beschikbaarheid](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Aanbevolen procedures voor toepassing patronen SQL HADR vereisen

Bij het instellen van een hoge beschikbaarheid van SQL Server en disaster recovery oplossingen in Azure virtuele Machines, is het instellen van een virtueel netwerk voor uw virtuele machines met [Virtual Network Azure](../virtual-network/virtual-networks-overview.md) verplicht.  Virtuele machines in een virtueel netwerk krijgt een stabiele particulier IP-adres na een onderhoudstijd, dus kunt u voorkomen dat de update duurt voor DNS-naamomzetting. Bovendien, het virtuele netwerk kunt u uw netwerk op het bedrijf uitbreiden naar Azure en maakt een vertrouwde beveiligingsgrens. Bijvoorbeeld als uw toepassing bedrijfsdomein beperkingen (bijvoorbeeld Windows-verificatie, Active Directory), het instellen van [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) is nodig.

De meeste klanten, die de productiecode op Azure uitvoert, zijn replica's van primaire en secundaire houden in Azure.

Zie voor uitgebreide informatie en zelfstudies voor hoge beschikbaarheid en herstel na storing hersteltechnieken, [hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>niveaus 2 en 3-tier met Azure VMs en Cloud Services

In dit patroon toepassing u Azure laag 2 of 3-tier toepassing implementeren met behulp van beide [Azure Cloud Services](../cloud-services/cloud-services-choose-me.md#tellmecs) (web werknemer rollen en -Platform als een Service (PaaS)) en [Azure virtuele Machines](virtual-machines-windows-about.md) (infrastructuur als Service (IaaS)). [Azure Cloud-Services](https://azure.microsoft.com/documentation/services/cloud-services/) gebruiken voor de presentatielaag laag/business en SQL Server in [Azure virtuele Machines](virtual-machines-windows-about.md) voor de gegevenslaag is nuttig voor de meeste toepassingen die worden uitgevoerd op Azure. De reden is dat met een compute-exemplaar met Cloud Services biedt een eenvoudig beheer, implementatie, bewaking en schalen.

Met Cloud Services, Azure onderhoudt de infrastructuur voor u, dagelijks onderhoud uitvoert, patches van de besturingssystemen en probeert te herstellen van service-en hardwarefouten. Wanneer uw toepassing schalen moet, zijn automatisch en handmatig schalen opties beschikbaar voor uw project cloud-service door de oplopende of aflopende volgorde van het aantal exemplaren of virtuele machines die worden gebruikt door de toepassing. Bovendien kunt u Visual Studio in ruimten voor de implementatie van de toepassing naar een cloud service project in Azure.

Kortom, als u niet wilt bezit uitgebreide trapsgewijs beheertaken voor de presentatie/bedrijf en uw toepassing niet alle complexe configuratie van software of het besturingssysteem vereist, Azure Cloud Services gebruiken. Als Azure SQL-Database niet alle functies die u zoekt ondersteunt, gebruikt u SQL Server in een Azure Virtual Machine voor de gegevenslaag. Combineert de voordelen van beide services uitvoeren van een toepassing op Azure Cloud Services en gegevens opslaan in Azure virtuele Machines. Zie de sectie in dit onderwerp op [de ontwikkelingsstrategieën Comparing in Azure](#comparing-web-development-strategies-in-azure)voor een gedetailleerde vergelijking.

In dit patroon toepassing omvat de presentatielaag een Webrol is een onderdeel van de Cloud-Services in de Azure omgeving uitgevoerd en deze is aangepast voor web application programming, ondersteund door IIS en ASP.NET. De bedrijfs- of back-end-laag bevat de rol van een werknemer, die is een onderdeel van de Cloud-Services in de Azure omgeving uitgevoerd en is handig voor de ontwikkeling van algemene en achtergrond voor een web-functie kan uitvoeren. De databaselaag bevindt zich in een SQL Server virtuele machine in Azure. De communicatie tussen de presentatielaag en de databaselaag gebeurt rechtstreeks of via de business tier – werknemer rol onderdelen.

Dit patroon van toepassing is handig wanneer:

- Wilt u bedrijfstoepassingen gevirtualiseerde platforms op ruimten naar Azure verplaatsen door de uitvoering van de beschikbaarheid van SQL Server en disaster recovery mogelijkheden.

- Wilt u de eigenaar van een infrastructuur omgeving die omhoog en omlaag op aanvraag schalen kunt.

- Azure SQL-Database ondersteunt niet alle functies die uw toepassing of database nodig.

- U wilt uitvoeren van stress testen op verschillende niveaus van de werkbelasting, maar op hetzelfde moment u niet wilt om te hebben en veel fysieke machines voortdurend onderhouden.

In het volgende diagram ziet u een scenario op locatie en de oplossing van de wolk is ingeschakeld. In dit scenario moet u de presentatielaag in web-rollen, de business-laag in de rollen van de werknemer, maar de gegevenslaag in virtuele machines in Azure plaatsen. Meerdere exemplaren van de presentatielaag uitgevoerd in verschillende web rollen zorgt voor het laden van de aanvragen over deze. Wanneer u Azure Cloud Services Azure virtuele Machines combineert, wordt aangeraden dat u [Virtual Network Azure](../virtual-network/virtual-networks-overview.md) ook instellen. Met [Virtual Network Azure](../virtual-network/virtual-networks-overview.md)hebt u stabiel en permanente particuliere IP-adressen binnen dezelfde cloud-service in de cloud. Nadat u een virtueel netwerk voor uw virtuele machines definiëren en cloud services, kan worden begonnen met elkaar communiceren via het particuliere IP-adres. Bovendien biedt met virtuele machines en Azure web/werknemer rollen in hetzelfde [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) lage latentie en meer beveiligde verbindingen. Zie [Wat is een cloud-service](../cloud-services/cloud-services-choose-me.md)voor meer informatie.

In het diagram weergegeven, verkeer verdeelt over meerdere virtuele machines en bepaalt ook welke webserver of application server verbinding maken met Azure Load Balancer. Met meerdere exemplaren van de web- en servers achter een load balancer zorgt voor de beschikbaarheid van de presentatielaag en de business-laag. Zie [Aanbevolen procedures voor toepassing patronen waarin SQL HADR](#best-practices-for-application-patterns-requiring-sql-hadr)voor meer informatie.

![Toepassing van patronen met Cloud Services](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Een andere manier voor het implementeren van dit patroon van toepassing is met een geconsolideerde Webrol dat presentatielaag en de business tier onderdelen zoals in het volgende diagram bevat. Dit patroon van toepassing is handig voor toepassingen waarvoor stateful ontwerp. Aangezien Azure stateless computerknooppunten op het web en werknemer rollen, wordt aangeraden dat u een logica voor het opslaan van de status van de sessie met een van de volgende technologieën implementeren: [Azure Caching](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure tabelopslag](../storage/storage-dotnet-how-to-use-tables.md) of [Azure SQL-Database](../sql-database/sql-database-technical-overview.md).

![Toepassing van patronen met Cloud Services](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Patroon met Azure VMs, Azure SQL-Database en Azure App Service (Web Apps)

Het voornaamste doel van dit patroon van toepassing is om te zien hoe Azure infrastructuur als een (IaaS) serviceonderdelen met Azure platform as a service-onderdelen (PaaS) in uw oplossing combineert u. Dit patroon is gericht op Azure SQL-Database voor relationele gegevensopslag. Hierbij worden SQL Server op een virtuele machine Azure, die deel uitmaakt van de infrastructuur Azure als een service aanbieden.

In dit patroon toepassing implementeert u een databasetoepassing naar Azure door de presentatie en de zakelijke lagen op dezelfde virtuele machine te plaatsen en de toegang tot een database op servers Azure SQL-Database (SQL-Database). U kunt de presentatielaag implementeren met behulp van traditionele gebaseerde IIS-weboplossingen. Of u een zakelijke laag en een samengevoegde presentatie kunt implementeren met behulp van [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/).

Dit patroon van toepassing is handig wanneer:

- U hebt al een bestaande SQL-databaseserver in Azure is geconfigureerd en u wilt snel testen van de toepassing.

- Wilt u de mogelijkheden van Azure-omgeving te testen.

- U wilt snel creëren van ontwikkeling en testen omgevingen voor korte perioden.

- Uw business logica en data access-componenten zijn zelfstandig binnen een webtoepassing.

In het volgende diagram ziet u een scenario op locatie en de oplossing van de wolk is ingeschakeld. In dit scenario plaatst u de lagen in een enkele virtuele machine in Azure en access-gegevens in Azure SQL-Database.

![Gemengde toepassing patroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Als u een gecombineerde webpagina en tier toepassing implementeren met behulp van Azure Web Apps, wordt u aangeraden de middenlaag of toepassing laag te houden als dynamic link libraries (DLL's) in het kader van een webtoepassing.

Bekijk ook de aanwijzingen in de sectie [Comparing web ontwikkelingsstrategieën in Azure](#comparing-web-development-strategies-in-azure) aan het einde van dit artikel voor meer informatie over het programmeren technieken.

## <a name="n-tier-hybrid-application-pattern"></a>Hybride van N-tier toepassing patroon

In hybride n-tier toepassing patroon, kunt u uw toepassing in meerdere lagen verdeeld tussen de lokale en Azure implementeren. U maakt een flexibele en herbruikbare hybride systeem, die u kunt wijzigen of een specifieke laag toevoegen zonder de andere lagen. Uitgebreid netwerk van uw bedrijf naar de cloud, u [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) service.

Deze toepassing hybride patroon is handig wanneer:

- Wilt u bouwen van toepassingen die gedeeltelijk in de cloud en deels op-ruimten.

- U wilt migreren van enkele of alle elementen van een bestaande toepassing op ruimten naar de cloud.

- Wilt u de bedrijfstoepassingen van platforms voor bedrijfsruimten gevirtualiseerde verplaatsen naar Azure.

- Wilt u de eigenaar van een infrastructuur omgeving die omhoog en omlaag op aanvraag schalen kunt.

- U wilt snel creëren van ontwikkeling en testen omgevingen voor korte perioden.

- Wilt u een voordelige manier nemen van back-ups van toepassingen voor bedrijfsdatabases.

In het volgende diagram ziet u een patroon van hybride n-tier-toepassing die van toepassing is op de lokale en Azure. Zoals in het diagram, bevat op gebouwen infrastructuur [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) -domeincontroller voor de ondersteuning van verificatie en machtiging. Houd er rekening mee dat het diagram ziet u een scenario waarin sommige delen van de gegevenslaag live in een datacenter op gebouwen dat sommige onderdelen van de gegevenslaag live in Azure. Afhankelijk van de behoeften van uw toepassing, kunt u verschillende andere hybride scenario's te implementeren. U zou bijvoorbeeld de presentatielaag en de business-laag in een omgeving van gebouwen, maar de gegevenslaag in Azure houden.

![Patroon van N-tier-toepassing](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

In Azure, kunt u Active Directory als een zelfstandige cloud-map voor uw organisatie of u kunt ook bestaande op ruimten met Active Directory integreren met [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Zoals weergegeven in het diagram, toegankelijk de business tier componenten met meerdere gegevensbronnen, zoals [SQL](virtual-machines-windows-sql-server-iaas-overview.md) server in Azure via een particuliere interne IP-adres, SQL-Server via een [Virtueel netwerk Azure](../virtual-network/virtual-networks-overview.md)voor gebouwen, of met behulp van de technologieën van .NET Framework data provider voor [SQL-Database](../sql-database/sql-database-technical-overview.md) . In dit diagram is Azure SQL-Database een optionele gegevens storage-service.

U kunt de volgende workflow in de opgegeven volgorde implementeren in hybride n-tier toepassing patroon:

1. Enterprise database-toepassingen die worden teruggezet moet naar de cloud met [Microsoft Assessment and Planning (kaart) Toolkit](http://microsoft.com/map)identificeren. De Toolkit kaart voorraad- en gegevens worden verzameld van computers, die bent u van plan voor virtualisatie en aanbevelingen van capaciteit en planning van de beoordeling.

1. Plan de bronnen en de configuratie in de Azure platform, zoals accounts voor opslag en virtuele machines nodig.

1. Netwerkverbinding tussen het bedrijfsnetwerk op gebouwen en [Azure virtueel netwerk](../virtual-network/virtual-networks-overview.md)instellen. Als u de verbinding tussen het bedrijfsnetwerk op gebouwen en een virtuele machine in Azure instelt, gebruikt u een van de volgende twee methoden:

    1. Verbinding tussen de lokale en Azure via openbare eindpunten op een virtuele machine in Azure. Deze methode biedt een eenvoudige installatie en kunt u met SQL Server-verificatie op de virtuele machine. Bovendien uw netwerk security groep regels instellen om openbare verkeer naar de VM. Zie [externe toegang toestaan voor uw VM met de Azure portal](virtual-machines-windows-nsg-quickstart-portal.md)voor meer informatie.

    1. Verbinding tussen de lokale en Azure via de tunnel van Azure Virtual Private network (VPN). Met deze methode kunt u een virtuele machine in Azure domeinbeleid installeren uitbreiden. Bovendien kunt u firewall-regels instellen en het gebruik van Windows-verificatie op de virtuele machine. Op dit moment ondersteunt Azure veilig VPN van site naar site en point-to-site VPN-verbindingen:

        - Met de beveiligde verbinding van site naar site, kunt u geen netwerkverbinding tussen uw netwerk in de lokalen en het virtuele netwerk in Azure. Het wordt aanbevolen voor de verbinding met uw op ruimten datacenteromgeving Azure.

        - Met een beveiligde verbinding punt-naar-site, kan geen netwerkverbinding tussen uw virtuele netwerk in Azure en afzonderlijke computers met overal. Meestal verdient het oog op de ontwikkeling en testen.

    Zie voor meer informatie over hoe u verbinding maakt met SQL Server in Azure, [verbinding maken met een virtuele Machine op Azure in SQL Server](virtual-machines-windows-classic-sql-connect.md).

1. Geplande taken en waarschuwingen die back-up van gegevens in een virtuele machine schijf in Azure voor ruimten instellen. Zie [SQL Server-back-up en terugzetten met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx) en [back-up en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md)voor meer informatie.

1. Afhankelijk van de behoeften van uw toepassing, kunt u een van de volgende drie veelvoorkomende scenario's implementeren:

    1. Kunt u uw webserver, toepassingsserver en weinig gevoelige gegevens op een databaseserver in Azure dat u de vertrouwelijke gegevens van ruimten.

    1. Kunt u uw webserver en toepassingsserver op-bedrijf dat de databaseserver in een virtuele machine in Azure.

    1. Kunt u uw databaseserver, webserver en toepassingsserver op-bedrijf dat de database replica's kunt u opslaan in virtuele machines in Azure. Met deze instelling kunt de webservers op gebouwen of rapportage toepassingen toegang krijgen tot de database replica's in Azure. Daarom kunt u bereiken lagere de werkbelasting in een database op gebouwen. Het is raadzaam het implementeren van dit scenario voor zware werkbelasting en ontwikkeling doeleinden lezen. Zie voor meer informatie over het maken van databasereplica in Azure AlwaysOn beschikbaarheidsgroepen met [hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Web ontwikkelingsstrategieën in Azure vergelijken

Als u wilt implementeren en te gebruiken een meerlaags SQL Server-toepassing in Azure, kunt u een van de volgende twee methoden programmeren:

- Een traditionele webserver (IIS - Internet Information Services) instellen in Azure en access-databases in SQL Server in Azure virtuele Machines.

- Implementatie en een cloud service implementeren op Azure. Zorg dat deze cloud-service toegang databases in SQL Server in Azure Virtual machines tot. Een cloud-service kan meerdere web- en werknemer rollen bevatten.

De volgende tabel bevat een vergelijking van de traditionele websites ontwikkelen met Azure Cloud Services en Azure Web Apps voor SQL Server in Azure virtuele Machines. De tabel bevat Azure Web Apps kunt u SQL Server gebruikt in Azure VM als gegevensbron voor Azure Web Apps via de openbare virtuele IP-adres of DNS-naam.

||Traditionele webontwikkeling Azure virtuele Machines|Cloud Services in Azure|Webhosting met Azure Web Apps|
|---|---|---|---|
|**Toepassingen migreren op ruimten**|Bestaande toepassingen als-is.|Toepassingen moeten rollen web en werknemer.|Bestaande toepassingen, zoals-maar geschikt is voor zelfstandig webtoepassingen en webservices die snelle schaalbaarheid vereisen.|
|**Ontwikkeling en implementatie**|Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP-, TFS, IIS-beheer PowerShell.|Visual Studio Azure SDK, TFS, PowerShell. Elke service cloud heeft twee omgevingen waarin u uw servicepakket en configuratie kunt implementeren: ontwikkel- en productiecomputers. U kunt een cloud service implementeren voor de testomgeving te testen voordat u het doorverwijzen naar productie.|Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, GitHub, volgt, TFS, Web implementeren, PowerShell.|
|**Beheers- en**|U bent verantwoordelijk voor administratieve taken op de toepassing, gegevens firewallregels, virtueel netwerk en besturingssysteem.|U bent verantwoordelijk voor administratieve taken op de toepassing, gegevens, firewallregels en virtueel netwerk.|U bent verantwoordelijk voor administratieve taken inzake de toepassing en alleen gegevens.|
|**Hoge beschikbaarheid en noodherstel (HADR)**|Wij raden aan dat u virtuele machines in dezelfde set beschikbaarheid en in dezelfde service cloud plaatsen. Houden van uw VMs in dezelfde kunt set beschikbaarheid Azure de knooppunten met hoge beschikbaarheid in een fout met betrekking tot afzonderlijke domeinen en domeinen upgraden. Op dezelfde manier houden uw VMs in dezelfde service cloud kunt taakverdeling en VMs via het lokale netwerk binnen een datacenter Azure met elkaar kunnen communiceren.<br/><br/>U bent verantwoordelijk voor de uitvoering van een hoge beschikbaarheid en disaster recovery-oplossing voor SQL Server in Azure virtuele Machines, elke uitvaltijd te voorkomen. Zie voor ondersteunde technologieën HADR, [hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>U bent verantwoordelijk voor de back-ups van uw eigen gegevens en toepassingen.<br/><br/>Azure kan uw virtuele machines kunt verplaatsen als de hostcomputer in het datacenter is mislukt als gevolg van hardwareproblemen. Bovendien kan er geplande uitvaltijd van uw VM wanneer de hostcomputer wordt bijgewerkt voor beveiliging of software updates. Daarom is het raadzaam dat u ten minste twee VMs in elke toepassingslaag om de continue beschikbaarheid bijhouden. Azure biedt voor een enkele virtuele machine geen SLA. Zie voor meer informatie, [technische richtsnoeren Azure herstellingsvermogen](../resiliency/resiliency-technical-guidance.md).|Azure beheert de storingen die voortvloeien uit de onderliggende hardware, besturingssysteem of software. Wij raden aan dat u meerdere exemplaren van een web of de werknemer om ervoor te zorgen de hoge beschikbaarheid van uw toepassing implementeert. Zie voor meer informatie, [Cloud-Services, virtuele Machines Virtual Network Service Level Agreement](http://www.microsoft.com/download/details.aspx?id=38427) en [noodherstel en hoge beschikbaarheid voor Azure toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>U bent verantwoordelijk voor de back-ups van uw eigen gegevens en toepassingen.<br/><br/>Voor databases die woonachtig zijn in een SQL Server-database in een VM Azure, bent u verantwoordelijk voor de uitvoering van een hoge beschikbaarheid en noodherstel recovery oplossing om de uitvaltijd te voorkomen. Zie ondersteunde HDAR technologieën, voor hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines.<br/><br/>**SQL Server Database-Mirroring**: gebruik met Azure Cloud Services (web/werknemer rollen). SQL Server VMs en een cloud service project kunnen zich in hetzelfde virtuele netwerk Azure. Als SQL Server VM zich niet in hetzelfde virtuele netwerk, moet u een SQL Server-Alias om communicatie te routeren naar het exemplaar van SQL Server te maken. Ook de naam van de SQL Server moet overeenkomen met de naam van de alias.|Hoge beschikbaarheid wordt overgenomen van Azure werknemer rollen, Azure blob-opslag en Azure SQL-Database. Azure opslag heeft bijvoorbeeld drie kopieën van alle blob-, tabel- en wachtrijgegevens. Tegelijkertijd een Azure SQL-Database houdt drie kopieën van gegevens uitgevoerd: één primaire replica en twee secundaire replica's. Zie [Opslag Azure](https://azure.microsoft.com/documentation/services/storage/) en [Azure SQL-Database](../sql-database/sql-database-technical-overview.md)voor meer informatie.<br/><br/>Wanneer u SQL Server gebruikt in Azure VM als gegevensbron voor Azure Web Apps, houd er rekening mee dat Azure Web Apps geen Azure Virtual Network ondersteunt. Met andere woorden, moeten alle verbindingen vanuit Azure Web Apps met SQL Server VMs in Azure gaan via openbare eindpunten van virtuele machines. Hierdoor kunnen bepaalde beperkingen voor hoge beschikbaarheid en herstel na storing scenario's. Bijvoorbeeld, zou de clienttoepassing op Azure Web Apps verbinding maken met SQL Server VM met database-mirroring niet kunnen verbinding maken met de nieuwe primaire server als database-mirroring vereist Azure Virtual Network tussen SQL Server host VMs in Azure in te stellen. Daarom wordt gebruik van **SQL Server Database-Mirroring** met Azure Web Apps momenteel niet ondersteund.<br/><br/>**SQL Server AlwaysOn beschikbaarheidsgroepen**: U kunt AlwaysOn beschikbaarheidsgroepen instellen wanneer Azure Web Apps met SQL Server VMs in Azure. Maar u moet AlwaysOn beschikbaarheid groep Listener voor de mededeling van de route naar de primaire replica via openbare taakverdeling eindpunten configureren.|
|**Meerdere lokale connectiviteit**|Azure Virtual Network kunt u verbinding maken met op gebouwen.|Azure Virtual Network kunt u verbinding maken met op gebouwen.|Azure virtueel netwerk wordt ondersteund. Zie [Web Apps virtuele netwerkintegratie](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)voor meer informatie.|
|**Schaalbaarheid**|Scale-up is beschikbaar door het verhogen van de grootte van de virtuele machine of meer schijven toe te voegen. Zie voor meer informatie over de grootte van de virtuele machine, [Virtual Machine formaten voor Azure](virtual-machines-windows-sizes.md).<br/><br/>**Server voor Database**: Scale-out is beschikbaar via de database partitionering technieken en de beschikbaarheid van SQL Server AlwaysOn groepen.<br/><br/>Voor zware werklasten lezen, kunt u [Groepen van AlwaysOn beschikbaarheid](https://msdn.microsoft.com/library/hh510230.aspx) op meerdere knooppunten secundaire als SQL Server-replicatie.<br/><br/>Voor het schrijven van zware werkbelasting, kunt u horizontale partitionering gegevens implementeren op verschillende fysieke servers bieden schalen van toepassing.<br/><br/>Bovendien kunt u een scale-out met behulp van [SQL Server met gegevensafhankelijke routering](https://technet.microsoft.com/library/cc966448.aspx)implementeren. Met gegevens afhankelijke routering (DDR), moet u de partitionering mechanisme in de clienttoepassing, meestal in de business tier laag, de database aanvragen doorsturen naar meerdere SQL Server-knooppunten te implementeren. De business-laag bevat toewijzingen aan hoe de gegevens is gepartitioneerd en welk knooppunt de gegevens bevat.<br/><br/>U kunt schalen van toepassingen die virtuele machines worden uitgevoerd. Zie voor meer informatie [hoe u de schaal van een toepassing](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Belangrijke opmerking**: de functie **automatisch schalen** in Azure kunt u automatisch verhogen of verlagen van de virtuele Machines die worden gebruikt door de toepassing. Deze functie zorgt ervoor dat de eindgebruiker niet negatief wordt beïnvloed tijdens de piekuren en VMs worden afgesloten als de vraag laag is. Het verdient aanbeveling Stel de optie automatisch schalen niet voor uw cloud-service als het VMs van SQL Server bevat. De reden is dat de functie voor automatisch schalen Azure in te schakelen kunt wanneer het CPU-gebruik in de VM die hoger is dan een bepaalde drempel is een virtuele machine en een virtuele machine uitschakelen wanneer het CPU-gebruik lager is dan het gaat. De functie automatisch schalen is handig voor staatloze toepassingen, zoals webservers, waar een VM de werkbelasting zonder verwijzingen naar een eerdere staat kan beheren. De functie automatisch schalen is echter niet geschikt voor stateful toepassingen, zoals SQL Server, waarbij slechts één exemplaar kunt u schrijven naar de database.|Scale-up is met behulp van meerdere rollen in web- en werknemer beschikbaar. Zie voor meer informatie over de grootte van de virtuele machine voor web-rollen en functies van de werknemer, [Formaten voor Cloud Services configureren](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Wanneer u **Cloud-Services**, kunt u meerdere rollen te distribueren van verwerking en bereiken ook flexibele schaling van uw toepassing definiëren. Elke cloud-service omvat een of meer rollen web en/of werknemer-functies, elk met een eigen toepassingsbestanden en -configuratie. U kunt een cloud service scale-up doordat het aantal exemplaren van de rol (virtuele machines) geïmplementeerd voor een rol en schalen naar een cloud-service verlaagt het aantal exemplaren van de rol. Zie voor gedetailleerde informatie [Azure uitvoering modellen](../cloud-services/cloud-services-choose-me.md).<br/><br/>Schaalvergroting is beschikbaar via de ondersteuning van ingebouwde Azure hoge beschikbaarheid door middel van [Cloud-Services, virtuele Machines en Virtual Network Service Level Agreement](http://www.microsoft.com/download/details.aspx?id=38427) en taakverdeling.<br/><br/>Voor een toepassing met meerdere niveaus is het raadzaam dat u verbinding maakt met web/werknemer rollen toepassing naar de database server VMs via Azure Virtual Network. Voorziet bovendien in Azure load balancing voor VMs in dezelfde cloud service aanvragen van gebruikers over deze verspreiding. Virtuele machines aangesloten op deze manier kunnen communiceren met elkaar via het lokale netwerk binnen een datacenter Azure.<br/><br/>U kunt **automatisch schalen** instellen op de klassieke Azure portal en de tijden van de planning. Zie voor meer informatie [hoe u de schaal van een toepassing](../cloud-services/cloud-services-how-to-scale.md).|**Omhoog en omlaag te schalen**: U kunt vergroten of verkleinen de grootte van de sessie (VM) gereserveerd voor uw website.<br/><br/>Geschaald uitbreiden: U kunt meer gereserveerde exemplaren (VMs) voor uw website toevoegen.<br/><br/>U kunt **automatisch schalen** instellen op de portal en de tijden van de planning. Voor meer informatie Zie [schaal Web Apps](../app-service-web/web-sites-scale.md).|

Zie voor meer informatie over het kiezen tussen deze methoden programming [Web Apps in Azure, Cloud Services en VMs: gebruik hiervan](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Volgende stappen

Zie [SQL Server op Azure virtuele Machines-overzicht](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server in Azure Virtual machines.
