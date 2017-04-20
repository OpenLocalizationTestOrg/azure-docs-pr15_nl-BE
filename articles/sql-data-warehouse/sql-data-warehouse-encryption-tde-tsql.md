<properties
   pageTitle="Transparante gegevenscodering in SQL datawarehouse (T-SQL) | Microsoft Azure"
   description="Transparante gegevenscodering (TDE) in SQL datawarehouse (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Aan de slag met transparante gegevens codering (TDE)


> [AZURE.SELECTOR]
- [Beveiliging-overzicht](sql-data-warehouse-overview-manage-security.md)
- [Verificatie](sql-data-warehouse-authentication.md)
- [Codering (Portal)](sql-data-warehouse-encryption-tde.md)
- [Codering (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Vereiste bevoegdheden

Om transparante gegevens codering (TDE) inschakelt, moet u een beheerder of als lid van de rol van dbmanager zijn.

## <a name="enabling-encryption"></a>Codering inschakelen

Volg deze stappen om TDE inschakelen voor een datawarehouse SQL:

1. Verbinding maken met de *master* -database op de server waarop de database met behulp van een aanmelding die een beheerder of een lid van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het coderen van de database.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Codering uitschakelen

Volg deze stappen om TDE uitschakelen voor een datawarehouse SQL:

1. Verbinding maken met de *master* -database met behulp van een aanmelding die een beheerder of een lid van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het coderen van de database.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Voordat u wijzigingen in de instellingen TDE moet een onderbroken SQL Data Warehouse worden hervat.

## <a name="verifying-encryption"></a>Codering controleren

Voer de volgende stappen uit om te controleren status codering voor een datawarehouse SQL:

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

## <a name="encryption-dmvs"></a>Codering DMVs  

- [sys.databases.][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases.]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
