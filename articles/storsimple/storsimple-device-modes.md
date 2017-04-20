<properties 
   pageTitle="De modus StorSimple apparaat | Microsoft Azure"
   description="De modi StorSimple apparaat worden beschreven en wordt uitgelegd hoe u met behulp van Windows PowerShell voor StorSimple de cameramodus verandert."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>De modus van het apparaat op het apparaat StorSimple

Dit artikel bevat een korte beschrijving van de verschillende modi waarin het apparaat StorSimple kan werken. Uw apparaat StorSimple kan functioneren in drie modi: normaal, onderhoud en herstel. 

Na het lezen van dit artikel, zal u weten:

- Wat de StorSimple apparaat zijn
- Hoe om erachter te komen welke modus het apparaat StorSimple is in
- Het wijzigen van normaal in de onderhoudsmodus en *vice versa*


De bovenstaande taken kunnen alleen worden uitgevoerd via de Windows PowerShell-interface van uw apparaat StorSimple.

## <a name="about-storsimple-device-modes"></a>Over de StorSimple apparaat modi

Het apparaat StorSimple kan worden uitgevoerd in de modus Normaal, onderhoud of herstel. Elk van deze modi wordt hieronder kort beschreven.

### <a name="normal-mode"></a>Normale modus

Dit wordt gedefinieerd als de normale operationele modus voor een volledig geconfigureerde StorSimple apparaat. Standaard moet het apparaat in de normale modus.

### <a name="maintenance-mode"></a>Onderhoudsmodus

Soms moet het apparaat StorSimple kan worden geplaatst in de onderhoudsmodus. In deze modus kunt u onderhoud uitvoeren op het apparaat en storend updates, zoals die welke betrekking hebben op de schijf firmware installeren.

U kunt het systeem in onderhoudsmodus alleen via de Windows PowerShell voor StorSimple plaatsen. In deze modus worden alle i/o-aanvragen onderbroken. Services zoals niet-vluchtige RAM-geheugen (NVRAM) of de clustering-service worden ook gestopt. Zowel de domeincontrollers opnieuw worden gestart wanneer u deze modus af te sluiten of. Wanneer u de onderhoudsmodus afsluit, worden alle services wordt hervat en moeten gezonde. Dit kan enkele minuten duren.

>[AZURE.NOTE]**In deze modus wordt alleen ondersteund op een apparaat naar behoren werkt. Wordt niet ondersteund op een apparaat waarop een van de domeincontrollers of beide niet functioneren.**
</br>

### <a name="recovery-mode"></a>Herstelmodus

Herstelmodus kan worden omschreven als 'Windows veilige modus met netwerkondersteuning'. Herstelmodus voert het Microsoft Support team en kan ze voor het uitvoeren van diagnostische gegevens op het systeem. Het voornaamste doel van de recovery-modus is voor het ophalen van het systeemlogboek in Logboeken.

Als uw systeem herstel gaat, moet u contact opnemen met Microsoft Support voor volgende stappen. Ga naar [Contact met Microsoft ondersteuning](storsimple-contact-microsoft-support.md)voor meer informatie.

>[AZURE.NOTE] **U kunt het apparaat in de herstelmodus plaatsen. Als het apparaat in slechte staat is, probeert de herstelmodus om het apparaat in een staat waarin medewerkers van Microsoft Support kunnen onderzocht.**

## <a name="determine-storsimple-device-mode"></a>Apparaatmodus StorSimple bepalen

#### <a name="to-determine-the-current-device-mode"></a>Om te bepalen van de huidige apparatuurmodus

1. Meld u op de seriële console apparaat door de stappen in de [Stopverf verbinding maken met de seriële console apparaat gebruiken](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Bekijk het bericht van het spandoek in de seriële console-menu van het apparaat. Dit bericht expliciet wordt aangegeven of het apparaat in de modus onderhoud of herstel. Als het bericht bevat geen specifieke informatie met betrekking tot de systeem-modus, is het apparaat in de normale modus.

## <a name="change-the-storsimple-device-mode"></a>De modus StorSimple apparaat wijzigen 

Uitvoeren van onderhoud of onderhoud modus updates installeren, kunt u het apparaat StorSimple plaatsen in onderhoudsmodus (normale modus). Voer de volgende procedures om in of uit in de onderhoudsmodus.

> [AZURE.IMPORTANT] Voordat u de onderhoudsmodus invoert, controleert u of beide apparaatcontrollers gezonde door toegang tot de **Status van de Hardware** op de pagina **onderhoud** in de klassieke Azure portal. Als een of beide van de controllers niet in orde, neem dan contact op met Microsoft Support voor de volgende stappen. Ga naar [Contact met Microsoft ondersteuning](storsimple-contact-microsoft-support.md)voor meer informatie.

#### <a name="to-enter-maintenance-mode"></a>In te voeren in de onderhoudsmodus

1. Meld u op de seriële console apparaat door de stappen in de [Stopverf verbinding maken met de seriële console apparaat gebruiken](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Kies optie 1, **zich aanmelden met volledige toegang**in het menu van de seriële console. Geef desgevraagd het **beheerderswachtwoord apparaat**. Het standaardwachtwoord is: `Password1`.

3. Typ bij de opdrachtprompt 

    `Enter-HcsMaintenanceMode`

4. Er wordt een waarschuwingsbericht weergegeven waarin staat dat in de onderhoudsmodus zal verstoren alle i/o-aanvragen en de verbinding met de klassieke Azure portal server en u om bevestiging wordt gevraagd. Type **Y** om in te voeren in de onderhoudsmodus.

5. Beide controllers opnieuw. Als het herstarten voltooid is, weergegeven een ander bericht dat aangeeft dat het apparaat in de onderhoudsmodus.


#### <a name="to-exit-maintenance-mode"></a>Om af te sluiten in de onderhoudsmodus

1. Meld u op de seriële console apparaat. Controleren of bericht vanuit het vaandel uw apparaat in de onderhoudsmodus is.

2. Typ het volgende achter de opdrachtprompt:

    `Exit-HcsMaintenanceMode`

3. Een waarschuwing en een bevestigingsbericht verschijnt. Type **Y** om af te sluiten in de onderhoudsmodus.

4. Beide controllers opnieuw. Als het herstarten voltooid is, weergegeven een ander bericht dat aangeeft dat het apparaat in de normale modus.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [Normaal en onderhoud modus updates toe te passen](storsimple-update-device.md) op uw apparaat StorSimple.

