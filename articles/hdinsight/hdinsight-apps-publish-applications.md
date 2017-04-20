<properties
    pageTitle="Uitgeven van toepassingen HDInsight | Microsoft Azure"
    description="Informatie over het maken en uitgeven van toepassingen van de HDInsight."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Uitgeven van toepassingen op de markt Azure HDInsight

Een HDInsight toepassing is een toepassing die gebruikers op een cluster van Linux-gebaseerde HDInsight installeren kunnen. Deze toepassingen kunnen worden ontwikkeld door Microsoft, onafhankelijke softwareleveranciers (ISV) of door uzelf. In dit artikel leert u het publiceren van een HDInsight toepassing op de markt Azure.  Zie [een voorstel aan de markt Azure publiceren](../marketplace-publishing/marketplace-publishing-getting-started.md)voor algemene informatie over publiceren op de markt Azure.

HDInsight-toepassingen gebruiken het model *Doen om uw eigen licentie (BYOL)* , waarbij toepassing serviceprovider is verantwoordelijk voor de licentie van de toepassing voor eindgebruikers en eindgebruikers worden alleen in rekening gebracht door Azure voor de bronnen die ze, zoals de cluster HDInsight en de VMs/knooppunten maken. Facturering van de toepassing zelf is op dit moment niet gedaan via Azure.

Andere toepassing HDInsight gerelateerd artikel:

- [Toepassingen installeren HDInsight](hdinsight-apps-install-applications.md): informatie over het installeren van een toepassing HDInsight clusters.
- [Aangepaste toepassingen voor HDInsight](hdinsight-apps-install-custom-applications.md): informatie over het installeren en testen van aangepaste toepassingen voor HDInsight.

 
## <a name="prerequisites"></a>Vereisten

Om uw aangepaste aanvraag indienen bij de markt, moet u hebt gemaakt en getest uw aangepaste toepassing. Raadpleeg de volgende artikelen:

- [Aangepaste toepassingen voor HDInsight](hdinsight-apps-install-custom-applications.md): informatie over het installeren en testen van aangepaste toepassingen voor HDInsight.

U moet hebt uw account developer ook registreren. Zie [een voorstel aan de markt Azure publiceren](../marketplace-publishing/marketplace-publishing-getting-started.md) en [een Microsoft Developer-account maken](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Toepassing definiëren

Er zijn twee stappen betrokken voor het uitgeven van toepassingen aan de markt Azure.  Eerst definieert u een **createUiDef.json** -bestand om aan te geven welke clusters uw toepassing compatibel is met; en vervolgens publiceert u de sjabloon van het portal voor Azure. Hier volgt een voorbeeld van een createUiDef.json-bestand.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Veld  | Beschrijving   | Mogelijke waarden|
|-------|---------------|----------------|
|typen  | De clustertypen dat compatibel is met de toepassing.    |Hadoop, HBase, Storm, Spark (of een combinatie hiervan)|
|lagen  | De lagen van het cluster dat compatibel is met de toepassing.    |Standaard, Premium, (of beide)|
|versies|  De HDInsight clustertypen dat compatibel is met de toepassing.    |3.4|

## <a name="package-application"></a>Pakket toepassen

Maak een zip-bestand met alle benodigde bestanden voor het installeren van uw toepassingen met HDInsight. U moet het zip-bestand in [de toepassing publiceren](#publish-application).

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Zie een voorbeeld bij het [installeren van aangepaste toepassingen voor HDInsight](hdinsight-apps-install-custom-applications.md).

    >[AZURE.IMPORTANT] De naam van de toepassing installatie scriptnamen moeten uniek zijn voor een bepaald cluster met de onderstaande indeling. Bovendien een installeren en verwijderen scriptacties moet idempotency is ingeschakeld, wat betekent dat de scripts worden aangeroepen repeatly terwijl hetzelfde resultaat te produceren.
    
    >   de naam': ' [concat (' kleurtoon-installatie-v0 ','-', uniquestring('applicationName')] "
        
    >Opmerking Er zijn drie delen op de scriptnaam:
        
    >   1. Een script voorvoegsel, waaronder de naam van de toepassing of een naam die relevant zijn voor de toepassing.
    >   2. A "-" voor de leesbaarheid.
    >   3. Een unieke tekenreeks-functie met de naam van de toepassing als de parameter.

    >   Een voorbeeld is de bovengenoemde uiteinden van steeds: Kleurtoon-installatie-v0-4wkahss55hlas in de lijst met permanente script actie. Zie [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json)voor een monster JSON-nettolading.

- Alle vereiste scripts.

> [AZURE.NOTE] De toepassingsbestanden (inclusief webtoepassingsbestanden als een) kan worden gevonden op een openbaar toegankelijke eindpunt.

## <a name="publish-application"></a>Toepassingen publiceren

De volgende stappen voor het publiceren van een HDInsight-toepassing:

1. Aanmelden op de [portal voor publiceren Azure](https://publish.windowsazure.com/).
2. Klik op **sjablonen van de oplossing** van de links voor het maken van een nieuwe sjabloon van de oplossing.
3. Voer een titel in en klik vervolgens op **een nieuwe sjabloon voor oplossing maken**.
3. Klik op **Create Dev Center account en deelnemen aan het programma Azure** om uw bedrijf te registreren als u nog niet hebt gedaan.  Zie [een Microsoft Developer-account maken](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Klik op **definiëren van bepaalde topologieën aan de slag**. Een sjabloon oplossing is een "bovenliggende" op al haar topologieën. Topologieën met meerdere kunt u definiëren in een sjabloon voor aanbod/oplossing. Wanneer een voorstel tot tijdelijke geduwd, is het met alle bijbehorende topologieën geduwd. 
4. Voer een naam voor de topologie en klik vervolgens op het plusteken (+).
5. Voer een nieuwe versie en klik vervolgens op het plusteken (+).
6. Upload het zip-bestand in [pakket toepassing](#package-application)wordt voorbereid.  
7. Klik op **certificaat aanvragen**. Het team van Microsoft certificering de bestanden te beoordelen en certificeren van de topologie.

## <a name="next-steps"></a>Volgende stappen

- [Toepassingen installeren HDInsight](hdinsight-apps-install-applications.md): informatie over het installeren van een toepassing HDInsight clusters.
- [Aangepaste toepassingen voor HDInsight](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een toepassing niet gepubliceerde HDInsight HDInsight.
- [HDInsight aanpassen Linux gebaseerde clusters met scriptactie](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van de actie Script om extra toepassingen te installeren.
- [Hadoop maken Linux gebaseerde clusters in HDInsight met behulp van bronbeheer Azure-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): informatie over het aanroepen van HDInsight clusters maken sjablonen Resource Manager.
- [Gebruik lege rand nodes in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een randknooppunt leeg voor toegang tot de cluster HDInsight, testen van toepassingen voor HDInsight en HDInsight toepassingen die als host fungeert.

