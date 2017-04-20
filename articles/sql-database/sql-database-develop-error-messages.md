<properties
    pageTitle="Foutcodes voor SQL - database verbindingsfout | Microsoft Azure"
    description="Meer informatie over de foutcodes van de SQL voor SQL-Database-clienttoepassingen, zoals algemene database verbindingsfouten, problemen met de database kopiëren en algemene fouten. "
    keywords="SQL-foutcode, access sql, database verbindingsfout, sql-foutcodes"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>SQL-foutcodes voor SQL-Database-clienttoepassingen: verbindingsfout en andere problemen


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


In dit artikel worden de foutcodes van de SQL voor SQL-Database client-toepassingen, inclusief database verbindingsfouten, tijdelijke fouten (ook wel fouten van voorbijgaande aard) resource governance fouten, problemen met de database kopiëren, elastische van toepassingen en andere fouten. De meeste categorieën zijn bepaalde Azure SQL-database en niet van toepassing op Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Database verbindingsfouten, fouten van voorbijgaande aard en andere tijdelijke fouten

In de volgende tabel heeft betrekking op de SQL-foutcodes voor verlies van verbindingsfouten en andere tijdelijke fouten die kunnen optreden wanneer de toepassing wordt geprobeerd toegang te krijgen tot de SQL-Database. Zie [verbinding maken met Azure SQL-Database](sql-database-libraries.md)om begonnen zelfstudies op verbinding maken met Azure SQL-Database.

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Meest voorkomende fouten in database verbinding en tijdelijke storing

De Azure infrastructuur heeft de mogelijkheid om dynamisch servers opnieuw te configureren als zware werklasten ontstaan in de service SQL-Database.  Dit dynamische gedrag ervoor zorgen dat het clientprogramma voor de verbinding met de SQL-Database wordt verbroken. Dit soort fout is een *fout met betrekking tot tijdelijke*genoemd.

Als het clientprogramma opnieuw logica, kunt het proberen om opnieuw verbinding te maken na de toediening van de tijd die tijdelijke fout gecorrigeerd.  Wij raden u aan om te vertragen voor 5 seconden voordat de eerste poging. Na een vertraging van minder dan 5 seconden risico's bedolven onder de cloud-service opnieuw. Voor elke opeenvolgende pogingen de vertraging moet exponentieel, groeien tot een maximum van 60 seconden.

Fouten van voorbijgaande aard veroorzaakt meestal tot uiting als een van de volgende foutberichten weergegeven vanuit uw programma:

- De database < db_name > op server < Azure_instance > is momenteel niet beschikbaar. Probeer de verbinding later opnieuw. Als het probleem zich blijft voordoen, neem contact op met customer support en geef de tracering sessie-ID van de < session_id >

- De database < db_name > op server < Azure_instance > is momenteel niet beschikbaar. Probeer de verbinding later opnieuw. Als het probleem zich blijft voordoen, neem contact op met customer support en geef de tracering sessie-ID van de < session_id >. (Microsoft SQL Server, fout: 40613)

- Een bestaande verbinding is verbroken door de externe host.

- System.Data.Entity.Core.EntityCommandExecutionException: Er is een fout opgetreden tijdens het uitvoeren van de definitie van de opdracht. Zie de interne uitzondering voor meer informatie. ---> System.Data.SqlClient.SqlException: een transportniveau fout is opgetreden tijdens het ontvangen van de resultaten van de server. (provider: sessieprovider, fout: 19 - fysieke verbinding is niet bruikbaar)

Zie voor voorbeelden van programmacode van Pogingslogica:

- [Gegevensverbindingsbibliotheek voor SQL-Database en SQL Server](sql-database-libraries.md) 
- [Acties verbindingsfouten en fouten van voorbijgaande aard in de SQL-Database herstellen](sql-database-connectivity-issues.md)

Een bespreking van de *periode te blokkeren* voor clients die gebruikmaken van ADO.NET is beschikbaar in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Fout met betrekking tot tijdelijke foutcodes

De volgende fouten zijn tijdelijk en moeten opnieuw worden geprobeerd in de toepassingslogica 

