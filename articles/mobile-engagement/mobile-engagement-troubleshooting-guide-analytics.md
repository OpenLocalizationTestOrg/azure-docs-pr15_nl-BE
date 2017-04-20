<properties 
   pageTitle="Azure mobiele betrokkenheid bij de Troubleshooting Guide - Analytics" 
   description="Problemen met Analytics, Monitoring, segmentatie en Dashboard in Azure Mobile Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Problemen oplossen met Analytics, Monitoring, segmentatie en Dashboard problemen

Hieronder vindt u mogelijke problemen optreden bij het Azure Mobile Engagement informatie over toepassingen, apparaten en gebruikers verzamelt.

## <a name="missingdelayed-information"></a>Ontbrekende/uitgestelde informatie

### <a name="issue"></a>Probleem
- Informatie is in die wordt weergegeven in Analytics, segmentatie of Dashboard vertraagd.
- Er ontbreekt informatie in controle.
- Er ontbreken Analytics, segmentatie of Dashboard.
- Segmentatie raken beperkt.

### <a name="causes"></a>Oorzaken

- Kunt u de Analytics API, Monitor-API en segmenten API om te zien of alle gegevens die u niet in de gebruikersinterface is zichtbaar via de API's.
- Als u de SDK van Azure Mobile Engagement is niet goed geïntegreerd in uw app vervolgens niet mogelijk gegevens in de Analytics, segmentatie, controle of Dashboards.
- Segmenten kunnen niet worden gewijzigd nadat ze zijn gemaakt, segmenten kunnen alleen worden "gekloonde" (gekopieerd) of "vernietigd" (verwijderd). Segmenten mogen alleen 10 criteria.
- De beste manier om de ontbrekende informatie uit de monitoring testen is een testapparaat installeren, verwijderen en/of opnieuw installeren van de app op het testapparaat.
- Informatie wordt elke 24 uur vernieuwd voor Analytics, segmentatie en Dashboards.
- Informatie in de nieuwe segmenten tot 24 uur nadat ze zijn gemaakt, zelfs als het segment is gebaseerd op eerdere gegevens mogelijk niet weergegeven.
- Filteren van uw analytics-gegevens in de gebruikersinterface ziet alle voorbeelden van dit type ongeacht de versie van uw app (bijvoorbeeld 'Crashes' gefilterd met de naam weergeven van versie 1 en versie 2 van uw app).
- De periode voor Analytics is gebaseerd op de datum van de instellingen van de gebruikers, zodat een gebruiker wiens telefoon heeft de datum niet juist ingesteld kan in de verkeerde tijdsperiode.
- Geen bestanden op de server gegevens geregistreerd wanneer u met de knop 'test' duwt, alleen gegevens vastgelegd voor echte push-campagnes.

## <a name="cant-locate-items-in-ui"></a>Items vinden in de gebruikersinterface niet

### <a name="issue"></a>Probleem
- Kan maken van de segmenten op basis van bepaalde ingebouwde of aangepaste app info label criteria.
- Kan niet zoeken naar bepaalde ingebouwde of aangepaste app info label criteria in Analytics, Monitoring of Dashboards.
- De gegevens in Analytics, Monitoring, segmentatie of Dashboards kunnen niet worden geïnterpreteerd.

### <a name="causes"></a>Oorzaken

- Sommige ingebouwde items en app info codes zijn alleen beschikbaar als push-criteria, maar mogen niet worden toegevoegd aan een segment of zichtbare Analytics, Monitoring of Dashboard. 
- Voor ingebouwde items en app info codes die niet kunnen worden toegevoegd aan een segment, moet u setup-lijst met criteria in elke campagne gericht uit te voeren dezelfde functie als de toewijzing op basis van een segment.
- Zie de contextmenu's in de secties Analytics, Monitoring, segmentatie en Dashboards van de UI Azure Mobile Engagement voor meer hulp en informatie.

## <a name="crash-troubleshooting"></a>Problemen met vastlopen

### <a name="issue"></a>Probleem
- Toepassing loopt vast in Analytics, Monitoring of Dashboard wordt weergegeven.

### <a name="causes"></a>Oorzaken

- Voor het oplossen van Controleer toepassing loopt vast in Analytics, Monitoring of Dashboard gezien de release-opmerkingen voor bekende problemen met eerdere versies van de SDK.
- Verder oplossen uitvoeren vastlopende toepassingen een gebeurtenis van een testapparaat met de toepassing geïnstalleerd en het uiterlijk van uw apparaat-ID in de sectie 'Monitor – Events' van de UI Azure Mobile Engagement. De gebeurtenis die wordt veroorzaakt door de toepassing vastlopen en zoek naar aanvullende informatie in de sectie "Crash Monitor –" de Azure Mobile Engagement UI vervolgens uitvoeren. 

 
