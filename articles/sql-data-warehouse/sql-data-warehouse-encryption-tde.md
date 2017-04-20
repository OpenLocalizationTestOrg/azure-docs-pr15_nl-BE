<properties
   pageTitle="Transparante gegevenscodering in SQL datawarehouse (Portal) | Microsoft Azure"
   description="Transparante gegevenscodering (TDE) in SQL datawarehouse"
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

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Aan de slag met transparante gegevens codering (TDE) in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Beveiliging-overzicht](sql-data-warehouse-overview-manage-security.md)
- [Verificatie](sql-data-warehouse-authentication.md)
- [Codering (Portal)](sql-data-warehouse-encryption-tde.md)
- [Codering (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Vereiste bevoegdheden

Om transparante gegevens codering (TDE) inschakelt, moet u een beheerder of als lid van de rol van dbmanager zijn.

## <a name="enabling-encryption"></a>Codering inschakelen

Volg de onderstaande stappen zodat TDE voor een datawarehouse SQL:

1. Open de database in de [Azure portal](https://portal.azure.com)
2. Klik op de knop **Instellingen** in de database-blade
3. Selecteer de optie **transparant gegevenscodering**![][1]
4. Selecteer de instelling **op**![][2]
5. Selecteer **Opslaan**
![][3]  

## <a name="disabling-encryption"></a>Codering uitschakelen

TDE uitschakelen voor een datawarehouse SQL, volg de onderstaande stappen:

1. Open de database in de [Azure portal](https://portal.azure.com)
2. Klik op de knop **Instellingen** in de database-blade
3. Selecteer de optie **transparant gegevenscodering**![][1]
4. Selecteer de instelling **uitschakelen**![][4]
5. Selecteer **Opslaan**
![][5]  

## <a name="encryption-dmvs"></a>Codering DMVs

Codering kan worden bevestigd met de volgende DMVs:

- [sys.databases.]
- [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases.]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
