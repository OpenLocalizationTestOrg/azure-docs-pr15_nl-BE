<properties
    pageTitle="Geavanceerde aanvraag met Azure API beheer beperken"
    description="Informatie over het maken en toepassen van flexibele quota en snelheidsbeperking beleid met Azure API Management."
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


# <a name="advanced-request-throttling-with-azure-api-management"></a>Geavanceerde aanvraag met Azure API beheer beperken

Kunnen binnenkomende aanvragen te beperken is een belangrijke rol van Azure API beheer. Hetzij door het aantal aanvragen of het totaal aanvragen/overgebrachte te beheren, kan beheer-API API providers hun API's beschermen tegen misbruik en waarde voor andere API product niveaus maken.

## <a name="product-based-throttling"></a>Op basis van product beperken
Op dit moment, de snelheid beperken zijn mogelijkheden beperkt tot binnen het bereik van een bepaald Product abonnement (in feite een sleutel genoemd), gedefinieerd in de beheer-API uitgever portal. Dit is handig voor de API-provider beperkingen toepassen op de ontwikkelaars die hun API gebruiken hebt aangemeld, echter dat niet helpt, bijvoorbeeld in afzonderlijke eindgebruikers van de API te beperken. Het is mogelijk dat voor één gebruiker van de ontwikkelaar van de toepassing het hele quotum verbruiken en vervolgens te voorkomen dat andere klanten van de ontwikkelaar de toepassing gebruiken. Verschillende klanten, die een groot aantal aanvragen genereren kunnen kunnen ook toegang tot occasionele gebruikers te beperken.

## <a name="custom-key-based-throttling"></a>Aangepaste sleutel gebaseerd beperken
Het nieuwe [tarief limiet door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) en [quota door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) beleid aanzienlijk meer flexibele oplossing bieden voor beheer van netwerkverkeer. Deze nieuwe beleidsregels kunnen u expressies voor het aanduiden van de sleutels die worden gebruikt voor het bijhouden van verkeer gebruik definiëren. De manier waarop die dit werkt is easiest geïllustreerd met een voorbeeld. 

## <a name="ip-address-throttling"></a>Beperking van IP-adres
Het volgende beleid beperken een enkele client-IP-adres tot slechts 10 gesprekken per minuut, met een totaal van 1.000.000 oproepen en 10.000 kB bandbreedte per maand. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Als u alle clients op het Internet een uniek IP-adres gebruikt, is dit mogelijk een effectieve manier van gebruik door de gebruiker beperken. Het is echter zeer waarschijnlijk dat meerdere gebruikers wordt één openbaar IP-adres door ze toegang tot het Internet via een NAT-apparaat te delen. Ondanks dit voor API's die niet-geverifieerde toegang tot het `IpAddress` misschien de beste optie.

## <a name="user-identity-throttling"></a>Gebruiker identiteit beperken
Als een gebruiker is geverifieerd en vervolgens een bandbreedteregeling sleutel worden gegenereerd op basis van informatie die uniek een die gebruiker.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

In dit voorbeeld is de verificatieheader uitpakken, converteren naar `JWT` object en het onderwerp van het token voor het identificeren van de gebruiker en als de snelheidsbeperking sleutel gebruiken. Als de identiteit van de gebruiker is opgeslagen in de `JWT` als een van de andere vervolgens beweert dat de waarde kan worden gebruikt in plaats daarvan.

## <a name="combined-policies"></a>Gecombineerde beleid
Hoewel het nieuwe beleid voor bandbreedteregeling meer controle dan het bestaande beleid voor bandbreedteregeling bieden, is er nog steeds waarde beide mogelijkheden te combineren. Beperken door abonnement-productcode ([limiet call rate abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) en [gebruiksgegevens door abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is een fantastische manier om te schakelen van een API monetizing door het in rekening brengen op basis van het gebruik van niveaus. De betere resultaten korrelig controle kunnen beperken door de gebruiker is een aanvulling en voorkomt dat één gebruiker gedrag tasten de ervaring van een andere. 

## <a name="client-driven-throttling"></a>Client gestuurd beperken
Wanneer u de bandbreedteregeling sleutel is gedefinieerd met een [beleidsexpressie](https://msdn.microsoft.com/library/azure/dn910913.aspx), is de API-provider die is hoe de beperking ligt binnen het bereik te kiezen. Echter een ontwikkelaar wilt bepalen hoe ze tarief limiet voor hun eigen klanten. Dit kan worden ingeschakeld door de API-provider door de invoering van een aangepaste koptekst om de ontwikkelaar van de clienttoepassing communiceren de sleutel voor de API.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

Hiermee kunt de ontwikkelaar van de clienttoepassing kiezen hoe ze de snelheidsbeperking sleutel maken. Met een beetje draaien kan een client-ontwikkelaar door sleutelsets toewijzen aan gebruikers en het sleutelgebruik draaien hun eigen niveaus tarief maken.

## <a name="summary"></a>Samenvatting
Azure API beheer biedt tarief en prijsopgave bandbreedteregeling zowel beschermen en waarde toevoegen aan uw service API. Het nieuwe bandbreedteregeling beleid met aangepaste bereikregels kunnen u betere resultaten korrelig controle over het betreffende beleid zodat uw klanten nog beter toepassingen te bouwen. De voorbeelden in dit artikel voorbeelden van het gebruik van deze nieuwe beleidsregels door productie-snelheidsbeperking sleutels met client-IP-adressen, gebruikersidentiteit en waarden van de client gegenereerd. Er zijn echter veel andere delen van het bericht dat kan worden gebruikt zoals gebruikersagent, fragmenten van URL-pad, de grootte van het bericht.

## <a name="next-steps"></a>Volgende stappen
Geef ons uw feedback in de Disqus-thread voor dit onderwerp. Het is geweldig voor informatie over andere mogelijke waarden die een logische keuze in de scenario's zijn.

## <a name="watch-a-video-overview-of-these-policies"></a>Bekijk een video-overzicht van dit beleid
Voor meer informatie over het beleid voor [snelheid beperken door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) en [quota door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) die in dit artikel, neem de volgende video bekijken.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
