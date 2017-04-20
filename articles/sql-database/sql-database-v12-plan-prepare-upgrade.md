<properties
    pageTitle="Plan uw upgrade naar SQL Database V12 | Microsoft Azure"
    description="Beschrijving van de voorbereidingen en de beperkingen die zijn betrokken bij een upgrade naar versie V12 van Azure SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Plannen en voorbereiden voor een upgrade naar SQL Database V12


Dit onderwerp beschrijft de planning en de voorbereidingen die u moet uitvoeren om uw Azure SQL-databases van versie V11: upgrade naar V12.


Een nieuwe [Portal in Azure](https://portal.azure.com/) is beschikbaar ter ondersteuning van uw upgrade naar V12.


De volgende tabel geeft een overzicht van andere Help-onderwerpen voor V12.


| Titel en link | Beschrijving van inhoud |
| :--- | :--- |
| [Nieuwe functies in een SQL-Database V12](sql-database-v12-whats-new.md) | Beschrijving van de details van hoe V12 dichter Azure SQL-Database tot volledige pariteit met Microsoft SQL Server brengt. |
| [Maak een database in SQL Database V12](sql-database-get-started.md) | Hierin wordt beschreven hoe u een nieuwe Azure SQL-database versie V12 kunt maken. Hierin worden verschillende opties dan alleen een lege database. |


## <a name="plan-ahead"></a>Plan vooruit


De volgende subsecties beschrijven leren en besluitvorming adresseren voordat u een acties naar de Azure SQL-database bijwerken naar V12.

### <a name="version-clarification"></a>Toelichting versie


Dit document heeft betrekking op de upgrade van Microsoft Azure SQL-Database uit versie V11: V12. Meer formeel gesteld de versienummers lijken op de volgende twee waarden, zoals gerapporteerd door de Transact-SQL-instructie **selecteren @@version; ** :


- 12.0.2000.8 *(of een hogere, bits V12)*
- 11.0.9228.18 *(V11:)*
 - V11: Er is ook wel SAWA v2.

### <a name="service-tier-planning"></a>Planning tier-service


Beginnen met V12, ondersteunen Azure SQL-Database alleen de service niveaus Basic, Standard en Premium. De Web- en service-laag wordt niet ondersteund op V12. Dus als u van plan uw Azure SQL-database die momenteel op het niveau van de service Web- of een upgrade uitvoert bent, moet u bepalen wat de nieuwe servicelaag moet zijn.


Zie voor gedetailleerde informatie over de Basic, Standard en Premium service niveaus:

- [SQL-Database service niveaus](sql-database-service-tiers.md)
- [SQL-Database Web/Business-Databases bijwerken naar nieuwe Service Tiers](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>De Geo-replicatie-configuratie controleren


Als uw database SQL Azure voor Geo-replicatie is geconfigureerd, te documenteren van de huidige configuratie, en Geo-replicatie stoppen voordat u begint met de voorbereidingen voor upgraden acties. Nadat de upgrade is voltooid kunt u de database moet opnieuw configureren voor Geo-replicatie.


De strategie is de bron wilt behouden en een kopie van de database testen.


## <a name="preparation-actions"></a>Voorbereiding van acties


Nadat de planning is voltooid, kunt u de actie stappen die worden beschreven in de volgende subsecties voor te bereiden voor de laatste fase van de upgrade kunt uitvoeren.


Gedetailleerde beschrijvingen van de laatste fase van de upgrade zijn beschikbaar in de Help-onderwerpen die zijn gekoppeld aan het begin van dit Help-onderwerp.


### <a name="service-tier-actions"></a>Service tier acties


De Web- en serviceprijzen laag wordt niet ondersteund op V12.


Als uw database V11: Azure SQL een Web- of database is, biedt het upgradeproces overschakelen van de database naar een ondersteunde laag. De upgrade wordt aanbevolen een laag die past bij de geschiedenis van de werkbelasting van de database. U kunt echter elke ondersteunde laag die u wilt kiezen.


De stappen die nodig zijn tijdens de upgrade kunt u verkleinen door uw database V11: van de Web-en Business-laag te schakelen voordat u de upgrade start. U kunt dit doen met behulp van de nieuwe [Portal Azure](https://portal.azure.com/).


Als u niet welke service tier weet overschakelen naar mogelijk het niveau van de standaard laag van S2 een verstandige eerste keuze. Alle lagere niveaus zal bevatten minder materialen dan de Web- en laag was.


### <a name="suspend-geo-replication-during-upgrade"></a>Geo-replicatie onderbreken tijdens de upgrade


De upgrade naar V12 kan worden uitgevoerd als Geo-replicatie actief op de database is. Eerst moet u de database als u wilt stoppen met het gebruik van Geo-replicatie de configuratie.


Nadat de upgrade is voltooid, kunt u de database voor het gebruik van Geo-replicatie opnieuw configureren.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Open poorten op een Azure VM voor clientverbindingen


Als uw clientprogramma verbinding maakt met SQL Database V12 terwijl de client wordt uitgevoerd op een Azure VM (virtual machine), moet u de volgende bereikwaarden op de VM openen:

- 11000 11999
- 14000 14999


Klik [hier](sql-database-develop-direct-route-ports-adonet-v12.md) voor meer informatie over de poorten voor SQL-Database V12. De poorten zijn door de verbeterde prestaties in SQL Database V12 nodig.


##<a id="limitations"></a>Beperkingen tijdens en na de upgrade naar V12


### <a name="portals-for-v12"></a>Portals voor V12


Er zijn drie portals voor Azure en elk heeft verschillende mogelijkheden met betrekking tot de SQL-Database V12.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Deze Portal Azure is nieuw en is nog steeds op de preview-status. Deze portal is nog helemaal niet op volledige algemene beschikbaarheid (GA). Deze portal:
 - Kunnen uw V12 server en database beheren.
 - Kan uw V11: database bijwerken naar V12.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Deze klassieke Portal Azure kan uiteindelijk worden afgebouwd. Deze portal:
 - Kunnen uw V12 server en database beheren.
 - Kan *geen* upgrade uw V11: database aan V12.


- (http://*servernaam*. database.windows.net)<br/>Klassieke Portal Azure SQL-Database:
 - Kan*geen* V12 servers beheren.


We raden u verbinding maken met uw Azure SQL-databases met Visual Studio 2015 (VS2015). VS2015 kan worden gebruikt voor de volgende taken:


- Een Transact-SQL-instructie uitgevoerd.
- Voor het ontwerpen van een schema.
- Voor de ontwikkeling van een database, on line of off line.


Of in plaats daarvan voor het vrije kunt u [Visual Studio Community 2015](https://www.visualstudio.com/vs/community/), een complete versie van VS2015.


In de oudere Azure klassieke Portal op de databasepagina, kunt u **in Visual Studio openen** om VS2015 starten op uw computer voor verbinding met uw Azure SQL-Database.


Voor een ander alternatief, kunt u SQL Server Management Studio (SSMS) 2014 met [CU6](http://support.microsoft.com/kb/3031047/) met Azure SQL-Database. Meer details zijn op dit blogbericht:<br/>[Client gereedschap updates voor Azure SQL-Database](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Beperking *tijdens de* upgrade naar V12


De database V11: blijft beschikbaar voor toegang tot gegevens tijdens de upgrade naar V12. Er zijn nog een aantal beperkingen te overwegen.


| Beperking | Beschrijving |
| :--- | :--- |
| Duur van de upgrade | De duur van de upgrade hangt, editie aantal en de omvang van databases op de server. Tijdens de upgrade kunt uitvoeren voor uren tot dagen voor servers, vooral voor servers met databases:<br/><br/>*Groter is dan 50 GB of<br/> * In een servicelaag niet-premium<br/><br/>Het maken van nieuwe databases op de server tijdens de upgrade kan de duur van de upgrade ook toenemen. |
| Geen Geo-replicatie | Geo-replicatie wordt niet ondersteund op een server V12 die betrokken is in een upgrade van V11:. |
| Database is korte tijd onbeschikbaar is in de laatste fase van de upgrade naar V12 | De databases die deel uitmaken van uw server V11: blijven beschikbaar tijdens het upgradeproces. De verbinding met de server en de databases is echter even niet beschikbaar is in de laatste fase, wanneer de schakeloptie via vanaf V11: aan de V12 gereed begint.<br/><br/>De schakeloptie periode kan variëren van 40 seconden op 5 minuten. Voor de meeste servers switch over naar verwachting voltooid binnen 90 seconden. Voor servers die een groot aantal databases of wanneer de databases schrijven zware werkbelasting verhoogt de switch na verloop van tijd. |


### <a name="limitation-after-upgrade-to-v12"></a>Beperking *na* een upgrade naar V12


| Beperking | Beschrijving |
| :--- | :--- |
| Kan niet teruggaan naar V11: | Na een upgrade ter plaatse, kan niet het resultaat worden teruggedraaid of ongedaan gemaakt. |
| Web- of laag | Nadat u de upgrade start, de server voor de nieuwe V12-database kunnen niet meer herkent of accepteert de service Web- of laag. |



### <a name="export-and-import-after-upgrade-to-v12"></a>*Nadat* de upgrade naar V12 importeren en exporteren


U kunt exporteren of importeren van een V12-database met behulp van de [Portal Azure](https://portal.azure.com/). Of u kunt exporteren of importeren met behulp van een van de volgende hulpprogramma's:


- SQL Server Management Studio (SSMS)
- Visual Studio 2015
- Gegevenslaag Application Framework (DacFx)


Echter voor het gebruik van de hulpprogramma's, moet u eerst hebt of installeer de nieuwste updates zodat ze de nieuwe V12 functies ondersteunen:


- [6 cumulatieve Update voor SQL Server Management Studio 2014](http://support2.microsoft.com/kb/3031047)
- [Februari 2015-Update voor SQL Server-Database in Visual Studio 2013 gereedschap](https://msdn.microsoft.com/data/hh297027)
- [Februari 2015-gegevenslaag Application Framework (DacFx) voor Azure SQL Database V12](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Het vorige gereedschap koppelingen zijn bijgewerkt op of na 2 maart 2015. Wij raden aan dat u deze nieuwere updates van deze hulpprogramma's gebruiken.


#### <a name="automated-export"></a>Geautomatiseerde exporteren


Automatische Export blijft beschikbaar als voorbeeld.  Geen client tool updates zijn vereist bij het gebruik van geautomatiseerde exporteren.  Als u ervoor kiest om het bestand te importeren naar een lokale server, worden de bovenvermelde updates tooling nodig geïmporteerd.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>V12 van een verwijderde V11: database herstellen

In het volgende scenario wordt uitgelegd dat een verwijderde V11: Azure SQL-database kan worden hersteld naar een server V12 Azure SQL-Database.

1. Stel dat u hebt een V11: Azure SQL-databaseserver. <br/> U hebt een database in de verouderde Web-en Business servicelaag op de server.
2. U kunt de database verwijderen.
3. U kunt de server bijwerken naar V12.
4. Herstel vervolgens de database op de server. <br/> De database wordt bijgewerkt naar V12, op het niveau van de standaard service-laag van S0.
5. U kunt de database op elke laag ondersteunde service overschakelen als S0 niet uw voorkeur is.


### <a name="powershell-cmdlets"></a>PowerShell-cmdlets


PowerShell-cmdlets zijn beschikbaar voor het starten, stoppen of een upgrade naar Azure SQL Database V12 uit V11: of een andere versie van de pre-V12 controleren.

- [Upgrade naar SQL Database V12 met PowerShell](sql-database-upgrade-server-powershell.md)

Voor documentatie over deze PowerShell-cmdlets, Zie:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Stop AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


De Stop-cmdlet betekent annuleren, niet onderbreken. Er is geen manier om een upgrade, dan start het programma opnieuw vanaf het begin te hervatten. De Stop-cmdlet schoont en versies van alle passende middelen.


## <a name="failure-resolution"></a>Fout resolutie


Als de upgrade voor oneven reden mislukt, blijft de database V11: actief en beschikbaar is als normaal.


## <a name="related-links"></a>Verwante koppelingen


- Microsoft Azure [voorbeeld functies](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
