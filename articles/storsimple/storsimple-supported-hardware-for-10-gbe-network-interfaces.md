<properties 
   pageTitle="Hardware voor StorSimple 10 GbE-netwerkinterfaces | Microsoft Azure"
   description="Beschrijving van de ondersteunde kleine vorm-factor pluggable (SFP)-transceivers, kabels en schakelopties voor de 10 GbE-netwerkinterfaces op uw apparaat StorSimple."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

Dit artikel bevat informatie over aanvullende hardware aan die met uw Microsoft Azure StorSimple apparaat samenwerkt.

## <a name="list-of-devices-tested-by-microsoft"></a>Lijst met apparaten die zijn getest door Microsoft

Microsoft heeft de volgende kleine vorm-factor pluggable (SFP)-transceivers, kabels en switches om ervoor te zorgen dat ze optimaal met apparaten werken getest. (In de volgende tabellen worden bijgewerkt als er nieuwe hardware is getest.)

### <a name="sfp-transceivers"></a>+-SFP-Transceivers

|Maken|Model|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Kabels

|S. Nr. |Maken|Model|
|---|---|---|
| 1.|Cisco|SFP-H10GB-CU1M|
| 2.|Cisco|SFP-H10GB-CU2M|
| 3.|Cisco|SFP-H10GB-CU3M|
| 4.|Tripp Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Switches

|S. Nr.|Maken|Model|
|---|---|---|
| 1. |Cisco|N3K-C3172PQ-10GE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K-C5596UP-VA|

## <a name="list-of-devices-tested-in-the-field"></a>Lijst met apparaten die zijn getest in het veld

Deze sectie bevat de lijst met apparaten die zijn met succes geïmplementeerd in het veld StorSimple klanten. Deze zijn niet getest door Microsoft, maar waarschijnlijk StorSimple apparaat.
 
| Parameter                         | Waarde                                    |
|-----------------------------------|------------------------------------------|
| Switch maken                     | Jeneverbes                                  |
| Model switch                    | ex4550 32F                               |
| Switch-besturingssysteem | JunOS 12.3R9.4                           |
| Blade-model                     | Poorten on-board PIC (0)                    |
| Type transceiver                  | Jeneverbes                                  |
| Model transceiver               | Onderdeelnummer 740-021308 <br></br> Onderdeelnummer 740-030658                   |
| Firmwareversie transceiver    | Rev 01 versie 0,0 (gerapporteerd)            |
| Kabel-model                     | Duplex jumper LC/LC 50/125µ, OM3, LSZH |
| Model StorSimple                | 8600                                     |
| StorSimple softwareversie     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Lijst met apparaten die zijn getest door OEM-leverancier (Mellanox)  

Mellanox heeft de volgende kleine vorm-factor pluggable (SFP)-transceivers, kabels en switches om ervoor te zorgen dat zij optimaal met netwerkinterfaces Mellanox zoals de 10 GbE-netwerkinterfaces op uw apparaat StorSimple functioneren getest.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kabels en modules ondersteund door Mellanox 

De volgende tabel worden de kabels en modules ondersteund door Mellanox. Deze zijn niet getest door Microsoft, maar waarschijnlijk StorSimple apparaat.

