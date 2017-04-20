<properties
    pageTitle="SQL-Database ontwikkelen-overzicht | Microsoft Azure"
    description="Meer informatie over de beschikbare verbindingen bibliotheken en aanbevolen procedures voor toepassingen die verbinding maken met een SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>Overzicht SQL Database-ontwikkeling
Dit artikel helpt bij de algemene overwegingen waarmee een ontwikkelaar moet houden bij het schrijven van code met Azure SQL-Database.

## <a name="language-and-platform"></a>Taal en platform
Er zijn voorbeelden van code beschikbaar voor verschillende programmeertalen en -platforms. Hier vindt u koppelingen naar de codevoorbeelden op: 

* Meer informatie: [bibliotheken verbinding voor SQL-Database en SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Beperkingen in de bronnen
Azure SQL-Database beheert de bronnen die beschikbaar zijn in een database met behulp van twee verschillende mechanismen: Resources Governance en handhaving van limieten.

* Meer informatie: [Azure SQL-Database resource limieten](sql-database-resource-limits.md)

## <a name="security"></a>Beveiliging
Azure SQL-Database bevat middelen voor het beperken van toegang, bescherming van gegevens, en de controle van de activiteiten op een SQL-Database.

* Meer informatie: [beveiliging van uw SQL-Database](sql-database-security.md)

## <a name="authentication"></a>Verificatie
* Azure SQL-Database ondersteunt zowel gebruikers van SQL Server-verificatie en -aanmeldingen, alsmede [verificatie van Azure Active Directory](sql-database-aad-authentication.md) -gebruikers en -aanmeldingen.
* U moet een bepaalde database, in plaats van de standaardinstelling van de *master* -database opgeven.
* U niet de instructie Transact-SQL **gebruiken myDatabaseName;** op de SQL-Database gebruiken om te schakelen naar een andere database.
* Meer informatie: [Database SQL security: beveiliging van de database-aanmelding en -instellingen beheren](sql-database-manage-logins.md)

## <a name="resiliency"></a>Tolerantie
Wanneer een tijdelijke fout optreedt tijdens het verbinden met een SQL-Database, moet uw code het gesprek opnieuw.  Het is raadzaam die logica gebruik backoff logica opnieuw zodat deze is de SQL-Database niet overstelpen met meerdere clients tegelijk opnieuw.

* Voorbeelden van code: codevoorbeelden die illustreren logica proberen, vindt u voorbeelden voor de taal van uw keuze op: [bibliotheken verbinding voor SQL-Database en SQL Server](sql-database-libraries.md)
* Meer informatie: [foutberichten voor SQL-Database-clientprogramma's](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Verbindingen beheren
* Overschrijven de standaardtime-out 30 seconden worden in de logica van de client-verbinding.  De standaardwaarde van 15 seconden is te kort voor verbindingen die afhankelijk van het internet zijn.
* Als u een [groep](http://msdn.microsoft.com/library/8xx3tyca.aspx), moet de verbinding op het moment dat het programma is niet actief gebruikt en niet opnieuw wordt voorbereid.

## <a name="network-considerations"></a>Overwegingen bij het netwerk
* Op de computer die fungeert als host voor uw clientprogramma, moet u ervoor zorgen dat de firewall maakt communicatie mogelijk uitgaande TCP op poort 1433.  Meer informatie: [een firewall Azure SQL-Database configureren](sql-database-configure-firewall-settings.md)
* Als uw clientprogramma verbinding maakt met SQL Database V12 terwijl de client wordt uitgevoerd op een Azure VM (virtual machine), moet u bepaalde poortbereiken op de VM te openen. Meer informatie: [behalve 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md)
* Clientverbindingen met Azure SQL Database V12 soms de proxyserver wordt overgeslagen en rechtstreeks communiceren met de database. Poorten dan 1433 steeds belangrijker. Meer informatie: [behalve 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Gegevens Sharding met elastische schaal
Elastische schaal vereenvoudigt het schalen (en). 

* [Ontwerppatronen voor meerdere huurder SaaS-toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)
* [Aan de slag met Azure SQL Database elastische schaal Preview](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Volgende stappen

Ontdek alle [mogelijkheden van SQL-Database](https://azure.microsoft.com/services/sql-database/).
