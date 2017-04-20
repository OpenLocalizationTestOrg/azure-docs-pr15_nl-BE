<properties
   pageTitle="Logboek voor Analytics-gegevens exporteren naar Power BI | Microsoft Azure"
   description="Power BI is een wolk op basis van business analytics-service van Microsoft waarmee rijke visualisaties en rapporten voor een analyse van de verschillende sets gegevens.  Logboek Analytics kunnen continu gegevens exporteren uit de opslagplaats OMS naar Power BI zodat u van de visualisaties en analyseprogramma's profiteren kunt.  In dit artikel wordt beschreven hoe query's configureren in logboek Analytics die automatisch naar Power BI met regelmatige tussenpozen exporteren."
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

# <a name="export-log-analytics-data-to-power-bi"></a>Logboek voor Analytics-gegevens exporteren naar Power BI

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is een wolk op basis van business analytics-service van Microsoft waarmee rijke visualisaties en rapporten voor een analyse van de verschillende sets gegevens.  Logboek Analytics kunt automatisch gegevens exporteren uit de opslagplaats OMS in Power BI zodat u van de visualisaties en analyseprogramma's profiteren kunt.

Wanneer u Power BI met Analytics logboek configureert, maakt u logboek query's die hun resultaten naar de bijbehorende datasets in Power BI exporteren.  De query- en blijft automatisch op een planning die u definieert als de dataset up-to-date te houden met de meest recente gegevens verzameld door logboek Analytics.

