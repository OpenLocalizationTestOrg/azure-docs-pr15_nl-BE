<properties 
    pageTitle="Web app met tabelopslag (Node.js) | Microsoft Azure" 
    description="Een zelfstudie die op het Web App met Express zelfstudie voortbouwt door toevoeging van Azure Storage-services en de module Azure." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>Webtoepassing node.js opslag

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt u de toepassing die is gemaakt in de [Webtoepassing Node.js Express met] zelfstudie met behulp van de Microsoft Azure Client Libraries voor Node.js werken met data management services uitbreiden. U kunt uw toepassing op het web gebaseerde takenlijst een toepassing maken die u op Azure implementeren kunt zal uitbreiden. De lijst met taken kan een gebruiker taken ophalen, nieuwe taken toevoegen en taken als voltooid markeren.

De items worden opgeslagen in Azure opslag. Azure opslag biedt ongestructureerde gegevensopslag fouttolerante en hoge beschikbaarheid. Azure opslag omvat verschillende gegevensstructuren, waar u kunt opslaan en toegang tot gegevens en u van de API's opgenomen in de SDK Azure voor Node.js of via API's de REST van de storage-services gebruikmaken kunt. Zie voor meer informatie [Opslaan en toegang krijgen tot gegevens in Azure].

In deze zelfstudie wordt ervan uitgegaan dat u de [Webtoepassing van Node.js] en [Node.js met Express][Node.js Web Application Express gebruiken] zelfstudies hebt voltooid.

U leert:

-   Het werken met de sjabloon Jade-engine
-   Werken met beheer van Azure services

Een schermafbeelding van de voltooide toepassing lager is dan:

