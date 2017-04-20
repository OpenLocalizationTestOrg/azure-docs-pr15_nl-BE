<properties
    pageTitle="Gegevens importeren in een Machine Learning Studio van on line bronnen | Microsoft Azure"
    description="Het importeren van uw trainingsgegevens Azure Machine Learning Studio uit diverse online bronnen."
    keywords="importeren van gegevens, gegevens opmaken, gegevenstypen, gegevensbronnen, trainingsgegevens"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Gegevens importeren in Azure Machine Learning Studio uit diverse online bronnen met de module gegevens importeren

Dit artikel wordt de ondersteuning voor het importeren van online gegevens uit verschillende bronnen en de informatie die nodig is om gegevens te verplaatsen van deze bronnen in een experiment Azure Machine Learning.

> [AZURE.NOTE] Dit artikel bevat algemene informatie over het [Importeren van gegevens] [ import-data] module. Voor meer informatie over de soorten gegevens die u openen gedetailleerde kunt, indelingen, parameters en antwoorden op veelgestelde vragen u de module naslagonderwerp voor het [Importeren van gegevens] [ import-data] module.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Inleiding

Kunt u gegevens vanuit de Studio Azure Machine Learning openen vanuit een van de verschillende online bronnen tijdens de proef wordt uitgevoerd met behulp van de [Gegevens importeren] [ import-data] module:

- De URL van een Web via HTTP
- Hadoop met HiveQL
- Azure blob-opslag
- Azure tabel
- Azure SQL-database of SQL Server op Azure VM
- SQL Server-database op de gebouwen
- Een data provider, die momenteel de OData-feed
 
De werkstroom voor het verrichten van proeven in Azure Machine Learning Studio bestaat uit componenten op het canvas te slepen en neerzetten. Voor toegang tot on line bronnen, de [Gegevens importeren] toevoegen[ import-data] module voor uw experiment, selecteert u de **gegevensbron**en geeft u vervolgens de parameters die nodig zijn voor toegang tot de gegevens. De online gegevensbronnen die worden ondersteund worden in de onderstaande tabel gespecificeerd. In deze tabel worden ook de bestandsindelingen die worden ondersteund en de parameters die worden gebruikt voor toegang tot de gegevens.

Houd er rekening mee dat omdat de gegevens van deze opleiding wordt geopend terwijl het experiment wordt uitgevoerd, alleen beschikbaar bij dit experiment is. Vergelijking: gegevens die is opgeslagen in een dataset-module beschikbaar zijn voor elk experiment in uw werkruimte.

> [AZURE.IMPORTANT] Op dit moment de [Gegevens importeren] [ import-data] en [Gegevens exporteren] [ export-data] modules kunnen lezen en schrijven van gegevens alleen uit de Azure opslag gemaakt met behulp van het model Klassiek implementatie. Met andere woorden, is nieuwe Azure Blob-opslag accounttype een hot opslag access laag of leuke opslag access laag biedt nog niet ondersteund. 

> In het algemeen geen Azure opslag accounts dat u hebt gemaakt voordat u deze serviceoptie bekend geworden geen invloed. Als u een nieuwe account te maken, **klassieke** selecteren voor het model Deployment of resourcemanager gebruiken en voor **type Account**kunt selecteren **voor algemene doeleinden** in plaats van **Blob-opslag**. 

