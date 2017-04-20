<properties
    pageTitle="Service Management API gebruikt voor het genereren van HTTP-aanvragen"
    description="Leren werken met beleid voor aanvraag en het antwoord in API-Management services extern aanroepen van de API"
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


# <a name="using-external-services-from-the-azure-api-management-service"></a>Met behulp van externe services uit de Azure API Management-service

Het beleid van Azure API Management-service kan een breed scala aan nuttig werk op basis van uitsluitend op de inkomende aanvraag, het uitgaande antwoord en basisconfiguratie-informatie kunnen doen. Echter kunnen communiceren met externe services API beheer beleid wordt veel meer mogelijkheden.

Eerder hebben we gezien hoe we kunnen werken met de [service voor het vastleggen, bewaken en analytics Azure gebeurtenis Hub](api-management-log-to-eventhub-sample.md). In dit artikel wordt ingegaan op beleid waarmee u kunt communiceren met een externe HTTP gebaseerde service. Dit beleid kunnen worden gebruikt voor het genereren van externe gebeurtenissen of voor het ophalen van informatie die wordt gebruikt voor het bewerken van de oorspronkelijke aanvraag en het antwoord op een bepaalde manier.

## <a name="send-one-way-request"></a>Een-manier-verzoek om te verzenden
Eventueel de eenvoudigste externe interactie is de brand-en-vergeet stijl van verzoek waarmee een externe service om te worden gesteld van een soort belangrijke gebeurtenis. Gebruiken we het beleid van de flow control `choose` voor het detecteren van elk soort voorwaarde dat we geïnteresseerd zijn in en vervolgens, als de voorwaarde is voldaan, kunnen we externe HTTP-aanvragen met behulp van het beleid [een-manier-verzoek om te verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) . Dit kan een aanvraag naar een berichtensysteem zoals Hipchat of vertraging of een e-mailbericht API zoals SendGrid of MailChimp, of voor van kritieke ondersteuningsincidenten als PagerDuty. Al deze systemen voor berichtenafhandeling hebben eenvoudige HTTP-APIs die we gemakkelijk kunt oproepen.

### <a name="alerting-with-slack"></a>Waarschuwen met een toegestane vertraging
In het volgende voorbeeld wordt gedemonstreerd hoe u een bericht verzenden naar een chatroom voor toegestane vertraging als de HTTP-statuscode van antwoord groter dan of gelijk is aan 500 is. Een van 500-bereikfout duidt op een probleem met onze back-end API die zelf kan niet worden opgelost door de client van onze API. Het is meestal een soort interventie van onze kant nodig.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Toegestane vertraging is de notie van inkomende web haken. Bij het configureren van een inkomende web haakje genereert vertraging een speciale URL waarmee u een eenvoudige POST-aanvraag te doen en door te geven van een bericht in het kanaal van de toegestane vertraging. De JSON-instantie die wij maken is gebaseerd op een indeling die is gedefinieerd door de toegestane vertraging.

