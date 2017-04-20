<properties
    pageTitle="Nieuwe functies in een SQL-Database V12 | Microsoft Azure"
    description="Hierin wordt beschreven waarom bedrijfssystemen Azure SQL-Database gebruikt in de cloud door te upgraden naar versie V12 nu profiteren."
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
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>Nieuwe functies in een SQL-Database V12


Dit onderwerp beschrijft de vele voordelen die de nieuwe V12-versie van Azure SQL-Database over V11: versie heeft.


We blijven V12 onderdelen toevoegen. Dus we je raden Bezoek onze webpagina Service Updates voor Azure en de filters te gebruiken:


- Gefilterd met de [service SQL-Database](https://azure.microsoft.com/updates/?service=sql-database).
- Gefilterd op de algemene beschikbaarheid [(GA) aankondigingen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) voor SQL-Database-functies.


De meest recente informatie over de resource limieten voor SQL-Database wordt beschreven op:<br/>[Azure SQL Database Resource limieten](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>Een grotere compatibiliteit met SQL Server


Een belangrijke doelstelling voor SQL-Database V12 is voor het verbeteren van de compatibiliteit met Microsoft SQL Server 2014, en de om compatibiliteit te behouden als nieuwe versies van SQL Server worden vrijgegeven. Onder andere gebieden realiseert V12 pariteit met SQL Server op het belangrijke gebied van programmeren. Bijvoorbeeld:

- [Ingebouwde ondersteuning voor JSON](https://msdn.microsoft.com/library/dn921897.aspx)

- [Functies venster](http://msdn.microsoft.com/library/ms189798.aspx)met [via](http://msdn.microsoft.com/library/ms189461.aspx)

- [XML-indexen](http://msdn.microsoft.com/library/bb934097.aspx) en [selectieve XML-indexen](http://msdn.microsoft.com/library/jj670104.aspx)

- [Wijzigingen bijhouden](http://msdn.microsoft.com/library/bb933875.aspx)

- [SELECTEER... IN](http://msdn.microsoft.com/library/ms188029.aspx)

- [Zoeken in volledige tekst](http://msdn.microsoft.com/library/ms142571.aspx)

- [ALTER DATABASE binnen bereik van configuratie (Transact-SQL)](http://msdn.microsoft.com/library/mt629158.aspx)

Zie [hier](sql-database-transact-sql-information.md) voor de kleine set functies die nog niet wordt ondersteund in SQL-Database.


### <a name="compatibility-level-130"></a>Het compatibiliteitsniveau van 130


> [AZURE.IMPORTANT] Vanaf **juni 2016**, hebben *nieuwe* databases gemaakt in Azure SQL Database V12 hun compatibiliteit beginnen bij 130, wat overeenkomt met Microsoft SQL Server 2016 GA.
> 
> U kunt `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` als u de voorkeur geeft.
> 
> Databases die zijn gemaakt vóór juni 2016 beschikt niet over het niveau van hun compatibiliteit gewijzigd door deze wijziging van de standaard. Noch is het niveau van een database is gewijzigd door een upgrade uit V11: naar V12.



Voor meer informatie over hoe u uw belangrijkste query's tussen de nieuwste versus vorig compatibiliteitsniveau kunt vergelijken, Zie:

- [Verbeterde prestaties met het niveau van compatibiliteit 130 in Azure SQL-Database](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>Meer topprestaties, nieuwe prestatieniveaus


In V12 verhoogd we de Database transactie-eenheden (DTUs) toegewezen aan alle Premium prestaties met 25%, zonder extra kosten. Nog grotere prestaties kunnen worden bereikt met nieuwe functies zoals:


- Ondersteuning voor in het geheugen, [columnstore-indexen](http://msdn.microsoft.com/library/gg492153.aspx).
- [Partitionering van de tabel door rijen](http://msdn.microsoft.com/library/ms187802.aspx) met gerelateerde verbeteringen in de [Tabel afkappen](http://msdn.microsoft.com/library/ms177570.aspx).
- De beschikbaarheid van dynamic management views [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) om te controleren en afstemmen van de prestaties.


### <a name="reliable-performance"></a>Betrouwbare prestaties


Als uw clientprogramma verbinding maakt met SQL Database V12 terwijl de client wordt uitgevoerd op een Azure VM (virtual machine), moet u de volgende bereikwaarden op de VM openen:

- 11000 11999
- 14000 14999


Klik [hier](sql-database-develop-direct-route-ports-adonet-v12.md) voor meer informatie over de poorten voor SQL-Database V12. De poorten zijn door de verbeterde prestaties in SQL Database V12 nodig.


## <a name="better-support-for-cloud-saas-vendors"></a>Betere ondersteuning voor cloud SaaS-leveranciers


Wij hebben uitgegeven alleen in V12, het nieuwe standaarden voor prestaties S3- en de public preview van [elastische database toepassingen](sql-database-elastic-pool.md). Elastische database van toepassingen is een oplossing die ontworpen zijn voor cloud SaaS leveranciers.  U kunt met elastische database-toepassingen:


- DTUs delen tussen databases te verlagen voor een groot aantal databases.
- [Elastische database taken](sql-database-elastic-jobs-overview.md) voor het beheren van databases op schaal worden uitgevoerd.


## <a name="security-enhancements"></a>Verbeterde beveiliging


Beveiliging is een belangrijk probleem voor iedereen die hun bedrijf in de cloud wordt uitgevoerd. De nieuwste beveiligingsfuncties uitgebracht in V12 omvatten:


- [Beveiliging](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [Dynamische gegevens maskeren](sql-database-dynamic-data-masking-get-started.md)
- [De opgenomen databases](http://msdn.microsoft.com/library/ff929188.aspx)
- [Toepassingsrollen](http://msdn.microsoft.com/library/ms190998.aspx) worden beheerd met het verlenen, weigeren, INTREKKEN
- [Transparante gegevenscodering](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [Verbinding maken met de SQL-Database via Azure Active Directory-verificatie](sql-database-aad-authentication.md)
 - Azure Active Directory-verificatie, een mechanisme om verbinding te maken met SQL-Database met behulp van identiteiten in Azure Active Directory (AD Azure) biedt nu ondersteuning voor SQL-Database. U kunt de identiteit van gebruikers en andere Microsoft-diensten op één centrale locatie centraal beheren met Azure Active Directory-verificatie.
- [Altijd gecodeerd](https://msdn.microsoft.com/library/mt163865.aspx) (in het voorbeeld) wordt codering transparant voor toepassingen en kunnen clients vertrouwelijke gegevens te coderen in toepassingen zonder de coderingssleutels delen met SQL-Database.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Bedrijfscontinuïteit verhoogd wanneer herstel noodzakelijk is


V12 biedt verbeterde herstel punt doelstellingen (productoutput) en herstel geschatte tijden (ERTs):


| Business continuity-functie | Eerdere versie | V12 |
| :-- | :-- | :-- |
| Geo-terugzetten | • Vrijgegeven Productieorder < 24 uur.<br/>• Invoegen < 12 uur. | • Vrijgegeven Productieorder < 1 uur.<br/>• Invoegen < 12 uur. |
| Actieve Geo-replicatie | • Vrijgegeven Productieorder < 5 minuten.<br/>• Invoegen < 1 uur. | • Vrijgegeven Productieorder < 5 seconden.<br/>• Invoegen < 30 seconden. |


Zie de [bedrijfscontinuïteit van de SQL-Database](sql-database-business-continuity.md) voor meer informatie.


## <a name="more-reasons-to-upgrade-now"></a>Meer redenen om te upgraden nu


Er zijn veel goede redenen waarom klanten nu bijwerken naar Azure SQL Database V12 uit V11 moeten::


- SQL Database V12 heeft een lange lijst met functies van V11: dan.
- We blijven nieuwe functies toevoegen aan V12, maar wordt er geen nieuwe functies toegevoegd aan V11:.
- De meeste nieuwe functies worden uitgebracht op de SQL-Database V12 voordat ze worden gepubliceerd voor Microsoft SQL Server.


## <a name="are-you-using-v12-already"></a>Gebruikt u V12 al?


Een eenvoudige manier om te zien of er een logische server met een eerdere versie van de service SQL-Database of een database is het volgende doen:


1. Ga naar de [Portal Azure](https://portal.azure.com/).
2. Klik op **Bladeren**.
3. Klik op **SQL-Servers**.
4. Het pictogram naast uw server of database vertelt het verhaal:
 - ![Pictogram voor een v12-server](./media/sql-database-v12-whats-new/v12_icon.png) **V12 logische server**
 - ![Pictogram voor een eerdere versie van server](./media/sql-database-v12-whats-new/earlier_icon.png) **eerdere versie logische server**


Een andere methode om na te gaan van de versie uit te voeren is de `SELECT @@version;` -instructie in uw database, en u ziet de resultaten vergelijkbaar met:


- **12**-.0.2000.10 &nbsp; *(versie V12)*
- **11**.0.9228.18 &nbsp; *(V11: versie)*


Een V12-database kan worden gehost op een logische server V12. En een V12-server alleen V12 databases kunt onderbrengen.


Als u nog niet worden uitgevoerd op V12, kunt u de logische server bijwerken door de stappen in [een Upgrade naar SQL Database V12 in plaats](sql-database-v12-plan-prepare-upgrade.md).


## <a name="V12AzureSqlDbPreviewGaTable"></a>Algemene beschikbaarheid van regio 's


- Op 31 juli 2015, was alle regio's gepromoveerd tot algemene beschikbaarheid (GA).
- V12 is uitgebracht in December 2014, maar alleen op de status van een voorbeeld.

[Aanvullende gebruiksrechtovereenkomst voor Microsoft Azure voorbeelden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
