<properties
    pageTitle="Aangepaste caching in Azure API beheer"
    description="Leren hoe u de items in de cache door de sleutel in Azure API beheer"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="custom-caching-in-azure-api-management"></a>Aangepaste caching in Azure API beheer
Azure API Management-service heeft ingebouwde ondersteuning voor [HTTP-antwoord in cache opslaan](api-management-howto-cache.md) met behulp van de bron-URL als de sleutel. De sleutel kan worden gewijzigd door aanvraagheaders met behulp van de `vary-by` eigenschappen. Dit is handig voor het hele HTTP-antwoorden (aka representaties) cache, maar soms is het handig om gewoon cache een gedeelte van een weergave. Het nieuwe beleid voor [cache-lookup-waarde](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) en [cache-winkel-waarde](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) bieden de mogelijkheid voor het opslaan en ophalen van willekeurige stukjes gegevens vanuit beleidsdefinities. Deze mogelijkheid ook wordt waarde toegevoegd aan het beleid eerder geïntroduceerd [verzoek om te verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) omdat u kunt nu antwoorden van services voor externe cache.

## <a name="architecture"></a>Architectuur  
API-Management-service maakt gebruik van een gedeelde huurder gegevenscache zodat als u tot schalen meerdere eenheden, krijgt u nog steeds toegang tot dezelfde gegevens in de cache opgeslagen. Als u werkt met een distributie met meerdere regio zijn er echter onafhankelijk caches in elke regio. Vanwege dit is het belangrijk dat de cache niet behandelen als een gegevensarchief waarin het is de enige bron van een stukje informatie. Als u hebt en later besloten om te profiteren van de implementatie van meerdere landen/regio, vervolgens klanten met gebruikers die onderweg geen toegang meer tot deze gegevens in de cache.

## <a name="fragment-caching"></a>Gefragmenteerde caching
Er zijn bepaalde gevallen waarin antwoorden worden geretourneerd bevatten een gedeelte van de gegevens die is duur om te bepalen en een redelijke hoeveelheid tijd nog up-to-date blijft. Een voorbeeld, kunt u een service die is ontwikkeld door een luchtvaartmaatschappij die informatie over reserveringen flight, flight status, enzovoort. Als de gebruiker lid van de luchtvaartmaatschappijen punten programma is, moet zij ook informatie met betrekking tot hun huidige status en geaccumuleerde kilometers. Kan deze gebruiker gerelateerde gegevens worden opgeslagen in een ander systeem kan, maar het wenselijk zijn opgenomen in antwoorden geretourneerd over de status van de vlucht en reserveringen. U kunt dit doen via een proces genaamd gefragmenteerde caching. De primaire voorstelling kan worden opgehaald uit de oorspronkelijke server met behulp van een soort token om aan te geven waarin de gebruiker informatie moet worden ingevoegd. 

U kunt de volgende JSON-reactie van een end-API.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

En een secundaire bron op `/userprofile/{userid}` dat lijkt,

     { "username" : "Bob Smith", "Status" : "Gold" }

We moeten bepalen de juiste gebruikersgegevens op te nemen, die de gebruiker te identificeren. Dit mechanisme is uitvoering afhankelijk. Als voorbeeld, ik gebruik de `Subject` claimen van een `JWT` token. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

We slaan dit `enduserid` waarde in een variabele context voor later gebruik. De volgende stap is om te bepalen als een eerdere aanvraag al de gegevens van de gebruiker opgehaald en in de cache opgeslagen. Hiervoor gebruiken we de `cache-lookup-value` beleid.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Als er geen vermelding in de cache die overeenkomt met de waarde van de sleutel en klik op Nee `userprofile` variabele context wordt gemaakt. We controleren of het succes van de zoekactie met behulp van de `choose` stroom beleid bepalen.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Als de `userprofile` variabele context bestaat niet, dan gaan we hebt om een HTTP-aanvraag te halen.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

We gebruiken de `enduserid` om de URL van de resource gebruiker profiel samen te stellen. Als we het antwoord hebt, kunnen we trekken van de platte tekst van het antwoord en opslaan in een variabele context.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Om te voorkomen dat we met dit HTTP-verzoek om weer te maken, wanneer de gebruiker een andere aanvraag indient, kunnen we het profiel in de cache worden opgeslagen.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Opgeslagen in de cache met exact dezelfde sleutel die we oorspronkelijk probeerde te halen met de waarde. De duur die wij kiezen voor het opslaan van de waarde moet worden gebaseerd op hoe vaak worden de wijzigingen en hoe tolerant gebruikers om actuele informatie. 

Het is belangrijk om te realiseren dat ophalen uit de cache nog steeds een out-of-process, netwerkaanvraag en mogelijk nog steeds tientallen milliseconden op de aanvraag toevoegen kunt. De voordelen komen bij het vaststellen van dat de profielgegevens aanzienlijk langer duurt dan die door hoeft te database query's en statistische gegevens van meerdere back-ends.

