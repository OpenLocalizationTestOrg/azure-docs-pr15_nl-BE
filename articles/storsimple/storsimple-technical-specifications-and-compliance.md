<properties 
   pageTitle="Technische specificaties StorSimple | Microsoft Azure"
   description="Beschrijving van de technische specificaties en wettelijke normen voldoen informatie voor de hardwareonderdelen StorSimple."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Technische specificaties en compatibiliteit voor de device StorSimple

## <a name="overview"></a>Overzicht

De hardwareonderdelen van uw apparaat met Microsoft Azure StorSimple voldoen aan de technische specificaties en wettelijke normen die in dit artikel worden beschreven. De technische specificaties beschrijven de voeding en koeling Modules (PCMs), harde schijven, opslagcapaciteit en behuizingen. De naleving van informatie heeft betrekking op zaken als de internationale normen, veiligheid en emissies en bekabeling.


## <a name="power-and-cooling-module-specifications"></a>Voeding en koeling Module.  

Het apparaat StorSimple heeft twee 100-240 v dual fan SBB-compatibele Power koeling Modules (PCMs). Dit zorgt voor een redundante configuratie. Als een PCM mislukt, blijft het apparaat normaal functioneren op de andere PCM, totdat de mislukte module wordt vervangen.  

De behuizing van de EBOD gebruikt een 580 W PCM en primaire behuizing een 764 W PCM. De volgende tabellen staan de technische specificaties die zijn gekoppeld aan de PCMs.

| Specificatie           | 580 W PCM (EBOD)                                    | 764 W PCM (primair)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Maximale vermogen    | 580 W                                               | 764                                                |
| Frequentie               | 50/60 Hz                                            | 50/60 Hz                                           |
| Keuze van voltage range | Variërend van auto: 90-264 V Wisselstroom, 47/63 Hz               | Variërend van auto: 90-264 V AC, 47/63 Hz               |
| Maximale stroomoverbelasting huidige  | 20 A                                                | 20 A                                               |
| Power factor correctie | > nominale Ingangsspanning 95%                          | > nominale Ingangsspanning 95%                         |
| Harmonische stroom               | Voldoet aan EN61000-3-2                                   | Voldoet aan EN61000-3-2                                  |
| Uitvoer                  | Stand-by-voltage 5v @ 2.0 A                          | Stand-by-voltage 5v @ A 2.7                         |
|                         | + 5V @ 42 A                                          | + 5V @ 40 A                                         |
|                         | + 12V @ 38 A                                         | + 12V @ 38 A                                        |
| Hot pluggable           |  Ja                                                | Ja                                                |
| Schakelopties en LED 's       | Netspanning aan/uit-schakelaar en vier statusindicator LED 's     | Netspanning aan/uit-schakelaar en zes statusindicator LED 's     |
| Behuizing koeling       | Axiale koelventilatoren met een ventilator met variabele snelheid controle  | Axiale koelventilatoren met een ventilator met variabele snelheid controle |

 
## <a name="power-consumption-statistics"></a>Energie verbruiksstatistieken  

De volgende tabel worden de gegevens over het verbruik normaal power (werkelijke waarden kunnen afwijken van de gepubliceerde) voor de verschillende modellen van StorSimple apparaat. 
 
 Voorwaarden | 240 V WISSELSTROOM | 240 V WISSELSTROOM | 240 V WISSELSTROOM | 110 V WISSELSPANNING | 110 V WISSELSPANNING | 110 V WISSELSPANNING 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Ventilatoren traag, stations niet actief | 1,45 A  |0.31 kW | 1057.76 BTU/uur | 3.19 A | 0.34 kW | 1160.13 BTU/uur 
 Langzame ventilatoren, toegang tot stations | 1,54 A | 0,33 kW | 1126.01 BTU/uur | 3.27 A | 0.36 kW | 1228.37 BTU/uur 
 Ventilatoren snel, stations niet-actief, twee PSUs aangedreven | 2.14 A | 0.49 kW  | 1671.95 BTU/uur | 4,99 A | 0.54 kW | 1842.56 BTU/uur 
 Ventilatoren, snel, niet-actieve stations, één PSU van stroom voorzien van een niet-actief | 2.05 A | 0.48 kW | 1637.83 BTU/uur | 4,58 A | 0,50 kW | 1706.07 BTU/uur 
 Snelle, ventilatoren-stations benaderen, twee PSUs aangedreven | 2,26-A | 0,51 kW | 1740.19 BTU/uur | 4.95 A | 0.54 kW | 1842.56 BTU/uur 
 Snelle ventilatoren, stations benaderen, een PSU voeding een niet-actief | 2.14 A |0.49 kW | 1671.95 BTU/uur | 4.81 A  | 0.53 kW | 1808.44 BTU/uur 

