<properties
    pageTitle="Het Microsoft-Account-verificatie configureren voor de toepassing Services App"
    description="Informatie over het Microsoft-Account-verificatie configureren voor uw App Services-toepassing."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Het configureren van uw toepassing App Service Microsoft-Account aanmelden

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp wordt beschreven hoe u Azure App Service Microsoft-Account gebruiken als een verificatieprovider configureert. 

## <a name="register-microsoft-account"> </a>Uw app registreren bij Microsoft-Account

1. Meld u aan bij de [Azure portal]en navigeer naar de toepassing. Kopieer de **URL**die u later uw app configureren met Microsoft-Account.

2. Navigeer naar de pagina [Mijn toepassingen] in het Microsoft-Account Developer Center en meld u aan met uw Microsoft-account, indien nodig.

3. Klik op **een app toevoegen**, en vervolgens typt u de naam van een toepassing en op **toepassing maken**.

4. Noteer de **Toepassings-ID**, zoals u zal het later nodig hebt. 

5. Onder "Platforms", klik op **Platform toevoegen** en selecteer 'Web'.

6. Onder 'URI's omleiden' het eindpunt voor uw toepassing te leveren, klik vervolgens op **Opslaan**. 
 
    >[AZURE.NOTE]Het omleiden van URI is de URL van uw toepassing met het pad naar de _/.auth/login/microsoftaccount/callback_toegevoegd. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Let erop dat u het HTTPS-schema.

7. Onder 'Toepassing geheimen', klikt u op **Nieuw wachtwoord genereren**. Noteer de waarde die wordt weergegeven. Nadat u de pagina verlaat, wordt deze niet opnieuw worden weergegeven.


    > [AZURE.IMPORTANT] Het wachtwoord is een belangrijke. Het wachtwoord met anderen delen of distribueren binnen een clienttoepassing niet.

## <a name="secrets"> </a>Informatie van Microsoft-Account toevoegen aan uw App Service toepassing

1. Terug in [Azure portal], gaat u naar de toepassing, klikt u op **Instellingen voor** > **Authentication / Authorization**.

2. Als de verificatie / vergunning functie niet is ingeschakeld, schakel deze **in**.

3. Klik op de **Microsoft-Account**. Plakken in de waarden toepassings-ID en wachtwoord die u eerder hebt verkregen en eventueel alle scopes die uw toepassing moet inschakelen. Klik vervolgens op **OK**.

    ![][1]

    Standaard App Service biedt verificatie, maar blokkeert niet geautoriseerde toegang tot uw site-inhoud en API's. U moet gebruikers machtigen in uw code app.

4. (Optioneel) Instellen om toegang te beperken tot alleen gebruikers die zijn geverifieerd door Microsoft-account van uw site, **actie moet worden ondernomen wanneer de aanvraag niet is geverifieerd** op de **Microsoft-Account**. Hiervoor is vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar een Microsoft-account voor verificatie.

5. Klik op **Opslaan**.

U bent nu gereed voor het Microsoft-Account gebruiken voor verificatie in uw app.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Mijn toepassingen]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/
