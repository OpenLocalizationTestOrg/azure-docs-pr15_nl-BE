<properties
    pageTitle="SQL-Database zelfstudie: aan de slag met beveiliging"
    description="Informatie over het maken van gebruikersaccounts te openen en om een database te beheren."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>Zelfstudie voor SQL-Database: gebruikersaccounts te openen en te beheren, een database van SQL-database maken


> [AZURE.SELECTOR]
- [Get begonnen zelfstudie](sql-database-get-started-security.md)
- [Toegang verlenen](sql-database-manage-logins.md)

In deze zelfstudie leert u hoe SQL Server Management Studio (SSMS) te gebruiken:

- Aanmelden bij een SQL-Database met behulp van een UPN-niveau van de server-aanmelding.
- Maak een gebruikersaccount voor SQL-Database.
- Een SQL-Database gebruiker [db_owner machtigingen](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0)verlenen.
- Verbinding maken met een SQL-database met een gebruikersaccount die niet een principal niveau van de server.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Volgende stappen
Nu u hebt voltooid deze zelfstudie SQL-Database en een gebruikersaccount gemaakt en machtigingen de gebruiker account dbo, bent u klaar voor meer informatie over [beveiliging voor SQL-Database](sql-database-manage-logins.md).


