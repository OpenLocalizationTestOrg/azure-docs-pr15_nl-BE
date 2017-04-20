<properties 
    pageTitle="Aan de slag met de toepassing van inzichten met Java in Eclips" 
    description="Gebruik de invoegtoepassing Eclips prestaties en gebruik controleren van uw Java-website met de inzichten van toepassing toevoegen" 
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
    ms.date="03/02/2016" 
    ms.author="awills"/>
 
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Aan de slag met de toepassing van inzichten met Java in Eclips

Zodat u en de prestaties analyseren kunt, de toepassing inzichten SDK telemetrie verzendt vanuit uw webtoepassing van Java. De invoegtoepassing voor toepassing inzichten Eclips installeert automatisch de SDK in uw project zodat u zich buiten het vak telemetrie, plus een API die u kunt gebruiken voor het schrijven van aangepaste telemetrie.   


## <a name="prerequisites"></a>Vereisten

Op dit moment de plug-in werkt voor Maven projecten en dynamische Web-projecten in de Eclips. ([Toepassing inzichten toevoegen aan andere soorten Java project][java].)

U hebt het volgende nodig:

* Oracle JRE 1.6 of hoger
* Een abonnement op [Microsoft Azure](https://azure.microsoft.com/). (U kunt starten met de [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).)
* [Eclips IDE voor ontwikkelaars van Java EE](http://www.eclipse.org/downloads/), Indigo of hoger.
* Windows 7 of hoger, of Windows Server 2008 of hoger

## <a name="install-the-sdk-on-eclipse-one-time"></a>De SDK installeren op Eclips (eenmalig)

U hoeft alleen te doen dit één keer per computer. In deze stap installeert een toolkit die vervolgens de SDK aan elk dynamische Web Project kunt toevoegen.

1. In de Eclips, klikt u op Help, nieuwe Software installeren.

    ![Help, nieuwe Software installeren](./media/app-insights-java-eclipse/0-plugin.png)

2. De SDK is in http://dl.windowsazure.com/eclipse onder Azure Toolkit. 
3. Schakel het selectievakje **contact opnemen met alle update-sites...**

    ![Schakel voor toepassing inzichten SDK, contact op met alle update-sites](./media/app-insights-java-eclipse/1-plugin.png)

De overige stappen voor elk project van Java.

## <a name="create-an-application-insights-resource-in-azure"></a>Maakt u een bron toepassing inzichten in Azure

1. Log in om de [Azure portal](https://portal.azure.com).
2. Maak een nieuwe resource van toepassing inzichten.  

    ![Klik op + en kies toepassing inzichten](./media/app-insights-java-eclipse/01-create.png)  
3. Het toepassingstype Java webtoepassing instellen.  

    ![Vul een naam en klik op maken kiest u Java web app](./media/app-insights-java-eclipse/02-create.png)  
4. De instrumentatie vinden van de nieuwe resource. U moet dit binnenkort in uw CodeProject plakken.  

    ![In de nieuwe resource-overzicht, klik op eigenschappen en de sleutel Instrumentation kopiëren](./media/app-insights-java-eclipse/03-key.png)  


## <a name="add-application-insights-to-your-project"></a>Inzichten van de toepassing aan uw project toevoegen

1. Toepassing inzichten in het contextmenu van het Java webproject toevoegen.

    ![In de nieuwe resource-overzicht, klik op eigenschappen en de sleutel Instrumentation kopiëren](./media/app-insights-java-eclipse/02-context-menu.png)

2. Plak de instrumentation sleutel die u hebt gekregen van het portal voor Azure.

    ![In de nieuwe resource-overzicht, klik op eigenschappen en de sleutel Instrumentation kopiëren](./media/app-insights-java-eclipse/03-ikey.png)


De sleutel wordt meegestuurd met alle items van Telemetrie en inzichten van de toepassing weer te geven in de bron wordt aangegeven.

## <a name="run-the-application-and-see-metrics"></a>Start de toepassing en statistieken bekijken

De toepassing wordt uitgevoerd.

Terug naar de bron van de inzichten van de toepassing in Microsoft Azure.

HTTP-verzoeken gegevens worden weergegeven op het blad Overzicht. (Als deze er niet is, wacht een paar seconden en klik vervolgens op vernieuwen.)

![Antwoord van de server, telt het aantal aanvragen en fouten ](./media/app-insights-java-eclipse/5-results.png)
 

Klik op een grafiek om te zien meer gedetailleerde statistieken. 

![Telt het aantal aanvragen met naam](./media/app-insights-java-eclipse/6-barchart.png)


[Meer informatie over parameters.][metrics]

 

En wanneer de eigenschappen van een aanvraag, kunt u de telemetrie gebeurtenissen gekoppeld zoals aanvragen en uitzonderingen.
 
![Alle sporen voor deze aanvraag](./media/app-insights-java-eclipse/7-instance.png)


## <a name="client-side-telemetry"></a>Client-side telemetrie

Klik op Get code voor het controleren van mijn webpagina's van de bladeserver QuickStart: 

![Kies Quick Start, programmacode voor het controleren van mijn webpagina's op uw app-overzicht-blade. Kopieer het script.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Het codefragment invoegen in de kop van de HTML-bestanden.

#### <a name="view-client-side-data"></a>Client-side-gegevens weergeven

Bijgewerkte webpagina's openen en gebruiken. Wacht een minuut of twee, en vervolgens terug te keren naar de inzichten van de toepassing en het gebruik van blade open. (Het blad Overzicht Schuif naar beneden en klik op Taakgebruik.)

Pagina weergave, de gebruiker en de sessie statistieken worden weergegeven op het blad gebruik:

![Sessies, gebruikers en bezoekers](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[Meer informatie over het instellen van client-side telemetrie.][usage]

## <a name="publish-your-application"></a>Publiceer de toepassing

Nu uw app publiceren naar de server, kunnen mensen gebruik en controle van de telemetrie op de portal weergegeven.

* Zorg ervoor dat uw firewall kan uw toepassing telemetrie verzenden tot deze poorten:

 * DC.Services.visualstudio.com:443
 * DC.Services.visualstudio.com:80
 * F5.Services.visualstudio.com:443
 * F5.Services.visualstudio.com:80


* Installeren op Windows-servers:

 * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Hiermee kunt u prestatiemeteritems.)

## <a name="exceptions-and-request-failures"></a>Uitzonderingen en mislukte aanvraag

Niet-verwerkte uitzonderingen worden automatisch verzameld:

![](./media/app-insights-java-eclipse/21-exceptions.png)

Om gegevens te verzamelen over andere uitzonderingen, hebt u twee mogelijkheden:

* Het [aanroepen van TrackException in uw code invoegen](app-insights-api-custom-events-metrics.md#track-exception). 
* [De Java-Agent op de server installeren](app-insights-java-agent.md). U de methoden die u wilt bekijken.


## <a name="monitor-method-calls-and-external-dependencies"></a>Methodeaanroepen en externe afhankelijkheden controleren

[De Agent Java installeren](app-insights-java-agent.md) aanmelden opgegeven interne methoden en oproepen via JDBC, met timinggegevens.


## <a name="performance-counters"></a>Prestatiemeteritems

Uw overzicht blade, schuif naar beneden en klik op de tegel **Servers** . Hier ziet u een reeks prestatiemeters.


![Ga naar klikken de Servers naast](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Verzameling van prestatiemeteritems aanpassen

Collectie van de standaard set prestatiemeteritems uitschakelen, voeg de volgende code onder het hoofdknooppunt van het bestand ApplicationInsights.xml:

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>

### <a name="collect-additional-performance-counters"></a>Aanvullende prestatie-items verzamelen

U kunt extra prestatiemeteritems te innen.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>JMX-items (weergegeven door de Java Virtual Machine)

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>

*   `displayName`-De naam die wordt weergegeven in de portal Application inzichten.
*   `objectName`-De naam van het object JMX.
*   `attribute`: Het kenmerk van de naam JMX-object op te halen
*   `type`(optioneel) - het type kenmerk JMX-object:
 *  Standaard: een eenvoudig type zoals integer of long.
 *  `composite`: de prestatiemetergegevens is de notatie 'Attribute.Data'
 *  `tabular`: de prestatiemetergegevens heeft de indeling van een tabelrij



#### <a name="windows-performance-counters"></a>Prestatiemeteritems van Windows

Elke [Windows Prestatiemeter](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is een lid van een categorie (op dezelfde manier dat een veld een lid van een klasse is). Categorieën kan zijn van globale, of niveaus of u benoemde exemplaren.

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>

*   displayName: de naam die wordt weergegeven in de portal Application inzichten.
*   Categorienaam: het Prestatiemeteritem-categorie (prestatie-object) waaraan dit prestatiemeteritem gekoppeld is.
*   counterName – de naam van de Prestatiemeter.
*   instanceName – de naam van het exemplaar van het Prestatiemeteritem categorie of een lege tekenreeks (""), als de categorie een enkel exemplaar bevat. Als de categorienaam is een proces en de Prestatiemeter die u wilt verzamelen uit het huidige proces van JVM op die de toepassing wordt uitgevoerd, geeft u `"__SELF__"`.

De prestatiemeteritems zichtbaar zijn als aangepaste metrics in [Explorer Metrics][metrics].

![](./media/app-insights-java-eclipse/12-custom-perfs.png)


### <a name="unix-performance-counters"></a>Prestatiemeteritems voor UNIX

* [Collectd met de invoegtoepassing voor de inzichten van de toepassing installeren](app-insights-java-collectd.md) om een grote verscheidenheid aan gegevens en het netwerk.

## <a name="availability-web-tests"></a>Beschikbaarheid van web-tests

Toepassing inzichten kunnen testen uw website regelmatig te controleren of is het goed reageert. [Voor het instellen van][availability], schuif naar beneden te klikken op beschikbaarheid.

![Schuif naar beneden en klik op beschikbaarheid, vervolgens de test Web toevoegen](./media/app-insights-java-eclipse/31-config-web-test.png)

Krijgt u grafieken responstijden, plus e-mailberichten als uw site afgesloten wordt.

![Voorbeeld van web-test](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Meer informatie over de beschikbaarheid van web-tests.][availability] 

## <a name="diagnostic-logs"></a>Diagnoselogboeken

Als u Logback of Log4J (v1.2 of v2.0) voor overtrekken, hebt u uw traceerlogboeken automatisch verzonden naar de inzichten van Application kunt u verkennen en zoeken op deze.

[Meer informatie over diagnostische logboeken][javalogs]

## <a name="custom-telemetry"></a>Aangepaste telemetrie 

Een paar regels code invoegen in uw webtoepassing van Java om erachter te komen wat gebruikers ermee doen of om te helpen bij het vaststellen van problemen. 

U kunt een code invoegen in webpagina's, JavaScript en de server-side Java.

[Meer informatie over aangepaste telemetrie][track]



## <a name="next-steps"></a>Volgende stappen

#### <a name="detect-and-diagnose-issues"></a>Opsporen en onderzoeken van problemen

* [Toevoegen web client telemetrie] [ usage] prestaties telemetrie ophalen uit de-webclient.
* [Web tests instellen] [ availability] controleren of uw toepassing blijft live en reageert.
* [Gebeurtenissen en logboeken zoeken] [ diagnostic] om u te helpen bij het vaststellen van problemen.
* [Leggen van sporen van Log4J of Logback][javalogs]

#### <a name="track-usage"></a>Gebruik bijhouden

* [Toevoegen web client telemetrie] [ usage] monitor paginaweergaven en elementaire gebruiker statistieken.
* [Aangepaste gebeurtenissen en statistieken bijhouden] [ track] voor meer informatie over hoe uw toepassing wordt gebruikt, zowel op de client en de server.


<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 