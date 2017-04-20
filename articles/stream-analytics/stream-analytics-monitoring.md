<properties 
    pageTitle="Wat stroom Analytics taken controleren | Microsoft Azure" 
    description="Wat stroom Analytics taken controleren" 
    keywords="query-monitor"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Stream Analytics taak toezicht op en het controleren van query's begrijpen

## <a name="introduction-the-monitor-page"></a>Inleiding: De pagina-monitor

De Azure Management portal en Portal Azure voornaamste prestatiestatistieken ophalen die kunnen worden gebruikt om te controleren en oplossen van problemen met de prestaties van de query- en het oppervlak. 

Klik op het tabblad **Monitor** van een actieve taak Stream Analytics voor een overzicht van deze gegevens in de portal Management Azure. Er is een vertraging van ten hoogste 1 minuut in de prestatiegegevens in de pagina weergegeven.  

  ![Project-Dashboard controleren](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

Ga naar de Stream Analytics taak u geïnteresseerd bent in het zien van de maatstaven voor en **de controlesectie** weergeven in de Portal Azure.  

  ![Azure project Monitoring Portal Dashboard](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

De eerste keer die een Stream Analytics taak wordt gemaakt in een gebied, moet u diagnostische gegevens configureren voor die regio. Hiertoe wordt Klik ergens in de sectie **bewaking** en de blade **Diagnostische gegevens** weergegeven. U kunt hier een diagnoseprogramma inschakelen en geeft u een account van de opslag voor het controleren van gegevens.  

  ![Azure Portal configureren query diagnostische gegevens](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Beschikbaar voor Stream Analytics metrics


| Metric | Definitie |
|--------|-------------|
| SU percentage in gebruik | Het gebruik van de Streaming-eenheid is toegewezen aan een taak op het tabblad schaal van het project. Deze indicator bereiken moet 80% of boven, hoge waarschijnlijkheid dat verwerking van de gebeurtenis kan worden vertraagd of gestopt bezig is. |
| Invoer gebeurtenissen | De hoeveelheid gegevens die worden ontvangen door de taak Analytics Stream in aantal gebeurtenissen. Dit kan worden gebruikt voor het valideren van gebeurtenissen worden verzonden naar de invoerbron. |
| Uitvoer-gebeurtenissen | De hoeveelheid gegevens die worden verzonden door de taak Analytics Stream naar het doel output in het aantal gebeurtenissen. |
| Out-volgorde gebeurtenissen | Aantal gebeurtenissen ontvangen die zijn verwijderd of een gecorrigeerde tijdstempel, op basis van het beleid voor het bestellen van gebeurtenis gegeven. Dit kan worden beïnvloed door de configuratie van de instelling Out van tolerantie venster. |
| Conversiefouten gegevens | Het aantal gegevens conversiefouten ontstaan door een stroom Analytics taak. |
| Runtime-fouten | Het aantal fouten die tijdens het uitvoeren van een taak Stream Analytics optreden. |
| Latere invoer gebeurtenissen | Aantal gebeurtenissen die laat aankomen uit de bron die ofwel is verbroken of de tijdstempel is gewijzigd op basis van de configuratie van de gebeurtenis bestellen beleid van de instelling van de Late aankomst tolerantie venster. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Monitoring in Azure Management portal aanpassen ##

In een grafiek kan maximaal 6 cijfers worden weergegeven.

Als u wilt schakelen tussen de weergave van de relatieve (definitieve waarde voor elke metrisch) en absolute waarden (Y-as weergegeven), selecteert u relatieve of Absolute boven aan de grafiek.

  ![Absolute, relatieve query-monitor](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Metrische gegevens kunnen worden weergegeven in de grafiek Monitor in samenvoegingen van 1 uur, 12 uur, 24 uur of 7 dagen.

Als selecteert u het tijdsbereik de metrics grafiek wordt weergegeven, 1 uur, 24 uur of 7 dagen aan het begin van de grafiek.

  ![Query monitor tijdschaal](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

U kunt instellen dat regels die u kunnen een melding per e-mail als de taak een gedefinieerde drempel overschrijdt. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Het toezicht op de Azure Portal aanpassen ##

U kunt het type grafiek, metrics weergegeven, aanpassen en tijdbereik in de grafiek bewerken-instellingen. Zie [controle aanpassen](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)voor meer informatie.

  ![Azure Portal Query Monitor tijdschaal](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Taakstatus

De status van de Stream Analytics taken kan worden weergegeven in de klassieke Azure-Portal waar u een van taken overzicht. Ziet u de lijst met taken op het pictogram Analytics stroom in de klassieke Azure-Portal.

| Status | Definitie |
|--------|------------|
| Gemaakt | Een taak is gemaakt, maar is niet gestart. |
| Starten | Een gebruiker klikt op Start de taak en de taak wordt gestart. |
| Wordt uitgevoerd | De taak is toegewezen, verwerken van invoer of wachten op invoer van de verwerking. Als de taak een staat uitvoeren zonder de uitvoer te produceren, is het waarschijnlijk dat het tijdvenster gegevensverwerking groot is of de query logica gecompliceerd is. Een andere reden kan zijn dat op dit moment is er geen gegevens worden verzonden naar de taak. |
| Stoppen | Een gebruiker heeft geklikt op Stop de taak en de taak wordt gestopt. |
| Gestopt | De taak is gestopt. |
| Verslechterd | Deze status geeft aan dat een taak Stream Analytics tijdelijke is fouten (voor ex. Invoer/uitvoer-fouten, fouten bij bestandsverwerking, conversiefouten enz.). De taak nog wordt uitgevoerd, maar er zijn echter veel fouten worden gegenereerd. Deze taak moet de aandacht van de klant en de klant de logboeken bewerkingen voor de fouten kunt zien. |
| Is mislukt | Dit betekent dat de taak is mislukt door fouten, en de verwerking is gestopt. De klant moet zoeken in de logboeken van bewerkingen om de fouten opsporen. |
| Verwijderen | Hiermee wordt aangegeven dat de taak wordt verwijderd. |

## <a name="diagnosis"></a>Diagnose

In de portal Management Azure biedt het dashboard taak informatie over waar u zoeken moet voor de diagnose, d.w.z. invoer, uitvoer en/of meld u de bewerkingen. Kunt u op de koppeling om de juiste locatie om de diagnose te bekijken.

  ![Query controleren fout](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Te klikken op de invoer- of resource biedt gedetailleerde diagnostische informatie. Dit is vernieuwd met de meest recente diagnose-informatie terwijl de taak wordt uitgevoerd.

  ![Diagnostische gegevens van query](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
