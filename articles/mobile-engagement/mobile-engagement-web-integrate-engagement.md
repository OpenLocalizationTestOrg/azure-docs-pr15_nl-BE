<properties
    pageTitle="Azure SDK van Mobile Engagement Web integratie | Microsoft Azure"
    description="De meest recente updates en procedures voor de Azure Mobile Engagement Web SDK"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Azure Mobile Engagement integreren in een webtoepassing

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

De procedures in dit artikel beschrijven de eenvoudigste manier om te activeren voor analyse en monitoring functies in Azure Mobile Engagement in uw webtoepassing.

Volg de stappen voor het activeren van de log-rapporten die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, crashes en technicals. Afhankelijk van de toepassing statistieken, zoals gebeurtenissen en fouten, taken, moet u log-rapporten handmatig activeren met behulp van de API Azure Mobile Engagement. Informatie over [het gebruik van geavanceerde mobiele aanstelling tagging API in een webtoepassing](mobile-engagement-web-use-engagement-api.md)voor meer informatie.

## <a name="introduction"></a>Inleiding

[De betrokkenheid van Azure mobiele Web SDK te downloaden](http://aka.ms/P7b453).
De Mobile Engagement Web SDK wordt geleverd als een JavaScript-bestand van één azure-engagement.js die u wilt opnemen in elke pagina van uw site of web-toepassing.

> [AZURE.IMPORTANT] Voordat u dit script uitvoert, moet u een fragment script of code die u schrijft voor het configureren van Mobile Engagement voor de toepassing uitvoeren.

## <a name="browser-compatibility"></a>Browsercompatibiliteit

Native JSON coderen en decoderen, naast het interdomein-AJAX-aanvragen (afhankelijk van de specificatie W3C CORS) maakt gebruik van de Mobile Engagement Web SDK. Het is compatibel met de volgende browsers:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chroom 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Betrokkenheid bij de mobiele configureren

Een script schrijven dat wordt gemaakt van een algemene `azureEngagement` JavaScript-object, zoals in het volgende voorbeeld. Omdat uw site kan meerdere pagina's, in dit voorbeeld wordt ervan uitgegaan dat dit script in elke pagina is opgenomen. In dit voorbeeld wordt het JavaScript-object met de naam `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

De `connectionString` waarde voor uw toepassing wordt weergegeven in de portal Azure.

> [AZURE.NOTE] `appVersionName`en `appVersionCode` zijn optioneel. We raden echter aan dat u deze zo configureren dat analytics versie-informatie kunnen verwerken.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Mobile Engagement scripts opnemen in uw pagina 's
Mobile Engagement scripts toevoegen aan uw pagina's in een van de volgende manieren:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Of dit:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias

Nadat het script Mobile Engagement Web SDK is geladen, wordt de alias **betrokkenheid bij de** toegang tot de SDK-API's gemaakt. U kunt deze alias niet gebruiken voor het definiëren van de SDK-configuratie. Deze alias wordt gebruikt als een verwijzing in deze documentatie.

Houd er rekening mee dat als de standaardalias met andere globale variabele van uw pagina conflicteert, u deze in de configuratie als volgt desgewenst kunt voordat u de SDK van Mobile Engagement webpagina laadt:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Eenvoudige rapportage

Eenvoudige rapportage in Mobile Engagement dekt sessie niveau statistieken, zoals statistieken over gebruikers, sessies, activiteiten en crashes.

### <a name="session-tracking"></a>Bijhouden van sessiegegevens

Een Mobile Engagement sessie is onderverdeeld in een reeks van activiteiten, elk worden aangeduid met een naam.

In een klassieke website, is het raadzaam dat u op elke pagina van uw site een andere activiteit declareren. Voor een website of webtoepassing toepassing waarin de huidige pagina nooit verandert, kunt u de activiteiten bijhouden op een kleinere schaal, zoals in de pagina.

In beide gevallen belt om te starten of het wijzigen van de huidige gebruikersactiviteit, de `engagement.agent.startActivity` functie. Bijvoorbeeld:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

De betrokkenheid bij de mobiele server wordt automatisch beëindigd een sessie openen binnen drie minuten nadat de pagina wordt gesloten.

U kunt ook u kunt een sessie beëindigen handmatig door het aanroepen van `engagement.agent.endActivity`. Hiermee stelt u de huidige gebruikersactiviteit op 'Niet-actief'.  De sessie 10 seconden later wordt beëindigd, tenzij een nieuwe oproep tot `engagement.agent.startActivity` de sessie hervatten.

U kunt de vertraging van 10 seconden in het object global engagement als volgt configureren:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] U kunt niet gebruiken `engagement.agent.endActivity` in de `onunload` terugbellen omdat u niet AJAX oproepen in dit stadium.

## <a name="advanced-reporting"></a>Geavanceerde rapportage

Als u rapporteren specifieke gebeurtenissen, fouten en taken wilt, moet u eventueel gebruikmaken van de API van Mobile Engagement. U hebt toegang tot de API Mobile Engagement tot en met de `engagement.agent` object.

U kunt toegang tot alle van de geavanceerde mogelijkheden van Mobile Engagement in de API van Mobile Engagement. De API wordt beschreven in het artikel in [het gebruik van geavanceerde mobiele aanstelling tagging API in een webtoepassing](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>De URL's gebruikt voor AJAX-aanroepen aanpassen

U kunt URL's die de SDK van Mobile Engagement Web wordt gebruikt. Bijvoorbeeld om te definiëren van de URL voor logboekregistratie (de SDK-eindpunt voor aanmelding), kunt u de configuratie zo overschrijven:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Als uw URL functies geven als een tekenreeks die begint resultaat met `/`, `//`, `http://`, of `https://`, het standaardschema niet wordt gebruikt. Standaard, de `https://` wordt gebruikt als schema voor deze URL's. Als u het standaardschema aanpassen, overschrijven de configuratie, als volgt:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
