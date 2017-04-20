<properties
   pageTitle="Verspreiden van gegevens wereldwijd met DocumentDB | Microsoft Azure"
   description="Meer informatie over de planeet schaal geo-replicatie, failover en gegevens herstellen met globale databases van Azure DocumentDB, een volledig beheerde service voor NoSQL database."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Verspreiden van gegevens wereldwijd met DocumentDB

> [AZURE.NOTE] Wereldwijde distributie van DocumentDB databases in het algemeen beschikbaar is en automatisch ingeschakeld voor alle nieuwe accounts DocumentDB. Wij werken om de wereldwijde distributie van alle bestaande accounts inschakelen, maar in de tussentijd, als u wilt dat de wereldwijde distributie is ingeschakeld op uw account, neem [contact op met support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) en wij gaat instellen voor u nu.

Azure DocumentDB is ontworpen om te voldoen aan de behoeften van IoT toepassingen die bestaat uit miljoenen globaal verdeelde apparaten en internet-toepassingen die uiterst responsieve ervaringen aan gebruikers over de hele wereld leveren. Deze databasesystemen de uitdaging van de verwezenlijking van lage latentie toegang tot toepassingsgegevens uit meerdere geografische regio's met goed gedefinieerde gegevens consistent en beschikbaarheid van garanties wordt geconfronteerd. Als een systeem wereldwijd gedistribueerde database vereenvoudigt DocumentDB de wereldwijde distributie van gegevens met een volledig beheerde, meerdere regio database accounts die duidelijk wisselwerking tussen consistentie, beschikbaarheid en prestaties, alle bijbehorende garanties bieden. DocumentDB database accounts worden aangeboden met hoge beschikbaarheid, ms vertragingstijden van één cijfer, meerdere [niveaus goed gedefinieerde consistentie] [consistency], regionale overname met meerdere homing-API's en de mogelijkheid voor het schalen van doorvoer en opslag elastically over de hele wereld. 

  
## <a name="configuring-multi-region-accounts"></a>Gebied met meerdere accounts configureren

Configureren van een account DocumentDB voor het schalen van over de hele wereld kan worden gedaan in minder dan een minuut via de [portal Azure](documentdb-portal-global-replication.md). Hoeft u het niveau van de juiste consistentie tussen ondersteunde welomschreven consistentie op verschillende niveaus selecteren en een willekeurig aantal Azure regio's koppelen aan de databaseaccount van uw is. DocumentDB consistentie niveaus bieden duidelijk wisselwerking tussen garantie specifiek consistentie en prestaties. 

![DocumentDB biedt verschillende, duidelijk zijn gedefinieerd (beperkte) consistentie modellen om uit te kiezen][1]

DocumentDB biedt meerdere modellen om uit te kiezen (beperkte) consistentie goed gedefinieerd.

Selecteren van het niveau van de juiste consistentie afhankelijk zijn van gegevens consistentie waarborgen uw applicaties. DocumentDB automatisch uw gegevens gerepliceerd over alle vermelde regio's en waarborgt de consistentie die u hebt geselecteerd voor uw databaseaccount. 


## <a name="using-multi-region-failover"></a>Met behulp van meerdere regio failover 

Azure DocumentDB is transparant failover-database accounts kunnen over meerdere Azure regio's – de nieuwe [multi-homing API's] [ developingwithmultipleregions] garantie dat uw app kunt blijven gebruiken een logisch eindpunt en ononderbroken door de failover-functie. Failover wordt beheerd door u, mits de rekening van de flexibiliteit om uw database onderbrengen in het geval van het bereik van mogelijke storingen optreden, met inbegrip van de toepassing, infrastructuur, service of regionale storingen (echte of gesimuleerde). Bij een storing van de DocumentDB-regionale transparant de service niet via de databaseaccount van uw en uw toepassing nog steeds toegang tot de gegevens zonder verlies van beschikbaarheid. Terwijl DocumentDB [99,99% beschikbaarheid Sla's biedt][sla], kunt u einde aan eigenschappen van beschikbaarheid van uw toepassing testen door beide [via programmering] een regionale storing simuleren[ arm] maar ook via de Portal Azure.


## <a name="scaling-across-the-planet"></a>Over de planeet schalen
DocumentDB kunt u onafhankelijk van elkaar inrichten doorvoer en opslag voor elke collectie DocumentDB op een schaal, globaal verbruiken in alle regio's die zijn gekoppeld aan de databaseaccount van uw. Een collectie DocumentDB wordt automatisch wereldwijd gedistribueerd en beheerd in alle regio's die zijn gekoppeld aan de databaseaccount van uw. Collecties in de database-account kunnen worden verdeeld over een van de Azure regio's waarin de [DocumentDB-service is beschikbaar][serviceregions]. 

De doorvoer gekocht en opslag voor elke collectie DocumentDB verbruikt is automatisch ingericht in alle regio's verdeeld. Hierdoor kan de toepassing voor het schalen van over de hele wereld [betaalt alleen voor de doorvoer en opslag die u in elk uur gebruikt]naadloos[pricing]. Bijvoorbeeld als het inrichten van de RUs 2 miljoen voor een verzameling DocumentDB haalt vervolgens elk van de regio's die zijn gekoppeld aan uw databaseaccount RUs 2 miljoen voor die collectie. Dit wordt hierna geïllustreerd.

![Schaal doorvoer voor een verzameling DocumentDB tussen de vier regio 's][2]

DocumentDB < 10 ms lezen en schrijven vertragingstijden bij P99 van < 15 ms wordt gewaarborgd. Nooit span datacenter grens te waarborgen van de [consistentie-vereisten die u hebt geselecteerd]de leesbevestiging aanvragen[consistency]. De schrijfbewerkingen zijn altijd quorum lokaal gepleegd voordat zij worden bevestigd aan de clients. Elke databaseaccount is geconfigureerd met schrijven regio prioriteit. De regio met de hoogste prioriteit aangewezen zal fungeren als het huidige gebied schrijven voor de account. Alle SDK's doorsturen transparant rekening schrijven naar een database naar het huidige gebied voor schrijven. 

Ten slotte, omdat DocumentDB volledig [schema agnostic is] [ vldb] -u hebt nooit zorgen te maken over schema's beheren/bijwerken of secundaire over meerdere datacenters. [SQL-query's] [ sqlqueries] blijven werken terwijl uw toepassing en gegevens modellen blijven evolueren. 


## <a name="enabling-global-distribution"></a>Wereldwijde distributie inschakelen 

U kunt uw gegevens lokaal of globaal gedistribueerd door beide koppelen database een of meer Azure regio's met een DocumentDB-account. U kunt toevoegen of verwijderen van de regio's aan uw databaseaccount op elk gewenst moment. Zie voor meer informatie over het inschakelen van wereldwijde distributie via de portal [DocumentDB globale databasereplicatie met de Azure portal uitvoeren](documentdb-portal-global-replication.md). Zie [Developing with DocumentDB-rekeningen meerdere regio](documentdb-developing-with-multiple-regions.md)wereldwijde distributie via programmacode inschakelen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de distributie gegevens wereldwijd met DocumentDB in de volgende artikelen:

* [Doorvoer en opslag voor een collectie inrichten] [throughputandstorage]
* [Met meerdere homing API's via REST. .NET, Java, Python en knooppunt SDK 's] [developingwithmultipleregions]
* [Niveaus van consistentie in DocumentDB] [consistency]
* [Beschikbaarheid van SLA 's] [sla]
* [Database-account beheren] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

