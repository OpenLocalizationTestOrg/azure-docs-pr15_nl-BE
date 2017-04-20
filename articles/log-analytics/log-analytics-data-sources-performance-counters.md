<properties 
   pageTitle="Windows en Linux prestatiemeteritems in logboek Analytics | Microsoft Azure"
   description="Prestatiemeteritems zijn verzameld door logboek Analytics voor het analyseren van de prestaties van Windows en Linux-agenten.  In dit artikel wordt beschreven hoe u de verzameling van prestatiemeteritems voor zowel Windows en Linux agenten, details van deze worden opgeslagen in de opslagplaats OMS en hoe ze in de portal OMS analyseren."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows en Linux prestaties gegevensbronnen in logboek Analytics 

Prestatiemeteritems van Windows en Linux bieden inzicht in de prestaties van de hardwareonderdelen, besturingssystemen en toepassingen.  Logboek Analytics kunt verzamelen van prestatiemeteritems op vaste intervallen voor analyse in de buurt van Real Time (NRT) naast het verzamelen van prestatiegegevens voor langere termijn analyse en rapportage.

![Prestatiemeteritems](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Configureren van prestatiemeteritems

De prestatiemeteritems in het [menu Data in Analytics logboekinstellingen](log-analytics-data-sources.md#configuring-data-sources)configureren.

Wanneer u eerst de Windows- of Linux-prestaties tellers voor een nieuwe OMS-werkruimte, krijgt u de optie voor het snel maken van diverse veelgebruikte items.  Ze worden weergegeven met een selectievakje naast elk.  Zorg ervoor dat alle items die u wilt maken in eerste instantie worden gecontroleerd en klik op **de geselecteerde items toevoegen**.

![Prestatiemeteritems van Windows configureren](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Volg deze procedure voor het toevoegen van een nieuwe Windows Prestatiemeter te verzamelen.

1. Typ de naam van het item in het tekstvak in de indeling *\counter object (instantie)*.  Wanneer u te typen begint, verschijnt er een overeenkomstige lijst met veelgebruikte items.  U kunt een item uit de lijst of typ een eigen selecteren.  U kunt ook alle exemplaren van een bepaald item retourneren door het *object item*op te geven. 
2. Klik op **+** of druk op **Enter** om het item aan de lijst toevoegen.
3. Wanneer u een item toevoegt, wordt de standaardwaarde van 10 seconden gebruikt voor de **Controle-Interval**.  U kunt dit wijzigen in een hogere waarde van maximaal 1800 seconden (30 minuten) als u wilt de opslagvereisten van de verzamelde prestatiegegevens te verminderen.
4. Klik op de knop **Opslaan** boven in het scherm de configuratie opslaan wanneer u klaar bent met het toevoegen van items.

![Prestatiemeteritems voor Linux configureren](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Volg deze procedure voor het toevoegen van een nieuwe Linux Prestatiemeter te verzamelen.

1. Standaard worden alle wijzigingen in de configuratie automatisch geduwd naar alle agenten.  Voor Linux-agenten wordt een configuratiebestand verzonden naar de gegevensverzamelaarset Fluentd.  Als u wilt dat dit bestand handmatig op elke Linux-agent wijzigen, schakel dan het selectievakje *toepassen onder configuratie van mijn Linux machines*in.
2. Typ de naam van het item in het tekstvak in de indeling *\counter object (instantie)*.  Wanneer u te typen begint, verschijnt er een overeenkomstige lijst met veelgebruikte items.  U kunt een item uit de lijst of typ een eigen selecteren.  
2. Klik op **+** of druk op **Enter** om het item toevoegen aan de lijst met items van het object.
3. Alle items voor een object gebruiken dezelfde **Controle-Interval**.  De standaardwaarde is 10 seconden.  Wijzigt u dit in een hogere waarde van maximaal 1800 seconden (30 minuten) als u wilt de opslagvereisten van de verzamelde prestatiegegevens te verminderen.
4. Klik op de knop **Opslaan** boven in het scherm de configuratie opslaan wanneer u klaar bent met het toevoegen van items.

## <a name="data-collection"></a>Het verzamelen van gegevens

Logboek Analytics verzamelt alle opgegeven prestatiemeteritems op hun opgegeven controle-interval op alle agenten die teller geïnstalleerd hebben.  De gegevens worden niet samengevoegd en de onbewerkte gegevens zijn beschikbaar in alle weergaven van logboek zoeken voor de duur die is opgegeven door uw abonnement OMS.


## <a name="performance-record-properties"></a>Eigenschappen van Performance record

Prestaties records hebben een type **Perf** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Computer         | De computer waarop de gebeurtenis is verzameld uit. |
| CounterName      | Naam van de Prestatiemeter |
| Itempad      | Volledige pad van het item in het formulier \\ \\ \<Computer >\\object(exemplaar)\\teller. |
| Tegenwaarde     | Een numerieke waarde van de teller.  |
| InstanceName     | Naam van de gebeurtenis.  Leeg als er geen exemplaar. |
| Objectnaam       | De naam van het object |
| SourceSystem  | Het type van de gegevens zijn verzameld van agent. <br> OpsManager – Windows agent, een directe verbinding of SCOM <br> Linux – alle Linux-agenten  <br> AzureStorage – Azure diagnostische gegevens |
| TimeGenerated       | Datum en tijd die de gegevens is bemonsterd. |


## <a name="sizing-estimates"></a>Maakt een schatting van formaat

 Een ruwe schatting voor het verzamelen van een bepaald item met intervallen van 10 seconden is ongeveer 1 MB per dag per exemplaar.  U kunt de opslagvereisten van een bepaald item met de volgende formule kunt schatten.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Logboek zoekopdrachten met prestaties records

De volgende tabel bevat voorbeelden van het logboek zoeken en ophalen van records van de prestaties.

| Query | Beschrijving |
|:--|:--|
| Type = prest | Alle gegevens |
| Type = Computer Perf = "DezeComputer" | Alle gegevens van een bepaalde computer. |
| Type = Perf CounterName = "Huidige wachtrijlengte" | Alle gegevens voor een bepaald item. |
| Type = Perf (objectnaam = Processor) CounterName = 'percentage processortijd' InstanceName = _Totaal & #124; meting van Avg(Average) als AVGCPU door de Computer | Gemiddelde CPU-gebruik op alle computers |
| Type = Perf (CounterName = 'percentage processortijd') & #124;  max(Max) door Computer meten | Maximaal CPU-gebruik op alle computers |
| Type = Perf objectnaam logische schijf CounterName = = Computer "Lengte van de huidige schijf" = "MijnComputernaam" & #124; Avg(Average) door InstanceName meten | Huidige wachtrijlengte voor schijf in alle exemplaren van een bepaalde computer gemiddelde |
| Type = Perf CounterName = "DiskTransfers / sec" & #124; percentile95(Average) door Computer meten | 95e percentiel van schijf overdrachten per seconde op alle computers |
| Type = Perf CounterName = 'percentage processortijd' InstanceName = "_Totaal" & #124; avg(CounterValue) meten met Computer Interval 1 uur | Het gemiddelde uurtarief van CPU-gebruik op alle computers |
| Type = Perf Computer = "MijnComputer" CounterName = % * InstanceName = _Totaal & #124; percentile70(CounterValue) meten met CounterName Interval 1 uur | Per uur 70 percentiel van elke % percentage teller voor een bepaalde computer. |
| Type Perf CounterName = 'percentage processortijd' InstanceName = = '_Totaal' (Computer = "DezeComputer") & #124; min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) meten met Computer Interval 1 uur | Uur gemiddelde, minimum, maximum, en 75-percentiel CPU-gebruik voor een specifieke computer |

## <a name="viewing-performance-data"></a>Prestatiegegevens weergeven

Wanneer u een logboek zoeken naar gegevens uitvoert, wordt de weergave **logboek** standaard weergegeven.  De gegevens in grafische vorm, klikt u op **parameters**.  Voor een gedetailleerde grafische weergave, klikt u op de **+** naast een item.  

![Statistieken weergeven die zijn samengevouwen](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Als het geselecteerde tijdsbereik 6 uur of minder, wordt de grafiek bijgewerkt om de paar seconden.  Live-gegevens wordt weergegeven aan de rechterkant van de grafiek in lichtblauw.

![Statistieken weergeven wordt uitgebreid met dynamische gegevens](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Statistische gegevens in een logboek zoeken, Zie [metrische aggregatie op verzoek en visualisatie in OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [logboek zoeken](log-analytics-log-searches.md) voor het analyseren van de gegevens die worden verzameld uit gegevensbronnen en oplossingen.  
- Verzamelde gegevens exporteren naar [Power BI](log-analytics-powerbi.md) voor extra visualisaties en analyse.