<properties
   pageTitle="Query datawarehouse Azure SQL (Visual Studio) | Microsoft Azure"
   description="Query SQL datawarehouse met Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Query Azure SQL datawarehouse (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Visual Studio op query Azure SQL Data Warehouse gebruiken in een paar minuten. Deze methode wordt de uitbreiding van de SQL Server Data Tools (SSDT) in Visual Studio. 

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie, hebt u nodig:

+ Een bestaande SQL-datawarehouse. Zie [een SQL-datawarehouse maken][]om een.
+ SSDT voor Visual Studio. Als u Visual Studio hebt, hebt u al waarschijnlijk dit. Zie de [installatie van Visual Studio en SSDT][]voor installatie-instructies en opties.
+ De volledige SQL server-naam. Dit Zie [verbinding maken met SQL Data Warehouse][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Maak verbinding met uw SQL datawarehouse

1. Open Visual Studio 2013 of 2015.
2. SQL Server-Object Explorer openen. Hiertoe selecteert u **weergave** > **SQL Server Object Explorer**.

    ![SQL Server-Object Explorer][1]

3. Klik op het pictogram van de **SQL-Server toevoegen** .

    ![SQL-Server toevoegen][2]

4. Vul de velden in het venster Server verbinding maken met.

    ![Verbinding maken met Server][3]

    - De **naam van de server**. Voer de **naam van de server** eerder aangeduid.
    - **Verificatie**. Selecteer **SQL Server-verificatie** of **geïntegreerde verificatie van Active Directory**.
    - **Gebruikersnaam** en **wachtwoord**. Voer de gebruikersnaam en wachtwoord als SQL Server-verificatie is geselecteerd.
    - Klik op **verbinden**.

5. Als u wilt verkennen, vouw de Azure SQL server. U kunt de databases die zijn gekoppeld aan de server weergeven. Vouw AdventureWorksDW overzicht van de tabellen in de voorbeelddatabase.

    ![Ontdek de AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. Voer een voorbeeldquery

Nu dat een verbinding met uw database gemaakt is, schrijven we een query.

1. Met de rechtermuisknop op uw database in SQL Server-Object Explorer.

2. Selecteer de **nieuwe Query**. Een nieuwe queryvenster geopend.

    ![Nieuwe query][5]

3. Kopieer deze TSQL query in het query-venster:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. De query uitvoeren. Hiertoe klikt u op de groene pijl of de volgende snelle methode gebruiken: `CTRL` + `SHIFT` + `E`.

    ![Query uitvoeren][6]

5. Bekijk de resultaten van de query. In dit voorbeeld is de tabel FactInternetSales 60398 rijen.

    ![Queryresultaten][7]

## <a name="next-steps"></a>Volgende stappen

Nu kunt u verbinding maken en opvragen, proberen [het visualiseren van gegevens met PowerBI][].

Zie voor uw omgeving configureren voor verificatie van Active Directory Azure, [verifiëren naar SQL Data Warehouse][].

<!--Arcticles-->
[Verbinding maken met SQL datawarehouse]: sql-data-warehouse-connect-overview.md
[Maken van een SQL-datawarehouse]: sql-data-warehouse-get-started-provision.md
[Installatie van Visual Studio en SSDT]: sql-data-warehouse-install-visual-studio.md
[Verificatie met SQL datawarehouse]: sql-data-warehouse-authentication.md
[visualiseren van gegevens met PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
