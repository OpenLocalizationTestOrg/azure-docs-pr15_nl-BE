<properties
   pageTitle="Installeer het apparaat StorSimple 8600 | Microsoft Azure"
   description="Beschrijving van het uitpakken, rack-mount en de kabel van het apparaat StorSimple 8600 voordat u implementeren en configureren van de software."
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
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Uitpakken, rack-mount en de kabel van het apparaat StorSimple 8600

## <a name="overview"></a>Overzicht
Uw Microsoft Azure StorSimple 8600 dual behuizing is en dat bestaat uit een primaire en een EBOD behuizing. In deze zelfstudie wordt uitgelegd hoe u uitpakken, rek en de StorSimple 8600 kabel apparaat hardware voordat u de software StorSimple configureren.

## <a name="unpack-your-storsimple-8600-device"></a>Het apparaat StorSimple 8600 uitpakken

De volgende stappen bevatten duidelijke, gedetailleerde instructies voor het uitpakken van uw opslagapparaat StorSimple 8600. Dit apparaat wordt geleverd in twee vakken, één voor de primaire behuizing en een andere voor de EBOD behuizing. Deze twee vakken worden vervolgens in één vak geplaatst.

### <a name="prepare-to-unpack-your-device"></a>Voorbereiden voor het uitpakken van het apparaat

Lees de volgende informatie voordat u bij het uitpakken van het apparaat.


