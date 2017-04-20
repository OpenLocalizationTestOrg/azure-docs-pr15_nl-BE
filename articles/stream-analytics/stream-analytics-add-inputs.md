<properties
    pageTitle="Een invoer aan uw projecten Stream Analytics toevoegen | Microsoft Azure"
    description="Informatie over het aansluiten van uw Stream Analytics baan als streaming gegevensinvoer van gegevens uit Blog opslag gebeurtenis Hubs of verwijzing naar een gegevensbron."
    keywords="gegevensinvoer, streaming-gegevens"
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


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Een streaming gegevens invoer- of gegevens toevoegen aan een taak Stream Analytics

Informatie over het aansluiten van uw Stream Analytics baan als streaming gegevensinvoer van gegevens uit de Blob-opslag gebeurtenis Hubs of verwijzing naar een gegevensbron.

Azure Stream Analytics taken kunnen worden verbonden met gegevensinvoer van één of meer, die elk een verbinding met een bestaande gegevensbron definieert. Als gegevens worden verzonden naar die gegevensbron, wordt door de taak Analytics stroom verbruikt en in real-time gegevensstromen verwerkt. Stream Analytics heeft eerste-klas integratie met [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) en [Azure Blob-opslag](../storage/storage-dotnet-how-to-use-blobs.md) zowel binnen als buiten het abonnement van de taak.

Dit artikel is een stap in de [Stream Analytics leren pad](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Invoer: Streaming en verwijzingsgegevens

Er zijn twee verschillende soorten "inputs" in Analytics Stream: gegevensstromen en referentiegegevens.

- **Gegevensstromen**: Stream Analytics taken moeten ten minste één stream invoer worden verbruikt en getransformeerd door de taak opnemen. Azure Blob-opslag en Azure gebeurtenis Hubs worden als stream input gegevensbronnen ondersteund. Azure gebeurtenis Hubs worden gebeurtenis streams verzameld van aangesloten apparaten, diensten en toepassingen gebruikt. Azure Blob-opslag kan worden gebruikt als invoerbron voor ingesting grote hoeveelheden gegevens als een stroom.  
- **Verwijzingsgegevens**: Stream Analytics ondersteunt een tweede soort referentiegegevens Aux input genoemd.  Deze gegevens zijn gegevens in beweging, in plaats van statische of de vertraging wijzigen.  Dit wordt meestal gebruikt voor het uitvoeren van op te zoeken en correlatie met gegevensstromen voor het maken van een rijkere set van gegevens.  Azure Blob-opslag is momenteel de enige ondersteunde invoerbron voor referentiegegevens.  

Invoer voor uw project Stream Analytics toevoegen:

1. Klik op **"inputs"** en klik op **toevoegen een invoer** in uw project Stream Analytics in Azure portal.

    ![Azure klassieke portal - invoer toevoegen.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Klik op het vakje **"inputs"** in uw Analytics Stream-project in de portal Azure.  

    ![Azure portal - gegevensinvoer toevoegen.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Geef het type van de input: **de gegevensstroom** of **referentiegegevens**.

    ![Invoer, stroomsgewijs verzonden of verwijzen naar de juiste gegevens toevoegen](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![Invoer, stroomsgewijs verzonden of verwijzen naar de juiste gegevens toevoegen](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. Als een gegevensstroom invoer maakt, geeft u het brontype voor de invoer.  In deze stap worden overgeslagen tijdens het maken van verwijzingen als alleen Blob opslag op dit moment wordt ondersteund.

    ![Gegevensinvoer stream-gegevens toevoegen](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Gegevensinvoer stream-gegevens toevoegen](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Een beschrijvende naam voor deze ingang in het vak invoer Alias opgeven.  Deze naam wordt gebruikt in de query van de taak later te verwijzen naar de invoer.

    Vul de rest van de vereiste eigenschappen van de verbinding met de gegevensbron. Deze velden is afhankelijk van het type invoer- en type en worden gedefinieerd in detail [hier](stream-analytics-create-a-job.md).  

    ![Gebeurtenis hub gegevensinvoer toevoegen](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. De serialisatie instellingen opgeven voor de ingevoerde gegevens:
    - Om te controleren of uw query's werken zoals verwacht, geeft u de **Gebeurtenis serialisatie-indeling** van binnenkomende gegevens.  Serialisatie ondersteunde indelingen zijn JSON, CSV- en Avro.
    - Controleer of de **codering** voor de gegevens.  UTF-8 is de enige ondersteunde coderingsindeling op dit moment.

    ![Instellingen voor serialisatie van gegevens voor de gegevensinvoer](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![Instellingen voor serialisatie van gegevens voor de gegevensinvoer](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Na het maken van de input controleren Stream Analytics dat verbinding met de invoerbron maken kan.  U kunt de status van de bewerking verbinding testen in de hub melding weergeven.

    ![Test de verbinding van de streaming-gegevens invoeren](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![Test de verbinding van de streaming-gegevens invoeren](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Hulp bij streaming gegevens ingangen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
