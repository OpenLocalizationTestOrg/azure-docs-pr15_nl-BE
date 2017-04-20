<properties
    pageTitle="Aan de slag met SQL-Databases gegevens synchroniseren"
    description="Deze zelfstudie kunt u aan de slag met de Azure SQL Data synchronisatie (voorbeeld)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Aan de slag met Azure SQL gegevens synchroniseren (voorbeeld)
In deze zelfstudie leert u de grondbeginselen van Azure SQL gegevens synchroniseren met de klassieke Azure-Portal.

In deze zelfstudie wordt ervan uitgegaan dat de minimale ervaring met SQL Server en Azure SQL-Database. In deze zelfstudie maakt u een hybride (SQL Server en SQL Database-exemplaren) sync groep volledig geconfigureerd en op de door u gepland tijdstip te synchroniseren.

> [AZURE.NOTE] De volledige technische documentatie voor Azure SQL-gegevens synchroniseren, voorheen ondergebracht op MSDN, is beschikbaar als een PDF-bestand. Download deze [hier](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Stap 1: Maak verbinding met de Azure SQL-Database

1. Aanmelden bij de [klassieke Portal](http://manage.windowsazure.com).

2. Klik op **SQL-DATABASES** in het linkerdeelvenster.

3. Klik op **SYNC** onderaan de pagina. Wanneer u op SYNCHRONISEREN klikt, verschijnt een lijst met dingen die u toevoegen kunt - **Nieuwe Sync-groep** en **Nieuwe Sync-Agent**.

4. De nieuwe SQL-gegevens synchroniseren Agent om wizard te starten, klikt u op **Nieuwe synchronisatie-Agent**.

5. Als u een agent voordat **Klik hier downloaden**hebt toegevoegd.

    ![Image1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Stap 2: De Client-Agent toevoegen
Deze stap is alleen vereist als u gaat zijn op ruimten SQL Server-databases opgenomen in uw groep sync. Ga verder met stap 4 als uw groep synchronisatie alleen exemplaren van SQL-Database heeft.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Stap 2a: de vereiste software installeren
Zorg ervoor dat de volgende software zijn geïnstalleerd op de computer waarop u de Client-Agent installeren.

- **.NET framework 4.0**

 .NET Framework 4.0 installeren vanaf [hier](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 SP1 systeem CLR-typen (x86)**

 Installeer de Microsoft SQL Server 2008 R2 SP1 systeem CLR-typen (x86) vanaf [hier](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Microsoft SQL Server 2008 R2 SP1 gedeelde objecten voor (x86)**

 Installeer de Microsoft SQL Server 2008 R2 SP1 gedeelde objecten (x86) van [hier](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Stap 2b: een nieuwe Client Agent installeren

Volg de instructies in het [installeren van een Client Agent (SQL Data synchronisatie)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) de agent installeren.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Stap 2c: de nieuwe SQL Data synchronisatie wizard voltooien

1.  Terug naar de wizard nieuwe SQL Data synchronisatie-Agent.
2.  De agent een betekenisvolle naam geven.
3.  In de vervolgkeuzelijst, selecteer het **gebied** (Datacenter) als host voor deze agent.
4.  In de vervolgkeuzelijst, selecteer het **abonnement** voor het hosten van deze agent.
5.  Klik op de pijl-rechts.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Stap 3: Een SQL Server-database met de Agent van de Client registreren

Nadat de Client-Agent is geïnstalleerd, elke SQL Server-database op de ruimten die u wilt opnemen in een groep voor synchronisatie met de agent worden geregistreerd.
Een database met de agent Volg de instructies op de [SQL Server-Database met een Client-Agent te registreren](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Stap 4: Maak een groep sync


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Stap 4a: Start de wizard nieuwe groep voor synchronisatie

1.  Terug naar de [klassieke Portal](http://manage.windowsazure.com).
2.  Klik op **SQL-DATABASES**.
3.  Klik op **SYNC toevoegen** onder aan de pagina en selecteer vervolgens nieuwe Sync-groep in de lade.

    ![Image2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Stap 4b: Voer de basisinstellingen


1.  Voer een duidelijke naam voor de groep van de synchronisatie.
2.  In de vervolgkeuzelijst, selecteer het **gebied** (Data Center) als host voor deze groep sync.
3. Klik op de pijl-rechts.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Stap 4c: de hub synchronisatie definiëren

1. In de vervolgkeuzelijst, selecteert u de SQL-Database-instantie moet fungeren als de hub sync-groep.
2. Voer de referenties voor dit exemplaar van de SQL-Database - **HUB gebruikersnaam** en het **Wachtwoord van de HUB**.
3. Wachten op SQL gegevens synchroniseren met de gebruikersnaam en het wachtwoord te bevestigen. Er verschijnt een groen vinkje weergegeven aan de rechterzijde van het wachtwoord wanneer de referenties worden bevestigd.
4. In de vervolgkeuzelijst, selecteer het beleid voor **CONFLICTOPLOSSING** .

 **Hub Wins** - elke wijziging in de hub database schrijven met de referentie-databases is geschreven, verwijst naar overschrijven wijzigingen in dezelfde databaserecord. Functioneel, betekent dit dat de eerste wijziging geschreven op de hub wordt doorgegeven naar de andere databases.


 **Wins Client** - wijzigingen weggeschreven naar de hub worden overschreven door wijzigingen in de verwijzing naar databases. Functioneel, betekent dit dat de laatste wijziging geschreven op de hub een bewaard en doorgegeven aan de andere databases.

5.  Klik op de pijl-rechts.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Stap 4d: een verwijzing naar database toevoegen


Herhaal deze stap voor elke extra database die u wilt toevoegen aan de sync-groep.

1. In de vervolgkeuzelijst, selecteer de database toe te voegen.

    Databases in de vervolgkeuzelijst bevatten zowel SQL Server-databases die zijn geregistreerd met de agent en de exemplaren van de SQL-Database.
2.  Referenties voor deze database - **gebruikersnaam** en **wachtwoord**invoeren.
3.  Selecteer in de vervolgkeuzelijst, de **Richting van synchronisatie** voor deze database.

    **Bi-directionele** - wijzigingen in de database verwijst zijn opgeslagen in de database van de hub en wijzigingen in de database van de hub worden geschreven naar de database verwijst.

    **Synchronisatie van de Hub** - de database ontvangt updates van de Hub. Het verzendt geen wijzigingen op de Hub.

    **Synchroniseren met de Hub** - de database verzendt updates naar de Hub. Wijzigingen in de Hub worden niet naar deze database geschreven.

4.  Klik op het vinkje in de rechterbenedenhoek van de wizard om de sync-groep maakt. Wacht totdat de synchronisatie SQL gegevens te bevestigen de referenties. Een groen vinkje geeft aan dat de referenties zijn bevestigd.

5.  Klik nogmaals op het selectievakje is ingeschakeld. Hiermee keert u terug naar de pagina **synchronisatie** onder SQL-Databases. Deze groep sync wordt nu weergegeven met de andere groepen voor synchronisatie en agenten.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Stap 5: Definieer de gegevens te synchroniseren

Azure SQL-gegevens synchroniseren, kunt u tabellen en kolommen te synchroniseren. Als u ook een kolom filteren, zodat die alleen met specifieke waarden rijen (zoals leeftijd > = 65) synchroniseren, gebruik van de SQL-gegevens Sync-portal op Azure en selecteert u de documentatie bij de tabellen, kolommen en rijen te synchroniseren voor het definiëren van de gegevens te synchroniseren.

1.  Terug naar de [klassieke Portal](http://manage.windowsazure.com).
2.  Klik op **SQL-DATABASES**.
3.  Klik op het tabblad **SYNCHRONISEREN** .
4.  Klik op de naam van deze groep sync.
5.  Klik op het tabblad **Regels voor synchronisatie** .
6.  Selecteer de database die u wilt de groep synchronisatieschema te bieden.
7.  Klik op de pijl-rechts.
8.  Klik op **SCHEMA te vernieuwen**.
9.  Selecteer de kolommen die u wilt opnemen in de synchronisatiebewerkingen voor elke tabel in de database.
    - Kolommen met niet-ondersteunde gegevenstypen kunnen niet worden geselecteerd.
    - Als er geen kolommen in een tabel worden geselecteerd, wordt de tabel niet opgenomen in de groep sync.
    - De tabellen te selecteren/selectie opheffen klikt u op selecteren onder aan het scherm.
10. Klik op **Opslaan**en wacht tot de groep synchronisatie voltooid wordt ingericht.
11. Om terug te keren naar de landingspagina gegevens synchroniseren, klikt u op de pijl Vorige in de linkerbovenhoek van het scherm (boven de naam van de groep sync).

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Stap 6: Configureer uw sync-groep

Door te klikken op SYNC onderaan in de pagina gegevens synchroniseren, kunt u altijd een groep sync synchroniseren.
Als u wilt synchroniseren, moet u de sync-groep configureren.

1.  Terug naar de [klassieke Portal](http://manage.windowsazure.com).
2.  Klik op **SQL-DATABASES**.
3.  Klik op het tabblad **SYNCHRONISEREN** .
4.  Klik op de naam van deze groep sync.
5.  Klik op het tabblad **configureren** .
6.  **AUTOMATISCHE SYNCHRONISATIE**
    - U configureert de sync-groep te synchroniseren op een frequentie instellen door op **ON**. U kunt nog steeds op verzoek synchroniseren door te klikken op SYNCHRONISEREN.
    - Klik op **uitgeschakeld** om de groep sync synchroniseert alleen wanneer u de synchronisatie op configureren.
7.  **SYNC-FREQUENTIE**
    - Als automatische synchronisatie ingeschakeld is, stelt u de synchronisatiefrequentie. De frequentie moet tussen 5 minuten en 1 maand.
8.  Klik op **Opslaan**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Van harte gefeliciteerd. U kunt een groep sync met zowel een SQL-Database-instantie en een SQL Server-database hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over de SQL-Database en SQL gegevens synchroniseren Zie:

* [De volledige SQL-gegevens synchroniseren technische documentatie downloaden](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [SQL-Database-overzicht](sql-database-technical-overview.md)
* [Levenscyclusbeheer voor uw database](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
