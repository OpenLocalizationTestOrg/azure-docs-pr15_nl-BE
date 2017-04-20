<properties
   pageTitle="Beheren van databases in Azure SQL Data Warehouse | Microsoft Azure"
   description="Overzicht van het beheren van databases SQL Data Warehouse. Bevat hulpprogramma's, DWUs en schaalbare prestaties, problemen met prestaties van query's, tot stand brengen van goed beleid en een database herstellen van beschadigde gegevens of van een regionale uitvalt."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Beheren van databases in Azure SQL Data Warehouse

SQL Data Warehouse automatisch veel aspecten van het beheer van uw databases. Bijvoorbeeld als u wilt schalen prestaties moet alleen u aanpassen en betalen voor het juiste ondersteuningsniveau compute bronnen en laat SQL Data Warehouse het werk van schalen en schalen terug. 

Ongetwijfeld zult u voor het controleren van uw werkbelasting te identificeren van de prestatiebehoeften van uw als langdurige query's oplossen. Ook moet u een aantal taken voor het beheren van machtigingen voor gebruikers en aanmeldingen uitvoeren.

In het overzicht worden deze aspecten van het beheer van SQL Data Warehouse.

- Beheerprogramma 's
- Schaal berekenen
- Onderbreken en hervatten
- Beste prestaties
- Query controleren
- Beveiliging
- Back-up en terugzetten

## <a name="management-tools"></a>Beheerprogramma 's

U kunt tal van hulpprogramma's voor het beheren van databases in SQL Data Warehouse. Als u databases wilt beheren, kunt u voorkeuren voor elke taak die u moet uitvoeren voor zal ontwikkelen.

### <a name="azure-portal"></a>Azure portal
De [Azure portal][] is een web-portal kunt u maken, bijwerken, en verwijder de databases en resources database controleren. Dit hulpprogramma is handig als u zich gewoon aan de slag met Azure, het beheer van een klein aantal databases gegevens magazijn, of snel iets te doen is.