![De voltooide webpagina in internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Opslag referenties instellen in Web.Config

Voor toegang tot opslag Azure, moet u in opslag referenties doorgeeft. Hiervoor moet u instellingen van web.config gebruiken.
Deze instellingen zullen worden doorgegeven als omgevingsvariabelen knooppunt, die vervolgens worden gelezen door de SDK Azure.

> [AZURE.NOTE] Opslag referenties worden alleen gebruikt wanneer de toepassing wordt gedistribueerd naar Azure. Wanneer in de emulator wordt uitgevoerd, wordt de toepassing de emulator opslag gebruiken.

Voer de volgende stappen uit om de referenties van de opslag te halen en deze toevoegen aan de instellingen van web.config:

1.  Als dit nog niet is geopend, de Azure PowerShell start vanuit het menu **Start** door **Alle programma's, Azure**uit te breiden met de rechtermuisknop op **Azure PowerShell**en selecteer **Als Administrator uitvoeren**.

2.  Mappen naar de map waarin de toepassing wijzigen. Bijvoorbeeld: C:\\knooppunt\\tasklist\\WebRole1.

3.  Voer in het venster Azure Powershell de volgende cmdlets voor het ophalen van gegevens van de opslag:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    Hierdoor wordt de lijst met accounts voor opslag en sleutels die zijn gekoppeld aan het gehoste service account opgehaald.

    > [AZURE.NOTE] Aangezien de Azure SDK een opslag-account maakt wanneer u een service implementeren, moet een account voor de opslag van de implementatie van de toepassing in de vorige hulplijnen bestaat.

4.  Open het bestand **ServiceDefinition.csdef** is de die omgevingsinstellingen bevat die worden gebruikt wanneer de toepassing wordt gedistribueerd naar Azure:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Voeg het volgende blok onder **milieu** -element vervangen door {opslag ACCOUNT} en {opslag SNELTOETS} met de naam van de account en de primaire sleutel voor de opslag die u wilt gebruiken voor de implementatie:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![De inhoud van het bestand web.cloud.config](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Sla het bestand op en sluit Kladblok af.

### <a name="install-additional-modules"></a>Extra modules installeren

2. Gebruik de volgende opdracht installeren [azure], [knooppunt-uuid] [nconf] en [async] modules lokaal ook over een vermelding voor deze opslaan in het bestand **package.json** :

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    De uitvoer van deze opdracht ziet er ongeveer als volgt:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Met behulp van de service van de tabel in een toepassing knooppunt

In deze sectie geldt de eenvoudige toepassing gemaakt met de **uitdrukkelijke** opdracht door een **task.js** -bestand waarin het model voor uw taken toe te voegen. U ook de bestaande **app.js** te wijzigen en een nieuw **tasklist.js** -bestand dat gebruikmaakt van het model.

### <a name="create-the-model"></a>Het model maken

1. Maak een nieuwe map met de naam **modellen**in de map **WebRole1** .

2. Maak een nieuw bestand met de naam **task.js**in de map **modellen** . Dit bestand bevat het model voor de taken die door uw toepassing zijn gemaakt.

3. Voeg de volgende code als u verwijst naar de vereiste bibliotheken aan het begin van het bestand **task.js** :

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Vervolgens voegt u code wilt definiëren en exporteren het Task-object. Dit object is verantwoordelijk voor het maken van verbinding met de tabel.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Vervolgens de volgende code toevoegen om extra methoden definiëren voor het Task-object, waardoor interacties met de gegevens die zijn opgeslagen in de tabel:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Opslaan en sluiten van het bestand **task.js** .

### <a name="create-the-controller"></a>De domeincontroller maakt

1. Maak een nieuw bestand met de naam **tasklist.js** in de map **WebRole1/routes** en openen in een teksteditor.

2. De volgende code toevoegen aan de **tasklist.js**. Dit laadt de azure en asynchrone modules, die worden gebruikt door **tasklist.js**. Hiermee definieert u ook de functie **TaskList** , die een exemplaar van het **Task** -object dat we eerder gedefinieerd wordt doorgegeven:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Blijven toevoegen aan het bestand **tasklist.js** door de gebruikte methoden voor **showTasks**, **addTask**en **completeTasks**toe te voegen:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Sla het bestand **tasklist.js** .

### <a name="modify-appjs"></a>App.js wijzigen

1. Open in de directory **WebRole1** het bestand **app.js** in een teksteditor. 

2. Voeg de volgende de azure module laden en stelt u de sleutel van de tabel en de partitie aan het begin van het bestand:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. Schuif in het bestand app.js naar waar u de volgende regel ziet:

        app.use('/', routes);
        app.use('/users', users);

    De bovenstaande regels vervangen door de onderstaande code. Dit wordt een exemplaar van de <strong>taak</strong> met een verbinding met uw account opslag initialiseren. Dit wordt doorgegeven aan de <strong>TaskList</strong>, die gebruiken zal om te communiceren met de service van de tabel:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Sla het bestand **app.js** .

### <a name="modify-the-index-view"></a>De weergave van de index wijzigen

1. Mappen naar de map **weergaven** wijzigen en open het bestand **index.jade** in een teksteditor.

2. De inhoud van het bestand **index.jade** vervangen door de onderstaande code. Hiermee definieert u de weergave voor het weergeven van bestaande taken, alsook een formulier voor het toevoegen van nieuwe taken en bestaande markeren als voltooid.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Opslaan en sluiten **index.jade** bestand.

### <a name="modify-the-global-layout"></a>De algemene lay-out wijzigen

Het bestand **layout.jade** in de map **weergaven** wordt gebruikt als een algemene sjabloon voor andere bestanden **.jade** . In deze stap wijzigt u het gebruik van [Twitter Bootstrap](https://github.com/twbs/bootstrap), namelijk een toolkit waarmee u gemakkelijk een leuk uitziende website ontwerpen.

1. Downloaden en uitpakken van de bestanden voor [Twitter Bootstrap](http://getbootstrap.com/). Kopieer het bestand **bootstrap.min.css** uit de **bootstrap\\dist\\css** map naar de **openbare\\stylesheets** map van uw toepassing tasklist.

2. De **layout.jade** in een teksteditor openen vanuit de map **weergaven** en de inhoud vervangen door de volgende:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Sla het bestand **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>De toepassing wordt uitgevoerd in de Emulator

Gebruik de volgende opdracht als u de toepassing wilt starten in de emulator.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

De browser wordt geopend en de volgende pagina wordt weergegeven:

![Een web wisselbaar getiteld mijn takenlijst met een tabel met taken en velden aan een nieuwe taak toevoegen.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Het formulier met items toevoegen of bestaande items verwijderen door deze te markeren als voltooid.

## <a name="publishing-the-application-to-azure"></a>De toepassing op Azure publiceren


In het venster Windows PowerShell roept de volgende cmdlet wilt implementeren uw gehoste service naar Azure.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

**Myuniquename** vervangen door een unieke naam voor deze toepassing. **Datacentername** vervangen door de naam van een Azure Datacenter, zoals **West VS**.

Nadat de installatie voltooid is, ziet u een antwoord van de volgende strekking:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Als voorheen, omdat u hebt opgegeven de **-starten** optie, de browser wordt geopend en wordt de toepassing uitgevoerd in Azure als publiceren is voltooid.

![Een venster van de browser de pagina Mijn taken weergeven. De URL geeft dat nu de pagina wordt gehost op Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Stoppen en de toepassing verwijderen

Na de implementatie van uw toepassing, kunt u uitgeschakeld zodat u kunt voorkomen kosten dat of toepassingen maken en andere binnen de gratis evaluatieversie periode distribueren.

Azure wissels web rol exemplaren per uur van de verbruikte servertijd.
Servertijd wordt verbruikt nadat de toepassing is geïmplementeerd, zelfs als de exemplaren zijn niet actief en gestopt worden.

De volgende stappen laten zien hoe stoppen en uw toepassing verwijderen.

1.  Stop de service implementatie gemaakt in de vorige sectie, met de volgende cmdlet in de Windows PowerShell-venster:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    De service wordt gestopt, kan enkele minuten duren. Wanneer de service wordt gestopt, wordt een bericht weergegeven dat is gestopt.

3.  Als u wilt verwijderen van de service, roept de volgende cmdlet:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Voer desgevraagd **Y** om de service te verwijderen.

    Als u de service verwijdert, kan enkele minuten duren. U ontvangt een bericht dat aangeeft dat de service is verwijderd nadat de service is verwijderd.

  [Node.js webtoepassing met Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Op te slaan en toegang tot gegevens in Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js-webtoepassing]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
