<properties
    pageTitle="Azure SQL-database kopiëren | Microsoft Azure"
    description="Maak een kopie van een Azure SQL-database"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Azure SQL-Database kopiëren

> [AZURE.SELECTOR]
- [Overzicht](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

U kunt de Azure [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md) een kopie maken van de SQL-database. De databasekopie maakt gebruik van dezelfde technologie als de functie voor geo-replicatie. Maar in tegenstelling tot geo-replicatie beëindigt de replicatiekoppeling als eenmaal de seeding fase is voltooid. De database kopiëren is daarom een momentopname van de brondatabase op het tijdstip van de copy-aanvraag.  
U kunt de databasekopie maken op dezelfde server of op een andere server. Het niveau en de prestaties serviceniveau (prijzen laag) van het database-exemplaar zijn hetzelfde als de brondatabase standaard. Wanneer u de API gebruikt, kunt u een prestatieniveau verschillende binnen hetzelfde service niveau (editie). Nadat het kopiëren voltooid is, wordt de kopie een volwaardige, onafhankelijke database. U kunt op dit moment upgraden of het downgraden naar een willekeurige editie. De aanmeldingen, gebruikers en machtigingen kunnen afzonderlijk worden beheerd.  

Wanneer u een database met dezelfde logische server kopieert, kunnen de aanmeldingen hetzelfde op beide databases worden gebruikt. De beveiligings-principal dat u gebruiken om de database te kopiëren, wordt de database-eigenaar (DBO) voor de nieuwe database. Alle gebruikers, hun machtigingen en hun beveiligings-id's (SID's) worden gekopieerd naar de databasekopie.  

Wanneer u een database naar een andere logische server kopieert, wordt de beveiligings-principal op de nieuwe server eigenaar van de database op de nieuwe database. Als u [gebruikers die zich](sql-database-manage-logins.md) voor hebt toegang tot gegevens zowel primaire als secundaire databases altijd dezelfde gebruikersreferenties zodat na het kopiëren is voltooid u onmiddellijk toegang hebt met dezelfde referenties. Als u [Azure Active Directory](../active-directory/active-directory-whatis.md)gebruikt, kunt u de noodzaak voor het beheren van referenties in de kopie volledig te elimineren. Echter wanneer u de database naar een nieuwe server kopieert, werkt de aanmelding op basis van access over het algemeen niet omdat de aanmeldingen niet op de nieuwe server bestaat. Zie [het beheren van Azure SQL databasebeveiliging na noodherstel](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van aanmeldingen bij het kopiëren van een database naar een andere logische server. 

Als u wilt kopiëren van een SQL-database, moet u het volgende:

- Een abonnement op Azure. Als u een abonnement op Azure gewoon **Gratis** boven aan deze pagina op en teruggaan naar het voltooien van dit artikel.
- Een SQL-database kopiëren. Als u niet een SQL-database, maken een volgens de stappen in dit artikel: [de eerste Azure SQL-Database maken](sql-database-get-started.md).

## <a name="next-steps"></a>Volgende stappen

- Zie [met behulp van de portal Azure Azure SQL-database kopiëren](sql-database-copy-portal.md) voor het kopiëren van een database met behulp van de portal Azure.
- Zie [kopie van een database SQL Azure PowerShell gebruiken](sql-database-copy-powershell.md) voor het kopiëren van een database met PowerShell.
- Zie [een Azure SQL-database kopiëren met behulp van T-SQL](sql-database-copy-transact-sql.md) voor het kopiëren van een database met behulp van Transact-SQL.
- Zie [het beheren van Azure SQL databasebeveiliging na noodherstel](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van gebruikers en aanmeldingen bij het kopiëren van een database naar een andere logische server.



## <a name="additional-resources"></a>Aanvullende bronnen

- [Aanmeldingen beheren](sql-database-manage-logins.md)
- [Verbinding maken met de SQL-Database met SQL Server Management Studio en een steekproef T-SQL-query uitvoeren](sql-database-connect-query-ssms.md)
- [De database exporteren naar een BACPAC](sql-database-export.md)
- [Business Continuity-overzicht](sql-database-business-continuity.md)
- [Documentatie voor SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)
