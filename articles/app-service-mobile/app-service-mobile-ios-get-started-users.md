<properties
    pageTitle="Verificatie op iOS met Azure Mobile Apps toevoegen"
    description="Informatie over het verifiëren van gebruikers van uw iOS-app via een groot aantal identiteitsaanbieders van, met inbegrip van AAD, Google, Facebook, Twitter en Microsoft met Azure Mobile Apps."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Verificatie toevoegen aan uw iOS-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In deze zelfstudie kunt u verificatie toevoegen aan het project [iOS snel starten] met een ondersteunde identiteitsprovider. Deze zelfstudie is gebaseerd op de zelfstudie [iOS snel starten] , die u eerst moet voltooien.

##<a name="register"></a>Registreer uw app voor verificatie en de App-Service configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Machtigingen voor geverifieerde gebruikers beperken

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

In Xcode, drukt u op de app start **uitvoeren** . Een uitzondering zal worden verhoogd omdat de app probeert te krijgen tot de back-end als een niet-geverifieerde gebruiker, maar _TodoItem_ tabel nu verificatie vereist is.

##<a name="add-authentication"></a>Verificatie aan app toevoegen

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[iOS snel starten]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
