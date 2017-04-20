<properties 
   pageTitle="Vervangen van de batterij op een apparaat StorSimple | Microsoft Azure"
   description="Beschrijving van het te verwijderen, vervangen en onderhouden van de back-up batterij module op uw apparaat StorSimple."
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

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>De back-up batterij module op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

De primaire behuizing, voeding en koeling Module (PCM) op het apparaat met Microsoft Azure StorSimple heeft een extra batterij pack. Dit pakket biedt stroom zodat het apparaat StorSimple gegevens kunt opslaan als er verlies van netspanning naar de primaire behuizing. Deze batterij is de *back-up batterij module*genoemd. De back-up batterij module bestaat alleen voor de primaire ruimte in uw StorSimple apparaat (de EBOD-behuizing bevat een back-up batterij module). 

In deze zelfstudie wordt uitgelegd hoe u kunt:

- De back-up batterij module verwijderen 
- Installeer een nieuwe module in de back-up batterij
- De back-up batterij module onderhouden

>[AZURE.IMPORTANT] Lees de veiligheidsinformatie in de [Inleiding tot StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md)voordat verwijderen en vervangen van een back-up batterij module:.

## <a name="remove-the-backup-battery-module"></a>De back-up batterij module verwijderen

De module back-up batterij voor uw apparaat StorSimple is een vervangbare eenheid. Voordat deze wordt geïnstalleerd in de PCM, kan de batterij module moet worden opgeslagen in de originele verpakking. Voer de volgende stappen uit om de back-up batterij verwijderen.

#### <a name="to-remove-the-backup-battery-module"></a>De back-up batterij module verwijderen

1. In de klassieke Azure portal, Ga naar **apparaten** > **onderhoud** > **Status van de Hardware**. Bekijk de status van de accu onder **Gedeelde onderdelen**.

2. Identificeer de PCM waarbij de accu is mislukt. Figuur 1 toont de achterzijde van het apparaat StorSimple.

    ![Backplane van apparaatmodules primaire behuizing](./media/storsimple-battery-replacement/IC740994.png)

    **Figuur 1** Weer terug van primaire apparaat PCM-controller en modules

  	|Label|Beschrijving|
  	|:----|:----------|
  	|1|PCM-0|
  	|2|PCM-1|
  	|3|0-controller|
  	|4|1-controller|

    Zoals aangegeven door nummer 3 in de figuur 2, moet de controle indicator LED PCM 0 komt overeen met **Batterij veroorzaakt** zijn ontstoken.

    ![Backplane van apparaat PCM controle Indicator-LED 's](./media/storsimple-battery-replacement/IC740992.png)

    **Figuur 2** Weer terug van PCM controle LED

  	|Label|Beschrijving|
  	|:---|:-----------|
  	|1|Stroomstoring AC|
  	|2|Ventilator fout|
  	|3|Accu fout|
  	|4|PCM-OK|
  	|5|Stroomstoring DC|
  	|6|Gezonde batterij|

3. Als u wilt verwijderen van de PCM met een defecte batterij, volg de stappen in [een PCM verwijderen](storsimple-power-cooling-module-replacement.md#remove-a-pcm).

4. Met de PCM verwijderd til pull-tot het verwijderen van de batterij en de batterij module-ingang omhoog draaien zoals aangegeven in de volgende afbeelding.

    ![Verwijderen van de batterij van PCM](./media/storsimple-battery-replacement/IC741019.png)

    **Figuur 3** Verwijderen van de batterij van de PCM

5. Plaats de module in de vervangbare eenheid verpakking.

6. Het defecte apparaat terug naar Microsoft voor het juiste onderhoud en verwerken.

## <a name="install-a-new-backup-battery-module"></a>Installeer een nieuwe module in de back-up batterij

Voer de volgende stappen uit om de vervangende batterij module in de PCM in de primaire behuizing van uw apparaat StorSimple te installeren.

#### <a name="to-install-the-battery-module"></a>De batterij-module installeren

1. De back-up batterij module plaatsen in de juiste richting in de PCM.

2. Houd de batterij module-ingang helemaal aan de connector.

3. De PCM in de primaire behuizing vervangen door de richtlijnen in het vak [vervangen een voeding en koeling Module op uw apparaat StorSimple](storsimple-power-cooling-module-replacement.md).

4. Nadat de vervanging voltooid is, gaat u naar **apparaten** > **onderhoud** > de**Status van de Hardware** in de klassieke Azure portal. Controleer of de status van de batterij om te controleren of de installatie is voltooid. Een groen status geeft aan dat de accu in orde is.

## <a name="maintain-the-backup-battery-module"></a>De back-up batterij module onderhouden

De back-up batterij module biedt in uw apparaat StorSimple stroom naar de domeincontroller tijdens een stroomstoring verloren gaan. Hiermee kunt het apparaat StorSimple opslaan van belangrijke gegevens voordat u op een gecontroleerde manier afsluiten. Het systeem kan twee opeenvolgende verlies gebeurtenissen verwerken met twee volledig opgeladen batterijen in de PCMs.

In de klassieke Azure portal, de **Status van de Hardware** op de pagina **onderhoud** geeft aan of de accu is defect of het einde van de levenscyclus nadert. De status van de accu wordt aangegeven door een **batterij in PCM 0** of **accu in de PCM-1** onder **Gedeelde onderdelen**. Deze pagina geeft een **GEDEGRADEERD** staat voor het einde van de levenscyclus bereikt en **** voor het einde van de levenscyclus bereikt. 

>[AZURE.NOTE] De batterij kunt **mislukt** verslag zo nodig te worden in rekening gebracht.
 
Als de staat **GEDEGRADEERD** wordt weergegeven, wordt het aangeraden de volgende cursus van actie:

- Het systeem mogelijk sprake van een recente stroomuitval of de batterijen worden periodiek onderhoud kunnen ondergaan. Houd rekening met het systeem voor 12 uur voordat u verdergaat.

    - Als de status nog steeds **GEDEGRADEERD** na 12 uur van een continue verbinding met wisselstroom met de controllers en PCMs uitgevoerd, moet de batterij worden vervangen. Neem [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) voor een back-up batterij vervanging module.

    - Als de status OK na 12 uur wordt, de accu operationeel is en dat alleen een gratis onderhoud nodig.

- Als er een bijbehorende verlies van netstroom niet is de PCM is ingeschakeld en aangesloten op netstroom, moet de batterij worden vervangen. [Contact Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor het bestellen van een back-up batterij vervanging module.

>[AZURE.IMPORTANT] De defecte batterij volgens de nationale en regionale wegdoen. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).
