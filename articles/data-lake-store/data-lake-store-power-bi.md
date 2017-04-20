<properties
   pageTitle="Gegevens in het gegevensarchief Lake analyseren met behulp van Power BI | Microsoft Azure"
   description="Power BI gebruiken voor het analyseren van gegevens die zijn opgeslagen in Azure Lake gegevensarchief"
   services="data-lake-store" 
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Gegevens in het gegevensarchief Lake analyseren met behulp van Power BI

In dit artikel leert u het gebruik van Power BI bureaublad te analyseren en gegevens die zijn opgeslagen in Azure Lake gegevensopslag te visualiseren.

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Lake gegevensarchief account**. Volg de instructies op de [aan de slag met Azure Lake gegevensarchief maken met de Portal Azure](data-lake-store-get-started-portal.md). In dit artikel wordt ervan uitgegaan dat u hebt al een account gegevensarchief Lake, **mybidatalakestore**, genoemd gemaakt en geüpload een voorbeeldbestand dat u (**Drivers.txt**) aan. In dit voorbeeld van een bestand is gedownload van [Azure gegevens Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).

- **Power BI-bureaublad**. Dit kunt u downloaden van het [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 


## <a name="create-a-report-in-power-bi-desktop"></a>Een rapport maken in Power BI-Desktop

1. Power BI bureaublad starten op de computer.

2. In het lint **Start** , klikt u op **Gegevens ophalen**en klik vervolgens op meer. Klik op **Azure**in het dialoogvenster **Gegevens ophalen** naar **Azure Lake gegevensopslag**en klik vervolgens op **verbinden**.

    ![Verbinding maken met een gegevensopslag Lake] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Verbinding maken met een gegevensopslag Lake")

3. Als er een dialoogvenster met de connector wordt in een ontwikkelingsfase, ervoor kiezen om door te gaan.

4. De URL naar uw account gegevensarchief Lake in het dialoogvenster **Microsoft Azure gegevensarchief Lake** en klik op **OK**.

    ![URL voor gegevensopslag Lake] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL voor gegevensopslag Lake")

5. In het volgende dialoogvenster, klikt u op **aanmelden** Lake gegevensarchief rekening ondertekenen. U wordt omgeleid naar de aanmeldingspagina van uw organisatie. Volg de aanwijzingen in de account aanmelden.

    ![Aanmelden bij Lake gegevensarchief] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Aanmelden bij Lake gegevensarchief")

6. Nadat u zich heeft aangemeld, klikt u op **verbinding maken**.

    ![Verbinding maken met een gegevensopslag Lake] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Verbinding maken met een gegevensopslag Lake")

7. Het volgende dialoogvenster ziet u het bestand dat u geüpload naar uw account Lake gegevensarchief. Controleer of de info en klik vervolgens op **laden**.

    ![Gegevens laden van Lake gegevensarchief] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Gegevens laden van Lake gegevensarchief")

8. Nadat de gegevens is geladen in Power BI, ziet u de volgende velden op het tabblad **velden** .

    ![Velden geïmporteerd] (./media/data-lake-store-power-bi/imported-fields.png "Velden geïmporteerd")

    Echter om te visualiseren en analyseren van de gegevens, wij geven de voorkeur aan de gegevens worden per de volgende velden beschikbaar

    ![Gewenste velden] (./media/data-lake-store-power-bi/desired-fields.png "Gewenste velden")

    In de volgende stappen wordt de query om te zetten van de geïmporteerde gegevens in de gewenste indeling bijgewerkt.

9. Klik op **Query bewerken**in het lint **Start** .

    ![Query's bewerken] (./media/data-lake-store-power-bi/edit-queries.png "Query's bewerken")

10. In de Query Editor onder de kolom **inhoud** , klik op **binair**.

    ![Query's bewerken] (./media/data-lake-store-power-bi/convert-query1.png "Query's bewerken")

11. Er verschijnt een bestandspictogram van een met het **Drivers.txt** -bestand dat u hebt geüpload. Klik met de rechtermuisknop op het bestand en klik op **CSV**.  

    ![Query's bewerken] (./media/data-lake-store-power-bi/convert-query2.png "Query's bewerken")

12. U ziet een output zoals hieronder wordt weergegeven. Uw gegevens zijn nu beschikbaar in een indeling die u gebruiken kunt voor het maken van visualisaties.

    ![Query's bewerken] (./media/data-lake-store-power-bi/convert-query3.png "Query's bewerken")

13. Klik op **sluiten en toepassen**in het lint **Start** en klik op **sluiten en toepassen**.

    ![Query's bewerken] (./media/data-lake-store-power-bi/load-edited-query.png "Query's bewerken")

14. Wanneer de query wordt bijgewerkt, wordt de nieuwe velden beschikbaar voor visualisatie in het tabblad **velden** weergegeven.

    ![Bijgewerkte velden] (./media/data-lake-store-power-bi/updated-query-fields.png "Bijgewerkte velden")

15. We maken een cirkeldiagram om aan te geven van de stuurprogramma's in elke plaats voor een bepaald land. Hiertoe de volgende selecties te maken.

    1. Klik op het symbool voor een cirkeldiagram van het tabblad visualisaties.

        ![Cirkeldiagram maken] (./media/data-lake-store-power-bi/create-pie-chart.png "Cirkeldiagram maken")

    2. De kolommen die we gaan zijn **kolom 4** (naam van de plaats) en **kolom 7** (naam van het land). Deze kolommen op het tabblad **velden** sleept naar tabblad **Visualisaties** zoals hieronder wordt weergegeven.

        ![Visualisaties maken] (./media/data-lake-store-power-bi/create-visualizations.png "Visualisaties maken")

    3. Het cirkeldiagram moet er nu ongeveer zo zoals hieronder wordt weergegeven.

        ![Cirkeldiagram] (./media/data-lake-store-power-bi/pie-chart.png "Visualisaties maken")

16. Als u een bepaald land van de pagina op filters, nu ziet u het aantal stuurprogramma's in elke plaats van het geselecteerde land. Selecteer bijvoorbeeld onder het tabblad **Visualisaties** onder **paginaniveau filters**, **Brazilië**.

    ![Selecteer een land] (./media/data-lake-store-power-bi/select-country.png "Selecteer een land")

17. Het cirkeldiagram wordt automatisch bijgewerkt zodat de stuurprogramma's in de steden van Brazilië.

    ![Stuurprogramma's in een land] (./media/data-lake-store-power-bi/driver-per-country.png "Stuurprogramma's per land")

18. Klik in het menu **bestand** op **Opslaan** om de visualisatie opslaan als een bestand Power BI bureaublad.

## <a name="publish-report-to-power-bi-service"></a>Rapport publiceren naar Power BI-service

Nadat u de visualisaties in Power BI bureaublad hebt gemaakt, kunt u deze met anderen delen door deze te publiceren naar de Power BI-service. Zie voor instructies over hoe u dat doet, [publiceren van Power BI-bureaublad](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Zie ook

* [Gegevens analyseren in Lake gegevensarchief gegevens Lake Analytics gebruiken](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
