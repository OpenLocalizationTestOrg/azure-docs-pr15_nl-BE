<properties
   pageTitle="Transparante gegevenscodering (TDE) voor uitrekken Database van SQL Server op Azure TSQL inschakelen | Microsoft Azure"
   description="Transparante gegevenscodering (TDE) voor uitrekken Database van SQL Server op Azure TSQL inschakelen"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Transparante gegevenscodering (TDE) inschakelen voor uitrekken Database op Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparante gegevens codering (TDE) biedt beveiliging tegen het gevaar van schadelijke activiteiten door middel van real-time codering en decodering van de database, back-ups van gekoppeld en transactielogboekbestanden in rust zonder wijzigingen in de toepassing.

De opslag van een volledige database codeert TDE met behulp van een symmetrische sleutel die de database coderingssleutel genoemd. De coderingssleutel voor de database is beveiligd met een ingebouwde server-certificaat. De ingebouwde servercertificaat is uniek voor elke server Azure. Microsoft draait deze certificaten automatisch ten minste elke 90 dagen. Zie voor een algemene beschrijving van TDE [Transparante gegevens codering (TDE)].

##<a name="enabling-encryption"></a>Codering inschakelen

Om TDE inschakelen voor een Azure database voor het opslaan van de gegevens uit een database van SQL Server uitrekken ingeschakeld gemigreerd, het volgende doen:

1. Verbinding maken met de *master* -database op de Azure-server die als host optreedt voor de database met behulp van een aanmelding die een beheerder of een lid van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het coderen van de database.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Codering uitschakelen

TDE uitschakelen voor een Azure database voor het opslaan van de gegevens uit een database van SQL Server uitrekken ingeschakeld gemigreerd, het volgende doen:

1. Verbinding maken met de *master* -database met behulp van een aanmelding die een beheerder of een lid van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het coderen van de database.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Codering controleren

Als u wilt controleren of de status van de codering voor een Azure database voor het opslaan van de gegevens gemigreerd van uitrekken ingeschakeld SQL Server-database, het volgende doen:

1. Verbinding maken met het *model* of de instantie database met behulp van een aanmelding die een beheerder of een lid van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het coderen van de database.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Een resultaat van ```1``` geeft u een versleutelde database ```0``` geeft aan dat een niet-versleutelde database.


<!--Anchors-->
[Transparante gegevenscodering (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