> Zie voor meer informatie [Azure Blob-opslag: Hot en Cool opslag lagen](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Ondersteunde gegevensbronnen voor on line
Azure Machine Learning **Data Import** -module ondersteunt de volgende gegevensbronnen:

Gegevensbron | Beschrijving | Parameters |
---|---|---|
Web-URL via HTTP |Leest de gegevens in de door komma's gescheiden waarden (CSV), door tabs gescheiden waarden (), kenmerkrelatie bestandsindeling (ARFF) en Support Vector Machines (SVM licht), van een web URL die gebruikmaakt van HTTP | <b>URL</b>: Hiermee geeft u de volledige naam van het bestand, inclusief de URL en de bestandsnaam met de extensie. <br/><br/><b>Gegevensindeling</b>: Hiermee geeft u een van de gegevens van de ondersteunde indelingen: CSV-, TSV, ARFF of SVM licht. Als de gegevens een veldnamenrij bevat, wordt gebruikt voor het toewijzen van kolomnamen.|
Hadoop/HDFS|Leest gegevens van gedistribueerde opslag in Hadoop. U opgeven welke gegevens die u wilt met behulp van HiveQL, een SQL-achtige querytaal. HiveQL kan ook worden gebruikt voor statistische gegevens en het uitvoeren van gegevens filteren voordat u de gegevens aan een Machine Learning Studio toevoegen.|<b>Databasequery component</b>: Hiermee geeft u de component query gebruikt om de gegevens te genereren.<br/><br/><b>HCatalog server-URI</b> : opgegeven de naam van het cluster met behulp van de indeling * &lt;de naam van de cluster&gt;. azurehdinsight.net.*<br/><br/><b>Naam van gebruikersaccount Hadoop</b>: geeft Hadoop naam van de gebruikersaccount die wordt gebruikt voor het inrichten van het cluster.<br/><br/><b>Hadoop wachtwoord</b> : Hiermee geeft u de referenties die worden gebruikt tijdens het inrichten van het cluster. Zie voor meer informatie, [Maak Hadoop clusters in het HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Locatie van uitvoergegevens</b>: Hiermee geeft u aan of de gegevens worden opgeslagen in een Hadoop gedistribueerd bestandssysteem (HDFS) of in Azure. <br/><ul>Als u gegevens over de output in HDFS opslaat, geeft u de HDFS server-URI. (Zorg ervoor dat de naam van het cluster HDInsight zonder het voorvoegsel HTTPS:// gebruiken). <br/><br/>Als u de gegevens over de output in Azure opslaat, moet u de accountnaam Azure opslag, opslag toegangstoets en containernaam opslag.</ul>|
SQL-database |Leest de gegevens die zijn opgeslagen in een Azure SQL-database of een SQL Server-database op een Azure virtuele machine wordt uitgevoerd. | <b>Naam van de databaseserver</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><ul>Voer de naam van de server die wordt gegenereerd in het geval van Azure SQL-Database. Deze heeft meestal de vorm * &lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Voer in het geval van een SQL-server die wordt gehost op een Azure Virtual machine *tcp:&lt;Virtual Machine DNS-naam&gt;, 1433*</ul><br/><b>Naam database </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>Naam van gebruikersaccount server</b>: Hiermee geeft u een gebruikersnaam voor een account met machtigingen voor de database. <br/><br/><b>Serverwachtwoord</b>: het wachtwoord voor de gebruikersaccount.<br/><br/><b>Accepteer het certificaat van elke server</b>: deze optie (minder veilig) gebruiken als u overslaan van het certificaat van de site controleren wilt voordat u de gegevens lezen.<br/><br/><b>Databasequery</b>: Voer een SQL-instructie over de gegevens die u wilt lezen.|
SQL-database op de gebouwen |Leest de gegevens die zijn opgeslagen in een SQL-database op gebouwen. | <b>Gateway gegevens</b>: de naam van de Data Management Gateway is geïnstalleerd op een computer waar zij toegang hebben tot uw SQL Server-database. Voor meer informatie over het instellen van de gateway zien [uitvoeren geavanceerde analytics met Azure Machine Learning met gegevens uit een SQL-server op locatie](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Naam van de databaseserver</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><br/><b>Naam database </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>Naam van gebruikersaccount server</b>: Hiermee geeft u een gebruikersnaam voor een account met machtigingen voor de database. <br/><br/><b>Gebruikersnaam en wachtwoord</b>: klik op <b>Enter waarden</b> invoeren, uw databasereferenties. U kunt Windows-verificatie of SQL Server-verificatie, afhankelijk van hoe uw op ruimten SQL-Server is geconfigureerd.<br/><br/><b>Databasequery</b>: Voer een SQL-instructie over de gegevens die u wilt lezen.|
Azure tabel|Leest gegevens van de tabel service in Azure opslag.<br/><br/>Als u grote hoeveelheden gegevens niet vaak worden gelezen, gebruikt u de tabel Azure Service. Het biedt een flexibel, niet-relationele (NoSQL), sterk schaalbare, voordelige en hoge beschikbaarheid opslagoplossing.| De opties voor het **Importeren van gegevens** veranderen afhankelijk van of u toegang tot openbare informatie of een particuliere opslag die inloggegevens vereist. Dit wordt bepaald door het <b>Type verificatie</b> waarvoor de waarde 'PublicOrSAS' of 'Account', die allemaal een eigen set parameters. <br/><br/><b>Openbare of gedeelde toegang handtekening (SAS) URI</b>: de parameters zijn:<br/><br/><ul><b>Tabel URI</b>: Hiermee geeft u aan het publiek of SAS-URL voor de tabel.<br/><br/><b>Hiermee bepaalt u welke rijen u wilt zoeken naar namen van eigenschappen</b>: de waarden zijn <i>TopN</i> voor het scannen van het opgegeven aantal rijen of <i>ScanAll</i> om alle rijen in de tabel. <br/><br/>Als de gegevens homogeen en voorspelbaar is, verdient u *TopN* selecteren en typ een getal voor N. Voor grote tabellen kan hierdoor sneller lezen tijden.<br/><br/>Als de structuur van de gegevens met de sets eigenschappen die verschillen op basis van de diepte en de positie van de tabel de optie *ScanAll* voor het scannen van alle rijen. Hierdoor wordt de integriteit van de resulterende eigenschappen en metagegevens conversie.<br/><br/></ul><b>Particuliere opslag Account</b>: de parameters zijn: <br/><br/><ul><b>Naam</b>: de naam van de account met de tabel te lezen.<br/><br/><b>Key account</b>: Hiermee geeft u de opslag-sleutel die is gekoppeld aan de account.<br/><br/><b>Naam</b> : de naam van de tabel met de gegevens te lezen.<br/><br/><b>Rijen om te zoeken naar namen van eigenschappen</b>: de waarden zijn <i>TopN</i> voor het scannen van het opgegeven aantal rijen of <i>ScanAll</i> om alle rijen in de tabel.<br/><br/>Als de gegevens homogeen en voorspelbaar is, wordt aangeraden dat u *TopN* selecteren en typ een getal voor N. Voor grote tabellen kan hierdoor sneller lezen tijden.<br/><br/>Als de structuur van de gegevens met de sets eigenschappen die verschillen op basis van de diepte en de positie van de tabel de optie *ScanAll* voor het scannen van alle rijen. Hierdoor wordt de integriteit van de resulterende eigenschappen en metagegevens conversie.<br/><br/>|
Azure Blob-opslag | Gegevens die zijn opgeslagen in de Blob-service in Azure, opslag, met inbegrip van afbeeldingen, niet-gestructureerde tekst of binaire gegevens leest.<br/><br/>U kunt de Blob-service gegevens openbaar toegankelijk te maken of voor privé gegevens van toepassing. U kunt toegang tot uw gegevens vanaf elke locatie met behulp van HTTP- of HTTPS-verbindingen. | De opties in de module **Gegevens importeren** veranderen afhankelijk van of u toegang tot openbare informatie of een particuliere opslag die inloggegevens vereist. Dit wordt bepaald door het <b>Type verificatie</b> die de waarde van 'PublicOrSAS' of 'Account'.<br/><br/><b>Openbare of gedeelde toegang handtekening (SAS) URI</b>: de parameters zijn:<br/><br/><ul><b>URI</b>: openbaar of SAS-URL voor de blob storage bevat.<br/><br/><b>Indeling</b>: Hiermee wordt de indeling van de gegevens in de Blob-service. De ondersteunde indelingen zijn CSV en TSV ARFF.<br/><br/></ul><b>Particuliere opslag Account</b>: de parameters zijn: <br/><br/><ul><b>Naam</b>: de naam van de account met de label die u wilt lezen.<br/><br/><b>Key account</b>: Hiermee geeft u de opslag-sleutel die is gekoppeld aan de account.<br/><br/><b>Pad naar de container, map, of blob</b> : Hiermee geeft u de naam van de blob met de gegevens te lezen.<br/><br/><b>BLOB-bestandsindeling</b>: Hiermee wordt de indeling van de gegevens in de blob-service. De gegevensindelingen ondersteund zijn CSV-, TSV, ARFF, CSV met een opgegeven codering en Excel. <br/><br/><ul>Als de indeling CSV- of TSV, moet u aangeven of het bestand een koprij bevat.<br/><br/>U kunt de optie Excel gegevens uit Excel-werkmappen te lezen. In de <i>bestandsindeling van Excel-gegevens</i> , geven of de gegevens in een bereik op een werkblad of in een Excel-tabel is. Geef de naam van het werkblad of de tabel die u wilt lezen in de optie <i>Excel-werkblad of een ingesloten tabel </i>.</ul><br/>|
Feed gegevensprovider | Leest gegevens van een ondersteunde feed-provider. Op dit moment alleen de Open Data Protocol (OData)-indeling wordt ondersteund. | <b>Gegevens type inhoud</b>: Hiermee geeft u de OData-indeling.<br/><br/><b>De bron-URL</b>: Hiermee geeft u de volledige URL voor de data-kanaal. <br/>Bijvoorbeeld de volgende URL van de voorbeelddatabase Noordenwind wordt gelezen: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