| Foutcode | Ernst | Beschrijving |
| ---: | ---: | :--- |
| 4060 | 16 | Kan database niet openen '% & #x2a; ls' aangevraagd door de aanmelding. De aanmelding is mislukt. |
|40197|17|De service is een fout bij het verwerken van je aanvraag opgetreden. Probeer het opnieuw. Foutcode: %d.<br/><br/>U ontvangt deze fout, wanneer de service uitgeschakeld door software of hardware-upgrades, hardwarestoringen of failover-problemen is. De foutcode (%d) die zijn ingesloten in het bericht van de fout 40197 bevat aanvullende informatie over het soort storing of failover die is opgetreden. Enkele voorbeelden van de fout codes worden ingesloten in het bericht van de fout 40197 zijn 40020, 40143, 40166 en 40540.<br/><br/>Opnieuw verbinding maken met de SQL-Database server, maakt u automatisch verbinding met een gezonde kopie van de database. Uw toepassing moet 40197, foutenlogboek ingesloten foutcode (%d) in het bericht om problemen op te vangen en opnieuw verbinding te maken met SQL-Database totdat de resources beschikbaar zijn en de verbinding opnieuw is gemaakt.|
|40501|20|De service is momenteel bezet. Probeer de aanvraag opnieuw na 10 seconden. Incident-ID: %ls. Code: %d.<br/><br/>*Opmerking:* Zie voor meer informatie:<br/>• [Azure SQL Database resource limieten](sql-database-resource-limits.md).
|40613|17|Database '% & #x2a; ls' op server '% & #x2a; ls' is momenteel niet beschikbaar. Probeer de verbinding later opnieuw. Als het probleem zich blijft voordoen, contact op met customer support en geef de ID van de sessie tracering van '% & #x2a; ls'.|
|49918|16|Kan aanvraag niet verwerken. Onvoldoende bronnen voor het verwerken van aanvragen.<br/><br/>De service is momenteel bezet. Probeer de aanvraag later opnieuw. |
|49919|16|Proces kan geen aanvraag maken of bijwerken. Te veel bewerkingen maken of bijwerken in uitvoering voor abonnement "% ld".<br/><br/>De service is bezig met verwerken van meerdere maken of bijwerken van aanvragen voor uw abonnement of server. Aanvragen voor de optimalisatie van de resource momenteel geblokkeerd. [Sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) voor bewerkingen in afwachting van een query. Wacht tot in afwachting van maken of bijwerken aanvragen voltooid zijn of verwijder een van de in behandeling zijnde aanvragen en uw aanvraag later opnieuw proberen. |
|49920|16|Kan aanvraag niet verwerken. Te veel bewerkingen uitgevoerd voor abonnement "% ld".<br/><br/>De service is bezig met de verwerking van meerdere aanvragen voor dit abonnement. Aanvragen voor de optimalisatie van de resource momenteel geblokkeerd. [Sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) voor de bewerkingsstatus query. Wacht totdat aanvragen in behandeling zijn voltooid of verwijder een van de in behandeling zijnde aanvragen en uw aanvraag later opnieuw proberen. |

## <a name="database-copy-errors"></a>Fouten in de database kopiëren

De volgende fouten kunnen optreden bij het kopiëren van een database in Azure SQL-Database. Zie voor meer informatie [Azure SQL-Database kopiëren](sql-database-copy.md).


|Foutcode|Ernst|Beschrijving|
|---:|---:|:---|
|40635|16|De client met het IP-adres '% & #x2a; ls' is tijdelijk uitgeschakeld.|
|40637|16|Maak databasekopie is momenteel uitgeschakeld.|
|40561|16|Databasekopie is mislukt. De bron- of doel-database bestaat niet.|
|40562|16|Databasekopie is mislukt. De brondatabase is verloren gegaan.|
|40563|16|Databasekopie is mislukt. De doeldatabase is verloren gegaan.|
|40564|16|Databasekopie is een interne fout opgetreden. Doeldatabase neerzetten en probeer het opnieuw.|
|40565|16|Databasekopie is mislukt. Niet meer dan 1 gelijktijdige database-exemplaar uit dezelfde bron is toegestaan. Doeldatabase neerzetten en probeer het later opnieuw.|
|40566|16|Databasekopie is een interne fout opgetreden. Doeldatabase neerzetten en probeer het opnieuw.|
|40567|16|Databasekopie is een interne fout opgetreden. Doeldatabase neerzetten en probeer het opnieuw.|
|40568|16|Databasekopie is mislukt. Brondatabase is niet meer beschikbaar. Doeldatabase neerzetten en probeer het opnieuw.|
|40569|16|Databasekopie is mislukt. Doeldatabase is niet meer beschikbaar. Doeldatabase neerzetten en probeer het opnieuw.|
|40570|16|Databasekopie is een interne fout opgetreden. Doeldatabase neerzetten en probeer het later opnieuw.|
|40571|16|Databasekopie is een interne fout opgetreden. Doeldatabase neerzetten en probeer het later opnieuw.|

