<properties
    pageTitle="Verbinding maken met de SQL-Database met behulp van PHP op Windows | Microsoft Azure"
    description="Geeft een voorbeeld PHP-programma dat verbinding maakt met Azure SQL-Database van een Windows-client en koppelingen naar de benodigde software-onderdelen die nodig zijn voor de client."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Verbinding maken met de SQL-Database met behulp van PHP op Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Dit onderwerp wordt beschreven hoe u verbinding kunt maken met Azure SQL-Database van een clienttoepassing geschreven in PHP die compatibel is met Windows.

## <a name="step-1--configure-development-environment"></a>Stap 1: Ontwikkelingsomgeving configureren

[Configureren voor de ontwikkeling van een PHP-ontwikkelomgeving](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Stap 2: Maak een SQL-database

Zie de [pagina aan de slag](sql-database-get-started.md) voor meer informatie over het maken van een voorbeelddatabase.  Het is belangrijk dat u de richtlijnen voor het maken van een **database AdventureWorks-sjabloon**volgen. De onderstaande voorbeelden werken alleen met de **AdventureWorks-schema**.


## <a name="step-3-get-connection-details"></a>Stap 3: Verbindingsgegevens ophalen

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Stap 4: Voorbeeldcode uitvoeren

* [Verbinden met SQL met PHP concepttest](https://msdn.microsoft.com/library/mt720665.aspx)
* [Verbinding resiliently met PHP](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Volgende stappen

* Bekijk het [overzicht van SQL-Database ontwikkeling](sql-database-develop-overview.md)
* Meer informatie over het [Microsoft PHP-stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/dn865013.aspx)
* Zie voor meer informatie over PHP installatie en het gebruik van [SQL Server-Databases benaderen met PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Aanvullende bronnen 

* [Ontwerppatronen voor meerdere huurder SaaS-toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ontdek alle [mogelijkheden van SQL-Database](https://azure.microsoft.com/services/sql-database/)
