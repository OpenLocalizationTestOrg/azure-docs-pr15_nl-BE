<properties
   pageTitle="Back-ups van SQL-Database - automatische, geo-redundante | Microsoft Azure" 
   description="SQL-Database maakt u een back-up van de lokale database om de vijf minuten automatisch en Azure leestoegang geo-redundante opslag (RA-GRS) gebruikt als redundantie geo. "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Meer informatie over back-ups van SQL-Database

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

SQL-Database wordt gemaakt van een lokale database back-up om de paar minuten automatisch en Azure leestoegang geo-redundante opslag voor geo-redundantie gebruikt. Back-ups zijn een essentieel onderdeel van de continuïteit en noodherstel herstel bedrijfsstrategie omdat ze voorkomen dat gegevens per ongeluk beschadigd of verwijderd. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Wat is een back-up van SQL-Database?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

Een back-up van SQL-Database bevat zowel lokale back-ups en geo-redundante back-ups. Deze back-ups worden gemaakt, automatisch en zonder extra kosten. U hoeft niet alles te doen plaatsvinden.

<!----------------- 
    Explains first component of the backup feature
------------------>

Voor lokale back-ups wordt SQL-Database SQL Server-technologie [volledige](https://msdn.microsoft.com/library/ms186289.aspx) [Differentieel](https://msdn.microsoft.com/library/ms175526.aspx )en [transactie-log](https://msdn.microsoft.com/library/ms191429.aspx) back-ups maken. De transactielogboeken om de vijf minuten, waarmee u een point-in-time terugzetten naar dezelfde server waarop de database gaat gebeuren. Als u een database terugzet, de service heeft weten te achterhalen welke volledige en differentiële transaction log back-ups moeten worden teruggezet.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Een lokale database back-up te gebruiken:

- Een database terugzet op een punt in de tijd binnen de bewaartermijn. Met back-up van een database kunt u een database terugzet op een punt in de tijd, een verwijderde database terugzetten naar de tijd die is verwijderd of een database terugzet op een andere geografische onderverdeling. Zie [herstellen van een database uit een back-up van de database](sql-database-recovery-using-backups.md)om een herstelbewerking uit te voeren.

- Een database kopiëren naar een SQL-server in dezelfde of een andere regio. De kopie transactioneel consistent is met de huidige SQL-Database. Zie voor het uitvoeren van een kopie, [databasekopie](sql-database-copy.md).

- Een reservekopie van de database buiten de bewaarperiode van de back-up archiveren. Voor het uitvoeren van een archief, [een SQL-database naar een Bacpac-](sql-database-export.md) exportbestand. U kunt vervolgens de BACPAC voor langere termijn opgeslagen archiveren en opslaan dan de bewaarperiode. Of gebruik de BACPAC een kopie van uw database overbrengen naar SQL Server, in ruimten of in een Azure VM (virtual machine).

<!----------------- 
    Explains first component of the backup feature
------------------>

Voor geo-redundante back-ups wordt SQL-Database [replicatie Azure opslag](../storage/storage-redundancy.md). SQL-Database slaat lokale database back-upbestanden op een rekening voor [Geo-redundante opslag lezen-toegang (RA-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) . De back-upbestanden repliceert Azure naar een [gepaarde Datacenter](../best-practices-availability-paired-regions.md). 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Een geo-redundante back-up te gebruiken:

- In het geval u geen toegang de database back-up van uw regio voor primaire database tot, kunt u een database terugzet op een andere geografische regio. 

>[AZURE.NOTE] In Azure opslag, wordt de term *replicatie* verwijst naar het kopiëren van bestanden van de ene locatie naar de andere. De SQL- *databasereplicatie* verwijst naar tot meerdere secundaire databases worden gesynchroniseerd met een primaire database. 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Hoeveel opslagruimte voor back-up is opgenomen zonder kosten?

SQL-Database biedt maximaal 200% van de opslagruimte van maximaal ingerichte database als back-up opslaan zonder extra kosten. Bijvoorbeeld als er een standaard DB exemplaar met een ingerichte DB grootte van 250 GB, hebt u 500 GB opslagruimte voor back-zonder extra kosten. Als uw database groter is dan de opgegeven back-up opslaan, kunt u de bewaarperiode verminderen door contact opnemen met ondersteuning van Azure. Een andere mogelijkheid is om te betalen voor extra back-up opslaan op het standaardtarief leestoegang tot geografisch redundante opslag (RA-GRS) wordt gefactureerd. 

## <a name="how-often-do-backups-happen"></a>Hoe vaak nu back-ups eenmaal gebeuren?

Voor lokale back-ups, worden volledige databaseback-ups wekelijks gebeuren differentiële back-ups gebeuren per uur en het transactie-log back-ups om de vijf minuten gebeuren. De eerste volledige back-up is gepland zodra een database is gemaakt. Meestal wordt voltooid binnen 30 minuten, maar dit kan langer duren wanneer de database van een aanzienlijke omvang is. Bijvoorbeeld, de eerste back-up kan langer duren voordat op een herstelde database of een databasekopie. Na de eerste volledige back-up, worden alle verdere back-ups automatisch gepland en beheerd stil op de achtergrond. Het exacte tijdstip van volledige en [differentiële](https://msdn.microsoft.com/library/ms175526.aspx) back-ups wordt bepaald als deze balans van het algehele systeem minder zwaar. 

Voor geo-redundante back-ups, volledige en differentiële back-ups volgens het replicatieschema Azure opslag gekopieerd.

## <a name="how-long-do-you-keep-my-backups"></a>Hoe lang houdt u de back-ups?

Elke back-up van SQL-Database heeft een periode die is gebaseerd op de [service-laag](sql-database-service-tiers.md) van de database. De bewaarperiode voor een database in de:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Basisdienst laag is zeven dagen.
- -Tier-service standaard is 35 dagen.
- Premium service tier is 35 dagen.


Als u de database van de Standard of Premium service niveaus in een standaardschijf downgraden, wordt de back-ups worden opgeslagen gedurende zeven dagen. Alle bestaande back-ups ouder zijn dan zeven dagen zijn niet langer beschikbaar. 

Als u uw database van de laag basisdienst naar Standard of Premium upgrade, blijven SQL-Database bestaande back-ups totdat ze 35 dagen oud zijn. Nieuwe back-ups blijven ze voor 35 dagen plaatsvinden.
 
Als u een database verwijdert, blijven SQL-Database de back-ups op dezelfde manier zou voor een on line database. Stel bijvoorbeeld dat u een eenvoudige database verwijderen die een periode van zeven dagen is. Een back-up die vier dagen oud is, wordt opgeslagen voor drie dagen.

>[AZURE.IMPORTANT]
    Als u de Azure SQL-server die als host fungeert voor SQL-Databases verwijdert, worden alle databases die deel uitmaken van de server ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Volgende stappen

Back-ups zijn een essentieel onderdeel van de continuïteit en noodherstel herstel bedrijfsstrategie omdat ze voorkomen dat gegevens per ongeluk beschadigd of verwijderd. Om te zien hoe database back-ups in een bredere strategie Zie [Business continuity overzicht](sql-database-business-continuity.md).


