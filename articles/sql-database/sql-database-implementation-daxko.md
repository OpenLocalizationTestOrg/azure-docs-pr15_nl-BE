<properties
   pageTitle="Azure SQL Database Azure casestudy - Daxko/CSI | Microsoft Azure"
   description="Meer informatie over hoe Daxko/CSI SQL-Database gebruikt de ontwikkelingscyclus en en de klantenservice en de prestaties verbeteren"
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
   ms.date="09/08/2016"
   ms.author="carlrab"/>
   
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI Azure gebruikt de ontwikkelingscyclus en en de klantenservice en de prestaties verbeteren

![Daxko/CSI-Logo](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI-Software een uitdaging geconfronteerd: het klantenbestand van geschiktheid en recreatie centers is groeien snel, dankzij het succes van de uitgebreide bedrijfssoftware oplossing, maar houden met de behoeften van de IT-infrastructuur voor die klant groeiende basis van het bedrijf is testen IT-personeel. Het bedrijf is steeds beperkt door toenemende overhead bewerkingen, met name voor het beheer van de groeiende databases. Erger nog, is dat overhead bewerkingen knippen in de ontwikkeling van middelen voor nieuwe initiatieven, zoals nieuwe functies voor mobiliteit voor de software van het bedrijf.

Volgens David Molina, directeur van Product Development bij Daxko/CSI, voorzien Azure van CSI Software het platform as a service (PaaS)-model dat moest worden Vereenvoudig het databasebeheer van de, verhoogt de schaalbaarheid en bronnen te concentreren op software in plaats van ops vrij te maken. "Azure SQL-Database is een geweldige optie voor ons. Geen zorgen te maken over het onderhoud van een SQL-Server, een failover-cluster en alle andere de infrastructuur moet met is ideaal voor ons."

CSI-Software moet na de migratie naar Azure, een operationele personeel van slechts twee klantendatabases met meer dan 600 beheren. Het bedrijf elastische pools Azure SQL-Database wordt verplaatst van de klantendatabases op basis van grootte en nodig hebt.

Molina blijft, "onze klanten vilt de wijziging onmiddellijk. Voor groepen van elastisch hadden soms ze time-outs en andere problemen tijdens perioden van burst. Met Azure elastische toepassingen, kunnen desgewenst burst en gebruik van de software zonder problemen."

Naast het verbeteren van de prestaties voor klanten, Azure elastische database vrijgemaakt CSI Software middelen te concentreren op het ontwikkelen van nieuwe services en functies, in plaats van omgaan met de uitvoering en beheer van toepassingen. Die IT-resources hielp CSI Software enterprise software aanbieden, SpectrumNG, om te oefenen sportfaciliteiten leden, medewerkers efficiënter, verbeteren en personeel en leden mobiele toegang geven voor interactieve taken en real-time-meldingen.

Azure heeft ook bijgedragen Software CSI versnellen en de ontwikkeling en kwaliteitscontrole (QA) cyclus verbeteren door opties voor automatisering. Met Azure-implementatie van het bedrijf kunnen samenstellen managers pakket onderdelen met het klikken op een knop. Als beschrijving van Molina, "als onderdeel van de release-cyclus, QA is nu kunt implementeren op een testomgeving in Azure, die komt sterk onze productie-stack overeen. We kunt builds onmiddellijk op onze dev omgeving te vet wijzigingen implementeren. Dat is een groot voordeel voor ons, omdat de pariteit voor testen voordat die we hebben."

## <a name="offloading-to-the-cloud"></a>Naar de cloud-offloading

Voordat u naar de cloud, had CSI Software is opgebouwd haar eigen multitenant-infrastructuur in een lokale datacenter in Houston. Het bedrijf is uitgevouwen, het toenemende groeipijn uit kopen, inrichten en onderhouden van alle hardware en software die nodig is ter ondersteuning van haar klanten te maken. IT werd een ander knelpunt die heeft geleid tot een vertraging van de nieuwe resources inrichten en implementeren van nieuwe services aan klanten van personeel voor het verwerken van transacties.

CSI-Software hebt bekeken in de cloud opties voor het elimineren van deze overhead, zodat deze zich op de code, in plaats van haar activiteiten concentreren kan. Het bedrijf ontdekte dat veel van de bovenste cloud providers alleen infrastructure as a service (IaaS) oplossingen bieden die nog een grote IT-personeel moeten voor het beheren van de stack IaaS. Uiteindelijk bepaald CSI Software dat de oplossing PaaS Azure aangepast aan de behoeften is. Molina wordt uitgelegd, "Azure haalt de hardware en software uit de weg, zodat we kunnen richten op onze software-oplossingen, onze IT-overhead te verlagen."

## <a name="making-the-transition-to-azure"></a>Waardoor de overgang naar Azure

Na het selecteren van Azure voor de PaaS-oplossing, CSI Software begon met de back-end infrastructuur en databases migreren naar de cloud. Voor Azure, SpectrumNG klanten die nodig zijn voor een clienttoepassing die uitgewisseld installeren met een Windows Communication Foundation (WCF)-service op de back-end. Volgens Molina, "Hoewel sommige klanten die alles in hun eigen datacenters, we gebouwd om het product te zijn multitenant. We gehost alles in een datacenter in Houston, SQL Server gebruikt als gegevensarchief.

"Ons product biedt ook opgenomen een lidstaat gerichte webportal (geschreven in ASP.net), dat is ontworpen om te worden wit-met de naam aan van de klant op het web presenteren en een SOAP-API ter ondersteuning van de online pagina's en de integratie van derden."

De migratie naar de cloud heeft niet lang duren voor de architectuur. Volgens Molina, "De meeste van de inspanningen die zijn behandeld bij het wijzigen van de manier waarop we config bestandsinformatie, een wijziging gecentraliseerde verbindingsreeks en automatiseren van de verpakking, uploaden en implementatie van onze releases lezen."

Voor het ontwikkelen van de build-automatisering gebruikt CSI Software engineers Azure PowerShell en REST API's pakketten maken en te uploaden naar een testomgeving voor elke avond.
De algemene overgang naar een Azure cloud-gebaseerde implementatie ging snel en vlot voor Software-het CSI team. Molina wordt uitgelegd, "In alle, hebben we een beta-omgeving in de cloud binnen drie tot vier weken voor het project te nemen. Dat was een verrassend win voor ons."

Na het configureren en testen van het milieu, CSI Software begonnen migreren klanten. Voor klanten die al CSI Software die als host fungeert, is bijna naadloos. Voor klanten die migreren van een implementatie van gebouwen, de migratie naar de cloud wat extra tijd heeft genomen, maar is nog steeds voornamelijk pijn vrij voor zowel klanten als CSI-Software.

Voor nieuwe klanten, CSI-Software van IT-medewerkers gebruiken het volgende proces om aan boord naar Azure:

1.  Azure PowerShell-scripts worden gebruikt voor het draaien van een nieuwe database voor de klant. alle klanten beginnen op een laag premium om ervoor te zorgen voldoende eerste doorvoer voor de overgang van.
2.  Indien mogelijk, CSI Software maakt gebruik van de Migration Wizard Azure SQL bestaande gegevens verplaatsen naar een Azure SQL-Database-instantie.
3.  Ten slotte gebruikt Microsoft SQL Server Integration Services (SSIS) afstemmen van eventuele verschillen in de gegevens of alle gegevens wissen zoals vereist.

Vandaag de dag worden ongeveer 99% van de klanten van CSI Software gehost in Azure, over vier regionale datacenters (centrale Noord, Zuid-centraal, Oost en West). Latentie is doordat de datacenters in de geografische regio van de klant tot een minimum beperkt.

## <a name="azure-elastic-database-pools-free-up-it-resources"></a>Azure elastische database toepassingen vrijmaken IT-middelen

Verscheidene functies van Azure CSI Software hebben geholpen verschuiving van infrastructuur en activiteiten gericht op functie en ontwikkeling is gericht. Misschien is het grootste voordeel van elastische database van toepassingen.

CSI-Software biedt op dit moment ongeveer 550 databases voor klanten. Voor groepen van elastisch is het moeilijk te beheren dat veel databases binnen een structuur laag. OPS managers moest toewijzen van de niveaus van prestaties op basis van de burst-behoeften van klanten, die belangrijke IT-resource overhead vereist. Met elastische database-toepassingen managers toewijzen huurders een premium of standard pool, voorzover van toepassing, en vervolgens verplaatsen van klanten op basis van grootte en nodig. Klanten ervaren de gevolgen van de elastische database-toepassingen vrijwel onmiddellijk; voordat elastische groepen klanten time-outs en andere problemen waren tijdens de burst-perioden, maar met elastische groepen klanten bursts activiteit indien nodig kunnen ondervinden en kunnen ze doorgaan met het SpectrumNG zonder problemen gebruiken.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure Active Geo-replicatie versnelt melden

Diverse Software CSI klanten ook profiteert van Azure Active Geo-replicatie. Met actieve Geo-replicatie, kunnen tot vier leesbaar secundaire databases worden geconfigureerd in het datacenter van dezelfde of een andere regio's. CSI-Software maakt gebruik van actieve Geo-replicatie op twee manieren: ten eerste de secundaire databases zijn beschikbaar in het geval van een storing in een datacenter of problemen met de verbinding met de primaire database; en ten tweede, de secundaire databases kunnen worden gelezen en kunnen worden gebruikt voor offload van alleen-lezen werkbelasting zoals taken melden. Sommige klanten CSI Software gebruiken deze vergoeding rapportage werkstromen te versnellen.

## <a name="csi-software-application-logic-and-architecture"></a>CSI Software toepassingslogica en architectuur

SpectrumNG maakt gebruik van web-rollen. Omdat de toepassing meerdere pachter is, wordt een WCF-service gebruikt voor het verwerken van de oorspronkelijke verbindingsaanvraag door klanten. Als Molina staat, geeft"de aanvraag elke klant, die vervolgens kan we een verbindingsreeks die tot hun databases te doen wat we moeten doen."

Voor de weblaag van de service maakt CSI-Software gebruik van Azure automatisch schalen op basis van de dag en tijd. Beschikbare bronnen worden automatisch verhoogd ten behoeve van hogere verbruik tijdens de kantooruren, afhankelijk van de tijdzone van elke regionale datacenter. Bronnen zijn ook in te perken in het weekend, wanneer de behoeften van de klant lager zijn ingesteld.

     
![Daxko/CSI-architectuur](./media/sql-database-implementation-daxko/figure1.png)

Figuur 1. Gestructureerde gegevens haalt een cloud services-rol werknemer uit Azure SQL-Database en semi-gestructureerde gegevens uit de tabelopslag. SpectrumNG gebruikers communiceren met die gegevens via een cloud functie web services.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Met behulp van web apps en een laag web plan voor mobiele toepassingen

Met behulp van Azure SQL-Database CSI Software bronnen vrijgemaakt om nieuwe initiatieven, met inbegrip van een volledige mobiele platform op basis van een aangepaste API gehost in Azure web apps. Het platform kan leden van sportfaciliteiten en het personeel met behulp van mobiele apparaten controleren van schema's, klassen van het adresboek en ontvangen van berichten.

Het platform maakt gebruik van servicegeoriënteerde architectuur (SOA) te nemen van één onderdeel, zoals een verkooppunt (POS) of een verkoop — onderweg verplaatsen naar een ander web plan en de film van een service voor de ondersteuning van dat onderdeel, terwijl al het andere op het oorspronkelijke plan van de website. Deze mogelijkheid biedt enorme flexibiliteit CSI-Software, en het houdt kosten omlaag.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure kunt CSI Software ontwikkelaars zich richten op apps en services

Azure SQL-Database is niet alleen een zegen voor SpectrumNG klanten, die profiteren van de snelle, betrouwbare service, maar ook een groot voordeel voor CSI-Software IT-medewerkers en ontwikkelaars. Ops naar Azure in de cloud-offloading, CSI Software beperkt de overhead voor resources en infrastructuur, sterk versnelde de ontwikkelingscycli en niet meer behoeften micromanage databases optimaliseren voor de huurders.

## <a name="more-information"></a>Meer informatie

- Zie voor meer informatie over groepen van Azure elastische database, [elastische database van toepassingen](sql-database-elastic-pool.md).

- Zie voor meer informatie over hulpmiddelen voor databases en elastische schalen, [elastische Databasehulpmiddelen en elastische schalen](sql-database-elastic-scale-get-started.md).

- Zie voor meer informatie over het migreren van SQL Server-database, [Azure SQL Migration Wizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md).

- Zie voor meer informatie over actieve Geo-replicatie, [Actieve Geo-replicatie](sql-database-geo-replication-overview.md).

- Zie voor meer informatie over Web-rollen en werknemer rollen, [rollen van de werknemer](../fundamentals-introduction-to-azure.md#compute). 

- Zie voor meer informatie over Azure Service Bus, [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- Zie voor meer informatie over het automatisch schalen, de [schaal cloud services](../cloud-services/cloud-services-how-to-scale.md).
