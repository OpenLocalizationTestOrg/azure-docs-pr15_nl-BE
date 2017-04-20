Data factory is een huurder met meerdere service die beschikt over de standaardlimieten voor het volgende om ervoor te zorgen dat klanten abonnementen zijn beschermd tegen elkaars werkbelasting. Veel van de grenzen kunnen worden eenvoudig verhoogd tot aan de maximale limiet voor ondersteuning contact opnemen met. 

**Resource** | **Standaardlimiet** | **Maximum aantal**
-------- | ------------- | -------------
fabrieken van gegevens in een Azure-abonnement | 50 | [Neem contact op met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
pijpleidingen binnen een fabriek gegevens | 2500 | [Neem contact op met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
DataSets binnen een fabriek gegevens | 5000 | [Neem contact op met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
gelijktijdige segmenten per gegevensset | 10 | 10
bytes per object voor pijpleiding objecten <sup>1</sup> | 200 KB | 2000 KB
bytes per object voor dataset en service gekoppelde objecten <sup>1</sup> | 100 KB | 2000 KB
HDInsight op verzoek cluster cores binnen een abonnement <sup>2</sup> | 48 | [Neem contact op met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Cloud data verkeer unit <sup>3</sup> | 8 | [Neem contact op met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Probeer opnieuw te tellen voor pijpleiding activiteit worden uitgevoerd | 1000 | MaxInt (32-bits)

<sup>1</sup> pijpleiding dataset en -gekoppelde objecten vertegenwoordigen een logische groepering van uw werkbelasting. Limieten voor deze objecten geen betrekking hebben op de hoeveelheid gegevens die u kunt verplaatsen en verwerken met de Azure Data Factory-service. Data factory is ontworpen om geschaald petabytes van gegevens verwerken.

<sup>2</sup> HDInsight on-demand cores worden toegewezen uit het abonnement met de data factory genoemd. Hierdoor is de bovengenoemde limiet van de Data Factory core limiet voor bellen op HDInsight cores afgedwongen en verschilt van de core limiet die is gekoppeld aan uw abonnement op Azure.

<sup>3</sup> wolk verplaatsing gegevenseenheid (DMU) wordt gebruikt in een wolk naar wolk kopieerbewerking. Het is een maatregel die de kracht (een combinatie van CPU, geheugen en netwerk brontoewijzing) van één eenheid in Data Factory vertegenwoordigt. Hogere doorvoer van de kopie kunt u bereiken door gebruik te maken van meer DMUs voor bepaalde scenario's. Raadpleeg de sectie [wolk eenheden verkeer](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) op details.

**Resource** | **Standaard ondergrens** | **Ondergrens**
-------- | ------------------- | -------------
Interval planning | 15 minuten | 15 minuten
Interval tussen nieuwe pogingen | 1 seconde | 1 seconde
Time-outwaarde opnieuw | 1 seconde | 1 seconde


### <a name="web-service-call-limits"></a>Web service call limieten

Azure Resource Manager heeft beperkingen voor de API-aanroepen. Met een snelheid binnen de [limieten van Azure Resource Manager API](../azure-subscription-service-limits.md#resource-group-limits)kunt u API-aanroepen. 


