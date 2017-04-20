<properties
    pageTitle="Off line synchronisatie inschakelen voor uw Azure Mobile App (iOS)"
    description="Informatie over het gebruik van mobiele Apps in App-Service op off line cache en synchronisatie van gegevens in uw toepassing iOS"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Off line synchronisatie van uw mobiele iOS-app inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie heeft betrekking op de functie Off line synchronisatie van Azure Mobile Apps voor iOS. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe back-end.

Als dit uw eerste ervaring met Azure Mobile Apps, moet u eerst de zelfstudie voor [maken van een iOS-App]te voltooien. Als u niet het project van de server gedownload snel starten, moet u de data access-extensie pakketten toevoegen aan uw project. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie over server-extensie.

Zie voor meer informatie over de functie Off line synchronisatie, het onderwerp [Offline gegevens synchroniseren in Azure Mobile Apps].

## <a name="review-sync"></a>Bekijk de clientcode sync

De client-project dat u de zelfstudie voor [maken van een iOS-App] al gedownload bevat code die off line synchronisatie met behulp van een lokale database op basis van Core-gegevens ondersteunen. In deze sectie vindt u een overzicht van wat al is opgenomen in de code van de zelfstudie. Zie voor een overzicht van de functie [Off line gegevens synchroniseren in Azure Mobile Apps].

De functie Off line gegevens synchroniseren synchronisatie van mobiele Apps in Azure kan eindgebruikers communiceren met een lokale database als het netwerk niet toegankelijk. Als u deze functies in uw app, u de context van de synchronisatie van initialiseren `MSClient` en verwijst naar een lokale winkel. Vervolgens verwijzen naar de tabel via de `MSSyncTable` interface.

1. In het **QSTodoService.m** (doel-C) of **ToDoTableViewController.swift** (Swift), ziet u het type van het lid `syncTable` is `MSSyncTable`. Off line synchronisatie wordt gebruikt voor deze interface sync tabel in plaats van `MSTable`. Wanneer een synchronisatietabel wordt gebruikt, worden alle bewerkingen gaat u naar het lokale archief en alleen worden gesynchroniseerd met de externe back-end met expliciete push- en pull-bewerkingen.

    Als u een verwijzing naar een synchronisatietabel, gebruikt u de methode `syncTableWithName` op `MSClient`. U kunt synchronisatie van off line functionaliteit met `tableWithName` in plaats daarvan.

2. Voordat u een tabelbewerkingen kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. Hier wordt de betreffende code. 
    
    **Doelstelling C**:
    
    In de `QSTodoService.init` methode:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    In de `ToDoTableViewController.viewDidLoad` methode:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Hiermee maakt u een lokale winkel met de interface `MSCoreDataStore`, die beschikbaar is in de SDK van mobiele Apps. U kunt in plaats daarvan een geven een andere lokale winkel door uitvoering van de `MSSyncContextDataSource` protocol. 
    
    Ook de eerste parameter van `MSSyncContext` geeft een conflict-handler wordt gebruikt. We zijn verstreken sinds de `nil`, krijgen we de standaardhandler conflict, die niet bij een conflict.
    
3. Nu gaan we de werkelijke synchronisatiebewerking uit te voeren en gegevens ophalen uit de externe back-end.

    **Doelstelling C**:
    
    `syncData`nieuwe wijzigingen eerst duwt roept vervolgens `pullData` voor het ophalen van gegevens uit de externe back-end. Op hun beurt, de methode `pullData` nieuwe gegevens die overeenkomt met een query als resultaat gegeven:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    In de doelstelling-C-versie in `syncData`, noemen we eerst `pushWithCompletion` in het kader van de synchronisatie. Deze methode is een lid van `MSSyncContext` (in plaats van de synchronisatie tabel zelf) omdat deze wijzigingen in alle tabellen pusht. Alleen records die zijn gewijzigd op enigerlei wijze (via CUD operations) lokaal wordt op de server verzonden. Vervolgens de helper `pullData` wordt genoemd, welke oproepen `MSSyncTable.pullWithQuery` externe gegevens ophalen en opslaan in de lokale database.
    
    In de Swift-versie is geen aanroep van `pushWithCompletion`. Dit komt omdat de push-bewerking niet strikt noodzakelijk is. Als er wijzigingen in behandeling in het kader van de synchronisatie van de tabel die een push-bewerking uitvoert zijn, halen altijd problemen een push eerst. Als er meer dan één synchronisatietabel, is het echter best expliciet aanroepen push om ervoor te zorgen dat alles consistent in gerelateerde tabellen is.
    
    In de doelstelling-C en de Swift-versies, de methode `pullWithQuery` kunt u opgeven van een query de records wilt filteren die u wilt ophalen. In dit voorbeeld wordt de query alleen haalt alle records op in de externe `TodoItem` tabel.
    
    De tweede parameter `pullWithQuery` is een query-ID die wordt gebruikt voor *incrementele synchronisatie*. Incrementele synchronisatie haalt alleen de records die zijn gewijzigd sinds de laatste synchronisatie, met behulp van de record `UpdatedAt` tijdstempel (zogenaamde `updatedAt` in het lokale archief.) De query-ID moet een beschrijvende tekenreeks die uniek is voor elke logische query in uw app. Doorgeven als opt-out van incrementele synchronisatie, `nil` als de query-ID. Houd er rekening mee dat dit mogelijk niet efficiënt is aangezien u alle records op elke pullbewerking worden opgehaald.

