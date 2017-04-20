<properties 
   pageTitle="Vervang een PCM op uw apparaat StorSimple | Microsoft Azure"
   description="Hoe te verwijderen en vervangen door de voeding en koeling Module (PCM) op uw apparaat StorSimple"
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Vervangen van een voeding en koeling Module op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

Voeding en koeling Module (PCM) in het Microsoft Azure StorSimple apparaat bestaat uit een voeding en koeling van ventilatoren die worden bestuurd door de primaire en de EBOD-behuizingen. Er is slechts één model van PCM is gecertificeerd voor elke ruimte. De primaire behuizing is gecertificeerd voor een 764 W PCM en de EBOD-behuizing is gecertificeerd voor een 580 W PCM. Hoewel de PCMs voor de primaire behuizing en de EBOD niet gelijk zijn, wordt de vervangingsprocedure identiek is.

In deze zelfstudie wordt uitgelegd hoe u kunt:

- Een PCM verwijderen
- Installeer een vervangende PCM

>[AZURE.IMPORTANT] Vóór verwijderen en vervangen van een PCM, bekijk de veiligheidsinformatie in [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).

## <a name="before-you-replace-a-pcm"></a>Voordat u een PCM vervangen

Worden op de hoogte van de volgende belangrijke problemen voordat u uw PCM vervangen:

- Als de voeding van de PCM mislukt, laat u de defecte module is geïnstalleerd, maar verwijder het netsnoer. De ventilator blijft stroom wordt uit de ruimte en gaat u verder met de juiste koeling bieden. Als de ventilator niet werkt, moet de PCM onmiddellijk worden vervangen.

- Voor het verwijderen van de PCM, de stroomvoorziening van de PCM door het uitschakelen van de belangrijkste switch (indien aanwezig) of fysiek verwijderen van het netsnoer. Dit geeft een waarschuwing op uw systeem afsluiten power dreigt.

- Zorg ervoor dat de andere PCM functionele voor Systeembewerking blijven voordat de PCM defect worden vervangen. Een defecte PCM moet zo spoedig mogelijk worden vervangen door een volledig operationeel PCM.

- Vervanging van PCM-module duurt slechts enkele minuten, maar deze moet worden voltooid binnen 10 minuten na het verwijderen van de mislukte PCM te zuigen.

- Houd er rekening mee dat vervangende 764 W PCM modules vanuit de fabriek verzonden de back-up batterij module bevatten. U moet de accu van uw defecte PCM verwijderen en vervolgens invoegen in de module vervangen voordat u de vervanging. Zie voor meer informatie het [verwijderen en een back-up batterij module invoegen](storsimple-battery-replacement.md).


## <a name="remove-a-pcm"></a>Een PCM verwijderen

Volg deze instructies als u wilt verwijderen van een voeding en koeling Module (PCM) van het Microsoft Azure StorSimple apparaat.

>[AZURE.NOTE] Voordat u uw PCM verwijdert, controleert u of een juiste vervangende (764 W voor de primaire behuizing) of 580 W voor de EBOD behuizing.

#### <a name="to-remove-a-pcm"></a>Voor het verwijderen van een PCM

1. Klik op **apparaten**in de klassieke Azure portal > **onderhoud** > **Status van de Hardware**. Controleer de status van de PCM-onderdelen onder **Gedeelde onderdelen** aangeven op welke PCM is mislukt:

     - Als een voeding in PCM 0 is mislukt, is de status van de **Voeding in PCM 0** zijn rood.

     - Als een voeding in PCM 1 is mislukt, is de status van de **Voeding in PCM 1** zijn rood.

     - Als de ventilator in de PCM-1 is mislukt, is de status van **koeling 0 voor PCM 0** of **1 koeling voor PCM 0** zijn rood.

