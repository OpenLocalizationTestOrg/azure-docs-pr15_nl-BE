<properties
   pageTitle="Gegevens van Analytics Stream streamen naar Lake gegevensarchief | Azure"
   description="Azure Stream Analytics voor stream-gegevens gebruiken in Azure Lake gegevensarchief"
   services="data-lake-store,stream-analytics" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Stream-gegevens van Azure de Blob Storage in Lake gegevensarchief Azure Stream Analytics gebruiken

In dit artikel leert u het gebruik van Azure Lake gegevensarchief als een uitvoer voor een project Azure Stream Analytics. In dit artikel ziet u een eenvoudige scenario dat gegevens leest uit een blob Storage Azure (input) en schrijft de gegevens naar Lake gegevensarchief (uitvoer).

>[AZURE.NOTE] Op dit moment wordt maken en configuratie van outputs Lake gegevensarchief voor Analytics stroom alleen ondersteund in de [Klassieke Azure-Portal](https://manage.windowsazure.com). Sommige onderdelen van deze zelfstudie gebruiken daarom de klassieke Azure-Portal.

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Uw abonnement op Azure inschakelen** voor gegevens Lake winkel Public Preview. Zie de [instructies](data-lake-store-get-started-portal.md#signup).

- **Azure opslag account**. U kunt een blob-container van deze account gegevens voor een taak Stream Analytics. Deze zelfstudie, wordt ervan uitgegaan dat u een opslag-account de naam **datalakestoreasa** en een container in de account **datalakestoreasacontainer**genoemd. Wanneer u de container hebt gemaakt, een voorbeeldbestand dat u aan het uploaden. U krijgt een voorbeeldbestand dat u uit de [Azure gegevens Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Verschillende clients, zoals [Azure Opslagverkenner](http://storageexplorer.com/), kunt u het uploaden van gegevens naar een blob-container.

    >[AZURE.NOTE] Als u de account vanaf de Portal Azure maken, moet dat u deze maken met de **klassieke** implementatiemodel. Dit zorgt ervoor dat de account van de opslag kan worden geopend vanuit de klassieke Portal Azure omdat wij gebruik maken van een taak Stream Analytics. Zie voor instructies voor het maken van een account voor de opslag van de Azure Portal met de implementatie van de klassieke [een Azure opslag account maken](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Of kunt u een account voor de opslag van de klassieke Azure-Portal.

- **Azure Lake gegevensarchief account**. Volg de instructies op de [aan de slag met Azure Lake gegevensarchief maken met de Portal Azure](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics taak maken

U begint met het maken van een Stream Analytics-project met een invoerbron en een bestemming voor de uitvoer. Voor deze zelfstudie is de bron een Azure blob-container en de bestemming Lake gegevensarchief.

1. Aanmelden bij de [Azure klassieke Portal](https://manage.windowsazure.com).

2. In de linkerbenedenhoek van het scherm, klikt u op **Nieuw**, **Data Services**, **Stream Analytics**, **Snel maken**. De waarden zoals hieronder wordt weergegeven en klik vervolgens op **Stroom Analytics-taak maken**.

    ![Een Stream Analytics taak maken] (./media/data-lake-store-stream-analytics/create.job.png "Een Stream Analytics taak maken")

## <a name="create-a-blob-input-for-the-job"></a>Een Blob-invoer voor het project maken

1. Open de pagina voor de Stream Analytics taak, klikt u op het tabblad **invoer** en klik op **een ingang toevoegen** om een wizard te starten.

2. **Stroom gegevens**selecteren en klikt u vervolgens op de pijl naar rechts op de pagina **invoer aan uw project toevoegen** .

    ![Invoer aan uw project toevoegen] (./media/data-lake-store-stream-analytics/create.input.1.png "Invoer aan uw project toevoegen")

3. **Blob-opslag**selecteren en klikt u vervolgens op de pijl naar rechts op de pagina **een gegevensstroom aan uw project toevoegen** .

    ![Een gegevensstroom aan de taak toevoegen] (./media/data-lake-store-stream-analytics/create.input.2.png "Een gegevensstroom aan de taak toevoegen")

4. Geef details voor de blobopslag die u als gegevensbron voor de invoer gebruiken wilt op de pagina **instellingen van Blob-opslag** .

    ![De blob storage-instellingen kan leveren] (./media/data-lake-store-stream-analytics/create.input.3.png "De blob storage-instellingen kan leveren")

    * **Enter een alias invoer**. Dit is een unieke naam die u voor de taak invoeren opgeeft.
    * **Selecteer een account voor opslag**. Controleer of de opslag account zich in hetzelfde gebied, als de taak Stream Analytics of u extra kosten van het verplaatsen van gegevens tussen de regio's zal oplopen.
    * **Geef een opslag container**. U kunt een nieuwe container maken of Selecteer een bestaande container.

    Klik op de pijl naar rechts.

5. De indeling serialisatie als **CSV**, scheidingsteken als **tabblad**als **UTF8**-codering instellen en klik op het vinkje op de pagina **Instellingen voor serialisatie** .

    ![De instellingen voor serialisatie] (./media/data-lake-store-stream-analytics/create.input.4.png "De instellingen voor serialisatie")

6. Als u klaar bent met de wizard de blob-invoer op het tabblad **invoer** worden toegevoegd en **OK**in de kolom van de **diagnose** moet worden weergegeven. U kunt ook expliciet de verbinding met de invoer met behulp van de knop **Verbinding testen** onder testen.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Een gegevensarchief Lake uitvoer voor het project maken

1. Open de pagina voor de Stream Analytics taak, klikt u op het tabblad **uitvoer** en klik op **uitvoer toevoegen** om een wizard te starten.

2. **Lake gegevensarchief**selecteren en klikt u vervolgens op de pijl naar rechts op de pagina **een vermogen aan uw project toevoegen** .

    ![Een vermogen aan uw project toevoegen] (./media/data-lake-store-stream-analytics/create.output.1.png "Een vermogen aan uw project toevoegen")

3. Op de pagina **verbinding machtigen** als u al een gegevensarchief Lake-account hebt gemaakt, klikt u op **Nu toestemming geven**. Klik op **Nu aanmelden** om een nieuwe account maken. Deze zelfstudie laat ons wordt ervan uitgegaan dat er al een gegevensarchief Lake-account (zoals vermeld in de vereiste) gemaakt. U wordt automatisch gemachtigd met de referenties waarmee u in de klassieke Portal Azure ondertekend.

    ![Lake gegevensarchief toestaan] (./media/data-lake-store-stream-analytics/create.output.2.png "Lake gegevensarchief toestaan")

4. Voer op de pagina **Instellingen voor gegevens Lake winkel** de gegevens in de onderstaande schermafbeelding.

    ![Instellingen opgeven Lake gegevensarchief] (./media/data-lake-store-stream-analytics/create.output.3.png "Instellingen opgeven Lake gegevensarchief")

    * **Enter een uitvoeralias**. Dit is een unieke naam die u voor de uitvoer van de taak opgeeft.
    * **Geef een gegevensarchief Lake-account**. U moet al hebt gemaakt, zoals vermeld in de vereiste.
    * **Geef een pad prefix patroon**. Dit is vereist voor het identificeren van de uitvoerbestanden naar Lake gegevensopslag worden geschreven door de taak Stream Analytics. Omdat de titels van outputs geschreven door de taak in de indeling van een GUID zijn, helpt met inbegrip van een voorvoegsel identificeren van de uitvoer geschreven. Als u wilt gebruiken in een datum- en tijdstempel van het prefix, zorg ervoor dat u `{date}/{time}` in het voorvoegselpatroon. Als u deze opneemt, de velden **Datum **- en **Tijdnotatie** zijn ingeschakeld en kunt u de indeling van de keuze.

    Klik op de pijl naar rechts.

5. De indeling serialisatie als **CSV**, scheidingsteken als **tabblad**als **UTF8**-codering instellen en klik op het vinkje op de pagina **Instellingen voor serialisatie** .

    ![Geef de uitvoerindeling] (./media/data-lake-store-stream-analytics/create.output.4.png "Geef de uitvoerindeling")

6. Zodra u klaar bent met de wizard, wordt de uitvoer Lake gegevensarchief toegevoegd onder het tabblad **uitvoer** en **OK**in de kolom van de **diagnose** moet worden weergegeven. U kunt ook expliciet de verbinding met de uitvoer met behulp van de knop **Verbinding testen** onder aan het testen.

## <a name="run-the-stream-analytics-job"></a>Voer de Stream Analytics.

Een Stream Analytics als taak wilt uitvoeren, moet u een query uitvoeren op het tabblad Query. Voor deze zelfstudie kunt u de voorbeeldquery uitvoeren door de tijdelijke aanduidingen te vervangen door de taak invoer en uitvoer van aliassen, zoals in de onderstaande schermafbeelding.

![Query uitvoeren] (./media/data-lake-store-stream-analytics/run.query.png "Query uitvoeren")

Klik op **Opslaan** vanaf de onderkant van het scherm en klik vervolgens op **starten**. In het dialoogvenster **Aangepaste tijd**selecteren en vervolgens een datum selecteren in het verleden, zoals **1/1/2016**. Klik op het selectievakje start de taak. Het kan een paar minuten duren om de taak te starten.

![Taaktijd instellen] (./media/data-lake-store-stream-analytics/run.query.2.png "Taaktijd instellen")

Nadat de taak is gestart, klikt u op het tabblad **Monitor** om te zien hoe de gegevens is verwerkt.

![Monitor taak] (./media/data-lake-store-stream-analytics/run.query.3.png "Monitor taak")

Ten slotte kunt u de [Portal Azure](https://portal.azure.com) uw account gegevens Lake winkel openen en controleren of de gegevens naar de rekening werd geschreven.

![Controleer of uitvoer] (./media/data-lake-store-stream-analytics/run.query.4.png "Controleer of uitvoer")

Klik in het deelvenster Data Explorer merk op dat de uitvoer wordt geschreven naar een map, zoals opgegeven in het gegevensarchief Lake uitvoerinstellingen (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Zie ook

* [Een cluster van HDInsight voor het gebruik van Lake gegevensarchief maken](data-lake-store-hdinsight-hadoop-use-portal.md)
