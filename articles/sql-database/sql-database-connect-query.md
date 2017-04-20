<properties
    pageTitle="Verbinding maken met een SQL-Database met een C#-query | Microsoft Azure"
    description="Een programma geschreven in C# en verbinding maken met een SQL-database. Info over IP-adressen, verbindingsreeksen beveiligde aanmelding en vrije Visual Studio."
    services="sql-database"
    keywords="c# databasequery query c# verbinding maken met database, SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Verbinding maken met een SQL-Database met Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Informatie over verbinding maken met een Azure SQL-database in Visual Studio. 

## <a name="prerequisites"></a>Vereisten


Als u verbinding maakt met een SQL-database met behulp van Visual Studio, wilt nodig u het volgende: 


- Een SQL-database verbinding maken met. In dit artikel wordt de **AdventureWorks** -voorbeelddatabase. Zie [de demodatabase maken](sql-database-get-started.md)de AdventureWorks-voorbeelddatabase.


- Visual Studio 2013 update 4 (of hoger). Microsoft biedt nu Visual Studio Community voor *gratis*.
 - [Download van Visual Studio-Gemeenschap,](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Meer opties voor vrije Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Visual Studio openen vanaf de portal Azure


1. Log in op de [Azure portal](https://portal.azure.com/).

2. Klik op **Meer Services** > **SQL-databases**
3. Open de database **AdventureWorks** blade te zoeken en te klikken op de *AdventureWorks* -database.

6. Klik op de knop **Extra** boven aan de database blade:

    ![Nieuwe query. Verbinding maken met een Database van SQL server: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Klik op **openen in Visual Studio** (als u Visual Studio, klikt u op de downloadkoppeling):

    ![Nieuwe query. Verbinding maken met een Database van SQL server: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio wordt geopend met het venster **verbinding maken met Server** is al ingesteld op verbinding maken met de server en de database die u hebt geselecteerd in de portal.  (Klik op **Opties** om te controleren of de verbinding met de juiste database is ingesteld). Typ uw server admin-wachtwoord en klik op **verbinding maken**.


    ![Nieuwe query. Verbinding maken met een Database van SQL server: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Als u nog geen firewallregel die is ingesteld voor het IP-adres van uw computer, krijgt u een bericht *kan niet verbinden* hier. Zie [een SQL-Database Azure serverniveau firewallregel configureren](sql-database-configure-firewall-settings.md)een firewallregel maken.


9. Nadat de verbinding tot stand komt, wordt het **SQL Server-Object Explorer** -venster geopend met een verbinding met de database.

    ![Nieuwe query. Verbinding maken met een Database van SQL server: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Een steekproef uitvoeren.

Nu dat we met de database verbonden bent, de volgende stappen wordt aangegeven hoe een eenvoudige query uit te voeren:

2. Klik met de rechtermuisknop op de database en selecteer vervolgens een **Nieuwe Query**.

    ![Nieuwe query. Verbinding maken met een Database van SQL server: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Kopieer en plak de volgende code in het queryvenster.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Klik op de knop **uitvoeren** als de query wilt uitvoeren:

    ![Succes. Verbinding maken met een Database van SQL server: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Volgende stappen

- SQL Server Data Tools maakt gebruik van SQL-databases openen in Visual Studio. Zie [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx)voor meer informatie.
- Als u verbinding maakt met een SQL-database met behulp van code, Zie [verbinding maken met de SQL-Database met behulp van .NET (C#)](sql-database-develop-dotnet-simple.md).



