<properties
    pageTitle="Een SQL-verbindingsfout, een tijdelijke fout herstellen | Microsoft Azure"
    description="Informatie over het oplossen van problemen, opsporen en voorkomen dat een tijdelijke fout in Azure SQL-Database of een SQL-verbindingsfout. "
    keywords="SQL-verbinding, verbindingsreeks, verbindingsproblemen, tijdelijke fout, fout"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Problemen oplossen, diagnose en voorkomen van fouten in SQL-verbinding en tijdelijke voor SQL-Database

In dit artikel wordt beschreven hoe om te voorkomen, problemen oplossen, diagnose en vermindert het aantal fouten en tijdelijke fouten die uw clienttoepassing tegenkomt wanneer u samenwerkt met Azure SQL-Database. Informatie over het configureren opnieuw logica, de verbindingsreeks en andere instellingen aanpassen.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Tijdelijke fouten tijdelijk

Een tijdelijke fout - ook tijdelijke storing - heeft een onderliggende oorzaak snel om op te lossen zelf. Een occasionele oorzaak van fouten van voorbijgaande aard is wanneer het systeem Azure snel hardwarebronnen voor een betere taakverdeling verschillende werkbelastingen verplaatst. De meeste van deze gebeurtenissen opnieuw worden vaak in minder dan 60 seconden voltooien. Tijdens deze tijdspanne herconfiguratie wellicht verbindingsproblemen met Azure SQL-Database. Toepassingen die verbinding maken met Azure SQL-Database moeten worden gemaakt om te verwachten dat deze fouten van voorbijgaande aard, verwerken ze opnieuw logica te implementeren in hun code in plaats van het zien van deze gebruikers fouten.