## <a name="resource-governance-errors"></a>Resourcefouten governance

De volgende fouten worden veroorzaakt door overmatig gebruik van resources tijdens het werken met Azure SQL-Database. Bijvoorbeeld:

- Een transactie is te lang geopend.
- Een transactie is te veel vergrendelingen.
- Een toepassing is er te veel geheugen.
- Een toepassing teveel verbruikt `TempDb` ruimte.

Verwante onderwerpen:

* Meer gedetailleerde informatie vindt u hier: [Azure SQL Database resource limieten](sql-database-resource-limits.md)

|Foutcode|Ernst|Beschrijving|
|---:|---:|:---|
|10928|20|Resource-ID: %d. De limiet voor de database voor %s is %d en is bereikt. Zie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637)voor meer informatie.<br/><br/>De Resource-ID geeft aan dat de resource die de limiet is bereikt. Voor worker-threads, de Resource-ID = 1. Voor de sessies, de Resource-ID = 2.<br/><br/>*Opmerking:* Zie voor meer informatie over deze fout en hoe u deze kunt oplossen:<br/>• [Azure SQL Database resource limieten](sql-database-resource-limits.md). |
|10929|20|Resource-ID: %d. De minimale garantie %s is %d, maximum aantal %d en het huidige gebruik van de database is %d. De server is echter momenteel te druk om te ondersteunen groter zijn dan %d aanvragen voor deze database. Zie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637)voor meer informatie. Zo niet, probeer het later opnieuw.<br/><br/>De Resource-ID geeft aan dat de resource die de limiet is bereikt. Voor worker-threads, de Resource-ID = 1. Voor de sessies, de Resource-ID = 2.<br/><br/>*Opmerking:* Zie voor meer informatie over deze fout en hoe u deze kunt oplossen:<br/>• [Azure SQL Database resource limieten](sql-database-resource-limits.md).|
|40544|20|De database heeft zijn grootte-quotum bereikt. Data-partitie of verwijderen, drop indexen of Raadpleeg de documentatie voor mogelijke oplossingen.|
|40549|16|Sessie is beëindigd omdat er een langlopende transactie. Probeer het verkorten van uw transactie.|
|40550|16|De sessie is beëindigd omdat er te veel sloten heeft verworven. Probeer het lezen of minder rijen in één transactie te wijzigen.|
|40551|16|De sessie is beëindigd omdat buitensporige `TEMPDB` gebruik. Probeer de query om het ruimtegebruik van de tijdelijke tabel te wijzigen.<br/><br/>*Tip:* Als u tijdelijke objecten ruimte besparen in de `TEMPDB` database door tijdelijke objecten slepen en neerzetten nadat u deze niet langer nodig zijn door de sessie.|
|40552|16|De sessie is beëindigd vanwege het gebruik van buitensporige transactielogboeken ruimte. Probeer minder rijen in één transactie te wijzigen.<br/><br/>*Tip:* Als u bulk ingevoegd met behulp van de `bcp.exe` hulpprogramma of de `System.Data.SqlClient.SqlBulkCopy` klasse, kunt u met behulp van de `-b batchsize` of `BatchSize` opties voor het beperken van het aantal rijen in elke transactie naar de server gekopieerd. Als u opnieuw van een index met maken de `ALTER INDEX` -instructie, probeer met behulp van de `REBUILD WITH ONLINE = ON` optie.|
|40553|16|De sessie is beëindigd vanwege overmatig geheugengebruik. Probeer de query voor het verwerken van minder rijen te wijzigen.<br/><br/>*Tip:* Vermindering van het aantal `ORDER BY` en `GROUP BY` operations in Transact-SQL-code de geheugenvereisten van de query wordt beperkt.|

## <a name="elastic-pool-errors"></a>Elastische pool fouten

De volgende fouten zijn gerelateerd aan het maken en gebruiken van groepen van Elastics.

