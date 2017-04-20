<properties
    pageTitle="Azure Active Directory-verificatie voor de toepassing van de App-Services configureren"
    description="Informatie over het Azure Active Directory-verificatie configureren voor uw App Services-toepassing."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
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

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Het configureren van uw App-servicetoepassing Azure Active Directory-aanmelding gebruiken

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp wordt beschreven hoe u Azure App Services Azure Active Directory gebruikt als een verificatieprovider configureren.

## <a name="express"> </a>Configureren Azure Active Directory met express-instellingen

13. Ga naar de toepassing in de [portal Azure]. Klik op **Instellingen**en klik vervolgens op **Verificatie**.

14. Als de verificatie / vergunning functie niet is ingeschakeld, de schakelaar op **On**.

15. Klik op **Azure Active Directory**en klik vervolgens op **Express** in de **Modus Beheer**.

16. Klik op **OK** om de toepassing in Azure Active Directory registreren. Hiermee maakt u een nieuwe registratie. Als u een bestaande registratie in plaats daarvan kiest, klikt u op **een bestaande app selecteren** en vervolgens zoekt u de naam van een eerder gemaakte registratie binnen uw huurder.
Klik op de registratie te selecteren en klik op **OK**. **Klik op het blad Azure Active Directory-instellingen.**

    ![][0]

    Standaard App Service biedt verificatie, maar blokkeert niet geautoriseerde toegang tot uw site-inhoud en API's. U moet gebruikers machtigen in uw code app.

17. (Optioneel) Instellen om toegang te beperken tot alleen gebruikers die zijn geverifieerd door Azure Active Directory van uw site, **actie moet worden ondernomen wanneer de aanvraag wordt niet geverifieerd** aan te **melden met Azure Active Directory**. Hiervoor is vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.

17. Klik op **Opslaan**.

U bent nu gereed voor Azure Active Directory gebruiken voor verificatie in uw app.

## <a name="advanced"> </a>(Alternatief) Azure Active Directory handmatig configureren met geavanceerde instellingen
U kunt er ook voor kiezen om configuratie-instellingen handmatig. Dit is de beste oplossing als de AAD huurder die u wilt gebruiken afwijkt van de huurder waarbij u zich Azure aanmeldt. Als u de configuratie hebt voltooid, moet u eerst een registratie in Azure Active Directory maken en moet u opgeven aantal van de registratiedetails met App-Service.

### <a name="register"> </a>Uw toepassing met Azure Active Directory registreren

1. Meld u aan bij de [Azure portal]en navigeer naar de toepassing. Kopieer de **URL**. U gebruikt dit uw app Azure Active Directory configureren.

3. Aanmelden bij de [Azure klassieke portal] en Ga naar **Active Directory**.

    ![][2]

4. Selecteer de map en selecteer vervolgens het tabblad **toepassingen** aan de bovenkant. Klik op **toevoegen** onder aan het maken van een nieuwe app registratie.

5. Klik op **het ontwikkelen van mijn organisatie toepassing toevoegen**.

6. Voer een **naam** voor de toepassing in de Wizard toepassing toevoegen en klik op het type **Web Application en/of Web-API** . Klik om verder te gaan.

7. Plak de URL van de toepassing die u eerder hebt gekopieerd in het vak **URL SIGN-ON** . Die dezelfde URL in het vak **URI van App-ID** invoeren. Klik om verder te gaan.

8. Wanneer u de toepassing hebt toegevoegd, klikt u op het tabblad **configureren** . Bewerk de **URL antwoord** bij **eenmalige aanmelding** worden de URL van uw toepassing toegevoegd aan het pad, _/.auth/login/aad/callback_. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Let erop dat u het HTTPS-schema.

    ![][3]

9. Klik op **Opslaan**. Kopieer de **Client-ID** voor de app. U uw toepassing dit om later te gebruiken.

10. In de opdrachtbalk onder **Eindpunten weergeven**, klikt u op en kopieer de URL van **Federation-metagegevens Document** en dit document downloaden of gaan in een browser.

