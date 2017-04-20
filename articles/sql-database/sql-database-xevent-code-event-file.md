<properties 
    pageTitle="Code XEvent gebeurtenisbestand voor SQL-Database | Microsoft Azure" 
    description="PowerShell en Transact-SQL biedt voor een in twee fasen codevoorbeeld dat het doel van de gebeurtenis-bestand in een uitgebreide gebeurtenis op Azure SQL-Database toont. Azure opslag is een noodzakelijk onderdeel van dit scenario." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Bestand doel gebeurteniscode voor uitgebreide gebeurtenissen in de SQL-Database

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Wilt u een volledige voorbeeldcode voor een robuuste manier vastleggen en rapporteren van gegevens voor een uitgebreide gebeurtenis.


Het [doel van de gebeurtenisbestand](http://msdn.microsoft.com/library/ff878115.aspx) wordt in Microsoft SQL Server gebruikt om de gebeurtenis uitvoer naar een bestand van de lokale vaste schijf worden opgeslagen. Maar deze bestanden zijn niet beschikbaar voor Azure SQL-Database. In plaats daarvan gebruiken we de Azure Storage-service voor de ondersteuning van het doel van de gebeurtenisbestand.


Dit onderwerp bevat een codevoorbeeld in twee fasen:


- PowerShell voor het maken van een container Azure opslag in de cloud.

- Transact-SQL:
 - De container Azure opslag toewijzen aan een gebeurtenisbestand doel.
 - Maken en de gebeurtenis sessie te starten, enzovoort.


## <a name="prerequisites"></a>Vereisten


- Azure-account en abonnement. U kunt zich aanmelden voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).


- De database die kunt u een tabel in.
 - Desgewenst kunt u [een demonstratie **AdventureWorksLT** database maken](sql-database-get-started.md) in minuten.


- SQL Server Management Studio (ssms.exe), in het ideale geval meest recente maandelijkse update versie. U kunt de meest recente ssms.exe van downloaden:
 - [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx)-onderwerp.
 - [Een directe link naar de download.](http://go.microsoft.com/fwlink/?linkid=616025)


- U moet de [Azure PowerShell modules](http://go.microsoft.com/?linkid=9811175) geïnstalleerd hebben.
 - De modules bevatten opdrachten als - **Nieuw-AzureStorageAccount**.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Fase 1: PowerShell code voor Azure opslag container


Deze PowerShell is fase 1 van het codevoorbeeld in twee fasen.

Het script wordt gestart met de opdrachten voor het opschonen van nadat een vorige mogelijk uitgevoerd en is rerunnable.



1. Plak het PowerShell-script in een eenvoudige teksteditor zoals Notepad.exe en het script opslaan als een bestand met de extensie **.ps1**.

2. PowerShell ISE starten als Administrator.

3. Typ achter de prompt<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>en druk vervolgens op Enter.

4. Open het bestand **.ps1** in PowerShell ISE. Het script uitvoeren.

5. Het script start eerst een nieuw venster waarin u zich met Azure aanmeldt.
 - Als u het script opnieuw uit te voeren zonder de sessie te onderbreken, hebt u de handige optie van de opdracht **Add-AzureAccount** uit te schakelen.


![PowerShell ISE met Azure-module is geïnstalleerd, het script uitvoeren.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Let op de enkele benoemde waarden die de PowerShell-script wordt afgedrukt wanneer deze wordt beëindigd. U moet deze waarden bewerken in de Transact-SQL-script dat volgt op de fase 2.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Fase 2: Transact-SQL-code die gebruikmaakt van Azure opslag container


- U hebt een PowerShell-script wilt maken van een container Azure opslag fase 1 van deze voorbeeldcode wordt uitgevoerd.
- De volgende Transact-SQL-script moet vervolgens in fase 2 gebruikt de container.


Het script wordt gestart met de opdrachten voor het opschonen van nadat een vorige mogelijk uitgevoerd en is rerunnable.


Het PowerShell-script afgedrukt enkele benoemde waarden wanneer deze is beëindigd. U moet de Transact-SQL-script voor het gebruik van deze waarden bewerken. **TODO** vinden in de Transact-SQL-script te vinden van de punten.


1. Open SQL Server Management Studio (ssms.exe).

2. Verbinding maken met uw database Azure SQL-Database.

3. Klik op om een nieuwe Querydeelvenster te openen.

4. Plak de volgende Transact-SQL-script in het Querydeelvenster.

5. Elke **taak** in het script te vinden en breng de gewenste wijzigingen.

6. Opslaan en vervolgens het script uitvoeren.


&nbsp;


> [AZURE.WARNING] De SAS waarde gegenereerd door de voorgaande PowerShell script begint met een '?' (vraagteken). Wanneer u de sleutel SA's in de volgende T-SQL-script gebruikt, moet u *de regelafstand '?' te verwijderen*. Anders uw inspanningen mogelijk geblokkeerd door de beveiliging.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Als het doel niet koppelen wanneer u uitvoert, moet u stoppen en opnieuw starten van de gebeurtenis-sessie:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Uitvoer


Wanneer de Transact-SQL-script is voltooid, klikt u op een cel onder de kolomkop van de **event_data_XML** . Één **<event>** element wordt weergegeven waarin u een UPDATE-instructie.

Hier is een **<event>** -element dat is gegenereerd tijdens de test:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


De volgende systeemfunctie het voorgaande Transact-SQL-script gebruikt voor het lezen van de event_file:

- [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Een uitleg van de geavanceerde opties voor het bekijken van gegevens van uitgebreide gebeurtenissen is beschikbaar op:

- [Geavanceerde weergave van doelgegevens van uitgebreide gebeurtenissen](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Het voorbeeld moet worden uitgevoerd op de SQL Server converteert


Stel dat u wilt het voorgaande voorbeeld van de Transact-SQL voor Microsoft SQL Server worden uitgevoerd.


- Voor eenvoud wilt u volledig gebruik van de container voor de opslag van Azure vervangen door een eenvoudige zoals **C:\myeventdata.xel**. Het bestand zou worden geschreven naar de lokale vaste schijf van de computer waarop SQL Server.


- U hoeft niet elk soort Transact-SQL-instructies voor de **hoofdsleutel maken** en **REFERENTIES maken**.


- In het overzicht **Maken GEBEURTENIS sessie** in de component **Doel toevoegen** , vervangt u de toegewezen waarde voor HTTP-ter **filename =** met een tekenreeks van volledige pad zoals **C:\myfile.xel**.
 - Geen opslag van Azure rekening moet worden betrokken.


## <a name="more-information"></a>Meer informatie


Zie voor meer informatie over accounts en containers in de Azure Storage-service:

- [Het gebruik van .NET Blob-opslag](../storage/storage-dotnet-how-to-use-blobs.md)
- [Naamgeving en verwijzen naar de Containers, BLOB's en metagegevens](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Werken met de basiscontainer](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Les 1: Een opgeslagen-beleid en een gedeelde access-handtekening maken op een Azure-container](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Les 2: Maak een SQL Server-referenties met behulp van een gedeelde access-handtekening](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

