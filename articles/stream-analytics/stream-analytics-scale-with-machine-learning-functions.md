<properties
    pageTitle="Schalen van uw Stream Analytics baan met Azure Machine Learning functies | Microsoft Azure"
    description="Meer informatie over het schalen goed Stream Analytics taken (partitioneren, SU hoeveelheid en meer) bij het gebruik van functies in Azure Machine Learning."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Uw taak Analytics Stream met Azure Machine Learning functies schalen

Het is vaak heel eenvoudig een Stream Analytics taak instellen en voorbeeldgegevens doorlopen. Wat moeten we doen wanneer moeten we dezelfde taak uitvoert met een groter gegevensvolume? U moet ons inzicht in de Stream Analytics taak zo configureren dat deze kan worden uitgebreid. In dit document gaan we in op de bijzondere aspecten van taken met functies van de Machine Learning Analytics Stream schalen. Zie het artikel [schaal taken](stream-analytics-scale-jobs.md)voor meer informatie over het schalen Analytics Stream-projecten in het algemeen.

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Wat is een functie Azure Machine Learning in Stream Analytics?

Een functie van Machine Learning in Analytics stroom kan worden gebruikt als een reguliere functie-aanroep in de querytaal Stream Analytics. Het aanroepen van functies zijn echter achter de scène, daadwerkelijk Azure Machine Learning Web Service aanvragen. Machine Learning webservices ondersteunen "batchen" meerdere rijen Mini partij, in de dezelfde web service API-aanroep, ter verbetering van de algehele doorvoer wordt genoemd. Raadpleeg de volgende artikelen voor meer informatie; [Functies in Analytics Stream Azure Machine Learning](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) en [Azure Machine Learning Web-Services](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Een taak Stream Analytics met Machine Learning functies configureren

Bij het configureren van een Machine Learning functie voor Analytics Stream-project, zijn er twee parameters kunt u de grootte van de partij van de functieaanroepen Machine Learning en de Streaming-eenheden (SUs) ingericht voor de taak Stream Analytics. Om te bepalen van de juiste waarden voor deze, moet eerst een besluit worden gemaakt tussen latentie en doorvoer, dat wil zeggen, vertraging van de Stream Analytics taak, en de productie van elk SU. SUs kunnen altijd worden toegevoegd aan een taak hogere doorvoersnelheid van een query ook gepartitioneerde Stream Analytics, hoewel extra SUs verhoogt de kosten van de taak wordt uitgevoerd.

Daarom is het belangrijk om te bepalen van de *tolerantie* van vertraging in de uitvoering van een project Stream Analytics. Natuurlijk verhoogt extra latentie Azure Machine Learning serviceaanvragen worden uitgevoerd met batchgrootte, die de vertraging van de taak van de Stream Analytics wordt samengesteld. Batch vergroten kan aan de andere kant de Stream Analytics taak voor het verwerken van *meer gebeurtenissen met de *hetzelfde nummer * van Machine Learning web serviceaanvragen. Vaak is de toename van de latentie van Machine Learning web service sub lineair tot de toename van de omvang van de partij dus is het belangrijk rekening te houden met de meest rendabele batchgrootte voor een webservice Machine Learning in een bepaalde situatie. De standaard-batchgrootte voor aanvragen voor het web is 1000 en kan worden gewijzigd via de [Stream Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST API") of de [PowerShell-client voor Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell client voor Stream Analytics").

Zodra een batchgrootte is bepaald, de hoeveelheid eenheden Streaming (SUs) kan worden bepaald, op basis van het aantal gebeurtenissen dat de functie moet per seconde verwerken. Raadpleeg voor meer informatie over eenheden Streaming [Stream Analytics schaal taken](stream-analytics-scale-jobs.md#configuring-streaming-units)van het artikel.

In het algemeen zijn 20 gelijktijdige verbindingen met de computer leren webservice voor elke 6 SUs, met dien verstande dat 1 SU taken en 3 SU taken 20 gelijktijdige verbindingen ook krijgen.  Als de invoergegevens 200.000 gebeurtenissen per seconde is de batchgrootte is van links naar de standaardwaarde van 1000 is de resulterende web service latentie met 1000 gebeurtenissen Mini batch 200ms. Dit betekent dat elke verbinding 5 verzoeken kan indienen bij de webservice Machine Learning in een seconde. Bij 20 verbindingen verwerken de taak Stream Analytics 20.000 in 200ms en daarom 100.000 gebeurtenissen in een tweede. 200.000 gebeurtenissen per seconde verwerken, moet de taak van de Stream Analytics 40 gelijktijdige verbindingen, die afkomstig uit 12 SUS is. Het onderstaande diagram ziet u de verzoeken van de Stream Analytics taak aan de Machine Learning web-service-eindpunt: elke 6 SUs heeft 20 gelijktijdige verbindingen met de computer leren webservice op max.

![Schaal Stream Analytics met Machine Learning functies 2 taak voorbeeld] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Schaal Stream Analytics met Machine Learning functies 2 taak voorbeeld")

***B*** voor de omvang van de partij, ***L*** voor de web service latentieperiode batchgrootte B in milliseconden de doorvoer van een taak Analytics Stream met ***N*** SUs is over het algemeen:

![Stream Analytics machine Learning formule functies schalen] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Stream Analytics machine Learning formule functies schalen")

Een extra overweging kan 'maximum aantal gelijktijdige gesprekken' aan de kant van Machine Learning web service, het is raadzaam deze optie instellen om de maximale waarde (momenteel 200).

Voor meer informatie over deze instelling-Bekijk het [artikel voor webservices van Machine Learning schaal](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Voorbeeld – Sentiment analyse

In het volgende voorbeeld bevat een Stream Analytics met het sentiment analyse Machine Learning functie, zoals beschreven in de [zelfstudie voor Stream Analytics Machine Learning integratie](stream-analytics-machine-learning-integration-tutorial.md).

De query is een selectiequery volledig gepartitioneerde is gevolgd door de functie **sentiment** , zoals hieronder wordt weergegeven:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Neem het volgende scenario; met een capaciteit van 10.000 tweets per seconde moet een Stream Analytics taak voor het uitvoeren van sentiment analyse van de tweets (gebeurtenissen) worden gemaakt. SU 1 gebruikt, kan deze taak Stream Analytics zijn kunnen het verkeer verwerken? Met behulp van de batch standaardgrootte van 1000 moeten de taak kunnen bij de invoer te houden. Verder moet de toegevoegde Machine Learning functie genereren niet meer dan een seconde van latentie, namelijk de algemene standaard latentie van de analyse sentiment Machine Learning webservice (met een standaardbatchgrootte 1000). De **algehele** of end-to-end latentie van de Stream Analytics taak is meestal een paar seconden. Bekijk meer in deze Stream Analytics taak *met name* de Machine Learning functieaanroepen. Met de batchgrootte als 1000, duurt een capaciteit van 10.000 gebeurtenissen ongeveer 10 aanvragen webservice. Zelfs met 1 SU zijn er genoeg gelijktijdige verbindingen voor deze invoer verkeer.

Maar wat gebeurt er als de gebeurtenis input frequentie wordt verhoogd met 100 x en nu de stroom Analytics moet dan 1.000.000 tweets per seconde verwerken? Er zijn twee opties:

1.  Verhoog de grootte van de partij, of
2.  Partitie de invoerstroom voor het verwerken van gebeurtenissen in parallel

De eerste optie, wordt de taak **vertraging** verhoogd.

De tweede optie moet meer SUs worden ingericht en daarom meer gelijktijdige Machine Learning webserviceverzoeken te genereren. Dit betekent dat de taak **kosten** zal toenemen.


Stel de latentie van het sentiment analyse Machine Learning webservice 200ms voor batches van 1000 gebeurtenissen of onder, 250ms voor 5.000 gebeurtenis batches, 300ms voor batches van 10.000 gebeurtenissen of 500ms voor 25.000 gebeurtenis batches.

1. De eerste optie (**niet** ingericht meer SUs) gebruikt, kan de batchgrootte worden verhoogd tot **25.000**. Dit kunnen op hun beurt de taak dan 1.000.000 om gebeurtenissen te verwerken met 20 gelijktijdige verbindingen met de computer leren webservice (met een vertraging van 500ms per gesprek). Zo zou de extra latentie van de Stream Analytics taak als gevolg van het sentiment functie aanvragen tegen de computer leren webserviceverzoeken uit **200 MS** worden verhoogd tot **500ms**. Batch grootte **niet kan** overigens wel betere oneindig als de Machine Learning web-services alleen gebruiken als de grootte van de nettolading van een aanvraag 4 MB of kleiner webservice aanvragen time-out na 100 seconden van de bewerking.
2. Met de tweede optie, de batchgrootte is links op 1000, met 200 MS web service latentie, elke 20 gelijktijdige verbindingen met de webservice zou kunnen 1000 gebeurtenissen van *20* , 5 = 100.000 per seconde. De taak zou dus 1.000.000 gebeurtenissen per seconde verwerken, moet 60 SUs. Vergeleken met de eerste optie, zou Stream Analytics taak webserviceverzoeken meer batch, op zijn beurt een extra kosten genereren.

Hieronder vindt u een tabel voor de doorvoer van de taak van de Stream Analytics voor verschillende SUs en batch grootte (in aantal gebeurtenissen per seconde).

| Batchgrootte (latentie ML)  | 500 (200 MS) | 1000 (200ms) | 5.000 (250ms) | 10.000 (300ms) | 25.000 (500ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SU** | 2500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **3 SUs** | 2500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **6 SUs** | 2500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **12 SUs** | 5.000 | 10.000 | 40.000 | 60.000 | 100.000 |
| **18 SUs** | 7.500 | 15.000 | 60.000 | 90,000 | 150.000 |
| **24 SUs** | 10.000 | 20.000 | 80.000 | 120.000 | 200.000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25.000 | 50.000 | 200.000 | 300.000 | 500.000 |

Nu hebt u al een goed inzicht in de werking van Machine Learning functies in Analytics Stream. U waarschijnlijk begrijpen ook dat Analytics Stream taken 'pull' gegevens uit gegevensbronnen en elke 'pull' geeft als resultaat een reeks gebeurtenissen voor het project Analytics stroom te verwerken. Hoe deze pull model gevolgen de Machine Learning web serviceaanvragen?

Normaal gesproken de batchgrootte voor Machine Learning functies stelt precies niet deelbaar is door het aantal gebeurtenissen die worden geretourneerd door elk project Stream Analytics 'pull'. Dit gebeurt wanneer dat de Machine Learning-webservice wordt aangeroepen met 'gedeeltelijk' batches. Reden hiervoor is geen extra taak latentie overhead in gebeurtenissen samenvoegen pull pull oplopen.

## <a name="new-function-related-monitoring-metrics"></a>Nieuwe functie betrekking monitoring metrics

In het gedeelte Beeldscherm van een taak Stream Analytics zijn drie extra functie-gerelateerde statistieken toegevoegd. Ze zijn functie aanvragen, functie gebeurtenissen en functie-aanvragen is niet zoals in de onderstaande afbeelding.

![Stream Analytics Metrics functies leren machine schalen] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Stream Analytics Metrics functies leren machine schalen")

Het zijn als volgt gedefinieerd:

**Functie aanvragen**: het aantal functie-aanvragen.

**Gebeurtenissen voor de functie**: het aantal gebeurtenissen in de functie-aanvragen.

**Functie-aanvragen is mislukt**: het aantal mislukte functie aanvragen.

## <a name="key-takeaways"></a>Belangrijke Takeaways  

Kort samengevat de belangrijkste punten om een taak Analytics Stream met functies van de Machine Learning schalen, moeten de volgende items worden beschouwd:

1.  De snelheid van de invoer, gebeurtenis
2.  De verdragen latentie voor de lopende stroom Analytics taak (en dus ook de grootte van de partij van de Machine Learning web serviceaanvragen)
3.  De ingerichte Stream Analytics SUs en het aantal aanvragen van Machine Learning web service (met de extra functie-gerelateerde kosten)

Een volledig gepartitioneerde Stream Analytics query is als voorbeeld gebruikt. Als u een ingewikkeldere query nodig is de [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) is een goede bron voor extra hulp krijgen van de Stream Analytics-team.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Stream Analytics, Zie:

- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
