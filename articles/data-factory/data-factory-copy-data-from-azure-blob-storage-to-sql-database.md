<properties
    pageTitle="Gegevens kopiëren uit de Blob-opslag met SQL-Database | Microsoft Azure"
    description="In deze zelfstudie wordt beschreven hoe u kopie activiteit in een pijpleiding Azure Data Factory gebruiken om gegevens te kopiëren van de Blob-opslag met SQL-database."
    Keywords="BLOB sql-blob-opslag gegevens kopiëren"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Gegevens kopiëren uit de Blob-opslag met SQL-Database met behulp van Data Factory 
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Sjabloon voor Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In deze zelfstudie maakt u een gegevens-fabriek met een pijpleiding om gegevens te kopiëren van de Blob-opslag met SQL-database.

De activiteit kopie uitvoert verplaatsing van gegevens in Azure Data Factory. Het wordt aangedreven door een wereldwijd beschikbare service waarmee gegevens tussen verschillende opgeslagen gegevens op een veilige, schaalbare en betrouwbare manier kunt kopiëren. Zie [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) -artikel voor meer informatie over de activiteit van de kopie.  

> [AZURE.NOTE] Zie het artikel [Inleiding tot Azure Data Factory](data-factory-introduction.md) voor een gedetailleerd overzicht van de Data Factory service.

##<a name="prerequisites-for-the-tutorial"></a>Vereisten voor de zelfstudie
Voordat u deze zelfstudie hebt u het volgende:

- **Azure-abonnement**.  Als u niet een abonnement hebt, kunt u een gratis proefperiode account in een paar minuten. Zie het artikel [Gratis proefversie](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
- **Azure opslag Account**. Gebruikt u de blobopslag als gegevensarchief **bron** in deze zelfstudie. Als u geen account Azure opslag, Zie het artikel [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) voor stapsgewijze instructies voor het maken van een.
- **Azure SQL-Database**. U gebruikt een Azure SQL-database als gegevensarchief **bestemming** in deze zelfstudie. Als u geen Azure SQL-database die u in de zelfstudie gebruiken kunt, Zie [het maken en configureren van een Azure SQL-Database](../sql-database/sql-database-get-started.md) maken.
- **SQL Server 2012/2014 of Visual Studio 2013**. Met SQL Server Management Studio of Visual Studio kunt u een voorbeelddatabase te maken en de resulterende gegevens in de database weergeven.  

## <a name="collect-blob-storage-account-name-and-key"></a>Verzamelen van blob storage accountnaam en sleutel 
U moet de naam en de sleutel van de account van uw account Azure opslag in deze zelfstudie doen. Opmerking **de naam** en **account-sleutel** voor uw account Azure opslag.

1. Log in op de [Azure portal](https://portal.azure.com/).
2. Klik op **meer services** in het menu links en selecteer **Accounts voor opslag**.

    ![Bladeren - opslag rekeningen](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. Selecteer in het blad **Opslag rekeningen** **Azure opslag account** die u wilt gebruiken in deze zelfstudie.
4. Selecteer de koppeling **toegangstoetsen** onder **Instellingen**.
5.  Klik op de knop **copy** (afbeelding) naast het tekstvak **accountnaam opslag** en opslaan en ergens plakken (bijvoorbeeld: in een tekstbestand).
6. Herhaal de vorige stap om te kopiëren of noteer de **key1**.
    
    ![Toegangstoets opslag](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Sluit alle bladen door te klikken op **X**.

## <a name="collect-sql-server-database-user-names"></a>SQL server, database, namen van gebruikers verzamelen
Moet u de namen van Azure SQL server-database en gebruiker deze zelfstudie. Let op de namen van de **server**en **database** **gebruiker** voor uw Azure SQL-database.

1. In **Azure portal**, klikt u op **meer services** aan de linkerkant en selecteer **SQL-databases**.
2. Selecteer de **database** die u wilt gebruiken in deze zelfstudie in de **SQL-databases blade**. Let op de **naam van de database**.  
3. Klik op **Eigenschappen** in het blad **SQL-database** onder **Instellingen**.
4. Opmerking omlaag de waarden voor de **Naam van de SERVER** en **SERVER ADMIN LOGIN**.
5. Sluit alle bladen door te klikken op **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Azure services voor toegang tot SQL server toestaan 
Zorg ervoor **dat **toegang tot Azure services** ingeschakeld voor uw Azure SQL-server zodat de Data Factory-service toegang heeft tot uw Azure SQL server** . Als u wilt controleren en deze instelling inschakelt, moet u de volgende stappen uitvoeren:

1. **Meer services** hub aan de linkerkant en klik op **SQL-servers**.
2. Selecteer uw server en klik op **Firewall** onder **Instellingen**. 
4. Klik in de blade **Firewall-instellingen** **op** voor **toegang tot de Azure services toestaan**.
5. Sluit alle bladen door te klikken op **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Blob-opslag en SQL-Database voorbereiden 
Nu uw Azure blob-opslag en voorbereiden Azure SQL-database voor de zelfstudie door de volgende stappen uit te voeren:  

1. Start Kladblok en plak de volgende tekst als **emp.txt** naar de map **C:\ADFGetStarted** op de vaste schijf opslaan.

        John, Doe
        Jane, Doe

2. Gebruik hulpprogramma's zoals [Azure Opslagverkenner](https://azurestorageexplorer.codeplex.com/) kunt u de container **adftutorial** maken en uploaden van het bestand **emp.txt** naar de container.

    ![Azure Opslagverkenner. Gegevens kopiëren uit de Blob-opslag met SQL-database](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Gebruik de volgende SQL-script voor het maken van de **emp** -tabel in uw Azure SQL-Database.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Als u SQL Server 2012/2014 op uw computer geïnstalleerd hebben:** Volg de instructies van [stap 2: verbinding maken met een SQL-Database van het beheren van Azure SQL-Database met behulp van SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md#Step2) artikel verbinding maken met uw Azure SQL server en de SQL-script wordt uitgevoerd. In dit artikel wordt de [klassieke Azure portal](http://manage.windowsazure.com), niet de [nieuwe portal van Azure](https://portal.azure.com)firewall voor een Azure SQL server configureren.

    Als de client is niet toegestaan de Azure SQL Server, moet u de firewall voor uw Azure SQL-server voor toegang van uw computer (IP-adres) te configureren. Zie [in dit artikel](../sql-database/sql-database-configure-firewall-settings.md) voor informatie over het configureren van de firewall voor uw Azure SQL server.

U kunt de vereisten hebt voltooid. U kunt een data factory met behulp van een van de volgende manieren maken. Klik op een van de tabs aan de bovenkant of de volgende koppelingen voor het uitvoeren van de zelfstudie.     

- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
