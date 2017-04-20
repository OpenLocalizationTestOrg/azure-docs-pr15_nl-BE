<properties
    pageTitle="Kopiëren met behulp van de portal Azure Azure SQL-database | Microsoft Azure"
    description="Maak een kopie van een Azure SQL-database"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Azure SQL-Database via de portal Azure kopiëren

> [AZURE.SELECTOR]
- [Overzicht](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

De volgende stappen laten zien hoe een SQL-database met [Azure portal](https://portal.azure.com) op dezelfde server of een andere server kopiëren.

Als u wilt kopiëren van een SQL-database, moet u de volgende items:

- Een abonnement op Azure. Als u een abonnement op Azure gewoon **Gratis** boven aan deze pagina op en teruggaan naar het voltooien van dit artikel.
- Een SQL-database kopiëren. Als u niet een SQL-database, maken een volgens de stappen in dit artikel: [de eerste Azure SQL-Database maken](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Kopieer de SQL-database

Open de pagina SQL-database voor de database die u wilt kopiëren:

1.  Ga naar de [portal Azure](https://portal.azure.com).
2.  Klik op **meer services** > **SQL-databases**, en klik vervolgens op de gewenste database.
3.  Klik op de pagina SQL-database **kopiëren**:

    ![SQL-Database](./media/sql-database-copy-portal/sql-database-copy.png)

1.  Op de pagina **kopiëren** , wordt de naam van een database standaard geleverd. Typ een andere naam als u (alle databases op de server een unieke namen moeten hebben).
2.  Selecteer een **doelserver**. De doelserver is waar de databasekopie wordt gemaakt. U kunt de database op dezelfde server of op een andere server kopiëren. U kunt een server maken of een bestaande server uit de lijst selecteren. 
3.  Nadat de **doelserver**de **elastische database toepassingen**en **prijzen laag** opties zijn ingeschakeld. Als de server een pool heeft, kunt u de database in het kopiëren.
3.  Klik op **OK** om het kopiëren te starten.

    ![SQL-Database](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>De voortgang van de kopieerbewerking

- Na het kopiëren wordt gestart, klikt u op de melding portal voor meer informatie.

    ![kennisgeving][3]
 
    ![monitor][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Controleer of de database op de server

- Klik op **meer services** > **SQL-databases** en controleer of de nieuwe database **on line**is.


## <a name="resolve-logins"></a>Aanmeldingen oplossen

Raadpleeg [oplossen aanmeldingen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes) aanmeldingen oplossen nadat de bewerking voor het kopiëren is voltooid


## <a name="next-steps"></a>Volgende stappen

- Zie [een Azure SQL-database kopiëren](sql-database-copy.md) voor een overzicht van het kopiëren van een SQL-Database Azure.
- Zie [kopie van een database SQL Azure PowerShell gebruiken](sql-database-copy-powershell.md) voor het kopiëren van een database met PowerShell.
- Zie [een Azure SQL-database kopiëren met behulp van T-SQL](sql-database-copy-transact-sql.md) voor het kopiëren van een database met behulp van Transact-SQL.
- Zie [het beheren van Azure SQL databasebeveiliging na noodherstel](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van gebruikers en aanmeldingen bij het kopiëren van een database naar een andere logische server.



## <a name="additional-resources"></a>Aanvullende bronnen

- [Aanmeldingen beheren](sql-database-manage-logins.md)
- [Verbinding maken met de SQL-Database met SQL Server Management Studio en een steekproef T-SQL-query uitvoeren](sql-database-connect-query-ssms.md)
- [De database exporteren naar een BACPAC](sql-database-export.md)
- [Business Continuity-overzicht](sql-database-business-continuity.md)
- [Documentatie voor SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

