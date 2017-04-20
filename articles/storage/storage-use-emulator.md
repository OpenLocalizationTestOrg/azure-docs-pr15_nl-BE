<properties 
    pageTitle="De Emulator Azure opslag gebruikt voor het ontwikkelen en testen van | Microsoft Azure" 
    description="De emulator Azure opslag biedt een gratis plaatselijke ontwikkeling-omgeving voor het ontwikkelen en testen tegen Azure opslag. Meer informatie over de opslag-emulator, waaronder hoe aanvragen worden geverifieerd, verbinding maken met de emulator van de toepassing en het gebruik van het opdrachtregelprogramma." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>De Emulator Azure opslag gebruikt voor het ontwikkelen en testen

## <a name="overview"></a>Overzicht

De emulator Microsoft Azure opslag biedt een lokale omgeving waarin de Azure Blob, wachtrij en tabel diensten voor ontwikkelingsdoeleinden worden geëmuleerd. De emulator opslag kunt u uw toepassing ten opzichte van de storage-services lokaal testen zonder een Azure-abonnement maken of het aangaan van de kosten. Wanneer u tevreden bent over hoe uw toepassing in de emulator werkt, kunt u overschakelen naar met een account Azure opslag in de cloud.

> [AZURE.NOTE] De opslag-emulator is beschikbaar als onderdeel van de [SDK van Microsoft Azure](https://azure.microsoft.com/downloads/). U kunt ook installeren met het [zelfstandige installatieprogramma voor](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409)de opslag-emulator. De emulator opslag configureren, hebt u beheerdersrechten nodig op de computer.
> 
> De emulator opslag op dit moment wordt alleen uitgevoerd op Windows.
>  
> Houd er rekening mee dat gegevens die zijn gemaakt in een versie van de opslag-emulator is niet gegarandeerd toegankelijk zijn wanneer u een andere versie. Als u nodig hebt om uw gegevens voor de lange termijn, het raadzaam dat u die gegevens opslaan in een account Azure opslag in plaats van in de emulator opslag.

## <a name="how-the-storage-emulator-works"></a>De werking van de emulator opslag
 
De emulator opslag maakt gebruik van een lokaal exemplaar van Microsoft SQL Server en het lokale bestandssysteem emuleren de Azure storage-services. De opslag-emulator standaard in een database in Microsoft SQL Server 2012 Express LocalDB.  U kunt de emulator storage voor toegang tot een lokaal exemplaar van SQL Server in plaats van het LocalDB-exemplaar configureren. Zie [Start en initialiseren van de emulator van de opslag](#start-and-initialize-the-storage-emulator) hieronder voor meer informatie.

U kunt SQL Server Management Studio Express voor het beheren van LocalDB. De emulator opslag verbinding met SQL Server- of LocalDB met behulp van Windows-verificatie. 

Sommige verschillen tussen de emulator van de opslag- en opslagservices Azure in functionaliteit. Zie voor meer informatie over deze verschillen, de [verschillen tussen de emulator voor opslag en opslag van Azure](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Verificatie aanvragen tegen de emulator opslag

Net als met Azure opslag in de cloud, moet elke aanvraag die u tegen de emulator opslag worden geverifieerd, tenzij het een anonieme aanvraag. U kunt verifiëren aanvragen tegen de emulator van de opslag met de verificatie Shared Key of met een gedeelde access-handtekening (SAS).

### <a name="authentication-with-shared-key-credentials"></a>Verificatie met een gedeelde sleutel referenties

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Zie voor meer informatie over verbindingsreeksen [Azure opslag verbindingsreeksen configureren](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Verificatie met een gedeelde access-handtekening 

Sommige clientbibliotheken Azure opslag, zoals de bibliotheek Xamarin alleen ondersteuning voor verificatie met een gedeelde toegangstoken handtekening (SAS). U moet deze met behulp van een hulpprogramma of toepassing die Shared Key-verificatie ondersteunt SAS-token te maken. Er is een eenvoudige manier om de SAS-token te genereren via Azure PowerShell:

1. Azure PowerShell installeren als u dat nog niet gedaan hebt. Gebruik de nieuwste versie van de Azure PowerShell-cmdlets wordt aanbevolen. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md#Install) voor installatie-instructies.

2. Azure PowerShell openen en voer de volgende opdrachten. Vervang de *accountnaam* en *ACCOUNT_KEY ==* met uw eigen referenties. *CONTAINER_NAME* vervangen door een zelfgekozen naam.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

De resulterende gedeelde toegang handtekening URI voor de nieuwe container moet met de volgende strekking:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

De gedeelde toegang handtekening gemaakt met dit voorbeeld is geldig voor één dag. De handtekening krijgen volledige toegang (lezen, schrijven, verwijderen of lijst) tot BLOB's in de container.

Zie voor meer informatie over handtekeningen voor gedeelde toegang, [Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Start en de opslag-emulator wordt geïnitialiseerd

Als de emulator Azure opslag, klik op de knop Start of de Windows-toets. Begin met typen **Azure opslag Emulator**en de emulator selecteren uit de lijst met toepassingen. 

Als de emulator wordt uitgevoerd, ziet u een pictogram in het systeemvak van Windows.

Wanneer de opslag-emulator wordt gestart, verschijnt een opdrachtregelvenster. U kunt dit venster opdrachtregel kunt starten en stoppen van de emulator opslag, alsmede gegevens wissen, krijgen de huidige status en de emulator wordt geïnitialiseerd. Zie [Opslag Emulator Command-Line Tool Reference](#storage-emulator-command-line-tool-reference)voor meer informatie.

Als het venster opdrachtregel is gesloten, blijft de emulator opslag. Om de regel met de opdracht opnieuw, volg de bovenstaande stappen als de emulator storage wordt gestart.

Het uitvoeren van de emulator opslag eerst wordt de omgeving van lokale opslag geïnitialiseerd voor u. Het initialisatieproces een database gemaakt in de LocalDB en HTTP-poorten voor elke lokale opslagservice reserves. 

De emulator opslag wordt standaard naar de map C:\Program Files (x86) \Microsoft SDKs\Azure\Storage-Emulator geïnstalleerd. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>De emulator opslag voor het gebruik van een andere SQL-database initialiseren

Het emulator-opdrachtregelprogramma opslag kunt u de emulator opslag om te verwijzen naar een SQL-database-instantie dan het standaardexemplaar LocalDB initialiseren. Houd er rekening mee dat moet worden uitgevoerd het opdrachtregelprogramma met beheerdersrechten voor de back-end-database initialiseren voor de opslag-emulator:

1. Klik op de knop **Start** of de **Windows** -toets. Typ `Azure Storage Emulator` en te selecteren als dit wordt weergegeven om het emulator-opdrachtregelprogramma opslag.
2. Typ in het opdrachtpromptvenster de volgende opdracht, waarbij `<SQLServerInstance>` is de naam van de SQL Server-exemplaar. U kunt gebruiken LocalDb `(localdb)\v11.0` als de SQL Server-exemplaar.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Ook kunt u de volgende opdracht, die ervoor zorgt de emulator dat gebruiken het standaardexemplaar van SQL Server:

        AzureStorageEmulator init /server .\\ 

    Of kunt u de volgende opdracht, die opnieuw de database met het standaardexemplaar van LocalDB initialiseert:

        AzureStorageEmulator init /forceCreate 

Zie [Opslag Emulator Command-Line Tool Reference](#storage-emulator-command-line-tool-reference)voor meer informatie over deze opdrachten.

## <a name="addressing-resources-in-the-storage-emulator"></a>Bronnen in de emulator opslag adressering

De eindpunten voor de emulator opslag zijn verschilt van een account Azure opslag. Het verschil is vanwege het feit dat de lokale computer geen domeinnamen omzetten, voert zodat de eindpunten van de emulator opslag een lokaal adres in plaats van een domeinnaam vereist.

Wanneer u een bron in een account Azure opslag adres, u het volgende schema, waarbij de accountnaam deel uitmaakt van de URI-hostnaam en de bron aangepakt deel van het pad van de URI:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

De volgende URI is bijvoorbeeld een geldig adres voor een blob in een opslag Azure-account:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

In de emulator opslag omdat de lokale computer geen Domeinnaamomzetting voert, uitmaakt naam van de account deel van de URI-pad in plaats van de host-naam. U gebruikt het volgende schema voor een resource is uitgevoerd in de emulator opslag:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Het volgende adres kan bijvoorbeeld worden gebruikt voor toegang tot een blob in de opslag-emulator:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

De eindpunten voor de emulator van de opslag zijn:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>De account met RA GRS secundaire adressering

Vanaf versie 3.1 ondersteunt de emulator opslag account leestoegang geo-redundante replicatie (RA-GRS). Opslagbronnen in de cloud en in de lokale emulator, u kunt toegang tot de secundaire locatie door toe te voegen - ondergeschikt aan de accountnaam. Het volgende adres kan bijvoorbeeld worden gebruikt voor toegang tot een blob met de secundaire alleen-lezen in de opslag-emulator:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Voor programmatische toegang tot de secundaire met de emulator opslag, gebruikt de clientbibliotheek opslag voor .NET versie 3.2 of hoger. Zie de [Microsoft Azure opslag Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) voor meer informatie.

## <a name="storage-emulator-command-line-tool-reference"></a>Opslag-emulator opdrachtregelprogramma verwijzing

In versie 3.0, wordt gestart wanneer u de Emulator opslag starten, ziet u een opdrachtregelvenster pop up. Gebruik het opdrachtregelprogramma venster starten en stoppen van de emulator ook opvragen van status en andere bewerkingen uitvoeren.

> [AZURE.NOTE] Als u de Microsoft Azure berekenen emulator geïnstalleerd hebt, verschijnt een pictogram op de taakbalk wanneer u de Emulator opslag start. Klik met de rechtermuisknop op het pictogram om een menu waarmee een grafische manier om te starten en stoppen van de Emulator opslag zichtbaar te maken.

### <a name="command-line-syntax"></a>Opdrachtregelsyntaxis

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Opties

De lijst van opties wilt weergeven, typt u `/help` bij de opdrachtprompt.

| Optie | Beschrijving                                                    | Opdracht                                                                                                 | Argumenten                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Start**  | De emulator storage wordt gestart.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: Start de emulator in het huidige proces in plaats van een nieuw proces te maken.                          |
| **Stop**   | Hiermee stopt u de emulator opslag.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Status** | De status van de emulator opslag afgedrukt.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Wissen**  | Hiermee wist u de gegevens in alle services die zijn opgegeven op de opdrachtregel. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *BLOB*: blob-gegevens wissen. <br/>*wachtrij*: wachtrijgegevens worden gewist. <br/>*tabel*: tabelgegevens worden gewist. <br/>*alle*: Hiermee wist u alle gegevens in alle services. |
| **Init**   | Eenmalige initialisatie voor het instellen van de emulator wordt uitgevoerd.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-server Servernaam\exemplaarnaam*: de server die als host fungeert voor de SQL-exemplaar. <br/>*instanceName - sqlinstance*: de naam van het SQL-exemplaar moet worden gebruikt in de standaard server-exemplaar. <br/>*-forcecreate*: zorgt ervoor dat het maken van de SQL-database, zelfs als deze al bestaat. <br/>*-inprocess*: initialisatie uitvoert in het huidige proces in plaats van een nieuw proces kuitschieten. U moet het huidige proces met verhoogde bevoegdheden om initialisatie starten.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Verschillen tussen de emulator voor opslag en opslag van Azure

Aangezien de emulator opslag een geëmuleerde omgeving in een lokale SQL-exemplaar wordt uitgevoerd, zijn er enkele verschillen in functionaliteit tussen de emulator en een account Azure opslag in de cloud:

- De emulator opslag ondersteunt slechts één vaste-account en een bekende verificatiesleutel.

- De opslag-emulator is geen schaalbare opslagservice en biedt geen ondersteuning voor een groot aantal clients tegelijkertijd.

- Zoals beschreven in [bronnen in de emulator opslag adressering](#addressing-resources-in-the-storage-emulator), worden bronnen anders in de emulator opslag versus een Azure opslag account behandeld. Dit verschil is vanwege het feit dat domeinnamen omzetten beschikbaar is in de cloud, maar niet op de lokale computer.

- Vanaf versie 3.1 ondersteunt de emulator opslag account leestoegang geo-redundante replicatie (RA-GRS). In de emulator, alle accounts hebben RA-GRS is ingeschakeld en er is nooit een vertraging tussen de primaire en secundaire replica's. De Blob Service statistieken opvragen en ophalen Queue Service statistieken tabelstatistieken Service krijgen bewerkingen worden ondersteund op de secundaire account en resulteert altijd in de waarde van het `LastSyncTime` reactie-element als de huidige tijd op basis van de onderliggende SQL-database.

    Voor programmatische toegang tot de secundaire met de emulator opslag, gebruikt de clientbibliotheek opslag voor .NET versie 3.2 of hoger. Zie de [Microsoft Azure opslag Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) voor meer informatie.

- De File service en eindpunten SMB-protocol worden momenteel niet ondersteund in de emulator opslag.

- De emulator opslag geeft een VersionNotSupportedByEmulator-fout (HTTP-statuscode 400 - Ongeldig verzoek) als u een versie van de storage-services die nog niet wordt ondersteund door de versie van de emulator die u gebruikt.

### <a name="differences-for-blob-storage"></a>Verschillen voor Blob-opslag 

De volgende verschillen zijn van toepassing op de Blob-opslag in de emulator:

- Blob voor opslag emulator ondersteunt alleen de grootte van maximaal 2 GB.

- Een Blob Put-bewerking mogelijk tegen een blob die bestaat in de opslag-emulator en heeft een actieve lease mislukt, zelfs als de lease-ID niet als onderdeel van de aanvraag opgegeven is. 

- Blob-bewerkingen worden niet ondersteund door de emulator toevoegen. U probeert een bewerking op een blob append geeft een FeatureNotSupportedByEmulator-fout (HTTP-statuscode 400 - Ongeldig verzoek).

### <a name="differences-for-table-storage"></a>Verschillen voor opslag van tabel 

De volgende verschillen zijn van toepassing op de opslag van de tabel in de emulator:

- Datumeigenschappen in de tabel service in de emulator opslag ondersteunen alleen ondersteund door SQL Server 2005 (*dat wil zeggen*, ze moeten uiterlijk 1 januari 1753). Alle datums van vóór 1 januari 1753 worden tot deze waarde gewijzigd. De precisie van datums is beperkt tot de precisie van SQL Server 2005, wat betekent dat de datums nauwkeurig tot 1 zijn/300th van een seconde.

- De emulator opslag ondersteunt partitie-toets en de rij belangrijke waarden van minder dan 512 bytes. Ook de totale grootte van de accountnaam, de tabelnaam en de sleuteleigenschap namen samen niet meer dan 900 bytes.

- De totale grootte van een rij in een tabel in de opslag-emulator is beperkt tot minder dan 1 MB.

- Typ in de emulator opslag eigenschappen van data `Edm.Guid` of `Edm.Binary` ondersteunt alleen de `Equal (eq)` en `NotEqual (ne)` vergelijkingsoperatoren in query strings filteren.

### <a name="differences-for-queue-storage"></a>Verschillen voor Queue storage

Er zijn geen verschillen die specifiek zijn voor de opslag van de wachtrij in de emulator.

## <a name="storage-emulator-release-notes"></a>Release-opmerkingen bij opslag-emulator

### <a name="version-45"></a>Versie 4.5

- Vast een bug waardoor de initialisatie en de installatie van de emulator opslag mislukt wanneer de back-database is gewijzigd.

### <a name="version-44"></a>Versie 4.4

- De emulator opslag ondersteunt nu versie 2015-12-11 van de storage-services op de eindpunten Blob, wachtrij en tabel.

- Opslag-emulator garbage collector van blob-gegevens is nu efficiënter omgaan met een groot aantal BLOB's.

- Vast een bug die container ACL XML veroorzaakt uit hoe de storage-service het werkt iets anders worden gevalideerd.

- Vast een bug die soms veroorzaakt max en min DateTime-waarden moeten worden gemeld in de verkeerde tijdzone.

### <a name="version-43"></a>Versie 4.3

- De emulator opslag ondersteunt nu 2015-07-08 versie van de storage-services op de eindpunten Blob, wachtrij en tabel.

### <a name="version-42"></a>Versie 4.2

- De emulator opslag ondersteunt nu versie 2015-04-05 van de storage-services op de eindpunten Blob, wachtrij en tabel.

### <a name="version-41"></a>Versie 4.1

- De emulator opslag ondersteunt nu versie 21-02-2015 van de storage-services op service-endpoints Blob, wachtrij en tabel, met uitzondering van de nieuwe functies in de Blob toevoegen. 

- De emulator opslag wordt nu een zinvolle foutbericht als u een versie van de storage-services die nog niet wordt ondersteund door deze versie van de emulator gebruikt. Het is raadzaam de meest recente versie van de emulator. Als u een VersionNotSupportedByEmulator-fout (HTTP-statuscode 400 - Ongeldig verzoek), download de nieuwste versie van de opslag-emulator.

- Een bug opgelost waarbij een entiteit race veroorzaakt tabelgegevens onjuist worden tijdens het van gelijktijdige samenvoegbewerkingen.

### <a name="version-40"></a>Versie 4.0

- De opslag van uitvoerbare emulator is gewijzigd in *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Versie 3.2

- De emulator opslag ondersteunt nu versie 2014-02-14 van de storage-services op de eindpunten Blob, wachtrij en tabel. Houd er rekening mee dat bestand eindpunten worden momenteel niet ondersteund in de emulator opslag. Zie [versiebeheer voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) voor meer informatie over versie 2014-02-14.

### <a name="version-31"></a>Versie 3.1

- Geo-redundante opslag lezen-toegang (RA-GRS) wordt nu ondersteund in de emulator opslag. De Blob Service statistieken opvragen, wachtrij Service statistieken opvragen en ophalen tabel Stats API's worden ondersteund voor de secundaire account en retourneert altijd de waarde van het element LastSyncTime antwoord als de huidige tijd op basis van de onderliggende SQL-database. Voor programmatische toegang tot de secundaire met de emulator opslag, gebruikt de clientbibliotheek opslag voor .NET versie 3.2 of hoger. Zie Microsoft Azure opslag Client Library voor .NET Reference voor meer informatie.

### <a name="version-30"></a>Versie 3.0

- De emulator Azure opslag wordt niet meer geleverd in hetzelfde pakket als de compute-emulator.

- De grafische gebruikersinterface van de opslag-emulator is vervangen door een scriptable opdrachtregelinterface. Zie opslag Emulator Command-Line Tool Reference voor meer informatie op de command line interface. De grafische interface zal blijven aanwezig in versie 3.0, maar deze zijn alleen toegankelijk wanneer de Emulator berekenen is geïnstalleerd door met de rechtermuisknop op het pictogram op de taakbalk en opslag Emulator gebruikersinterface weergeven te selecteren.

- Versie 2013-08-15 van de van Azure opslagservices wordt nu volledig ondersteund. (Deze versie werd voorheen alleen ondersteund door opslag Emulator versie 2.2.1 voorbeeld.)
