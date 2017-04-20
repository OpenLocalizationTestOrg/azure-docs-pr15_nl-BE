<properties
    pageTitle="Verificatie op Android met mobiele Apps toevoegen | Azure App-Service"
    description="Informatie over het gebruik van mobiele Apps in Azure App-Service voor het verifiëren van gebruikers van uw Android app via een groot aantal identiteitsaanbieders van, met inbegrip van Google, Facebook, Twitter en Microsoft."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Verificatie toevoegen aan uw Android app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Samenvatting

In deze zelfstudie kunt u verificatie toevoegen aan de takenlijst van quickstart project op Android met behulp van een ondersteunde identiteitsprovider. Deze zelfstudie is gebaseerd op de zelfstudie [aan de slag met mobiele Apps] die u eerst moet voltooien.

##<a name="register"></a>Registreer uw app voor verificatie en de App-Service configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Machtigingen voor geverifieerde gebruikers beperken

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Open het project de geschatte u voltooid met de zelfstudie [aan de slag met mobiele Apps]in Android Studio. In het menu **uitvoeren** op **app uitvoeren** en controleren dat een niet-verwerkte uitzondering met de statuscode van 401 (Unauthorized) treedt op nadat de toepassing wordt gestart.

     Deze uitzondering treedt op omdat de toepassing probeert te krijgen tot de back-end als een niet-geverifieerde gebruiker, maar de tabel _TodoItem_ nu verificatie vereist is.

Vervolgens kunt u de app om gebruikers te verifiëren voordat u bronnen van de mobiele App-end aanvraagt bijwerken.

## <a name="add-authentication-to-the-app"></a>Verificatie toevoegen aan de app.

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Verificatietokens cache op de client

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Volgende stappen

Nu dat u deze zelfstudie basisverificatie hebt voltooid, kunt u door te gaan op een van de volgende cursussen:

+ [Push-meldingen toevoegen aan uw Android app](app-service-mobile-android-get-started-push.md) Informatie over het configureren van uw mobiele App backend Azure melding Hubs gebruikt om push-meldingen te verzenden.

+ [Off line synchronisatie van uw Android app inschakelen](app-service-mobile-android-get-started-offline-data.md) Informatie over het off line ondersteuning toevoegen uw app met behulp van een mobiele App backend. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Aan de slag met mobiele Apps]: app-service-mobile-android-get-started.md
