<properties 
   pageTitle="Een apparaat StorSimple controller vervangen | Microsoft Azure"
   description="Hoe te verwijderen en vervangen door een of beide controller modules op het apparaat StorSimple."
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

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Een controllermodule op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe te verwijderen en vervangen door een of beide controller modules in een StorSimple apparaat. Het wordt ook de onderliggende logica van de enkelvoudige en dubbele controller vervanging scenario's beschreven.

>[AZURE.NOTE] Voordat u de vervanging van een domeincontroller uitvoert, is het raadzaam dat u altijd uw controllerfirmware naar de nieuwste versie bijwerken.
>
>Om te voorkomen dat schade aan het apparaat StorSimple, niet verwijdert de controller totdat de LED's worden weergegeven als een van de volgende opties:
>
>- Alle lampjes zijn uitgeschakeld.
>- LED-3 ![groen vinkje pictogram](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), en ![pictogram rood kruis](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) knipperen, en LED 0 en 7 LED **op**.

De volgende tabel bevat de ondersteunde controller vervanging scenario's.

|Case|Vervangende scenario|Van toepassing zijnde procedure|
|:---|:-------------------|:-------------------|
|1|Één domeincontroller in een mislukte staat is, de andere controller is gezond en actief is.|[Vervanging van één controller](#replace-a-single-controller), waarin de [logica achter één controller vervanging](#single-controller-replacement-logic), evenals de [vervangende stappen](#single-controller-replacement-steps).|
|2|Zowel de domeincontrollers zijn mislukt en moeten worden vervangen. De behuizing, schijven, and.disk behuizing in orde zijn.|[Vervanging van twee controllers](#replace-both-controllers), waarin de [logica achter een vervangende twee controllers](#dual-controller-replacement-logic), evenals de [vervangende stappen](#dual-controller-replacement-steps). |
|3|Controllers van het apparaat of van verschillende apparaten worden verwisseld. Het chassis, schijfbehuizingen en schijven in orde zijn.|Een sleuf incompatibel-waarschuwingsbericht wordt weergegeven.|
|4|Er ontbreekt één domeincontroller en de andere controller uitvalt.|[Vervanging van twee controllers](#replace-both-controllers), waarin de [logica achter een vervangende twee controllers](#dual-controller-replacement-logic), evenals de [vervangende stappen](#dual-controller-replacement-steps).|
|5|Een of beide domeincontrollers zijn mislukt. U kunt geen toegang tot het apparaat via de seriële console of via Windows PowerShell remoting.|[Contact Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor een handmatige controller vervangen-procedure.|
|6|De controllers hebben een verschillende build-versie, die zijn verschuldigd aan:<ul><li>Controllers hebben een verschillende software-versie.</li><li>Controllers hebben een verschillende firmwareversie.</li></ul>|De controller softwareversies verschillend zijn, de logica van de vervangende detecteert die als bijgewerkt versie van de software op de controller vervangen.<br><br>Als de controller firmware-versies verschillend zijn en de oude firmwareversie is **niet** automatisch kan worden uitgebreid, een waarschuwingsbericht wordt weergegeven in de klassieke Azure portal. U moet zoeken naar updates en installeer de firmware-updates.</br></br>Als de controller firmware-versies verschillend zijn en de oude firmwareversie kan automatisch worden uitgebreid, de logica van de vervangende controller dit wordt gedetecteerd en nadat de domeincontroller wordt gestart, de firmware wordt automatisch bijgewerkt.|

U moet een controllermodule verwijderen als dat niet is. Een of beide de controller modules kunnen mislukken, hetgeen kan leiden tot een enkele controller vervanging of ter vervanging van twee controllers. Vervangende procedures en de logica achter deze, Zie het volgende:

- [Één controller vervangen](#replace-a-single-controller)
- [Beide controllers vervangen](#replace-both-controllers)
- [Een domeincontroller verwijderen](#remove-a-controller)
- [Invoegen van een domeincontroller](#insert-a-controller)
- [De actieve controller op uw apparaat identificeren](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Vóór verwijderen en vervangen van een domeincontroller, bekijk de veiligheidsinformatie in [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).

## <a name="replace-a-single-controller"></a>Één controller vervangen

Wanneer een van de twee controllers op de Microsoft Azure StorSimple-apparaat is mislukt, is defect of ontbreekt, moet u één controller vervangen. 

### <a name="single-controller-replacement-logic"></a>Logica van één controller vervangen

In één controller vervanging, moet u eerst de defecte controller verwijderen. (De resterende domeincontrollers in het apparaat is de actieve domeincontroller.) Als u de controller vervanging invoegt, worden de volgende acties uitgevoerd:

1. De controller vervanging begint direct communiceren met het apparaat StorSimple.

2. Een momentopname van de virtuele harde schijf (VHD) voor de actieve domeincontroller wordt gekopieerd op de controller vervangen.

3. De momentopname wordt gewijzigd zodat deze bij het starten van de controller vervanging van deze VHD, dat wordt herkend als een stand-by-controller.

4. Wanneer de wijzigingen voltooid zijn, wordt de controller vervangen als de stand-by-controller gestart.

5. Wanneer zowel de domeincontrollers uitvoert, wordt het cluster on line komt.

### <a name="single-controller-replacement-steps"></a>Stappen van één controller vervangen

Voer de volgende stappen uit als een van de controllers in het Microsoft Azure StorSimple apparaat uitvalt. (De andere controller moeten actief en wordt uitgevoerd. Als beide domeincontrollers mislukt of er problemen optreden, gaat u naar [twee controllers vervangend werk](#dual-controller-replacement-steps).)

>[AZURE.NOTE] Het kost 30-45 minuten voor de controller op te starten en de vervangingsprocedure één controller volledig herstellen. De totale tijd voor de gehele procedure, is met inbegrip van de kabels verbonden ongeveer 2 uur.

#### <a name="to-remove-a-single-failed-controller-module"></a>Een afzonderlijke schijfcontroller module verwijderen

1. In de klassieke Azure portal, gaat u naar de StorSimple Manager-service, klikt u op het tabblad **apparaten** en klikt u vervolgens op de naam van het apparaat dat u wilt controleren.

2. Ga naar **onderhoud > hardwarestatus**. De status van de Controller 0 of 1 Controller nu rood moet zijn, die een fout aangeeft.

    >[AZURE.NOTE] De defecte controller in één controller vervanging is altijd een stand-by-controller.

3. Figuur 1 en in de volgende tabel gebruiken om te zoeken van de schijfcontroller module.  

    ![Backplane van apparaatmodules primaire behuizing](./media/storsimple-controller-replacement/IC740994.png)

    **Figuur 1** Achterzijde van apparaat StorSimple

  	|Label|Beschrijving|
  	|:----|:----------|
  	|1|PCM-0|
  	|2|PCM-1|
  	|3|0-controller|
  	|4|1-controller|

4. Verwijderen alle verbonden netwerkkabels vanaf de havens van de gegevens op de defecte controller. Als u een model 8600, ook de SAS-kabels die verbinding maken met de controller op de EBOD-controller te verwijderen.

5. Volg de stappen in het [verwijderen van een domeincontroller](#remove-a-controller) voor het verwijderen van de schijfcontroller. 

6. Installeer de vervanging van de fabriek in dezelfde sleuf waaruit de schijfcontroller is verwijderd. Hierdoor wordt de vervanging van één controller logica. Zie voor meer informatie [één controller vervanging logica](#single-controller-replacement-logic).

7. Terwijl de logica van de vervanging van één controller op de achtergrond verloopt, sluit de kabels. Wees voorzichtig met exact dezelfde manier dat ze zijn verbonden voordat u de vervanging van alle kabels verbinden.

8. Nadat de domeincontroller opnieuw is opgestart, controleert u de **status van de domeincontroller** en de **status van het Cluster** in de klassieke Azure portal om te controleren of de controller een gezonde staat is en in de stand-by-modus.

>[AZURE.NOTE] Als u het apparaat via de seriële console controleert, ziet u mogelijk meerdere opnieuw wordt opgestart tijdens het herstellen van de controller in de vervangingsprocedure is. Wanneer de seriële console-menu wordt weergegeven, weet u dat de vervanging voltooid is. Als het menu binnen twee uur na het starten van de vervanging van de controller niet wordt weergegeven, neemt u [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Beide controllers vervangen

Wanneer beide domeincontrollers op de Microsoft Azure StorSimple-apparaat is mislukt, defect zijn of ontbreken, moet u beide domeincontrollers vervangen. 

### <a name="dual-controller-replacement-logic"></a>Twee controllers vervanging logica

In een vervangende twee controllers beide domeincontrollers mislukte eerst verwijderen en vervolgens invoegen vervangingen. Als de vervanging van twee domeincontrollers wordt geplaatst, gebeurt het volgende:

1. De controller vervanging in sleuf 0 wordt het volgende gecontroleerd:
 
   1. Werkt het in de huidige versies van de firmware en software?

   2. Het is een onderdeel van het cluster?

   3. De peer-controller met en is het cluster?
                            
    Als geen van deze voorwaarden voldaan wordt, wordt de controller de nieuwste dagelijkse back-up (te vinden in de **nonDOMstorage** op station S). De laatste momentopname van de VHD de domeincontroller opgehaald uit de back-up.

2. De controller in de sleuf 0 wordt de momentopname van het image zelf.

3. Ondertussen wacht de controller in sleuf 1 controller 0 de imaging voltooien en start.

4. Als controller 0 is opgestart, detecteert controller 1 cluster gemaakt door controller 0, wat inhoudt dat de vervanging van één controller logica geactiveerd. Zie voor meer informatie [één controller vervanging logica](#single-controller-replacement-logic).

5. Daarna beide domeincontrollers wordt uitgevoerd en het cluster on line.

>[AZURE.IMPORTANT] Na de vervanging van twee controllers, nadat de StorSimple-apparaat is geconfigureerd, is het essentieel dat u een handmatige back-up van het apparaat neemt. Dagelijkse apparaat configuratie van back-ups worden niet geactiveerd pas na 24 uur zijn verstreken. Werken met [Microsoft Support](storsimple-contact-microsoft-support.md) voor een handmatige back-up van uw apparaat te maken.

### <a name="dual-controller-replacement-steps"></a>Twee controllers vervanging stappen

Deze werkstroom is vereist wanneer een van de controllers in het Microsoft Azure StorSimple-apparaat is mislukt. Dit kan gebeuren in een datacenter waar het koelsysteem werkt niet meer en daardoor zowel de domeincontrollers binnen een korte tijdsperiode mislukken. Afhankelijk van of het apparaat StorSimple is in- of uitgeschakeld, en of u een 8600 of een model 8100 is een andere reeks stappen vereist.

>[AZURE.IMPORTANT] Het kost 45 minuten tot 1 uur voor de controller opnieuw en volledig herstellen van een dual-controller vervangen-procedure. De totale tijd voor de gehele procedure, is met inbegrip van de kabels verbonden ongeveer 2,5 uur.

#### <a name="to-replace-both-controller-modules"></a>Beide modules controller vervangen

1. Als het apparaat is uitgeschakeld, wordt deze stap overslaan en doorgaan naar de volgende stap. Als het apparaat is ingeschakeld, schakelt u het apparaat uit.
                                        
    1. Als u een model 8600, de primaire behuizing eerste uitschakelen en schakel vervolgens uit de behuizing van de EBOD.

    2. Wacht totdat het apparaat volledig is afgesloten. De LED's aan de achterzijde van het apparaat is uitgeschakeld.

2. Verwijder alle netwerkkabels die zijn verbonden met de data-poorten. Als u een model 8600, ook de SAS-kabels die verbinding maken met de primaire behuizing met de behuizing van de EBOD te verwijderen.

3. Beide controllers van het StorSimple apparaat verwijderen. Zie [een domeincontroller verwijderen](#remove-a-controller)voor meer informatie.

4. Plaatst u eerst de fabriek vervanging voor Controller 0 en 1 Controller plaats. Zie [een domeincontroller invoegen](#insert-a-controller)voor meer informatie. Hierdoor wordt de logica van de vervanging van twee controllers. Zie voor meer informatie, [twee controllers vervanging logica](#dual-controller-replacement-logic).

5. Terwijl de logica van de vervangende controller op de achtergrond verloopt, sluit de kabels. Wees voorzichtig met exact dezelfde manier dat ze zijn verbonden voordat u de vervanging van alle kabels verbinden. Zie de gedetailleerde instructies voor het model in de sectie apparaat van de kabel van de [installatie van het apparaat StorSimple 8100](storsimple-8100-hardware-installation.md) of [uw apparaat StorSimple 8600](storsimple-8600-hardware-installation.md).

6. Schakel het apparaat StorSimple. Als u een model 8600:

    1. Zorg ervoor dat de behuizing EBOD eerste is ingeschakeld.

    2. Wacht totdat de behuizing van de EBOD wordt uitgevoerd.

    3. Schakel in de primaire behuizing.

    4. Nadat de eerste domeincontroller wordt opnieuw opgestart en in orde zijn is, wordt het systeem worden uitgevoerd.

    >[AZURE.NOTE] Als u het apparaat via de seriële console controleert, ziet u mogelijk meerdere opnieuw wordt opgestart tijdens het herstellen van de controller in de vervangingsprocedure is. Als de seriële console-menu wordt weergegeven, weet u dat de vervanging voltooid is. Als het menu binnen 2,5 uur van het starten van de vervanging van de controller niet wordt weergegeven, neemt u [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Een domeincontroller verwijderen

Gebruik de volgende procedure voor het verwijderen van een defecte controllermodule van het apparaat StorSimple.

>[AZURE.NOTE] De volgende afbeeldingen zijn voor 0-controller. Voor 1-controller, zou deze worden omgekeerd.

#### <a name="to-remove-a-controller-module"></a>Een controllermodule verwijderen

1. De hendel van de module tussen uw duim en wijsvinger te vatten.

2. Druk zachtjes uw duim en wijsvinger samen om de hendel van de controller.

    ![Controller hendel los](./media/storsimple-controller-replacement/IC741047.png)

    **Figuur 2** Controller hendel los

2. Het slot gebruiken als ingang naar dia de controller van het chassis.

    ![Schuif de controller uit chassis](./media/storsimple-controller-replacement/IC741048.png)

    **Figuur 3** Schuif de controller van het chassis

## <a name="insert-a-controller"></a>Invoegen van een domeincontroller

Gebruik de volgende procedure in een controllermodule fabriek meegeleverde installeren nadat u een defecte module hebt verwijderd van uw apparaat StorSimple.

#### <a name="to-install-a-controller-module"></a>Een controllermodule installeren

1. Controleer of er geen schade aan de aansluitingen van de interface is. Installeer de module niet als een van de pennen van de connector worden beschadigd of ontregeld.

2. Schuif in het chassis de controllermodule terwijl de hendel van de volledige release. 

    ![Graduele controller in chassis](./media/storsimple-controller-replacement/IC741053.png)

    **Figuur 4** Graduele controller in het chassis

3. Met de controllermodule ingevoegd, begint het slot gesloten terwijl u de controllermodule in het chassis te duwen. Het slot wordt om te leiden van de controller in plaats.

    ![Sluit de controller hendel](./media/storsimple-controller-replacement/IC741054.png)

    **Figuur 5** Sluiten van de hendel van de controller

4. Is voltooid wanneer de vergrendeling vastklikt. Er worden nu de **OK** -LED op.  

    >[AZURE.NOTE] Het kan duren tot 5 minuten voor de controller en de LED te activeren.

5. Ga naar **apparaten**om te controleren dat de vervanging geslaagd, in de klassieke Azure portal is, > **onderhoud** > **Status van de Hardware**, en zorg ervoor dat zowel controller 0 en 1-controller in orde (status is groen).

## <a name="identify-the-active-controller-on-your-device"></a>De actieve controller op uw apparaat identificeren

Er zijn veel situaties, zoals het apparaat eerst registratie of controller vervanging waarvoor u de actieve domeincontroller te vinden op een StorSimple apparaat. De actieve domeincontroller verwerkt alle de firmware en netwerken schijfbewerkingen. Kunt u een van de volgende methoden voor het identificeren van de actieve domeincontroller:

- [De klassieke Azure portal gebruiken ter identificatie van de actieve controller](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Windows PowerShell voor StorSimple gebruiken voor de identificatie van de actieve domeincontroller.](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Het fysieke apparaat ter identificatie van de actieve domeincontroller controleren](#check-the-physical-device-to-identify-the-active-controller)

Elk van deze procedures wordt nu beschreven.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>De klassieke Azure portal gebruiken ter identificatie van de actieve controller

In de klassieke Azure portal, Ga naar **apparaten** > **onderhoud**en blader naar de sectie **Controllers** . Hier kunt u controleren welke domeincontroller actief is.

![Actieve domeincontroller in Azure klassieke portal identificeren](./media/storsimple-controller-replacement/IC752072.png)

**Figuur 6** Azure klassieke portal met de active controller

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Windows PowerShell voor StorSimple gebruiken voor de identificatie van de actieve domeincontroller.

Wanneer u het apparaat via de seriële console, wordt een bannerbericht gepresenteerd. Spandoek bericht bevat informatie zoals het model, naam, versie van de geïnstalleerde software en status van de controller die krijgt u toegang tot basisinformatie over het apparaat. De volgende afbeelding toont een voorbeeld van een bannerbericht:

![Seriële bannerbericht](./media/storsimple-controller-replacement/IC741098.png)

**Figuur 7** Bericht tonen controller 0 als actief banner

U kunt het paginavaandel bericht om te bepalen of de controller dat u met verbonden bent een actieve of passieve.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Het fysieke apparaat ter identificatie van de actieve domeincontroller controleren

Zoek de blauwe LED boven de gegevens 5-poort op de achterkant van de behuizing van de primaire ter identificatie van de actieve controller op uw apparaat.

Deze LED knippert, de controller is actief als de andere controller bevindt zich in de standby-stand. Het volgende diagram en de tabel als hulpmiddel gebruiken.

![Apparaat primaire behuizing backplane met datapoorten](./media/storsimple-controller-replacement/IC741055.png)

**Figuur 8** Achterzijde van de primaire behuizing met data-poorten en controle LED 's

|Label|Beschrijving|
|:----|:----------|
|1-6|DATA 0 – 5 netwerkpoorten|
|7|Blauwe LED|


## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).
