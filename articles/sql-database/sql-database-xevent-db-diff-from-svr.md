<properties
    pageTitle="Uitgebreide gebeurtenissen in de SQL-Database | Microsoft Azure"
    description="Beschrijving van uitgebreide gebeurtenissen (XEvents) in Azure SQL-Database en hoe gebeurtenis sessies enigszins verschillen van de gebeurtenis sessies in Microsoft SQL Server."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""
    tags=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="genemi"/>


# <a name="extended-events-in-sql-database"></a>Uitgebreide gebeurtenissen in de SQL-Database

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Dit onderwerp wordt uitgelegd hoe de uitvoering van uitgebreide gebeurtenissen in Azure SQL-Database is iets anders in vergelijking met uitgebreide gebeurtenissen in Microsoft SQL Server.


- SQL Database V12 gewonnen de functie uitgebreide gebeurtenissen in de tweede helft van de agenda 2015.
- SQL Server heeft uitgebreide gebeurtenissen sinds 2008.
- De functie reeks uitgebreide gebeurtenissen op de SQL-Database is een robuuste subset van de functies van SQL Server.


*XEvents* is een informeel bijnaam die soms wordt gebruikt voor 'uitgebreide gebeurtenissen' in blogs en andere informele locaties.


> [AZURE.NOTE] Vanaf oktober 2015, uitgebreide gebeurtenis sessie functie in Azure SQL-Database op het niveau van de voorvertoning. De datum van de algemene beschikbaarheid (GA) is nog niet ingesteld.
>
> De pagina Azure [Service Updates](https://azure.microsoft.com/updates/?service=sql-database) bevat advertenties wanneer GA aankondigingen worden aangebracht.


Als u meer informatie over gebeurtenissen uitgebreide, voor Azure SQL-Database en een Microsoft SQL Server is beschikbaar op:

- [Quick Start: Uitgebreide gebeurtenissen in SQL Server](http://msdn.microsoft.com/library/mt733217.aspx)
- [Uitgebreide gebeurtenissen](http://msdn.microsoft.com/library/bb630282.aspx)


## <a name="prerequisites"></a>Vereisten


In dit onderwerp wordt ervan uitgegaan dat u al enige kennis hebt van:


- [Service azure SQL-Database](https://azure.microsoft.com/services/sql-database/).


- [Uitgebreide gebeurtenissen](http://msdn.microsoft.com/library/bb630282.aspx) in Microsoft SQL Server.
 - Het merendeel van onze documentatie over gebeurtenissen uitgebreide geldt voor zowel SQL Server en SQL-Database.


Eerdere blootstelling aan de volgende items is handig bij het kiezen van de gebeurtenis als [doel](#AzureXEventsTargets):


- [Azure Storage-service](https://azure.microsoft.com/services/storage/)


- PowerShell
 - [Met behulp van Azure PowerShell met Azure opslag](../storage/storage-powershell-guide-full.md) - biedt uitgebreide informatie over PowerShell en Azure Storage service.


## <a name="code-samples"></a>Codevoorbeelden


Verwante onderwerpen vindt u twee voorbeelden van code:


- [Ring Buffer doel code voor uitgebreide gebeurtenissen in de SQL-Database](sql-database-xevent-code-ring-buffer.md)
 - Korte eenvoudige Transact-SQL-script.
 - Wij benadrukken in de code monster onderwerp dat, wanneer u klaar bent met het doel van een Ring-Buffer, u de bronnen vrijgeven moet door het uitvoeren van een alter neer `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` instructie. Later kunt u een ander exemplaar van de Buffer door Ring toevoegen `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Bestand doel gebeurteniscode voor uitgebreide gebeurtenissen in de SQL-Database](sql-database-xevent-code-event-file.md)
 - Fase 1 is PowerShell een container Azure opslag maken.
 - Fase 2 is de Transact-SQL die gebruikmaakt van de container voor de opslag van Azure.


## <a name="transact-sql-differences"></a>Verschillen in Transact-SQL


- Als u de opdracht [Maken GEBEURTENIS sessie](http://msdn.microsoft.com/library/bb677289.aspx) van SQL Server uitvoert, moet u de **SERVER ON** -component gebruiken. Maar op de SQL-Database de **DATABASE ON** -component in plaats daarvan.


- De component **ON DATABASE** is ook van toepassing op de [GEBEURTENIS wijzigen sessie](http://msdn.microsoft.com/library/bb630368.aspx) en [DROP GEBEURTENIS sessie](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-opdrachten.


- Beste manier is om op te nemen van de optie gebeurtenis sessie van **STARTUP_STATE = ON** in uw overzichten **Maken GEBEURTENIS sessie** of **GEBEURTENIS wijzigen-sessie** .
 - De waarde **= ON** ondersteunt opnieuw worden opgestart na een herconfiguratie van de logische database als gevolg van een failover.


## <a name="new-catalog-views"></a>Nieuwe catalogusweergaven


De functie uitgebreide gebeurtenissen wordt ondersteund door verschillende [catalogusweergaven](http://msdn.microsoft.com/library/ms174365.aspx). Catalogusweergaven vertellen u *metagegevens of definities* van door de gebruiker gemaakte gebeurtenis sessies in de huidige database. De weergaven retourneren geen informatie over gevallen van gebeurtenis actieve sessies.


| Naam van<br/>catalogus weergeven | Beschrijving |
| :-- | :-- |
| **sys.database_event_session_actions** | Geeft als resultaat een rij voor elke actie die bij elke gebeurtenis van een sessie van de gebeurtenis. |
| **sys.database_event_session_events** | Geeft als resultaat een rij voor elke gebeurtenis in een sessie van de gebeurtenis. |
| **sys.database_event_session_fields** | Deze eigenschap retourneert een rij voor elke kolom aanpassen kunt expliciet is ingesteld op gebeurtenissen en doelen. |
| **sys.database_event_session_targets** | Geeft als resultaat een rij voor elk gebeurtenisdoel voor een gebeurtenis-sessie. |
| **sys.database_event_sessions** | Geeft als resultaat een rij voor elke sessie van de gebeurtenis in de database van SQL-Database. |


In Microsoft SQL Server, vergelijkbare catalogusweergaven hebben namen die zijn *.server\_ * in plaats van *.database\_*. De syntaxis is vergelijkbaar met **sys.server_event_%**.


## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Nieuwe dynamic management views [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)


Azure SQL-Database heeft [beheer van dynamische weergaven (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) die ondersteuning bieden van uitgebreide gebeurtenissen. DMVs vertellen u over *actieve* sessies van de gebeurtenis.


| Naam van DMV | Beschrijving |
| :-- | :-- |
| **sys.dm_xe_database_session_event_actions** | Retourneert informatie over de gebeurtenis sessie acties. |
| **sys.dm_xe_database_session_events** | Retourneert informatie over de sessiegebeurtenissen. |
| **sys.dm_xe_database_session_object_columns** | Bevat de configuratiewaarden voor objecten die afhankelijk zijn van een sessie. |
| **sys.dm_xe_database_session_targets** | Retourneert informatie over de doelen van de sessie. |
| **sys.dm_xe_database_sessions** | Geeft als resultaat een rij voor elke gebeurtenis-sessie die ligt binnen het bereik naar de huidige database. |


In Microsoft SQL Server, vergelijkbare catalogusweergaven worden genoemd zonder de * \_database* gedeelte van de naam, zoals:


- **sys.dm_xe_sessions**, in plaats van naam<br/>**sys.dm_xe_database_sessions**.


### <a name="dmvs-common-to-both"></a>Voor beide DMVs


Er zijn extra DMVs die gemeenschappelijk voor zowel Azure SQL-Database en een Microsoft SQL Server zijn voor uitgebreide gebeurtenissen:


- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**



 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Zoeken naar de beschikbare uitgebreide gebeurtenissen, acties en doelen


U kunt uitvoeren om een eenvoudige SQL **selecteren** als u een lijst met de beschikbare gebeurtenissen, acties en doel.


```
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```



<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a>

&nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Doelen voor uw sessies van de gebeurtenis SQL-Database


Dit zijn doelen die de resultaten van uw sessies van de gebeurtenis op de SQL-Database kunnen vastleggen:


- [Ring Buffer doel](http://msdn.microsoft.com/library/ff878182.aspx) - bevat een korte gegevens in het geheugen.
- [Gebeurtenis item doel](http://msdn.microsoft.com/library/ff878025.aspx) - telt alle gebeurtenissen die tijdens een sessie van uitgebreide gebeurtenissen plaatsvinden.
- [Doel van de gebeurtenisbestand](http://msdn.microsoft.com/library/ff878115.aspx) - volledige buffers schrijft aan een container voor de opslag van Azure.


De [Gebeurtenis Tracing voor Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API is niet beschikbaar voor uitgebreide gebeurtenissen op de SQL-Database.


## <a name="restrictions"></a>Beperkingen


Er zijn een aantal beveiligings-gerelateerde verschillen befitting de cloud-omgeving van de SQL-Database:


- Uitgebreide gebeurtenissen berusten op het model met één huurder isolatie. De sessie van een gebeurtenis in de ene database geen toegang tot gegevens of gebeurtenissen uit een andere database.

- U kunt een overzicht **Maken GEBEURTENIS sessie** in het kader van de **master** -database kan niet uitgeven.


## <a name="permission-model"></a>Machtiging model


U moet gemachtigd zijn **controle** op de database naar een **GEBEURTENIS maken sessie** -instructie. De database-eigenaar (dbo) heeft machtiging **beheer** .


### <a name="storage-container-authorizations"></a>Opslag container vergunningen


De SAS-token voor uw Azure opslag container genereren moet **rwl** voor de machtigingen opgeven. De waarde **rwl** biedt de volgende machtigingen:


- Lezen
- Schrijven
- Lijst


## <a name="performance-considerations"></a>Overwegingen bij prestaties


Er zijn scenario's waar intensief gebruik van uitgebreide gebeurtenissen actiever geheugen dan voor het systeem als geheel in orde is, zich ophopen. Daarom de Azure SQL-Database-systeem dynamisch ingesteld en limieten op het bedrag van het actieve geheugen dat kan worden opgebouwd door de sessie van een gebeurtenis wordt aangepast. Veel factoren een rol spelen in de dynamische berekening.


Als er een foutbericht dat een maximale geheugen is toegepast, worden sommige corrigerende maatregelen die u kunt nemen zijn:


- Minder gebeurtenis gelijktijdige sessies uitvoeren.


- Via uw overzichten **maken** en **wijzigen** voor gebeurtenis sessies verminderen de hoeveelheid geheugen die u opgeeft op de **MAX\_geheugen** component.


### <a name="network-latency"></a>Netwerkvertraging


Het doel van het **Bestand van de gebeurtenis** kan optreden netwerkvertraging of fouten bij het vastleggen van gegevens voor de opslag van Azure BLOB's. Andere gebeurtenissen in de SQL-Database kunnen worden uitgesteld, wachten op voor de communicatie te voltooien. Deze vertraging kan vertragen uw werkbelasting.

- Om deze prestaties risico vermijden als u de optie **EVENT_RETENTION_MODE** op **NO_EVENT_LOSS** in uw evenement-sessies.


## <a name="related-links"></a>Verwante koppelingen


- [Azure PowerShell met Azure opslag gebruiken](../storage/storage-powershell-guide-full.md).
- [Azure opslag Cmdlets](http://msdn.microsoft.com/library/dn806401.aspx)


- [Met behulp van Azure PowerShell met Azure opslag](../storage/storage-powershell-guide-full.md) - biedt uitgebreide informatie over PowerShell en Azure Storage service.
- [Het gebruik van .NET Blob-opslag](../storage/storage-dotnet-how-to-use-blobs.md)


- [REFERENTIE (Transact-SQL) maken](http://msdn.microsoft.com/library/ms189522.aspx)
- [GEBEURTENIS sessie (Transact-SQL)](http://msdn.microsoft.com/library/bb677289.aspx)


- [Jonathan Kehayias van blogberichten over uitgebreide gebeurtenissen in Microsoft SQL Server](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


Andere code monster onderwerpen voor uitgebreide gebeurtenissen zijn beschikbaar op de volgende koppelingen. Echter, moet u regelmatig een voorbeeld om te zien of het monster is bedoeld voor Microsoft SQL Server versus Azure SQL-Database controleren. Vervolgens kunt u beslissen of kleine wijzigingen nodig zijn voor het uitvoeren van de steekproef.


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
