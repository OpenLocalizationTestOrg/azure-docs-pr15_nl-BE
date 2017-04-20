**Doelstelling C**: 

1. Op uw Mac, open _QSTodoListViewController.m_ in Xcode en voegt u de volgende methode. Wijzigen _google_ _microsoftaccount_, _twitter_, _facebook_of _windowsazureactivedirectory_ als u niet Google als uw identiteitsprovider gebruikt. Als u Facebook, [moet u "witte" lijst Facebook domeinen in uw app](https://developers.facebook.com/docs/ios/ios9#whitelist).

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Vervangen `[self refresh]` in `viewDidLoad` in _QSTodoListViewController.m_ met het volgende:

            [self loginAndGetData];

3. Druk op _starten_ start de app en meld u vervolgens aan. Als u bent aangemeld, moet u mogelijk zijn de Todo-lijst bekijken en bijwerken.

**Swift**:

1. Op uw Mac, open _ToDoTableViewController.swift_ in Xcode en voegt u de volgende methode. Wijzigen _google_ _microsoftaccount_, _twitter_, _facebook_of _windowsazureactivedirectory_ als u niet Google als uw identiteitsprovider gebruikt. Als u Facebook, [moet u "witte" lijst Facebook domeinen in uw app](https://developers.facebook.com/docs/ios/ios9#whitelist).
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. Verwijder de regels `self.refreshControl?.beginRefreshing()` en `self.onRefresh(self.refreshControl)` aan het einde van `viewDidLoad()` in _ToDoTableViewController.swift_. Toevoegen van een aanroep naar `loginAndGetData()` op hun plaats:

            loginAndGetData()

3. Druk op _starten_ start de app en meld u vervolgens aan. Als u bent aangemeld, moet u mogelijk zijn de Todo-lijst bekijken en bijwerken.
