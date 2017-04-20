Gebruik de procedure die overeenkomt met uw back-end-projecttype&mdash; [.NET back-end](#dotnet) of [Node.js backend](#nodejs).

### <a name="dotnet"></a>.NET back-end project

1. In Visual Studio met de rechtermuisknop op het serverproject en klik op **Beheren NuGet pakketten**, zoeken naar `Microsoft.Azure.NotificationHubs`, klikt u op **installeren**. Hiermee installeert u de melding Hubs client library.

2. Open TodoItemController.cs in de map Controllers en voeg de volgende `using` instructies:

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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

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

### <a name="nodejs"></a>Node.js back-end project

1. Als u nog niet hebt gedaan, [het project quickstart downloaden](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) of anders de [online editor in Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)gebruiken.
 
1. De bestaande code in het bestand todoitem.js wordt vervangen door het volgende:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });
        
        module.exports = table;  

    Dit stuurt een GCM melding met het item.text wanneer een nieuw item van de taak wordt ingevoegd. 

2. Bij het bewerken van het bestand op uw lokale computer publiceert u het serverproject. 
