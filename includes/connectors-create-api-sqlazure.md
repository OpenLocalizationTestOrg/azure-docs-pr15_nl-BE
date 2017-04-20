### <a name="prerequisites"></a>Vereisten
- Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken
- Een [Azure SQL-Database](../articles/sql-database/sql-database-get-started.md) met de verbindingsgegevens, inclusief de naam van de server, databasenaam en gebruikersnaam en wachtwoord. Deze informatie is opgenomen in de verbindingsreeks voor de SQL-Database:
  
    Server = tcp:*yoursqlservername*. database.windows.net,1433;Initial Catalog =*yourqldbname*; Beveiligingsinfo persistent = False; Gebruikers-ID = {your_username}; Wachtwoord = {your_password}; Voor MultipleActiveResultSets = False; Coderen = True; TrustServerCertificate = False; Time-out van verbinding = 30;

    Lees meer over [Azure SQL-Databases](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Wanneer u een Azure SQL-Database maakt, kunt u ook de voorbeelddatabases in SQL. 



Voordat u uw Azure SQL-Database in een app logica, verbinding maken met de SQL-Database. U kunt dit eenvoudig doen binnen uw app logica op de Azure portal.  

Verbinding maken met uw Azure SQL-Database met behulp van de volgende stappen uit:  

1. Maak een app logica. In de ontwerpfunctie voor Apps logica een trigger toevoegen en vervolgens een actie toevoegen. **Weergeven Microsoft beheerde API's** selecteren in de vervolgkeuzelijst en voer 'sql' in het zoekvak. Selecteer een van de acties:  

    ![Stap voor Azure SQL-verbinding maken](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Als u eerder geen verbindingen met SQL-Database hebt gemaakt, wordt u gevraagd de verbindingsdetails:  

    ![Stap voor Azure SQL-verbinding maken](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Voer de SQL-Database. Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
|---|---|
| Verbinding maken via de Gateway | Laat u deze uitgeschakeld. Dit wordt gebruikt wanneer u verbinding maakt met een SQL-Server op locatie. |
| Naam van de verbinding * | Voer een naam voor de verbinding. | 
| SQL Server-naam * | Voer de naam van de server; dat is ongeveer *servername.database.windows.net*. De naam van de server wordt weergegeven in de SQL-Database-eigenschappen in de portal Azure en ook weergegeven in de verbindingsreeks. | 
| De naam van de SQL-Database * | U hebt een SQL-Database invoeren. Dit wordt vermeld in de SQL-Database-eigenschappen in de verbindingsreeks: Initial Catalog =*yoursqldbname*. | 
| Gebruikersnaam * | Voer de gebruikersnaam die u hebt gemaakt toen u de SQL-Database is gemaakt. Dit wordt vermeld in de SQL-Database-eigenschappen in de portal Azure. | 
| Wachtwoord * | Voer het wachtwoord dat u hebt gemaakt toen u de SQL-Database is gemaakt. | 

    Deze referenties worden gebruikt om uw logica app verbinding maken en toegang tot uw SQL-gegevens toestaan. Wanneer voltooid, er de verbindingsdetails van uw de volgende strekking:  

    ![Stap voor Azure SQL-verbinding maken](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Selecteer **maken**. 

5. U ziet dat de verbinding is gemaakt. Nu gaat u verder met de overige stappen in uw app logica: 

    ![Stap voor Azure SQL-verbinding maken](./media/connectors-create-api-sqlazure/table.png)