<properties
   pageTitle="Apparaatcontrollers StorSimple beheren | Microsoft Azure"
   description="Informatie over het stoppen, opnieuw starten, afsluiten of opnieuw instellen van uw domeincontrollers StorSimple apparaat."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>Uw apparaat StorSimple domeincontrollers beheren

## <a name="overview"></a>Overzicht

Deze handleiding beschrijft de verschillende bewerkingen die kunnen worden uitgevoerd op de domeincontrollers StorSimple apparaat. De domeincontrollers in uw apparaat StorSimple zijn (peer) redundante controllers in de configuratie van een actief-passief. Op een bepaald moment slechts één domeincontroller actief is en verwerkt alle schijf- en bewerkingen. De andere controller is in de passieve modus. Als de actieve controller uitvalt, actief de passieve controller automatisch.

Deze zelfstudie bevat stapsgewijze instructies voor het beheren van apparaten met behulp van de:

- Sectie van de pagina **onderhoud** in de service Manager StorSimple **controllers**
- Windows PowerShell voor StorSimple.

Wij raden aan dat u de apparaatcontrollers via de StorSimple Manager-service beheren. Als een actie kan alleen worden uitgevoerd met behulp van Windows PowerShell voor StorSimple, kunt u de zelfstudie OK.

Na het lezen van deze zelfstudie, is het mogelijk om:

- Opnieuw opstarten of afsluiten van een apparaat StorSimple controller
- Een apparaat StorSimple afsluiten
- Het apparaat StorSimple fabrieksinstellingen herstellen


## <a name="restart-or-shut-down-a-single-controller"></a>Opnieuw opstarten of afsluiten van één controller

Een domeincontroller opnieuw opstarten of afsluiten is niet vereist als onderdeel van de normale regeling. Shutdown bewerkingen voor een apparaat met één domeincontroller zijn slechts in gevallen waarin een apparaat hardware-onderdeel moet worden vervangen. Een domeincontroller opnieuw op te starten kan ook nodig zijn in een situatie waarin de prestaties wordt beïnvloed door overmatig geheugengebruik of een defecte controller. Mogelijk moet u ook aan de start van een domeincontroller na de vervanging van een succesvolle controller, als u wilt inschakelen en het testen van de controller vervangen.

Opstarten van een apparaat is niet storend voor verbonden initiators, ervan uitgaande dat de passieve domeincontroller beschikbaar is. Als een passieve controller niet is beschikbaar of is uitgeschakeld, uitgeschakeld en klik vervolgens opnieuw op te starten in de actieve domeincontroller kan resulteren in de onderbreking van de service en de uitvaltijd.

> [AZURE.IMPORTANT]

> - **Een actieve domeincontroller moet nooit fysiek worden verwijderd omdat dit leiden een verlies van redundantie en een verhoogd risico van downtime tot zou.**

> - De volgende procedure geldt alleen voor het fysieke apparaat StorSimple. Zie [werken met het virtuele apparaat](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device)voor meer informatie over het starten, stoppen en opnieuw starten van het virtuele apparaat.

U kunt opnieuw opstarten of afsluiten een controller voor één apparaat in door de Azure klassieke portal van de StorSimple Manager-service of de Windows PowerShell voor StorSimple

Voor het beheren van uw apparaatcontrollers van het klassieke Azure portal, kunt u de volgende stappen uitvoeren.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Voor het opnieuw opstarten of afsluiten van een domeincontroller in de klassieke portal

1. Ga naar **apparaten > onderhoud**.

1. Ga naar de **Status van de Hardware** en controleer of de status van zowel de domeincontrollers op uw apparaat is **orde**.

    ![Controleer of de StorSimple apparaatcontrollers gezond zijn](./media/storsimple-manage-device-controller/IC766017.png)

