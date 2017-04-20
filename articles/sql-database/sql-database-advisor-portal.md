<properties 
   pageTitle="Azure SQL Database-adviseur met de Azure portal | Microsoft Azure" 
   description="Kunt u de Advisor Azure SQL-Database in de Azure portal te controleren en de implementatie van de aanbevelingen voor uw bestaande SQL-Databases die de huidige prestaties van query's kunt verbeteren." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>SQL Database-adviseur met de Azure portal

> [AZURE.SELECTOR]
- [Overzicht SQL Database-adviseur](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

Kunt u de Advisor Azure SQL-Database in de Azure portal te controleren en de implementatie van de aanbevelingen voor uw bestaande SQL-Databases die de huidige prestaties van query's kunt verbeteren.

## <a name="viewing-recommendations"></a>Aanbevelingen weergeven

De aanbevelingen pagina is waar u de belangrijkste aanbevelingen om prestaties te verbeteren op basis van de mogelijke invloed daarvan bekijken. U kunt ook de status van de historische bewerkingen weergeven. Selecteer een aanbeveling of status voor meer details.

Bekijken en aanbevelingen van toepassing, moet u de machtigingen van het juiste [toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-configure.md) in Azure. **Lezer**, **SQL DB Inzender** machtigingen zijn vereist voor aanbevelingen weergeven en **eigenaar**, **SQL DB Inzender** machtigingen zijn vereist voor het uitvoeren van acties; maken of drop indexen en indexen maken te annuleren.

1. Log in om de [Azure portal](https://portal.azure.com/).
2. Klik op **meer services** > **SQL-databases**, en de database te selecteren.
5. Klik op **prestaties aanbeveling** om beschikbare aanbevelingen voor de geselecteerde database weer te geven.

> [AZURE.NOTE] Als u aanbevelingen van een database, moet over een dag van gebruik en moet er een activiteit. Ook moet er een consistente activiteit. De SQL-Database-adviseur kan op eenvoudige wijze optimaliseren voor consistente query patronen dan kan het voor willekeurige gevlekte bursts van activiteit. Als de aanbevelingen niet beschikbaar zijn, dient de pagina **prestaties aanbeveling** een bericht waarin wordt uitgelegd waarom.

![Aanbevelingen](./media/sql-database-advisor-portal/recommendations.png)

Hier volgt een voorbeeld van de aanbeveling 'Schema oplossen' in de portal Azure.

![Schema oplossen](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Aanbevelingen worden gesorteerd op hun mogelijke invloed op de prestaties in de volgende vier categorieën:

| Impact | Beschrijving |
| :--- | :--- |
| Hoog | Hoge impact aanbevelingen dient de meest significante invloed op de prestaties. |
| Gemiddeld | Gemiddeld effect aanbevelingen moeten de prestaties verbeteren, maar niet in aanzienlijke mate. |
| Laag | Lage impact aanbevelingen moeten bieden betere prestaties dan zonder, maar mogelijk aanzienlijke verbeteringen niet. 


### <a name="removing-recommendations-from-the-list"></a>Aanbevelingen verwijderen uit de lijst

Als uw lijst met aanbevelingen items die u wilt verwijderen uit de lijst bevat, kunt u de aanbeveling te negeren:

1. Selecteer een aanbeveling in de lijst met **aanbevelingen**.
2. Klik op **verwijderen** in het blad **Details** .


Indien gewenst, kunt u verwijderde items terug naar de lijst met **aanbevelingen** toevoegen:

1. Klik op het blad **aanbevelingen** **weergave verwijderd**.
1. Een verwijderd item selecteren in de lijst om de details te bekijken.
1. Klik eventueel op **Verwijderen ongedaan maken** om de index opnieuw toevoegen aan de hoofdlijst van **aanbevelingen**.



## <a name="applying-recommendations"></a>Toepassing van aanbevelingen

SQL Database Advisor biedt u volledige controle over hoe de aanbevelingen worden ingeschakeld met behulp van een van de volgende drie opties: 

- Individuele aanbevelingen één tegelijk toepassen.
- Inschakelen van de adviseur automatisch toe te passen aanbevelingen (op dit moment geldt alleen aanbevelingen index).
- Een aanbeveling handmatig implementeren, het aanbevolen T-SQL-script worden uitgevoerd ten opzichte van de database.

Selecteer een aanbeveling om de details te bekijken en klik op **script weergeven** om de exacte details van hoe de aanbeveling wordt gemaakt.

De database online blijft tijdens de advisor de aanbeveling geldt--met behulp van SQL-Database Advisor nooit heeft een database off line.

### <a name="apply-an-individual-recommendation"></a>De aanbeveling van een afzonderlijke toepassing

U kunt bekijken en aanbevelingen één tegelijk accepteren.

1. Klik op het blad **aanbevelingen** een aanbeveling.
2. Klik op **toepassen**op de bladeserver **Details** .

    ![Aanbeveling toepassing](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Automatische index-beheer inschakelen

U kunt de SQL-Database Advisor bij de implementatie van aanbevelingen automatisch instellen. Aanbevelingen beschikbaar komen ze automatisch worden toegepast. Als alle handelingen verrichten die index beheerd door de service als de invloed op de prestaties negatief is zal de aanbeveling worden teruggedraaid.

1. Klik op het blad **aanbevelingen** **automatiseren**:

    ![Instellingen](./media/sql-database-advisor-portal/settings.png)

2. De adviseur ingesteld op automatisch **maken** of **Drop** indexen:

    ![Aanbevolen indexen](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>De aanbevolen T-SQL-script uitvoeren handmatig

Selecteer een aanbeveling en klik op **script weergeven**. Dit script uitvoeren op de database handmatig toepassen van de aanbeveling.

*Indexen die handmatig worden uitgevoerd niet worden gecontroleerd en gevalideerd voor de invloed op de prestaties door de service* zodanig wordt aangeraden dat u deze indexen nadat het is gemaakt om te controleren of ze bieden hogere prestaties controleren en passen of te verwijderen indien nodig. Zie voor meer informatie over het maken van indexen, [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### <a name="canceling-recommendations"></a>Annuleren van aanbevelingen

Aanbevelingen die de status **in behandeling** **controleren**of een **succes** kunnen worden geannuleerd. Aanbevelingen met de status van de **uitvoering** kunnen niet worden geannuleerd.

1. Selecteer een aanbeveling in het gedeelte **Geschiedenis afstemmen van** de bladeserver **aanbevelingen details** te openen.
2. Klik op **Annuleren** om af te breken, het proces van de toepassing van de aanbeveling.



## <a name="monitoring-operations"></a>De controles

Toepassen van een aanbeveling kan niet onmiddellijk gebeuren. De portal biedt informatie over de status van de aanbeveling. Mogelijke statussen die een index worden zijn:

| Status | Beschrijving |
| :--- | :--- |
| In behandeling | Aanbeveling opdracht is ontvangen en is gepland voor uitvoering van toepassing. |
| Uitvoeren | De aanbeveling wordt toegepast. |
| Succes | Aanbeveling is toegepast. |
| Fout | Er is een fout opgetreden tijdens het proces van de toepassing van de aanbeveling. Dit is een probleem van voorbijgaande aard, of eventueel een schema om de tabel te wijzigen en het script is niet langer geldig. |
| Terugdraaien | De aanbeveling is toegepast, maar wordt geacht niet zodat automatisch wordt omgezet. |
| Teruggedraaid | De aanbeveling is teruggedraaid. |

Klik op een aanbeveling in het proces in de lijst om meer details te bekijken:

![Aanbevolen indexen](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Een aanbeveling terug

Als u de adviseur de aanbeveling wordt (wat betekent dat u handmatig niet de T-SQL-script hebt uitgevoerd) automatisch overgeschakeld deze als de invloed op de prestaties negatief worden gevonden. Als u om welke reden u gewoon terug wilt naar een aanbeveling kunt u het volgende doen:


1. Selecteer een aanbeveling toegepast in het gedeelte **Geschiedenis afstemmen** .
2. Klik op **herstellen** op de **details van de aanbeveling** -bladeservers.

![Aanbevolen indexen](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Controle van invloed op de prestaties van de aanbevelingen van de index

Nadat de aanbevelingen worden geïmplementeerd (momenteel index bewerkingen en aanbevelingen voor query's alleen voorzien) klikt u op **Query inzichten** op het blad aanbeveling details te openen [Query prestaties inzichten](sql-database-query-performance.md) en Zie de invloed op de prestaties van uw top-query's.

![Invloed op de prestaties controleren](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Samenvatting

SQL Database Advisor bevat aanbevelingen voor het verbeteren van de prestaties van SQL-database. Door middel van T-SQL-scripts en afzonderlijke en volledig-automatische (momenteel index alleen) de advisor biedt nuttige ondersteuning bij het optimaliseren van uw database en uiteindelijk verbeteren van de prestaties van query's.



## <a name="next-steps"></a>Volgende stappen

Controleer uw aanbevelingen en blijven toepassen om de prestaties te verfijnen. Database werkbelasting zijn dynamisch en continu wijzigen. SQL-Database-adviseur blijven en voor de aanbevelingen die mogelijk de prestaties van uw database kunnen verbeteren. 

 - Zie [SQL Database Advisor](sql-database-advisor.md) voor een overzicht van de SQL-Database Advisor.
 - Zie [Prestaties inzichten van Query](sql-database-query-performance.md) voor meer informatie over het weergeven van invloed op de prestaties van uw top-query's.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Query opslaan](https://msdn.microsoft.com/library/dn817826.aspx)
- [INDEX MAKEN](https://msdn.microsoft.com/library/ms188783.aspx)
- [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-configure.md)