| Foutnummer | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | De elastische toepassingen kan de opslaglimiet heeft bereikt. Het gebruik van opslag voor de elastische toepassingen niet meer dan MBs (%d). | Elastische ruimte de limiet in MBs. | Er wordt geprobeerd om gegevens te schrijven naar een database als de opslaglimiet van de elastische groep is bereikt. | Geef op rekening houden met het verhogen van de DTUs van de elastische van toepassingen indien mogelijk om de opslaglimiet te verhogen, verlagen de opslag die wordt gebruikt door afzonderlijke databases in de groep met elastische of databases verwijderen uit de groep met elastische. |
| 10929 | EX_USER | De minimale garantie %s is %d, maximum aantal %d en het huidige gebruik van de database is %d. De server is echter momenteel te druk om te ondersteunen groter zijn dan %d aanvragen voor deze database. Zie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) voor meer informatie. Zo niet, probeer het later opnieuw. | DTU min per database. Max DTU per database | Het totaal aantal gelijktijdige werknemers (aanvragen) in alle databases in de groep met elastische heeft geprobeerd te overschrijden de limiet van de. | Geef op rekening houden met de DTUs van de elastische van toepassingen indien mogelijk verhogen om de limiet van de werknemer of databases verwijderen uit de groep met elastische. |
| 40844 | EX_USER | Database '%ls' op de Server '%ls' is een '%ls'-database in een elastische toepassingen en een continue kopiëren relatie kan hebben. | naam van de database, database edition, servernaam | Een StartDatabaseCopy-opdracht wordt uitgevoerd voor een niet-premium db in een elastische toepassingen. | Binnenkort beschikbaar |
| 40857 | EX_USER | Elastische adresgroep voor de server niet gevonden: '%ls', elastische groepnaam: '%ls'. | naam van de server. elastische groepnaam | Opgegeven elastische toepassingen bestaat niet in de opgegeven server. | Geef de naam van een geldige elastische toepassingen. |
| 40858 | EX_USER | Elastische toepassingen '%ls' bestaat al in de server: '%ls' | naam van groep elastische servernaam | Opgegeven elastische toepassingen bestaat al in de opgegeven logische server. | Nieuwe naam voor elastisch toepassingen bieden. |
| 40859 | EX_USER | Elastische toepassingen ondersteunt geen service tier '%ls'. | elastische pool service tier | Laag van de opgegeven service wordt niet ondersteund voor het inrichten van elastische pool. | Bieden de juiste versie of servicelaag de laag standaard service leeg laten. |
| 40860 | EX_USER | Combinatie van elastische pool '%ls' en service doelstelling '%ls' is ongeldig. | de naam van de elastische toepassingen; niveau objectieve servicenaam | Elastische doelstelling en de service kan samen worden opgegeven als service doel is opgegeven als 'ElasticPool'. | Geef de juiste combinatie van elastische en de doelstelling van de service. |
| 40861 | EX_USER | De editie van database ' %. *ls' kunnen niet anders dan de elastische pool servicelaag is ' %.* ls. | database edition elastische pool service tier | De database-editie is anders dan de elastische pool servicelaag. | Geef niet een database edition die verschilt van de laag elastische pool service.  Houd er rekening mee dat de database edition niet hoeft te worden opgegeven. |
| 40862 | EX_USER | Elastische groepnaam moet worden opgegeven als de doelstelling elastische groep service is opgegeven. | Geen | Elastische pool service doelstelling vormt geen unieke identificatie voor een elastische toepassingen. | Geef de naam elastische toepassingen als met de elastische pool service doelstelling. |
| 40864 | EX_USER | De DTUs voor de elastische toepassingen moet ten minste (%d) DTUs voor servicelaag van de ' %. * ls'. | DTUs voor elastische toepassingen; elastische pool servicelaag. | Bij het instellen van de DTUs voor de elastische toepassingen beneden de ondergrens. | Probeer het opnieuw instellen van de DTUs voor de elastische pool aan ten minste het minimum. |
| 40865 | EX_USER | De DTUs voor de elastische toepassingen niet overschrijden (%d) DTUs voor servicelaag van de ' %. * ls'. | DTUs voor elastische toepassingen; elastische pool servicelaag. | Bij het instellen van de DTUs voor de elastische boven het maximum van toepassingen. | Probeer het opnieuw instellen van de DTUs voor de elastische toepassingen op niet groter is dan de maximale limiet. |
| 40867 | EX_USER | De DTU max per database moet ten minste (%d) voor de opgegeven service ' %. * ls'. | Max DTU per database. elastische pool service tier | Bij het instellen van de DTU max per database onder de ondersteunde limiet. | Overweeg het gebruik van de elastische pool service-laag die de gewenste instelling ondersteunt. |
| 40868 | EX_USER | De DTU max per database mag niet meer dan (%d) voor de opgegeven service ' %. * ls'. | Max DTU per database. elastische pool servicelaag. | Bij het instellen van de DTU max per database de ondersteunde limiet overschrijdt. | Overweeg het gebruik van de elastische pool service-laag die de gewenste instelling ondersteunt. |
| 40870 | EX_USER | De DTU min per database mag niet meer dan (%d) voor de opgegeven service ' %. * ls'. | DTU min per database. elastische pool servicelaag. | Bij het instellen van de DTU min per database de ondersteunde limiet overschrijdt. | Overweeg het gebruik van de elastische pool service-laag die de gewenste instelling ondersteunt. |
| 40873 | EX_USER | Het aantal databases (%d) en DTU min per database (%d) mag niet meer dan de DTUs van de elastische pool (%d). | Aantal databases in elastische pool; DTU min per database. DTUs van elastische van toepassingen. | DTU min voor databases opgeven in de elastische toepassingen die groter is dan de DTUs van de elastische toepassingen probeert. | Overweeg te verhogen van de DTUs van de elastische toepassingen of de DTU min per database verkleinen of verminder het aantal databases in de groep met elastische. |
| 40877 | EX_USER | Een elastische toepassingen kan niet worden verwijderd tenzij niet alle databases bevat. | Geen | De elastische groep een of meer databases bevat en kan niet worden verwijderd. | Verwijder de databases van de elastische toepassingen te verwijderen. |
| 40881 | EX_USER | De elastische pool ' %. * ls de database limiet heeft bereikt.  De limiet voor het aantal databases voor de elastische toepassingen niet meer dan (%d) voor een elastische toepassingen met DTUs (%d). | Naam van elastische toepassingen; database-limiet van elastische pool; e DTUs voor de resourcegroep. | Probeert te maken of de database toevoegen aan de groep met elastische wanneer de limiet van de database van de elastische groep is bereikt. | Geef op rekening houden met de DTUs van de elastische van toepassingen indien mogelijk om de database limiet verhogen of databases verwijderen uit de groep met elastische. |
| 40889 | EX_USER | De DTUs of de opslaglimiet voor de elastische pool ' %. * ls kunnen niet worden verminderd, omdat die niet voldoende opslagruimte voor de databases wilt opgeven. | Naam van elastische toepassingen. | Probeert de opslaglimiet van de elastische toepassingen onder gebruik van de opslag te verlagen. | Beperk het gebruik van opslag van afzonderlijke databases in de groep met elastische of databases verwijderen uit de groep ter vermindering van de DTUs of de maximale opslagruimte. |
| 40891 | EX_USER | De DTU min per database (%d) mag niet meer dan de maximale DTU per database (%d). | DTU min per database. DTU max per database. | Bij het instellen van de DTU min per database hoger is dan de maximale DTU per database. | Zorg ervoor dat de DTU min per databases niet groter zijn dan de maximale DTU per database. |
| NOG TE BEPALEN | EX_USER | De maximale grootte voor een afzonderlijke database in een groep met elastische niet groter zijn dan de maximale grootte die is toegestaan door ' %. * ls' service elastische laag van toepassingen. | elastische pool service tier | De maximale grootte voor de database groter is dan de maximale grootte die is toegestaan door de elastische pool servicelaag. | Stel de maximale grootte van de database binnen de grenzen van de maximale grootte die is toegestaan door de elastische pool servicelaag. |

