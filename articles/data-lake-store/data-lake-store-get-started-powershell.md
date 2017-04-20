<properties
   pageTitle="Aan de slag met het gegevensarchief Lake | Azure"
   description="Azure PowerShell gebruiken voor het maken van een gegevensarchief Lake account en basisbewerkingen uitvoeren"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Aan de slag met Azure Lake gegevensarchief met Azure PowerShell

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over het gebruik van Azure PowerShell een account Azure Lake gegevensarchief maken en uitvoeren van eenvoudige bewerkingen zoals zoals mappen maakt, uploaden en downloaden van bestanden, het verwijderen van uw account, enz. Zie voor meer informatie over gegevensopslag Lake [Overzicht van Lake gegevensarchief](data-lake-store-overview.md).

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

* **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell 1.0 of hoger**. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

## <a name="authentication"></a>Verificatie

In dit artikel wordt het eenvoudiger voor verificatie met Lake gegevensarchief, waarbij u wordt gevraagd uw accountreferenties Azure-in te voeren. Het toegangsniveau voor gegevensopslag van Lake systeem en het bestand vervolgens wordt beheerst door het toegangsniveau van de aangemelde gebruiker. Er zijn echter andere benaderingen alsmede voor de verificatie met Lake gegevensarchief, die **eindgebruikers** of **verificatie Services**. Zie voor instructies en meer informatie over het verifiëren [verifiëren met Lake gegevensarchief Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Maak een account Azure Lake gegevensarchief

1. Op het bureaublad opent een nieuw venster van Windows PowerShell en voer het volgende fragment log in op uw account Azure, stelt u het abonnement en registreren van de provider Lake gegevensarchief. Wanneer u wordt gevraagd aan te melden, moet dat u zich aanmelden als een van de admininistrators abonnement/eigenaar:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Een gegevensarchief voor Lake Azure-account is gekoppeld aan een resourcegroep Azure. Begin met het maken van een Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Maak een brongroep Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Maak een brongroep Azure")

2. Maak een account Azure Lake gegevensarchief. De naam die u opgeeft moet alleen kleine letters en cijfers bevatten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Een account Azure Lake gegevensarchief maken] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Een account Azure Lake gegevensarchief maken")

3. Controleer of de account is gemaakt.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    De uitvoer van dit moet **waar**zijn.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Directory-structuren maken in uw winkel Azure gegevens Lake

U kunt mappen maken onder uw account Azure Lake gegevensopslag beheren en opslaan van gegevens.

1. Een basismap opgeven.

        $myrootdir = "/"

2. Maak een nieuwe map met de naam **mynewdirectory** in de hoofdmap opgegeven.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Controleer of de nieuwe map is gemaakt.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Het moet de volgende uitvoer weergegeven:

    ![Controleer of de map] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Controleer of de map")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Gegevens uploaden naar uw winkel Azure gegevens Lake

U kunt uw gegevens uploaden Lake gegevensarchief rechtstreeks op het hoogste niveau of naar een map die u hebt gemaakt in de account. De onderstaande fragmenten demonstratie voorbeeldgegevens uploaden naar de map (**mynewdirectory**) die u in de vorige sectie hebt gemaakt.

Als u voorbeeldgegevens wilt uploaden zoekt, krijgt u de map **Data ambulances** uit de [Azure gegevens Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Download het bestand en deze opslaan in een lokale map op uw computer, zoals C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Wijzigen, downloaden en verwijderen van gegevens uit het gegevensarchief Lake

Wijzig de naam van een bestand, gebruik de volgende opdracht:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Gebruik de volgende opdracht om een bestand te downloaden:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Als u wilt een bestand verwijdert, gebruik de volgende opdracht:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Voer desgevraagd **Y** om het item te verwijderen. Als er meer dan één bestand wilt verwijderen, kunt u de paden die zijn gescheiden door komma opgeven.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Uw account Azure Lake gegevensarchief verwijderen

Gebruik de volgende opdracht als u uw account Lake gegevensopslag te verwijderen.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Voer desgevraagd **Y** om de account te verwijderen.


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)
