<properties
    pageTitle="Aan de slag met cross-databasequery's (verticale partities) | Microsoft Azure"   
    description="het gebruik van de elastische databasequery met verticaal gepartitioneerde databases"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Aan de slag met cross-databasequery's (verticale partities) (voorbeeld)

Elastische databasequery (voorbeeld) voor Azure SQL-Database kunt u T-SQL-query's die zich uitstrekken over meerdere databases met één verbindingspunt uitvoeren. Dit onderwerp is van toepassing op [databases verticaal gepartitioneerd](sql-database-elastic-query-vertical-partitioning.md).  

Wanneer voltooid, zal u: informatie over het configureren en gebruiken van een SQL-Database Azure voor het uitvoeren van query's die zich uitstrekken over meerdere databases voor verwante. 

Zie voor meer informatie over de functie elastische database query [Azure SQL Database elastische database query overzicht](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>De voorbeelddatabases maken

Beginnen met, moet twee databases, **klanten** en **Orders**, in dezelfde of een andere logische servers maken.   

De volgende query's uitvoeren op de database **Orders** in de tabel **OrderInformation** maken en invoeren van de voorbeeldgegevens. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Nu, na de query op de database **klanten** aan de tabel **CustomerInformation** maken en invoeren van de voorbeeldgegevens worden uitgevoerd. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Databaseobjecten maken
### <a name="database-scoped-master-key-and-credentials"></a>Database binnen het bereik van hoofdsleutel en referenties

1. Open SQL Server Management Studio of SQL Server Data Tools in Visual Studio.
2. Verbinding maken met de database Orders en de volgende T-SQL-opdrachten uitvoeren:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Username" en "wachtwoord" moet de gebruikersnaam en het wachtwoord dat wordt gebruikt om in te loggen in de database van klanten.
    Verificatie met behulp van Azure Active Directory met elastische query's wordt momenteel niet ondersteund.

### <a name="external-data-sources"></a>Externe gegevensbronnen
Wilt maken in een externe gegevensbron, de volgende opdracht worden uitgevoerd op de database Orders: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externe tabellen
Maak een externe tabel in de database Orders, die overeenkomt met de definitie van de tabel CustomerInformation:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Een monster database elastische T-SQL-query uitvoeren

Als u de externe gegevensbron en de externe tabellen die u nu kunt T-SQL-query uitvoeren op de externe tabellen hebt gedefinieerd. Deze query uitvoeren op de database Orders: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Kosten

Op dit moment is de functie elastische database query opgenomen in de kosten van uw Azure SQL-Database.  

Zie [SQL Database prijzen](/pricing/details/sql-database)voor prijsinformatie. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
