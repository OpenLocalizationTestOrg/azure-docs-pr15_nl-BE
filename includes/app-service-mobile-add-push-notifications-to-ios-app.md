
**Doelstelling C**:

1. Importeren in **QSAppDelegate.m**, de iOS SDK en **QSTodoService.h**:
        
        #import <MicrosoftAzureMobile/MicrosoftAzureMobile.h>
        #import "QSTodoService.h"

2. In `didFinishLaunchingWithOptions` in **QSAppDelegate.m**, plaats de volgende regels rechts voordat `return YES;`:

        UIUserNotificationSettings* notificationSettings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeAlert | UIUserNotificationTypeBadge | UIUserNotificationTypeSound categories:nil];
        [[UIApplication sharedApplication] registerUserNotificationSettings:notificationSettings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

3. **QSAppDelegate.m**, voeg de volgende methoden in de handler. De app is nu bijgewerkt ter ondersteuning van push-meldingen. 

        // Registration with APNs is successful
        - (void)application:(UIApplication *)application
        didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
        
            QSTodoService *todoService = [QSTodoService defaultService];
            MSClient *client = todoService.client;
        
            [client.push registerDeviceToken:deviceToken completion:^(NSError *error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
            }];
        }
        
        // Handle any failure to register
        - (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:
        (NSError *)error {
            NSLog(@"Failed to register for remote notifications: %@", error);
        }
        
        // Use userInfo in the payload to display an alert.
        - (void)application:(UIApplication *)application
              didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
        
            NSDictionary *apsPayload = userInfo[@"aps"];
            NSString *alertString = apsPayload[@"alert"];
        
            // Create alert with notification content.
            UIAlertController *alertController = [UIAlertController
                                          alertControllerWithTitle:@"Notification"
                                          message:alertString
                                          preferredStyle:UIAlertControllerStyleAlert];
        
            UIAlertAction *cancelAction = [UIAlertAction
                                           actionWithTitle:NSLocalizedString(@"Cancel", @"Cancel")
                                           style:UIAlertActionStyleCancel
                                           handler:^(UIAlertAction *action)
                                           {
                                               NSLog(@"Cancel");
                                           }];
            
            UIAlertAction *okAction = [UIAlertAction
                                       actionWithTitle:NSLocalizedString(@"OK", @"OK")
                                       style:UIAlertActionStyleDefault
                                       handler:^(UIAlertAction *action)
                                       {
                                           NSLog(@"OK");
                                       }];
            
            [alertController addAction:cancelAction];
            [alertController addAction:okAction];
            
            // Get current view controller.
            UIViewController *currentViewController = [[[[UIApplication sharedApplication] delegate] window] rootViewController];
            while (currentViewController.presentedViewController)
            {
                currentViewController = currentViewController.presentedViewController;
            }
            
            // Display alert.
            [currentViewController presentViewController:alertController animated:YES completion:nil];
        
        }

**Swift**:

1. **ClientManager.swift** -bestand met de volgende inhoud toevoegen. _% AppUrl %_ vervangen door de URL van de backend Azure Mobile App.
        
        class ClientManager {
            static let sharedClient = MSClient(applicationURLString: "%AppUrl%")
        }

2. Vervang in de **ToDoTableViewController.swift**, de `let client` die wordt geïnitialiseerd een `MSClient` met deze regel:

        let client = ClientManager.sharedClient
 
3. In **AppDelegate.swift**, vervangt u de tekst van `func application` als volgt:

        func application(application: UIApplication,
          didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
           application.registerUserNotificationSettings(
               UIUserNotificationSettings(forTypes: [.Alert, .Badge, .Sound],
                   categories: nil))
           application.registerForRemoteNotifications()
           return true
        }

2. **AppDelegate.swift**, voeg de volgende methoden in de handler. De app is nu bijgewerkt ter ondersteuning van push-meldingen.
   
        func application(application: UIApplication,
           didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {
            ClientManager.sharedClient.push?.registerDeviceToken(deviceToken) { error in
                print("Error registering for notifications: ", error?.description)
            }
        }
        
        func application(application: UIApplication,
           didFailToRegisterForRemoteNotificationsWithError error: NSError) {
            print("Failed to register for remote notifications: ", error.description)
        }
        
        func application(application: UIApplication,
           didReceiveRemoteNotification userInfo: [NSObject: AnyObject]) {
            
            print(userInfo)
            
            let apsNotification = userInfo["aps"] as? NSDictionary
            let apsString       = apsNotification?["alert"] as? String
            
            let alert = UIAlertController(title: "Alert", message: apsString, preferredStyle: .Alert)
            let okAction = UIAlertAction(title: "OK", style: .Default) { _ in
                print("OK")
            }
            let cancelAction = UIAlertAction(title: "Cancel", style: .Default) { _ in
                print("Cancel")
            }
            
            alert.addAction(okAction)
            alert.addAction(cancelAction)
            
            var currentViewController = self.window?.rootViewController
            while currentViewController?.presentedViewController != nil {
                currentViewController = currentViewController?.presentedViewController
            }
            
            currentViewController?.presentViewController(alert, animated: true) {}
            
        }
            
