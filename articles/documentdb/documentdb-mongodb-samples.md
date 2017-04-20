<properties 
    pageTitle="DocumentDB voor voorbeelden van MongoDB | Microsoft Azure" 
    description="Voorbeelden van ondersteuning voor het protocol voor MongoDB van DocumentDB vinden." 
    keywords="Voorbeelden van mongodb"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-protocol-support-for-mongodb-examples"></a>DocumentDB-protocolondersteuning voor voorbeelden van MongoDB
U moet voor het gebruik van deze voorbeelden:

- [Maken](documentdb-create-mongodb-account.md) een Azure DocumentDB rekening met ondersteuning voor MongoDB-protocol.
- Uw account DocumentDB met ondersteuning voor MongoDB [verbindingsreeks](documentdb-connect-mongodb-account.md) informatie ophalen.

## <a name="get-started-with-a-sample-aspnet-mvc-task-list-application"></a>Aan de slag met een voorbeeld van een toepassing van ASP.NET MVC taak lijst

U kunt de zelfstudie [een web app in Azure die verbinding maakt met MongoDB uitgevoerd op een virtuele machine maken](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) met minimale wijzigingen, snel een MongoDB-toepassing instellen (hetzij lokaal of gepubliceerd op een Azure web app) die verbinding maakt met een DocumentDB-account met ondersteuning voor MongoDB-protocol.  

1. Volg de zelfstudie, met een wijziging.  De code van de Dal.cs vervangen door dit:
    
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Web;
        using MyTaskListApp.Models;
        using MongoDB.Driver;
        using MongoDB.Bson;
        using System.Configuration;
        using System.Security.Authentication;

        namespace MyTaskListApp
        {
            public class Dal : IDisposable
            {
                //private MongoServer mongoServer = null;
                private bool disposed = false;

                // To do: update the connection string with the DNS name
                // or IP address of your server. 
                //For example, "mongodb://testlinux.cloudapp.net
                private string connectionString = "mongodb://localhost:27017";
                private string userName = "<your user name>";
                private string host = "<your host>";
                private string password = "<your password>";

                // This sample uses a database named "Tasks" and a 
                //collection named "TasksList".  The database and collection 
                //will be automatically created if they don't already exist.
                private string dbName = "Tasks";
                private string collectionName = "TasksList";

                // Default constructor.        
                public Dal()
                {
                }

                // Gets all Task items from the MongoDB server.        
                public List<MyTask> GetAllTasks()
                {
                    try
                    {
                        var collection = GetTasksCollection();
                        return collection.Find(new BsonDocument()).ToList();
                    }
                    catch (MongoConnectionException)
                    {
                        return new List<MyTask>();
                    }
                }

                // Creates a Task and inserts it into the collection in MongoDB.
                public void CreateTask(MyTask task)
                {
                    var collection = GetTasksCollectionForEdit();
                    try
                    {
                        collection.InsertOne(task);
                    }
                    catch (MongoCommandException ex)
                    {
                        string msg = ex.Message;
                    }
                }
        
                private IMongoCollection<MyTask> GetTasksCollection()
                {
                    MongoClientSettings settings = new MongoClientSettings();
                    settings.Server = new MongoServerAddress(host, 10250);
                    settings.UseSsl = true;
                    settings.SslSettings = new SslSettings();
                    settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
        
                    MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                    MongoIdentityEvidence evidence = new PasswordEvidence(password);
        
                    settings.Credentials = new List<MongoCredential>()
                    {
                        new MongoCredential("SCRAM-SHA-1", identity, evidence)
                    };

                    MongoClient client = new MongoClient(settings);
                    var database = client.GetDatabase(dbName);
                    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                    return todoTaskCollection;
                }
        
                private IMongoCollection<MyTask> GetTasksCollectionForEdit()
                {
                    MongoClientSettings settings = new MongoClientSettings();
                    settings.Server = new MongoServerAddress(host, 10250);
                    settings.UseSsl = true;
                    settings.SslSettings = new SslSettings();
                    settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
        
                    MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                    MongoIdentityEvidence evidence = new PasswordEvidence(password);
        
                    settings.Credentials = new List<MongoCredential>()
                    {
                        new MongoCredential("SCRAM-SHA-1", identity, evidence)
                    };
                    MongoClient client = new MongoClient(settings);
                    var database = client.GetDatabase(dbName);
                    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                    return todoTaskCollection;
                }

                # region IDisposable
        
                public void Dispose()
                {
                    this.Dispose(true);
                    GC.SuppressFinalize(this);
                }

                protected virtual void Dispose(bool disposing)
                {
                    if (!this.disposed)
                    {
                        if (disposing)
                        {
                        }
                    }

                    this.disposed = true;
                }

                # endregion
            }
        }

2.  De volgende variabelen in het bestand Dal.cs per uw accountinstellingen wijzigen:

        private string userName = "<your user name>";
        private string host = "<your host>";
        private string password = "<your password>";

3. Gebruik de app!

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe te [gebruiken MongoChef](documentdb-mongodb-mongochef.md) met een DocumentDB-account met protocol ondersteunen voor MongoDB.

 
