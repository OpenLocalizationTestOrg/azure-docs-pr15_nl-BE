<properties
    pageTitle="Het gebruik van opslag van Node.js wachtrij | Microsoft Azure"
    description="Informatie over het gebruik van de service Azure wachtrij maken en verwijderen van wachtrijen en invoegen, ophalen en verwijderen van berichten. De voorbeelden geschreven in Node.js."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Het gebruik van opslag van Node.js wachtrij

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's voor het gebruik van de service Microsoft Azure wachtrij uitvoeren. De monsters worden geschreven met de API Node.js. De scenario's die zijn **ingevoegd**, **inspecteren**, **ophalen**en **verwijderen van** berichten, alsmede **maken en verwijderen van wachtrijen**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken

Een lege Node.js-toepassing maken. Zie voor instructies over het maken van een toepassing Node.js [een Node.js in Azure App Service web app maken], [bouwen en implementeren van een toepassing Node.js een Azure Cloud-service] met behulp van Windows PowerShell of [bouwen en implementeren van een Node.js web app met Azure met behulp van Web-Matrix].

## <a name="configure-your-application-to-access-storage"></a>De toepassing toegang tot opslag configureren

Azure opslag gebruikt, moet u de SDK Azure opslag voor Node.js, inclusief een set met gemak bibliotheken die met de REST opslagservices communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken voor het verkrijgen van het pakket

1.  Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix), Ga naar de map waar u de voorbeeldtoepassing hebt gemaakt.

2.  Typ **npm azure opslag installeren** in het opdrachtvenster. Uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  U kunt handmatig uitvoeren met de opdracht **ls** te controleren dat een **knooppunt\_modules** map is gemaakt. In deze map vindt u de **azure opslag** -pakket de bibliotheken die u nodig hebt bevat voor toegang tot opslag.

### <a name="import-the-package"></a>Het pakket importeren

Met Kladblok of een andere teksteditor, Voeg het volgende toe aan de bovenkant van het **server.js** -bestand van de toepassing waarin u van plan bent opslag gebruiken:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Setup een verbinding Azure opslag