![Logboek Analytics Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI-schema 's

Een *Schema voor Power BI* bevat een logboek zoeken die een set gegevens worden geëxporteerd uit de bibliotheek OMS naar een overeenkomstige dataset in Power BI en een schema dat hoe vaak deze zoekopdracht wordt uitgevoerd bepaalt om de dataset actueel te houden.

De velden in de dataset komt overeen met de eigenschappen van de records die worden geretourneerd door de zoekfunctie.  Als de zoekactie records van verschillende typen retourneert vervolgens bevat de dataset alle eigenschappen van elk type record opgenomen.  

> [AZURE.NOTE] Het is beste gebruik van een zoekopdracht voor logboek geeft als resultaat de onbewerkte gegevens en het uitvoeren van een consolidatie met opdrachten als [maatregel](log-analytics-search-reference.md#measure).  U kunt aggregatie- en berekeningen uitvoeren in Power BI van onbewerkte gegevens.

## <a name="connecting-oms-workspace-to-power-bi"></a>Verbinding maken met OMS werkruimte Power BI

Voordat u uit logboek Analytics Power BI exporteren kunt, moet u uw werkruimte OMS verbinden met uw Power BI rekening met de volgende procedure.  

1. Klik op de tegel **Instellingen** in de console OMS.
2. Selecteer **Accounts**.
3. Klik op **verbinding met Power BI-Account**in de sectie **Informatie over de werkruimte** .
4. Voer de referenties voor uw account Power BI.

## <a name="create-a-power-bi-schedule"></a>Een Power BI planning maken

Maak een planning van de Power BI voor elke gegevensset met behulp van de volgende procedure.

1. Klik op de tegel **Logboek zoeken** in de console OMS.
2. Typ in een nieuwe query of Selecteer een opgeslagen zoekactie die de gegevens die u wilt exporteren naar **Power BI**geretourneerd.  
3. Klik boven aan de pagina te openen van het dialoogvenster **Power BI** op **Power BI** .
4. De informatie in de volgende tabel en klik op **Opslaan**.

| Eigenschap | Beschrijving |
|:--|:--|
| Naam | De naam voor het schema wanneer u de lijst met schema's Power BI. |
| Opgeslagen zoekopdracht | Het logboek zoeken uit te voeren.  De huidige query selecteren of Selecteer een bestaande opgeslagen zoekopdracht in de vervolgkeuzelijst. |
| Planning | Hoe vaak de opgeslagen zoekopdracht uitvoeren en exporteren naar Power BI dataset.  De waarde moet liggen tussen 15 minuten en 24 uur. |
| Naam gegevensset | De naam van de gegevensset in Power BI.  Deze wordt gemaakt als het niet bestaat en bijgewerkt als deze bestaat. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Bekijken en verwijderen van Power BI-schema 's

Bekijk de lijst met bestaande Power BI-schema's met de volgende procedure.

1. Klik op de tegel **Instellingen** in de console OMS.
2. Selecteer **Power BI**.

Naast de details van de planning, het aantal keren dat de planning wordt uitgevoerd in de afgelopen week en de status van de laatste synchronisatie weergegeven.  Als de synchronisatie fouten heeft gevonden, kunt u de koppeling als u wilt een logboek zoeken naar records met gegevens over de fout.

U kunt een schema verwijderen door te klikken op de **X** in de **kolom verwijderen**.  U kunt een schema uitschakelen door te selecteren **uit**.  U moet verwijderen en opnieuw te maken met de nieuwe instellingen wilt wijzigen.

![Power BI-schema 's](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Voorbeeld scenario
De volgende sectie helpt bij een voorbeeld van een Power BI-schema maken en eenvoudige rapporten maken met behulp van de dataset.  In dit voorbeeld wordt alle prestatiegegevens voor een set computers wordt geëxporteerd naar Power BI en vervolgens een lijngrafiek wordt gemaakt om processorgebruik weer te geven.

### <a name="create-log-search"></a>Maken van logboek zoeken
Begin met het maken van een zoekfunctie voor de gegevens die we willen sturen naar de dataset.  In dit voorbeeld gebruikt u een query die alle gegevens voor computers met een naam die met *srv begint*als resultaat geeft.  

![Power BI-schema 's](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Power BI zoekopdracht maken
We Klik op de knop **Power BI** opent het dialoogvenster Power BI en geef de vereiste gegevens.  Wij willen deze zoekopdracht uitvoeren eenmaal per uur en het maken van een dataset *Contoso Perf*genoemd.  Omdat we hebben al de zoekactie openen waarmee u de gegevens willen we, we houden de standaardinstelling van de *huidige zoekopdracht gebruiken* voor de **Opgeslagen zoekopdracht**.

![Power BI zoeken](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Controleer of Power BI zoeken
Om te controleren of we de planning goed hebt gemaakt, bekijken we de lijst van Power BI zoekacties onder de tegel **Instellingen** in het dashboard OMS.  Wacht enkele minuten en we deze weergave vernieuwen totdat deze rapporteert dat de synchronisatie is uitgevoerd.

![Power BI zoeken](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Controleer of de dataset in Power BI
We melden ons rekening bij [powerbi.microsoft.com](http://powerbi.microsoft.com/) en blader naar **Datasets** onder aan het linkerdeelvenster.  U ziet dat de dataset *Contoso Perf* is opgenomen die aangeeft dat de uitvoer met succes is uitgevoerd.

![Power BI dataset](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Maken op basis van een dataset
We de dataset **Contoso Perf** selecteren en klik vervolgens op op de **resultaten** in het deelvenster **velden** aan de rechterkant naar de velden weergeven die deel van deze dataset uitmaken.  Als u wilt een lijndiagram met processorgebruik voor elke computer maken, uitvoeren we de volgende acties.

1. Selecteer de visualisatie van de grafiek lijn.
2. Sleep de **objectnaam** naar **rapportfilter niveau** en controleren van de **Processor**.
3. Sleep **CounterName** naar **rapportfilter niveau** en **percentage processortijd**te controleren.
4. Sleep de **tegenwaarde** in **waarden**.
5. Sleep de **Computer** aan de **legenda**.
6. Sleep **TimeGenerated** **as**.

U ziet dat de resulterende lijngrafiek met de gegevens van de dataset wordt weergegeven.

![Power BI-lijndiagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Het rapport opslaan
We het rapport opslaan door te klikken op de knop opslaan boven in het scherm en valideren dat nu wordt weergegeven in de sectie rapporten in het linkerdeelvenster.

![Power BI-rapporten](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [logboek zoeken](log-analytics-log-searches.md) naar query's opbouwen die kunnen worden geëxporteerd naar Power BI.
- Meer informatie over [Power BI](http://powerbi.microsoft.com) te bouwen op basis van de uitvoer van Log Analytics visualisaties.
