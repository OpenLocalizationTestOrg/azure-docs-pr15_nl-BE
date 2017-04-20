<properties
    pageTitle="Sleutel kluis met sleutel rotatie van begin tot eind en controle instellen | Microsoft Azure"
    description="Deze procedures gebruiken om setup met belangrijke draaien en de sleutel kluis logboeken controleren"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Sleutel kluis met sleutel rotatie van begin tot eind en controle instellen

##<a name="introduction"></a>Inleiding

Na het maken van uw Azure sleutel kluis, kun je starten gebruik te maken van deze kluis om uw sleutels en geheimen te slaan. Uw toepassingen niet langer nodig hebt om uw sleutels of geheimen, maar eerder vraagt om ze uit de sleutel kluis indien nodig. Hiermee kunt u voor het bijwerken van sleutels en geheimen zonder invloed op de werking van de toepassing van een groot aantal mogelijkheden om uw sleutel te openen en het gedrag van de geheime management.

In dit artikel vindt u een voorbeeld van het gebruik van Azure sleutel kluis voor het opslaan van een geheim, in dit geval een sleutel Azure opslag Account die door een toepassing wordt geopend. Deze demonstreert implementatie van een geplande draaien van de sleutel rekening opslag ook. Ten slotte doorlopen het die een demonstratie van het controleren van de controlelogboeken sleutel kluis en waarschuwingen te verhogen wanneer onverwachte aanvragen worden gedaan.

