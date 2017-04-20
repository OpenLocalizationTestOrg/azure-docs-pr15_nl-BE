<properties
    pageTitle="Altijd gecodeerd: Beschermen van vertrouwelijke gegevens in Azure SQL-Database met de codering van de database | Microsoft Azure"
    description="Beschermen van vertrouwelijke gegevens in uw SQL-database in minuten."
    keywords="coderen van gegevens, sql-versleuteling, codering van de database, gevoelige gegevens altijd gecodeerd"
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

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Altijd gecodeerd: Bescherming van gevoelige gegevens in een SQL-Database en uw coderingssleutels worden opgeslagen in het Windows-certificaatarchief

> [AZURE.SELECTOR]
- [Azure sleutel kluis](sql-database-always-encrypted-azure-key-vault.md)
- [Windows-certificaatarchief](sql-database-always-encrypted.md)


In dit artikel wordt beschreven hoe u voor het beveiligen van vertrouwelijke gegevens in een SQL-database met de codering van de database met behulp van de [Wizard altijd gecodeerd](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Dit ziet u ook hoe u uw coderingssleutels worden opgeslagen in het Windows-certificaatarchief.

Terwijl de gegevens in gebruik is, ervoor te zorgen dat vertrouwelijke gegevens nooit als leesbare tekst in het databasesysteem weergegeven altijd gecodeerd is een nieuwe technologie voor gegevens codering in Azure SQL-Database en SQL Server die helpt bij het beschermen van vertrouwelijke gegevens in rust op de server tijdens het verkeer tussen client en server. Nadat u de gegevens versleutelen, alleen client-toepassingen of servers app die toegang tot de sleutels hebben toegang tot gegevens als leesbare tekst. Zie voor gedetailleerde informatie [Altijd gecodeerd (Database-Engine)](https://msdn.microsoft.com/library/mt163865.aspx).

Na het configureren van de database voor het gebruik altijd gecodeerd, maakt u een clienttoepassing in C# met Visual Studio om te werken met de gecodeerde gegevens.

Volg de stappen in dit artikel voor informatie over het instellen van altijd gecodeerd voor Azure SQL-database. In dit artikel leert u hoe u de volgende taken uitvoeren:

- Gebruik de wizard altijd gecodeerd in SSMS [Altijd gecodeerde sleutels](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)maken.
    - Maak een [Kolom hoofdsleutel (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Maak een [kolom coderingssleutel (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Een databasetabel maken en kolommen te coderen.
- Een toepassing maken die wordt ingevoegd en worden gegevens weergegeven uit de gecodeerde kolommen selecteert.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u nodig:

- Azure-account en abonnement. Als u nog geen hebt, meld u aan voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) versie 13.0.700.242 of hoger.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) of hoger (op de clientcomputer).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Maak een lege database voor SQL
1. Log in om de [Azure portal](https://portal.azure.com/).
2. Klik op **nieuwe** > **Data- +** > **SQL-Database**.
3. Een **lege** database maken met de naam **kliniek** op een nieuwe of bestaande. Zie voor gedetailleerde instructies over het maken van een database in de Azure portal [maken een SQL-database, in minuten](sql-database-get-started.md).

    ![Een lege database maken](./media/sql-database-always-encrypted/create-database.png)

U moet de verbindingsreeks verderop in de handleiding. Nadat de database is gemaakt, gaat u naar de nieuwe database in de kliniek en de verbindingsreeks te kopiëren. U kunt de verbindingsreeks ophalen op elk gewenst moment, maar het is gemakkelijk om deze te kopiëren in de portal Azure.

1. Klik op **SQL-databases** > **kliniek** > **tekenreeksen voor een databaseverbinding wordt weergegeven**.
2. Kopieer de verbindingsreeks voor **ADO.NET**.

    ![De verbindingsreeks kopiëren](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Verbinding maken met de database met SSMS

SSMS openen en verbinding maken met de server met de database van de kliniek.


1. SSMS openen. (Klik op **verbinding maken met** > **-Database-Engine** voor het openen van het venster **verbinding maken met Server** als deze nog niet geopend).
2. Voer uw naam en referenties. De naam van de server kunt u vinden op de SQL-database blade en in de verbindingsreeks u hebt gekopieerd eerder. Typ de volledige servernaam, met inbegrip van *database.windows.net*.

    ![De verbindingsreeks kopiëren](./media/sql-database-always-encrypted/ssms-connect.png)

Hiermee opent u het venster **Nieuwe firewallregel** , Azure aanmelden als SSMS een nieuwe firewallregel maken voor u laten.


## <a name="create-a-table"></a>Een tabel maken

In dit gedeelte maakt u een tabel Patiënten gegevens kan bevatten. Dit is een normale tabel aanvankelijk--u codering configureren in de volgende sectie.

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

SSMS biedt een wizard kunt u eenvoudig configureren altijd gecodeerd door het instellen van het CMK, CEK en gecodeerde kolommen voor u.

1. **Databases**uitbreiden > **kliniek** > **tabellen**.
2. Klik met de rechtermuisknop op de tabel **patiënten** en selecteer **Kolommen coderen** om de wizard altijd gecodeerd te openen:

    ![Coderen van kolommen](./media/sql-database-always-encrypted/encrypt-columns.png)

De wizard altijd gecodeerd bevat de volgende secties: **Kolom selecteren**, **Hoofdsleutel configuratie** (CMK), **validatie**en **Overzicht**.

### <a name="column-selection"></a>Kolom selecteren ###

Klik op **volgende** op **de introductiepagina te openen op de pagina **Kolom** ** . Op deze pagina, selecteert u de kolommen die u coderen wilt, [het type codering en de coderingssleutel voor welke kolom (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) te gebruiken.

**Sofi-nummer** en de **Geboortedatum** van informatie voor elke patiënt worden gecodeerd. Deterministic codering, dat ondersteuning biedt voor gelijkheid lookups, joins en groeperen op de kolom **sofi-nummer** gebruikt. De kolom **Geboortedatum** gebruikt randomiseren codering die wordt niet ondersteund.

Het **Type codering** voor de kolom **sofi-nummer** instellen **Deterministic** en de kolom **Geboortedatum** **Randomized**. Klik op **volgende**.

![Coderen van kolommen](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Configuratie van hoofdsleutel###

De pagina **Configuratie van hoofdsleutel** is waar u uw CMK instellen en selecteer de provider sleutelarchief waar het CMK worden opgeslagen. U kunt op dit moment een CMK opslaan in het Windows-certificaatarchief, Azure sleutel kluis of een hardwarebeveiligingsmodule (HSM). Deze zelfstudie laat zien hoe uw sleutels worden opgeslagen in het Windows-certificaatarchief.

Controleer of **Windows-certificaatarchief** is geselecteerd en klik op **volgende**.

![Configuratie van hoofdsleutel](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Validatie###

U kunt de kolommen nu coderen of sla een PowerShell script later uit te voeren. Voor deze zelfstudie **Doorgaan om nu te voltooien** en klik op **volgende**.

### <a name="summary"></a>Samenvatting###

Controleer of de instellingen voor alle corrigeren en klik op **Voltooien** om de instellingen voor altijd gecodeerd.

![Samenvatting](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Controleer of de acties van de wizard

Nadat de wizard voltooid is, wordt de database instellen voor altijd gecodeerd. De wizard heeft de volgende acties uitgevoerd:

- Een CMK gemaakt.
- Een CEK gemaakt.
- De geselecteerde kolommen voor codering geconfigureerd. De tabel **patiënten** op dit moment geen gegevens bevat, maar worden bestaande gegevens in de geselecteerde kolommen worden nu gecodeerd.

U kunt controleren of het maken van de sleutels in SSMS door te gaan naar de **kliniek** > **Security** > **Altijd gecodeerde sleutels**. U ziet nu de nieuwe sleutels die de wizard voor u gegenereerd.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Een clienttoepassing die met de gecodeerde gegevens werkt maken

Nu dat altijd gecodeerd is ingesteld, kunt u een toepassing die op de gecodeerde kolommen *Invoegen* en *selecteert u* maken. De voorbeeldtoepassing is uitgevoerd, moet u uitvoeren op dezelfde computer waarop u de wizard altijd gecodeerd uitgevoerd. Als u de toepassing uitvoert op een andere computer, moet u uw certificaten altijd gecodeerd op de computer waarop de clienttoepassing te implementeren.  

> [AZURE.IMPORTANT] Uw toepassing moet gebruiken [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objecten leesbare gegevens worden doorgegeven aan de server met kolommen altijd gecodeerd. Letterlijke waarden doorgeven zonder SqlParameter objecten resulteert in een uitzondering.


1. Visual Studio openen en een nieuwe C#-consoletoepassing maken. Zorg ervoor dat uw project is ingesteld op **.NET Framework 4.6** of hoger.
2. Geef het project de **AlwaysEncryptedConsoleApp** en klik op **OK**.

![Nieuwe consoletoepassing](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>De verbindingsreeks waarmee altijd gecodeerd wijzigen

In dit gedeelte wordt uitgelegd hoe inschakelen altijd gecodeerd in de verbindingsreeks. Wijzigt u de console app die u zojuist hebt gemaakt in de volgende sectie, "Altijd gecodeerd voorbeeldtoepassing console."


Zodat altijd gecodeerd, moet u de **Coderingsinstelling kolom** trefwoord toevoegen aan uw verbindingsreeks en stel deze in op **ingeschakeld**.

U kunt deze rechtstreeks in de verbindingsreeks instellen of kunt u deze eigenschap instellen met behulp van een [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). De voorbeeldtoepassing in het volgende gedeelte ziet u hoe u **SqlConnectionStringBuilder**gebruikt.

> [AZURE.NOTE] Dit is de enige wijziging is vereist in een clienttoepassing specifieke altijd gecodeerd. Als u een bestaande toepassing die de verbindingsreeks extern opgeslagen (dat wil zeggen in een configuratiebestand), u mogelijk altijd gecodeerd inschakelen zonder code.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Schakel altijd gecodeerd in de connection string

Het volgende trefwoord toevoegen aan uw verbindingsreeks:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Altijd gecodeerd met een SqlConnectionStringBuilder inschakelen

De volgende code toont hoe u altijd gecodeerd door de [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) instellen op [ingeschakeld](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Altijd gecodeerd console voorbeeldtoepassing

In dit voorbeeld wordt gedemonstreerd hoe u:

- Wijzig de verbindingsreeks waarmee altijd gecodeerd.
- Gegevens invoegen in de kolommen gecodeerd.
- Selecteer een record door te filteren op een specifieke waarde in een kolom gecodeerd.

De inhoud van de **Program.cs** vervangen door de volgende code. De verbindingsreeks voor de globale connectionString-variabele in de regel direct boven de Main-methode vervangen door de geldige verbindingsreeks van de Azure portal. Dit is de enige wijziging moet u deze code.

De app om te zien altijd gecodeerd in actie worden uitgevoerd.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
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

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
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

U kunt snel controleren dat de gegevens op de server worden gecodeerd door het opvragen van de gegevens van de **patiënten** met SSMS. (Gebruik uw huidige verbinding waar nog niet de instelling van de kolom codering is ingeschakeld.)

De volgende query uitvoeren op de database van de kliniek.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

U kunt zien dat de gecodeerde kolommen gegevens als leesbare tekst bevatten.

   ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted/ssms-encrypted.png)


Voor het gebruik van SSMS toegang tot de gegevens als leesbare tekst, kunt u toevoegen de **kolom coderingsinstelling = ingeschakeld** -parameter voor de verbinding.

1. In SSMS, uw server in **Object Explorer**met de rechtermuisknop en klik vervolgens op **verbinding verbreken**.
2. Klik op **verbinding maken met** > **-Database-Engine** , open het venster **verbinding maken met de Server** en klik vervolgens op **Opties**.
3. Klik op **Extra verbindingsparameters** en typ **kolom coderingsinstelling = ingeschakeld**.

    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. De volgende query uitvoeren op de database van de **kliniek** .

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Nu kunt u de gegevens in de kolommen gecodeerde leesbare tekst zien.


    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Als u verbinding met SSMS (of een willekeurige client) vanaf een andere computer maakt, wordt geen toegang hebben tot de coderingssleutels en is niet mogelijk om de gegevens te decoderen.



## <a name="next-steps"></a>Volgende stappen
Nadat u een database die altijd gecodeerd maakt worden, kunt u het volgende doen:

- In dit voorbeeld worden uitgevoerd vanaf een andere computer. Deze geen toegang tot de sleutels voor gegevenscodering, zodat deze geen toegang tot de gegevens als platte tekst en kan niet worden uitgevoerd.
- [Draaien en het opschonen van uw sleutels](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migreren gegevens die al is gecodeerd met altijd gecodeerd](https://msdn.microsoft.com/library/mt621539.aspx).
- [Certificaten op andere clientcomputers implementeren altijd gecodeerd](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (Zie de sectie 'Maken van certificaten beschikbaar voor toepassingen en gebruikers').

## <a name="related-information"></a>Verwante informatie

- [Altijd gecodeerd (ontwikkeling van de client)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Transparante gegevenscodering](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server-versleuteling](https://msdn.microsoft.com/library/bb510663.aspx)
- [Wizard altijd gecodeerd](https://msdn.microsoft.com/library/mt459280.aspx)
- [Altijd gecodeerde Blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
