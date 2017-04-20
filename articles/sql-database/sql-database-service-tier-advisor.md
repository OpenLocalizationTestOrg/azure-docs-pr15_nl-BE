<properties 
   pageTitle="Prijzen laag aanbevelingen voor Azure SQL-Database" 
   description="Bij het wijzigen van prijzen zijn lagen in de prijzen laag aanbevelingen Azure portal mits aanbevelen de laag die het best geschikt is voor het uitvoeren van een bestaande Azure SQL-Database van werkbelasting. Prijzen lagen beschrijven het serviceniveau voor laag en de prestaties van een SQL-database." 
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
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>SQL-Database prijzen laag aanbevelingen

 Prijzen laag aanbevelingen voorstellen het niveau van service tier en prestaties die optimaal geschikt is voor het uitvoeren van de werklast van een bestaande Azure SQL-database.

> [AZURE.NOTE] Prijzen laag aanbevelingen zijn alleen beschikbaar voor Web- en databases en toepassingen elastische database-- en is alleen beschikbaar in de [portal Azure](https://portal.azure.com/).


Krijg aanbevelingen laag prijzen tijdens de volgende taken:

- [De laag en prestaties serviceniveau (prijzen laag) van een SQL-database wijzigen](sql-database-scale-up.md)
- [Azure SQL server bijwerken naar V12](sql-database-upgrade-server-portal.md)
- Ga naar uw server V12. Zie de [SQL-Database prijzen laag aanbevelingen](sql-database-service-tier-advisor.md).
- [Maak een elastische database toepassingen](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>Overzicht

De service SQL-Database analyseert de huidige prestaties en functie-eisen door de beoordeling van de historische Resourcegebruik voor een SQL-database. Bovendien is de minimaal aanvaardbare servicelaag bepaald op basis van de grootte van de database en de [bedrijfscontinuïteit](sql-database-business-continuity.md) van de ingeschakelde functies. 

Deze informatie wordt geanalyseerd en het niveau van service niveau en de prestaties die het best geschikt is voor het uitvoeren van de normale werklast van de database en handhaving van dat het huidige functieset is wordt aanbevolen.

- De service de laatste 15 tot 30 dagen van historische gegevens (Resourcegebruik, grootte van de database en database-activiteit) wordt onderzocht en een vergelijking tussen het bedrag van de resources is verbruikt en de werkelijke beperkingen van de momenteel beschikbare service niveaus en prestaties.
- Gegevens worden geanalyseerd met intervallen van 15 tweede resultaatset van elk interval is onderverdeeld in de bestaande servicelaag en prestatieniveau die het best geschikt is voor het verwerken van deze resultaatset werkbelasting.
- Deze tweede 15 monsters worden vervolgens samengevoegd in de grotere 15-30 dagen-analyse en het niveau en de prestaties serviceniveau dat 95% van de historische werkbelasting kan optimaal worden verwerkt wordt aanbevolen.

### <a name="recommendations"></a>Aanbevelingen

Op basis van het gebruik van uw database, zijn er momenteel 2 categorieën van aanbevelingen die u kunnen tegenkomen:


| Aanbeveling | Beschrijving |
| :--- | :--- |
| Upgrade | Upgrade naar een nieuwe laag. |
| Niet beschikbaar | Een database vereist een minimale werklast of ongeveer 35 dagen van activiteit. Het is niet voldoende gegevens om een geldige aanbeveling. |

## <a name="getting-pricing-tier-recommendations"></a>Prijzen laag aanbevelingen ophalen

Prijzen laag aanbevelingen door het selecteren van een bestaande Web- of database ophalen, **alle instellingen**Klik op **prijzen laag (schaal DTUs)**. (Prijzen laag aanbevelingen zijn ook beschikbaar wanneer u een [Upgrade van Azure SQL server V12](sql-database-upgrade-server-portal.md).)

1. Log in om de [Azure portal](https://portal.azure.com/).
2. Klik op **Bladeren** > **SQL-databases**.
4. Klik op de database die u wilt zien van een aanbeveling voor in de blade **SQL-databases** :

    ![Database selecteren][1]

5. Op het blad database selecteert u **alle instellingen** en selecteer vervolgens **prijzen laag (schaal DTUs)**.


7. De **aanbevolen niveaus prijsstelling** open waar klikt u op de voorgestelde laag en klik vervolgens op de knop **selecteren** om te wijzigen in die laag.

    ![Meld u aan voor de voorvertoning][4]

8. Klik eventueel op **details van gebruik** om de **Prijzen laag aanbeveling Details** blade waar u het aanbevolen niveau voor de database, een functie vergelijking tussen huidige en aanbevolen niveaus en een grafiek van de gebruiksanalyse historische bron zien kunt openen.

    ![Meld u aan voor de voorvertoning][5]



## <a name="summary"></a>Samenvatting

Prijzen laag aanbevelingen bieden een geautomatiseerde ervaring voor het verzamelen van telemetriegegevens voor elke SQL-database en de beste service tier-prestatie-niveau combinatie op basis van de behoeften van de werkelijke prestaties van een database en de functie-eisen aanraden. Klik op de instellingen voor blade **prijzen laag (DTUs schaal)** voor prijzen laag aanbevelingen voor Web- en -databases.



## <a name="next-steps"></a>Volgende stappen

Afhankelijk van de details van uw specifieke database uitvoeren van een upgrade of een downgrade meestal niet het geval onmiddellijk. De portal biedt meldingen als de overgangen van de database naar de nieuwe laag of u kunt de bijwerkstatus controleren bij het controleren van de weergave [(SQL-Database Azure) sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) in de hoofddatabase van de SQL-databaseserver.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