Zie aan de slag met Azure portal [maken een datawarehouse SQL (Azure portal)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools in Visual Studio
[SQL Server Data Tools][] (SSDT) in Visual Studio kunt u verbinding kunt maken, beheren en ontwikkelen van uw databases. Als u bekend bent met Visual Studio of andere geïntegreerde ontwikkelomgevingen (IDEs) toepassingen ontwikkelt, probeer SSDT in Visual Studio.

SSDT bevat de SQL Server-Object Explorer waarmee u kunt visualiseren, verbinding maken en uitvoeren van scripts in databases van SQL Data Warehouse. Om snel een verbinding maken met SQL Data Warehouse, kunt u Klik op de knop **openen in Visual Studio** in de opdrachtbalk bij het bekijken van de informatie over de database in de klassieke Azure-Portal.  

Zie [Query Azure SQL Data Warehouse met Visual Studio][]aan de slag met SSDT in Visual Studio.

### <a name="command-line-tools"></a>Opdrachtregelprogramma 's
Opdrachtregelprogramma's zijn ideaal voor het automatiseren van uw werkbelasting.  PowerShell en sqlcmd zijn twee uitstekende manieren om uw processen te automatiseren.  Het is raadzaam deze hulpprogramma's voor een groot aantal logische servers beheren en implementeren van wijzigingen van resources in een productieomgeving, zoals de taken kunnen worden vastgelegd in een script en vervolgens automatisch.

### <a name="dynamic-management-views"></a>Beheer van dynamische weergaven 

DMVs zijn het brood en boter van het beheer van SQL Data Warehouse. Bijna alle informatie die in de portal oppervlakken is gebaseerd op DMVs. Zie voor een overzicht van SQL gegevens magazijn DMVs [systeemweergaven SQL Data Warehouse][].

Zie [verbinding maken en query met sqlcmd][]en [een database (PowerShell) maken][]om te beginnen.

## <a name="scale-compute"></a>Schaal berekenen

In SQL Data Warehouse, kunt u snel prestaties uit of terug te verhogen of verlagen compute middelen van CPU, geheugen en i/o-bandbreedte te schalen. Als u wilt schalen prestaties, is hoeft u het aantal magazijn eenheden (DWUs) waarmee SQL Data Warehouse aan uw database toegewezen aanpassen. SQL Data Warehouse snel de wijziging en alle onderliggende wijzigingen in de hardware of software verwerkt.

Zie voor meer informatie over het schalen van DWUs, [schaal][].

##  <a name="pause-and-resume"></a>Onderbreken en hervatten

Om kosten te besparen, kunt u onderbreken en hervatten compute resources op afroep. Bijvoorbeeld als uw computer niet op de database's nachts en in het weekend, kunt u onderbreken tijdens die momenten en gedurende de dag te hervatten. U geen bedrag afgeschreven voor DWUs terwijl de database is onderbroken.

Zie voor meer informatie, [berekenen onderbreken][]en [hervatten berekenen][].

## <a name="performance-best-practices"></a>Beste prestaties

Wanneer u aan de slag met een nieuwe technologie, bespaart ontdekken de tips en trucs die het beste meteen vanaf het begin werken u veel tijd.  Vindt u de beste praktijken in veel van onze onderwerpen.

Veel een samenvatting van de belangrijkste aandachtspunten bij het ontwikkelen van uw werkbelasting, Zie [Aanbevolen procedures voor magazijn SQL-gegevens][].

## <a name="query-monitoring"></a>Query controleren

Soms een query te lang wordt uitgevoerd, maar u niet weet welke is de boosdoener. SQL Data Warehouse is management dynamische weergaven (DMVs) die u gebruiken kunt om erachter te komen welke query het duurt te lang. 

Langdurige query's Zie [controleren uw werkbelasting met DMVs][].

## <a name="security"></a>Beveiliging

Om een veilig systeem onderhouden, moet u op de waarschuwing en bescherming tegen alle soorten toegang door onbevoegden. Een beveiligingssysteem moet om te controleren of de firewall-regels aanwezig zijn zodat alleen geautoriseerde IP-adressen kunnen aansluiten. Juiste verificatie van gebruikersreferenties nodig. Nadat een gebruiker is verbonden met de database, moet de gebruiker alleen machtigingen voor het uitvoeren van een minimum aantal acties hebben. Om gegevens te beveiligen, kunt u codering gebruiken. Het is ook belangrijk om het bijhouden en controleren zodat u gebeurtenissen keren kunt als er verdachte activiteiten.

Voor meer informatie over het beheren van beveiliging, hoofd aan het [overzicht van de beveiliging][].

## <a name="backup-and-restore"></a>Back-up en terugzetten

Met betrouwbare backps van uw gegevens is een essentieel onderdeel van de productiedatabase. SQL Data Warehouse uw gegevens houdt veilig automatisch een back-up van uw actieve databases met regelmatige tussenpozen. Deze back-ups kunnen u scenario's waar u hebt uw gegevens beschadigd of per ongeluk verwijderd van de database of gegevens herstellen.  Zie [herstellen van een momentopname][]voor het back-upschema gegevens bewaarbeleid en het herstellen van een database.

## <a name="next-steps"></a>Volgende stappen
Met behulp van goed ontwerp principes te vergemakkelijken voor het beheren van uw databases in SQL Data Warehouse. Voor meer informatie, hoofd aan het [overzicht van de ontwikkeling][].

<!--Image references-->

<!--Article references-->
[Maak een SQL datawarehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Maak een database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Query SQL Azure datawarehouse met Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Verbinding maken en een query met sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md
[Controleer uw werkbelasting met DMVs]: sql-data-warehouse-manage-monitor.md
[Compute onderbreken]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Herstellen van momentopname]: sql-data-warehouse-restore-database-overview.md
[Cv berekenen]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Schaal prestaties]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Beveiliging-overzicht]: sql-data-warehouse-overview-manage-security.md
[Aanbevolen procedures voor magazijn SQL-gegevens]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse systeemweergaven]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