| S. Nr. | Snelheid | Model                 | Beschrijving                                            | Maken                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB-SFP-SFP - 1M        | passieve Koperen SFP + 10 Gb/s 1m kabel                   | Arista                |
| 2.     | 10 GbE| CAB-SFP-SFP - 2M        | passieve Koperen SFP + 10 Gb/s-2m kabel                   | Arista                |
| 3.     | 10 GbE| CAB-SFP-SFP - 3M        | passieve Koperen SFP + 10 Gb/s 3m kabel                   | Arista                |
| 4.     | 10 GbE| CAB-SFP-SFP - 5M        | passieve Koperen SFP + 10 Gb/s-5m kabel                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + kabel                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + kabel                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + kabel                                       | Cisco                 |
| 8.     | 10 GbE| HP J9281B X242 10 G    | SFP + naar SFP + 1m Direct Attach koperen kabel             | HP                    |
| 9.     | 10 GbE| 455883-B21 HP BLc     | 10Gb SR SFP + Opt                                       | HP                    |
| 10.    | 10 GbE| 455886-B21 HP BLc     | 10Gb LR SFP + Opt                                       | HP                    |
| 11.    |10 GbE | 487649-B21 HP BLc     | SFP + 0,5 m 10 GbE koperen kabel                           | HP                    |
| 12.    |10 GbE | 487652-B21 HP BLc     | SFP + 1m 10 GbE koperen kabel                             | HP                    |
| 13.    |10 GbE | 487655-B21 HP BLc     | SFP + 3m 10 GbE koperen kabel                             | HP                    |
| 14.    |10 GbE | 487658-B21 HP BLc     | SFP + 7m 10 GbE koperen kabel                             | HP                    |
| 15.    |10 GbE | 537963-B21 HP BLc     | SFP + 5m 10 GbE koperen kabel                             | HP                    |
| 16.    |10 GbE | HP AP784A             | C-serie passieve Koperen SFP + kabel van 3 meter                  | HP                    |
| 17.    |10 GbE | HP AP785A             | C-serie passieve Koperen SFP + kabel van 5m                  | HP                    |
| 18.    |10 GbE | HP AP818A             | B-reeks Active Koperen SFP + kabel van 1 meter                   | HP                    |
| 19.    |10 GbE | HP AP819A             | B-reeks Active Koperen SFP + kabel van 3 meter                   | HP                    |
| 20.    |10 GbE | HP J9150A             | X132 10 G SFP + LC SR Transceiver                        | HP                    |
| 21.    |10 GbE | HP J9151A             | X132 10 G SFP + LC LR Transceiver                        | HP                    |
| 22.    |10 GbE | HP J9283B             | X242 10 G SFP + SFP + DAC kabel van 3 meter                        | HP                    |
| 23.    |10 GbE | HP J9285B             | X242 10 G SFP + SFP + 7 m DAC-kabel                        | HP                    |
| 24.    | 10 GbE| HP JD095B             | X240 10 G SFP + SFP + 0.65 m DAC-kabel                     | HP                    |
| 25.    | 10 GbE| HP JD096B             | X240 10 G SFP + SFP + 1,2 m DAC-kabel                      | HP                    |
| 26.    | 10 GbE| HP JD097B             | X240 10 G SFP + SFP + Pa kabel van 3 meter                        | HP                    |
| 27.    | 10 GbE| Mellanox MAM1Q00A QSA | QSFP SFP + adapter                                   | Mellanox technologieën |
| 28.    | 10 GbE| MC2309124-006-Mt      | X SFP+ passieve koperen kabel 1 tot 10 Gb/s-24awg QSFP 7 m   | Mellanox technologieën |
| 29.    | 10 GbE| MC2309124-007-Mt      | X SFP+ passieve koperen kabel 1 tot 10 Gb/s-24awg QSFP 7 m   | Mellanox technologieën |
| 30.    | 10 GbE| Mt MC2309130-003      | X SFP+ passieve koperen kabel 1 tot 10 Gb/s-30awg QSFP 3 m   | Mellanox technologieën |
| 31.    | 10 GbE| Mt MC2309130 00A      | Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 30awg 0,5 m | Mellanox technologieën |
| 32.    | 10 GbE| MC3309124-005 Mt      | Passieve koper 1 x SFP+ 10 Gb/s 24awg kabel 5 m           | Mellanox technologieën |
| 33.    | 10 GbE| MC3309124-007-Mt      | Passieve koperen kabel 1 x SFP+ 10 Gb/s 24awg 7 m           | Mellanox technologieën |
| 34.    | 10 GbE| Mt MC3309130-003      | 1 x SFP+ 10 Gb/s 30awg een passieve koperen kabel 3 m           | Mellanox technologieën |
| 35.    | 10 GbE| Mt MC3309130 00A      | Passieve koperen kabel 1 x SFP+ 10 Gb/s 30awg 0,5 m         | Mellanox technologieën |

### <a name="switches-supported-by-mellanox"></a>Schakelopties worden ondersteund door Mellanox 

De volgende tabel staan de schakelopties die worden ondersteund door Mellanox. Deze zijn niet getest door Microsoft, maar waarschijnlijk StorSimple apparaat.

| S. Nr. | Snelheid | Model | Beschrijving                                                         | Maken              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10 GbE | 516733-B21  | HP ProCurve 6120XG 10GbE Ethernet-Blade Switch                      | HP    |
| 2.     | 10 GbE | 538113-B21  | HP 10 GbE Pass-Through-Module (PTM)                                  | HP    |
| 3.     | 10 GbE | EN4093      | IBM PureFlex Fabric EN4093 10 Gigabit-Switch schaalbare Systeemmodule | IBM   |
| 4.     | 1GbE  | 3020        | Cisco Catalyst 3020 1GbE-switch, blade                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Cisco Catalyst 3020 X 1GbE-switch, blade                              | Cisco |
| 6.     | 1GbE  | 438030-B21  | 1GbE schakelaar module voor HP - GbE2c Layer 2/3 Ethernet-Blade Switch       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE-switch, blade                              | HP    |

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over hardwareonderdelen StorSimple en status](storsimple-monitor-hardware-status.md).
