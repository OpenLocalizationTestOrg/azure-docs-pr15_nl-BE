<properties
    pageTitle="Twitter-verificatie voor de toepassing van de App-Services configureren"
    description="Informatie over het Twitter-verificatie configureren voor uw App Services-toepassing."
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

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Het configureren van uw toepassing App Service Twitter login

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp wordt beschreven hoe u Azure App Service Twitter gebruiken als een verificatieprovider configureert.

Als u de procedure in dit onderwerp wilt, moet u een Twitter-account met een geverifieerde e-mail adres en telefoon nummer hebben. U maakt een nieuwe Twitter-account, gaat u naar <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Uw toepassing registreren met Twitter


1. Meld u aan bij de [Azure portal]en navigeer naar de toepassing. Kopieer de **URL**. U gebruikt dit voor het configureren van uw Twitter-app.

2. Ga naar de website van [Twitter ontwikkelaars] , aanmelden met de referenties van uw Twitter-account en klikt u op **Nieuwe toepassing maken**.

3. Typ de **naam** en een **Beschrijving** voor de nieuwe toepassing. In de **URL** voor de waarde van de **Website** van uw toepassing te plakken. Voor de **Terugbelfunctie URL**, plak de **Callback-URL** die u eerder hebt gekopieerd. Dit is uw mobiele App gateway toegevoegd met het pad naar de _/.auth/login/twitter/callback_. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Let erop dat u het HTTPS-schema.

3.  Lees en accepteer de voorwaarden onderaan de pagina. Klik vervolgens op **uw Twitter-toepassing maken**. Hiermee wordt de app geeft de Toepassingdetails.

4. Klik op het tabblad **Instellingen** , controleren **dat deze toepassing worden gebruikt om aan te melden met Twitter**aan en klik vervolgens op **Instellingen voor updates**.

5. Selecteer het tabblad **toetsen en Access Tokens** . Maak een notitie van de waarden van de **Consumentsleutel (API)** en het **geheim van de consument (geheime API)**.

    > [AZURE.NOTE] Het consumentgeheim is een belangrijke. Dit geheim delen met iedereen of distribueren met uw app niet.


## <a name="secrets"> </a>Informatie aan uw toepassing toevoegen Twitter

13. Ga terug in de [Azure portal]aan uw toepassing. Klik op **Instellingen**en vervolgens **verificatie / vergunning**.

14. Als de verificatie / vergunning functie niet is ingeschakeld, de schakelaar op **On**.

15. Klik op **Twitter**. Waarden die u eerder hebt verkregen in de App-ID en geheim App plakken. Klik vervolgens op **OK**.

    ![][1]

    Standaard App Service biedt verificatie, maar blokkeert niet geautoriseerde toegang tot uw site-inhoud en API's. U moet gebruikers machtigen in uw code app.

17. (Optioneel) Om toegang te beperken tot uw site alleen gebruikers die door Twitter, **Twitter**ingesteld **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** . Hiervoor is vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Twitter voor verificatie.

17. Klik op **Opslaan**.

U bent nu klaar om te gebruiken Twitter voor verificatie in uw app.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter-ontwikkelaars]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
