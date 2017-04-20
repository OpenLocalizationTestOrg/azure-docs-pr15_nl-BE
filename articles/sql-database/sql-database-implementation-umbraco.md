<properties
   pageTitle="Azure SQL Database Azure casestudy - Umbraco | Microsoft Azure"
   description="Meer informatie over het gebruik van SQL-Database snel inrichten in Umbraco en schaal diensten voor duizenden huurders in de cloud"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco Azure SQL-Database gebruikt snel levering en schaal diensten voor duizenden huurders in de cloud

![Umbraco-Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco is een populaire open source inhoud management systeem (CMS) geven uit kleine campagne of brochure sites complexe toepassingen voor Fortune 500-bedrijven en algemene media websites. 

> "We hebben vrij een grote Gemeenschap van ontwikkelaars die gebruikmaken van het systeem, met meer dan 100.000 ontwikkelaars op onze forums en meer dan 350.000 websites die zijn levende, Umbraco uitgevoerd."

> Umbraco Morten Christensen, technische specialist

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Om te vereenvoudigen implementaties bij klanten, Umbraco Umbraco-as-a-Service (UaaS) toegevoegd: een software-as-a-service (SaaS) aanbieden zodat het niet nodig voor installaties voor gebouwen, biedt ingebouwde schaling en beheer overhead worden verwijderd doordat ontwikkelaars zich richten op innovatie in plaats van een oplossing van productbeheer. Umbraco is, kunnen alle vergoedingen die door het vertrouwen op het flexibele platform as a service (PaaS)-model aangeboden door Microsoft Azure.

UaaS kunnen SaaS-klanten gebruiken Umbraco CMS mogelijkheden die eerder buiten hun bereik zijn. Deze klanten zijn voorzien van een CMS werkomgeving waarin een productiedatabase. Klanten kunnen maximaal twee extra databases voor ontwikkel- en testomgevingen, afhankelijk van hun eisen toevoegen. Bij een aanvraag voor een nieuwe omgeving is een geautomatiseerd proces die klant een database automatisch toegewezen. De nieuwe database is klaar in seconden, omdat de database al vooraf door Umbraco uit een Azure elastische groep met beschikbare databases ingericht is (Zie figuur 1).


![Umbraco provisioning lifecycle](./media/sql-database-implementation-umbraco/figure1.png)

Figuur 1. Lifecycle voor Umbraco ingericht als een Service (UaaS)
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure elastische pools en automatisering Vereenvoudig implementaties

Azure SQL-Database en andere Azure diensten, Umbraco klanten kunnen zelf hun omgeving inrichten en Umbraco eenvoudig kunt bewaken en beheren van databases als onderdeel van een intuïtieve workflow:

1.  Bepaling

    Umbraco houdt een maximale capaciteit van 200 vooraf ingerichte beschikbare databases van elastisch van toepassingen. Als een nieuwe klant zich voor UaaS aanmeldt, biedt Umbraco de klant met een nieuwe CMS-omgeving in bijna real-time met een database van de beschikbaarheid van toepassingen toewijst.

    Een beschikbaarheid van toepassingen de drempel bereikt, wordt een nieuwe groep met elastische gemaakt als nieuwe databases vooraf worden toegewezen aan klanten naar wens ingericht worden.

    Implementatie is volledig geautomatiseerd met behulp van C# management bibliotheken en wachtrijen Azure Service Bus.

2.  Gebruik

    Klanten één tot drie-omgevingen (productie, staging en/of ontwikkeling), elk met een eigen database gebruiken. Klantendatabases zijn in elastische database toepassingen waarmee Umbraco om efficiënte schalen zonder dat u te weinig inrichten.

    ![Umbraco project-overzicht](./media/sql-database-implementation-umbraco/figure2.png)

    ![Umbraco projectdetails](./media/sql-database-implementation-umbraco/figure3.png)

    Figuur 2. Website van Umbraco-as-a-Service (UaaS) klant, projectoverzicht en details weergeven

    Database-transactie-eenheden (DTUs) Azure SQL-Database gebruikt om aan te geven van de relatieve kracht nodig is voor werkelijke databasetransacties. Databases werken meestal met ongeveer 10 DTUs voor UaaS klanten, maar elk heeft de elasticiteit te schalen op aanvraag. Dit betekent dat UaaS zorgt ervoor dat klanten altijd benodigde bronnen zelfs tijdens piekuren hebben. Bijvoorbeeld tijdens een recente zondagavond sportevenement, een UaaS ervaren klantendatabase waar de pieken tot 100 DTUs voor de duur van het spel. Azure elastische pools gesteld Umbraco ter ondersteuning van die hoge vraag zonder vertragingen.

3.  Monitor

    Umbraco monitoren database met behulp van dashboards binnen de Azure portal, samen met de aangepaste e-mailwaarschuwingen activiteit.

4.  Noodherstel

    Azure biedt twee opties voor noodherstel (DR): actieve Geo-replicatie en Geo terugzetten. De DR-optie die een bedrijf moet selecteren is afhankelijk van de [doelstellingen van de business continuity](sql-database-business-continuity.md).

    Actieve Geo-replicatie biedt de snelste reactie in geval van uitval. Actieve Geo-replicatie kunt u secundaire servers voor maximaal vier leesbaar maken op servers in verschillende regio's en u kunt vervolgens failover naar een van de secundaire servers starten bij een storing.

    Umbraco Geo-replicatie niet vereist, maar deze er voordeel van Azure Geo-terugzetten om ervoor te zorgen dat minimale uitvaltijd in geval van een stroomstoring. Geo-terugzetten is afhankelijk van back-ups in geo-redundante opslag van Azure. Waarmee gebruikers terugzetten vanaf een back-up wanneer er een storing in de primaire regio.

5.  De voorziening

    Wanneer een project-omgeving wordt verwijderd, worden alle gekoppelde databases (ontwikkeling, gefaseerde installatie of live) verwijderd tijdens Azure Service Bus wachtrij opschonen. Dit geautomatiseerde proces herstelt u de niet-gebruikte databases van Umbraco elastische database-beschikbaarheid van toepassingen, zodat ze beschikbaar zijn voor de toekomstige inrichting van behoud van de maximale capaciteit.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Elastische groepen kunt u UaaS wilt schalen met gemak

Door gebruik te maken van groepen van Azure elastische database, kunt Umbraco optimaliseren voor klanten zonder te boven of onder bepaling. Umbraco heeft momenteel bijna 3.000 databases in 19 elastische database van toepassingen kunnen eenvoudig worden aangepast aan die nodig zijn voor een van hun bestaande 325,000 klanten of nieuwe klanten die klaar zijn voor het implementeren van een CMS in de cloud.

In feite volgens Morten Christensen, technische leiden in Umbraco, "UaaS is nu met de groei van ongeveer 30 nieuwe klanten per dag. Onze klanten zijn zeer tevreden over het gemak voor het creëren van nieuwe projecten in seconden, direct updates publiceren hun live-sites van een ontwikkelomgeving met één klik-implementatie en breng wijzigingen net zo snel als ze fouten vinden. "

Als een klant een tweede en/of derde omgeving niet meer nodig, deze hoeft te verwijderen deze omgevingen. Die middelen die kunnen worden gebruikt voor andere klanten als onderdeel van de groep Umbraco elastisch-beschikbaarheid van de databases hebt vrijgemaakt.

![Umbraco implementatie-architectuur](./media/sql-database-implementation-umbraco/figure4.png)

In figuur 3. Implementatie-architectuur op Microsoft Azure UaaS

##<a name="the-path-from-datacenter-to-cloud"></a>Het pad van het datacenter naar cloud

Wanneer de ontwikkelaars van Umbraco in eerste instantie de beschikking verplaatsen naar een SaaS-model, wisten zij dat zij moet een kosteneffectieve en schaalbare manier om te bouwen van de service.

> "Elastische database toepassingen zijn een bij uitstek geschikt voor onze SaaS biedt omdat we capaciteit omhoog en omlaag zo nodig kunt bellen. Inrichten is eenvoudig en met onze instelling, we gebruik maximaal kunt houden."

> Umbraco Morten Christensen, technische specialist

"We wilden onze tijd besteden aan het oplossen van problemen voor onze klanten, niet het beheer van infrastructuur. We wilden voor onze klanten om de meeste waarde gemakkelijk", zegt Niels Hartvig, oprichter van Umbraco. "We aanvankelijk overwogen die als host fungeert voor de servers onszelf maar capaciteitsplanning zou zijn een nachtmerrie." Coincidentally, Umbraco geen beveiligingsvoorziening is geïmplementeerd de databasebeheerders die een toegevoegde waarde voor het gebruik van UaaS onderstrepingstekens.

Een belangrijk doel voor de ontwikkelaars van Umbraco is op een manier voor klanten UaaS omgevingen inrichten, snel en zonder beperkingen van de capaciteit. Maar bieden een speciale gehoste service in Umbraco datacentra moest veel overcapaciteit voor het verwerken van bursts in verwerking. Dat zou hebben bedoeld aanzienlijke compute-infrastructuur zou hebben is regelmatig volledig toe te voegen.

Het ontwikkelteam Umbraco wilde bovendien een oplossing die kunnen zou worden zo veel mogelijk van hun bestaande code mogelijk opnieuw te gebruiken. Als ontwikkelaar van Umbraco Mikkel Madsen wordt aangegeven, "We zijn blij met de Microsoft-ontwikkelprogramma's die we al bekend zijn met, zoals Microsoft SQL Server, Microsoft Azure SQL-Database, ASP.net en Internet informatieservices (IIS waren). Voordat u investeert in een IaaS of een PaaS cloud oplossing, we wilden om ervoor te zorgen dat zou ondersteund onze Microsoft-hulpprogramma's en -platforms, zodat we niet hoeft te enorme wijzigingen aanbrengen in onze code base. "

Umbraco gezocht om te voldoen aan de criteria, cloud partner van de volgende situaties:

-   Voldoende capaciteit en betrouwbaarheid
-   Ondersteuning voor Microsoft-ontwikkelprogramma's, zodat technici Umbraco zou niet worden gedwongen te doelen volledig hun ontwikkelomgeving
-   Aanwezigheid in alle geografische markten waarin UaaS (bedrijven nodig hebben concurreert om ervoor te zorgen dat ze toegang hun gegevens snel tot en dat hun gegevens worden opgeslagen op een locatie die voldoet aan hun regionale regelgevende vereisten)

##<a name="why-umbraco-chose-azure-for-uaas"></a>Waarom Umbraco Azure kiest voor UaaS

Op basis van Morten Christensen "na afweging van alle opties voor ons, we geselecteerd Azure omdat deze is voldaan aan alle onze criteria, beheerbaarheid en schaalbaarheid voor vertrouwdheid en kosteneffectiviteit. We stellen de Azure VMs-omgevingen en elke omgeving heeft zijn eigen Azure SQL-Database-instantie met de exemplaren in elastische database-toepassingen. Door databases tussen ontwikkeling, gefaseerde installatie en productieomgevingen worden gescheiden, bieden wij onze klanten robuuste prestaties isolatie afgestemd op schaal, een enorme win. "

Morten blijft, "voordat we servers voor Webdatabases handmatig te creëren moesten. We hebben nu geen denken. Alles is geautomatiseerd, van het inrichten van opruimen. "

Morten is ook tevreden met de schaal mogelijkheden van Azure. "Elastische database toepassingen zijn een bij uitstek geschikt voor onze SaaS biedt omdat we capaciteit omhoog en omlaag zo nodig kunt bellen. Inrichten is eenvoudig en met onze instelling, we gebruik maximaal kunt houden." Morten Staten, "de eenvoud van elastisch van toepassingen, samen met de zekerheid van service tier-gebaseerde DTUs geeft ons de kracht om te creëren nieuwe resourcegroepen op aanvraag. Onlangs een van onze grotere klanten een piek naar 100 DTUs in de productieomgeving. Met Azure, onze elastische pools databases met de middelen die zij nodig in real-time zonder te voorspellen DTU eisen van de klant geleverd. Kortom, onze klanten krijgen de turn-around-tijd die ze verwachten en kunnen we onze service level agreements van prestaties voldoen."

Mikkel Madsen dit opgeteld: "We helemaal bent gewonnen voor de krachtige Azure algoritme dat SaaS vaak (onboarding nieuwe klanten in real time op schaal) verbindt met onze toepassing patroon (databases, beide ontwikkeling vooraf worden ingericht en live) op de onderliggende technologie (met Azure Service Bus wachtrijen in combinatie met Azure SQL-Database)."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Met Azure, UaaS meer dan verwachtingen van klanten

Sinds Azure als de cloud partner te kiezen, is Umbraco, kunnen klanten UaaS met geoptimaliseerde inhoud management prestaties zonder de vereiste van een oplossing voor zelfgehoste IT resource investering. Morten zegt: "We graag de ontwikkelaars gemakkelijker te maken en schaalbaarheid die Azure ons geeft en onze klanten zijn geweldig met de functies en betrouwbaarheid. Over het algemeen is een geweldig voordeel voor ons!"
 
## <a name="more-information"></a>Meer informatie

- Zie voor meer informatie over groepen van Azure elastische database, [elastische database van toepassingen](sql-database-elastic-pool.md).

- Zie voor meer informatie over Azure Service Bus, [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- Zie voor meer informatie over Web-rollen en werknemer rollen, [rollen van de werknemer](../fundamentals-introduction-to-azure.md#compute). 

- Zie voor meer informatie over virtuele netwerken, [virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Zie voor meer informatie over back-up en herstel, [bedrijfscontinuïteit](sql-database-business-continuity.md).  

- Zie voor meer informatie over het controleren van ppols, de [controle van toepassingen](sql-database-elastic-pool-manage-portal.md). 

- Zie voor meer informatie over Umbraco als een Service, [Umbraco](https://umbraco.com/cloud).

