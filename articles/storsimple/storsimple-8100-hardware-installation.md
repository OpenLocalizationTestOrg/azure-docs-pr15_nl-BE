<properties
   pageTitle="Installeer het apparaat StorSimple 8100 | Microsoft Azure"
   description="Beschrijving van het uitpakken, rack-mount en de kabel van het apparaat StorSimple 8100 voordat u implementeren en configureren van de software."
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

# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Uitpakken, rack-mount en de kabel van het apparaat StorSimple 8100

## <a name="overview"></a>Overzicht

Uw Microsoft Azure StorSimple 8100 is een één behuizing, rackmontage apparaat. In deze zelfstudie wordt uitgelegd hoe u uitpakken, rek en de StorSimple 8100 kabel apparaat hardware voordat u configureren en implementeren van het apparaat StorSimple.

## <a name="unpack-your-storsimple-8100-device"></a>Het apparaat StorSimple 8100 uitpakken

De volgende stappen bevatten duidelijke, gedetailleerde instructies voor het uitpakken van uw StorSimple 8100-opslagapparaat. Dit apparaat wordt geleverd in één vak.

### <a name="prepare-to-unpack-your-device"></a>Voorbereiden voor het uitpakken van het apparaat

Lees de volgende informatie voordat u bij het uitpakken van het apparaat.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![pictogram zwaar gewicht](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Waarschuwing!**

1. Zorg ervoor dat er twee mensen beschikbaar zijn voor het beheer van het gewicht van de behuizing als u deze handmatig worden verwerkt. Een volledig geconfigureerde behuizing kan maximaal 32 kg (70 lbs) wegen.
1. Het vak plaatsen op een plat vlak niveau.

De volgende stappen uit om het uitpakken van het apparaat vervolgens voltooien.

#### <a name="to-unpack-your-device"></a>Uitpakken van het apparaat

1. Het vak en het schuim verpakking voor crushes, delen, water schade of andere duidelijke schade te controleren. Open het niet als het vak of de verpakking ernstig is beschadigd. Neem [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) om te beoordelen of het apparaat in goede staat is.

2. Verzenddozen. De volgende afbeelding ziet u de uitgepakte weergave van uw apparaat StorSimple.

     ![Uw opslagapparaat uitpakken](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **Uitgepakte weergave van uw opslagapparaat**

     Label | Beschrijving
     ----- | -------------
     1     | Vak verpakking
     2     | Onder schuim
     3     | Apparaat
     4     | Bovenste schuim
     5     | Vak voor accessoires


3. Zorg ervoor dat er na het uitpakken van het vak:

   - 1 één behuizing apparaat
   - 2 stekkers
   - 1 crossover Ethernet-kabel
   - 2 seriële console kabels
   - 1 seriële USB converter voor seriële toegang
   - 1 fraudebestendig T10 schroevendraaier
   - 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces
   - 1 rack-mount kit (2 kant rails met montagehardware)
   - Getting Started-documentatie

    Als u een van de hierboven vermelde items niet hebt ontvangen [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md).

De volgende stap is het apparaat voor rackmontage.

## <a name="rack-mount-your-storsimple-8100-device"></a>Het apparaat StorSimple 8100 voor rackmontage

De volgende stappen als u wilt uw opslagapparaat StorSimple 8100 installeren in een standaard 19-inch rack met vooraan en achteraan-advertenties. Het apparaat StorSimple 8100 heeft één primaire behuizing.

De installatie bestaat uit meerdere stappen, die in de volgende procedures wordt beschreven.

> [AZURE.IMPORTANT]
StorSimple apparaten moeten voor rackmontage voor een juiste werking.

### <a name="prepare-the-site"></a>De website voorbereiden

Het apparaat moet worden geïnstalleerd in een standaard 19-inch rack met voor- en achterzijde advertenties. Gebruik de volgende procedure voor het voorbereiden voor installatie in rack.

#### <a name="to-prepare-the-site-for-rack-installation"></a>De site voorbereiden voor installatie in rack

1. Zorg ervoor dat het apparaat veilig op een vlakke, stabiele en niveau werk oppervlak (of vergelijkbaar) gelegen.

2. Controleer of de site waar u van plan bent voor het instellen van standaard netstroom van een onafhankelijke bron of een rek power distribution Units (PDU's) met een UPS (uninterruptible power supply).

3. Zorg ervoor dat een 2U-sleuf is beschikbaar op het rek waarop u van plan bent het apparaat te koppelen.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![pictogram zwaar gewicht](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Waarschuwing!**

Zorg ervoor dat er twee mensen beschikbaar zijn voor het beheer van het gewicht als u de instellingen van het apparaat handmatig verwerkt. Een volledig geconfigureerde behuizing kan maximaal 32 kg (70 lbs) wegen.

### <a name="rack-prerequisites"></a>Rack-vereisten

De behuizing 8100 is ontworpen voor installatie in een standaard 19-inch rack cab met:

- Minimale diepte van 27.84 inch van rek te boeken.
- Maximaal gewicht van 32 kg voor het apparaat
- Maximale tegendruk van 5 Pascal (0,5 mm waterkolom).

### <a name="rack-mounting-rail-kit"></a>Rek-montage railkit

Een aantal rails montage geleverd voor gebruik met de 19-inch rack CAB-bestand. De rails zijn getest voor het verwerken van het gewicht van de maximale ruimte. Deze rails kunt ook de installatie van meerdere bijlagen zonder verlies van de ruimte binnen het rack.


#### <a name="to-install-the-device-on-the-rails"></a>Het apparaat op de rails installeren

2. Deze stap alleen uitvoeren als interne rails zijn niet geïnstalleerd op uw apparaat. De binnenste rails worden meestal geïnstalleerd in de fabriek. Als rails niet zijn geïnstalleerd, installeer vervolgens links spoor en recht spoor dia's op de zijkanten van het chassis van de behuizing. Ze koppelen met behulp van zes metrische schroeven aan elke kant. Om te helpen met de afdrukstand, het spoor dia's zijn gemarkeerd **LH-voorzijde** en **RH-voorzijde**en het einde die is aangebracht vanaf de achterzijde van de behuizing heeft een taps toelopend uiteinde.<br/>

    ![Spoor dia's koppelen aan behuizing chassis](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
    **Attaching inner spoor dia's op de zijkanten van de behuizing**

       Label | Beschrijving
       ----- | -----------
       1     | 3 x 4 M knop head schroeven
       2     | Chassis dia 's

3. De buitenste linker rail en right outer-rail assembly's aan de leden rack cab verticale koppelen. De haakjes zijn gemarkeerd **LH** **RH**en **deze kant naar boven** om u te begeleiden door middel van de juiste stand.

4. Zoek de pinnen per spoor op de voor- en achterzijde van de assemblage van het spoor. De spoorstaaf past tussen de advertenties rack en plaats de pinnen in de voor- en bagagedrager boeken lid verticale gaten uitbreiden. Zorg ervoor dat de assembly spoor niveau.

5. Twee van de opgegeven metrische schroeven gebruiken voor het beveiligen van de assemblage van het spoor op het rek verticale leden. Gebruik één schroef op de voorkant en één aan de achterzijde.

6. Herhaal deze stappen voor de andere rail montage.<br/>

     ![Spoor dia's koppelen aan rack cab](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Buitenste spoor assembly's koppelen aan het rek**

     Label | Beschrijving
     ----- | -----------
     1     | Schroef klemmen
     2     | Voorste rek vierkant gat post schroef
     3     | Spoor links front locatie pennen
     4     | Schroef klemmen
     5     | Links spoor achter locatie pennen


### <a name="mounting-the-device-in-the-rack"></a>Het apparaat in het rack monteren

Met behulp van het rackrails die zojuist zijn geïnstalleerd, voeren de volgende stappen uit om het apparaat in het rek te koppelen.

#### <a name="to-mount-the-device"></a>Het apparaat koppelen

1. Met een helper, tilt u de ruimte en met de rackrails uitlijnen.

2. Zorgvuldig plaats het apparaat in de spoorstaven, en vervolgens duw het apparaat volledig in het rack cab.<br/>

    ![Apparaat invoegen in het rek](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Het apparaat in het rack monteren**


3. De links en rechts front flens caps omdat de vrije caps op verwijderen. De flens caps snap gewoon naar de flenzen.

5. De ruimte in het rack beveiligen door het installeren van een opgegeven kruiskopschroevendraaier hoofd schroef via elke flens, links en rechts.

4. De flens caps installeren door ze naar de gewenste positie en ze magnetisch uitlijnen op zijn plaats.<br/>

     ![Flens caps installeren](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)

    **Installeren van de flens-caps**

     Label | Beschrijving
     ----- | -----------
     1     | Behuizing sluitzegel schroef

De volgende stap is het apparaat voor de energie-, netwerk- en seriële kabel.

## <a name="cable-your-storsimple-8100-device"></a>Het apparaat StorSimple 8100-kabel

De volgende procedures wordt uitgelegd hoe het apparaat StorSimple 8100 voor energie-, netwerk- en seriële verbindingen met een kabel.

### <a name="prerequisites"></a>Vereisten

Voordat u de bekabeling van uw apparaat, hebt u nodig:

- Uw opslagapparaat, volledig uitgepakt en rek worden gemonteerd.

- 2 elektriciteitssnoeren die bij het apparaat

- Toegang tot 2 Power Distribution Units (aanbevolen).

- Netwerkkabels

- Seriële kabels geleverd

- Seriële USB-conversieprogramma met het juiste stuurprogramma geïnstalleerd op uw PC (indien nodig)

- 4 QSFP verstrekt-naar-SFP + adapters voor gebruik met 10 GbE-netwerkinterfaces

- [Ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw apparaat StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)


### <a name="power-cabling"></a>Power kabels

Het apparaat bevat redundante voeding en koeling Modules (PCMs). Beide PCMs worden geïnstalleerd en aangesloten op andere energiebronnen, hoge beschikbaarheid garanderen.

Voer de volgende stappen uit om het apparaat voor power kabel.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Netwerkkabels

Uw apparaat is een actieve stand-by-configuratie: op een bepaald moment één controllermodule actief is en verwerkingen alle schijf en netwerk terwijl de andere controllermodule is op stand-by. Als een controller uitvalt, wordt de stand-by-controller direct geactiveerd en blijft de schijf en netwerk activiteiten.

Ter ondersteuning van deze redundante controller failover, moet u de kabel van uw netwerk apparaat zoals beschreven in de volgende stappen uit.

#### <a name="to-cable-for-network-connection"></a>Kabel voor netwerkverbinding

1. Uw apparaat heeft zes netwerkinterfaces op elke domeincontroller: vier 1 GB/s, en twee 10 Gbps Ethernet-poorten. Identificeren van de verschillende data-poorten op de backplane van uw apparaat.

    ![Backplane 8100 apparaat](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Terug van het apparaat weergegeven gegevenspoorten**

     Label   | Beschrijving
     ------- | -----------
     0,1,4,5 |  1 GbE-netwerkinterfaces
     2,3     | 10 GbE-netwerkinterfaces
     6       | Seriële poorten

2. Zie het volgende diagram voor netwerkbekabeling. (De minimale netwerkconfiguratie wordt weergegeven door een ononderbroken blauwe lijnen. Aanvullende configuratie vereist voor hoge beschikbaarheid en prestaties wordt als stippellijnen weergegeven.)


    ![Uw apparaat 2U voor netwerk kabel](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Netwerk bekabeling voor uw apparaat**


  	|Label | Beschrijving |
  	|----- | ----------- |
  	| A    | LAN met toegang tot het Internet |
  	| B    | 0-controller |
  	| C    | PCM-0 |
  	| D    | 1-controller |
  	| E    | PCM-1 |
  	| F, G | Hosts |
  	| 0-5  | Netwerkinterfaces |



Wanneer het apparaat bekabeling, is de minimale configuratie vereist:


- Ten minste twee netwerkinterfaces worden aangesloten op elke domeincontroller met een wolk en een voor iSCSI. De gegevens van 0-poort wordt automatisch ingeschakeld en geconfigureerd via de seriële console van het apparaat. 0-gegevens, met uitzondering van moet een andere poort ook worden geconfigureerd via de klassieke Azure portal. In dit geval verbinding gegevens 0 poort met het primaire LAN (netwerk met toegang tot het Internet). De data-poorten kunnen worden verbonden met SAN/iSCSI-LAN (VLAN) segment van het netwerk, afhankelijk van de gewenste rol.

- Identieke interfaces op elke controller is verbonden met hetzelfde netwerk om de beschikbaarheid te garanderen als een controller failover plaatsvindt. Bijvoorbeeld, als u de verbinding gegevens 0 en 3 van de gegevens voor een van de domeincontrollers, moet u verbinding maken met de bijbehorende gegevens 0 en 3 van de gegevens op de andere domeincontroller.

Houd in gedachten voor hoge beschikbaarheid en prestaties:


- Configureren, indien mogelijk, een paar netwerkinterface voor een cloud-toegang (1 GbE) en een ander paar voor iSCSI (10 GbE aanbevolen) op elke domeincontroller.

- Indien mogelijk verbinding te activeren wordt de beschikbaarheid tegen een schakeloptie netwerkinterfaces van elke domeincontroller. De afbeelding ziet u de twee 10 GbE netwerkinterfaces, gegevens 2 en 3 van de gegevens, vanaf elke domeincontroller die is aangesloten op twee verschillende schakelopties.

Raadpleeg voor meer informatie de **Netwerkinterfaces** volgens de [vereisten voor uw apparaat StorSimple hoge beschikbaarheid](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Als u +-SFP-transceivers met de 10 GbE-netwerkinterfaces, gebruikt u de meegeleverde QSFP-SFP + adapters. Ga voor meer informatie naar [ondersteunde hardware voor de 10 GbE-netwerkinterfaces op uw apparaat StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).



### <a name="serial-port-cabling"></a>Bekabeling van de seriële poort

Voer de volgende stappen uit om de kabel van de seriële poort.

#### <a name="to-cable-for-serial-connection"></a>Kabel voor seriële verbinding

1. Uw apparaat heeft een seriële poort op elke domeincontroller die wordt aangeduid met een moersleutelpictogram van een. Zie de afbeelding in de sectie [netwerkkabels](#network-cabling) de seriële poorten op het backplane van uw apparaat vinden.

2. Identificeer de active controller op uw apparaat backplane. Een knipperende blauwe LED geeft aan dat de domeincontroller actief is.

3. Gebruik de meegeleverde seriële kabels (indien nodig, de USB-serieel converter voor uw laptop) en uw console of een computer (met terminal-emulatiesoftware aan op het apparaat) verbinding met de seriële poort van de actieve domeincontroller.

4. De seriële USB-stuurprogramma's (geleverd bij het apparaat) op uw computer installeren.

5. De seriële verbinding als volgt instellen: 115.200 baud, 8 databits, geen pariteit, 1 stopbit en datatransportbesturing is ingesteld op geen.

6. Controleer of de verbinding werkt door te drukken op Enter op de console. Een seriële console-menu moet worden weergegeven.

>[AZURE.NOTE] **Lights-Out Management**: wanneer het apparaat is geïnstalleerd in een computerruimte met beperkte toegang of in een extern datacenter, ervoor te zorgen dat de seriële verbindingen met beide domeincontrollers altijd met een seriële console-switch of soortgelijke werktuigen verbonden bent. Hierdoor kan de out-of-band extern beheer en ondersteuning voor bewerkingen als netwerk onderbrekingen of onverwachte storingen.

Het apparaat is nu kabels voor voeding, toegang tot het netwerk en een seriële verbinding. De volgende stap is het configureren van de software en implementeren van uw apparaat.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [implementeren en configureren van uw apparaat op gebouwen StorSimple](storsimple-deployment-walkthrough.md).