De module azure leest de omgevingsvariabelen AZURE\_opslag\_ACCOUNT en AZURE\_opslag\_ACCESS\_sleutel of AZURE\_opslag\_verbinding\_STRING voor de informatie die nodig is om verbinding met uw account Azure opslag. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **createQueueService**.

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in [Azure Portal](https://portal.azure.com) voor een Website Azure [Node.js web app met behulp van de tabel Azure Service].

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken

De volgende code maakt een **QueueService** -object kunt u werken met wachtrijen.

    var queueSvc = azure.createQueueService();

Gebruik de methode **createQueueIfNotExists** de opgegeven wachtrij retourneert als dit al bestaat of maakt een nieuwe wachtrij met de opgegeven naam als deze nog niet bestaat.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Als de wachtrij wordt gemaakt, `result.created` is ingesteld op true. Als de wachtrij bestaat, `result.created` is ingesteld op false.

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunnen worden toegepast op de bewerkingen die worden uitgevoerd met behulp van **QueueService**. Filteren van bewerkingen kunt opnemen, aanmelden, automatisch opnieuw wordt geprobeerd, enz. Filters zijn objecten die een methode met de handtekening te implementeren:

    function handle (requestOptions, next)

Na de voorverwerking van de opties van de aanvraag, moet de methode aan te roepen 'volgende' geven een retouraanroep met de volgende handtekening:

    function (returnObject, finalCallback, next)

In dit mechanisme, en na de verwerking van de returnObject (de reactie van de aanvraag naar de server), moet de retouraanroep volgende aanroepen als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon finalCallback anders om uiteindelijk het oproepen van de service starten.

Twee filters die logica opnieuw implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende manier maakt een **QueueService** -object dat de **ExponentialRetryPolicyFilter**wordt gebruikt:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Plaats een bericht in een wachtrij

Gebruik de methode **createMessage** een nieuw bericht maken en aan de wachtrij toevoegen om een bericht in een wachtrij plaatst.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Procedure: Het volgende bericht bekijken

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder het te verwijderen uit de wachtrij door het aanroepen van de methode **peekMessages** . Standaard geeft **peekMessages** een enkel bericht.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

De `result` het bericht bevat.

> [AZURE.NOTE] Met **peekMessages** als er dat geen berichten in de wachtrij retourneert geen fout, maar geen berichten worden geretourneerd.

## <a name="how-to-dequeue-the-next-message"></a>Procedure: Het volgende bericht in wachtrij

Verwerken van een bericht is twee fasen:

1. Het bericht uit de wachtrij halen.

2. Het bericht verwijderen.

Als u wilt een bericht uit de wachtrij halen, gebruikt u **getMessages**. Hierdoor kunt u de berichten zichtbaar in de wachtrij, zodat ze geen andere clients kunnen worden verwerkt. Als uw toepassing een bericht is verwerkt, roept u **deleteMessage** om het te verwijderen uit de wachtrij. In het volgende voorbeeld wordt een bericht weergegeven en vervolgens wordt deze verwijderd:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Een bericht wordt standaard alleen verborgen voor 30 seconden, waarna het zichtbaar voor andere clients is. U kunt een andere waarde opgeven met behulp van `options.visibilityTimeout` met **getMessages**.

> [AZURE.NOTE]
> Met **getMessages** als er dat geen berichten in de wachtrij retourneert geen fout, maar geen berichten worden geretourneerd.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht ter plaatse in de wachtrij met behulp van **updateMessage**. In het volgende voorbeeld wordt de tekst van een bericht bijgewerkt:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Procedure: Aanvullende opties voor waarbij berichten

Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij:

* `options.numOfMessages`-Ophalen van een reeks berichten (maximaal 32).
* `options.visibilityTimeout`-Een langere of kortere invisibility time-out ingesteld.

In het volgende voorbeeld wordt de methode **getMessages** 15 berichten in één aanroep ophalen. En vervolgens de verwerking van elk bericht met een for-lus. De time-out invisibility wordt ingesteld op vijf minuten voor alle berichten die door deze methode wordt geretourneerd.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Procedure: De lengte van de wachtrij ophalen

De **getQueueMetadata** wordt metagegevens over de wachtrij, inclusief het geraamde aantal berichten in de wachtrij staan.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Procedure: Lijst wachtrijen

U haalt een lijst van wachtrijen met **listQueuesSegmented**. Gebruik voor het ophalen van een lijst die is gefilterd op een bepaald voorvoegsel **listQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Als niet alle wachtrijen worden geretourneerd, `result.continuationToken` als de eerste parameter van de **listQueuesSegmented** of de tweede parameter van de **listQueuesSegmentedWithPrefix** kan worden gebruikt om meer resultaten te halen.

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen

Voor het verwijderen van een wachtrij en alle berichten die dit, roept u de methode **deleteQueue** van het wachtrijobject.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Om alle berichten uit een wachtrij wissen zonder het te verwijderen, gebruikt u **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Procedure: werken met gedeelde toegang handtekeningen

Gedeelde toegang handtekeningen (SAS) zijn een veilige methode voor gedetailleerde toegang tot wachtrijen te bieden zonder dat u uw accountnaam voor opslag of sleutels. SA's worden vaak gebruikt voor beperkte toegang tot de wachtrijen, zoals een mobiele toepassingen om berichten te versturen.

Een vertrouwde toepassing, zoals een cloud-gebaseerde service genereert een SAS met behulp van de **generateSharedAccessSignature** van de **QueueService**en biedt dit aan een niet-vertrouwde of semi-vertrouwde toepassing. Een mobiele app. De SAS wordt gegenereerd op basis van een beleid waarin de begin- en einddatums waarvan de SA's geldig is als het toegangsniveau verleend aan de houder SAS.

In het volgende voorbeeld genereert een nieuwe gedeelde-beleid waarmee de houder SAS berichten toevoegen aan de wachtrij en verloopt de 100 minuten na het tijdstip waarop dat deze is gemaakt.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Houd er rekening mee dat de moet hostinformatie worden verstrekt, zoals vereist is wanneer de houder SAS probeert toegang te krijgen tot de wachtrij.

Vervolgens de clienttoepassing gebruikmaakt van de SA's met **QueueServiceWithSAS** voor het uitvoeren van bewerkingen op de wachtrij. In het volgende voorbeeld maakt verbinding met de wachtrij en een bericht wordt gemaakt.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Aangezien de SA's is gegenereerd met toegang toevoegen, als een poging om te lezen, bijwerken of verwijderen van berichten zijn aangebracht, wordt een fout geretourneerd.

### <a name="access-control-lists"></a>Access control list

U kunt ook een lijst met ACL (Access Control) het-beleid instellen voor een SA's. Dit is handig als u dat meerdere clients toegang tot de wachtrij wilt, maar bieden verschillende-beleid voor elke client.

Een ACL is geïmplementeerd met behulp van een array-beleid, met een ID die is gekoppeld aan elk beleid. In het volgende voorbeeld definieert twee beleid; een voor 'user1' en een voor 'gebruiker2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

In het volgende voorbeeld wordt de huidige ACL voor **rapportberichten**en vervolgens voegt het nieuwe beleid met behulp van **setQueueAcl**. Deze benadering kunt:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Zodra de ACL is ingesteld, kunt u vervolgens een SAS op basis van de ID voor een beleid. Het volgende voorbeeld wordt een nieuwe SA's voor 'gebruiker2':

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van de opslag van de wachtrij, volg deze koppelingen voor meer informatie over complexere opslagtaken.

-   Ga naar de [Azure Storage teamblog][].
-   Ga naar de opslagplaats [Azure opslag SDK voor knooppunt][] op GitHub.

  [Azure opslag SDK voor knooppunt]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Een Node.js web app in Azure App Service maken]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js web app met behulp van de tabel Azure Service]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Bouwen en implementeren van een toepassing Node.js een Azure Cloud-service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Azure opslag-teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Bouwen en implementeren van een Node.js web app met Azure met behulp van Web-Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
