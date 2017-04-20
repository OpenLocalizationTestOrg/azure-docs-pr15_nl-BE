<properties 
   pageTitle="Aan de slag met het gegevensarchief Lake | Azure" 
   description="De portal te gebruiken voor het maken van een gegevensarchief Lake account en eenvoudige bewerkingen uitvoeren in het gegevensarchief Lake" 
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
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Aan de slag met Azure Lake gegevensarchief maken met de Portal Azure

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over het gebruik van de Portal Azure een account Azure Lake gegevensarchief maken en uitvoeren van eenvoudige bewerkingen zoals zoals mappen maakt, uploaden en downloaden van bestanden, verwijderen van uw account, enz. Zie voor meer informatie over gegevensopslag Lake [Overzicht van Azure Lake gegevensarchief](data-lake-store-overview.md).

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Informatie snel met video's?

Bekijk de volgende video's aan de slag met Lake gegevensarchief.

* [Een gegevensarchief Lake-account maken](https://mix.office.com/watch/1k1cycy4l4gen)
* [Gegevens in het gegevensarchief van Lake Verkenner gegevens beheren](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Maak een account Azure Lake gegevensarchief

1. Aanmelden voor de nieuwe [Portal Azure](https://portal.azure.com).

2. Klik op **Nieuw**, klikt u op **gegevens- en**en klik op **Azure Lake gegevensarchief**. Lees de informatie in de blade **Azure Lake gegevensopslag** en klik vervolgens op **maken** in de linkerbenedenhoek van het blad.

3. In het blad **New Lake gegevensarchief** bevatten de waarden zoals aangegeven in de onderstaande schermafbeelding:

    ![Een nieuwe account met Azure Lake gegevensarchief maken] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Een nieuwe Azure gegevens Lake-account maken")

    - **Abonnement**. Selecteer het abonnement waarmee u wilt een nieuw gegevensarchief Lake-account maken.
    - **Resourcegroep**. Selecteer een bestaande resourcegroep of klikt u op het **maken van een resourcegroep** te maken. Een resourcegroep is een container met verwante bronnen voor een toepassing. Zie voor meer informatie de [Resourcegroepen in Azure](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Locatie**: Selecteer een locatie waar u het gegevensarchief Lake-account maken.

4. Selecteer **Pin met Startboard** als u het gegevensarchief Lake account toegankelijk zijn vanuit de Startboard.

5. Klik op **maken**. Als u met de account voor de startboard pin, u terug naar de startboard worden genomen, en ziet u de voortgang van uw Lake gegevensarchief voor het inrichten. Zodra de account Lake gegevensarchief wordt ingericht, weergegeven de blade account.

6. Vouw de **Essentials** -omlaag om te zien dat de informatie over uw account Lake gegevensarchief, zoals de resources groeperen is een onderdeel van, de locatie, enz. Klik op het pictogram **Quick Start** om koppelingen naar andere bronnen die betrekking hebben op het gegevensarchief Lake.

    ![Uw Azure Lake gegevensarchief account] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Uw Azure gegevens Lake account")

## <a name="createfolder"></a>Mappen maken in Azure Lake gegevensarchief account

U kunt mappen maken onder uw account Lake gegevensopslag beheren en opslaan van gegevens.

1. Open de account Lake gegevensarchief dat u zojuist hebt gemaakt. Klik op **Bladeren**in het linkerdeelvenster naar **Lake gegevensopslag**en klik vervolgens op de accountnaam die u wilt maken van mappen van de bladeserver Lake gegevensarchief. Als u de account aan de startboard vastgemaakt, klikt u op die account naast elkaar.

2. In uw account Lake gegevensarchief blade, klikt u op **Gegevens**.

    ![Mappen maken in het gegevensarchief Lake account] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Mappen maken in het gegevensarchief Lake account")

3. In uw account Lake gegevensarchief blade, klikt u op **Nieuwe map**, typ een naam voor de nieuwe map en klik op **OK**.
    
    ![Mappen maken in het gegevensarchief Lake account] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Mappen maken in het gegevensarchief Lake account")
    
    De nieuwe map wordt weergegeven in de blade **Data Explorer** . U kunt geneste mappen tot elk niveau te maken.

    ![Mappen maken in rekening voor meer gegevens] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Mappen maken in rekening voor meer gegevens")


## <a name="uploaddata"></a>Gegevens uploaden naar Azure Lake gegevensarchief rekening

U kunt uw gegevens uploaden naar een gegevensarchief voor Lake Azure rechtstreeks op het hoogste niveau of naar een map die u hebt gemaakt in de account. Volg de stappen voor het uploaden van een bestand naar een submap van de bladeserver **Data Explorer** in de schermafbeelding hieronder. In dit scherm vastleggen, wordt het bestand geüpload naar een submap in de broodkruimels (gemarkeerd in een rood vak) weergegeven.

Als u voorbeeldgegevens wilt uploaden zoekt, krijgt u de map **Data ambulances** uit de [Azure gegevens Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Gegevens uploaden] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Gegevens uploaden")


## <a name="properties"></a>Eigenschappen en acties die beschikbaar zijn op de opgeslagen gegevens

Klik op het toegevoegde bestand te openen, de **Eigenschappen** blade. De eigenschappen die zijn gekoppeld aan het bestand en de acties die u voor het bestand uitvoeren kunt zijn beschikbaar in deze blade. U kunt ook het volledige pad naar het bestand kopiëren in uw account Azure Lake gegevensarchief, gemarkeerd in het rode vak in de onderstaande schermafbeelding.

![Eigenschappen van de gegevens] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Eigenschappen van de gegevens")

* Klik op **voorbeeld** om een voorbeeld van het bestand rechtstreeks vanuit de browser. U kunt de notatie van het voorbeeld ook opgeven. Klik op **voorbeeld**, klikt u op **Opmaak** in het blad **Bestandsvoorbeeld** en geef in de **Bestandsindeling voor de voorvertoning** blade de opties, zoals het aantal rijen wilt weergeven, codering moet worden gebruikt als scheidingsteken gebruiken, enz.

  ![Voorbeeld indeling] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Voorbeeld indeling")

* Klik op **Download** het bestand te downloaden naar uw computer.

* Klik op de **naam van het bestand** om de naam van het bestand.

* Klik op **bestand verwijderen** om het bestand te verwijderen.


## <a name="secure-your-data"></a>Beveilig uw gegevens

U kunt de gegevens die zijn opgeslagen in uw Azure Lake gegevensarchief rekening met Azure Active Directory en toegangsbeheer (ACL's) beveiligen. Voor instructies over hoe u dat doet, Zie [Securing data in Azure Lake gegevensarchief](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Account verwijderen Azure gegevensarchief Lake

Om een account Azure Lake gegevensarchief verwijderen uit het gegevensarchief Lake blade, klikt u op **verwijderen**. Bevestig de actie door u gevraagd naar de naam van de account die u wilt verwijderen. Voer de naam van de account en klik vervolgens op **verwijderen**.

![Account verwijderen gegevens Lake] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Account verwijderen gegevens Lake")


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Diagnostische logboeken voor gegevensopslag Lake](data-lake-store-diagnostic-logs.md)
