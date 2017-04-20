<properties
    pageTitle="Excel verbinden met SQL-Database | Microsoft Azure"
    description="Informatie over Microsoft Excel verbinding met Azure SQL-database in de cloud. Gegevens importeren in Excel voor rapportage- en exploratie."
    services="sql-database"
    keywords="verbinding maken met sql excel, excel-gegevens importeren"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Zelfstudie voor SQL-Database: Excel verbinding met een Azure SQL-database en een rapport maken

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Informatie over Excel verbinding met een SQL-database in de cloud, zodat u kunt gegevens importeren en maken van tabellen en grafieken op basis van de waarden in de database. In deze zelfstudie is die u de verbinding tussen Excel en een databasetabel wilt instellen, sla het bestand met de gegevens en de verbindingsinformatie voor Excel en maak een draaitabel uit de databasewaarden.

Voordat u begint, moet je een SQL-database in Azure. Als u er geen hebt, Zie [je eerste SQL-database maken](sql-database-get-started.md) om een database met voorbeeldgegevens omhoog en uitgevoerd in een paar minuten. In dit artikel, zult u voorbeeldgegevens importeren in Excel van dat artikel, maar kunt u vergelijkbare stappen volgen op uw eigen gegevens.

U moet ook een exemplaar van Excel. In dit artikel wordt [Microsoft Excel 2016](https://products.office.com/en-US/).

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Excel verbinden met een SQL-database en een ODC-bestand maken

1.  Voor Excel verbinding met SQL-database, opent Excel en maak een nieuwe werkmap of open een bestaande Excel-werkmap.

2.  Klik op **gegevens**in de menubalk boven aan de pagina naar **Van andere bronnen**en klik vervolgens op **Vanuit SQL Server**.

    ![Gegevensbron selecteren: Excel verbinding met SQL-database.](./media/sql-database-connect-excel/excel_data_source.png)

    De Wizard Gegevensverbinding wordt geopend.

3.  Typ in het dialoogvenster **verbinding maken met Database Server** SQL van de Database **servernaam** u tot stand wilt brengen in het formulier <*servernaam*>**. database.windows.net**. Bijvoorbeeld **adworkserver.database.windows.net**.

4.  Onder **aanmeldingsreferenties**, klikt u op **de volgende gebruikersnaam en het wachtwoord**, typt u de **Gebruikersnaam** en het **wachtwoord** die u instelt voor de Database van SQL server hebt gemaakt en klik op **volgende**.

    ![Typ de naam en de aanmelding referenties](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] Afhankelijk van uw netwerkomgeving mogelijk geen verbinding maken of verliest u mogelijk de verbinding als de Database van SQL server-verkeer van IP-adres van de client niet toegestaan. Ga naar de [Azure portal](https://portal.azure.com/)op SQL-servers, klikt u op de server, klikt u op firewall onder instellingen en toevoegen het IP-adres van de client. Zie [firewall-instellingen configureren](sql-database-configure-firewall-settings.md) voor meer informatie.

5. Selecteer in het dialoogvenster **Database en tabel selecteren** de database die u wilt gebruiken in de lijst en klik vervolgens op tabellen of weergaven die u wilt werken met (Wij hebben gekozen voor **vGetAllCategories**), en klik op **volgende**.

    ![Een database en tabel selecteren.](./media/sql-database-connect-excel/select-database-and-table.png)

    In het dialoogvenster **bestand opslaan en de einddatum** wordt geopend, waar u informatie geven over Office database connection (*.odc) bestand die Excel gebruikt. U laat de standaardinstellingen of aanpassen van uw selecties.

6. U kunt de standaardwaarden laten staan, maar noteer de **Bestandsnaam** in het bijzonder. Een **Beschrijving**, een **Beschrijvende naam**en **Trefwoorden zoeken** u en andere gebruikers onthouden wat u verbinding en de verbinding. Klik op **altijd proberen te gebruiken dit bestand om gegevens te vernieuwen** als u wilt dat verbindingsgegevens in het ODC-bestand opgeslagen, zodat u deze kunt bijwerken wanneer u een verbinding maken en klik vervolgens op **Voltooien**.

    ![Een ODC-bestand opslaan](./media/sql-database-connect-excel/save-odc-file.png)

    Het dialoogvenster **gegevens importeren** wordt weergegeven.

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>De gegevens importeren in Excel en maak een draaitabel
Nu dat u de verbinding tot stand gebracht en het bestand met de gegevens en de verbinding is gemaakt, leest u de gegevens wilt importeren.

1. Klik op de optie die u wilt gebruiken voor de presentatie van de gegevens in het werkblad en klik op **OK**in het dialoogvenster **Gegevens importeren** . Wij hebben gekozen voor **draaigrafiekrapporten**. U kunt ook een **Nieuw werkblad** maken of **deze gegevens aan een Model gegevens**toevoegen. Zie voor meer informatie over Data-modellen [maken een gegevensmodel in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Klik op **Eigenschappen** om te verkennen, informatie over het ODC-bestand dat u in de vorige stap hebt gemaakt en kiest u opties voor het vernieuwen van de gegevens.

    ![Kiezen van de opmaak van gegevens in Excel](./media/sql-database-connect-excel/import-data.png)

    Het werkblad heeft nu een lege draaitabel en grafiek.

8. Selecteer onder **Velden van de draaitabel**alle selectievakjes voor de velden die u wilt weergeven.

    ![Databaserapport configureren.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Als u andere Excel-werkmappen en werkbladen in de database verbinding wilt maken, klikt u op **gegevens**, klikt u op **verbindingen**, klikt u op **toevoegen**, kiest u de verbinding die u hebt gemaakt in de lijst en klik op **openen**.
> ![Een verbinding van een andere werkmap openen](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe u kunt [verbinding maken met een SQL-Database met SQL Server Management Studio](sql-database-connect-query-ssms.md) voor Geavanceerd zoeken en analyse.
- Meer informatie over de voordelen van de [elastische pools](sql-database-elastic-pool.md).
- Meer informatie over het [maken van een webtoepassing die verbinding met de SQL-Database op de back-end maakt](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
