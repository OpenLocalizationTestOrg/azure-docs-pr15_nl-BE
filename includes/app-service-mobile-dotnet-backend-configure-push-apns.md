
+ **Backend .NET (C#)**:    
    1. In Visual Studio met de rechtermuisknop op het serverproject en klik op **Beheren NuGet pakketten**, zoeken naar `Microsoft.Azure.NotificationHubs`, klikt u op **installeren**. Hiermee installeert u de melding Hubs-bibliotheek voor het verzenden van meldingen van uw back-end.

    2. Open in Visual Studio-project van de backend, **Controllers** > **TodoItemController.cs**. Voeg het volgende toe aan de bovenkant van het bestand `using` instructie:

            using Microsoft.Azure.Mobile.Server.Config;
            using Microsoft.Azure.NotificationHubs;


    3. Vervang de `PostTodoItem` methode met de volgende code:  
      
            public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
            {
                TodoItem current = await InsertAsync(item);
                // Get the settings for the server project.
                HttpConfiguration config = this.Configuration;
    
                MobileAppSettingsDictionary settings = 
                    this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
    
                // Get the Notification Hubs credentials for the Mobile App.
                string notificationHubName = settings.NotificationHubName;
                string notificationHubConnection = settings
                    .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;
    
                // Create a new Notification Hub client.
                NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);
    
                // iOS payload
                var appleNotificationPayload = "{\"aps\":{\"alert\":\"" + item.Text + "\"}}";
    
                try
                {
                    // Send the push notification and log the results.
                    var result = await hub.SendAppleNativeNotificationAsync(appleNotificationPayload);
    
                    // Write the success result to the logs.
                    config.Services.GetTraceWriter().Info(result.State.ToString());
                }
                catch (System.Exception ex)
                {
                    // Write the failure result to the logs.
                    config.Services.GetTraceWriter()
                        .Error(ex.Message, null, "Push.SendAsync Error");
                }
                return CreatedAtRoute("Tables", new { id = current.Id }, current);
            }

    4. De serverproject publiceren.

+ **Node.js backend** : 
   
    1. Als u nog niet hebt gedaan, [het project quickstart downloaden](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) of anders de [online editor in Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)gebruiken. 
    
    2. Het script todoitem.js tabel vervangen door de volgende code:


            var azureMobileApps = require('azure-mobile-apps'),
                promises = require('azure-mobile-apps/src/utilities/promises'),
                logger = require('azure-mobile-apps/src/logger');
            
            var table = azureMobileApps.table();
            
            // When adding record, send a push notification via APNS
            table.insert(function (context) {
                // For details of the Notification Hubs JavaScript SDK, 
                // see http://aka.ms/nodejshubs
                logger.info('Running TodoItem.insert');
                
                // Create a payload that contains the new item Text.
                var payload = "{\"aps\":{\"alert\":\"" + context.item.text + "\"}}";
                
                // Execute the insert; Push as a post-execute action when results are returned as a Promise.
                return context.execute()
                    .then(function (results) {
                        // Only do the push if configured
                        if (context.push) {
                            context.push.apns.send(null, payload, function (error) {
                                if (error) {
                                    logger.error('Error while sending push notification: ', error);
                                } else {
                                    logger.info('Push notification sent successfully!');
                                }
                            });
                        }
                        return results;
                    })
                    .catch(function (error) {
                        logger.error('Error while running context.execute: ', error);
                    });
            });
            
            module.exports = table;

    2. Bij het bewerken van het bestand op uw lokale computer publiceert u het serverproject.
