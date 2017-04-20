<properties 
    pageTitle="Behalve 1433 voor SQL-Database | Microsoft Azure"
    description="Verbindingen van ADO.NET met Azure SQL Database V12 soms de proxyserver wordt overgeslagen en rechtstreeks communiceren met de database. Poorten dan 1433 steeds belangrijker."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Behalve 1433 voor ADO.NET 4.5 en SQL Database V12


Dit onderwerp beschrijft de wijzigingen die Azure SQL Database V12 brengt op het Verbindingsgedrag van clients die gebruikmaken van ADO.NET 4.5 of hoger.


## <a name="v11-of-sql-database-port-1433"></a>V11: van SQL-Database: poort 1433


Wanneer het clientprogramma ADO.NET 4.5 verbinding te maken en een query met een SQL-Database V11: gebruikt, wordt is de interne volgorde als volgt uit:


1. ADO.NET probeert verbinding te maken met SQL-Database.

2. ADO.NET poort 1433 wordt gebruikt om aan te roepen van een module middleware en de middleware maakt verbinding met de SQL-Database.

3. SQL-Database stuurt een antwoord terug naar de middleware die het antwoord naar ADO.NET naar poort 1433 stuurt.


**Terminologie:** De voorgaande reeks worden beschreven door te zeggen dat ADO.NET met de SQL-Database communiceert met behulp van de *proxy-route*. Als geen middleware betrokken waren, zouden we zeggen dat de *rechtstreekse route* is gebruikt.


## <a name="v12-of-sql-database-outside-vs-inside"></a>V12 van SQL-Database: buiten de vs binnen


Voor verbindingen met V12, moeten we vragen of uw clientprogramma wordt uitgevoerd *binnen* of *buiten* de rand van de Azure cloud. De subsecties besproken twee algemene scenario's.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Buiten:* Client wordt uitgevoerd op uw computer


Poort 1433 is de enige poort die moet zijn geopend op uw computer die fungeert als host voor uw clienttoepassing SQL-Database.


#### <a name="inside-client-runs-on-azure"></a>*In:* Client wordt uitgevoerd op Azure


Wanneer de client wordt uitgevoerd binnen de grens van Azure cloud, wordt wat wij een *rechtstreekse route* om te communiceren met de Database van SQL server kunnen bellen. Nadat u een verbinding tot stand is gebracht, verdere inhouden interactie tussen de client en de database geen proxy middleware.


De volgorde is als volgt:


1. ADO.NET 4,5 (of later) een korte interactie met de Azure cloud Start en ontvangt een poortnummer dynamisch geïdentificeerd.
 - Het poortnummer dynamisch geïdentificeerd is in het bereik van 11000 11999 of 14000 14999.

2. ADO.NET vervolgens verbindt met de Database van SQL server rechtstreeks met geen middleware ertussen.

3. Query's rechtstreeks naar de database verzonden en de resultaten zijn direct naar de client.


Ervoor te zorgen dat de bereiken van 11000 11999 en 14000-14999 op de clientmachine Azure beschikbaar voor ADO.NET 4.5 client interactie met SQL-Database V12 blijven poort.

- Poorten in het bereik moet met name vrij van andere uitgaande upblokkeringsprogramma's.

- Op de Azure VM bepaalt de **Windows Firewall met geavanceerde beveiliging** de poortinstellingen.
 - De [gebruikersinterface van de firewall](http://msdn.microsoft.com/library/cc646023.aspx) kunt u een regel waarvoor u het **TCP-** protocol met een bereik van poorten met de syntaxis zoals **11000 11999 opgeven**toevoegen.


## <a name="version-clarifications"></a>Toelichting versie


In deze sectie wordt uitleg gegeven over de bijnamen die naar versies van het product verwijzen. De lijst bevat ook enkele koppelingen tussen versies tussen producten.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 ondersteunt het protocol TDS 7.3, maar geen 7.4.
- ADO.NET 4.5 en hoger ondersteunt de 7.4 TDS-protocol.


#### <a name="sql-database-v11-and-v12"></a>V11: SQL-Database en V12


De client verbinding verschillen tussen V11: SQL-Database en V12 worden in dit onderwerp gemarkeerd.


*Opmerking:* De Transact-SQL-instructie `SELECT @@version;` geeft als resultaat een waarde die beginnen met een getal bijvoorbeeld '11'. of "12." en die overeenkomen met de versienamen van onze van V11: en V12 voor SQL-Database.


## <a name="related-links"></a>Verwante koppelingen


- ADO.NET 4.6 werd uitgebracht op 20 juli 2015. Een blog aankondiging van het .NET team is beschikbaar [hier](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4.5 werd uitgebracht op 15 augustus 2012. Een blog aankondiging van het .NET team is beschikbaar [hier](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Een blog over ADO.NET 4.5.1 vindt u [hier](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [Lijst van TDS-protocol versie](http://www.freetds.org/userguide/tdshistory.htm)


- [Overzicht SQL Database-ontwikkeling](sql-database-develop-overview.md)


- [Azure SQL-Database-firewall](sql-database-firewall-configure.md)


- [Procedure: firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md)