5. De doelstelling C app wordt gesynchroniseerd wanneer we wijzigen of toevoegen van gegevens, een gebruiker voert de penbeweging vernieuwen en klik op starten. De Swift-app wordt gesynchroniseerd wanneer een gebruiker de penbeweging vernieuwen uitvoert en op starten. 

Omdat de app wordt gesynchroniseerd wanneer gegevens zijn gewijzigd (doelstelling-C) of als de toepassing wordt gestart (doelstelling C & Swift), de app wordt ervan uitgegaan dat de gebruiker on line is. In een ander gedeelte wordt bijgewerkt de app zodat gebruikers bewerken kunnen, zelfs wanneer ze offline zijn.

## <a name="review-core-data"></a>Bekijk het gegevensmodel Core

Wanneer u de Core off line gegevensopslag gebruikt, moet u bepaalde tabellen en velden definiëren in het gegevensmodel van uw. Het monster app bevat al een gegevensmodel met de juiste indeling. In dit gedeelte bekijken we via deze tabellen en hoe ze worden gebruikt.

- Open **QSDataModel.xcdatamodeld**. Er zijn vier tabellen gedefinieerd--drie die worden gebruikt door de SDK, en één tabel voor de todo items zelf:     *MS_TableOperations: voor het bijhouden van de artikelen die moeten worden gesynchroniseerd met de server    * MS_TableOperationErrors: voor het bijhouden van fouten die tijdens de synchronisatie van off line optreden     *MS_TableConfig: bijgewerkt als voor de laatste bewerking synchroniseren voor alle pull-bewerkingen voor het bijhouden van de laatste    * TodoItem : Voor het opslaan van de items die moeten worden. Het systeem kolommen, **createdAt**, **updatedAt**en **versie** zijn optionele eigenschappen.

>[AZURE.NOTE] De SDK van Azure Mobile Apps reserves kolomnamen die wordt met "**``**". U moet dit voorvoegsel niet gebruiken op een, systeemkolommen, anders de kolomnamen worden gewijzigd wanneer u de externe back-end.

- Als de functie Off line synchronisatie, moet u de systeemtabellen definiëren, zoals hieronder wordt weergegeven.

    ### <a name="system-tables"></a>Systeemtabellen

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Kenmerk  |    Type     |
  	|----------- |   ------    |
  	| ID         | Geheel getal van 64  |
  	| item-id     | Tekenreeks      |
  	| Eigenschappen | Binaire gegevens |
  	| tabel      | Tekenreeks      |
  	| tableKind  | Geheel getal 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Kenmerk  |    Type     |
  	|----------- |   ------    |
  	| ID         | Tekenreeks      |
  	| Bewerkingsnummer | Geheel getal van 64 |
  	| Eigenschappen | Binaire gegevens |
  	| tableKind  | Geheel getal 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Kenmerk  |    Type     |
  	|----------- |   ------    |
  	| ID         | Tekenreeks      |
  	| sleutel        | Tekenreeks      |
  	| keyType    | Geheel getal van 64  |
  	| tabel      | Tekenreeks      |
  	| waarde      | Tekenreeks      |

    ### <a name="data-table"></a>Gegevenstabel

    **TodoItem**

  	| Kenmerk    |  Type   | Opmerking                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| ID           | String, gemarkeerd als vereist  | primaire sleutel in de externe opslag                            |
  	| voltooien     | Boole-waarde | Todo veld                                        |
  	| tekst         | Tekenreeks  | Todo veld                                        |
  	| createdAt | Datum    | (optioneel) wordt toegewezen aan de eigenschap createdAt-systeem         |
  	| updatedAt | Datum    | (optioneel) wordt toegewezen aan de eigenschap updatedAt-systeem         |
  	| Versie   | Tekenreeks  | (optioneel) gebruikt voor het detecteren van conflicten tussen toewijzingen naar versie |


