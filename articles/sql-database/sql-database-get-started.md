<properties
    pageTitle="Zelfstudie voor SQL-Database: een SQL-database maken | Microsoft Azure"
    description="Informatie over het instellen van een logische server SQL-Database, de firewallregel server, SQL-database en voorbeeldgegevens. Ook informatie over het verbinden met clientprogramma's, gebruikers configureren en instellen van een firewallregel database."
    keywords="zelfstudie voor SQL-database, maakt u een sql-database"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>Zelfstudie voor SQL-Database: een SQL-database maken in minuten met behulp van de portal voor Azure

> [AZURE.SELECTOR]
- [Azure portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

In deze zelfstudie leert u hoe de Azure portal te gebruiken:

- Azure SQL-database maken met voorbeeldgegevens.
- Een niveau van de server firewallregel voor één IP-adres of een bereik van IP-adressen maken.

U kunt dezelfde taken uitvoeren met behulp van [C#](sql-database-get-started-csharp.md) of [PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Uw eerste Azure SQL-database maken 

1. Als u momenteel niet verbonden bent, moet u een verbinding maken met de [Azure portal](http://portal.azure.com).
2. Klik op **Nieuw**en klikt u op **gegevens- en**Zoek de **SQL-Database**.

    ![Nieuwe sql-database 1](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Klik op **SQL-Database** voor het openen van de SQL-Database-blade. De inhoud van deze blade is afhankelijk van het aantal abonnementen en uw bestaande objecten (zoals bestaande servers).

    ![Nieuwe sql-database 2](./media/sql-database-get-started/sql-database-new-database-2.png)

4. Geef een naam voor uw eerste database 'mijn database' in het tekstvak **naam van de Database** . Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.

    ![Nieuwe sql-database 3](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Selecteer een abonnement hebt u meerdere abonnementen.
6. Klik op **Nieuw** onder **resourcegroep**, en geef een naam op voor de eerste resourcegroep - zoals "mijn-resource-groep". Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.

    ![Nieuwe sql-database 4](./media/sql-database-get-started/sql-database-new-database-4.png)

7. Onder **bron selecteren**, klik op **voorbeeld** en klik vervolgens onder **voorbeeld selecteren** op **AdventureWorksLT [V12]**.

    ![Nieuwe sql-database 5](./media/sql-database-get-started/sql-database-new-database-5.png)

8. **Server**, klik op **configureren, instellingen vereist**.

    ![Nieuwe sql-database 6](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Klik op de blade Server **maken een nieuwe server**. Azure SQL-database in een server-object een nieuwe server of een server kan worden gemaakt.

    ![Nieuwe sql-database 7](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Bekijk de **nieuwe server** -blade om te begrijpen van de informatie die u nodig hebt voor uw nieuwe server.

    ![Nieuwe sql-database 8](./media/sql-database-get-started/sql-database-new-database-8.png)

11. Geef een naam voor de eerste server - zoals 'Mijn-nieuwe-server-object' in het tekstvak **naam van de Server** . Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.

    ![Nieuwe sql-database 9](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. Onder **Server admin aanmelden**, voorzien in een gebruikersnaam voor aanmelding bij de beheerder van deze server - zoals 'Mijn-Administrator-account'. Deze aanmelding is bekend als de belangrijkste server-aanmelding. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.

    ![Nieuwe sql-database 10](./media/sql-database-get-started/sql-database-new-database-10.png)

13. Bij **wachtwoord** en **Bevestig het wachtwoord**een wachtwoord opgeven voor de server principal aanmeldingsaccount - zoals "p@ssw0rd1". Een groen vinkje geeft aan dat u een geldig wachtwoord hebt opgegeven.

    ![Nieuwe sql-database 11](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. Selecteer onder **locatie**, afhankelijk van uw locatie - zoals "Oost-Australië" een datacenter.

    ![Nieuwe sql-database 12](./media/sql-database-get-started/sql-database-new-database-12.png)

15. Onder ** V12 maken server (laatste update), ziet u alleen de optie voor het maken van een actuele versie van Azure SQL server hebt.

    ![Nieuwe sql-database 13](./media/sql-database-get-started/sql-database-new-database-13.png)

16. U ziet dat het selectievakje voor het **toestaan van azure services voor toegang tot server** standaard is ingeschakeld en kan hier niet worden gewijzigd. Dit is een geavanceerde optie. U kunt deze instelling in de firewall-instellingen voor dit serverobject wijzigen, hoewel voor de meeste scenario's dit niet nodig is.

    ![Nieuwe sql-database 14](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Controleer uw selecties op de nieuwe server-blade en klik vervolgens op **selecteren** om deze nieuwe server voor de nieuwe database te selecteren.

    ![Nieuwe sql-database 15](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Op het blad SQL-Database onder de **prijzen laag**, **S2 norm** op en klik op **standaard** om de goedkoopste prijzen laag voor uw eerste database. U kunt de prijzen laag later altijd wijzigen.

    ![Nieuwe sql-database 16](./media/sql-database-get-started/sql-database-new-database-16.png)

19. Controleer uw selecties en klik op **maken** om uw eerste server en database maken op de SQL-Database-blade. De waarden die u hebt opgegeven, worden gevalideerd en implementatie begint.

    ![Nieuwe sql-database 17](./media/sql-database-get-started/sql-database-new-database-17.png)

20. Op de portal-werkbalk, klikt u op de items **meldingen** u de status van uw implementatie.

    ![Nieuwe sql-database 18](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Wanneer de implementatie is voltooid, worden de nieuwe Azure SQL server en de database gemaakt in Azure. Niet mogelijk verbinding maken met uw nieuwe server of database met behulp van hulpprogramma's voor SQL Server totdat u een server firewall regels maken om de SQL-Database verbindingen van buiten Azure firewall openen.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt voltooid deze zelfstudie SQL-Database en een database gemaakt met voorbeeldgegevens, bent u klaar om te verkennen met behulp van uw favoriete programma's.

- Als u vertrouwd bent met Transact-SQL- en SQL Server Management Studio (SSMS), informatie over het [verbinden en de query een SQL-database met SSMS](sql-database-connect-query-ssms.md).

- Als u Excel, meer informatie over hoe u kunt [verbinding maken met een SQL-database in Azure met Excel](sql-database-connect-excel.md).

- Bent u klaar om te gaan programmeren, kies uw programmeertaal op [bibliotheken verbinding voor SQL-Database en SQL Server](sql-database-libraries.md).

- Als u wilt dat SQL Server-databases op gebouwen verplaatsen naar Azure, Zie [een database naar een SQL-Database migreren](sql-database-cloud-migrate.md) voor meer informatie.

- Als u wilt dat bepaalde gegevens in een nieuwe tabel in een CSV-bestand laden met behulp van het opdrachtregelprogramma BCP, Zie [het laden van gegevens in een SQL-Database vanuit een CSV-bestand met behulp van BCP](sql-database-load-from-csv-with-bcp.md).

- Als u verkennen Azure SQL-Database beveiliging wilt, Zie [aan de slag met beveiliging](sql-database-get-started-security.md)


## <a name="additional-resources"></a>Aanvullende bronnen

[Wat is een SQL-Database?](sql-database-technical-overview.md)
