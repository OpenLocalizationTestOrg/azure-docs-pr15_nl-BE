<properties 
   pageTitle="Gegevensbronnen in logboek Analytics | Microsoft Azure"
   description="Gegevensbronnen definiëren gegevens dat logboek Analytics verzameld van agenten en andere bronnen.  In dit artikel wordt het concept van hoe Log Analytics gegevensbronnen gebruikt, verklaart de details van hoe u deze configureert en biedt een overzicht van de verschillende beschikbare gegevensbronnen."
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
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="data-sources-in-log-analytics"></a>Gegevensbronnen in logboek Analytics

Logboek Analytics verzamelt gegevens van de aangesloten bronnen in uw werkruimte OMS en opgeslagen in de opslagplaats van OMS.  De gegevens die worden verzameld uit elk wordt gedefinieerd door de gegevensbronnen die u configureert.  Gegevens in de bibliotheek OMS wordt opgeslagen als een set records.  Records van een bepaald type gegevensbron gemaakt met elk type heeft een eigen set eigenschappen.

![Meld u Analytics gegevens verzamelen](./media/log-analytics-data-sources/overview.png)

Gegevensbronnen zijn anders dan OMS oplossingen die ook gegevens van aangesloten bronnen verzamelen en records maken in de opslagplaats OMS.  Oplossingen zal kunnen worden toegevoegd aan de werkruimte vanuit de galerie met oplossingen en meestal extra analyseprogramma's in de portal OMS.  

## <a name="summary-of-data-sources"></a>Overzicht van gegevensbronnen

De gegevensbronnen die momenteel beschikbaar in logboek Analytics zijn worden weergegeven in de volgende tabel.  Elk heeft een koppeling naar een afzonderlijk artikel met details voor de gegevensbron.

| Gegevensbron | Gebeurtenistype | Beschrijving |
|:--|:--|:--|
| [Aangepaste logboeken](log-analytics-data-sources-custom-logs.md) | \<Logboeknaam\>_CL | Tekstbestanden in de Windows- of Linux-agenten met logboekgegevens. |
| [Windows-gebeurtenislogboeken](log-analytics-data-sources-windows-events.md) | Gebeurtenis | Gebeurtenissen die zijn verzameld in het gebeurtenislogboek op Windows-computers. |
| [Prestatiemeteritems van Windows](log-analytics-data-sources-performance-counters.md) | Perf | Prestatiemeteritems van Windows-computers worden verzameld. |
| [Prestatiemeteritems voor Linux](log-analytics-data-sources-performance-counters.md) | Perf | Prestatiemeteritems van Linux-computers worden verzameld. |
| [IIS-logboeken](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Internet Information Services worden vastgelegd in de W3C-indeling. |
| [Syslog](log-analytics-data-sources-syslog.md) | Syslog | Syslog-gebeurtenissen op Windows- of Linux-computers. |

## <a name="configuring-data-sources"></a>Gegevensbronnen configureren

U configureren gegevensbronnen in het menu **Data** in logboek Analytics- **Instellingen**.  Elke configuratie is bezorgd bij alle aangesloten bronnen in uw werkruimte OMS.  U kan niet op dit moment agenten uitsluiten van deze configuratie.

![Configureren van Windows-gebeurtenissen](./media/log-analytics-data-sources/configure-events.png)

2. Selecteer de tegel **Instellingen** in de console OMS.
3. **Gegevens**selecteren.
4. Klik op de gegevensbron te configureren.
5. Volg de koppeling in de documentatie bij de gegevensbron in de tabel hierboven voor meer informatie op hun configuratie.

## <a name="data-collection"></a>Het verzamelen van gegevens

Data source configuraties worden afgeleverd bij de agenten die rechtstreeks zijn verbonden met OMS binnen enkele minuten.  De opgegeven gegevens worden verzameld van de agent en rechtstreeks afgeleverd bij logboek Analytics met tussenpozen die specifiek zijn voor elke gegevensbron.  Raadpleeg de documentatie bij de gegevensbron voor deze specificaties.

Voor System Center Operations Manager (SCOM)-agents in een groep verbonden management, data source configuraties beheerpakketten vertaald en bezorgd in de management-groep om de 5 minuten standaard.  De agent het management pack net als elke andere downloads en de opgegeven gegevens worden verzameld. De gegevens worden dat ofwel verzonden naar een server voor documentbeheer die de gegevens naar het logboek Analytics stuurt afhankelijk van de gegevensbron of de agent stuurt de gegevens naar logboekbestanden Analytics zonder tussenkomst van de server. Verwijzen naar [gegevens de gegevens voor OMS-functies en -oplossingen](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) voor meer informatie.  U kunt lezen over de details van de verbinding van SCOM en OMS en aanpassing van de frequentie die configuratie op [Integratie met System Center Operations Manager configureren](log-analytics-om-agents.md)wordt afgeleverd.

## <a name="log-analytics-records"></a>Logboekrecords Analytics

Alle gegevens die zijn verzameld door Analytics logboek wordt opgeslagen in de opslagplaats OMS records.  Records die zijn verzameld door verschillende gegevensbronnen heeft een eigen set eigenschappen en worden geïdentificeerd door de eigenschap **Type** .  Zie de documentatie voor elke gegevensbron en oplossing voor meer informatie op elk recordtype.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [oplossingen](log-analytics-add-solutions.md) die functionaliteit toevoegen aan een logboek Analytics en ook het verzamelen van gegevens in de opslagplaats OMS.
- Informatie over het [logboek zoeken](log-analytics-log-searches.md) voor het analyseren van de gegevens die worden verzameld uit gegevensbronnen en oplossingen.  
- Zo configureren dat [berichten](log-analytics-alerts.md) proactief om u te waarschuwen van kritieke gegevens verzameld van gegevensbronnen en oplossingen.
