<properties
   pageTitle="Overzicht van Azure Lake gegevensarchief | Microsoft Azure"
   description="Begrijpen wat is Azure Lake gegevensopslag en de waarde die het biedt via andere opgeslagen gegevens"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Overzicht van Azure Lake gegevensarchief

Azure Lake gegevensarchief is een hele onderneming hyperscale '-opslagplaats voor grote gegevens analytisch werkbelasting. Azure gegevens Lake kunt u gegevens van elke grootte, type en opname snelheid op één enkele locatie voor operationele en experimentele analytics opnemen.

> [AZURE.TIP] Met het [gegevensarchief Lake leren pad](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) kunt u uitgaan van de service Azure Lake gegevensarchief.

Azure Lake gegevensarchief van Hadoop (beschikbaar bij HDInsight cluster) toegankelijk zijn met behulp van de WebHDFS-compatibele REST API's. Het is speciaal ontworpen om analytics van de opgeslagen gegevens en voor data analytics scenario's is afgestemd. Uit de doos, bevat alle mogelijkheden van de enterprise-grade, beveiliging, beheersbaarheid, schaalbaarheid, betrouwbaarheid en beschikbaarheid, essentieel zijn voor de onderneming concrete use-cases.


![Azure gegevens Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

De belangrijkste mogelijkheden van de Lake Azure gegevens onder andere de volgende.

### <a name="built-for-hadoop"></a>Gebouwd voor Hadoop

Het archief Azure gegevens Lake is een bestandssysteem dat Apache Hadoop compatibel met Hadoop Distributed bestand System (HDFS) en het werkt met het Hadoop-ecosysteem.  Uw bestaande HDInsight-toepassingen of services die gebruikmaken van de API WebHDFS kunnen gemakkelijk integreren met Lake gegevensarchief. Lake gegevensarchief stelt tevens een REST WebHDFS-compatibele interface voor toepassingen

Gegevens die zijn opgeslagen in het gegevensarchief Lake kunnen gemakkelijk worden geanalyseerd met analytische kaders, Hadoop zoals MapReduce of component. HDInsight voor Microsoft Azure clusters kunnen worden ingericht en rechtstreeks toegang tot gegevens die zijn opgeslagen in het gegevensarchief Lake geconfigureerd.

### <a name="unlimited-storage-petabyte-files"></a>Onbeperkte opslag, petabyte bestanden

Azure Lake gegevensarchief biedt onbeperkte opslag en is geschikt voor het opslaan van een aantal gegevens voor analytics. Het is niet de eventuele limieten van de account, formaat, grootte of de hoeveelheid gegevens die kan worden opgeslagen in een meer gegevens opleggen. Afzonderlijke bestanden kunnen variëren van kilobyte naar petabytes in grootte, waardoor het een ideale keuze voor het opslaan van elk type gegevens. Gegevens blijvend worden opgeslagen door meerdere kopieën te maken en er is geen beperking van de duur waarvoor de gegevens kunnen worden opgeslagen in het data meer tijd.

### <a name="performance-tuned-for-big-data-analytics"></a>Prestaties worden afgestemd voor big data analytics

Azure Lake gegevensarchief is gebouwd voor het uitvoeren van grootschalige analytische systemen waarvoor een massale doorvoer opvragen en analyseren van grote hoeveelheden gegevens. Delen van een bestand de gegevens meer gespreid over een aantal afzonderlijke opslagservers. Dit verbetert de doorvoer lezen bij het lezen van het bestand parallel voor het uitvoeren van data-analytics.


### <a name="enterprise-ready-highly-available-and-secure"></a>Enterprise-ready: Hoge beschikbaarheid en beveiliging

Azure Lake gegevensarchief biedt industriestandaard beschikbaarheid en betrouwbaarheid. De activa van uw gegevens zijn blijvend door redundante exemplaren te beschermen tegen onverwachte fouten opgeslagen. Ondernemingen kunnen Azure gegevens meer gebruiken in hun oplossingen als een belangrijk onderdeel van hun bestaande platform voor gegevens.

Lake gegevensarchief biedt beveiliging ook enterprise-klasse voor de opgeslagen gegevens. Zie [gegevens beveiligen in Azure Lake gegevensarchief](#DataLakeStoreSecurity)voor meer informatie.


### <a name="all-data"></a>Alle gegevens

Azure-gegevensarchief voor meer gegevens kunt opslaan in de oorspronkelijke indeling in de huidige staat, zonder enige voorafgaande transformaties. Lake gegevensarchief hoeft niet een schema worden gedefinieerd voordat de gegevens zijn geladen, waardoor het tot de afzonderlijke analytisch kader te interpreteren van de gegevens en een schema te definiëren op het moment van de analyse. Kunnen bestanden van willekeurige grootte en indelingen op te slaan, maakt het mogelijk voor Lake gegevensarchief gestructureerde, semi-gestructureerde en ongestructureerde gegevens verwerken.

Azure Lake gegevensarchief containers voor gegevens zijn in feite de mappen en bestanden. U werkt op de opgeslagen gegevens met behulp van de SDK's, Azure Portal en Azure Powershell. Als u de gegevens in het archief van deze interfaces en het gebruik van de juiste containers plaatst, kunt u elk type gegevens opslaan. Lake gegevensarchief voert geen eventuele speciale verwerking van gegevens op basis van het type opgeslagen gegevens.


## <a name="DataLakeStoreSecurity"></a>Het beveiligen van gegevens in de gegevensopslag Lake Azure

Azure Lake gegevensarchief Azure Active Directory wordt gebruikt voor verificatie en toegangsbeheerlijsten (ACL's) voor het beheren van toegang tot uw gegevens.

| Functie                                 | Beschrijving                              |
|-----------------------------------------|------------------------------------------|
| Verificatie | Azure Lake gegevensarchief integreert met Azure Active Directory (AAD) voor identiteits- en toegangsbeheer voor de gegevens die zijn opgeslagen in Azure Lake gegevensarchief. Als gevolg van de integratie, Azure gegevens meer voordelen uit alle AAD functies zoals meerledige verificatie, voorwaardelijke toegang, op rollen gebaseerde toegangscontrole, controle op het gebruik van toepassingen, beveiliging, bewaking en signalering, enz. Azure Lake gegevensarchief ondersteunt het OAuth 2.0-protocol voor verificatie met in de interface van de REST. |
| Toegangsbeheer                          | Toegangsbeheer biedt Azure Lake gegevensarchief door de ondersteuning van POSIX-achtige machtigingen weergegeven door het WebHDFS-protocol. In de huidige versie kunnen ACL's op de map, submappen, alsmede afzonderlijke bestanden worden ingeschakeld. De ACL's die u op de hoofdmap toepast wordt ook van toepassing op alle onderliggende mappen/bestanden ook.|

Wilt u meer informatie over het beveiligen van gegevens in het gegevensarchief Lake. Volg de onderstaande koppelingen.

* Zie voor instructies over hoe u beveiligde gegevens in het gegevensarchief Lake, [gegevens beveiligen in Azure Lake gegevensarchief](data-lake-store-secure-data.md).
* Voorkeur voor video's? [Bekijk deze video](https://mix.office.com/watch/1q2mgzh9nn5lx) voor het beveiligen van gegevens die zijn opgeslagen in het gegevensarchief Lake.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Toepassingen die compatibel zijn met Azure Lake gegevensarchief

Azure Lake gegevensarchief is compatibel met de meeste open source componenten in het Hadoop-ecosysteem. Ook worden geïntegreerd netjes in andere Azure diensten. Dit maakt een perfecte optie voor uw opslagbehoeften gegevens Lake gegevensarchief. Volg de onderstaande koppelingen voor meer informatie over hoe Lake gegevensopslag kan worden gebruikt met open-source componenten, alsmede andere Azure diensten.

* Zie [toepassingen en services die compatibel zijn met Azure Lake gegevensarchief](data-lake-store-compatible-oss-other-applications.md) voor een lijst van open-source-toepassingen met Lake gegevensarchief.
* Zie [integratie met andere Azure services](data-lake-store-integrate-with-other-services.md) om te begrijpen hoe gegevensarchief Lake met andere Azure services kan worden gebruikt om een breder scala van scenario's.
* Zie [scenario's voor het gebruik van Lake gegevensarchief](data-lake-store-data-scenarios.md) voor informatie over het gegevensarchief Lake gebruiken in scenario's zoals ingesting, gegevens, gegevens verwerken downloaden van gegevens en gegevens visualiseren.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Wat is Azure Lake gegevensarchief bestandssysteem (adl: / /)?

Lake gegevensarchief toegankelijk is via het nieuwe bestandssysteem, de AzureDataLakeFilesystem (adl: / /), in een omgeving van Hadoop (beschikbaar bij HDInsight cluster). Toepassingen en services die gebruikmaken van adl: / / kunnen profiteren van de verdere verbetering van de prestaties die zijn momenteel niet beschikbaar in WebHDFS. Als gevolg hiervan Lake gegevensarchief biedt u de flexibiliteit of gebruik de beste prestaties met de aanbevolen optie van het gebruik van adl: / / of onderhouden van bestaande code rechtstreeks gebruik van de API WebHDFS voort te zetten. Azure HDInsight volledig maakt gebruik van de AzureDataLakeFilesystem voor de beste prestaties op Lake gegevensarchief.

U kunt toegang tot uw gegevens in het gegevensarchief Lake met `adl://<data_lake_store_name>.azuredatalakestore.net`. Zie voor meer informatie over hoe u toegang tot de gegevens in het gegevensarchief Lake, [Eigenschappen van de opgeslagen gegevens bekijken](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Hoe start ik met behulp van Azure Lake gegevensarchief

Zie [aan de slag met het gegevensarchief Lake met behulp van de Portal Azure](data-lake-store-get-started-portal.md), voor het inrichten van een gegevensarchief Lake met behulp van de Portal Azure. Zodra u hebt ingericht Azure gegevens Lake, leert u aanbiedingen zoals Azure gegevens Lake Analytics of Azure HDInsight grote gegevens gebruiken met Lake gegevensarchief. Ook kunt u een .NET-toepassing een account Azure Lake gegevensarchief maken en uitvoeren van bewerkingen, zoals het uploaden, downloaden van gegevens, enz.

- [Aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Aan de slag met Azure Lake gegevensarchief maken met .NET SDK](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Video's Lake gegevensarchief

Als u liever met het bekijken van video's om te leren, biedt Lake gegevensarchief video's voor een scala aan functies.

* [Maak een Account Azure Lake gegevensarchief](https://mix.office.com/watch/1k1cycy4l4gen)
* [De Data Explorer gebruiken voor het beheren van gegevens in de gegevensopslag Lake Azure](https://mix.office.com/watch/icletrxrh6pc)
* [Verbinding maken met gegevens van Azure Lake Analytics Azure Lake gegevensarchief](https://mix.office.com/watch/qwji0dc9rx9k)
* [Access Azure Lake gegevensarchief via Data Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)
* [Verbinding maken met Azure HDInsight Azure Lake gegevensarchief](https://mix.office.com/watch/l93xri2yhtp2)
* [Access Azure Lake gegevensarchief via component en varken](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [DistCp (Hadoop Distributed kopie) gebruiken om gegevens te kopiëren en naar Azure Lake gegevensarchief](https://mix.office.com/watch/1liuojvdx6sie)
* [Apache-Sqoop gebruiken om gegevens te verplaatsen tussen relationele gegevensbronnen en Azure Lake gegevensarchief](https://mix.office.com/watch/1butcdjxmu114)
* [Integratie van gegevens met Azure Data Factory voor Azure Lake gegevensarchief](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Het beveiligen van gegevens in de Azure Lake gegevensopslag](https://mix.office.com/watch/1q2mgzh9nn5lx)



