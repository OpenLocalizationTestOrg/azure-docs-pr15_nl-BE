<properties 
    pageTitle="Indicatoren voor het toezicht StorSimple | Microsoft Azure" 
    description="Beschrijft de luminescentiedioden (LED's) en gebruikt voor het controleren van de status van het apparaat StorSimple een hoorbaar alarm."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carmonm"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>StorSimple indicatoren gebruiken voor het beheren van uw apparaat   

## <a name="overview"></a>Overzicht

Uw apparaat StorSimple luminescentiedioden (LED's) bevat en die u gebruiken kunt voor het controleren van de modules en de algehele status van het apparaat StorSimple alarmen. De toezichtindicatoren vindt u op de hardware-onderdelen van de primaire behuizing van het apparaat en de EBOD. De toezichtindicatoren kunnen LED's of hoorbaar alarm zijn.

Er zijn drie LED Staten gebruikt om aan te geven de status van een module: groen, groen rood oranje of rood oranje knippert.  

- Groene LED's geven een gezonde operationele status.  
- Knippert groen rood oranje LED's voor de aanwezigheid van niet-essentiële voorwaarden waarvoor tussenkomst van de gebruiker.  
- Rood oranje LED's geven aan dat er een kritieke fout aanwezig is in de module beschikbaar is.  

De rest van dit artikel beschrijft de verschillende controle indicator LED's, de locaties op het apparaat StorSimple, de status van het apparaat op basis van de Staten LED en eventuele bijbehorende hoorbaar alarm.

## <a name="front-panel-indicator-leds"></a>Voorpaneel LED

Het voorpaneel, ook bekend als de *bewerkingen deelvenster* of *ops-deelvenster*, wordt de statistische status van alle modules in het systeem. Het voorpaneel is identiek zijn op de primaire StorSimple en de EBOD en hieronder wordt geïllustreerd.  

   ![Voorpaneel apparaat][1]
 
Het voorpaneel bevat de volgende indicatoren:  

1. Knop Dempen
2. Power-indicator LED (rood-groen-geel)
3. Module fout indicator LED (op rood-oranje/uit)
4. Logische fout indicator LED (op rood-oranje/uit
5. Eenheid-ID weergeven  

Het belangrijkste verschil tussen het voorpaneel LED's voor het apparaat en die voor de EBOD-behuizing is het **Systeem eenheid-id-nummer** op het LED-scherm weergegeven. De standaard-eenheid-ID weergegeven op het apparaat is **00**, terwijl de standaard eenheid-ID weergegeven op de EBOD-behuizing **01 is**. Hiermee kunt u snel onderscheid maken tussen het apparaat en de EBOD wanneer het apparaat is ingeschakeld. Als het apparaat is uitgeschakeld, gebruikt de informatie in [een nieuw apparaat inschakelt](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) om te onderscheiden van het apparaat van de behuizing EBOD.  

## <a name="front-panel-led-status"></a>Voorpaneel LED status  

In de volgende tabel gebruiken voor het identificeren van de status die wordt aangegeven door de LED's op het voorpaneel van het apparaat of de ruimte EBOD.  

|Inschakelen van het systeem | Module fout | Logische fout | Alarm | Status|
|-------------|---------------|-----------------|-------|-------|
|Rood-oranje | UITSCHAKELEN     | UITSCHAKELEN | N.V.T. | Netstroom verloren, die op back-energie of netstroom op en de controller modules zijn verwijderd.|
|Groen | OP | OP | N.V.T. | Deelvenster zet OPS (5s) staat testen|
|Groen | UITSCHAKELEN | UITSCHAKELEN | N.V.T. | Aan alle goede functies|
|Groen | OP |N.V.T. | Fout met betrekking tot PCM LED's, ventilator fout-LED 's | PCM schuld, ventilator probleem, over of onder temperatuur|
| Groen | OP | N.V.T. | I/o-module, LED 's  | Een module voor controllerfouten|
| Groen | OP | N.V.T. | N.V.T. | Fout met betrekking tot bedrijfslogica behuizing|
| Groen | Flash | N.V.T. | Status van de module LED op controllermodule. Fout met betrekking tot PCM LED's, ventilator fout-LED 's | Onbekende controller module geïnstalleerd, I2C-bus fout, controller module vitale product data (VPD) configuratiefout |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Power module (PCM) LED koeling   

Vermogen koeling module (PCM) LED kunt u vinden op de achterkant van de behuizing van primaire of EBOD behuizing op elke module PCM. In dit onderwerp wordt beschreven hoe u met de volgende LED's voor het controleren van de status van uw apparaat StorSimple.  

- PCM-LED's voor de primaire behuizing
- PCM-LED's voor de EBOD-behuizing

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM-LED's voor de primaire behuizing  

Het apparaat StorSimple heeft een 764W PCM-module met een extra batterij. De volgende afbeelding ziet u het deelvenster LED voor het apparaat.  

   ![PCM-LED's op de primaire behuizing][2]

Legend LED:

1. Stroomstoring AC
2. Ventilator fout
3. Accu fout
4. PCM-OK
5. DC-fout
6. Goede batterij  

De status van de PCM wordt aangegeven op het LED-paneel. Het apparaat PCM LED paneel heeft zes LED's. Vier van deze LED's geven de status van de energievoorziening en de ventilator. De resterende twee LED's geven de status van de back-up batterij module in de PCM. De volgende tabellen kunt u de status van de PCM bepalen.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM-indicator LED's voor voeding en ventilator
| Status | PCM OK (groen) | AC mislukken (geel) | Ventilator mislukken (geel) | DC mislukken (geel) |
|--------|----------------|-----------------------|------------------|----------------------|
| Geen netspanning (naar behuizing) | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN|
| Geen netspanning (alleen deze PCM) | UITSCHAKELEN | OP | UITSCHAKELEN | OP |
| AC presenteren aan PCM - OK     | OP | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| PCM is mislukt (ventilator mislukt) | UITSCHAKELEN | UITSCHAKELEN | OP | N.V.T. |
| Fout met betrekking tot PCM (via het amp op spanning op de huidige) | UITSCHAKELEN | OP | OP | OP |
| PCM (ventilator van tolerantie) | OP | UITSCHAKELEN | UITSCHAKELEN | OP |
| Stand-by-modus | Knipperend | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| PCM-firmware downloaden | UITSCHAKELEN | Knipperend | Knipperend | Knipperend |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>PCM-indicator LED's voor back-up batterij  

| Status | Goede accu (groen) | Fout met betrekking tot accu (geel) |
|--------|----------------------|-----------------------|
| De batterij is niet aanwezig | UITSCHAKELEN | UITSCHAKELEN |
| Accu aanwezig zijn en in rekening gebracht | OP | UITSCHAKELEN |
| Accu opladen of onderhoud lozing | Knipperend | UITSCHAKELEN |
| Accu 'zachte' storing (herstelbaar) | UITSCHAKELEN | Knipperend |
| Accu 'harde' fout (onherstelbaar) | UITSCHAKELEN | OP |
| Disarmed accu | Knipperend | UITSCHAKELEN |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM-LED's voor de EBOD-behuizing  

De behuizing van de EBOD heeft een PCM 580 w bij en geen extra batterij. Het deelvenster PCM voor de EBOD-behuizing is indicator LED's alleen voor de voedingen en de ventilator. De volgende afbeelding ziet u deze LED's.

   ![PCM-LED's op de EBOD-behuizing][3] 
 
De volgende tabel kunt u de status van de PCM bepalen.  

| Status | PCM OK (groen) | AC mislukken (geel) | Ventilator mislukken (geel) | DC mislukken (geel) |
|--------|---------------|------------------------|------------------|----------------------|
| Geen netspanning (naar behuizing) | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| Geen netspanning (alleen deze PCM) | UITSCHAKELEN | OP | UITSCHAKELEN | OP |
| AC presenteren op PCM: OK | OP | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| PCM is mislukt (ventilator mislukt) | UITSCHAKELEN | UITSCHAKELEN | OP | X |
| Fout met betrekking tot PCM (via het amp op spanning op de huidige | UITSCHAKELEN | OP | OP | OP |
| PCM (ventilator van tolerantie) | OP | UITSCHAKELEN | UITSCHAKELEN | OP |
| Stand-by-model | Knipperend | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| PCM-firmware downloaden | UITSCHAKELEN | Knipperend | Knipperend | Knipperend |

## <a name="controller-module-indicator-leds"></a>Controller-module LED  

Het apparaat StorSimple bevat LED's voor de primaire domeincontroller en de domeincontroller EBOD modules.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Controle LED's voor de primaire domeincontroller
In de volgende afbeelding kunt u de LED's op de primaire controller herkennen. (Alle onderdelen staan steun in stand.)  

   ![LED's - primaire domeincontroller controleren][4]
 
Gebruik de volgende tabel om te bepalen of de controllermodule correct functioneert.  

### <a name="controller-indicator-leds"></a>Controller LED  

| LED | Beschrijving                                                                            
|---- | ----------- |
| ID-LED (blauw) | Geeft aan dat de module wordt wordt geïdentificeerd. Als de blauwe LED op een actieve domeincontroller knippert, de controller is de actieve domeincontroller en de andere is de stand-by-controller. Zie voor meer informatie [de active controller op uw apparaat identificeren](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Fout-LED (geel) | Geeft aan dat een storing in de controller.        
| OK-LED (groen) | Brandt, geeft aan dat de controller OK is. Knippert groen geeft aan dat een controller VPD configuratiefout. |
| SAS activiteit LED's (groen) | Brandt geeft een verbinding met geen huidige activiteiten. Knippert groen geeft aan dat de verbinding heeft een voortdurende activiteit. |
| Ethernet-status-LED 's | Rechts geeft aan dat de koppeling/netwerkactiviteit: (brandt) koppeling actief (knippert groen) netwerkactiviteit. Links geeft Netwerksnelheid: 1000 Mb/s (geel) en (groen) 100 Mb/s (uit) 10 Mb/s. Afhankelijk van het model component kan dit licht knipperen zelfs als de netwerkinterface is niet ingeschakeld. |
| LED's boeken | Hier wordt de voortgang van de boot wanneer de domeincontroller is ingeschakeld. Als het apparaat StorSimple niet opstarten, helpt dit LED Microsoft Support bepalen van het punt in het opstartproces waarin de storing is opgetreden. |

>[AZURE.IMPORTANT] 
Als de fout-LED brandt, is er een probleem met de controllermodule die kan worden opgelost door de controller opnieuw te starten. Neem contact op met Microsoft Support als dit probleem niet is opgelost wanneer u de domeincontroller opnieuw te starten.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Controle LED's voor de EBOD (EBOD behuizing)  

Elk van de 6 Gb/s SAS EBOD-controllers heeft LED's die de status aangeven, zoals in de volgende afbeelding.  

  ![Controle LED's - EBOD-behuizing][5]

Gebruik de volgende tabel om te bepalen of de module EBOD controller naar behoren werkt.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD-controller module-LED  

|Status | I/o-module OK (groen) | I/o-module fout (geel) | Activiteit van host-poort (groen) |
|-------|----------------------|-------------------------------|----------------------------|
| Controllermodule OK | OP | UITSCHAKELEN | - |
| Module voor controllerfouten | UITSCHAKELEN | OP | - |
| Er is geen externe host poortverbinding | - | - | UITSCHAKELEN |
| Externe host-poortverbinding-geen activiteit | - | - | OP |
| Externe host-poortverbinding - activiteit | - | - | Knipperend |
| Fout van de schijfcontroller module metagegevens | Knipperend | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Schijf-indicator LED's voor de primaire behuizing en EBOD behuizing

Het apparaat StorSimple heeft schijven zich in zowel de primaire behuizing en de EBOD. Elk station bevat controle indicator LED's, zoals beschreven in deze sectie. 

Voor de schijfstations, de stationsstatus wordt aangegeven met een groene LED en een rood oranje LED bevestigd aan de voorzijde van elke module station vervoerders. De volgende afbeelding ziet u deze LED's.

  ![Schijf-LED 's][6]
 
Gebruik de volgende tabel om te bepalen wat de status van elke schijf die op zijn beurt van invloed op de algehele voorpaneel LED status.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Schijf-indicator LED's voor de EBOD-behuizing  

| Status | Activiteit OK LED (groen) | Fout-LED (rood-geel) | Gekoppelde deelvenster ops LED |
|-------|--------------------------|----------------------|-------------------------|
| Geen station is geïnstalleerd | UITSCHAKELEN | UITSCHAKELEN | Geen |
| Station geïnstalleerd en operationeel | Knipperen in-of uitschakelen met de activiteit | X | Geen |
| De apparaatset identiteit SCSI Enclosure Services (SE) | OP | 1 seconde op/1 seconde uit knippert | Geen |
| Fout met betrekking tot SE's apparaat bit is ingesteld | OP | OP | Logische fout (rood) |
| Power defecten in het bedieningscircuit | UITSCHAKELEN | OP | Module fout (rood) |

## <a name="audible-alarms"></a>Hoorbaar alarm  

Een apparaat StorSimple bevat die zijn gekoppeld aan zowel de primaire behuizing en de EBOD een hoorbaar alarm. Een hoorbaar alarm bevindt zich op het voorpaneel van beide behuizingen (ook bekend als het deelvenster ops). Hoorbaar alarm geeft aan wanneer de gebreken aanwezig is. De volgende voorwaarden wordt het alarm geactiveerd:  

- Ventilator fout of storing
- Spanning valt buiten het bereik
- Over of onder voorwaarde van temperatuur
- Thermische bufferoverloop
- Systeemfout
- Logische fout
- Power supply veroorzaakt
- Het verwijderen van een module (PCM) koeling macht  

De volgende tabel beschrijft de verschillende Staten van alarm.  

### <a name="alarm-states"></a>De lidstaten alarm  

| Alarm staat | Actie | Actie met de knop ingedrukt |
|------------|---------|---------------------------------|
| S0 | Normale modus: stil | Pieptoon tweemaal |
| S1 | Fout met betrekking tot modus: 1 seconde op/1 tweede uitschakelen | Overgang naar S2 of S3 (Zie Opmerkingen) |
| S2 | Modus herinneren: periodieke pieptoon | Geen |
| S3 | Zacht modus: stil | Geen |
| S4 | Kritieke fout modus: doorlopende alarm | Niet beschikbaar: dempen niet actief |

> [AZURE.NOTE] 

>  - In staat van alarm S1, als u niet op dempen binnen 2 minuten overgangen de status automatisch S2 of S3.  
>  - Alarm-Staten S1 S4 terug naar S0 nadat de gebreken is uitgeschakeld.  
>  - Kritieke fout staat S4 kan worden ingevoerd in een andere staat.  

U kunt het geluidssignaal dempen door te drukken op de knop Dempen op de ops-paneel. Automatische dempen, treedt na twee minuten de aan-/ uitschakelaar niet handmatig wordt geëxploiteerd. Wanneer het alarm wordt gedempt, blijven het geluid met korte intermitterende pieptonen om aan te geven dat het probleem blijft optreden. Het alarm niet stil wanneer alle problemen zijn uitgeschakeld.  

De volgende tabel beschrijft de verschillende situaties die alarm.  

### <a name="alarm-conditions"></a>Alarm-voorwaarden  

| Status | Ernst | Alarm | OPS LED-scherm |
|--------|---------|--------|----------------|
| PCM-waarschuwing – verlies van gelijkstroom van een enkel PCM | Fout: geen verlies van redundantie | S1 | Module fout|
|PCM-waarschuwing – verlies van gelijkstroom van een enkel PCM | Fout – verlies van redundantie | S1 | Module fout |
| PCM-ventilator mislukt | Fout – verlies van redundantie | S1 | Module fout |
| SBB-module PCM fout gedetecteerd | Fout met betrekking tot | S1 | Module fout |
| PCM verwijderd | Fout in de configuratie | Geen | Module fout |
| Configuratiefout behuizing | Fout: kritiek | S1 | Module fout |
| Lage temperatuur melding | Waarschuwing | S1 | Module fout |
| Hoge temperatuur melding | Waarschuwing | S1 | Module fout |
| Via de temperatuur | Fout: kritiek | S1 | Module fout |
| I2C-bus fout | Fout – verlies van redundantie | S1 | Module fout |
| Het deelvenster OPS communicatiefout (I2C) | Fout: kritiek     | S1 | Module fout |
| Fout van de schijfcontroller | Fout: kritiek | S1 | Module fout |
| SBB-interface module fout | Fout: kritiek | S1 | Module fout |
| SBB-interface module fout – geen functionerende modules resterende | Fout: kritiek | S4 | Module fout |
| SBB-interfacemodule verwijderd | Waarschuwing | Geen | Module fout |
| Fout met power control station | Waarschuwing: geen verlies van power station | S1 | Module fout |
| Fout met power control station | Fout met betrekking tot – kritiek; verlies van power station | S1 | Module fout |
| Station verwijderd | Waarschuwing | Geen | Module fout |
| Onvoldoende stroom beschikbaar | Waarschuwing | geen | Module fout |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hardwareonderdelen StorSimple en status](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