1. Klik op **Domeincontrollers beheren**vanaf de onderkant van de pagina **onderhoud** .

    ![Apparaatcontrollers StorSimple beheren](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Als u **Domeincontrollers beheren**niet ziet, moet u updates te installeren. Zie [het StorSimple apparaat bijwerken](storsimple-update-device.md)voor meer informatie.

1. Klik in het dialoogvenster **Domeincontroller wijzigen** het volgende doen:
    1. Selecteer de spelbesturing die u wilt beheren uit de vervolgkeuzelijst **Selecteren Controller** . De opties zijn Controller 0 en 1 Controller. Deze controllers worden tevens geïdentificeerd als actief of passief.

        >[AZURE.NOTE] Een domeincontroller kan niet worden beheerd als deze is uitgeschakeld of niet beschikbaar is uitgeschakeld en verschijnt niet in de lijst.

    2. Kies uit de vervolgkeuzelijst **Actie selecteren** **controller opnieuw opstarten** of **afsluiten van controller**.

        ![Passieve apparaatbesturing StorSimple starten](./media/storsimple-manage-device-controller/IC766020.png)
    3. Klik op het pictogram ![Pictogram](./media/storsimple-manage-device-controller/IC740895.png).

Dit zal opnieuw opstarten of afsluiten van de controller. In de volgende tabel bevat een overzicht van de details van wat er gebeurt afhankelijk van de selecties die u hebt aangebracht in het dialoogvenster **Domeincontroller wijzigen** .  


|Selectie #|Als u kiest voor...|Dit zal gebeuren.|
|---|---|---|
|1.|Start de passieve domeincontroller.|Een taak wordt gemaakt om de domeincontroller opnieuw opstarten en u krijgt bericht wanneer de taak is gemaakt. Dit wordt de domeincontroller opnieuw starten. Kunt u het proces opnieuw controleren door in te loggen **Service > Dashboard > weergeven van Logboeken voor** en vervolgens filteren door parameters die specifiek zijn voor uw service.|
|2.|Start de actieve domeincontroller.|De volgende waarschuwing wordt weergegeven: 'als u de actieve domeincontroller opnieuw start, het apparaat wordt failover uitvoeren naar de passieve controller. Wilt u doorgaan?" </br>Als u ervoor kiest om verder te gaan met deze bewerking, de volgende stappen zijn identiek aan die welke de passieve controller opnieuw (Zie selectie 1).|
|3.|Sluit de passieve controller.|U ziet het volgende bericht: "nadat het systeem is uitgeschakeld, u moet push-/ uit-knop op uw domeincontrollers wilt inschakelen. Weet u zeker dat u deze controller afsluiten?" </br>Als u ervoor kiest om verder te gaan met deze bewerking, de volgende stappen zijn identiek aan die welke de passieve controller opnieuw (Zie selectie 1).|
|4.|Sluit de actieve domeincontroller.|U ziet het volgende bericht: "nadat het systeem is uitgeschakeld, u moet push-/ uit-knop op uw domeincontrollers wilt inschakelen. Weet u zeker dat u deze controller afsluiten?" </br>Als u ervoor kiest om verder te gaan met deze bewerking, de volgende stappen zijn identiek aan die welke de passieve controller opnieuw (Zie selectie 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Opnieuw opstarten of afsluiten van een domeincontroller in Windows PowerShell voor StorSimple
Voer de volgende stappen uit als u wilt afsluiten of opnieuw opstarten van één controller op uw apparaat StorSimple vanuit Azure klassieke portal.


1. Toegang tot het apparaat via de seriële console of met een Telnet-sessie vanaf een externe computer. Verbinding maken met domeincontroller 0 of 1 Controller door de stappen in de [Stopverf verbinding maken met de seriële console apparaat gebruiken](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

1. Kies optie 1, **zich aanmelden met volledige toegang**in het menu van de seriële console.

1. In het vaandel wordt weergegeven, noteert u de controller dat u met (Controller 0 of 1 Controller verbonden bent) en de actieve of de passieve controller (stand-by).
    - Typ het volgende als u één controller bij de prompt wilt afsluiten:

        `Stop-HcsController`

        Hiermee sluit u de domeincontroller waarmee u bent verbonden. Als u de actieve domeincontroller stopt, wordt vervolgens het niet via de controller passieve voordat het wordt afgesloten.
    - Om opnieuw een domeincontroller bij de opdrachtprompt te typen:

        `Restart-HcsController`

        Dit wordt opnieuw opgestart de domeincontroller waarmee u bent verbonden. Als u de actieve domeincontroller opnieuw start, wordt het niet via de passieve controller voor het opnieuw opstarten.


## <a name="shut-down-a-storsimple-device"></a>Een apparaat StorSimple afsluiten

In dit gedeelte wordt uitgelegd hoe u een actief of een apparaat StorSimple vanaf een externe computer afsluiten. Een apparaat is uitgeschakeld na de apparaatcontrollers afgesloten. Het afsluiten van een apparaat gebeurt als het apparaat daadwerkelijk is verplaatst, of wordt uitgeschakeld.

> [AZURE.IMPORTANT] Voordat u het apparaat uitschakelt, controleert u de status van de Apparaatonderdelen. Ga naar **apparaten > onderhoud > Status Hardware** en controleer of de status LED van alle onderdelen is groen. Alleen een gezonde apparaat heeft een groene status. Als uw apparaat is wordt hier een slecht werkend onderdeel vervangen, ziet u een mislukte (rood) of een gedegradeerde (geel) status voor de bijbehorende onderdelen.

#### <a name="to-shut-down-a-storsimple-device"></a>Een apparaat StorSimple afsluiten

1. Gebruik de procedure [opnieuw opstarten of afsluiten van een domeincontroller](#restart-or-shut-down-a-single-controller) te identificeren en de passieve controller op uw apparaat afgesloten. U kunt deze bewerking uitvoeren in de klassieke Azure portal of in Windows PowerShell voor StorSimple.
2. Herhaal deze stap voor het afsluiten van de actieve domeincontroller.
3. Nu moet u zoeken op het achterste vlak van het apparaat. Nadat de twee controllers zijn volledig wordt afgesloten, moet de status-LED's op beide domeincontrollers de rood knipperen. Als u het apparaat volledig uitschakelen op dit moment, spiegelen de netschakelaars op voeding en koeling Modules (PCMs) op de OFF-stand. Dit moet het apparaat uitschakelen.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Het apparaat standaardinstellingen opnieuw instellen

> [AZURE.IMPORTANT] Als u het apparaat resetten naar fabrieksinstellingen, neem dan contact op met Microsoft Support. De hieronder beschreven procedure moet worden gebruikt in combinatie met Microsoft Support.

Deze procedure wordt beschreven hoe u uw Microsoft Azure StorSimple apparaat resetten fabrieksinstellingen met behulp van Windows PowerShell voor StorSimple.
Verwijdert alle gegevens en instellingen opnieuw instellen van een apparaat uit het gehele cluster standaard.

Voer de volgende stappen uit als uw apparaat met Microsoft Azure StorSimple standaardinstellingen opnieuw wilt instellen:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Het apparaat resetten naar standaardinstellingen van Windows PowerShell voor StorSimple

1. Toegang tot het apparaat via de seriële console. Controleer of het spandoek bericht om ervoor te zorgen dat u met de actieve domeincontroller verbonden bent.

1. Kies optie 1, **zich aanmelden met volledige toegang**in het menu van de seriële console.

1. Een opdrachtprompt, typ de volgende opdracht het gehele cluster, verwijdert alle gegevens en metagegevens controller-instellingen opnieuw instellen:

    `Reset-HcsFactoryDefault`

    Als u in plaats daarvan een enkele domeincontroller herstellen, gebruik de cmdlet [Reset-HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) met de `-scope` parameter.)

    Het systeem opnieuw meerdere keren. U krijgt bij het opnieuw instellen van het met succes is voltooid. Afhankelijk van het systeemmodel kunt duurt 45 tot 60 minuten voor een apparaat 8100 en 60-90 minuten voor een 8600 om dit proces te voltooien.

    > [AZURE.TIP]

    > - Als u Update 1.2 of eerder gebruiken de `–SkipFirmwareVersionCheck` -parameter voor de firmware-versiecontrole overslaan (anders wordt er een firmware fout door incompatibel: Factory reset kan niet doorgaan omdat in de firmware-versies komen niet overeen. ).

    > - De factory reset procedure kan mislukken voor StorSimple apparaten die met Update 1 of 1.1 in de portal van de overheid en een succesvolle single of dual controller vervanging (met vervanging van domeincontrollers die zijn verzonden met software Update vóór 1) hebt uitgevoerd. Dit gebeurt wanneer de factory reset afbeelding op de aanwezigheid van een SHA1-bestand op de domeincontroller die niet bestaat voor 1 vóór Update-software wordt gevalideerd. Als u deze factory reset fout ziet, neem dan contact op met Microsoft Support om u te helpen met de volgende stappen. Dit probleem is niet zichtbaar met vervangende-controllers vanuit de fabriek met Update 1 of hoger verzonden zijn.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Vragen en antwoorden over het beheren van domeincontrollers apparaat

In deze sectie, hebben we een aantal veelgestelde vragen samengevat beheren StorSimple apparaatcontrollers betreft.

**Q.** Wat gebeurt er als beide de controllers op mijn apparaat gezond en gedraaide zijn op en ik opnieuw opstarten of afsluiten van de actieve domeincontroller?

**A.** Als zowel de domeincontrollers op het apparaat gezond en ingeschakeld zijn, wordt u gevraagd om bevestiging. U kunt:

- **Start de domeincontroller actief** : U wordt gewaarschuwd dat een actieve domeincontroller opnieuw wordt opgestart, wordt het apparaat op de controller passieve failover. De controller wordt opnieuw opgestart.

- **Afsluiten met een actieve domeincontroller** : U wordt gewaarschuwd dat het afsluiten van een actieve domeincontroller leiden uitvaltijd tot zal. U moet ook de push-/ uit-knop op het apparaat in te schakelen op de domeincontroller.

**Q.** Wat gebeurt er als de passieve controller op mijn apparaat niet beschikbaar of is uitgeschakeld is uit en moet ik opnieuw opstarten of afsluiten van de actieve domeincontroller?

**A.** Als de passieve controller op uw apparaat is uitgeschakeld of niet beschikbaar is uitgeschakeld en u wilt:

- **Start de domeincontroller actief** : U wordt gewaarschuwd dat u doorgaat met de bewerking tot een tijdelijke onderbreking van de service leidt en dat u om bevestiging wordt gevraagd.

- **Afsluiten met een actieve domeincontroller** : U wordt gewaarschuwd dat u doorgaat met de bewerking zal leiden tot uitvaltijd en u moet push-/ uit-knop op een of beide domeincontrollers in te schakelen op het apparaat. U wordt om bevestiging gevraagd.

**Q.** Mislukt wanneer de domeincontroller opnieuw opstarten of afsluiten doet verlopen?

**A.** Opnieuw opstarten of afsluiten van een controller kan mislukken als:

- De update van een apparaat wordt uitgevoerd.

- Er wordt al een domeincontroller opnieuw op te starten.

- Er wordt al een domeincontroller afsluiten.

**Q.** Hoe kunt u zien als een domeincontroller is opgestart of afgesloten?

**A.** U kunt de status van de controller op de pagina Onderhoud controleren. De status van de domeincontroller wordt aangegeven of een domeincontroller is opgestart of afgesloten. De pagina waarschuwingen bevatten ook een informatieve waarschuwing als de controller is opgestart of afgesloten. De domeincontroller opnieuw opstarten en afsluiten bewerkingen zijn ook vastgelegd in de bewerking. Ga voor meer informatie over Logboeken [de logboeken](storsimple-service-dashboard.md#view-the-operations-logs)bekijken.

**Q.** Is er gevolgen voor de i/o's als gevolg van de controller failover?

**A.** De TCP-verbindingen tussen initiators en actieve domeincontroller wordt opnieuw ingesteld als gevolg van de controller failover, maar wordt pas gemaakt wanneer de passieve controller wordt ervan uitgegaan dat de bewerking. Kan er een pauze tijdelijk (minder dan 30 seconden) van de i/o-activiteit tussen initiators en het apparaat in de loop van deze bewerking.

**Q.** Hoe kan ik mijn controller nadat deze is afgesloten en verwijderd voor retourneren?

**A.** Een domeincontroller als u wilt service, moet u deze invoegen in het chassis, zoals wordt beschreven in [een controllermodule op uw apparaat StorSimple vervangen](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Volgende stappen

- Als u problemen met uw StorSimple apparaatcontrollers die u niet kunt oplossen ondervindt met de procedures die in deze zelfstudie, [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md).

- Voor meer informatie over het gebruik van de StorSimple Manager-service, gaat u naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md).
