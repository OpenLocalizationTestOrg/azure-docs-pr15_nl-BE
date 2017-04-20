<properties
    pageTitle="Verbinding maken met de SQL-Database met behulp van Java met JDBC op Windows | Microsoft Azure"
    description="Geeft een voorbeeld van Java-code die kunt u verbinding maken met Azure SQL-Database. In dit voorbeeld worden de JDBC en wordt uitgevoerd op een Windows-clientcomputer."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Verbinding maken met de SQL-Database met behulp van Java met JDBC in Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Dit onderwerp bevat een codevoorbeeld in Java kunt u verbinding maken met Azure SQL-Database. Het monster van Java is gebaseerd op de Java Development Kit (JDK) versie 1.8. Het monster maakt verbinding met een Azure SQL-Database via JDBC-stuurprogramma.

## <a name="step-1--configure-development-environment"></a>Stap 1: Ontwikkelingsomgeving configureren

[Configureren voor de ontwikkeling van Java-ontwikkelomgeving](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Stap 2: Maak een SQL-database

Zie de [pagina aan de slag](sql-database-get-started.md) voor meer informatie over het maken van een database.  

## <a name="step-3-get-connection-string"></a>Stap 3: De verbindingsreeks ophalen

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Als u de JTDS JDBC-stuurprogramma gebruikt, wordt u wilt toevoegen ' ssl = vereist "aan de URL van de verbinding string en u moet de volgende optie instellen voor JVM"-Djsse.enableCBCProtection=false ". Deze optie JVM een oplossing voor een beveiligingsprobleem wordt uitgeschakeld, dus zorg ervoor dat u begrijpt welke risico's voor het instellen van deze optie is betrokken.

## <a name="step-4-run-sample-code"></a>Stap 4: Voorbeeldcode uitvoeren

* [Verbinden met SQL met behulp van Java concepttest](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Volgende stappen

* Ga naar de [Java Developer Center](/develop/java/).
* Bekijk het [overzicht van SQL-Database ontwikkeling](sql-database-develop-overview.md)
* Meer informatie over het [Microsoft JDBC-stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Aanvullende bronnen 

* [Ontwerppatronen voor meerdere huurder SaaS-toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ontdek alle [mogelijkheden van SQL-Database](https://azure.microsoft.com/services/sql-database/)