## <a name="disk-drive-specifications"></a>Specificaties van de schijf  

Uw apparaat StorSimple ondersteunt maximaal 12 3,5-inch formulier factor Serial Attached SCSI (SAS) harde schijven. De werkelijke stations zijn een combinatie van solid-state schijven (SSD) of vaste-schijfstations (harde schijven), afhankelijk van de productconfiguratie. De 12 schijf sleuven bevinden zich in een configuratie met 4 3 voor de ruimte. De behuizing van de EBOD kunt u extra opslagruimte voor een andere schijfstations 12. Dit zijn altijd harde schijven.  

## <a name="storage-specifications"></a>Opslag specificaties
De StorSimple apparaten hebben een combinatie van harde schijven en stations ' Solid-State ' voor zowel de 8100 8600. De totale bruikbare capaciteit voor de 8100 en 8600 zijn ongeveer 15 TB TB 38 respectievelijk en. De volgende tabel worden de details van SSD harde schijven en capaciteit in het kader van de capaciteit van de oplossing StorSimple wolk.

| Apparaat model / capaciteit                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Het aantal vaste schijven (harde schijven)              |   8                                                     |  19                                                     |
| Aantal solid-state schijven (SSD)            |   4                                                     |  5                                                      |
| Capaciteit van één harde schijf                            |   4 TB                                                  |  4 TB                                                   |
| Enkele SSD capaciteit                            |   400 GB                                                |  800 GB                                                 |
| Onbenutte capaciteit                                 |   4 TB                                                  | 4 TB                                                    |
| Bruikbare capaciteit van harde schijf                            | 14 TB                                                   | 36 TB                                                   |
| Bruikbare SSD capaciteit                            | 800 GB                                                  | 2 TB                                                    |
| Totale bruikbare capaciteit *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Maximale oplossing capaciteit (met inbegrip van de wolk)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *De totale bruikbare capaciteit bevat de beschikbare capaciteit voor gegevens, metagegevens en buffers.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Behuizing afmetingen en gewicht specificaties  

De volgende tabellen staan de verschillende specificaties van de behuizing voor afmetingen en gewicht.  

### <a name="enclosure-dimensions"></a>Afmetingen behuizing
De volgende tabel worden de afmetingen van de behuizing in millimeters en inches.

|Behuizing |Millimeter |Inches |
|----------|------------|-------| 
| Hoogte |87.9 | 3,46 |
| Breedte tussen de flens montage | 483 | 19.02 |
| Breedte in de hoofdtekst van de behuizing | 443 | 17.44 |
| Diepte van de flens front montage aan het uiteinde van de instantie behuizing | 577 | 22.72 |
| Diepte van deelvenster bewerkingen aan het verst uiteinde van de behuizing | 630.5 | 24.82 |
| Diepte van de flens montage aan verst uiteinde van de behuizing |   603 | 23.74 |

### <a name="enclosure-weight"></a>Gewicht van de behuizing  

Afhankelijk van de configuratie, de behuizing van een volledig geladen primaire van 21 tot en met 33 kg wegen en vereist twee personen kunnen worden verwerkt. 
 
| Behuizing | Gewicht |
|-----------|--------| 
| Maximaal gewicht (afhankelijk van de configuratie) |30 kg – 33 kg |
| Leeg (geen stations gemonteerd) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Behuizing milieu specificaties  

In deze sectie worden de specificaties die betrekking hebben op het milieu van de behuizing. De temperatuur, vochtigheid, hoogte, schok, trilling, afdrukstand, veiligheid en elektromagnetische compatibiliteit (EMC) zijn in deze categorie opgenomen.  

### <a name="temperature-and-humidity"></a>Temperatuur en vochtigheid