Als het clientprogramma ADO.NET, is het programma door het weggooien van een **SqlException**verteld over tijdelijke fout. De eigenschap **Number** kan worden vergeleken met de lijst van tijdelijke fouten bij de bovenkant van het onderwerp: [foutcodes van de SQL voor SQL-Database-clienttoepassingen](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Verbinding versus de opdracht

U zult opnieuw verbinding met de SQL of opnieuw, bepalen afhankelijk van de volgende:

* **Een tijdelijke fout optreedt tijdens een verbindingspoging**: de verbinding moet worden geprobeerd na enkele seconden vertraagd.

* **Een tijdelijke fout optreedt tijdens een SQL-queryopdracht**: de opdracht moet niet onmiddellijk opnieuw geprobeerd. In plaats daarvan, na een vertraging, het moet vers verbinding. Vervolgens de opdracht opnieuw kan worden verzonden.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Logische fouten van voorbijgaande aard opnieuw proberen


Clientprogramma's die af en toe een tijdelijke fout tegenkomen zijn krachtiger wanneer ze opnieuw bedrijfslogica bevatten.


Wanneer het programma met Azure SQL-Database via een 3e partij middleware communiceert, informeren bij de leverancier of de middleware de logica opnieuw fouten van voorbijgaande aard bevat.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Beginselen voor opnieuw proberen


- Als de fout van voorbijgaande aard is, moet opnieuw geprobeerd een poging om een verbinding te openen.


- Een SQL SELECT-instructie met een tijdelijke fout mislukt moet niet opnieuw geprobeerd rechtstreeks.
 - In plaats daarvan een nieuwe verbinding tot stand brengen en probeer het vervolgens opnieuw de selecteren.


- Wanneer u een SQL UPDATE-instructie is mislukt vanwege een tijdelijke fout, moeten vers verbinding maken voordat de UPDATE wordt herhaald.
 - De logica opnieuw moet ervoor zorgen dat de gehele databasetransactie is voltooid of dat de gehele transactie wordt teruggedraaid.


#### <a name="other-considerations-for-retry"></a>Meer informatie over het opnieuw proberen


- Een batch-programma dat automatisch wordt gestart na de werkuren en die worden voltooid voordat de ochtend, erg zeer patiënt met lange tijdsintervallen tussen de nieuwe pogingen.


- Een gebruikersinterfaceprogramma dient rekening gehouden met de menselijke neiging te geven na te lang wachten.
 - De oplossing moet echter niet worden elke paar seconden opnieuw uit te voeren omdat dit beleid met aanvragen voor het systeem overspoelen kan.


#### <a name="interval-increase-between-retries"></a>Verhoging van de interval tussen nieuwe pogingen



Wij raden u aan om te vertragen voor 5 seconden voordat de eerste poging. Na een vertraging van minder dan 5 seconden risico's bedolven onder de cloud-service opnieuw. Voor elke opeenvolgende pogingen de vertraging moet exponentieel, groeien tot een maximum van 60 seconden.

Een bespreking van de *periode te blokkeren* voor clients die gebruikmaken van ADO.NET is beschikbaar in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

U kunt ook instellen hoeveel pogingen voordat het programma zelf wordt beëindigd.


#### <a name="code-samples-with-retry-logic"></a>Codevoorbeelden met logica opnieuw


Codevoorbeelden met Pogingslogica in allerlei verschillende programmeertalen, zijn beschikbaar op:

- [Gegevensverbindingsbibliotheek voor SQL-Database en SQL Server](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>De logica opnieuw testen


Uw logica opnieuw testen, moet u simuleren of treedt er een fout niet kan worden hersteld, terwijl het programma nog actief is.


##### <a name="test-by-disconnecting-from-the-network"></a>Testen met het netwerk verbreken


Een manier kunt u de logica opnieuw testen is uw computer loskoppelen van het netwerk, terwijl het programma actief is. De fout is:

- **SqlException.Number** = 11001
- Bericht: "geen host is onbekend"


Als onderdeel van de eerste nieuwe poging het programma de spelfout corrigeren, en vervolgens probeert om verbinding te maken.


Om dit praktische, haal de computer uit het netwerk voordat u het programma start. Het programma herkent een runtime-parameter die ervoor zorgt het programma dat:

1. Tijdelijk 11001 toevoegen aan de lijst met fouten als tijdelijk worden beschouwd.
2. Poging zoals gewoonlijk de eerste verbinding.
3. Nadat de fout is opgetreden, 11001 uit de lijst verwijderen.
4. Wordt een bericht weergegeven waarin de gebruiker om de computer op het netwerk aansluiten.
 - Verdere uitvoering onderbreken met behulp van de methode **Console.ReadLine** of een dialoogvenster met de knop OK. De gebruiker op de Enter-toets drukt nadat de computer is aangesloten op het netwerk.
5. Opnieuw verbinding maken, verwacht succes.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Testen door de naam van de database bevat een typefout wanneer verbinding wordt gemaakt


Het programma kan de naam van de gebruiker voordat de eerste verbindingspoging doelbewust spelt. De fout is:

- **SqlException.Number** = 18456
- Bericht: 'aanmelding mislukt voor gebruiker 'WRONG_MyUserName'. "


Als onderdeel van de eerste nieuwe poging het programma de spelfout corrigeren, en vervolgens probeert om verbinding te maken.


Als u dit praktische, herkent het programma een runtime-parameter die ervoor zorgt de dat:

1. Tijdelijk 18456 toevoegen aan de lijst met fouten als tijdelijk worden beschouwd.
2. Opzet 'WRONG_' toevoegen aan de gebruikersnaam.
3. Nadat de fout is opgetreden, 18456 uit de lijst verwijderen.
4. 'WRONG_' uit de naam van de gebruiker verwijderen.
5. Opnieuw verbinding maken, verwacht succes.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Parameters voor de verbinding opnieuw SqlConnection .NET


Als het clientprogramma een verbinding maakt met klasse **System.Data.SqlClient.SqlConnection**Azure SQL-database met behulp van het .NET Framework, moet u .NET 4.6.1 of hoger, zodat u van de functie van de verbinding opnieuw profiteren kunt. Details van de functie zijn [hier](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Wanneer u de [verbindingsreeks](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) voor het **SqlConnection** -object maakt, kunt u de waarden uit de volgende parameters moet coördineren:

- ConnectRetryCount &nbsp; &nbsp; *(de standaardinstelling is 1. Bereik is 0 tot en met 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(de standaardinstelling is 1 seconde. Bereik is 1 tot en met 60).*
- Time-out van verbinding &nbsp; &nbsp; *(de standaardwaarde is 15 seconden. Bereik is 0 en 2147483647)*


De gekozen waarden moet met name de volgende gelijkheid true:

- Time-out van verbinding = ConnectRetryCount * ConnectionRetryInterval

Bijvoorbeeld, als het aantal = 3 en interval 10 seconden een time-out van = alleen 29 seconden niet helemaal het systeem voldoende tijd voor de 3e en laatste opnieuw te verbinden krijgt: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Verbinding versus de opdracht


De parameters **ConnectRetryCount** en **ConnectRetryInterval** kunnen uw verbinding probeer zonder melding of het programma, zoals het programma de besturing weer terugkrijgt lastigvalt **SqlConnection** -object. Het aantal pogingen kunnen optreden in de volgende situaties:

- aanroepen van de methode mySqlConnection.Open
- aanroepen van de methode mySqlConnection.Execute

Er is een geldt de volgende werkwijze. Als een tijdelijke fout optreedt terwijl de *query* wordt uitgevoerd, het **SqlConnection** -object is niet probeer het verbinden en het zeker is niet opnieuw de query. **SqlConnection** controleert echter zeer snel de verbinding voor het verzenden van de query wordt uitgevoerd. Als snelle controle een probleem detecteert, pogingen **SqlConnection** de bewerking verbinden. Als de poging slaagt, wordt voor de uitvoering van query u verzonden.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Moeten ConnectRetryCount worden gecombineerd met de toepassingslogica opnieuw proberen?

Stel dat uw toepassing opnieuw met krachtige aangepaste logica. Het kan probeer opnieuw verbinding maken met 4 keer. Als u **ConnectRetryInterval** en **ConnectRetryCount** = 3 aan uw verbindingsreeks, verhoogt u het aantal nieuwe pogingen tot en met 4 * 3 = 12 pogingen. U zult niet altijd dat een groot aantal nieuwe pogingen.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Verbindingen met Azure SQL-Database

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>: Verbindingsreeks verbinding


De verbindingsreeks die nodig zijn om verbinding te maken met de SQL-Database Azure wijkt enigszins af van de tekenreeks voor de verbinding met Microsoft SQL Server. Kunt u de verbindingsreeks voor de database van de [Portal Azure](https://portal.azure.com/).


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Verbinding: IP-adres


Moet u de SQL-databaseserver mededeling van het IP-adres van de computer waarop de client wordt geaccepteerd. Hiertoe bewerkt de firewall-instellingen via de [Portal Azure](https://portal.azure.com/).


Als u vergeet om het IP-adres, mislukt het programma met een handige foutmelding dat het vereiste IP-adres.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Zie voor meer informatie: [hoe: firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Verbinding: poorten


Meestal hoeft u alleen om te controleren of poort 1433 voor uitgaande communicatie, op de computer waarop u clientprogramma geopend.


Bijvoorbeeld wanneer uw clientprogramma wordt gehost op een computer met Windows, kunt Windows Firewall op de host u open poort 1433:


1. Open het Configuratiescherm
2. &gt;Alle Configuratiescherm-onderdelen
3. &gt;Windows Firewall
4. &gt;Geavanceerde instellingen
5. &gt;Regels voor uitgaande verbindingen
6. &gt;Acties
7. &gt;Nieuwe regel


Als het clientprogramma op een Azure VM (virtual machine) wordt gehost, moet u lezen:<br/>[Behalve 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).


Zie voor achtergrondinformatie over de cofiguration poorten en IP-adres: [firewall Azure SQL-Database](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Verbinding: ADO.NET 4.6.1


Als uw programma gebruikmaakt van ADO.NET klassen als **System.Data.SqlClient.SqlConnection** verbinding maken met Azure SQL-Database, raden we aan dat u .NET Framework versie 4.6.1 of hoger.


4.6.1 ADO.NET:

- Voor Azure SQL-Database wordt het verbeterde betrouwbaarheid bij het openen van een verbinding met de methode **SqlConnection.Open** . Beste inspanning opnieuw mechanismen in reactie op fouten van voorbijgaande aard, voor bepaalde fouten binnen de time-out van verbinding is nu uitgerust met de methode **Open** .
- Biedt ondersteuning voor groepsgewijze verbindingen. Het gaat hierbij om een doeltreffende controle op de werking van het connection-object geeft het programma.



Wanneer u een connection-object uit een verbindingsgroep gebruikt, wordt aangeraden dat het programma de verbinding tijdelijk sluiten wanneer deze niet direct gebruikt. Opnieuw een verbinding te openen is niet duur is de manier waarop een nieuwe verbinding maken.


Als u met behulp van ADO.NET 4.0 of eerder, wordt aangeraden om te upgraden naar de nieuwste ADO.NET.

- Vanaf November 2015 kunt u [downloaden van ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostische gegevens

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostische gegevens: Test of u kunnen verbinding maken met de hulpprogramma 's


Als uw programma niet goed werkt met Azure SQL-Database, is één diagnostische optie om te proberen verbinding te maken met een hulpprogramma's. In het ideale geval zou het hulpprogramma verbinding maken met behulp van dezelfde bibliotheek die het programma wordt gebruikt.


Op elke Windows-computer, kunt u proberen deze hulpprogramma's:

- SQL Server Management Studio (ssms.exe), die verbinding maakt met behulp van ADO.NET.
- Sqlcmd.exe, verbinding maakt met behulp van [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).


Zodra verbonden, test u of een korte SQL SELECT-query werkt.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostische gegevens: Open poorten controleren


Stel dat u vermoedt dat verbindingspogingen omdat poort problemen mislukken. U kunt een hulpprogramma waarmee u over de poortconfiguraties rapporten uitvoeren op uw computer.


Op Linux kunnen het handig zijn de volgende hulpprogramma's:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (Het van de Voorbeeldwaarde van uw IP-adres wijzigen.)


Op Windows het [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) kan hulpprogramma handig zijn. Hier volgt een voorbeeld kan worden uitgevoerd die de situatie van de poort op een Azure SQL-databaseserver gevraagd en die werd uitgevoerd op een laptop-computer:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostische gegevens: Uw fouten in logboek registreren


Een tussentijds probleem is soms best gediagnosticeerd op grond van een algemeen patroon van dagen of weken.


De client kan helpen in een diagnose door alle fouten logboekregistratie. U kunt mogelijk de logboekvermeldingen correleren met foutgegevens die Azure SQL-Database meldt zich zelf intern.


Onderneming bibliotheek 6 (EntLib60) biedt beheerde klassen om te helpen bij de registratie:

- [5 - zo eenvoudig als een logboek vallen: met behulp van de blokkering van de toepassing aanmelden](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostische gegevens: Het systeemlogboek in Logboeken op fouten controleren


Hier zijn sommige Transact-SQL-instructies SELECT die query van fout- en andere informatie.


| Query van logboek | Beschrijving |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | De weergave van de [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) biedt informatie over afzonderlijke gebeurtenissen, waaronder enkele die ervoor zorgen tijdelijke fouten of fouten in connectiviteit dat.<br/><br/>In het ideale geval kunt u de **start_time** of **end_time** waarden correleren met informatie over wanneer het clientprogramma problemen ondervonden.<br/><br/>**TIP:** U moet verbinding maken met de **master** -database uitvoeren. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | De weergave van de [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) biedt geaggregeerde aantal van de gebeurtenistypen voor meer diagnostische gegevens.<br/><br/>**TIP:** U moet verbinding maken met de **master** -database uitvoeren. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostische gegevens: Zoeken naar probleem-gebeurtenissen in het logboek van de SQL-Database


U kunt zoeken naar gegevens over gebeurtenissen in het logboek van de SQL-Database Azure probleem. Probeer de volgende Transact-SQL SELECT-instructie in de **master** -database:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Een aantal geretourneerde rijen uit sys.fn_xe_telemetry_blob_target_read_file


Volgende is wat een rij geretourneerd als volgt uitzien. De null-waarden weergegeven zijn vaak niet null in andere rijen.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Onderneming bibliotheek 6


Onderneming bibliotheek 6 (EntLib60) is een raamwerk van .NET-klassen dat helpt u bij het implementeren van krachtige clients van cloud-diensten, waarvan de service Azure SQL-Database is. Kunt u zoeken naar onderwerpen die speciaal voor elk gebied waar EntLib60 kunnen helpen bij het eerste bezoek:

- [Onderneming bibliotheek 6 – April 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Pogingslogica voor het afhandelen van fouten van voorbijgaande aard is een gebied waar EntLib60 kunnen helpen:

- [4 - perseverance, geheim van alle successen: met de toepassing tijdelijke fout afhandeling blok](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] De broncode voor de EntLib60 is beschikbaar voor openbare [downloaden](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft heeft geen plannen om verdere functie-updates en onderhoud aan EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60-klassen voor tijdelijke fouten en probeer het opnieuw


De volgende EntLib60-klassen zijn vooral nuttig voor Pogingslogica. Deze zijn of onder de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**verder:

*In de naamruimte* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** , klasse
 - **ExecuteAction** , methode


- **ExponentialBackoff** , klasse


- **SqlDatabaseTransientErrorDetectionStrategy** , klasse


- **ReliableSqlConnection** , klasse
 - **ExecuteCommand** -methode


In de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** , klasse

- **NeverTransientErrorDetectionStrategy** , klasse


Hier vindt u koppelingen naar informatie over EntLib60:

- Gratis [downloaden van het adresboek: Developer's Guide to Microsoft Enterprise Library, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)

- Aanbevolen procedures: [algemene richtlijnen opnieuw](../best-practices-retry-general.md) heeft uitstekende uitvoerige informatie opnieuw logica.

- NuGet [Enterprise Library - toepassing blokkeren tijdelijke fout afhandeling 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) te downloaden

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: De registratie-blok


- Het blok logboekregistratie is een zeer flexibele en configureerbare oplossing waarmee u kunt:
 - Maken en berichten in het logboek opslaan in verschillende locaties.
 - Categoriseren en berichten filteren.
 - Contextuele informatie verzameld die nuttig zijn voor foutopsporing en tracering en voor de controle en de algemene registratie-eisen.


- Het blok logboekregistratie abstracts de functionaliteit voor logboekregistratie van de bestemming van het logboek, zodat de toepassingscode consistent is, ongeacht de locatie en het type van het archief van de doel-logboekregistratie is.


Zie voor meer informatie: [5 - als gemakkelijk als die uit een logboek: de registratie toepassing blokkeren met](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient methode broncode


Vervolgens is de C#-broncode voor de methode **IsTransient** van de klasse **SqlDatabaseTransientErrorDetectionStrategy** . De broncode wordt uitleg gegeven over de fouten die werden beschouwd als tijdelijke en tot slot opnieuw vanaf April 2013.

Talloze **//comment** regels zijn verwijderd uit deze kopie leesbaarheid te benadrukken.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Volgende stappen

- Voor het oplossen van andere veelvoorkomende verbindingsproblemen van Azure SQL-Database, gaat u naar [verbindingsproblemen oplossen met Azure SQL-Database](sql-database-troubleshoot-common-connection-issues.md).

- [SQL Server groepsgewijze (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Bezig met opnieuw proberen* is dat een Apache 2.0 gelicentieerd voor algemene doeleinden Bezig met opnieuw bibliotheek geschreven in **Python**gedrag opnieuw toe te voegen om alles te vereenvoudigen.](https://pypi.python.org/pypi/retrying)