![Toegestane Web haakje](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Brand is en goed vergeten?
Er zijn bepaalde en nadelen wanneer u een stijl brand en vergeten van de aanvraag. Als voor een of andere reden mislukt de aanvraag vervolgens wordt de fout niet gerapporteerd. In dit geval, is de complexiteit van een secundaire fout rapportage systeem en de kosten van de prestaties van het wachten op het antwoord dat niet gerechtvaardigd. Voor scenario's waar is het van belang om het antwoord vervolgens de verzenden - beleid voor [verbindingsaanvragen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) is een betere optie.

## <a name="send-request"></a>Verzoek om verzenden
De `send-request` beleid maakt met een externe service uit te voeren complexe verwerkingsfuncties retourneren van gegevens naar het management API service die kunnen worden gebruikt voor verdere verwerking.

### <a name="authorizing-reference-tokens"></a>Afgifte verwijzingstokens
Een belangrijke functie van de beheer-API is bescherming van de bronnen van de back-end. Als de vergunning maakt de server die wordt gebruikt door de API [JWT tokens](http://jwt.io/) als onderdeel van de stroom is OAuth2, evenals [Azure Active Directory](../active-directory/active-directory-aadconnect.md) , dan kunt u de `validate-jwt` beleid om de geldigheid van het token. Sommige servers vergunning maken, zogenaamde [verwijzingstokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) zonder een oproep naar de server van de vergunning kan niet worden gecontroleerd.

### <a name="standardized-introspection"></a>Gestandaardiseerde introspection
In het verleden is er geen gestandaardiseerde manier van een verwijzing naar sleutel met een vergunning server is geverifieerd. Echter een onlangs voorgestelde standaard [RFC 7662](https://tools.ietf.org/html/rfc7662) is gepubliceerd door de IETF die definieert hoe een server resource kan de geldigheid van een token.

### <a name="extracting-the-token"></a>Het token ophalen
De eerste stap is het uitpakken van het token van de kop van de vergunning. De waarde van de header moet worden opgemaakt met de `Bearer` autorisatieschema, een spatie en vervolgens de Autorisatietoken aan de hand van [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Helaas zijn er gevallen waarbij het autorisatieschema wordt weggelaten. Als u daarmee rekening houden bij het parseren, we splitsen de header-waarde voor een spatie en selecteert u de laatste tekenreeks in de geretourneerde matrix van tekenreeksen. Dit biedt een oplossing voor onjuist opgemaakte vergunning headers.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>De aanvraag voor validatie
Als we de Autorisatietoken hebt, kunnen we de aanvraag voor het valideren van het token. RFC 7662 roept deze introspection proces en vereist dat u `POST` een HTML-formulier aan de resource introspection. Het HTML-formulier moet ten minste een sleutel/waarde-paar bevatten met de sleutel `token`. Deze aanvraag naar de server vergunning moet ook worden geverifieerd om ervoor te zorgen dat kwaadwillende clients voor geldige tokens kunnen niet gaan sleepnetten.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Controle van de reactie
De `response-variable-name` kenmerk wordt gebruikt om toegang te geven het geretourneerde antwoord. De naam van deze eigenschap kan worden gebruikt als een sleutel in het `context.Variables` woordenlijst voor toegang tot de `IResponse` object.

Van het response-object de instantie kan worden opgehaald en RFC 7622 vertelt ons dat het antwoord een JSON moet en ten minste een eigenschap met de naam bevat `active` wordt een Boole-waarde. Wanneer `active` is ingesteld op true, wordt het token wordt beschouwd als geldig.

### <a name="reporting-failure"></a>Fout melden
We gebruiken een `<choose>` beleid te detecteren als het token ongeldig is en zo ja, kan het resultaat 401.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Aan de hand van [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) beschrijft hoe `bearer` tokens moeten worden gebruikt, wij ook vragen een `WWW-Authenticate` kop met de 401-respons. WWW-verificatie is bedoeld als instructie voor een client op het samenstellen van een aanvraag voor een naar behoren gemachtigde. Vanwege de grote verscheidenheid aan benaderingen die mogelijk zijn met het kader van de OAuth2 is het moeilijk om de benodigde informatie doorgeven. Gelukkig zijn er inspanningen bezig om [clients ontdekken hoe goed aanvragen naar de server van een resource toe te staan](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Definitieve oplossing
Het samenstellen van alle delen, krijgen we het volgende beleid:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Dit is slechts een van de vele voorbeelden van hoe de `send-request` beleid kan worden gebruikt om nuttige externe services integreren in het proces van het aanvragen en antwoorden die door de API-Management-service.

## <a name="response-composition"></a>Samenstelling van de reactie
De `send-request` beleid kan worden gebruikt voor het verbeteren van een primaire aanvraag naar een back endsysteem, zoals u hebt gezien in het vorige voorbeeld, of het kan worden gebruikt als een volledige vervangen voor van de backend-oproep. Met deze techniek kan gemakkelijk maken we samengestelde bronnen die zijn samengevoegd uit meerdere verschillende systemen.

### <a name="building-a-dashboard"></a>Een dashboard maken   
Soms wilt u mogelijk toegankelijk maken van informatie die in meerdere back-end systemen, bijvoorbeeld bestaat, om een dashboard te sturen. De KPI's komen uit alle verschillende back-ends, maar u liever niet voor directe toegang tot deze en het is leuk als alle gegevens kan worden opgehaald in een enkele aanvraag. Enkele van de backend-gegevens moet misschien sommige segmenteren en dobbelstenen en eerst een beetje sanitizing! Samengestelde bron in de cache kunnen zou een handig zijn om back-end reduceren zoals u weet, hebben gebruikers een gewoonte van de toets F5 hammering om te zien of hun underperforming metrics kan veranderen.    

### <a name="faking-the-resource"></a>De bron faking
De eerste stap bij het samenstellen van onze dashboard resource is een nieuwe bewerking in de beheer-API uitgever portal configureren. Dit is een tijdelijke aanduiding voor bewerking gebruikt voor het configureren van ons beleid voor het opbouwen van onze dynamische bron voor compositie.

![Dashboard-bewerking](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Maken van de aanvragen
Zodra de `dashboard` bewerking is gemaakt is een beleid dat specifiek voor die bewerking kunt configureren. 

![Dashboard-bewerking](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

De eerste stap is een query-parameters ophalen uit de inkomende aanvraag zodat we naar onze back-end doorsturen kunnen. In dit voorbeeld is onze dashboard informatie op basis van een periode waarin een heeft daarom een `fromDate` en `toDate` parameter. Gebruiken we de `set-variable` beleid op gegevens ophalen uit de aanvraag-URL.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Zodra deze informatie kunnen we aanvragen voor alle back-end systemen. Elke aanvraag wordt een nieuwe URL met de informatie over de parameters en roept de desbetreffende server en het antwoord wordt opgeslagen in een variabele met de context.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Deze aanvragen worden uitgevoerd in de volgorde die niet ideaal is. In een toekomstige versie zullen we introductie van een nieuw beleid voor `wait` waarmee al deze verzoeken worden parallel uitgevoerd.

### <a name="responding"></a>Reageren

Om het samengestelde antwoord maken gebruiken we de [terug-reactie](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) beleid. De `set-body` element kan een expressie gebruiken om samen te stellen een nieuwe `JObject` met alle onderdeel representaties ingesloten als eigenschappen.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Het volledige beleid ziet er als volgt uit:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

In de configuratie van de tijdelijke aanduiding voor betekent bewerking die kunnen wij het dashboard resource in de cache opgeslagen gedurende ten minste een uur omdat wij begrijpen dat de aard van de gegevens configureren dat zelfs als het een uur verouderd, dat deze nog niet in voldoende kracht om waardevolle informatie aan de gebruikers over te brengen.

## <a name="summary"></a>Samenvatting
Azure API Management-service biedt flexibele beleidsregels die selectief kunnen worden toegepast op HTTP-verkeer en samenstelling van de backend-services kan. Of u wilt uw gateway API met waarschuwingen functies, verificatie, validatie mogelijkheden te verbeteren of nieuwe samengestelde resources op basis van meerdere back-end services maken het `send-request` en het bijbehorende beleid opent een wereld van mogelijkheden.

## <a name="watch-a-video-overview-of-these-policies"></a>Bekijk een video-overzicht van dit beleid
Voor meer informatie over het [verzenden een-manier aanvraag](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [verzoek om verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)en [retour reactie](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) beleid die in dit artikel, neem de volgende video bekijken.

> [AZURE.VIDEO send-request-and-return-response-policies]