| Behuizing        | Temperatuurbereik  | De relatieve vochtigheid | Maximale natte bol   |
|------------------|----------------------------|---------------------------|--------------------|
| Operationele      | 5° C - 35° C (41° F - 95° F)    | 20% tot 80% niet-condenserend- | 28° C (82° F)        |
| Niet-operationele  | -40° C - 70° C (40° F - 158° F) | 5% - 100% niet-condenserend  | 29° C (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luchtstroom, hoogte, schok, trilling, afdrukstand, veiligheid en EMC
 
| Behuizing          | Operationele specificaties                                                |
|--------------------|---------------------------------------------------------------------------| 
| Luchtstroom            | Systeem luchtdoorstroming is voor naar achter. Systeem moet worden gebruikt met een low-pressure, achterzijde uitlaat installatie. Gemaakt door rek deuren en belemmeringen tegendruk mag niet meer dan 5 Pascal (0,5 mm waterkolom). |
| Hoogte, operationele  | -30 meter aan 3045 meter (voet-100 tot 10.000 voet) met een maximale bedrijfstemperatuur opgeheven beoordeeld door 5 ° C boven de 7000 feet. |
| Hoogte, niet-operationele  | -305 meters tot 12.192 meter (-1,000 voet naar 40.000 voet) |
| Schok, operationele  | 5g 10 ms ½ sinus |
| Schok, niet-operationele  | 30g 10 ms ½ sinus |
| Trillingen, operationele  | 0.21g RMS 5 tot 500 Hz willekeurige |
| Trillingen, niet-operationele  | 1.04g RMS 2 tot 200 Hz in willekeurige |
| Trillingen, verplaatsing  | sinus van 3g-2-200 Hz |
| Richting en montage  | 19-inch rack mount (2 EIA-eenheden) |
| Rackrails  | Aangepast aan de diepte van minimaal 700 mm (31.50 inch) rekken conform IEC 297 |
| Veiligheid en goedkeuringen  |   CE en UL EN 61000-3 IEC 61000-3, UL 61000-3 |
| EMC  | EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Naleving van internationale standaarden
Het Microsoft Azure StorSimple apparaat voldoet aan de volgende internationale normen:  

- CE - EN 60950-1  
- CB-rapport volgens IEC 60950-1  
- UL en cUL UL 60950-1  

## <a name="safety-compliance"></a>Naleving van de veiligheid  

Het Microsoft Azure StorSimple apparaat voldoet aan de volgende beoordelingen van de veiligheid:  

- Systeem-productgoedkeuring: UL, cUL, CE  
- Naleving van de veiligheid: UL 60950 IEC 60950, EN 60950  

## <a name="emc-compliance"></a>EMC-compatibiliteit 

Het Microsoft Azure StorSimple apparaat voldoet aan de volgende EMC beoordelingen.  

### <a name="emissions"></a>Emissies

Het apparaat is compatibel met EMC voor emissieniveau uitgevoerd en uitgezonden.  

- Uitgevoerde emissies niveaus beperken: CFR 47 Part 15 ter klasse A EN55022 klasse A CISPR klasse A  
- Uitgezonden emissies niveaus beperken: CFR 47 Part 15 ter klasse A EN55022 klasse A CISPR klasse A   

### <a name="harmonics-and-flicker"></a>Harmonischen en knipperen  

Het apparaat voldoet aan de EN61000-3-2-3.  

### <a name="immunity-limit-levels"></a>Immuniteit limiet niveaus  
Het apparaat voldoet aan de EN55024.  

## <a name="ac-power-cord-compliance"></a>AC power cord naleving
  
De plug en de assemblage van complete voeding snoer moeten voldoen aan de normen van het land waarin het apparaat wordt gebruikt en veiligheidskeurmerken die acceptabel in dat land zijn moet hebben. De volgende tabellen staan de normen voor de Verenigde Staten van Amerika en Europa.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC-stekkers - Verenigde Staten van Amerika (zijn NRTL vermeld)

| Onderdeel       | Specificatie                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Type kabel       | Maximale lengte van AVP of SVT, 18 AWG minimum, 3 geleider 2.0 meter |
| Plug            | NEMA 5-15P basiskennis type bijlage plug geclassificeerd 120 V 10 A; of IEC 320 C14 250 V, 10 A |
| Socket          | IEC 320 C-13, 250 V 10 A                                         |

### <a name="ac-power-cords---europe"></a>AC-stekkers - Europa

| Onderdeel       | Specificatie                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Type kabel       | Geharmoniseerde, H05-VVF-3G1.0                                         |
| Socket          | IEC 320 C-13, 250 V 10 A                                         |

## <a name="supported-network-cables"></a>Ondersteunde kabels  

Raadpleeg voor de 10 GbE-netwerkinterfaces, gegevens 2 en 3 van de gegevens, de [lijst van ondersteunde netwerkkabels en modules](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Volgende stappen

U bent nu gereed voor de implementatie van een StorSimple apparaat in uw datacenter. Voor meer informatie, Zie [het apparaat op gebouwen](storsimple-deployment-walkthrough-u2.md).  
