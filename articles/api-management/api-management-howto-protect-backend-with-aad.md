<properties
    pageTitle="Het beveiligen van een Web API backend met Azure Active Directory en de beheer-API"
    description="Informatie over het beveiligen van een Web API backend met Azure Active Directory en het beheer van de API." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Het beveiligen van een Web API backend met Azure Active Directory en de beheer-API

De volgende video laat zien hoe een Web API backend bouwen en OAuth 2.0-protocol met Azure Active Directory en de beheer-API te beschermen.  Dit artikel bevat een overzicht en de aanvullende informatie over de stappen in de video. Deze 24 minuten video ziet u hoe te:

-   Bouwen van een Web API backend en beveiligen met AAD - beginnend bij 1:30
-   Importeren van de API in API-beheer - vanaf 7:10
-   Configureer de Developer-portal aanroepen van de API - beginnen bij 9:09
-   Een desktop configureren om aan te roepen, de API - vanaf 18:08
-   Configureer een beleid JWT validatie vooraf om aanvragen te verifiëren - vanaf 20:47

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Maak een map Azure AD

Voor het beveiligen van uw Web-API back met Azure Active Directory, moet u eerst over een een huurder AAD. In deze video een huurder met de naam **APIMDemo** gebruikt. Als u een huurder AAD, aanmelden bij de [Klassieke Azure-Portal](https://manage.windowsazure.com) en klik op **Nieuw**->**App Services**->**Active Directory**->**map**->**Aangepaste maken**. 

![Azure Active Directory][api-management-create-aad-menu]

In dit voorbeeld wordt een map met de naam **APIMDemo** gemaakt met een standaard-domein met de naam **DemoAPIM.onmicrosoft.com**. Deze map wordt gebruikt in de video.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Een Web API service beveiligd door Azure Active Directory maken

In deze stap wordt een Web-API backend gemaakt met behulp van Visual Studio 2013. In deze stap van de video begint bij 1:30. Voor het maken van Web-API backend-project in Visual Studio klikt u op **bestand**->**nieuwe**->-**Project**en kiest u **ASP.NET-webtoepassing** in **de lijst met sjablonen** . In deze video het project met de naam **APIMAADDemo**. Klik op **OK** om het project te maken. 

![Visual Studio][api-management-new-web-app]

Klik op **Web-API** van de **Selecteer een sjabloon** voor het maken van een project Web API. Azure Directory-verificatie op **Verificatie wijzigen**om te configureren.

![Nieuw project][api-management-new-project]

Klik op **Organisatie-Accounts**en geef het **domein** van de huurder AAD. In dit voorbeeld wordt het domein **DemoAPIM.onmicrosoft.com**. Het domein van de map kan worden verkregen uit **het tabblad van de map** .

![Domeinen][api-management-aad-domains]

Configureer de gewenste instellingen in het dialoogvenster **Verificatie wijzigen** en klik op **OK**.

![De verificatie wijzigen][api-management-change-authentication]

Wanneer u op **OK** klikt Visual Studio zal proberen uw toepassing registreren bij de Azure AD-map en wordt u mogelijk gevraagd aan te melden door Visual Studio. Inloggen met een Administrator-account voor de map.

![Aanmelden bij Visual Studio][api-management-sign-in-vidual-studio]

Dit project als een Azure Web API uit het vak configureren van **Host in de cloud** en klik vervolgens op **OK**.

![Nieuw project][api-management-new-project-cloud]

Wordt u mogelijk gevraagd aan te melden Azure en vervolgens kunt u de Web App.

![Configureren][api-management-configure-web-app]

In dit voorbeeld wordt een nieuw **plan App-Service** met de naam **APIMAADDemo** opgegeven.

Klik op **OK** om de Web App configureren en maken van het project.

## <a name="add-the-code-to-the-web-api-project"></a>Voeg de code toe aan het project Web-API

De code wordt toegevoegd aan het project Web-API van de volgende stap in de video. Deze stap begint om 4:35.

In dit voorbeeld de Web-API implementeert een eenvoudige rekenmachine-service met behulp van een model en een controller. Het model voor de service, **modellen** in de **Solution Explorer** met de rechtermuisknop en kies **toevoegen**, **klasse**. Naam van de klasse `CalcInput` en klik op **toevoegen**.

Voeg de volgende `using` -instructie aan de bovenkant van de `CalcInput.cs` bestand.

    using Newtonsoft.Json;

 De gegenereerde klasse vervangen door de volgende code.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

De **Controllers** in de **Solution Explorer** met de rechtermuisknop en kies **toevoegen**->**Controller**. Kies **Web API 2 Controller - leeg** en klikt u op **toevoegen**. Typ **CalcController** voor de naam van de domeincontroller en klik op **toevoegen**.

![Domeincontroller toevoegen][api-management-add-controller]

Voeg de volgende `using` -instructie aan de bovenkant van de `CalcController.cs` bestand.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

De klasse gegenereerde controller vervangen door de volgende code. Deze code implementeert de `Add`, `Subtract`, `Multiply`, en `Divide` bewerkingen van de eenvoudige rekenmachine-API.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Druk op **F6** om te bouwen en de oplossing te controleren.

## <a name="publish-the-project-to-azure"></a>Het project publiceren naar Azure

In deze stap van de Visual Studio wordt project gepubliceerd in Azure. In deze stap van de video begint bij 5:45.

Als u het project wilt Azure publiceren, met de rechtermuisknop op het **APIMAADDemo** -project in Visual Studio en kies **publiceren**. Wijzig de standaardinstellingen in het dialoogvenster **Web publiceren** en klik op **publiceren**.

![Web publiceren][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Machtigingen verlenen voor de toepassing van Azure AD backend-service

Een nieuwe toepassing voor de backend-service wordt gemaakt in de map Azure AD als onderdeel van het proces van configureren en publiceren van uw project Web API. In deze stap van de video, vanaf 6:13, machtigingen op de Web-API backend.

![Toepassing][api-management-aad-backend-app]

Klik op de naam van de toepassing de vereiste machtigingen configureren. Ga naar het tabblad **configureren** en Ga naar de sectie **machtigingen voor andere toepassingen** . Klik op **Machtigingen** -omlaag naast **Windows** **Azure Active Directory**, schakel het selectievakje in voor **de Active directory-gegevens lezen**en klikt u op **Opslaan**.

![Machtigingen toevoegen][api-management-aad-add-permissions]

>[AZURE.NOTE] Als **Windows** **Azure Active Directory** niet wordt vermeld onder machtigingen voor andere toepassingen, klikt u op **toepassing toevoegen** en uit de lijst toe te voegen.

Maak een notitie van de **App-Id-URI** voor gebruik in een latere stap wanneer een toepassing AD Azure is geconfigureerd voor de beheer-API developer-portal.

![App id-URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Importeren van het Web API in Management API

API's worden geconfigureerd via de API publisher portal, dat toegankelijk is via de klassieke Azure-Portal. Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API voor het bereiken van de publisher-portal. Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in het [beheren van uw eerste API][] -zelfstudie.

![Publisher-portal][api-management-management-console]

Bewerkingen kunnen worden [toegevoegd aan de API's handmatig](api-management-howto-add-operations.md)of kan worden geïmporteerd. In deze video, worden bewerkingen geïmporteerd vanaf 6:40 Swagger-indeling.

Maak een bestand met de naam `calcapi.json` met de volgende inhoud en opslaan op uw computer. Ervoor te zorgen dat de `host` punten van het kenmerk op het Web API backend. In dit voorbeeld `"host": "apimaaddemo.azurewebsites.net"` wordt gebruikt.

{'swagger': "2.0", "info": {"title": "Rekenmachine", "omschrijving": "Arithmetics via HTTP!", 'versie': "1.0"}, "host": "apimaaddemo.azurewebsites.net", "basePath": "/ api", "schema's": ['http'] "paden": {"/ add? een = {a} & b = {b}": {"get": {"Beschrijving": "Reageert met een som van twee getallen." "bewerkingsnummer": "Add twee gehele getallen", "parameters": [{'naam': 'a', 'in': 'query', 'beschrijving': "eerste operand. Standaardwaarde is <code>51</code>. ', 'vereiste': true, 'default': "51", "enum": ["51"]}, {"naam": 'b', 'in': 'query', 'beschrijving': 'de tweede operand. Standaardwaarde is <code>49</code>. ', 'vereiste': true, 'default': "49", "enum": ["49"]}], 'antwoorden': {}}}, "/ sub?a = {a} & b = {b}": {"get": {"Beschrijving": "Reageert met een verschil tussen twee getallen." "bewerkingsnummer": "Subtract twee gehele getallen", "parameters": [{'naam': 'a', 'in': 'query', 'beschrijving': 'eerste operand. Standaardwaarde is <code>100</code>. ', 'vereiste': true, 'default': "100', 'enum": ["100"]}, {"naam": 'b', 'in': 'query', 'beschrijving': 'de tweede operand. Standaardwaarde is <code>50</code>. ', 'vereiste': true, 'default': "50", "enum": ["50"]}], 'antwoorden': {}}}, "/ div?a = {a} & b = {b}": {"get": {"Beschrijving": "Reageert met een quotiënt van twee cijfers." "bewerkingsnummer": "Twee gehele getallen delen", "parameters": [{'naam': 'a', 'in': 'query', 'beschrijving': 'eerste operand. Standaardwaarde is <code>100</code>. ', 'vereiste': true, 'default': "100', 'enum": ["100"]}, {"naam": 'b', 'in': 'query', 'beschrijving': 'de tweede operand. Standaardwaarde is <code>20</code>. ', 'vereiste': true, 'default': "20', 'enum": ['20']}], 'antwoorden': {}}}, "/ mul?a = {a} & b = {b}": {"get": {"Beschrijving": "Reageert met een product van twee getallen." "bewerkingsnummer": "Vermenigvuldigen twee gehele getallen", "parameters": [{'naam': 'a', 'in': 'query', 'beschrijving': 'eerste operand. Standaardwaarde is <code>20</code>. ', 'vereiste': true, 'default': "20', 'enum": ["20"]}, {"naam": 'b', 'in': 'query', 'beschrijving': 'tweede operand. Standaardwaarde is <code>5</code>. ', 'vereiste': true, 'default': "5", "enum": ["5"]}], "antwoorden": {}}}}}

Voor het importeren van de Rekenmachine API **API's** klikt u op in het menu **API beheer** aan de linkerkant en klik vervolgens op **Importeren API**.

![Knop importeren API][api-management-import-api]

Voer de volgende stappen voor het configureren van de Rekenmachine API.

1. Klik op **uit bestand**, blader naar de `calculator.json` u het opgeslagen bestand en klik op het keuzerondje **Swagger** .
2. **Calc** Typ in **achtervoegsel Web API-URL** textbox.
3. Klik in het vak **producten (optioneel)** en kies **Starter**.
4. Klik op **Opslaan** om de API te importeren.

![Nieuwe API toevoegen][api-management-import-new-api]

Zodra de API wordt geïmporteerd, wordt de overzichtspagina voor de API in de portal van publisher weergegeven.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Tevergeefs de API aanroepen vanuit de developer-portal

Op dit moment de API in de beheer-API is ingevoerd, maar kan nog worden aangeroepen met succes de developer-portal omdat de backend-service wordt beveiligd met Azure AD-verificatie. Dit wordt geïllustreerd in de video vanaf 7:40 met behulp van de volgende stappen uit.

Klik boven rechts van de uitgever portal **Developer-portal** .

![Developer-portal][api-management-developer-portal-menu]

**API's** en klik op **Rekenmachine** API.

![Developer-portal][api-management-dev-portal-apis]

Klik op **Probeer het**.

![Probeer het nu][api-management-dev-portal-try-it]

Klik op **verzenden** en bekijk de status van de reactie van **401 Unauthorized**.

![Verzenden][api-management-dev-portal-send-401]

Het verzoek is niet-geautoriseerde omdat de backend-API wordt beschermd door Azure Active Directory. Voordat u met succes de API de developer moet portal zodanig zijn ontwikkelaars met OAuth 2.0 machtigen. Dit proces wordt in de volgende secties beschreven.

## <a name="register-the-developer-portal-as-an-aad-application"></a>De developer-portal te registreren als een toepassing AAD

De eerste stap bij het configureren van de developer-portal voor het autoriseren van ontwikkelaars met OAuth 2.0 is de developer-portal te registreren als een toepassing AAD. Dit wordt geïllustreerd vanaf 8:27 in de video.

Ga naar de huurder Azure AD vanaf de eerste stap van deze video, in dit voorbeeld **APIMDemo** en Ga naar het tabblad **toepassingen** .

![Nieuwe toepassing][api-management-aad-new-application-devportal]

Klik op de knop **toevoegen** als een nieuwe Azure Active Directory-toepassing wilt maken en kies **Mijn organisatie ontwikkelt toepassing toevoegen**.

![Nieuwe toepassing][api-management-new-aad-application-menu]

Kies **webtoepassing en/of Web-API**, voer een naam en klikt u op de pijl volgende gemarkeerd. In dit voorbeeld wordt **APIMDeveloperPortal** gebruikt.

![Nieuwe toepassing][api-management-aad-new-application-devportal-1]

Voer de URL van uw service Management API voor **eenmalige aanmelding URL** en append `/signin`. In dit voorbeeld wordt **https://contoso5.portal.azure-api.net/signin **gebruikt.

Voer de URL van uw service Management API voor **App Id URL** en enkele unieke tekens toevoegen. Dit kunnen elke gewenste tekens en in dit voorbeeld wordt **https://contoso5.portal.azure-api.net/dp** gebruikt. Als de gewenste **App-eigenschappen** zijn geconfigureerd, klikt u op het vinkje om de toepassing te maken.

![Nieuwe toepassing][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Een API Management OAuth 2.0 vergunning-server configureren

De volgende stap is een vergunning OAuth 2.0 server configureren in beheer-API. Deze stap wordt geïllustreerd in de video vanaf 9:43.

Klik op **beveiliging** in het menu API beheer aan de linkerkant **OAuth 2.0**en klik vervolgens op server **toevoegen vergunning** .

![Vergunning server toevoegen][api-management-add-authorization-server]

Voer een naam en een optionele beschrijving in de velden **naam** en **Beschrijving** . Deze velden worden gebruikt ter identificatie van de server 2.0 OAuth vergunning binnen het exemplaar van de beheer-API. In dit voorbeeld wordt **demo vergunning-server** gebruikt. Later wanneer u een OAuth 2.0-server moet worden gebruikt voor de verificatie voor een API opgeeft, selecteert u deze naam.

Voor de **Client de registratie van URL** -Voer een tijdelijke waarde zoals `http://localhost`.  De **Client de registratie van URL** verwijst naar de pagina die gebruikers kunnen maken en hun eigen accounts configureren voor 2.0 OAuth-providers die ondersteuning bieden voor gebruikersbeheer van accounts gebruiken. In dit voorbeeld gebruikers niet maken en hun eigen accounts configureren zodat een tijdelijke aanduiding wordt gebruikt.

![Vergunning server toevoegen][api-management-add-authorization-server-1]

Vervolgens **URL-autorisatie eindpunt** en **Token eindpunt-URL**opgeven.

![Verificatie server][api-management-add-authorization-server-1a]

Deze waarden kunnen worden opgehaald via de **App eindpunten** van de AAD-toepassing die u hebt gemaakt voor de developer-portal. Voor toegang tot de eindpunten Ga naar het tabblad **configureren** voor de toepassing van AAD en klikt u op **weergave eindpunten**.

![Toepassing][api-management-aad-devportal-application]

![Eindpunten weergeven][api-management-aad-view-endpoints]

Het **OAuth 2.0 vergunning eindpunt** kopiëren en plakken in het tekstvak **URL-autorisatie eindpunt** .

![Vergunning server toevoegen][api-management-add-authorization-server-2]

Het **token eindpunt OAuth 2.0** kopiëren en plakken in het tekstvak **URL van het eindpunt Token** .

![Vergunning server toevoegen][api-management-add-authorization-server-2a]

Niet plakken in het token eindpunt toevoegen een extra body-parameter met de naam **bron** en voor de waarde de **App Id URI** van de toepassing AAD voor de backend-service die is gemaakt bij de Visual Studio-project is gepubliceerd.

![App id-URI][api-management-aad-sso-uri]

Vervolgens geeft u de clientreferenties. Dit zijn de referenties voor de resource die u wilt openen, in dit geval de backend-service.

![Clientreferenties][api-management-client-credentials]

Als u de **Client-Id**, Ga naar het tabblad **configureren** AAD waarin voor de backend-service en kopieert u de **Id van de Client**.

Het **Geheim van de Client** klikken op **Selecteer duur** vervolgkeuzelijst in de sectie **sleutels** en een interval opgeven. In dit voorbeeld wordt 1 jaar gebruikt.

![Client-ID][api-management-aad-client-id]

Klik op **Opslaan** om de configuratie op te slaan en de sleutel weer te geven. 

>[AZURE.IMPORTANT] Noteer deze sleutel. Zodra u het venster Azure Active Directory-configuratie sluit, kan niet de sleutel worden weergegeven.

De sleutel naar het Klembord kopiëren, overschakelen naar de uitgever portal en de sleutel in het tekstvak **Geheim Client** plakken klikt u op **Opslaan**.

![Vergunning server toevoegen][api-management-add-authorization-server-3]

Onmiddellijk na de referenties van de client is een code vergunning verlenen. Deze vergunning code kopiëren en schakelt u terug naar uw Azure AD developer portal toepassing configureren pagina en plak de vergunning verlenen in het veld **URL antwoord** en klik nogmaals op **Opslaan** .

![Antwoord-URL][api-management-aad-reply-url]

De volgende stap is om de machtigingen voor de developer-portal AAD toepassing configureren. Klik op **Machtigingen** en schakel het selectievakje in voor **de Active directory-gegevens lezen**. Klik op **Opslaan** om deze wijziging opslaan en klik vervolgens op **toepassing toevoegen**.

![Machtigingen toevoegen][api-management-add-devportal-permissions]

Klik op het zoekpictogram **APIM** typt in het vak vanaf **APIMAADDemo**selecteren en klikt u op het vinkje op te slaan.

![Machtigingen toevoegen][api-management-aad-add-app-permissions]

**Overgedragen machtigingen** voor **APIMAADDemo** klikt u op het selectievakje in voor **Toegang tot APIMAADDemo**en klik op **Opslaan**. Hierdoor kan de developer portal-toepassing toegang tot de back-end-service.

![Machtigingen toevoegen][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Gebruikersautorisatie OAuth 2.0 voor de Rekenmachine API inschakelen

Nu dat de OAuth 2.0-server is geconfigureerd, kunt u deze in de beveiligingsinstellingen voor uw API. Deze stap wordt geïllustreerd in de video vanaf 14:30.

**API's** in het linkermenu, en klik op **Rekenmachine** om de instellingen weergeven en configureren.

![Rekenmachine-API][api-management-calc-api]

Ga naar het tabblad **beveiliging** , schakel het selectievakje **OAuth 2.0** en de gewenste vergunning server selecteren uit de vervolgkeuzelijst **vergunning server** klikt u op **Opslaan**.

![Rekenmachine-API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Is de Rekenmachine API aanroepen vanuit de developer-portal

Nu dat de vergunning OAuth 2.0 is geconfigureerd op de API, kunnen bewerkingen met succes worden aangeroepen vanuit het developer center. Deze stap wordt geïllustreerd in de video begint om 15:00 uur.

Ga terug naar de bewerking **toevoegen twee gehele getallen** van de Rekenmachine service in de developer-portal en klik op **Probeer het**. Let op het nieuwe item in de sectie **verificatie** voor de autorisatie-server die u zojuist hebt toegevoegd.

![Rekenmachine-API][api-management-calc-authorization-server]

**Autorisatiecode** te selecteren uit de vervolgkeuzelijst vergunning en voert u de referenties van de account die u wilt gebruiken. Als je al bent aangemeld met de account die u niet wordt gevraagd.

![Rekenmachine-API][api-management-devportal-authorization-code]

Klik op **verzenden** en bekijk de **Response-status** van **200 OK** en de resultaten van de bewerking in de inhoud van het antwoord.

![Rekenmachine-API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Configureren van een desktop-toepassing de API aan te roepen

De volgende procedure in de video begint om 16:30 en configureert u een eenvoudige desktop-toepassing de API aan te roepen. De eerste stap is de bureaubladtoepassing in Azure AD registreren en toegang geven tot de map en de backend-service. 18:25 is er een demonstratie van het aanroepen van een bewerking op de Rekenmachine API bureaubladtoepassing.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configureer een beleid voor de validatie van JWT vooraf om aanvragen te verifiëren

De definitieve regeling in de video begint om 20:48 en ziet u hoe het beleid [JWT valideren](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) met vooraf toestemming aanvragen door het valideren van de toegangstokens voor de inkomende aanvragen. Als de aanvraag wordt niet gevalideerd door het beleid JWT valideren, wordt de aanvraag wordt geblokkeerd door de beheer-API en wordt niet doorgegeven aan de back-end.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Zie voor een andere demonstratie van configureren en gebruiken van dit beleid [Cloud Cover aflevering 177: meer API-beheerfuncties](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit op 13:50. Snel vooruitspoelen tot 15:00 voor een overzicht van de beleidsregels die zijn geconfigureerd in de Groepsbeleid-editor en vervolgens op 18:50 voor een demonstratie van het aanroepen van een bewerking van de developer-portal, zowel met als zonder de vereiste vergunning token.

## <a name="next-steps"></a>Volgende stappen
-   Bekijk meer [video's](https://azure.microsoft.com/documentation/videos/index/?services=api-management) over beheer-API.
-   Zie [verbinding maken via VPN of ExpressRoute](api-management-howto-setup-vpn.md)voor [wederzijdse verificatie](api-management-howto-mutual-certificates.md) en voor andere manieren voor het beveiligen van uw back-end-service.

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance
[Uw eerste API beheren]: api-management-get-started.md
