<properties 
    pageTitle="Controleren, afhankelijkheden, beperkingen en uitvoeringstijden in Java web apps" 
    description="Uitgebreide controle van uw Java-website met de inzichten van toepassing" 
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
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Controleren, afhankelijkheden, beperkingen en uitvoeringstijden in Java web apps

*Inzichten van toepassing is in het afdrukvoorbeeld.*

Als u [uw Java web app met inzichten toepassing geïmplementeerd]hebben[java], kunt u de Java-Agent om meer inzicht, zonder enige codewijzigingen:


* **Afhankelijkheden:** Gegevens over gesprekken die uw toepassing naar andere onderdelen, waaronder:
 * **Overige oproepen** via HttpClient, OkHttp en RestTemplate (veer).
 * **Bestand Vgx.** oproepen via de client Jedis. Als het gesprek langer dan 10s duurt, haalt de agent ook de argumenten van de oproep.
 * **[Gesprekken JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB of Apache Derby DB. 'executeBatch' aanroepen worden ondersteund. Als de aanroep langer dan 10s duurt, meldt de agent voor MySQL en PostgreSQL, het queryplan. 
* **Gevangen uitzonderingen:** Gegevens over uitzonderingen die worden verwerkt door de code.
* **Methode uitvoeringstijd:** Gegevens over de tijd die nodig is voor het uitvoeren van specifieke methoden.

Voor het gebruik van de Java-agent, installeren u deze op uw server. Uw web-apps moeten worden geïmplementeerd met de [Toepassing inzichten Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>Installeer de toepassing inzichten agent voor Java

1. Java-server, [de agent downloaden](https://aka.ms/aijavasdk)op de computer wordt uitgevoerd.
2. Het opstartscript application server bewerken en toevoegen van de volgende JVM:

    `javaagent:`*volledig pad naar het bestand agent JAR*

    Bijvoorbeeld, in Tomcat op een machine Linux:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Start de application server.

## <a name="configure-the-agent"></a>De agent configureren

Maak een bestand met de naam `AI-Agent.xml` en plaats deze in dezelfde map als de agent JAR-bestand.

De inhoud van het XML-bestand instellen. In het volgende voorbeeld als u wilt opnemen in of uitsluiten van de functies die u wilt bewerken. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

U moet rapporten uitzondering en de timing van de methode voor de afzonderlijke methoden inschakelen.

Standaard `reportExecutionTime` is ingesteld op true en `reportCaughtExceptions` is ingesteld op false.

## <a name="view-the-data"></a>De gegevens weergeven

In de bron toepassing inzichten geaggregeerde externe afhankelijkheid en de methode uitvoeringstijden wordt weergegeven [onder de tegel prestaties][metrics]. 

Open [Zoeken]om te zoeken naar afzonderlijke exemplaren van afhankelijkheid, uitzondering en methode rapporten,[diagnostic]. 

[Afhankelijkheidsproblemen diagnose - meer informatie](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Heb je vragen? Problemen?

* Er zijn geen gegevens? [Firewall-uitzonderingen instellen](app-insights-ip-addresses.md)
* [Problemen met Java oplossen](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