![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![pictogram zwaar gewicht](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Waarschuwing!**

1. Zorg ervoor dat er twee mensen beschikbaar zijn voor het beheer van het gewicht van het apparaat als u deze handmatig worden verwerkt. Een volledig geconfigureerde behuizing kan maximaal 32 kg (70 lbs) wegen.
1. Het vak plaatsen op een plat vlak niveau.

De volgende stappen uit om het uitpakken van het apparaat vervolgens voltooien.

#### <a name="to-unpack-your-device"></a>Uitpakken van het apparaat

1. Het vak en het schuim verpakking voor crushes, delen, water schade of andere duidelijke schade te controleren. Open het niet als het vak of de verpakking ernstig is beschadigd. Neem [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) om te beoordelen of het apparaat in goede staat is.

2. Het buitenste openen en uitvoeren van de twee vakken die overeenkomt met primaire en EBOD behuizingen. U kunt nu de primaire en behuizingen EBOD uitpakken. De volgende afbeelding ziet u de uitgepakte weergave van een van de bijlagen.

    ![Uw opslagapparaat uitpakken](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **Uitgepakte weergave van uw opslagapparaat**

     Label | Beschrijving
     ----- | -------------
     1     | Vak verpakking
     2     | SAS-kabels (in Bureau-accessoires en kabels lade)
     3     | Onder schuim
     4     | Apparaat
     5     | Bovenste schuim
     6     | Vak voor accessoires

3. Zorg ervoor dat er na het uitpakken van de twee vakken:

  - 1 primaire behuizing (de primaire behuizing en de behuizing van de EBOD zijn in twee afzonderlijke vakken)
  - 1 EBOD-behuizing
  - 4 stroomsnoeren 2 in elk vak
  - 2 SAS-kabels (met de behuizing van de primaire EBOD behuizing)
  - 1 crossover Ethernet-kabel
  - 2 seriële console kabels
  - 1 seriële USB converter voor seriële toegang
  - 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
  - 2 rack mount Kit (4 kant rails met montagehardware, 2 voor de primaire behuizing en EBOD behuizing), 1 in elk vak
  - Ophalen van gestart

    Als u een van de hierboven vermelde items niet hebt ontvangen [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md).  

De volgende stap is het apparaat voor rackmontage.

## <a name="rack-mount-your-storsimple-8600-device"></a>Het apparaat StorSimple 8600 voor rackmontage

De volgende stappen als u wilt uw opslagapparaat StorSimple 8600 installeren in een standaard 19-inch rack met vooraan en achteraan-advertenties. Dit apparaat wordt geleverd met twee gedeelten: een primaire behuizing en een EBOD behuizing. Deze moeten rack gemonteerd.

De installatie bestaat uit meerdere stappen, die in de volgende procedures wordt beschreven.

> [AZURE.IMPORTANT]
StorSimple apparaten moeten voor rackmontage voor een juiste werking.



### <a name="site-preparation"></a>Site-voorbereiding

De bijlagen moeten worden geïnstalleerd in een standaard 19-inch rack met voor- en achterzijde advertenties. Gebruik de volgende procedure voor het voorbereiden voor installatie in rack.

#### <a name="to-prepare-the-site-for-rack-installation"></a>De site voorbereiden voor installatie in rack

1. Controleer of de primaire- en EBOD-behuizingen zijn veilig op een vlakke, stabiele en niveau werkoppervlak verwerken (of vergelijkbaar).

2. Controleer of de site waar u van plan bent voor het instellen van standaard netstroom van een onafhankelijke bron of een rek Power Distribution Unit (PDU) met een UPS (uninterruptible power supply).

3. Zorg ervoor dat een 4U (2 X 2U)-sleuf is beschikbaar op het rek waarop u van plan bent te koppelen van de bijlagen.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![pictogram zwaar gewicht](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Waarschuwing!**

 Zorg ervoor dat er twee mensen beschikbaar zijn voor het beheer van het gewicht als u de instellingen van het apparaat handmatig verwerkt. Een volledig geconfigureerde behuizing kan maximaal 32 kg (70 lbs) wegen.

### <a name="rack-prerequisites"></a>Rack-vereisten

De behuizingen zijn ontworpen voor installatie in een standaard 19-inch rack cab met:

- Minimale diepte van 27.84 inch van rek te boeken
- Maximaal gewicht van 32 kg voor het apparaat
- Maximale tegendruk van 5 Pascal (0,5 mm waterkolom)

### <a name="rack-mounting-rail-kit"></a>Rek-montage railkit

Reeks rails montage geleverd voor gebruik met de 19-inch rack cabinet. De rails zijn getest voor het verwerken van het gewicht van de maximale ruimte. Deze rails kunt ook de installatie van meerdere bijlagen zonder verlies van de ruimte binnen het rack. Installeer eerst de EBOD behuizing.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>De behuizing van de EBOD op de rails installeren

2. Deze stap alleen uitvoeren als interne rails zijn niet geïnstalleerd op uw apparaat. De binnenste rails worden meestal geïnstalleerd in de fabriek. Als rails niet zijn geïnstalleerd, installeer vervolgens links spoor en recht spoor dia's op de zijkanten van het chassis van de behuizing. Ze koppelen met behulp van zes metrische schroeven aan elke kant. Om te helpen met de afdrukstand, het spoor dia's zijn gemarkeerd **LH-voorzijde** en **RH-voorzijde**en het einde die is aangebracht vanaf de achterzijde van de behuizing heeft een taps toelopend uiteinde.

    ![Spoor dia's koppelen aan chassis behuizing](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Spoor dia's koppelen aan de zijkanten van de behuizing**

    Label | Beschrijving
    ----- | -----------
    1     | 3 x 4 M knop head schroeven
    2     | Chassis dia 's

3. Het spoor links en rechts spoor samenstellen aan de leden rack cab verticale koppelen. De haakjes zijn gemarkeerd **LH** **RH**en **deze kant naar boven** om u te begeleiden door middel van de juiste richting.

4. Zoek de pinnen per spoor op de voor- en achterzijde van de assemblage van het spoor. De spoorstaaf past tussen de advertenties rack en plaats de pinnen in de voorzijde en achterzijde rack boeken lid verticale gaten uitbreiden. Zorg ervoor dat de assembly spoor niveau.

5. Veilig de assemblage van het spoor op het rek verticale leden met behulp van twee van de metrische schroeven geleverd. Gebruik één schroef op de voorkant en één aan de achterzijde.

6. Herhaal deze stappen voor de andere rail montage.

     ![Spoor dia's koppelen aan rack cab](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Spoor assembly's koppelen aan het rek**

     Label | Beschrijving
     ----- | -----------
     1     | Schroef klemmen
     2     | Voorste rek vierkant gat post schroef
     3     | Links front spoor locatie pennen
     4     | Schroef klemmen
     5     | Linker achterzijde spoor locatie pennen

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>De behuizing van de EBOD in het rack monteren

De volgende stappen uit om de EBOD ruimte in het rek te koppelen met de rackrails die zojuist zijn geïnstalleerd, worden uitgevoerd.

#### <a name="to-mount-the-ebod-enclosure"></a>Voor het koppelen van de EBOD-behuizing

1. Met een helper, tilt u de ruimte en met de rackrails uitlijnen.

2. Zorgvuldig de ruimte in de spoorstaven invoegen en vervolgens duwen volledig in het rack cab.

    ![Apparaat invoegen in het rek](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **De ruimte in het rack monteren**

3. De links en rechts front flens caps omdat de vrije caps op verwijderen. De flens caps snap gewoon naar de flenzen.

4. De ruimte in het rack beveiligen door het installeren van een opgegeven kruiskopschroevendraaier hoofd schroef via elke flens, links en rechts.

4. Installeer de flens caps door ze naar de gewenste positie en ze magnetisch naar hun plaats.

     ![Flens caps installeren](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **Installeren van de flens-caps**

     Label | Beschrijving
     ----- | -----------
     1     | Behuizing sluitzegel schroef


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>De primaire ruimte in het rack monteren

Nadat u klaar bent met de montage van de behuizing van de EBOD, moet u de primaire ruimte om te koppelen.

> [AZURE.NOTE]
>
> - Het is mogelijk om een paar lege sleuven in het rek tussen de primaire behuizing en de EBOD.
> - Gebruik de meegeleverde 2m SAS-kabel verbinding maken met de primaire behuizing met de EBOD behuizing.
> - Er zijn geen beperkingen op de relatieve plaatsing van de hoofd-eenheid aan de eenheid van de EBOD. Daarom kan de primaire behuizing worden geplaatst in de bovenste sleuf en de EBOD hieronder, of vice versa.

De volgende stap is het apparaat voor de energie-, netwerk- en seriële kabel.

## <a name="cable-your-storsimple-8600-device"></a>Kabel van het apparaat StorSimple 8600

De volgende procedures wordt uitgelegd hoe het apparaat StorSimple 8600 voor energie-, netwerk- en seriële verbindingen met een kabel.

### <a name="prerequisites"></a>Vereisten

Voordat u het apparaat met een kabel, hebt u nodig:

- Uw primaire behuizing en de EBOD, volledig uitgepakt.
- 4 power kabels (2 elk voor de primaire en de EBOD) die bij uw apparaat is geleverd
- 2 SAS-kabels meegeleverd met het apparaat verbinding maken met de behuizing van de EBOD de primaire behuizing
- Toegang tot 2 Power Distribution Units (PDU's) (aanbevolen)
- Netwerkkabels
- Seriële kabels geleverd
- Seriële USB converter met het juiste stuurprogramma geïnstalleerd op uw PC (indien nodig)
- 4 QSFP verstrekt-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
- [Ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw apparaat StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS en Power kabels

Het apparaat is zowel een primaire behuizing en een EBOD behuizing. Moet hiervoor de eenheden die samen worden kabels voor Serial Attached SCSI (SAS) verbinding en geen stroom.

Wanneer u dit apparaat voor de eerste keer instelt, voer de stappen uit voor SAS kabels eerst en vervolgens de stappen voor power kabels.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Netwerkkabels

Het apparaat is in een actieve stand-by-configuratie: op een bepaald moment één controllermodule actief is en verwerkingen alle schijf en netwerk terwijl de andere controllermodule is op stand-by. Als een domeincontroller een fout optreedt, wordt de stand-by-controller onmiddellijk wordt geactiveerd en blijft alle schijf en netwerk activiteiten.

Ter ondersteuning van deze redundante controller failover, moet u de kabel van uw netwerk apparaat zoals aangegeven in de volgende stappen uit.

#### <a name="to-cable-for-network-connection"></a>Kabel voor netwerkverbinding

1. Uw apparaat heeft zes netwerkinterfaces op elke domeincontroller: vier 1 Gbps en twee 10 Gbps Ethernet-poorten. Zie de volgende afbeelding voor het identificeren van de gegevenspoorten op het backplane van uw apparaat.

     ![Backplane 8600 apparaat](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Achterzijde van het apparaat met de gegevens-poort**

     Label   | Beschrijving
     ------- | -----------
     0,1,4,5 |  1 GbE-netwerkinterfaces
     2,3     | 10 GbE-netwerkinterfaces
     6       | Seriële poorten



1. Zie het volgende diagram voor netwerkbekabeling. (De minimale netwerkconfiguratie wordt weergegeven door een ononderbroken blauwe lijnen. Voor hoge beschikbaarheid en prestaties, aanvullende configuratie vereist aangegeven door onderbroken lijnen.)

![Uw apparaat 4U voor netwerk kabel](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Netwerk bekabeling voor uw apparaat**

Label | Beschrijving
----- | -----------
A    | LAN met toegang tot het Internet
B    | 0-controller
C    | PCM-0
D    | 1-controller
E    | PCM-1
F    | EBOD-controller 0
G    | 1 EBOD-controller
H, I  | Hosts (zoals bestandsservers)
0-5  | Netwerkinterfaces
6    | Primaire behuizing
7    | EBOD-behuizing

Wanneer het apparaat bekabeling, is de minimale configuratie vereist:


- Ten minste twee netwerkinterfaces worden aangesloten op elke domeincontroller met een wolk en een voor iSCSI. De gegevens van 0-poort wordt automatisch ingeschakeld en geconfigureerd via de seriële console van het apparaat. 0-gegevens, met uitzondering van moet een andere poort ook worden geconfigureerd via de klassieke Azure portal. In dit geval verbinding gegevens 0 poort met het primaire LAN (netwerk met toegang tot het Internet). De data-poorten kunnen worden verbonden met SAN/iSCSI-LAN (VLAN) segment van het netwerk, afhankelijk van de gewenste rol.

- Identieke interfaces op elke controller is verbonden met hetzelfde netwerk om de beschikbaarheid te garanderen als een controller failover plaatsvindt. Bijvoorbeeld, als u de verbinding gegevens 0 en 3 van de gegevens voor een van de domeincontrollers, moet u verbinding maken met de bijbehorende gegevens 0 en 3 van de gegevens op de andere domeincontroller.

Houd in gedachten voor hoge beschikbaarheid en prestaties:


- Configureren, indien mogelijk, een paar netwerkinterface voor een cloud-toegang (1 GbE) en een ander paar voor iSCSI (10 GbE aanbevolen) op elke domeincontroller.

- Indien mogelijk verbinding te activeren wordt de beschikbaarheid tegen een schakeloptie netwerkinterfaces van elke domeincontroller. De afbeelding ziet u de twee 10 GbE netwerkinterfaces, gegevens 2 en 3 van de gegevens, vanaf elke domeincontroller die is aangesloten op twee verschillende schakelopties. Raadpleeg voor meer informatie de **Netwerkinterfaces** volgens de [vereisten voor uw apparaat StorSimple hoge beschikbaarheid](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Als +-SFP-transceivers met de 10 GbE-netwerkinterfaces, gebruikt u de meegeleverde QSFP-SFP + adapters. Ga voor meer informatie naar [ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw apparaat StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

### <a name="serial-port-cabling"></a>Bekabeling van de seriële poort

Voer de volgende stappen uit om de kabel van de seriële poort.

#### <a name="to-cable-for-serial-connection"></a>Kabel voor seriële verbinding

1. Uw apparaat heeft een seriële poort op elke domeincontroller die wordt aangeduid met een moersleutelpictogram van een. Als u de seriële poorten, verwijzen naar de afbeelding de gegevens poorten op de achterkant van het apparaat toont.

2. Identificeer de active controller op uw apparaat backplane. Een knipperende blauwe LED geeft aan dat de domeincontroller actief is.

3. Gebruik de meegeleverde seriële kabel (indien nodig, de USB-serieel converter voor uw laptop) en uw console of een computer (met terminal-emulatiesoftware aan op het apparaat) verbinding met de seriële poort van de actieve domeincontroller.

4. De seriële USB-stuurprogramma's (geleverd bij het apparaat) op uw computer installeren.

5. De seriële verbinding instellen als volgt:
   - 115.200 baud
   - 8 databits
   - 1 stopbit
   - Geen pariteit
   - Ingesteld op **geen** datatransportbesturing

6. Controleer of de verbinding werkt door te drukken op Enter op de console. Een seriële console-menu moet worden weergegeven.

> [AZURE.NOTE] **Lights-Out Management:** Wanneer het apparaat is geïnstalleerd in een extern datacenter of in een computerruimte met beperkte toegang, moet u ervoor zorgen dat de seriële verbindingen met beide domeincontrollers altijd met een seriële console-switch of soortgelijke werktuigen verbonden bent. Hierdoor kan de out-of-band extern beheer en ondersteuning voor bewerkingen in geval van verstoring van de netwerk- of onverwachte storingen.

Kabels van het apparaat voor voeding, toegang tot het netwerk en seriële verbinding is voltooid. De volgende stap is het configureren van de software op uw apparaat.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te [implementeren en configureren van het apparaat op gebouwen StorSimple](storsimple-deployment-walkthrough-u2.md).
