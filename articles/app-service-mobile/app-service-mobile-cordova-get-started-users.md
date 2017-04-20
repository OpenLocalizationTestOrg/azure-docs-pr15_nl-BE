<properties
    pageTitle="Verificatie op Apache Cordova met mobiele Apps toevoegen | Azure App-Service"
    description="Informatie over het gebruik van mobiele Apps in Azure App-Service voor het verifiëren van gebruikers van uw app Apache Cordova via een groot aantal identiteitsaanbieders van, met inbegrip van Google, Facebook, Twitter en Microsoft."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Verificatie toevoegen aan uw app Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Samenvatting

In deze zelfstudie kunt u verificatie toevoegen aan de takenlijst van quickstart-project op Apache Cordova met een ondersteunde identiteitsprovider. Deze zelfstudie is gebaseerd op de zelfstudie [aan de slag met mobiele Apps] die u eerst moet voltooien.

##<a name="register"></a>Registreer uw app voor verificatie en de App-Service configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Bekijk een video met vergelijkbare stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Machtigingen voor geverifieerde gebruikers beperken

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nu kunt u controleren dat anonieme toegang tot uw backend is uitgeschakeld. Open het project dat u hebt gemaakt toen u de zelfstudie [aan de slag met mobiele Apps], uitgevoerd in Visual Studio en vervolgens uw toepassing uitvoeren in de **Google Android Emulator** en controleer of dat er een onverwachte fout van de verbinding wordt weergegeven nadat de toepassing is gestart.

Vervolgens werkt de app om gebruikers te verifiëren voordat u bronnen van de mobiele App-end aanvraagt.

##<a name="add-authentication"></a>Verificatie toevoegen aan de app.

1. Open het project in **Visual Studio**en open vervolgens de `www/index.html` bestand voor bewerking.

2. Zoek de `Content-Security-Policy` de meta-tag in de head-sectie.  U moet de OAuth host toevoegen aan de lijst met toegestane bronnen.

  	| Provider               | Naam van de Provider SDK | OAuth-Host                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | AAD               | https://login.Windows.NET   |
  	| Facebook               | Facebook          | https://www.Facebook.com    |
  	| Google                 | Google            | https://accounts.Google.com |
  	| Microsoft              | MicrosoftAccount  | https://login.live.com      |
  	| Twitter                | Twitter           | https://API.Twitter.com     |

    Een voorbeeld van de inhoud--beveiligingsbeleid (geïmplementeerd voor Azure Active Directory) is als volgt:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    U moet vervangen `https://login.windows.net` met de host OAuth uit de bovenstaande tabel.  Raadpleeg de [documentatie bij de inhoud beveiligingsbeleid] voor meer informatie over deze meta-code.

    Houd er rekening mee dat sommige verificatieproviders niet dat inhoud beveiligingsbeleid wijzigt hoeven wanneer op de juiste mobiele apparaten gebruikt.  Geen inhoud beveiligingsbeleid wijzigingen zijn vereist met Google-verificatie op een Android apparaat.

3. Open de `www/js/index.js` voor het bewerken van het bestand, Ga naar de `onDeviceReady()` methode, en onder de client maken code de volgende toevoegen:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    De opmerking dat deze code vervangt de bestaande code die de tabelverwijzing maakt en de gebruikersinterface wordt vernieuwd.

    De methode login() begint de verificatie met de provider. De methode login() is een asynchrone functie geeft als resultaat een belofte van JavaScript.  De rest van de initialisatie wordt in het promise-antwoord geplaatst zodat deze niet wordt uitgevoerd totdat de methode login() is voltooid.

4. In de code die u zojuist hebt toegevoegd, vervangen `SDK_Provider_Name` met de naam van de aanmeldingsprovider van uw. Bijvoorbeeld voor Azure Active Directory, gebruikt u `client.login('aad')`.

4. Het project wordt uitgevoerd.  Wanneer het project is geïnitialiseerd, wordt uw toepassing de aanmeldingspagina OAuth weergegeven voor de geselecteerde verificatieprovider.

##<a name="next-steps"></a>Volgende stappen

* Lees meer [Over verificatie] met Azure App-Service.
* De zelfstudie gaan door het [Push-meldingen] toe te voegen aan uw app Apache Cordova.

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Aan de slag met mobiele Apps]: app-service-mobile-cordova-get-started.md
[Inhoud beveiligingsbeleid documentatie]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Push-meldingen]: app-service-mobile-cordova-get-started-push.md
[Info over verificatie]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