11. In het hoofdelement **EntityDescriptor** moet er een kenmerk **id van de entiteit** van het formulier `https://sts.windows.net/` gevolgd door een specifieke GUID aan de huurder (de zogenaamde "huurder ID"). Kopieer deze waarde - het zal dienen als de **URL van de uitgevende instelling**. U uw toepassing dit om later te gebruiken.

### <a name="secrets"> </a>Informatie Azure Active Directory toevoegen aan uw toepassing

13. Ga terug in de [Azure portal]aan uw toepassing. Klik op **Instellingen**en klik vervolgens op **Verificatie**.

14. Als de functie voor verificatie niet is ingeschakeld, schakelt u de schakeloptie **op**.

15. Klik op **Azure Active Directory**en klik op **Geavanceerd** onder **Beheer-modus**. Plakken in de Client-ID en de URL van de uitgevende instelling de waarde die u eerder hebt verkregen. Klik vervolgens op **OK**.

    ![][1]

    Standaard App Service biedt verificatie, maar blokkeert niet geautoriseerde toegang tot uw site-inhoud en API's. U moet gebruikers machtigen in uw code app.

17. (Optioneel) Instellen om toegang te beperken tot alleen gebruikers die zijn geverifieerd door Azure Active Directory van uw site, **actie moet worden ondernomen wanneer de aanvraag wordt niet geverifieerd** aan te **melden met Azure Active Directory**. Hiervoor is vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.

17. Klik op **Opslaan**.

U bent nu gereed voor Azure Active Directory gebruiken voor verificatie in uw app.

## <a name="optional-configure-a-native-client-application"></a>(Optioneel) Een native client configureren

Azure Active Directory kunt u ook registreren native clients, waarmee u meer controle over het toewijzen van machtigingen. U moet dit als u wilt uitvoeren met behulp van een bibliotheek zoals de **Active Directory-verificatie Library**aanmeldingen.

1. Ga naar **Active Directory** in de [Azure klassieke portal].

2. Selecteer de map en selecteer vervolgens het tabblad **toepassingen** aan de bovenkant. Klik op **toevoegen** onder aan het maken van een nieuwe app registratie.

3. Klik op **het ontwikkelen van mijn organisatie toepassing toevoegen**.

4. Voer een **naam** voor de toepassing in de Wizard toepassing toevoegen en klik op het type **Native Client-toepassing** . Klik om verder te gaan.

5. Voer in het vak **URI omleiden** van uw site _/.auth/login/done_ eindpunt, met behulp van het HTTPS-schema. Deze waarde moet zijn vergelijkbaar met _https://contoso.azurewebsites.net/.auth/login/done_. Als u een Windows-toepassing maakt, gebruiken het [pakket SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als de URI.

6. Wanneer u de oorspronkelijke toepassing hebt toegevoegd, klikt u op het tabblad **configureren** . De **Client-ID** zoeken en maak een notitie van deze waarde.

7. Blader naar de sectie **machtigingen voor andere toepassingen** en klik op **toepassing toevoegen**.

8. Zoek de webtoepassing die u eerder hebt geregistreerd en klikt u op het pictogram. Klik vervolgens op het selectievakje om het dialoogvenster te sluiten. Als de webtoepassing niet kunt vinden, gaat u naar de registratie en een nieuw antwoord URL (bijv. de HTTP-versie van de huidige URL) toevoegen, klikt u op Opslaan en vervolgens deze stappen herhalen - moet de toepassing weergegeven in de lijst.

9. Op de nieuwe post die u zojuist hebt toegevoegd, open de dropdown **Overgedragen machtigingen** en **toegang (toepnaam)**selecteert. Klik vervolgens op **Opslaan**.

U hebt nu een native clienttoepassing die toegang uw App Service toepassing tot geconfigureerd.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure klassieke portal]: https://manage.windowsazure.com/
[alternative method]:#advanced