2. Vinden van de mislukte PCM op de achterkant van de behuizing van de primaire. Als u een model 8600 herkennen aan de primaire behuizing kijken naar het systeem eenheid id-nummer op het voorpaneel LED-scherm weergegeven. Standaard die eenheid-ID wordt weergegeven op het primaire behuizing is **00**, terwijl de eenheid-ID wordt weergegeven op de behuizing EBOD standaardwaarde **01 is**. Het volgende diagram en de tabel wordt uitgelegd het voorpaneel van de LED-scherm.

    ![Systeem-ID op voorpaneel OPS](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Figuur 1** Voorpaneel van het apparaat  

  	|Label|Beschrijving|
  	|:---|:-----------|
  	|1|Knop Dempen|
  	|2|Inschakelen van het systeem|
  	|3|Module fout|
  	|4|Logische fout|
  	|5|Eenheid-ID weergeven|

3. De controle-indicator LED's aan de achterzijde van de behuizing van de primaire kan ook worden gebruikt voor het identificeren van de defecte PCM. Zie het volgende diagram en de tabel om te begrijpen hoe de defecte PCM vinden met de LED's. Bijvoorbeeld, als de LED die correspondeert met het **Mislukken van de ventilator** wordt belicht, is de ventilator mislukt. Op dezelfde manier als het overeenkomt met het **Mislukken van AC** -LED oplichten, de voeding kan niet. 

    ![Backplane apparaat PCM controle indicator LED 's](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Figuur 2** Achterzijde van PCM met LED

  	|Label|Beschrijving|
  	|:---|:-----------|
  	|1|Stroomstoring AC|
  	|2|Ventilator fout|
  	|3|Accu fout|
  	|4|PCM-OK|
  	|5|Stroomstoring DC|
  	|6|Gezonde batterij|

4. Raadpleeg het volgende diagram van de achterkant van het apparaat StorSimple te vinden van de mislukte PCM-module. PCM-0 aan de linkerkant en PCM 1 aan de rechterkant. De volgende tabel wordt uitgelegd dat de modules.

     ![Backplane van apparaatmodules primaire behuizing](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Figuur 3** Achterzijde van apparaat met Plug-ins 

  	|Label|Beschrijving|
  	|:---|:-----------|
  	|1|PCM-0|
  	|2|PCM-1|
  	|3|0-controller|
  	|4|1-controller|

5. De defecte PCM uitschakelen en het aanbod netsnoer loskoppelen. U kunt nu de PCM verwijderen.

6. Het slot en de zijkant van de handgreep PCM tussen uw duim en wijsvinger te vatten en verondersteld samen om de greep te openen.

    ![Openen PCM-ingang](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **Figuur 4** U opent de PCM-ingang

7. De ingang houvastOpening en verwijderen van de PCM.

    ![PCM-apparaat verwijderen](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Figuur 5** Verwijderen van de PCM

## <a name="install-a-replacement-pcm"></a>Installeer een vervangende PCM

Volg deze instructies voor het installeren van een PCM in uw apparaat StorSimple. Zorg ervoor dat u de back-up batterij module vóór de installatie van de vervanging van PCM (764 W PCMs alleen voor) hebt ingevoegd. Zie voor meer informatie het [verwijderen en een back-up batterij module invoegen](storsimple-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Een PCM installeren

1. Controleer of u de juiste vervangende PCM voor deze bijlage. De primaire ruimte moet een 764 W PCM en de behuizing van de EBOD moet een 580 W PCM. U moet niet proberen de 580 W PCM in de primaire ruimte of het 764 PCM W in de ruimte EBOD te gebruiken. In de volgende afbeelding laat zien waar deze informatie op het etiket dat wordt aangebracht op de PCM identificeren.

    ![Apparaat PCM-Label](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Figuur 6** PCM-label

2. Controleren op beschadiging van de behuizing, met bijzondere aandacht voor de verbindingslijnen. 
                                        
    >[AZURE.NOTE] **Installeer de module niet als een verbindingslijn pennen verbogen zijn.**

3. Dia met de PCM-ingang in de open positie, de module in de meetruimte.

    ![PCM-apparaat installeren](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Figuur 7** De PCM installeren

4. De PCM-ingang handmatig af te sluiten. U moet een klik horen als de hendel van de ingang alleen mogelijk. 
                                        
    >[AZURE.NOTE] Om ervoor te zorgen dat de pins connector hebt ingeschakeld, kunt u zachtjes tug op de greep zonder de hendel los te laten. Als de PCM glijdt, betekent dit dat het slot is afgesloten voordat de verbindingslijnen die betrokken zijn.

5. Sluit de stroomsnoeren aan het stopcontact en de PCM.

6. De stam vrijstelling balen beveiligen. 

7. De PCM inschakelen.

8. Verifiëren dat de vervanging: Ga naar **apparaten**in Azure klassieke portal van uw service Manager StorSimple > **onderhoud** > **Status van de Hardware**. De status van de PCM moeten groen zijn onder **Gedeelde onderdelen**. 
                                        
    >[AZURE.NOTE] Het kan enkele minuten duren voordat vervanging PCM volledig geïnitialiseerd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).
