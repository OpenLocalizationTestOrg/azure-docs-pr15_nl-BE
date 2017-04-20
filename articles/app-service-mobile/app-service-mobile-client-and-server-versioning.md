<properties
  pageTitle="Versiebeheer voor client en server SDK in Mobile Apps en mobiele Services | Azure App-Service"
  description="Lijst met client-SDK's en compatibiliteit met server SDK versies voor mobiele Services en Azure mobiele Apps"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Client en server versies in Mobile Apps en mobiele Services

De meest recente versie van Azure mobiele diensten is de functie van de **Mobiele Apps** van Azure App-Service.

De mobiele Apps client en server SDK's oorspronkelijk zijn gebaseerd op die van mobiele Services, maar ze zijn *niet* compatibel met elkaar.
Dat wil zeggen, moet u een *Mobiele Apps* client SDK met een *Mobiele Apps* server SDK en ook voor *Mobiele diensten*. Dit contract wordt afgedwongen via een speciale headerwaarde die door de client en server SDK's, `ZUMO-API-VERSION`.

Opmerking: wanneer dit document naar een *Mobiele Services* backend verwijst, het noodzakelijkerwijs hoeft niet te worden gehost op mobiele diensten. Is het nu mogelijk voor het migreren van een mobiele service uit te voeren op App Service code ongewijzigd, maar de service zou nog steeds met *Mobiele Services* SDK versies.

Voor meer informatie over het migreren naar App Service zonder codewijzigingen, Zie het artikel [een mobiele Service met Azure App Service migreren].

## <a name="header-specification"></a>Header-specificatie

De sleutel `ZUMO-API-VERSION` kan worden opgegeven in de HTTP-header of de query-tekenreeks. De waarde is een tekenreeks in de formulier- **x.y.z**.

Bijvoorbeeld:

Https://service.azurewebsites.net/tables/TodoItem ophalen

KOPTEKSTEN: ZUMO API VERSIE: 2.0.0

Https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0 boeken

## <a name="opting-out-of-version-checking"></a>Kiest uit de versie controleren

U kunt ervoor kiezen niet de versie controleren door de waarde **true** voor het instellen van **MS_SkipVersionCheck**app. Geef dit in uw web.config of in de sectie instellingen voor toepassing van de portal Azure.

> [AZURE.NOTE] Er zijn een aantal gedragswijzigingen tussen Mobile Services en Mobile Apps, met name op het gebied van de off line synchronisatie, verificatie en push-meldingen. U moet niet alleen deelnemen aan versie controleren na het volledige testen om ervoor te zorgen dat deze wijzigingen niet de functionaliteit van uw app doen breken.

## <a name="summary-of-compatibility-for-all-versions"></a>Overzicht van de compatibiliteit voor alle versies

In onderstaande tabel ziet u de compatibiliteit tussen alle client en server typen. Een back-end is ingedeeld als mobiele **diensten** of mobiele **Apps** , gebaseerd op de server SDK die wordt gebruikt.

|                           | **Mobiele Services** Node.js of .NET | **Mobiele toepassingen** Node.js of .NET |
| ----------                | -----------------------             |   ----------------              |
| [Services voor mobiele clients] | OK                                  | Fout\*                         |
| [Apps voor mobiele clients]     | Fout\*                             | OK                              |

\*Dit kan worden beheerd door de **MS_SkipVersionCheck**op te geven.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobiele Services-client en server

De client-SDK's in de onderstaande tabel zijn compatibel met **Mobiele Services**.

Opmerking: sturen de Mobile Services client SDK's *niet* de waarde van een header voor `ZUMO-API-VERSION`. Als de service deze kop- of query string-waarde ontvangt, een fout, tenzij u expliciet uit zoals hierboven beschreven hebt gekozen.

### <a name="MobileServicesClients"></a>Mobiele *Services* client SDK 's

| Client platform                   | Versie                                                                   | De waarde van de header versie |
| -------------------               | ------------------------                                                  | -------------------  |
| Beheerde client (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n.v.t.                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | n.v.t.                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | n.v.t.                  |
| HTML-CODE                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n.v.t.     |

### <a name="mobile-services-server-sdks"></a>Mobiele *Services* server SDK 's

| Server-platform  | Versie                                                                                                        | Geaccepteerde versiekop |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [WindowsAzure.MobileServices.Backend.* versie 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Geen koptekst versie** |
| Node.js          | (binnenkort beschikbaar)                        | **Geen koptekst versie** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Gedrag van mobiele Services backends

| API-ZUMO-VERSIE | Waarde van MS_SkipVersionCheck | Reactie |
| ---------------- | ---------------------------- | -------- |
| Niet opgegeven    | Alle                          | 200 - OK |
| Een willekeurige waarde        | True                         | 200 - OK |
| Een willekeurige waarde        | ONWAAR of niet opgegeven          | 400 - Ongeldig verzoek |

## <a name="2.0.0"></a>Azure Apps Mobile client en server

### <a name="MobileAppsClients"></a>Mobiele *Apps* client SDK 's

Versiecontrole is gestart met de volgende versies van de client-SDK voor **Azure Mobile Apps**geïntroduceerd:

| Client platform                   | Versie                   | De waarde van de header versie |
| -------------------               | ------------------------  | -----------------    |
| Beheerde client (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobiele *Apps* server SDK 's

Versiecontrole is opgenomen in de volgende versies van server SDK:

| Server-platform  | SDK                                                                                                        | Geaccepteerde versiekop |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [Azure mobile apps)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Gedrag van mobiele Apps backends

| API-ZUMO-VERSIE | Waarde van MS_SkipVersionCheck | Reactie |
| ---------------- | ---------------------------- | -------- |
| x.y.z of Null    | True                         | 200 - OK |
| Null             | ONWAAR of niet opgegeven          | 400 - Ongeldig verzoek |
| 1.x.y            | ONWAAR of niet opgegeven          | 400 - Ongeldig verzoek |
| 2.0.0-2.x.y      | ONWAAR of niet opgegeven          | 200 - OK |
| 3.0.0-3.x.y      | ONWAAR of niet opgegeven          | 400 - Ongeldig verzoek |


## <a name="next-steps"></a>Volgende stappen

- [Een mobiele Service migreren naar Azure App-Service]


[Services voor mobiele clients]: #MobileServicesClients
[Apps voor mobiele clients]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Een mobiele Service migreren naar Azure App-Service]: app-service-mobile-migrating-from-mobile-services.md

