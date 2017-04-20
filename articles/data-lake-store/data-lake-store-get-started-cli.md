<properties
   pageTitle="Aan de slag met Lake gegevensopslag met de opdrachtregelinterface platforms | Microsoft Azure"
   description="Azure cross-platform-opdrachtregel gebruiken voor het maken van een gegevensarchief Lake account en basisbewerkingen uitvoeren"
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
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Aan de slag met Azure Lake gegevensarchief Azure-opdrachtregel gebruiken

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over het Azure opdrachtregelinterface gebruikt een gegevensarchief voor Lake Azure-account maken en uitvoeren van eenvoudige bewerkingen zoals zoals mappen maakt, uploaden en downloaden van bestanden, het verwijderen van uw account, enz. Zie voor meer informatie over gegevensopslag Lake [Overzicht van Lake gegevensarchief](data-lake-store-overview.md).

De CLI Azure is geïmplementeerd in Node.js. Het kan worden gebruikt op elk platform dat Node.js, waaronder Windows, Mac en Linux ondersteunt. De CLI Azure is open-source. De broncode wordt beheerd in de GitHub op <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. In dit artikel beschreven hoe u met alleen de CLI Azure met Lake gegevensarchief. Zie [het gebruik van de CLI Azure]voor algemene richtlijnen voor het gebruik van Azure CLI [azure-command-line-tools].


##<a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI** - Zie [installeren en configureren van de CLI Azure](../xplat-cli-install.md) voor informatie over installatie en configuratie. Zorg ervoor dat u de computer opnieuw opstarten nadat u de CLI.

## <a name="authentication"></a>Verificatie

In dit artikel wordt het eenvoudiger voor verificatie met Lake gegevensarchief, waar u zich als een gebruiker van de eindgebruiker aanmeldt. Het toegangsniveau voor gegevensopslag van Lake systeem en het bestand vervolgens wordt beheerst door het toegangsniveau van de aangemelde gebruiker. Er zijn echter andere benaderingen alsmede voor de verificatie met Lake gegevensarchief, die **eindgebruikers** of **verificatie Services**. Zie voor instructies en meer informatie over het verifiëren [verifiëren met Lake gegevensarchief Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Meld u aan bij uw abonnement Azure

1. Volg de stappen beschreven in [verbinding maken met een Azure-abonnement van de Azure-opdrachtregelinterface (CLI Azure)](../xplat-cli-connect.md) en maak verbinding met uw abonnement met behulp van de `azure login` methode.

2. Overzicht van de abonnementen die gekoppeld aan uw account met behulp van zijn de `azure account list` opdracht.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Uit de bovenstaande uitvoer **Azure-sub-1** is ingeschakeld en het andere abonnement is **Azure-sub-2**. 

3. Selecteer het abonnement dat u gebruiken wilt onder. Als u werken onder het abonnement Azure-sub-2 wilt, gebruikt u de `azure account set` opdracht.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Maak een account Azure Lake gegevensarchief

Open een opdrachtprompt, shell of een terminalsessie en voer de volgende opdrachten.

2. Overschakelen naar de modus van Azure Resource Manager met de volgende opdracht:

        azure config mode arm


5. Een nieuwe resourcegroep maken. In de volgende opdracht geven de parameterwaarden die u wilt gebruiken.

        azure group create <resourceGroup> <location>

    Als de naam spaties bevat, zet u deze tussen aanhalingstekens. Bijvoorbeeld "Oost-US 2".

5. Het gegevensarchief Lake-account maken.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Mappen maken in het gegevensarchief Lake

U kunt mappen maken onder uw account Azure Lake gegevensopslag beheren en opslaan van gegevens. Gebruik de volgende opdracht maakt u een map met de naam 'mynewfolder' in de hoofdmap van het gegevensarchief Lake.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Bijvoorbeeld:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Gegevens uploaden naar uw winkel gegevens Lake

U kunt uw gegevens uploaden Lake gegevensarchief rechtstreeks op het hoogste niveau of naar een map die u hebt gemaakt in de account. De onderstaande fragmenten demonstratie voorbeeldgegevens uploaden naar de map (**mynewfolder**) die u in de vorige sectie hebt gemaakt.

Als u voorbeeldgegevens wilt uploaden zoekt, krijgt u de map **Data ambulances** uit de [Azure gegevens Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Download het bestand en deze opslaan in een lokale map op uw computer, zoals C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Bijvoorbeeld:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Lijst met bestanden in het gegevensarchief Lake

Gebruik de volgende opdracht om de bestanden in een gegevensarchief Lake-account.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Bijvoorbeeld:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

De uitvoer van deze moet er ongeveer als volgt:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Wijzigen, downloaden en verwijderen van gegevens uit het gegevensarchief Lake

* **Om een bestand te wijzigen**, gebruikt u de volgende opdracht:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Bijvoorbeeld:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Een bestand te downloaden**, gebruikt u de volgende opdracht. Controleer of de opgegeven bestemmingspad al bestaat.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Bijvoorbeeld:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Een bestand te verwijderen**, gebruikt u de volgende opdracht:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Bijvoorbeeld:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Voer desgevraagd **Y** om het item te verwijderen.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>De toegangsbeheerlijst voor een map weergeven in een gegevensarchief Lake

Met de volgende opdracht kunt u de ACL's voor een map Lake gegevensarchief. In de huidige versie kunnen ACL's alleen worden ingesteld op de hoofdmap van het gegevensarchief Lake. De parameter path hieronder worden dus altijd hoofdmap (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Bijvoorbeeld:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Het gegevensarchief Lake account verwijderen

Gebruik de volgende opdracht uit om een gegevensarchief Lake account verwijderen.

    azure datalake store account delete <dataLakeStoreAccountName>

Bijvoorbeeld:

    azure datalake store account delete mynewdatalakestore

Voer desgevraagd **Y** om de account te verwijderen.


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
