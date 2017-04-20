<properties
    pageTitle="Facebook-verificatie voor de toepassing van de App-Services configureren"
    description="Informatie over het Facebook-verificatie configureren voor uw App Services-toepassing."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Het configureren van uw toepassing App Service Facebook login

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp wordt beschreven hoe u Azure App Service Facebook gebruiken als een verificatieprovider configureert.

U moet een Facebook-account met een geverifieerd e-mailadres en een mobiel telefoonnummer hebben om de procedure in dit onderwerp. U maakt een nieuwe Facebook-account, Ga naar [facebook.com].

## <a name="register"> </a>De toepassing aanmelden bij Facebook

1. Meld u aan bij de [Azure portal]en navigeer naar de toepassing. Kopieer de **URL**. U gebruikt dit voor het configureren van uw Facebook-app.

2. In een ander browservenster, Ga naar de website van de [Facebook ontwikkelaars] en aanmelden met de referenties van uw Facebook-account.

3. (Optioneel) U hebt niet geregistreerd, klik op de **Apps** > **registreren als ontwikkelaar**, vervolgens het beleid te accepteren en de registratie stappen.

4. Klik op **Mijn Apps** > **toevoegen van een nieuwe App** > **Website** > **overslaan en App-ID maken**. 

5. In het vak **Weergegeven naam**Typ een unieke naam voor uw app, typt u uw **E-mailadres van contactpersoon**, kies een **categorie** voor uw app, en vervolgens op **App-ID maken** en vul de beveiligingscontrole. Hiermee gaat u naar het dashboard voor de nieuwe Facebook app voor ontwikkelaars.

6. 'Facebook aanmeldingen', klik op **Aan de slag**. Toevoegen van uw toepassing **Redirect URI** **OAuth geldige URI's**omleiden en klik vervolgens op **Wijzigingen opslaan**. 

    > [AZURE.NOTE] Het omleiden van URI is de URL van uw toepassing met het pad naar de _/.auth/login/facebook/callback_toegevoegd. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Let erop dat u het HTTPS-schema.

6. Klik op **Instellingen**in de linkernavigatiebalk. Klik op **weergeven**op het veld **Geheim App** , uw wachtwoord op te geven als er aangevraagd, noteert u de waarden van **App-ID** en het **Geheim van de App**. U deze later gebruiken voor het configureren van uw toepassing in Azure.

    > [AZURE.IMPORTANT] Het geheim van app is een belangrijke. Dit geheim delen met iedereen of verspreiden binnen een clienttoepassing niet.

7. De Facebook-account die is gebruikt om de toepassing te registreren is een beheerder van de app. Alleen beheerders kunnen nu inloggen op deze toepassing. Voor het verifiëren van andere Facebook-account, klikt u op **App controleren** en **openbaar maken < - app-naam >** algemene toegang van het publiek via Facebook-verificatie inschakelen inschakelen.

## <a name="secrets"> </a>Facebook toevoegen van informatie aan uw toepassing

1. Ga terug in de [Azure portal]aan uw toepassing. Klik op **Instellingen voor** > **verificatie / vergunning**, en zorg ervoor dat de **App Service verificatie** **ingeschakeld is**.

2. **Facebook**, plak in de App-ID en het geheim van App-waarden die u eerder hebt verkregen, eventueel alle scopes die nodig is voor uw toepassing inschakelen, klik op **OK**.

    ![][0]

    Standaard App Service biedt verificatie, maar blokkeert niet geautoriseerde toegang tot uw site-inhoud en API's. U moet gebruikers machtigen in uw code app.

3. (Optioneel) Ingesteld om toegang te beperken tot uw site alleen gebruikers die door Facebook, **Facebook** **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** . Hiervoor is vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Facebook voor verificatie.

4. Wanneer u klaar bent verificatie configureren, klikt u op **Opslaan**.

U bent nu klaar voor gebruik Facebook voor verificatie in uw app.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-ontwikkelaars]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portal]: https://portal.azure.com/
