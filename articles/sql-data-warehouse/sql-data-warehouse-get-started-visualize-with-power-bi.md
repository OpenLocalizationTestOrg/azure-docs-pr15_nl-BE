<properties
   pageTitle="Gegevens met Power BI Microsoft Azure SQL Data Warehouse"
   description="Gegevens met Power BI SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Gegevens met Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

In deze zelfstudie wordt beschreven hoe u met Power BI kunt u verbinding maken met SQL Data Warehouse en enkele elementaire visualisaties te maken.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Vereisten

Stap in deze zelfstudie, wilt u het volgende nodig:

- Een datawarehouse SQL vooraf geladen met de AdventureWorksDW-database. Om deze bepaling Zie [maken een datawarehouse SQL][] en kiest u de voorbeeldgegevens worden geladen. Als u al een datawarehouse maar geen voorbeeldgegevens hebt, kunt u de [voorbeeldgegevens handmatig laden][].


## <a name="1-connect-to-your-database"></a>1. Maak verbinding met de database

Power BI openen en verbinding maken met uw database AdventureWorksDW:

1. Inloggen op de [portal Azure][].
2. Klik op **SQL-databases** en kies uw AdventureWorks SQL Data Warehouse-database.

    ![Zoek de database][1]

3. Klik op de knop 'Openen in Power BI'.

    ![BI-knop][2]

4. U ziet nu de pagina SQL Data Warehouse verbinding het webadres van uw database weergeven. Klik op volgende.

    ![Stroomaansluiting BI][3]

6. Voer uw Azure SQL server-gebruikersnaam en wachtwoord en u wordt volledig verbonden met uw SQL Data Warehouse-database.

    ![Power BI inloggen][4]

7. Zodra je je hebt aangemeld in Power BI, klikt u op de dataset AdventureWorksDW op de linker blade. De database wordt geopend.

    ![Power BI open AdventureWorksDW][5]



## <a name="2-create-a-report"></a>2. een rapport maken

U bent nu klaar voor gebruik Power BI voor het analyseren van uw voorbeeldgegevens AdventureWorksDW. AdventureWorksDW heeft voor het uitvoeren van de analyse, een weergave met de naam AggregateSales. Deze weergave bevat enkele van de belangrijkste cijfers voor het analyseren van de verkoop van het bedrijf.

1. U maakt een plattegrond van het verkoopbedrag op postcode, in het deelvenster velden rechts, op de AggregateSales weergave uit te vouwen. Klik op de postcode en de totale omzet kolommen te selecteren.

    ![Power BI AggregateSales selecteren][6]

    Power BI herkent automatisch deze geografische gegevens in een map voor u plaatsen.

    ![Power BI-kaart][7]

2. Deze stap maakt een staafdiagram waarin de omzet per klant inkomsten. Dit gaat aan de uitgevouwen weergave van AggregateSales te maken. Klik in het veld Omzetbedrag. Sleep het veld klant inkomsten naar links en zet deze neer in de as.

    ![Power BI Selecteer as][8]

    We het staafdiagram links geplaatst.

    ![Power BI-balk][9]

3. Deze stap maakt een lijndiagram met een verkoopbedrag per datum bevat. Dit gaat aan de uitgevouwen weergave van AggregateSales te maken. Klik op de totale omzet en de orderdatum. In de visualisaties kolom klikt u op het pictogram lijndiagram. Dit is het eerste pictogram in de tweede regel onder visualisaties.

    ![Power BI Selecteer lijndiagram][10]

    U hebt nu een rapport met drie verschillende visualisaties van de gegevens.

    ![Power BI-regel][11]

Op elk gewenst moment kunt u uw voortgang opslaan op **bestand** en selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen
Nu dat we hebt u enige tijd met de voorbeeldgegevens opgewarmd, Zie hoe te [ontwikkelen][], te [laden][]of te [migreren][]. Of kijk eens naar de [website van de Power BI][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[voorbeeldgegevens handmatig laden]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Maken van een SQL-datawarehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI-website]: http://www.powerbi.com/
