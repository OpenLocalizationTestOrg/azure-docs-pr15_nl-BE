<properties
    pageTitle="Het Google-verificatie configureren voor de toepassing Services App"
    description="Informatie over het Google-verificatie configureren voor uw App Services-toepassing."
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

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Het configureren van uw toepassing App Service Google-aanmelding

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp wordt beschreven hoe u Azure App Service Google gebruiken als een verificatieprovider configureert.

Als u de procedure in dit onderwerp wilt, moet u een Google-account met een geverifieerd e-mailadres hebben. Als u wilt een nieuwe Google-account maakt, gaat u naar [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Uw toepassing registreren met Google

1. Meld u aan bij de [Azure portal]en navigeer naar de toepassing. Kopieer de **URL**die u later configureren met uw Google-app.

2. Ga naar de website van [Google API's](http://go.microsoft.com/fwlink/p/?LinkId=268303) , aanmelden met uw Google-accountreferenties, **Project maken**, een **naam van het Project**, klik op **maken**.

3. Klik onder **Sociale API's** op **Google + API** en klik vervolgens op **inschakelen**.

4. In de linkernavigatiebalk **referenties** > **OAuth toestemming scherm**, selecteer vervolgens uw **e-mailadres**, voer een **Productnaam**en klik op **Opslaan**.

5. Klik op het tabblad **referenties** **maken referenties** > **OAuth client-ID**en klik op select- **webtoepassing**.

6. De App **URL** u eerder hebt gekopieerd in **JavaScript oorsprong mag**plakken en vervolgens het omleiden van URI in **Redirect URI mag**plakken. De omleiding URI is de URL van uw toepassing met het pad naar de _/.auth/login/google/callback_toegevoegd. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/google/callback`. Let erop dat u het HTTPS-schema. Klik vervolgens op **maken**.

7. Maak een notitie van de waarden van de client-ID en het geheim van de client op het volgende scherm.


    > [AZURE.IMPORTANT]
    Het geheim van de client is een belangrijke. Dit geheim delen met iedereen of verspreiden binnen een clienttoepassing niet.


## <a name="secrets"> </a>Informatie van Google toevoegen aan uw toepassing

8. Ga terug in de [Azure portal]aan uw toepassing. Klik op **Instellingen**en vervolgens **verificatie / vergunning**.

9. Als de verificatie / vergunning functie niet is ingeschakeld, de schakelaar op **On**.

10. Klik op **Google**. Plakken in de App-ID en het geheim van App-waarden die u eerder hebt verkregen en eventueel alle scopes die uw toepassing moet inschakelen. Klik vervolgens op **OK**.

    ![][1]

    Standaard App Service biedt verificatie, maar blokkeert niet geautoriseerde toegang tot uw site-inhoud en API's. U moet gebruikers machtigen in uw code app.

17. (Optioneel) Ingesteld om toegang te beperken tot alleen gebruikers die door Google uw site, **Google** **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** . Hiervoor is vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Google voor verificatie.

12. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Google voor verificatie in uw app.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portal]: https://portal.azure.com/

