<properties
    pageTitle="REST gebruiken om een back-up en herstel App Service apps"
    description="Informatie over het gebruik van RESTful API-aanroepen om een back-up maken en terugzetten van een app in Azure App-Service"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>REST gebruiken om een back-up en herstel App Service apps

> [AZURE.SELECTOR]
- [PowerShell](../app-service/app-service-powershell-backup.md)
- [REST-API](websites-csm-backup.md)

[App Service apps](https://azure.microsoft.com/services/app-service/web/) kunnen worden gestaafd als BLOB's in Azure opslag. De back-up kan ook de app van databases bevatten. Als de app ooit per ongeluk is verwijderd of als de app worden teruggezet naar een vorige versie moet, kan dit worden hersteld van een vorige back-up. Back-ups kunnen worden uitgevoerd op elk gewenst moment op de vraag of de back-ups kunnen worden gepland met passende tussenpozen.

In dit artikel wordt uitgelegd hoe u een back-up maken en terugzetten van een app met RESTful API aanvragen. Als u wilt maken en beheren van back-ups van app grafisch via de portal Azure, Zie [Back-up van een web app in Azure App-Service.](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Aan de slag
REST-aanvragen worden verzonden, moet u weten van uw app **naam**, **resourcegroep**en **abonnements-id**. Deze informatie kunt u vinden door te klikken op uw app in de **App Service** blade van de [Azure portal](https://portal.azure.com). Wij zijn de website **backuprestoreapiexamples.azurewebsites.net**configureren voor de voorbeelden in dit artikel. Het wordt opgeslagen in de resourcegroep van de standaard-Web-WestUS en een abonnement met de ID 00001111-2222-3333-4444-555566667777 wordt uitgevoerd.

![Informatie over monster websites][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Back-up en terugzetten REST API
Nu behandelen we enkele voorbeelden van hoe een back-up en terugzetten van een app met de REST API. Elk voorbeeld bevat een hoofdgedeelte van de URL en HTTP-aanvraag. De voorbeeld-URL bevat tijdelijke aanduidingen die worden omsloten door accolades, zoals {abonnement-id}. De tijdelijke aanduidingen vervangen door de overeenkomstige gegevens van uw app. Ter referentie is hier een uitleg van elke tijdelijke aanduiding die wordt weergegeven in de voorbeeld-URL's.

* abonnement-id-ID van het Azure abonnement met de app.
* resource-groep-naam: de naam van de bronnengroep met de app.
* naam: de naam van de app.
* back-up-id: de ID van de app back-up

Zie voor de volledige documentatie van de API, met inbegrip van diverse optionele parameters die kunnen worden opgenomen in de HTTP-aanvraag, de [Azure Resource Explorer](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Back-up maken van een app op aanvraag
Verzenden naar een **POST** -aanvraag om een back-up van een app direct, **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Hier ziet u hoe de URL eruitziet met behulp van onze website voorbeeld. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/Backup/**

Een JSON-object in de hoofdtekst van uw verzoek om op te geven welke opslag-account te gebruiken voor het opslaan van de back-up op te geven. De JSON-object moet een eigenschap met de naam **storageAccountUrl**, waarin een [URL SAS](../storage/storage-dotnet-shared-access-signature-part-1.md) schrijven toegang verlenen tot de Azure opslag container met de back-blob. Als u back-up van de databases wilt, moet u ook een lijst met de namen, typen en verbindingsreeksen van de databases worden back-up moet opgeven.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Een back-up van de app begint zodra de aanvraag is ontvangen. Het back-upproces kan lang duren om te voltooien. Het HTTP-antwoord bevat een ID die u kunt gebruiken in een andere aanvraag voor de status van de back-up. Hier volgt een voorbeeld van het hoofdgedeelte van de HTTP-antwoord op onze back-aanvraag.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Foutberichten vindt u in de eigenschap voor het registreren van het HTTP-antwoord.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Automatische back-ups plannen
Naast back-ups van een app op verzoek, kunt u ook een back-up niet automatisch reserveren.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Een nieuwe automatische back-upschema instellen
Als u een back-upschema instelt, verzendt een aanvraag **plaatsen** om te **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Hier is wat de URL voor de website van ons voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/Backup**

Het hoofdgedeelte van de aanvraag moet een JSON-object dat de back-up. Hier volgt een voorbeeld met de vereiste parameters.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

In dit voorbeeld wordt de app om automatisch een reservekopie maken van elke zeven dagen geconfigureerd. De parameters **frequencyInterval** en **frequencyUnit** samen bepalen hoe vaak de back-ups gebeuren. Geldige waarden voor **frequencyUnit** zijn **uur** en **dag**. Stel bijvoorbeeld frequencyInterval wilt maken van een app elke 12 uur, 12 en frequencyUnit uur.

Oude back-ups automatisch verwijderd uit de opslag-account. U kunt bepalen hoe oud de back-ups kunnen worden door de parameter **retentionPeriodInDays** . Als u wilt dat deze altijd ten minste één back-up opgeslagen, ongeacht hoe oud is, het **keepAtLeastOneBackup** ingesteld op true.

### <a name="get-the-automatic-backup-schedule"></a>De automatische back-upschema ophalen
Als u de reservekopie van de configuratie van de Apps, door een **POST** -aanvraag te verzenden naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

De URL voor de site van ons voorbeeld is **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>De status van een back-up
Een back-up kan een tijdje duren afhankelijk van hoe groot de app is. Back-ups kunnen ook mislukken, time-out, of gedeeltelijk mislukt. Overzicht van de status van de back-ups van een app een **GET** -verzoek naar de URL te verzenden **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Overzicht van de status van een bepaalde back-up een GET-aanvraag te verzenden naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Hier is wat de URL voor de website van ons voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**

De hoofdtekst van het antwoord bevat een vergelijkbaar met het volgende voorbeeld JSON-object.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

De status van een back-up wordt een opgesomd type. Hier wordt elke mogelijke status.

* 0 – InProgress: de back-up is gestart maar nog niet voltooid.
* 1 – niet: De back-up is mislukt.
* 2 – geslaagd: De back-up is voltooid.
* 3 – time-out: De back-up is niet voltooid in de tijd en is geannuleerd.
* 4 – gemaakt: De back-aanvraag in de wachtrij is geplaatst, maar is niet gestart.
* 5 – overgeslagen: De back-up niet worden voortgezet door een schema te veel back-ups te activeren.
* 6 – PartiallySucceeded: De back-up is voltooid, maar sommige bestanden zijn geen back-up gemaakt omdat ze niet worden gelezen. Dit gebeurt meestal omdat een exclusieve vergrendeling is geplaatst op de bestanden.
* 7: DeleteInProgress: De back-up is aangevraagd moet worden verwijderd, maar nog niet zijn verwijderd.
* 8 – DeleteFailed: De back-up kan niet worden verwijderd. Dit kan gebeuren omdat de SAS-URL die is gebruikt voor het maken van de back-up is verlopen.
* 9 – verwijderd: De back-up is verwijderd.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Een toepassing terugzetten vanaf een back-up
Als uw app is verwijderd of als u terug wilt naar uw app naar een eerdere versie, kunt u de app terugzetten vanaf een back-up. Verzenden om aan te roepen een terugzetbewerking, een **POST** -aanvraag naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Hier is wat de URL voor de website van ons voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/Restore**

In het hoofdgedeelte van de aanvraag verzenden een JSON-object dat de eigenschappen voor de bewerking voor terugzetten. Hier volgt een voorbeeld met alle vereiste eigenschappen:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Terugzetten naar een nieuwe app
Soms wilt u misschien een nieuwe toepassing maken bij het terugzetten van een back-up, in plaats van een al bestaande app te overschrijven. Hiertoe wijzigt u de aanvraag-URL naar de nieuwe toepassing die u wilt maken en wijzigen van de eigenschap **overschrijven** in de JSON op **false**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Een app back-up verwijderen
Als u een back-up te verwijderen wilt, moet u een aanvraag **verwijderen** verzenden naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Hier is wat de URL voor de website van ons voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>SAS-URL voor een back-up beheren
Azure App-Service probeert te verwijderen van uw back-up van Azure opslag met SAS-URL die is opgegeven bij de back-up is gemaakt. Als deze URL SAS niet langer geldig is, kan de back-up kan niet worden verwijderd via de REST API. U kunt echter de SAS-URL die is gekoppeld aan een back-up door het verzenden van een **POST** -aanvraag naar de URL bijwerken **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Hier is wat de URL voor de website van ons voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/List**

In het hoofdgedeelte van de aanvraag verzenden een JSON-object dat de nieuwe SAS-URL bevat. Hier volgt een voorbeeld.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] De SAS-URL die is gekoppeld aan een back-up is om veiligheidsredenen niet geretourneerd wanneer u een GET-verzoek verzendt voor een specifieke back-up. Als u weergeven van de SAS-URL die is gekoppeld aan een back-up wilt, verzendt u een POST-aanvraag aan dezelfde URL hierboven. Een leeg JSON-object opnemen in het hoofdgedeelte van de aanvraag. De reactie van de server bevat alle back-up van gegevens, met inbegrip van de SAS-URL.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
