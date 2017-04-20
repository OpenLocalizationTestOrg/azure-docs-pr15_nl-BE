<properties 
    pageTitle="Media Services bijwerken na het walsen toegangstoetsen opslag | Microsoft Azure" 
    description="In dit artikel krijgt u richtlijnen voor het bijwerken van Media Services na het walsen toegangstoetsen voor opslag." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Media Services bijwerken na het walsen toegangstoetsen opslag

Wanneer u een nieuwe Azure Media Services-account maakt, kunt u wordt ook gevraagd om een rekening Azure opslag die wordt gebruikt voor het opslaan van uw media-inhoud te selecteren. Houd er rekening mee dat u [meer dan één opslag-accounts toevoegen](meda-services-managing-multiple-storage-accounts.md) aan uw Media Services-account kunt.

Wanneer een nieuwe opslag-account is gemaakt, genereert Azure twee opslag 512 bits toegangstoetsen, die worden gebruikt voor toegang tot uw account opslag te verifiëren. Het wordt aanbevolen om beveiligen opslag verbindingen meer, periodiek opnieuw genereren en draaien van de sleutel van de toegang tot opslag. Twee toegangstoetsen (primaire en secundaire) worden zodat u verbindingen naar de opslag rekening met een toegangstoets terwijl u de toegangstoets genereren onderhouden geleverd. Deze procedure wordt ook "rolling toegangstoetsen" genoemd.

Media Services is afhankelijk van een sleutel aan opslag. Met name de locators die worden gebruikt om te streamen of te downloaden van uw activa zijn afhankelijk de toegangstoets voor opslag opgegeven. Bij het maken van een account AMS duurt het een afhankelijkheid op de toegangstoets primaire opslag standaard, maar als een gebruiker kunt u de sleutel opslag met AMS bijwerken. U moet ervoor zorgen dat Media Services weet welke sleutel moet worden gebruikt door de stappen in dit onderwerp wordt beschreven. Wanneer de toegangstoetsen opslag, moet u ook controleren of uw locators dus bijwerken wordt er geen onderbreking in de streaming-service (deze stap wordt ook beschreven in het onderwerp).

>[AZURE.NOTE]Als u meerdere accounts voor opslag hebt, zou u deze procedure elke account opslag uitvoeren.
>
>Vóór het uitvoeren van de stappen in dit onderwerp wordt beschreven in een productie-account controleren of om deze te testen op een rekening voorafgaand aan de productie.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Stap 1: Secundaire opslag toegangssleutel opnieuw genereren

Beginnen met secundaire opslag sleutel opnieuw genereren. De secundaire sleutel wordt standaard niet door Media Services gebruikt.  Zie voor meer informatie over het herstellen van sleutels voor opslag [hoe: weergeven, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Stap 2: Update Media Services naar de nieuwe sleutel voor secundaire opslag gebruiken

Services voor het gebruik van de toegangstoets secundaire opslag Media bijwerken. Kunt u een van de volgende twee methoden voor het synchroniseren van de sleutel geregenereerde opslag met Media-Services.

- De Azure portal gebruiken: als de naam en sleutel waarden, gaat u naar de portal Azure en selecteer uw account. Het venster instellingen verschijnt aan de rechterkant. Selecteer in het venster Instellingen toetsen. Afhankelijk van welke toets opslag wilt u selecteren voor de Media-Services te synchroniseren met de primaire sleutel synchroniseren of secundaire sleutel knop synchroniseren. In dit geval gebruikt u de secundaire sleutel.

- Gebruik Media Services-beheer REST API.

In het volgende voorbeeld ziet u hoe de https://endpoint/*subscriptionId*services/mediaservices*Accounts/accountnaam*/StorageAccounts/**storageAccountName/Key verzoek om de sleutel opgegeven opslag worden gesynchroniseerd met Media Services maken. In dit geval wordt wordt de waarde van de secundaire opslag gebruikt. Zie voor meer informatie [hoe: Gebruik Media Services Management REST API](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Update na deze stap, bestaande locators (die afhankelijk zijn van de oude opslag sleutel) in de volgende stap.

>[AZURE.NOTE]Wachten tot 30 minuten duren voordat alle bewerkingen met Media-Services (bijvoorbeeld maken nieuwe locators) om te voorkomen dat eventuele gevolgen op openstaande taken uitvoeren.

##<a name="step-3-update-locators"></a>Stap 3: Update locators

>[AZURE.NOTE]Wanneer rollend toegangstoetsen opslag, moet u controleren of uw bestaande locators bijwerken zodat er geen onderbreking in de streaming-service.

Wacht ten minste 30 minuten na het synchroniseren van de nieuwe sleutel voor opslag met AMS. Vervolgens kunt u opnieuw maken uw locators OnDemand zodat deze afhankelijkheid van de sleutel opgegeven opslag nemen en de bestaande URL houden.

Houd er rekening mee dat wanneer u bijwerkt (of opnieuw) een SAS-locator, de URL wordt altijd wijzigen.

>[AZURE.NOTE] Als u wilt controleren of dat u de bestaande URL's van uw locators OnDemand behouden, moet u de bestaande locator verwijderen en een nieuw bestand maken met dezelfde-ID.

Het volgende .NET-voorbeeld ziet u hoe opnieuw een locator met dezelfde-ID.

persoonlijke statische ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / opslaan van eigenschappen van bestaande locator.
var activa = locator. Actief; var accessPolicy = locator. AccessPolicy; var locatorId = locator. ID; var startDate = locator. StartTime; var locatorType = locator. Type; var locatorName = locator. Naam;

Verwijder oude locator.
Locator. Delete();

Als (locator. ExpirationDateTime < = DateTime.UtcNow) {nieuw uitzondering (String.Format ("Id locator kan niet opnieuw = {0} omdat de verlooptijd locator is in het verleden ', locator. ID)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Stap 5: Primaire opslag toegangssleutel opnieuw genereren

De toegangstoets primaire opslag opnieuw te genereren. Zie voor meer informatie over het herstellen van sleutels voor opslag [hoe: weergeven, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Stap 6: Update Media Services naar de nieuwe primaire opslag sleutel gebruiken
    
Gebruik dezelfde procedure zoals beschreven in [stap 2](media-services-roll-storage-access-keys.md#step2) alleen deze keer de nieuwe primaire opslag toegangstoets worden gesynchroniseerd met de Media-Services-account.

>[AZURE.NOTE]Wachten tot 30 minuten duren voordat alle bewerkingen met Media-Services (bijvoorbeeld maken nieuwe locators) om te voorkomen dat eventuele gevolgen op openstaande taken uitvoeren.

##<a name="step-7-update-locators"></a>Stap 7: Update locators  

Na 30 minuten kunt u uw locators OnDemand opnieuw maken zodat deze afhankelijkheid op de nieuwe sleutel van de primaire opslag uitvoeren en onderhouden van de bestaande URL.

Gebruik dezelfde procedure zoals beschreven in [stap 3](media-services-roll-storage-access-keys.md#step-3-update-locators).


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Bevestigingen 

Wij willen graag de volgende personen die hebben bijgedragen tot de verwezenlijking van dit document erkent: Cenk Dingiloglu, Gada Milaan, Seva Titov.
