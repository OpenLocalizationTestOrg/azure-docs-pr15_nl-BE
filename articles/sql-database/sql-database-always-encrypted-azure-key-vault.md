<properties
    pageTitle="Altijd gecodeerd: Beschermen van vertrouwelijke gegevens in Azure SQL-Database met de codering van de database | Microsoft Azure"
    description="Beschermen van vertrouwelijke gegevens in uw SQL-database in minuten."
    keywords="gegevenscodering, coderingssleutel, cloud-codering"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Altijd gecodeerd: Bescherming van gevoelige gegevens in een SQL-Database en opslaan van uw coderingssleutels in Azure sleutel kluis

> [AZURE.SELECTOR]
- [Azure sleutel kluis](sql-database-always-encrypted-azure-key-vault.md)
- [Windows-certificaatarchief](sql-database-always-encrypted.md)


In dit artikel wordt beschreven hoe u vertrouwelijke gegevens in een SQL-database te beveiligen met codering van gegevens met de [Wizard altijd gecodeerd](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). De handleiding bevat ook instructies die aangeven hoe u elke sleutel opslaat in Azure sleutel kluis.

Altijd is gecodeerd een nieuwe technologie voor gegevens codering in Azure SQL-Database en SQL Server die helpt bij het beschermen van vertrouwelijke gegevens in rust op de server tijdens het verkeer tussen client en server, en wanneer de gegevens wordt gebruikt. Altijd gecodeerd zorgt ervoor dat gevoelige gegevens nooit als leesbare tekst in het databasesysteem weergegeven. Na het configureren van de codering van gegevens, alleen client-toepassingen of servers app die toegang tot de sleutels hebben toegang tot gegevens als leesbare tekst. Zie voor gedetailleerde informatie [Altijd gecodeerd (Database-Engine)](https://msdn.microsoft.com/library/mt163865.aspx).


Nadat u de database voor het gebruik altijd gecodeerd configureert, maakt u een clienttoepassing in C# met Visual Studio om te werken met de gecodeerde gegevens.

Volg de stappen in dit artikel en informatie over het instellen van altijd gecodeerd voor Azure SQL-database. In dit artikel leert u hoe u de volgende taken uitvoeren:

- Gebruik de wizard altijd gecodeerd in SSMS [altijd gecodeerde sleutels](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)maken.
    - Maak een [kolom hoofdsleutel (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Maak een [kolom coderingssleutel (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Een databasetabel maken en kolommen te coderen.
- Een toepassing maken die wordt ingevoegd en worden gegevens weergegeven uit de gecodeerde kolommen selecteert.


## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u nodig:

- Azure-account en abonnement. Als u nog geen hebt, meld u aan voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) versie 13.0.700.242 of hoger.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) of hoger (op de clientcomputer).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
- [Azure PowerShell](../powershell-install-configure.md), versie 1.0 of hoger. Type **(Get Module azure - ListAvailable). Versie** om te zien welke versie van PowerShell uitgevoerd.



## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>De clienttoepassing toegang tot de service SQL-Database inschakelen

U kunt uw clienttoepassing toegang krijgen tot de service SQL-Database door de vereiste verificatie instellen en het verwerven van de *ClientId* en u moet voor de verificatie van uw toepassing in de volgende code *geheim* moet inschakelen.

1. Open de [Azure klassieke portal](http://manage.windowsazure.com).
2. **Active Directory** en klik op de Active Directory-instantie die uw toepassing wordt gebruikt.
3. Klik op **toepassingen**en klik vervolgens op **toevoegen**.
4. Typ een naam voor de toepassing (bijvoorbeeld: *myClientApp*), **WEBTOEPASSING**selecteren en klik op de pijl om door te gaan.
5. U kunt voor de **SIGN-ON URL** en de **URI van APP-ID** typt u een geldige URL (bijvoorbeeld *http://myClientApp*) en doorgaan.
6. Klik op **configureren**.
7. Kopieer de **CLIENT-ID**. (U moet deze waarde in uw code later.)
8. Selecteer in de sectie **sleutels** **1 jaar** uit de vervolgkeuzelijst **Selecteer duur** . (Kopieert u de sleutel nadat u hebt opgeslagen in stap 14.)
11. Blader omlaag en klik op **toepassing toevoegen**.
12. Laat **Microsoft Apps** set **weergeven** en selecteer **Microsoft Azure Service Management**. Klik op het vinkje om door te gaan.
13. **Toegangsbeheer Azure Service** selecteren in de lijst **Machtigingen overgedragen** .
14. Klik op **Opslaan**.
15. Na het opslaan is voltooid, kopieert u de waarde van de sleutel in de sectie **sleutels** . (U moet deze waarde in uw code later.)



## <a name="create-a-key-vault-to-store-your-keys"></a>Een sleutel kluis voor het opslaan van uw sleutels maken

Nu uw app client is geconfigureerd en u de client-ID hebt, is het tijd voor het maken van een sleutel kluis en het toegangsbeleid configureren zodat u en uw toepassing toegang heeft tot de kluis van geheimen (de toetsen altijd gecodeerd). Machtigingen voor het *maken*, *ophalen*, *lijst*, *ondertekenen*, *verifiëren*, *wrapKey*en *unwrapKey* zijn vereist voor het maken van een nieuwe kolom hoofdsleutel en voor het instellen van de codering met SQL Server Management Studio.

U kunt snel een sleutel kluis maken door het volgende script uit te voeren. Zie voor een gedetailleerde uitleg van deze cmdlets en meer informatie over het maken en configureren van een sleutel kluis [aan de slag met Azure sleutel kluis](../Key-Vault/key-vault-get-started.md).



    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).SubscriptionId
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup –Name $resourceGroupName –Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Maak een lege database voor SQL
1. Log in om de [Azure portal](https://portal.azure.com/).
2. Ga naar de **nieuwe** > **Data- +** > **SQL-Database**.
3. Een **lege** database maken met de naam **kliniek** op een nieuwe of bestaande. Zie voor gedetailleerde instructies over het maken van een database in de Azure portal [maken een SQL-database, in minuten](sql-database-get-started.md).

    ![Een lege database maken](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

U moet de verbinding later in de zelfstudie tekenreeks, dus nadat u de database hebt gemaakt, bladert u naar de nieuwe kliniek database en kopieer de verbindingsreeks. U kunt de verbindingsreeks ophalen op elk gewenst moment, maar het is gemakkelijk om deze te kopiëren in de portal Azure.

1. Ga naar de **SQL-databases** > **kliniek** > **tekenreeksen voor een databaseverbinding wordt weergegeven**.
2. Kopieer de verbindingsreeks voor **ADO.NET**.

    ![De verbindingsreeks kopiëren](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Verbinding maken met de database met SSMS

SSMS openen en verbinding maken met de server met de database van de kliniek.


1. SSMS openen. (Ga naar **Connect** > **-Database-Engine** voor het openen van het venster **verbinding maken met de Server** als niet geopend is.)
2. Voer uw naam en referenties. De naam van de server kunt u vinden op de SQL-database blade en in de verbindingsreeks u hebt gekopieerd eerder. Typ de volledige servernaam, met inbegrip van *database.windows.net*.

    ![De verbindingsreeks kopiëren](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Hiermee opent u het venster **Nieuwe firewallregel** , Azure aanmelden als SSMS een nieuwe firewallregel maken voor u laten.


## <a name="create-a-table"></a>Een tabel maken

In dit gedeelte maakt u een tabel Patiënten gegevens kan bevatten. Het is niet in eerste instantie gecodeerd--configureert u codering in de volgende sectie.

1. **Databases**uitbreiden.
1. Klik met de rechtermuisknop op de **kliniek** -database en klik op **Nieuw**.
2. Plak de volgende Transact-SQL (T-SQL) in de nieuwe query-venster en **uitvoeren** het.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Coderen van kolommen (altijd gecodeerd configureren)

SSMS biedt een wizard waarmee u eenvoudig configureren altijd gecodeerd door het instellen van de hoofdsleutel van de kolom, kolom coderingssleutel en gecodeerde kolommen voor u.

1. **Databases**uitbreiden > **kliniek** > **tabellen**.
2. Klik met de rechtermuisknop op de tabel **patiënten** en selecteer **Kolommen coderen** om de wizard altijd gecodeerd te openen:

    ![Coderen van kolommen](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

De wizard altijd gecodeerd bevat de volgende secties: **Kolom selecteren**, **Configuratie van de hoofdsleutel**, **validatie**en **Overzicht**.

### <a name="column-selection"></a>Kolom selecteren##

Klik op **volgende** op **de introductiepagina te openen op de pagina **Kolom** ** . Op deze pagina, selecteert u de kolommen die u coderen wilt, [het type codering en de coderingssleutel voor welke kolom (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) te gebruiken.

**Sofi-nummer** en de **Geboortedatum** van informatie voor elke patiënt worden gecodeerd. Deterministic codering, dat ondersteuning biedt voor gelijkheid lookups, joins en groeperen op de kolom sofi-nummer gebruikt. De kolom Geboortedatum gebruikt randomiseren codering die wordt niet ondersteund.

Het **Type codering** voor de kolom sofi-nummer instellen **Deterministic** en de kolom Geboortedatum **Randomized**. Klik op **volgende**.

![Coderen van kolommen](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Configuratie van hoofdsleutel###

De pagina **Configuratie van hoofdsleutel** is waar u uw CMK instellen en selecteer de provider sleutelarchief waar het CMK worden opgeslagen. U kunt op dit moment een CMK opslaan in het Windows-certificaatarchief, Azure sleutel kluis of een hardwarebeveiligingsmodule (HSM).

Deze zelfstudie laat zien hoe om uw sleutels in Azure sleutel kluis.

1.     Selecteer **Azure sleutel kluis**.
1.     Selecteer de gewenste sleutel kluis in de vervolgkeuzelijst.
1.     Klik op **volgende**.

![Configuratie van hoofdsleutel](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)


### <a name="validation"></a>Validatie###

U kunt de kolommen nu coderen of sla een PowerShell script later uit te voeren. Voor deze zelfstudie **Doorgaan om nu te voltooien** en klik op **volgende**.

### <a name="summary"></a>Samenvatting ###

Controleer of de instellingen voor alle corrigeren en klik op **Voltooien** om de instellingen voor altijd gecodeerd.


![Samenvatting](./media/sql-database-always-encrypted-azure-key-vault/summary.png)


### <a name="verify-the-wizards-actions"></a>Controleer of de acties van de wizard

Nadat de wizard voltooid is, wordt de database instellen voor altijd gecodeerd. De wizard heeft de volgende acties uitgevoerd:

- Een kolom hoofdsleutel gemaakt en opgeslagen in Azure sleutel kluis.
- Een kolom versleutelingssleutel gemaakt en opgeslagen in Azure sleutel kluis.
- De geselecteerde kolommen voor codering geconfigureerd. De tabel patiënten op dit moment geen gegevens bevat, maar worden bestaande gegevens in de geselecteerde kolommen worden nu gecodeerd.

U kunt controleren of het maken van de sleutels in SSMS uitbreiding **kliniek** > **Security** > **Altijd gecodeerde sleutels**.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Een clienttoepassing die met de gecodeerde gegevens werkt maken

Nu dat altijd gecodeerd is ingesteld, kunt u een toepassing die op de gecodeerde kolommen *Invoegen* en *selecteert u* maken.  

> [AZURE.IMPORTANT] Uw toepassing moet gebruiken [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objecten leesbare gegevens worden doorgegeven aan de server met kolommen altijd gecodeerd. Letterlijke waarden doorgeven zonder SqlParameter objecten resulteert in een uitzondering.

1. Visual Studio openen en een nieuwe C#-consoletoepassing maken. Zorg ervoor dat uw project is ingesteld op **.NET Framework 4.6** of hoger.
2. Geef het project de **AlwaysEncryptedConsoleAKVApp** en klik op **OK**.
![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/console-app.png)
3. De volgende NuGet-pakketten te installeren gaat u naar **Extra** > **NuGet Package Manager** > **Package Manager-Console**.

Deze twee regels met code worden uitgevoerd in de Package Manager-Console.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>De verbindingsreeks waarmee altijd gecodeerd wijzigen

In dit gedeelte wordt uitgelegd hoe inschakelen altijd gecodeerd in de verbindingsreeks.


Zodat altijd gecodeerd, moet u de **Coderingsinstelling kolom** trefwoord toevoegen aan uw verbindingsreeks en stel deze in op **ingeschakeld**.

U kunt deze rechtstreeks in de verbindingsreeks instellen of u kunt deze eigenschap instellen met behulp van [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). De voorbeeldtoepassing in het volgende gedeelte ziet u hoe u **SqlConnectionStringBuilder**gebruikt.



### <a name="enable-always-encrypted-in-the-connection-string"></a>Schakel altijd gecodeerd in de connection string

Het volgende trefwoord toevoegen aan uw verbindingsreeks.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Altijd gecodeerd met SqlConnectionStringBuilder inschakelen

De volgende code laat zien hoe inschakelen altijd gecodeerd door [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) in te stellen op [ingeschakeld](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>Registreren van de provider Azure sleutel kluis

De volgende code ziet u hoe de provider Azure sleutel kluis registreren bij de ADO.NET-stuurprogramma.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Altijd gecodeerd console voorbeeldtoepassing

In dit voorbeeld wordt gedemonstreerd hoe u:

- Wijzig de verbindingsreeks waarmee altijd gecodeerd.
- Azure sleutel kluis registreren als de belangrijkste archiefvoorziening van de toepassing.  
- Gegevens invoegen in de kolommen gecodeerd.
- Selecteer een record door te filteren op een specifieke waarde in een kolom gecodeerd.

De inhoud van de **Program.cs** vervangen door de volgende code. Vervang de verbindingsreeks voor de globale connectionString-variabele in de regel die direct voorafgaat aan de Main-methode met de geldige verbindingsreeks van de Azure portal. Dit is de enige wijziging moet u deze code.

De app om te zien altijd gecodeerd in actie worden uitgevoerd.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }



## <a name="verify-that-the-data-is-encrypted"></a>Controleer of dat de gegevens worden gecodeerd

U kunt snel controleren dat de gegevens op de server worden gecodeerd door de patiënten gegevens opvragen met SSMS (met behulp van uw huidige verbinding waar nog geen **Kolom coderingsinstelling** is ingeschakeld).

De volgende query uitvoeren op de database van de kliniek.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

U kunt zien dat de gecodeerde kolommen gegevens als leesbare tekst bevatten.

   ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)


Voor het gebruik van SSMS toegang tot de gegevens als leesbare tekst, kunt u toevoegen de *kolom coderingsinstelling = ingeschakeld* -parameter voor de verbinding.

1. In SSMS, uw server in **Object Explorer** met de rechtermuisknop en kies **verbinding verbreken**.
2. Klik op **verbinding maken met** > **-Database-Engine** , open het venster **verbinding maken met Server** en klikt u op **Opties**.
3. Klik op **Extra verbindingsparameters** en typ **kolom coderingsinstelling = ingeschakeld**.

    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)

4. De volgende query uitvoeren op de database van de kliniek.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Nu kunt u de gegevens in de kolommen gecodeerde leesbare tekst zien.


    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Volgende stappen
Nadat u een database die altijd gecodeerd maakt worden, kunt u het volgende doen:

- [Draaien en het opschonen van uw sleutels](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migreren gegevens die al is gecodeerd met altijd gecodeerd](https://msdn.microsoft.com/library/mt621539.aspx).


## <a name="related-information"></a>Verwante informatie

- [Altijd gecodeerd (ontwikkeling van de client)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Transparante gegevenscodering](https://msdn.microsoft.com/library/bb934049.aspx)
- [Codering voor SQL Server](https://msdn.microsoft.com/library/bb510663.aspx)
- [Altijd gecodeerd wizard](https://msdn.microsoft.com/library/mt459280.aspx)
- [Altijd gecodeerde blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
