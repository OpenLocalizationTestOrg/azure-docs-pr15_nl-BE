<properties 
   pageTitle="Stream Analytics beperkt tabel"
   description="Beschrijving van systeem grenzen en aanbevolen formaten voor Stream Analytics-onderdelen en verbindingen."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Limiet-ID | Limiet       | Opmerkingen |
|----------------- | ------------|--------- |
| Maximum aantal stroomsgewijze eenheden per abonnement per regio | 50 | Een streaming eenheden voor uw abonnement na 50 verhogen kunt verzoek contact opnemen met [Microsoft Support](https://support.microsoft.com/en-us). |
| Maximale doorvoer van een Streaming-eenheid | 1MB / s * | Maximale doorvoer per SU is afhankelijk van het scenario. Werkelijke doorvoer mogelijk lager en afhankelijk van de complexiteit van de query en partitioneren. Meer informatie vindt u in het artikel [schaal Azure Stream Analytics taken hogere doorvoersnelheid](../articles/stream-analytics/stream-analytics-scale-jobs.md) . |
| Maximum aantal ingangen per project | 60 | Er is een vaste limiet van 60 inputs per Stream Analytics project. |
| Maximum aantal resultaten per project | 60 | Er is een vaste limiet van 60 resultaten per Stream Analytics project. |
| Maximum aantal functies per project | 60 | Er is een vaste limiet van 60 functies per Stream Analytics project. |
| Maximum aantal taken per regio | 1500 | Elk abonnement mogelijk tot 1500 projecten per regio. |