## <a name="setup-sync"></a>Het sync-gedrag van de app wijzigen

In dit gedeelte wijzigt u de app zodat wordt niet gesynchroniseerd op toepassing starten of bij het invoegen en bijwerken van artikelen, maar alleen wanneer de knop Vernieuwen beweging wordt uitgevoerd.

**Doelstelling C**:

1. Wijzigen in **QSTodoListViewController.m**, de **viewDidLoad** -methode voor het verwijderen van de aanroep van `[self refresh]` aan het einde van de methode. Nu de gegevens zullen niet worden gesynchroniseerd met de server in een toepassing starten, maar in plaats daarvan wordt de inhoud van het lokale archief.

2. Wijzigen in **QSTodoService.m**, de definitie van `addItem` , zodat het niet synchroniseren nadat het artikel is ingevoegd. Verwijder de `self syncData` blokkeren en vervangen door het volgende:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Wijzigen van de definitie van `completeItem` als hierboven; Verwijder de blokkering voor `self syncData` en vervangen door het volgende:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. In `viewDidLoad` in **ToDoTableViewController.swift**, een reactie op deze twee regels, niet meer synchroniseren op toepassing starten. Op het moment van schrijven van dit artikel, de Swift Todo app niet de service bijgewerkt wanneer iemand toevoegt of een item, klikt u op toepassing starten is voltooid.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>De app testen

In dit gedeelte maakt u verbinding met een ongeldige URL voor het simuleren van een off line scenario. Wanneer u gegevens toevoegt, worden ze gehouden in de lokale gegevensopslag van Core, maar niet is gesynchroniseerd met de mobiele back-end.

1. Een ongeldige URL wijzigen in de URL van de mobiele App in **QSTodoService.m** en de app opnieuw uit te voeren:

    **Doel-C** in QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** in ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Sommige todo-items toevoegen. Sluit de simulator (of de toepassing geforceerd sluiten) en opnieuw te starten. Controleer of de wijzigingen behouden.

3. De inhoud van de tabel met externe TodoItem weergeven:

    + Een backend Node.js Ga de [Azure portal](https://portal.azure.com/), en met uw mobiele App back-end klikt u op **Eenvoudige tabellen** > **TodoItem** de inhoud van de `TodoItem` tabel.
    + Weergeven voor een .NET back-end, de inhoud van de tabel met een hulpprogramma SQL als SQL Server Management Studio of een andere client zoals Fiddler of Postman.

    Controleer de nieuwe items *niet* zijn gesynchroniseerd met de server:

4. De URL naar de juiste op wijzigen in **QSTodoService.m** en de app opnieuw. De penbeweging vernieuwen omdat de lijst met items op uitvoeren. Ziet u een kringveld in voortgang.

5. De gegevens van de TodoItem weer. De nieuwe en gewijzigde TodoItems moet nu worden weergegeven.

## <a name="summary"></a>Samenvatting

We gebruikt ter ondersteuning van de functie Off line synchronisatie van de `MSSyncTable` interface en geïnitialiseerd `MSClient.syncContext` met een lokale winkel. In dit geval is het lokale archief een database op basis van gegevens van Core.

Wanneer u een Core lokale gegevensopslag gebruikt, moet u verschillende tabellen met de [juiste eigenschappen](#review-core-data)definiëren.

De normale bewerkingen CRUD voor Azure mobiele Apps werken alsof de app is nog steeds verbonden, maar alle bewerkingen tegen het lokale archief plaatsvinden.

Als we willen dat het lokale archief gesynchroniseerd met de server, hebben we gebruikt de `MSSyncTable.pullWithQuery`methode.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Synchronisatie van off line gegevens in Azure mobiele Apps]

* [Cloud Cover: Offline synchronisatie in Azure mobiele Services] \(Opmerking: de video op mobiele diensten, maar off line synchronisatie werkt op vergelijkbare wijze in Azure mobiele Apps\)

<!-- URLs. -->


[Maak een iOS-App]: app-service-mobile-ios-get-started.md
[Synchronisatie van off line gegevens in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Cloud Cover: Offline synchronisatie in Azure mobiele Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
