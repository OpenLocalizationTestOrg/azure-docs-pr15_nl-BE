
Standaard worden-API's van een mobiele App backend anoniem aangeroepen. Vervolgens moet u de toegang beperken tot alleen geverifieerde clients.  

+ **Node.js backend (via de portal)** :  
    
    Van uw mobiele App- **Instellingen**op **Eenvoudige tabellen** en selecteer de tabel. **Machtigingen wijzigen**, **alleen geverifieerde toegang** voor alle machtigingen te selecteren, klik op **Opslaan**. 

+ **Backend .NET (C#)**:  

    Ga naar **domeincontrollers**in de serverproject > **TodoItemController.cs**. Voeg toe de `[Authorize]` kenmerken als volgt aan de klasse **TodoItemController** . Om toegang te beperken tot specifieke methoden, kunt u dit kenmerk ook toepassen om deze methoden in plaats van de klasse. De serverproject publiceren.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js backend (via Node.js code)** :  
    
    Als u wilt verificatie vereist voor toegang tot tabellen, voeg de volgende regel aan het script Node.js server:


        table.access = 'authenticated';

    Raadpleeg voor meer informatie [hoe: verificatie vereist is voor toegang tot tabellen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Zie voor informatie over het downloaden van het project quickstart-code van uw site, [procedure: de Node.js backend quickstart project coderen met behulp van Git downloaden](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

