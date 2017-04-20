<properties 
    pageTitle="Toepassing inzichten in Java webproject oplossen" 
    description="Problemen oplossen met - controle live Java apps met inzichten van toepassing." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Problemen oplossen en Q and A voor de toepassing van inzichten voor Java

Vragen of problemen met de [Visual Studio-toepassing inzichten in Java][java]? Hier volgen enkele tips.


## <a name="build-errors"></a>Fouten maken

*Ik krijg build of checksum validatiefouten in Eclips, wanneer de toepassing inzichten SDK via Maven of Gradle toe te voegen.*

* Als de afhankelijkheid <version> element is met behulp van een patroon met jokertekens (bijvoorbeeld (Maven) `<version>[1.0,)</version>` of (Gradle) `version:'1.0.+'`), kunt u in plaats daarvan een specifieke versie opgeven zoals `1.0.2`. Zie de [release-opmerkingen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) voor de meest recente versie.

## <a name="no-data"></a>Geen gegevens 

*Ik toepassing inzichten toegevoegd en Mijn app hebt uitgevoerd, maar ik heb nooit gezien de gegevens in de portal.*

* Wacht een minuut en klik op vernieuwen. De grafieken regelmatig zelf vernieuwen, maar u kunt ook handmatig vernieuwen. Het vernieuwingsinterval is afhankelijk van het tijdsbereik van het diagram.
* Controleer of u een sleutel instrumentation is gedefinieerd in het bestand ApplicationInsights.xml (in de map resources in het project hebt)
* Controleer of er geen `<DisableTelemetry>true</DisableTelemetry>` knooppunt in het XML-bestand.
* Mogelijk moet u in uw firewall, open TCP-poorten 80 en 443 voor uitgaand verkeer op dc.services.visualstudio.com. Zie de [volledige lijst met firewalluitzonderingen](app-insights-ip-addresses.md)
* Start in de Microsoft Azure board, bekijk het overzicht van de status van service. Als er enige waarschuwing aanwijzingen, wacht u totdat ze hebt teruggezonden naar OK sluit en opnieuw uw toepassing inzichten toepassing blade opent.
* Registratie van gegevens in het consolevenster IDE inschakelen door het toevoegen van een `<SDKLogger />` element onder het hoofdknooppunt in het bestand ApplicationInsights.xml (in de map resources in uw project) en op posten die worden voorafgegaan door [fout].
* Zorg ervoor dat het juiste ApplicationInsights.xml-bestand is geladen door de SDK voor Java, door te kijken van de console-Uitvoerberichten voor een instructie "configuratiebestand is met succes gevonden".
* Als het config-bestand niet wordt gevonden, controleert de output berichten om te zien waar het config-bestand wordt gezocht en zorg ervoor dat de ApplicationInsights.xml bevindt zich in een van deze zoeklocaties. Als een vuistregel kunt u het config-bestand plaatsen in de buurt van de toepassing inzichten SDK potten. Bijvoorbeeld: in Tomcat, betekent dit dat de map WEB-INF/lib.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Ik gebruikt om gegevens te bekijken, maar deze is gestopt.

* Controleer de [status blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u uw maandelijkse quota van gegevenspunten raakt? Open instellingen/Quota en prijzen om erachter te komen. In dat geval kunt u uw planning bijwerken of betalen voor extra capaciteit. Zie het [tarievenstelsel](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Ik zie niet alle gegevens die ik heb verwacht

* Open de quota en prijzen blade en controleer of [de bemonstering](app-insights-sampling.md) uitgevoerd wordt. (100% transmissie betekent dat bemonstering in bewerking niet.) De service Application inzichten kan worden ingesteld op het accepteren slechts een fractie van de telemetrie van uw app binnenkomt. Zo kunt u binnen uw maandelijkse quota van telemetrie houden. 

## <a name="no-usage-data"></a>Geen gegevens

*Gegevens over aanvragen en responstijden, maar niet in de paginaweergave, browser of de gegevens van de gebruiker weergegeven.*

U geconfigureerd uw app telemetrie verzenden vanaf de server. Nu de volgende stap is het instellen [van uw webpagina's voor het verzenden van de telemetrie van de webbrowser][usage].

Ook als de client een app op een [telefoon of ander apparaat]wordt[platforms], kunt u de telemetrie vandaar verzenden. 

Gebruik dezelfde sleutel instrumentation telemetrie van zowel de client als de server instellen. De gegevens weergegeven in dezelfde toepassing inzichten resource en u zult kunnen correleren van gebeurtenissen op de client en server.



## <a name="disabling-telemetry"></a>Telemetrie uitschakelen

*Hoe kan ik de telemetrie collectie uitschakelen?*

In de code:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Of** 

ApplicationInsights.xml (in de map resources in uw project) bijwerken. Voeg de volgende onder het hoofdknooppunt:

    <DisableTelemetry>true</DisableTelemetry>

Met behulp van de methode die u moet de toepassing opnieuw te starten wanneer u de waarde wijzigen.

## <a name="changing-the-target"></a>Het doel wijzigen

*Hoe kan ik wijzigen welke Azure resource verzendt gegevens naar van mijn project?*

* [De sleutel van de instrumenten van de nieuwe resource krijgt.][java]
* Als u toepassing inzichten toegevoegd aan uw project met de Toolkit Azure voor Eclips, klik met de rechtermuisknop uw webproject, **Azure**, **Inzichten voor toepassingen configureren**, selecteert en de sleutel wijzigen.
* Anders wordt de sleutel in de ApplicationInsights.xml in de map resources in het project bijwerken.


## <a name="the-azure-start-screen"></a>Het startscherm van Azure

*Ik zoek op [de Azure portal](https://portal.azure.com). De kaart zegt iets over mijn app?*

Nee, wordt de status van Azure servers over de hele wereld.

*Van het bord Azure start (beginscherm) hoe vind ik gegevens over mijn app?*

Ervan uitgaande dat [uw app voor toepassing inzichten instellen][java], klikt u op Bladeren, inzichten toepassing selecteren en selecteer de app resource die u hebt gemaakt voor uw app. Als u kunt er voortaan sneller u vastmaken aan het begin van uw app in.

## <a name="intranet-servers"></a>-Servers van intranet

*Kan ik een server controleren op mijn intranet?*

Ja, die dat uw server telemetrie op de portal Application inzichten via het openbare internet kan verzenden. 

Mogelijk moet u in uw firewall, open TCP-poorten 80 en 443 voor uitgaand verkeer voor dc.services.visualstudio.com en f5.services.visualstudio.com.

## <a name="data-retention"></a>Gegevens bewaren 

*Hoe lang gegevens behouden in de portal? Is het veilig?*

Zie [gegevens en privacy][data].

## <a name="next-steps"></a>Volgende stappen

*Ik instellen voor mijn server Java app toepassing inzichten. Wat kan ik doen?*

* [Beschikbaarheid van uw webpagina's controleren][availability]
* [Gebruik van webpagina's controleren][usage]
* [Gebruik bijhouden en vaststellen van problemen in uw apparaat apps][platforms]
* [Code schrijven voor het gebruik van uw app bijhouden][track]
* [Vastleggen van diagnostische logboeken][javalogs]


## <a name="get-help"></a>Help-informatie opvragen

* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 