<properties
   pageTitle="Azure SQL Database beveiligingsrichtlijnen en beperkingen | Microsoft Azure"
   description="Meer informatie over Microsoft Azure SQL-Database-richtlijnen en beperkingen die betrekking hebben op beveiliging."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Richtlijnen voor beveiliging van Azure SQL-Database en beperkingen

Dit onderwerp beschrijft de SQL-Database van Microsoft Azure richtlijnen en beperkingen die betrekking hebben op beveiliging. Overweeg de volgende punten bij het beheren van de beveiliging van uw Azure SQL-Databases.

## <a name="access-to-the-virtual-master-database"></a>Toegang tot de virtuele master database

Normaal gesproken nodig alleen beheerders toegang tot de master-database. Routinematige toegang tot elke gebruikersdatabase moet door de beheerder die gebruikers in elke database gemaakt. Als u databasegebruikers in die, hoeft u niet om aanmeldingen te maken in de hoofddatabase. Zie voor meer informatie [Opgenomen databasegebruikers - maken van uw Database draagbaar](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Firewall

De SQL Server-firewall, die ligt binnen het bereik voor het hele Azure SQL Server is meestal geconfigureerd via de portal en moet alleen de IP-adressen die worden gebruikt door beheerders worden vastgesteld. Verbinding maken met de gebruikersdatabase van een wilt maken van een firewallregel database binnen het bereik dat de vereiste IP-adressen voor elke database wordt geopend, en gebruik [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) Transact-SQL-instructie.

De service Azure SQL-Database is alleen beschikbaar via TCP-poort 1433. Voor toegang tot een SQL-Database van uw computer, moet u ervoor zorgen dat uw firewall client computer kan uitgaande TCP-communicatie via TCP-poort 1433. Als het niet nodig is voor andere toepassingen, blokkeert u binnenkomende verbindingen via TCP-poort 1433. 

Als onderdeel van de verbindingsprocedure verbindingen van Azure virtual machines omgeleid naar een ander IP-adres en poort, die uniek is voor elke functie van de werknemer. Het nummer van de poort is in het bereik van 11000 tot 11999. Voor meer informatie over TCP-poorten Zie [poorten dan 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Verificatie

Active Directory-verificatie gebruiken (geïntegreerde beveiliging) indien mogelijk. Zie [verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md)en [een andere modus te kiezen](https://msdn.microsoft.com/library/ms144284.aspx) in SQL Server Books Online voor meer informatie over het configureren van AD-verificatie. 

Gebruik die gebruikers van de schaalbaarheid verbeteren. Zie voor meer informatie [Opgenomen databasegebruikers - maken van uw Database draagbare](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx)en [Databases opgenomen](https://technet.microsoft.com/library/ff929071.aspx).

De Database-Engine wordt gesloten verbindingen die meer dan 30 minuten inactief. De verbinding opnieuw inloggen moet voordat deze kan worden gebruikt.

Voortdurend verbindingen met SQL-Database actief reauthorization (uitgevoerd door de Database-Engine) moeten ten minste om 10 uur. De Database-Engine probeert met de oorspronkelijk ingediende wachtwoord reauthorization en geen invoer van de gebruiker is vereist. Voor betere prestaties wanneer u een wachtwoord opnieuw is ingesteld in de SQL-Database, de verbinding is niet geverifieerd, zelfs als de verbinding wordt hersteld door het groeperen van verbindingen. Dit verschilt van het gedrag van de SQL-Server voor gebouwen. Als het wachtwoord is gewijzigd sinds de verbinding voor het eerst is toegestaan, moet de verbinding worden verbroken en een nieuwe verbinding tot stand gebracht met het nieuwe wachtwoord. Een gebruiker met de machtiging DATABASEVERBINDING KILL beëindigen een verbinding met een SQL-Database met behulp van de opdracht [KILL](https://msdn.microsoft.com/library/ms173730.aspx) expliciet.

## <a name="logins-and-users"></a>Aanmeldingen en gebruikers

Bij het beheren van aanmeldingen en gebruikers in een SQL-Database, zijn er beperkingen.


- U moet verbinding maken met de **master** -database bij het uitvoeren van de ``CREATE/ALTER/DROP DATABASE`` instructies. -De databasegebruiker in de hoofddatabase overeenkomt met het niveau van de server principal aanmelding kan niet worden gewijzigd of verwijderd. 
- Amerikaans-Engels is de standaardtaal van de principal-niveau van de server-aanmelding.
- Alleen de beheerders (principal login niveau van de server of de beheerder Azure AD) en de leden van de databaserol **dbmanager** in **de hoofddatabase** machtigingen hebben voor het uitvoeren van de ``CREATE DATABASE`` en ``DROP DATABASE`` instructies.
- U moet verbinding maken met de master-database bij het uitvoeren van de ``CREATE/ALTER/DROP LOGIN`` instructies. Echter met behulp van aanmeldingen wordt afgeraden. Gebruik in plaats daarvan databasegebruikers in die.
- Verbinding van een gebruiker-database, moet u de naam van de database in de verbindingsreeks opgeven.
- Alleen de belangrijkste login niveau van de server en de leden van de databaserol **loginmanager** in **de hoofddatabase** machtiging uitvoeren hebben de ``CREATE LOGIN``, ``ALTER LOGIN``, en ``DROP LOGIN`` instructies.
- Bij het uitvoeren van de ``CREATE/ALTER/DROP LOGIN`` en ``CREATE/ALTER/DROP DATABASE`` -instructies in een toepassing ADO.NET geparametriseerde opdrachten niet is toegestaan. Zie voor meer informatie, [opdrachten en Parameters](https://msdn.microsoft.com/library/ms254953.aspx).
- Bij het uitvoeren van de ``CREATE/ALTER/DROP DATABASE`` en ``CREATE/ALTER/DROP LOGIN`` -instructies, elk van deze instructies moet de enige in een batch Transact-SQL-instructie. Anders treedt er een fout op. De volgende Transact-SQL controleert bijvoorbeeld of de database bestaat. Indien aanwezig, een ``DROP DATABASE`` instructie aangeroepen om de database te verwijderen. Omdat de ``DROP DATABASE`` instructie is niet de enige instructie in de batch, het uitvoeren van de volgende Transact-SQL-instructie resulteert in een fout.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Bij het uitvoeren van de ``CREATE USER`` -instructie met de ``FOR/FROM LOGIN`` optie, de enige in een batch Transact-SQL-instructie moet worden.
- Bij het uitvoeren van de ``ALTER USER`` -instructie met de ``WITH LOGIN`` optie, de enige in een batch Transact-SQL-instructie moet worden.
- Op ``CREATE/ALTER/DROP`` , moet een gebruiker de ``ALTER ANY USER`` machtiging voor de database.
- Als de eigenaar van de databaserol van een probeert toe te voegen of te verwijderen van een andere gebruiker naar of uit de databaserol, het volgende foutbericht worden weergegeven: **gebruiker of rol 'Naam' bestaat niet in deze database.** Deze fout treedt op omdat de gebruiker niet zichtbaar voor de eigenaar is. Verlenen dit probleem oplossen door de roleigenaar van de van de ``VIEW DEFINITION`` machtiging voor de gebruiker. 

Zie voor meer informatie over aanmeldingsnamen en gebruikers [beheren, databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md).


## <a name="see-also"></a>Zie ook

[Azure SQL Database Firewall](sql-database-firewall-configure.md)

[Procedure: Firewall-instellingen (Azure SQL Database) configureren](sql-database-configure-firewall-settings.md)

[Beheer van databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md)

[Security Center voor SQL Server-Database-Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)