Verwante onderwerpen:

* [Maak een elastische database-toepassingen (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Een elastische database-toepassingen (C#) beheren](sql-database-elastic-pool-manage-csharp.md). 
* [Maak een elastische database-toepassingen (PowerShell)](sql-database-elastic-pool-create-powershell.md) 
* [Controleren en beheren van een elastische database-toepassingen (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Algemene fouten

De volgende fouten vallen niet in alle voorgaande categorieën.

|Foutcode|Ernst|Beschrijving|
|---:|---:|:---|
|15006|16|<AdministratorLogin>is geen geldige naam omdat deze ongeldige tekens bevat.|
|18452|14|Aanmelden is mislukt. De aanmelding is van een niet-vertrouwd domein en kan niet worden gebruikt met Windows authentication.%. & #x2a; ls (Windows-aanmeldingen worden niet ondersteund in deze versie van SQL Server.)|
|18456|14|Aanmelding mislukt voor gebruiker ' % #x2a;ls'.%. & #x2a % ls. & #x2a; ls (de aanmelding mislukt voor gebruiker '% & #x2a; ls'. Het wachtwoord is niet gewijzigd. Wachtwoord wijzigen tijdens het aanmelden wordt niet ondersteund in deze versie van SQL Server.)|
|18470|14|Aanmelding mislukt voor gebruiker '% & #x2a; ls'. Reden: De rekening is disabled.%. & #x2a; ls|
|40014|16|In dezelfde transactie kunnen meerdere databases worden gebruikt.|
|40054|16|Tabellen zonder een geclusterde index worden niet ondersteund in deze versie van SQL Server. Een gegroepeerde index maken en probeer het opnieuw.|
|40133|15|Deze bewerking wordt niet ondersteund in deze versie van SQL Server.|
|40506|16|Opgegeven SID is ongeldig voor deze versie van SQL Server.|
|40507|16|' % & #x2a; ls kunnen niet worden aangeroepen met de parameters die in deze versie van SQL Server.|
|40508|16|USE-instructie wordt niet ondersteund als u wilt schakelen tussen databases. Een nieuwe verbinding om verbinding met een andere database te gebruiken.|
|40510|16|Instructie '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server|
|40511|16|Ingebouwde functie '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40512|16|Afgeschaft '%ls' wordt niet ondersteund in deze versie van SQL Server.|
|40513|16|Server variabele '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40514|16|'%ls' wordt niet ondersteund in deze versie van SQL Server.|
|40515|16|Verwijzing naar de database-en/of de naam in '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40516|16|Globale tijdelijke objecten worden niet ondersteund in deze versie van SQL Server.|
|40517|16|Trefwoord of instructie optie '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40518|16|DBCC-opdracht '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40520|16|Beveiligbare klasse '% S_MSG' wordt niet ondersteund in deze versie van SQL Server.|
|40521|16|Beveiligbare klasse '% S_MSG' wordt niet ondersteund in de serverscope in deze versie van SQL Server.|
|40522|16|De belangrijkste '% & #x2a; ls' databasetype wordt niet ondersteund in deze versie van SQL Server.|
|40523|16|Maken van een impliciete gebruikersaccount '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server. De gebruiker expliciet maken voordat u deze gebruikt.|
|40524|16|Gegevenstype '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40525|16|MET '%.ls' wordt niet ondersteund in deze versie van SQL Server.|
|40526|16|' % & #x2a; ls rijenset provider niet wordt ondersteund in deze versie van SQL Server.|
|40527|16|Gekoppelde servers worden niet ondersteund in deze versie van SQL Server.|
|40528|16|Gebruikers kunnen niet worden toegewezen aan certificaten of asymmetrische sleutels Windows-aanmeldingen in deze versie van SQL Server.|
|40529|16|Ingebouwde functie '% & #x2a; ls' imitatie context wordt niet ondersteund in deze versie van SQL Server.|
|40532|11|Server kan niet worden geopend '% & #x2a; ls' aangevraagd door de aanmelding. De aanmelding is mislukt.|
|40553|16|De sessie is beëindigd vanwege overmatig geheugengebruik. Probeer de query voor het verwerken van minder rijen te wijzigen.<br/><br/>*Opmerking:* Vermindering van het aantal `ORDER BY` en `GROUP BY` operations in Transact-SQL-code vermindert de geheugenvereisten van de query.|
|40604|16|Kan geen CREATE/ALTER DATABASE omdat deze meer dan het quotum van de server.|
|40606|16|Databases koppelen wordt niet ondersteund in deze versie van SQL Server.|
|40607|16|Windows-aanmeldingen worden niet ondersteund in deze versie van SQL Server.|
|40611|16|Servers kunnen maximaal 128 firewall-regels gedefinieerd hebben.|
|40614|16|Begin-IP-adres van de firewallregel niet groter zijn dan het laatste IP-adres.|
|40615|16|Server '{0}' aangevraagd door de aanmelding kan niet worden geopend. De client met het IP-adres '{1}' is niet toegestaan voor toegang tot de server.  Als toegang wilt inschakelen, gebruikt u de SQL-Database Portal of sp_set_firewall_rule uitgevoerd op de master-database voor het maken van een firewallregel voor dit IP-adres of adresbereik.  Duurt het vijf minuten duren voordat deze wijziging door te voeren.|
|40617|16|De naam van de firewall-regel die met begint <rule name> is te lang. Maximale lengte is 128.|
|40618|16|Naam van de firewallregel mag niet leeg zijn.|
|40620|16|De aanmelding mislukt voor gebruiker '% & #x2a; ls'. Het wachtwoord is niet gewijzigd. Wachtwoord wijzigen tijdens het aanmelden wordt niet ondersteund in deze versie van SQL Server.|
|40627|20|Bewerking op server {0} en {1}-database wordt uitgevoerd. Wacht enkele minuten en probeer het opnieuw.|
|40630|16|Wachtwoordvalidatie is mislukt. Het wachtwoord voldoet niet aan de vereisten van omdat het te kort is.|
|40631|16|Het wachtwoord dat u hebt opgegeven is te lang. Het wachtwoord mag niet meer dan 128 tekens hebben.|
|40632|16|Wachtwoordvalidatie is mislukt. Het wachtwoord voldoet niet aan de vereisten van omdat het niet complex genoeg.|
|40636|16|Een gereserveerde naam niet gebruiken '% & #x2a; ls' in deze bewerking.|
|40638|16|Ongeldige abonnement-id < abonnement-id >. Abonnement bestaat niet.|
|40639|16|Aanvraag voldoet niet aan het schema: <schema error>.|
|40640|20|De server heeft een onverwachte uitzondering aangetroffen.|
|40641|16|De opgegeven locatie is ongeldig.|
|40642|17|De server is momenteel bezet. Probeer het later opnieuw.|
|40643|16|Waarde van de opgegeven x-ms-versie header is ongeldig.|
|40644|14|Kan de toegang tot het opgegeven abonnement.|
|40645|16|Servernaam <servername> kan niet leeg of null. Dit kan alleen bestaan uit kleine letters 'a'-'z', de cijfers 0-9 en het afbreekstreepje. Het afbreekstreepje mag niet leiden of vastgelegd in de naam.|
|40646|16|Abonnement-ID mag niet leeg zijn.|
|40647|16|Abonnement < abonnement-id is geen server servernaam.|
|40648|17|Te veel aanvragen zijn uitgevoerd. Probeer later nogmaals.|
|40649|16|Ongeldige inhoudstype is opgegeven. Alleen toepassings-xml wordt ondersteund.|
|40650|16|< Abonnement-id > abonnement bestaat niet of is niet gereed voor de bewerking.|
|40651|16|Kan geen server maken omdat het abonnement < abonnement-id > is uitgeschakeld.|
|40652|16|Kan niet verplaatsen of server maken. Abonnement < abonnement-id > overschrijdt server-quotum.|
|40671|17|Storingen in de communicatie tussen de gateway en de management-service. Probeer later nogmaals.|
|40852|16|Kan database niet openen ' %. *ls op server "%.* ls aangevraagd door de aanmelding. Toegang tot de database is alleen toegestaan met een verbindingsreeks met ingeschakelde beveiliging. Voor toegang tot deze database, wijzigt u de tekenreeksen bevatten veilig in de FQDN - naam van server.database.windows-server .net moet worden gewijzigd om de naam van server-.database. `secure`. windows.net.|
|45168|16|Het systeem met SQL Azure wordt belast en het plaatsen van een bovengrens voor gelijktijdige DB CRUD bewerkingen voor één server (bijvoorbeeld, database maken). De server die is opgegeven in het foutbericht heeft het maximum aantal gelijktijdige verbindingen overschreden. Probeer het later opnieuw.|
|45169|16|Het systeem azure SQL wordt belast en het plaatsen van een bovengrens voor het aantal gelijktijdige serverbewerkingen CRUD voor een abonnement op één (bijvoorbeeld server maakt). Het abonnement is opgegeven in het foutbericht overschrijdt het maximum aantal gelijktijdige verbindingen en de aanvraag is geweigerd. Probeer het later opnieuw.|

## <a name="related-links"></a>Verwante koppelingen

- [Richtlijnen en beperkingen voor Azure SQL Database-algemeen](sql-database-general-limitations.md)
- [Azure SQL-Database resource limieten](sql-database-resource-limits.md)
