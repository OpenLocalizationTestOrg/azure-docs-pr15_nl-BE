<properties
    pageTitle="Uitrekken geschikte databases herstellen | Microsoft Azure"
    description="Informatie over het herstellen van Stretch\-databases ingeschakeld."
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
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Uitrekken geschikte databases terugzetten

Een back-up van de database voor het herstellen van verschillende soorten fouten, fouten en rampen herstellen.

Voor meer informatie over back-up Zie [back-up uitrekken geschikte databases](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Back-up is slechts een onderdeel van een volledige hoge beschikbaarheid en continuïteit bedrijfsoplossing. Zie voor meer info over hoge beschikbaarheid [Oplossingen met hoge beschikbaarheid](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>De SQL Server-gegevens herstellen
Herstelt het uitrekken om te herstellen van een hardwarefout of een beschadiging,\-SQL Server-database vanuit een back-up wordt ingeschakeld. U kunt de methoden voor het terugzetten van SQL Server die u momenteel gebruikt blijven. Zie [terugzetten en herstellen-overzicht](https://msdn.microsoft.com/library/ms191253.aspx)voor meer informatie.

Nadat u de SQL Server-database herstelt, u hebt voor het uitvoeren van de opgeslagen procedure **sys.sp_rda_reauthorize_db** om te herstellen van de verbinding tussen het uitrekken\-SQL Server-database en de externe database Azure ingeschakeld. Zie [de verbinding tussen de SQL Server-database en de externe Azure-database terugzetten](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database)voor meer informatie.

## <a name="restore-your-remote-azure-data"></a>Uw externe Azure gegevens terugzetten

### <a name="recover-a-live-azure-database"></a>Een actieve Azure-database herstellen
De SQL Server-Database uitrekken service op Azure momentopnamen alle live-gegevens ten minste elke 8 uur met Azure opslag momentopnamen. Deze momentopnamen worden bijgehouden voor 7 dagen. Hiermee kunt u de gegevens terugzetten op een van ten minste 21 punten in de tijd in de afgelopen 7 dagen tot het tijdstip waarop de laatste momentopname is genomen.

Als u wilt een live Azure database terugzet op een eerder tijdstip met behulp van de portal Azure, het volgende doen.

1. Log in op de Azure portal.
2. Aan de linkerkant van het scherm selecteren op **Bladeren** en selecteer vervolgens de **SQL-Databases**.
3. Ga naar de database en selecteer deze.
4. Aan de bovenkant van het blad van de database, klikt u op **herstellen**.
5. Geef een nieuwe **naam van de Database**, selecteer een **Herstelpunt** en klik vervolgens op **maken**.
6. De herstelprocedure voor de database wordt gestart en kan worden gecontroleerd met behulp van **meldingen**.

### <a name="recover-a-deleted-azure-database"></a>Een verwijderde Azure-database herstellen
De service SQL Server-Database uitrekken op Azure neemt een momentopname van de database voordat een database wordt verbroken en deze 7 dagen behoudt. Hierna niet meer opnamen van de actieve database behouden. U kunt een verwijderde database terugzetten naar het punt wanneer deze is verwijderd.

Als u een verwijderde database Azure herstellen naar het punt wanneer deze via de portal Azure is verwijderd, het volgende doen.

1. Log in op de Azure portal.
2. Aan de linkerkant van het scherm selecteren op **Bladeren** en selecteer vervolgens de **SQL-Servers**.
3. Ga naar de server en selecteer deze.
4. Ga naar bewerkingen op van de server-blade, klikt u op de tegel **Databases verwijderd** .
5. Selecteer de verwijderde database die u wilt terugzetten.
5. Geef een nieuwe **naam** en klik op **maken**.
6. De herstelprocedure voor de database wordt gestart en kan worden gecontroleerd met behulp van **meldingen**.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>De verbinding tussen de SQL Server-database en de externe Azure-database terugzetten

1.  Als u verbinding maakt met een herstelde database Azure met een andere naam of in een andere regio, worden de opgeslagen procedure [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) verbreken vanuit de vorige Azure database uitgevoerd.  

2.  Voer de opgeslagen procedure [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) om de verbinding van de lokale uitrekken te\-de Azure database database ingeschakeld.  

    -   De bestaande database binnen het bereik van de referenties verstrekken als een sysname of een varchar\(128\) waarde. \(Gebruik geen varchar\(max\).\) U kunt de naam van de referentie in de weergave opzoeken **sys.database\_binnen het bereik van\_referenties**.  

    -   Geef op of maak een kopie van de gegevens op afstand en verbinding maken met het exemplaar (aanbevolen).  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Zie ook

[Beheren en oplossen van problemen met Database uitrekken](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Back-Up en terugzetten van SQL Server-Databases](https://msdn.microsoft.com/library/ms187048.aspx)
