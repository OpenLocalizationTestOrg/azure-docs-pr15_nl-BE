<properties 
    pageTitle="Het gebruik van de API-inspecteur traceren roept in Azure API beheer" 
    description="Informatie over het traceren met behulp van de API-inspecteur in Azure API beheer." 
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

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Het gebruik van de API-inspecteur traceren roept in Azure API beheer

Beheer-API biedt een API-inspecteur hulpmiddel om u te helpen bij het opsporen van fouten en problemen met de API's. De inspecteur API programmatisch kan worden gebruikt en kan ook rechtstreeks vanaf de portal voor ontwikkelaars worden gebruikt. 

Naast de transacties traceren traceert API Inspector ook [beleidsexpressie](https://msdn.microsoft.com/library/azure/dn910913.aspx) beoordelingen. Zie voor een demonstratie, [wolk dekken aflevering 177: meer API-beheerfuncties](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruitspoelen tot 21:00.

Deze handleiding bevat een overzicht van het gebruik van de API-inspecteur.

>[AZURE.NOTE] API-inspecteur sporen worden alleen gegenereerd en beschikbaar gesteld voor aanvragen met abonnement toetsen die bij de [administrator](api-management-howto-create-groups.md) -account horen.

## <a name="trace-call"></a> Gebruik API inspecteur voor het traceren van een oproep

Voor het gebruik van de API-inspecteur toevoegen een **ocp-apim-trace: true** koptekst voor uw gesprek bewerking aanvragen en vervolgens downloaden en controleren van de tracering met behulp van de URL die wordt aangegeven door de **ocp apim-trace locatie** response-header. Dit kan via programmacode worden uitgevoerd en kan worden uitgevoerd direct vanaf de portal voor ontwikkelaars.

Deze zelfstudie laat zien hoe de inspecteur API trace bewerkingen met de eenvoudige rekenmachine-API die is geconfigureerd in het [beheren van uw eerste API](api-management-get-started.md) begonnen zelfstudie ophalen gebruiken. Als u nog niet hebt voltooid die zelfstudie alleen duurt het enkele seconden duren voordat de eenvoudige rekenmachine API importeren of kunt u een andere API van uw keuze zoals de Echo-API. Elk exemplaar van de service Management API is vooraf geconfigureerd met een Echo-API die kan worden gebruikt om te experimenteren met en informatie over het beheer van de API. De Echo retourneert de API weer welke input is verzonden. U kunt HTTP-woord aanroepen om het te gebruiken, en de resulterende waarde is gewoon wat u hebt verzonden. 



Klik op **developer-portal** in de klassieke Azure-Portal voor uw service Management API om te beginnen. Activiteiten kunnen rechtstreeks vanuit de developer-portal die is een handige manier om te bekijken en testen van de activiteiten van een API worden aangeroepen.

>Als u nog een exemplaar van de service Management API hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

![Beheer-API developer-portal][api-management-developer-portal-menu]

**API's** op in het bovenste menu, en klik op **Eenvoudige rekenmachine**.

![Echo-API][api-management-api]

Klik op het **proberen** om te proberen de bewerking van **twee gehele getallen toevoegen** .

![Probeer het nu][api-management-open-console]

Standaard parameterwaarden behouden en de sleutel abonnement voor het product dat u wilt gebruiken uit de **sleutel abonnement** vervolgkeuzelijst selecteren.

Kop is al in de developer-portal de **Ocp-Apim-Trace** standaard ingesteld op **true**. Deze header wordt geconfigureerd, of er een tracering wordt gegenereerd.

![Verzenden][api-management-http-get]

Klik op **verzenden** om de bewerking te starten.

![Verzenden][api-management-send-results]

In het antwoord headers een **ocp apim-trace locatie** met een waarde, zoals in het volgende voorbeeld worden.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

De trace kan worden gedownload vanaf de opgegeven locatie en gecontroleerd, zoals in de volgende stap.

## <a name="inspect-trace"> </a>De trace controleren

Als u wilt de waarden in de trace bekijken, het traceringsbestand van de **ocp apim-trace locatie** -URL te downloaden. Het is een tekstbestand in JSON-indeling en bevat items zoals in het volgende voorbeeld.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Volgende stappen

-   Bekijk een demonstratie van het traceren van expressies in beleid [Cloud Cover aflevering 177: meer API-beheerfuncties](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Vooruitspoelen tot 21:00 voor de demo bekijken.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Aan de slag met Azure API beheer]: api-management-get-started.md
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 