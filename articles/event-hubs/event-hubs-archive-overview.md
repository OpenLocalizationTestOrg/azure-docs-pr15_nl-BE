<properties
    pageTitle="Azure gebeurtenis Hubs archief | Microsoft Azure"
    description="Overzicht van de functie Azure gebeurtenis Hubs archief."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Azure gebeurtenis Hubs archief

Azure gebeurtenis Hubs archief kunt u automatisch de streaming gegevens leveren in de gebeurtenis Hubs naar een Blob-opslag-account van uw keuze met extra flexibiliteit om een tijd opgeven of het formaat van het interval van uw keuze. Het instellen van het archief is snel, zijn er geen administratieve kosten uit te voeren en wordt automatisch aangepast aan uw gebeurtenis Hubs [doorvoer eenheden](event-hubs-overview.md#capacity-and-security). Gebeurtenis Hubs archief is de eenvoudigste manier om de streaming-gegevens laden in Azure en kunt u zich richten op gegevens verwerken in plaats van het opnemen van gegevens.

Azure gebeurtenis Hubs archief kunt u real-time en op basis van batch pijpleidingen op dezelfde gegevensstroom te verwerken. Hiermee kunt u oplossingen ontwerpen die met uw behoeften na verloop van tijd meegroeien kunnen. Of u bij het bouwen van batch-systemen met het oog op toekomstige real-time verwerking vandaag of u wilt dat een efficiënte koude pad toevoegen aan een bestaande oplossing voor real-time, maakt gebeurtenis Hubs archief het werken met streaming gegevens gemakkelijker.

## <a name="how-event-hubs-archive-works"></a>De werking van de gebeurtenis Hubs archief

Gebeurtenis Hubs is een duurzame buffer tijd bewaren voor telemetrie ingress, vergelijkbaar met een gedistribueerde logboek. De sleutel te schalen in de gebeurtenis Hubs is het [model van gepartitioneerde consument](event-hubs-overview.md#partition-key). Elke partitie een gegevenssegment onafhankelijk is en onafhankelijk wordt verbruikt. Na verloop van tijd veroudert deze gegevens, op basis van de aanhoudperiode worden geconfigureerd. Als gevolg hiervan krijgt een bepaalde gebeurtenis Hub nooit "te vol is."

Gebeurtenis Hubs archief kunt u uw eigen account Azure Blob-opslag en de Container die worden gebruikt voor het opslaan van de gearchiveerde gegevens op te geven. Deze account kan worden in een ander gebied of in dezelfde regio als Hub voor de gebeurtenis toe te voegen aan de flexibiliteit van de functie voor het archiveren van gebeurtenis-Hubs.

Gearchiveerde gegevens worden geschreven in [Apache Avro][] -indeling. een compact, snel, binaire indeling waarmee uitgebreide data structuren met in line schema. Deze indeling wordt veel gebruikt in het Hadoop-ecosysteem en Stream Analytics en Azure Data Factory. Verderop in dit artikel vindt u meer informatie over het werken met de Avro.

### <a name="archive-windowing"></a>Archief Windowing

Gebeurtenis Hubs te archiveren, kunt u een venster instellen om te bepalen archiveren. Dit venster is een minimale grootte en configuratie van de tijd met een 'eerste WINS-beleid', wat betekent dat de eerste trigger is opgetreden, een archiveringsbewerking wordt. Als u een 15-minuut/100 MB archief venster en 1 MB/s verzenden, wordt het venster grootte voordat het venster geactiveerd. Elke partitie afzonderlijk worden gearchiveerd en schrijft een blob voltooide blok op het moment van archief, met de naam van het moment wanneer het interval archief is opgetreden. De naamgeving is als volgt:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Schaalbaarheid van doorvoer eenheden

Gebeurtenis Hubs verkeer wordt gecontroleerd door [eenheden van de doorvoer](event-hubs-overview.md#capacity-and-security). Een eenheid met één doorvoer kan 1 MB/s of 1000 gebeurtenissen per seconde van de ingress- en egress tweemaal dat bedrag. Standaard gebeurtenis Hubs kan worden geconfigureerd met eenheden van 1-20-doorvoer en meer kunnen worden aangeschaft via een quotum verhogen [ondersteuning vragen][]. Gebruik dan de doorvoer van gekochte eenheden verminderd. Gebeurtenis Hubs archief kopieert de gegevens rechtstreeks vanuit de interne opslag van de gebeurtenis Hubs zonder doorvoer eenheid egress-quota en de egress opslaan voor andere lezers verwerking zoals Analytics Stream of vonk.

Na de configuratie gebeurtenis Hubs archief wordt automatisch uitgevoerd zodra u uw eerste gebeurtenis verzendt. Blijft deze actief te allen tijde. Gebeurtenis Hubs schrijft lege bestanden gemakkelijker om voor uw downstream verwerkt om te weten dat het proces werkt, als er geen gegevens. Dit biedt een voorspelbare cadence en markering die uw batch-processors kan voeden.

## <a name="setting-up-event-hubs-archive"></a>Gebeurtenis Hubs archief instellen

Gebeurtenis Hubs archief kan worden geconfigureerd tijdens gebeurtenis Hub maken via de portal of Azure Resource Manager. U kunt gewoon archiveren inschakelen door te klikken **op** . U configureren een opslag- en container door te klikken op het gedeelte van de **Container** van het blad. Aangezien de gebeurtenis Hubs archief Services verificatie met opslag gebruikt, hoeft u niet een verbindingsreeks opslag. Kiezen van de resource wordt de bron-URI voor uw opslag account automatisch geselecteerd. Als u de Azure Resource Manager gebruikt, moet u deze URI expliciet opgeven als een tekenreeks.

De standaard tijd-venster is vijf minuten. De minimumwaarde is 1, het maximum 15. Het venster **grootte** heeft een bereik van 10 tot 500 MB.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Archief toe te voegen aan een bestaande gebeurtenis Hub

Archieven kunnen worden geconfigureerd op een bestaande gebeurtenis Hubs die zich in een naamruimte gebeurtenis Hubs. De functie is niet beschikbaar op oudere berichten of gemengd type naamruimten. Klik op de naamruimte de blade **Essentials** laden en klik vervolgens op de Hub van de gebeurtenis die u wilt inschakelen of wijzig de instelling archief archief op een bestaande gebeurtenis Hub inschakelen of de archiefinstellingen wijzigen. Ten slotte, op in het gedeelte **Eigenschappen** van het blad geopend zoals in de volgende afbeelding.

![][2]

U kunt ook gebeurtenis Hubs archief via Azure Resource Manager-sjablonen. Raadpleeg [dit artikel](event-hubs-resource-manager-namespace-event-hub-enable-archive.md)voor meer informatie.

## <a name="exploring-the-archive-and-working-with-avro"></a>Het archief verkennen en het werken met Avro

Na de configuratie gebeurtenis Hubs archief bestanden gemaakt in de Azure opslag- en container die op de geconfigureerde tijdsduur. U kunt deze bestanden bekijken in een hulpprogramma zoals [Opslagverkenner Azure][]. U kunt de bestanden lokaal werken op deze downloaden.

De bestanden die met de gebeurtenis Hubs archief hebben de volgende Avro schema:

![][3]

Een eenvoudige manier verkennen van Avro is met behulp van de [Avro extra][] potjes van Apache. Na het downloaden van dit oppervlak, ziet u het schema van een specifiek Avro-bestand door de volgende opdracht uit te voeren:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

Met deze opdracht geeft als resultaat

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

U kunt ook extra Avro naar het bestand converteren naar JSON-indeling en andere bewerkingen uitvoeren.

Als u meer geavanceerde verwerking, voor downloaden en installeren Avro platform naar keuze. Op het moment van schrijven van dit, er zijn implementaties beschikbaar voor C, C++, C\#, Java, NodeJS, Perl, PHP, Python en Ruby.

Apache Avro heeft volledige gidsen aan de slag voor [Java][] en [Python][]. U kunt ook lezen het artikel [Aan de slag met de gebeurtenis Hubs archief](event-hubs-archive-python.md) .

## <a name="how-event-hubs-archive-is-charged"></a>Hoe de gebeurtenis Hubs archief is opgeladen

Gebeurtenis Hubs archief wordt op dezelfde manier gemeten doorvoer-eenheden als uurtarief kosten. De toeslag is rechtstreeks evenredig aan het aantal eenheden van doorvoer gekocht voor de naamruimte. Doorvoer eenheden zijn verhoogd en verlaagd, gebeurtenis Hubs archief vergroot en verkleint om overeenkomende prestaties te bieden. De meter in tandem gebeuren. De toeslag voor de gebeurtenis Hubs archief is $0,10 per uur per eenheid van doorvoer, met 50% korting tijdens de proefperiode aangeboden.

Gebeurtenis Hubs archief is echt de gemakkelijkste manier om gegevens te verkrijgen in Azure. Azure gegevens Lake, Azure Data Factory en Azure HDInsight gebruikt, kunt u uitvoeren batch-verwerking en andere analytics van uw keuze met behulp van vertrouwde hulpmiddelen en platforms op een schaal die u nodig hebt.

## <a name="next-steps"></a>Volgende stappen

U kunt weten over gebeurtenis Hubs via de volgende koppelingen:

- Een volledig [voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs][].
- Het monster van de [schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs][] .
- [Overzicht van de gebeurtenissen Hubs][]

[Apache Avro]: http://avro.apache.org/
[verzoek om ondersteuning]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Azure Opslagverkenner]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Avro-hulpprogramma 's]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3