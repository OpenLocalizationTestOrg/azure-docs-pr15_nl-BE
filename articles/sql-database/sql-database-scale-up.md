<properties
    pageTitle="Het serviceniveau voor laag en de prestaties van een Azure SQL-database wijzigen | Microsoft Azure"
    description="Wijzig de servicelaag en prestatieniveau van Azure SQL-database ziet u hoe u de SQL-database geschaald omhoog of omlaag. Het wijzigen van de prijzen laag van een Azure SQL-database."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>De laag en prestaties serviceniveau (prijzen laag) van een SQL-database via de portal Azure wijzigen


> [AZURE.SELECTOR]
- [**Azure portal**](sql-database-scale-up.md)
- [PowerShell](sql-database-scale-up-powershell.md)


Service niveaus en prestatieniveaus beschrijven de functies en bronnen die beschikbaar zijn voor uw SQL-database en kunnen worden bijgewerkt als de behoeften van uw toepassing wijzigen. Zie voor meer informatie, [Service Tiers](sql-database-service-tiers.md).

Veranderen van het niveau van de service en/of prestatieniveau van een database een replica van de oorspronkelijke database maakt op het nieuwe prestatieniveau van en vervolgens overgeschakeld verbindingen in de replica. Er zijn geen gegevens verloren is gegaan tijdens dit proces, maar tijdens het korte moment wanneer we naar de replica overschakelen, verbindingen met de database zijn uitgeschakeld, zodat sommige transacties tijdens de vlucht kunnen worden teruggedraaid. Dit venster kan variëren, maar is gemiddeld binnen 4 seconden en is in meer dan 99% van de gevallen minder dan 30 seconden. Heel af en toe vooral als er grote aantallen transacties tijdens de vlucht op het moment dat verbindingen zijn uitgeschakeld, dit venster kan niet langer.  

De duur van het hele proces van schaal-up is afhankelijk van de grootte en service niveaus van de database vóór en na de wijziging. Bijvoorbeeld, voltooid een 250 GB-database wordt gewijzigd naar, vanuit of binnen een laag standaard service binnen 6 uur. Voor een database van dezelfde grootte prestatieniveaus binnen de tier Premium service wordt gewijzigd, moet er binnen drie uur uitgevoerd.


Gebruik de informatie in [Azure SQL Database Service Tiers en prestaties](sql-database-service-tiers.md) op het juiste niveau en de prestaties serviceniveau voor uw Azure SQL-Database bepaalt.

- Als u wilt wijzigen in een database, moet de database kleiner zijn dan de maximale toegestane grootte van de doel-tier-service. 
- Bij het upgraden van een database met [Geo-replicatie](sql-database-geo-replication-overview.md) is ingeschakeld, moet u eerst de secundaire databases upgraden naar de gewenste prestaties laag voordat u een upgrade van de primaire database.
- Wanneer downgraden van de servicelaag van een, moet u eerst alle relaties van Geo-replicatie beëindigen. 
- Het serviceaanbod terugzetten zijn verschillend voor de verschillende niveaus van de service. Als u bent u downgraden kunnen verliezen de mogelijkheid om te zetten naar een punt in de tijd, of hebben een lagere bewaartermijn voor back-up. Zie [Azure SQL Database back-up en terugzetten](sql-database-business-continuity.md)voor meer informatie.
- De prijzen laag database verandert niet de maximale databasegrootte. Als u wilt uw database wijzigen gebruik maximaal [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) of [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- De nieuwe eigenschappen van de database worden pas toegepast als de wijzigingen voltooid zijn.



**Voor het voltooien van dit artikel hebt u het volgende nodig:**

- Azure SQL-database. Als u niet een SQL-database, maken een volgens de stappen in dit artikel: [de eerste Azure SQL-Database maken](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Het serviceniveau voor laag en de prestaties van uw database wijzigen


Open het SQL-Database-blade voor de database die u schalen wilt, omhoog of omlaag:

1.  Klik op **meer services**in [Azure portal](https://portal.azure.com) > **SQL-databases**.
2.  Klik op de database die u wilt wijzigen.
3.  Klik op de **SQL-database** -blade **prijzen laag (schaal DTUs)**:

    ![prijzen laag][1]

1.  Kies een nieuwe laag en klik op **selecteren**:

    Te klikken op **selecteren** , dient een aanvraag schaal de prijzen laag te wijzigen. De schaal bewerking kan enige tijd duren om te voltooien (Zie de informatie die aan het begin van dit artikel) afhankelijk van de grootte van de database.

    > [AZURE.NOTE] De prijzen laag database verandert niet de maximale databasegrootte. Als u wilt uw database wijzigen gebruik maximaal [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) of [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).

    ![prijzen laag selecteren][2]

3.  Klik op pictogram in het systeemvak (bel), in de rechterbovenhoek:

    ![meldingen][3]

    Klik op de tekst van het bericht u opent het deelvenster met details waar u de status van de aanvraag ziet.




## <a name="next-steps"></a>Volgende stappen

- De maximale grootte van de database met behulp van [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) of [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx)wijzigen.
- [Uit- en schalen](sql-database-elastic-scale-get-started.md)
- [Verbinding maken en een SQL-database met SSMS](sql-database-connect-query-ssms.md)
- [Azure SQL-database exporteren](sql-database-export.md)

## <a name="additional-resources"></a>Aanvullende bronnen

- [Business Continuity-overzicht](sql-database-business-continuity.md)
- [Documentatie voor SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