> \[AZURE. Opmerking\] deze zelfstudie is niet bedoeld om uit te leggen in detail de eerste set up van uw Azure sleutel kluis. Zie voor deze informatie [aan de slag met Azure sleutel kluis](key-vault-get-started.md). Of Zie voor instructies de opdrachtregelinterface Cross-Platform [in deze zelfstudie gelijkwaardig](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>KeyVault instellen

Om een toepassing op te halen van een geheim van Azure sleutel kluis, moet u eerst het geheim maken en uploaden naar uw kluis. Dit kunt u doen gemakkelijk via PowerShell zoals hieronder wordt weergegeven.

Een Azure PowerShell-sessie start en aanmelden bij uw account Azure met de volgende opdracht:

```powershell
Login-AzureRmAccount
```

Voer uw Azure gebruikersnaam en wachtwoord in het pop-browservenster. Alle abonnementen die zijn gekoppeld aan deze account en standaard, gebruikt de eerste die krijgen Azure PowerShell.

Als u meerdere abonnementen hebt, is het mogelijk om op te geven van een specifieke naam die is gebruikt voor het maken van uw Azure sleutel kluis. Typ het volgende als u wilt zien van de abonnementen voor uw account:

```powershell
Get-AzureRmSubscription
```

Als u het abonnement dat is gekoppeld aan uw sleutel kluis die u meldt zich aan, typ:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Als dit artikel wordt beschreven van een sleutel opslag account als een geheim op te slaan, moet u die account opslag sleutel ophalen.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Na het ophalen van uw geheime, in dit geval uw sleutel opslag account, u moet die converteren naar een veilige en maak vervolgens een geheim met de waarde in de sleutel kluis.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Het volgende dat is het verstandig om de URI voor het geheim dat u zojuist hebt gemaakt. Dit wordt gebruikt in een latere stap wanneer u de sleutel kluis te halen uw geheime aanroept. De volgende PowerShell-opdracht uitvoeren en noteer de waarde 'Id', die de geheime URI is.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Toepassing instellen

Nu dat er een geheim opgeslagen zult u die geheim ophalen en het gebruik van de code. Er zijn een paar stappen die nodig zijn om te bereiken dat dit de eerste en belangrijkste van uw toepassing met Azure Active Directory registreren en vervolgens aan de Azure sleutel kluis uw aanvraaginformatie zodat het aanvragen van uw toepassing.

> \[AZURE. Opmerking\] uw toepassing moet worden gemaakt op de dezelfde huurder Azure Active Directory als de sleutel kluis. 

De eerste keer opent op het tabblad toepassingen van Azure Active Directory

![Geopende toepassingen in Azure AD](./media/keyvault-keyrotation/AzureAD_Header.png)

Kies 'Toevoegen' om een nieuwe toepassing toevoegen aan je advertentie Azure

![Kies toepassing toevoegen](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Laat u het toepassingstype als Web Application en/of WEB API en uw toepassing een naam geven.

![Naam van de toepassing](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Geef de toepassing een 'aanmelding URL' en een 'App ID URI'. Dit kunnen alles wat die u wilt gebruiken voor deze demo, en kunnen later worden gewijzigd indien nodig.

![Vereiste URI's bieden](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Zodra de toepassing wordt toegevoegd aan de AD Azure, wordt u op de pagina toepassing worden binnengebracht. Klik op het tabblad 'Configureren' en zoek vanaf dat punt en kopieer de Client-ID-waarde. Noteer de client-ID voor de latere stappen.

Vervolgens moet u een sleutel voor uw toepassing te kunnen werken met uw Azure advertentie genereren. Kunt u dit onder de sectie 'Sleutels' op het tabblad 'Configuratie'. Noteer de de zojuist gegenereerde sleutel vanuit uw toepassing AD Azure voor gebruik in een latere stap.

![Azure AD App sleutels](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Voordat er oproepen vanuit de toepassing in een kluis met sleutel moet u de sleutel kluis vertellen over de toepassing en de bijbehorende ' machtigingen. De volgende opdracht wordt de naam van de kluis en de client-ID van uw Azure AD app en verleent toegang tot de sleutel kluis 'Ophalen' voor de toepassing.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

U bent nu klaar om te beginnen met het samenstellen van uw toepassing aanroepen. In uw toepassing moet u eerst de NuGet pakketten nodig om te communiceren met Azure sleutel kluis en Azure Active Directory installeren. Voer de volgende opdrachten vanuit de Visual Studio Package Manager-console. Houd er rekening mee dat bij het schrijven van dit artikel de huidige versie van het pakket ActiveDirectory 3.10.305231913, is dus u kunt de meest recente versie bevestigen en dienovereenkomstig bijwerken.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

De toepassingscode een klasse waarin de methode voor de verificatie van Active Directory te maken. In dit voorbeeld klasse heet 'Utils'. Vervolgens moet u de volgende via toevoegen.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Voeg vervolgens de volgende methode voor het ophalen van het token JWT uit Azure Active Directory. Gecodeerde string-waarden in de configuratie van uw web- of voor onderhoud kunt u de vaste verplaatsen.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Ten slotte kunt u de benodigde code sleutel kluis aanroepen en het ophalen van uw geheime waarde toevoegen. Eerst moet u de volgende toevoegen met behulp van de instructie.

```csharp
using Microsoft.Azure.KeyVault;
```

Vervolgens voegt u de methodeaanroepen sleutel kluis aanroepen en uw geheime ophalen. In deze methode krijgt u het geheim URI die u hebt opgeslagen in een eerdere stap. Let op het gebruik van de methode GetToken van de bovenstaande Utils-klasse.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Wanneer u uw toepassing uitvoert, nu moet u verificatie naar Azure Active Directory en vervolgens uw geheime waarde ophalen uit uw Azure sleutel kluis.

##<a name="key-rotation-using-azure-automation"></a>Sleutel rotatie met behulp van automatisering Azure

Er zijn verschillende opties voor het implementeren van een strategie voor waarden als Azure sleutel kluis geheimen voor rotatie. Geheimen kunnen worden gedraaid als onderdeel van een handmatig proces, kan via programmacode door gebruik te maken van de API-aanroepen worden gedraaid of ze kunnen worden gedraaid in een automatiseringsscript. Voor de toepassing van dit artikel zullen we gebruik van Azure PowerShell in combinatie met Azure Automatisering een toegangstoets Azure opslag Account wijzigen en vervolgens een geheime sleutel kluis wordt bijgewerkt met de nieuwe sleutel. 

Teneinde de Azure automatisering geheime waarden instellen in uw sleutel kluis moet u voor de client-ID voor de verbinding met de naam 'AzureRunAsConnection' die is gemaakt toen u uw exemplaar van Azure automatisering tot stand gebracht. U krijgt deze id "Activa" te kiezen in uw exemplaar Azure automatisering. Van daaruit kiezen 'Verbindingen' en selecteert u het beginsel van de service 'AzureRunAsConnection'. U wilt nemen nota van de toepassing ID. 

![De Client-ID Azure automatisering](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Terwijl u nog steeds in het venster activa wilt u ook kiest u 'Modules'. Uit modules u 'Galerie' te selecteren en vervolgens zoeken naar en 'Import' bijgewerkte versies van elk van de volgende modules.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. Opmerking\] bij het schrijven van dit artikel alleen de hierboven aangegeven modules die nodig zijn voor het script onder gedeeld worden bijgewerkt. Als u vindt dat uw taak automatisering niet goed werkt, moet u bevestigen dat u alle benodigde modules en bijbehorende afhankelijkheden geïmporteerd hebt.

Nadat u hebt de toepassings-ID voor uw verbinding Azure automatisering opgehaald, moet u uw Azure sleutel kluis laten weten dat deze toepassing toegang heeft tot geheimen in uw kluis bijwerken. Dit kunt u doen met de volgende opdracht met PowerShell.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Vervolgens selecteert u de resource 'Runbooks' onder uw automatisering Azure-instantie en selecteer 'een Runbook toevoegen'. Selecteer ' snel '. De naam van uw runbook en selecteer 'PowerShell' als het type runbook. U kunt desgewenst een beschrijving toevoegen. Klik tot slot op 'Maken'.

![Runbook maken](./media/keyvault-keyrotation/Create_Runbook.png)

In het venster editor voor de nieuwe runbook wordt u de volgende PowerShell script plakken.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Vanuit het deelvenster editor kunt u 'Testvenster' Test uw script. Zodra het script wordt uitgevoerd zonder fouten kunt u de optie 'Publiceren' en vervolgens kunt u een schema voor de runbook weer in het deelvenster runbook configuratie toepassen.

##<a name="key-vault-auditing-pipeline"></a>Sleutel kluis controle pijpleiding

Wanneer u een Azure sleutel kluis kunt u de controle voor het verzamelen van Logboeken op toegangsaanvragen voor de sleutel kluis. Deze logboeken worden opgeslagen in een aangewezen Azure opslag-account en vervolgens kunnen worden opgevraagd, bewaakt en geanalyseerd. Hieronder een scenario dat gebruikmaakt van functies voor Azure doorloopt, controlelogboeken Azure logica Apps en sleutel kluis om te maken van een pijpleiding voor het verzenden van een e-mailbericht wanneer de geheimen van de kluis worden opgehaald door een app die overeenkomt met de id van de toepassing van het web app.

Eerst moet u logboekregistratie inschakelen op uw sleutel kluis. U kunt dit doen via de volgende PowerShell-opdrachten (volledige gegevens kunnen worden gezien [hier](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Als deze optie is ingeschakeld, vervolgens start controlelogboeken verzamelen opslag aangewezen rekening te houden. Deze logboeken bevatten gebeurtenissen over hoe en wanneer uw sleutel kluizen worden benaderd, en door wie. 

> \[AZURE. Opmerking\] u toegang hebt tot uw gegevens vastleggen op meest, 10 minuten nadat de sleutel kluis bewerking. In de meeste gevallen worden sneller dan dit.

De volgende stap is om een wachtrij Azure Service Bus te [maken](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Dit is waar de sleutel kluis controlelogboeken worden geduwd. Eenmaal in de wachtrij, de logica App zal ze afhalen en ondernemen. Voor het maken van een Service Bus is relatief ongecompliceerde en hieronder vindt u de stappen van hoog niveau:

1. Een Service Bus-naamruimte maken (als u al die u wilt gebruiken voor deze vervolgens verder met stap 2 hebt).
2. Ga naar de Bus-Service in de portal en selecteer de naamruimte die u wilt maken van de wachtrij in.
3. Selecteer Nieuw en kies Service Bus -> wachtrij en voer de vereiste gegevens.
4. Pak de verbindingsgegevens Service Bus door de naamruimte te kiezen en _De verbindingsgegevens_op. U moet deze informatie voor het volgende gedeelte.

Vervolgens kun je het [maken van een Azure-functie](../azure-functions/functions-create-first-azure-function.md) om te peilen de sleutel kluis Logboeken in de account van de opslag en pick up nieuwe gebeurtenissen. Dit is een functie die wordt geactiveerd op een schema.

Maak een functie Azure (Kies Nieuw -> functie App in de portal). U kunt tijdens het maken van een bestaande hosting plan gebruiken of een nieuwe maken. U kan ook kiezen voor het hosten van dynamische. Meer informatie over de functie hosting-opties vindt u [hier](../azure-functions/functions-scale.md).

Wanneer de functie Azure is gemaakt, gaat u naar het en kiest u een timer functie en C\# klik vervolgens op **maken** van het startscherm.

![Azure functies Start Blade](./media/keyvault-keyrotation/Azure_Functions_Start.png)

In het tabblad _opstellen_ en de run.csx-code te vervangen door het volgende:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. Opmerking\] Vervang de variabelen in de code hierboven verwijst naar de rekening van uw opslag waar de sleutel kluis Logboeken geschreven, de Service Bus die u eerder hebt gemaakt en het specifieke pad naar de sleutel kluis opslag Logboeken.

De functie het meest recente logboekbestand van de opslag rekening waar de sleutel kluis Logboeken geschreven opvangt, pakt de meest recente gebeurtenissen uit dat bestand en duwt ze naar een wachtrij Service Bus. Aangezien een bestand kan meerdere gebeurtenissen, bijvoorbeeld maken via een volledig uur, vervolgens we een _sync.txt_ bestand dat de functie wordt ook gekeken om te bepalen van het tijdstempel van de laatste gebeurtenis die is opgehaald. Hierdoor weet u zeker dat we geen push-dezelfde gebeurtenis vaker. Dit bestand _sync.txt_ bevat alleen een tijdstempel van de laatste gebeurtenis opgetreden. De logboeken wanneer geladen, moeten worden gesorteerd op basis van het tijdstempel zodat ze correct worden besteld.

Voor deze functie, verwijzen we naar een aantal aanvullende bibliotheken die niet uit de doos in Azure-functies beschikbaar zijn. Zodanig dat deze, moeten we Azure functies om ze met behulp van nuget. Kies de optie _Bestanden weergeven_ 

![De optie bestanden weergeven](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

en voeg een nieuw bestand met de naam _project.json_ met de volgende inhoud:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Bij het _Opslaan_ wordt dit geactiveerd Azure functies om de vereiste binaire bestanden te downloaden. 

Overschakelen naar het tabblad **integratie** en geef de parameter timer een betekenisvolle naam te gebruiken in de functie. In de code wordt ervan uitgegaan dat de timer moet _myTimer_worden aangeroepen. Geef een [expressie voor CRON](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) als volgt: 0 \* \* \* \* \* voor de timer, waardoor de functie voor het uitvoeren van elke minuut. 

Voeg hetzelfde tabblad **integreren** , input van het type _Azure Blob-opslag zijn zal_. Dit verwijst naar het bestand _sync.txt_ met het tijdstempel van de laatste gebeurtenis die door de functie gekeken. Dit zal beschikbaar worden gesteld binnen de functie door de parameternaam van de. De invoer Azure Blob-opslag verwacht in de code, de naam van de parameter _inputBlob_worden. Kies de opslag account waarin het bestand _sync.txt_ op te geven (mogelijk dezelfde of een andere opslag-account) en geef in het padveld het pad waar het bestand zich in de notatie {container-name}/path/to/sync.txt.

Voeg een uitvoer die van het type uitvoer _Azure Blob-opslag_ . Dit verwijst ook naar het bestand _sync.txt_ dat u zojuist hebt gedefinieerd in de invoer. Dit wordt gebruikt door de functie voor het schrijven van de tijdstempel van de laatste gebeurtenis gekeken. De bovenstaande code verwacht dat deze parameter _outputBlob_worden aangeroepen.

De functie is nu klaar. Controleer of overschakelen naar het tabblad **opstellen** en _Sla_ de code. Het uitvoervenster voor eventuele compileerfouten controleren en corrigeren die dienovereenkomstig. Als het compileren, vervolgens de code moet nu worden uitgevoerd en elke minuut wordt de sleutel kluis logboeken controleren en nieuwe gebeurtenissen naar de gedefinieerde Service Bus wachtrij push. Hier ziet u logboekgegevens wegschrijven naar de telkens van het log venster wanneer die de functie wordt geactiveerd.

###<a name="azure-logic-app"></a>Azure logica App

Vervolgens moeten we maken een Azure logica App die de gebeurtenissen die de functie naar de wachtrij Service Bus pusht afhalen, parseren van de inhoud en stuurt u een e-mail op basis van een voorwaarde wordt aangepast.

[Een App logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md) door te gaan naar nieuw -> logica App. 

Zodra de App logica is gemaakt, gaan en kies _bewerken_. In de editor logica App kiest dat de _Service Bus wachtrij_ beheerde api en voer uw referenties Service Bus om naar de wachtrij.

![Azure logica App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Kies vervolgens _een voorwaarde_toevoegen. In de voorwaarde, overschakelen naar de _Geavanceerde editor_ en voer de volgende gegevens, de APP_ID vervangen door de werkelijke APP_ID van uw web app:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Deze expressie in feite retourneert **false** als de **toepassings-id** -eigenschap van de binnenkomende gebeurtenis (dit is de hoofdtekst van het bericht Service Bus) niet de **toepassings-id** van de toepassing is. 

Maak nu een actie onder de optie _Als er geen,... niets doen_ .

![Azure logica App kiezen actie](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Kies voor de actie, _Office 365 - e-mailbericht verzenden_. Vul de velden voor het maken van een e-mailbericht te verzenden wanneer de opgegeven voorwaarde wordt false geretourneerd. Als u geen Office 365 kan u alternatieven voor het bereiken van dezelfde bekijken.

U hebt nu een complete pijpleiding die minuut, wordt gezocht naar een nieuwe sleutel kluis controlelogboeken. Alle nieuwe logboeken die wordt gevonden, het zal duwen ze naar een wachtrij Service Bus. De App logica wordt geactiveerd zodra een nieuw bericht in de wachtrij terechtkomt en als de appid binnen de gebeurtenis niet overeen met de app-id van de aanroepende toepassing een e-mailbericht verzenden. 
