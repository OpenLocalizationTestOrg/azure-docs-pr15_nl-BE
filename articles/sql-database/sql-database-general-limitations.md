<properties
   pageTitle="Richtlijnen en beperkingen voor Azure SQL Database-algemeen"
   description="Deze pagina beschrijft enkele algemene beperkingen voor Azure SQL-Database, alsmede de gebieden van samenwerking en ondersteuning."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>Richtlijnen en beperkingen voor Azure SQL Database-algemeen

Dit onderwerp bevat algemene beperkingen en richtlijnen voor Azure SQL-Database. Zie de [aanvullende informatie](#additional-guidelines) aan het einde van dit onderwerp voor een compleet begrip van de quota, resourcemanagement en ondersteuning.

## <a name="connectivity-and-authentication"></a>Connectiviteit en verificatie

  - Windows-verificatie wordt niet ondersteund. Zie [beheer van Databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md). Azure Active Directory-verificatie wordt echter ondersteund met bepaalde beperkingen. Zie [verbinding maken met een SQL-Database met Azure Active Directory-verificatie](sql-database-aad-authentication.md).

  - Gegevens in tabelvorm stream (TDS) protocol clientversie 7.3 of hoger biedt ondersteuning voor Microsoft Azure SQL-Database.

  - Alleen TCP/IP-verbindingen zijn toegestaan.

  - De SQL Server 2008 SQL Server-browser wordt niet ondersteund omdat Microsoft Azure SQL-Database geen dynamische poorten alleen poort 1433.

## <a name="sql-server-agentjobs"></a>SQL Server Agent-/ taken

Microsoft Azure SQL Database biedt geen ondersteuning voor SQL Server Agent, maar u Elastische taken kunt uit te voeren taken op een groot aantal databases. Zie voor meer informatie over taken elastische [elastische taken](sql-database-elastic-jobs-overview.md).

## <a name="sql-server-collation-support"></a>Ondersteuning voor SQL Server-sortering

De standaardsortering database die wordt gebruikt door Microsoft Azure SQL-Database is **SQL_LATIN1_GENERAL_CP1_CI_AS**, waarbij **LATIN1_GENERAL** Engels (Verenigde Staten), **CP1** is codetabel 1252, **CI** is niet hoofdlettergevoelig en **Accentgevoelig valt** . Het is niet mogelijk de sortering van V12 databases. Zie voor meer informatie over het instellen van de sortering [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Informatie over de vereisten

Bepaalde gebruikersnamen zijn niet toegestaan om veiligheidsredenen. U kunt de volgende namen niet gebruiken:

 - **Admin**
 - **beheerder**
 - **Gast**
 - **hoofdmap**
 - **SA**

Namen voor alle nieuwe objecten moeten voldoen aan de SQL Server-regels voor id's. Zie [id's](https://msdn.microsoft.com/library/ms175874.aspx)voor meer informatie.

Aanmeldings- en gebruikersnamen kunnen niet bevatten ook de \ teken (Windows-verificatie wordt niet ondersteund).

## <a name="additional-guidelines"></a>Aanvullende richtlijnen

- Naast de algemene beperkingen die in dit artikel worden beschreven, heeft SQL-Database specifieke resource quota's en beperkingen op basis van uw **niveau van de service**. Zie voor een overzicht van de niveaus van de service [SQL-Database service tiers](sql-database-service-tiers.md).

- Zie [Azure SQL Database Resource limieten](sql-database-resource-limits.md)voor andere limieten SQL-Database.

- Richtlijnen, Zie [Azure SQL Database beveiligingsrichtlijnen en beperkingen](sql-database-security-guidelines.md)voor beveiliging.

- Een ander verwant gebied rondom de compatibiliteit met Azure SQL-Database met versies van SQL Server, zoals SQL Server 2014 en SQL Server 2016 voor gebouwen. De nieuwste versie van de V12 van Azure SQL-Database heeft veel verbeteringen aangebracht in dit gebied. Voor meer informatie, Zie [Wat is er nieuw in SQL Database V12](sql-database-v12-whats-new.md).

- Zie voor meer informatie over de beschikbaarheid van stuurprogramma's en ondersteuning voor SQL-Database [Verbinding bibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md).
