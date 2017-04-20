<properties 
   pageTitle="Vervangen door een domeincontroller StorSimple EBOD | Microsoft Azure"
   description="Hoe te verwijderen en vervangen door een of beide EBOD-controllers op een apparaat StorSimple 8600."
   services="storsimple"
   documentationCenter=""
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

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Een EBOD-controller op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u een defecte EBOD controllermodule op uw apparaat met Microsoft Azure StorSimple vervangen. Ter vervanging van een EBOD-controller-module, moet u:

- De defecte controller met EBOD verwijderen
- Een nieuwe EBOD-controller installeren

Voordat u begint, moet u rekening houden met de volgende informatie:

- Lege EBOD-modules moeten worden ingevoegd in alle ongebruikte "slots". De ruimte koel niet correct als een sleuf wordt geopend.

- De EBOD-controller is hot-swappable en kan worden verwijderd of vervangen. Een defecte module niet verwijderen totdat u een vervanging. Wanneer u het proces start, moet u deze binnen 10 minuten voltooien.

>[AZURE.IMPORTANT] Voordat u probeert te verwijderen of te vervangen enig onderdeel van de StorSimple, moet u controleren de [veiligheid pictogram conventies](storsimple-safety.md#safety-icon-conventions) en andere [voorzorgsmaatregelen](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Verwijderen van een EBOD-controller

Zorg dat de andere EBOD-controller-module actief en wordt uitgevoerd is voordat de mislukte EBOD controller-module in uw apparaat StorSimple wordt vervangen. De volgende procedure en de tabel wordt uitgelegd hoe de module EBOD controller verwijderen.

#### <a name="to-remove-an-ebod-module"></a>Voor het verwijderen van een module EBOD

1. Open de Azure klassieke portal.

2. Ga naar **apparaten** > **onderhoud** > **Status van de Hardware**, en controleer of de status van de LED voor de actieve EBOD controller-module is groen en de LED voor de mislukte EBOD controller-module is rood.

3. Vinden van de mislukte EBOD controller-module aan de achterkant van het apparaat.

4. De kabels die verbinding maken met de module EBOD-controller op de controller voordat u de module EBOD van het systeem verwijderen.

5. Noteer de exacte SAS-poort van de EBOD-controller-module die op de controller is aangesloten. U moet het systeem in deze configuratie herstellen nadat u de module EBOD vervangen. 

    >[AZURE.NOTE] Dit is gewoonlijk poort A, die in het volgende diagram wordt aangeduid als **Host in** .

    ![Backplane van EBOD-controller](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Figuur 1** Achterzijde van de module EBOD

  	|Label|Beschrijving|
  	|:----|:----------|
  	|1|Fout-LED|
  	|2|LED voor voeding|
  	|3|SAS-connectors|
  	|4|SAS-LED 's|
  	|5|Seriële poorten alleen voor fabriek|
  	|6|Poort (Host in)|
  	|7|Poort B (Host uit)|
  	|8|C poort (alleen voor gebruik van de fabriek)|

## <a name="install-a-new-ebod-controller"></a>Een nieuwe EBOD-controller installeren

De volgende procedure en de tabel wordt uitgelegd hoe een EBOD-controller-module in uw StorSimple apparaat installeren.

#### <a name="to-install-an-ebod-controller"></a>Een EBOD-controller installeren

1. Controleer of het apparaat EBOD voor schade, vooral aan de interface-connector. Installeer de nieuwe EBOD-controller niet als er pennen verbogen zijn.

2. Met de hendels in de open stand, schuift u de module in de meetruimte totdat de hendels uitoefenen.

    ![EBOD-controller installeren](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Figuur 2**  Installeren van de module EBOD-controller

3. Sluit het slot. U moet een klik horen, zoals het slot voert.

    ![EBOD hendel los](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Figuur 3**  Sluit de module EBOD hendel

4. Sluit de kabels. De precieze configuratie die zich bevond voordat de vervanging gebruiken. Zie het volgende diagram en de tabel voor meer informatie over het aansluiten van de kabels.

    ![Uw apparaat 4U voor voeding kabel](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **In figuur 4**. Opnieuw verbinding maken met kabels

  	|Label|Beschrijving|
  	|:----|:----------|
  	|1|Primaire behuizing|
  	|2|PCM-0|
  	|3|PCM-1|
  	|4|0-controller|
  	|5|1-controller|
  	|6|EBOD-controller 0|
  	|7|1 EBOD-controller|
  	|8|EBOD-behuizing|
  	|9|Power Distribution Units|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).