De laatste stap in het proces is het bijwerken van het geretourneerde antwoord met onze informatie over gebruikersprofielen.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Ik heb gekozen voor de aanhalingstekens opnemen als onderdeel van het token dat zelfs wanneer het vervangen niet optreden, het antwoord nog steeds geldig JSON is. Dit was voornamelijk foutopsporing gemakkelijker maken.

Als u al deze stappen samen combineert, is het eindresultaat een beleid dat eruit als een ziet.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Deze benadering in het cachegeheugen wordt voornamelijk gebruikt in websites waarbij HTML bestaat op de server, zodat deze kan worden weergegeven als een enkele pagina. Het kan echter ook zijn handig in API's waar clients niet mogelijk client zijde HTTP caching of het wenselijk is niet te plaatsen dat de verantwoordelijkheid van de client.

Dit dezelfde soort fragment opslaan in de cache kan worden uitgevoerd op de backend-webservers met behulp van een bestand Vgx. caching server, echter met behulp van de API-Management-service voor het uitvoeren van deze werkzaamheden is handig wanneer de cache fragmenten afkomstig zijn uit verschillende back-ends dan de primaire antwoorden.

## <a name="transparent-versioning"></a>Transparante versiebeheer
Het is gebruikelijk voor meerdere versies van de andere implementatie van een API op elk gewenst moment worden ondersteund. Dit is misschien voor de ondersteuning van verschillende omgevingen, zoals dev, testen, productie, enz., of kan zijn voor de ondersteuning van oudere versies van de API om tijd voor de consument om te migreren naar nieuwere versies API te geven. 

Een benadering van deze verwerking hoeft de client-ontwikkelaars te wijzigen van de URL's van `/v1/customers` te `/v2/customers` opslaan in de profielgegevens van de consument welke versie van de API die zij op dit moment willen gebruiken en de juiste back-end URL aanroepen. Om te bepalen van de juiste back-end-URL voor het aanroepen van een bepaalde client, is het noodzakelijk bepaalde configuratiegegevens opvragen. Door deze configuratiegegevens caching, kunnen we de prestaties van de deze zoekactie te minimaliseren.

De eerste stap is om te bepalen welke identificatie die wordt gebruikt voor het configureren van de gewenste versie. Ik wilde in dit voorbeeld wordt de versie op de productcode abonnement koppelen. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Dan doen we een cache zoeken om te zien als we al hebt opgehaald versie van de gewenste client.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Vervolgens controleren we om te zien als we niet in de cache vinden heeft.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Als we niet en we gaan en deze op te halen.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Pak de hoofdtekst van het antwoord in het antwoord.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Opslaan in de cache voor toekomstig gebruik.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

En ten slotte selecteert u de versie van de service die door de client de gewenste de back-end-URL.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Het beleid volledig is als volgt.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Om de consumenten ongemerkt bepalen welke versie van de backend wordt gebruikt door clients bijwerken en implementeren van clients zonder API is een elegante oplossing die voldoet aan veel API versie betreft.

## <a name="tenant-isolation"></a>Isolatie van de huurder

In grotere, huurder met meerdere implementaties maken sommige bedrijven afzonderlijke groepen van huurders op verschillende implementaties van de back-end hardware. Dit verkleint het aantal klanten die erdoor worden beïnvloed door de hardware op de backend. U kunt ook nieuwe versies van software te worden uitgerold in fasen. Als deze back-end-architectuur worden transparant voor consumenten API. Dit kan worden bereikt op een vergelijkbare manier in transparante versiebeheer omdat het is gebaseerd op dezelfde techniek van het manipuleren van de backend URL configuratiestatus per API-sleutel gebruikt.  

In plaats van een voorkeurs-versie van de API voor elke sleutel abonnement, zou u een aanduiding dat een huurder bij de Hardwaregroep toegewezen hoort terug. Deze id kan worden gebruikt om de juiste back-end URL samen te stellen.

## <a name="summary"></a>Samenvatting
De vrijheid om de cache Azure API beheer gebruiken voor het opslaan van elk type gegevens maakt een efficiënte toegang tot de configuratiegegevens die kan invloed hebben op de manier waarop die een binnenkomende aanvraag wordt verwerkt. Het kan ook worden gebruikt voor het opslaan van gegevens fragmenten die worden geretourneerd van een API-end antwoorden kunnen verbeteren.

## <a name="next-steps"></a>Volgende stappen
Geef ons uw feedback in de Disqus-thread voor dit onderwerp als er andere scenario's waarin dit beleid hebt ingeschakeld, of als er scenario's u wilt bereiken, maar niet verplicht zijn op dit moment mogelijk doen.
