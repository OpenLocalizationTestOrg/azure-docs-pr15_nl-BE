<properties
    pageTitle="Een enkele tabel Azure SQL Database back-up terugzetten | Microsoft Azure"
    description="Informatie over één tabel Azure SQL Database back-up terugzetten."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Een enkele tabel uit een Azure SQL Database back-up terugzetten

Een situatie waarin u bepaalde gegevens in een SQL-database per ongeluk hebt gewijzigd en u wilt nu hersteld met behulp van één waarin dit probleem optreedt kunnen optreden. In dit artikel wordt beschreven hoe een enkele tabel in een database terugzetten vanaf een van de SQL-Database [automatisch back-ups](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Voorbereidende stappen: naam van de tabel en een kopie van de database terugzetten
1. Identificatie van de tabel in uw Azure SQL-database die u wilt vervangen door de herstelde kopie. Microsoft SQL Management Studio om de naam van de tabel te gebruiken. Wijzig bijvoorbeeld de tabel &lt;naam van de tabel&gt;_old.

    **Opmerking** Om te voorkomen dat wordt geblokkeerd, zorg dat er geen activiteit uitvoeren op de tabel waarin u de naam wijzigt. Als u problemen ondervindt, zorg dat deze procedure uitvoeren tijdens een onderhoudsvenster.

2. Een back-up van de database herstellen naar een punt in de tijd die u wilt herstellen met behulp van de stappen voor het [Herstellen van punt In_Time](sql-database-recovery-using-backups.md#point-in-time-restore) .

    **Notities**:
    - De naam van de teruggezette database zal worden in de indeling DBName + TimeStamp. bijvoorbeeld: **Adventureworks2012_2016-01-01T22-12Z**. Deze stap overschrijft geen naam van de bestaande database op de server. Dit is een maatstaf voor de veiligheid en het is bedoeld om te controleren of de teruggezette database voordat ze hun huidige database verwijderen en de naam van de teruggezette database voor gebruik in productie.
    - Alle prestaties lagen van standaardvolumes naar Premium wordt automatisch een back-up van de dienst, met variërende back-inhouding metrics, afhankelijk van de laag:

| DB herstellen | Basis laag | Standaard niveaus | Premium-lagen |
| :-- | :-- | :-- | :-- |
|  Tijdstip herstellen |  Een herstelpunt binnen 7 dagen|Een herstelpunt in 35 dagen| Een herstelpunt in 35 dagen|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>De tabel kopiëren van de teruggezette database met behulp van het hulpprogramma SQL-Database migreren
1. Download en installeer de [Wizard SQL-Database migreren](https://sqlazuremw.codeplex.com).

2. Open de Wizard SQL-Database migreren, op de pagina **Selecteer proces** , selecteert u **de Database onder analyseren/migreren**en klik op **volgende**.
![Wizard SQL databasemigratie - proces selecteren](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. Klik in het dialoogvenster **verbinding maken met Server** gelden de volgende instellingen:
 - **Naam**: uw SQL Azure exemplaar
 - **Verificatie**: **SQL Server-verificatie**. Geef uw aanmeldingsgegevens.
 - **Database**: **Master DB (lijst met alle databases)**.
 - **Opmerking** Standaard slaat de wizard uw aanmeldingsgegevens. Als u niet wilt, selecteert u **Aanmeldingsgegevens bent vergeten**.
![SQL Database Migration wizard - bron selecteren - stap 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. Selecteer de naam herstelde database in de sectie **voorbereidende stappen** als de bron in het dialoogvenster **Gegevensbron selecteren** en klik op **volgende**.

    ![SQL Database Migration wizard - bron selecteren - stap 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. Selecteer de optie **specifieke databaseobjecten selecteren** in het dialoogvenster **Kies objecten** en selecteer de table(or tables) die u wilt migreren naar de doelserver.
![Wizard migratie van SQL-Database - objecten kiezen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. Op de pagina **Samenvatting van de Wizard Script** , klik op **Ja** wanneer u wordt gevraagd over de vraag of u nu een SQL-script genereren. U hebt ook de optie voor het opslaan van het Script TSQL voor later gebruik.
![Wizard SQL-Database migreren - overzicht van de Wizard Script](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Klik op **volgende**op de pagina **Samenvatting van de resultaten** .
![Wizard SQL-Database migreren - overzicht van de resultaten](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. Op de pagina **Setup Target Server-verbinding** op **verbinding maken met Server**en voer vervolgens de details:
    - **Naam van de server**: server doelinstantie
    - **Verificatie**: **SQL Server-verificatie**. Geef uw aanmeldingsgegevens.
    - **Database**: **Master DB (lijst met alle databases)**. Deze optie geeft een overzicht van alle databases op de doelserver.

    ![SQL Database Migration wizard - aansluiting op Target Server Setup](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Klik op **verbinding maken**, selecteert u de doeldatabase waarop u wilt dat de tabel die u wilt verplaatsen en klik op **volgende**. Dit moet voltooien de eerder gegenereerde script wordt uitgevoerd en u ziet nu de nieuwe verplaatste tabel gekopieerd naar de doeldatabase.

## <a name="verification-step"></a>Controlestap
1. Opvragen en test u de zojuist gekopieerde tabel om ervoor te zorgen dat de gegevens intact zijn. U kunt het formulier hernoemde tabel **voorbereidende stappen** sectie neerzetten na bevestiging. (bijvoorbeeld, &lt;naam van de tabel&gt;_old).

## <a name="next-steps"></a>Volgende stappen

[Automatische back-ups van SQL-Database](sql-database-automated-backups.md)
