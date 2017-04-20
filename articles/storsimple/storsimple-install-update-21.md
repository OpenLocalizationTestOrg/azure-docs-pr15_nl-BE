<properties
   pageTitle="2.2 Update te installeren op uw apparaat StorSimple | Microsoft Azure"
   description="Hoe StorSimple 8000-serie Update 2.2 installeren op uw apparaat StorSimple 8000-serie."
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
   ms.date="08/02/2016"
   ms.author="alkohli" />

# <a name="install-update-22-on-your-storsimple-device"></a>2.2 Update te installeren op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u 2.2 Update installeren op een StorSimple apparaat met een eerdere softwareversie via de klassieke Azure portal en de hotfix-methode. De hotfix-methode wordt gebruikt als een gateway voor een netwerkinterface dan 0 gegevens van het apparaat StorSimple is geconfigureerd en u probeert bij te werken vanuit een vooraf Update 1 softwareversie.

Update 2.2 bevat apparaatsoftware, WMI- en iSCSI-updates. Als het bijwerken van versie 2.1, moet alleen het apparaat software-update worden toegepast. Als uit een pre-Update 2-versie bijwerkt, wordt u ook verplicht LSI stuurprogramma, Spaceport, Storport en schijf firmware-updates toe te passen. De apparaatsoftware WMI, iSCSI, LSI stuurprogramma, Spaceport en Storport correcties zijn ononderbroken updates en via de klassieke Azure portal kan worden toegepast. De schijf firmware-updates zijn storend updates en kunnen alleen worden toegepast via de Windows PowerShell-interface van het apparaat. 

> [AZURE.IMPORTANT]

> - Een aantal handmatige en automatische voorafgaande controles voorafgaand aan de installatie om de toestand van het apparaat in de hardware staat en de netwerkconnectiviteit te worden gedaan. Deze voorafgaande controles worden alleen uitgevoerd als u de updates vanaf de klassieke Azure portal toepassen.
> - Het is raadzaam dat u de software en stuurprogramma-updates via de klassieke Azure-portal installeert. U moet alleen gaat u naar de Windows PowerShell-interface van het apparaat (updates te installeren) als de gateway vóór update controle in de portal mislukt. Afhankelijk van de versie die u bijwerkt, duurt de updates 1,5-2,5 uur om te installeren. Het onderhoud modus-updates moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Onderhoud modus updates updates storend zijn, dit resulteert in een tijd voor uw apparaat.
> - Als de optionele StorSimple Snapshot Manager wordt uitgevoerd, moet u ervoor zorgen u uw Snapshot Manager versie hebt bijgewerkt naar 2.2 Update voor het bijwerken van het apparaat.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>2.2 Update installeren via de klassieke Azure portal

Voer de volgende stappen uit om het apparaat bijwerken naar [2.2 bijwerken](storsimple-update21-release-notes.md).


> [AZURE.NOTE]
Als u Update 2 toepast of later (met inbegrip van 2.1 Update), Microsoft kan worden om aanvullende diagnostische gegevens van het apparaat. Als gevolg hiervan bij ons team van bewerkingen wordt aangegeven welke apparaten die problemen ondervindt, zijn we beter uitgerust voor het verzamelen van informatie op het apparaat en het vaststellen van problemen. Update 2 of hoger te accepteren, kunnen wij deze proactieve ondersteuning.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Controleer of het apparaat is **StorSimple 8000 serie Update 2.2 (6.3.9600.17708)**. De **datum voor het laatst bijgewerkt** moet ook worden gewijzigd. 

    Als u vanaf een eerdere versie dan Update 2 bijwerkt, ook ziet u dat de modus onderhoud updates beschikbaar zijn (dit bericht mogelijk nog steeds weergegeven gedurende 24 uur nadat u de updates hebt geïnstalleerd).

    Onderhoud modus updates zijn storend updates die leiden tot uitvaltijd apparaat en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat. In sommige gevallen wanneer u Update 1.2, de firmware van uw schijf mogelijk al up-to-date is, in welk geval u hoeft niet alle onderhoud modus updates te installeren.

    Als u vanaf Update 2 bijwerkt, het apparaat worden nu bijgewerkt. U kunt de resterende stappen overslaan.

13. Download de updates van de modus onderhoud met behulp van de stappen uit [voor het downloaden van hotfixes](#to-download-hotfixes) wilt zoeken en downloaden van KB3121899, welke installaties schijf firmware updates (de andere updates moeten al geïnstalleerd nu).

13. Volg de stappen in het [installeren en onderhoud modus hotfixes controleren of](#to-install-and-verify-maintenance-mode-hotfixes) de modus onderhoud updates te installeren. 

  

## <a name="install-update-22-as-a-hotfix"></a>2.2 Update installeren als u een hotfix

Gebruik deze procedure als u het selectievakje gateway niet wanneer u probeert de updates te installeren via de klassieke Azure portal. De controle mislukt als er een gateway is toegewezen aan een niet-0 netwerkinterface en het apparaat een softwareversie dan Update 1 wordt uitgevoerd.

De versies van de software die u kunnen upgraden met behulp van het hotfix-methode zijn:

- 0.1, 0.2, 0.3 bijwerken
- Update 1, 1.1, 1.2
- Update 2, 2.1 

> [AZURE.IMPORTANT]
>
> - Als uw apparaat versie Release (GA), neem contact op met [Microsoft Productondersteuning](storsimple-contact-microsoft-support.md) om u te helpen met de update.

De hotfix-methode bestaat uit de volgende drie stappen:

- De hotfixes downloaden vanaf de Microsoft Update-catalogus.
- Installeren en controleer of de normale modus hotfixes.
- Installeren en controleer of de hotfix onderhoud modus (alleen bij het bijwerken van 2 vóór Update-software).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Updates downloaden voor een apparaat waarop software Update 2.1

**Als uw apparaat bijwerken 2.1 wordt uitgevoerd**, moet u alleen het apparaat software-update KB3179904 downloaden. Installeer alleen het binaire bestand, voorafgegaan door 'alle hcsmdssoftwareudpate'. Het DIS niet installeren en de update van de agent MDS voorafgegaan door `all-cismdsagentupdatebundle`. Niet doet, zal resulteren in een fout. Dit is een niet-onderbrekende update zal, IO wordt niet verstoord en het apparaat niet elke uitvaltijd.


#### <a name="download-updates-for-a-device-running-update-2-software"></a>Updates downloaden voor een apparaat waarop software Update 2

**Als uw apparaat 2 Update wordt uitgevoerd**, moet u downloaden en installeren van de volgende hotfixes in de aangegeven volgorde:

| Order  | KB        | Beschrijving                    | Updatetype  | Installatietijd |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3179904 | Software-update & #42;  |  Gewone <br></br>Niet storend     | ~ 45 minuten |
| 2.      | KB3146621 | iSCSI-pakket | Gewone <br></br>Niet storend  | ~ 20 minuten |
| 3.      | KB3103616 | WMI-pakket |  Gewone <br></br>Niet storend      | ~ 12 minuten |


 & #42;  *Opmerking, software update bestaat uit twee binaire bestanden: apparaat software-update worden voorafgegaan door `all-hcsmdssoftwareupdate` en de Cis- en Mds agent voorafgegaan door `all-cismdsagentupdatebundle`. De software-update van het apparaat moet zijn geïnstalleerd voordat de agent Cis en Mds. U moet ook opnieuw starten met de actieve domeincontroller via de `Restart-HcsController` cmdlet na het toepassen van de agent Cis en MDS bijwerken (en voordat de toepassing van de resterende updates).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Voor een apparaat met 2 vóór Update-software-updates downloaden

**Als uw apparaat versie 0.2, 0.3, 1.0 en 1.1 wordt uitgevoerd**, moet u downloaden en installeren het LSI-stuurprogramma en firmware bijwerken naast de software, WMI- en iSCSI-updates. Deze update is al geïnstalleerd als u Update 1.2 of 2. 
 
| Order  | KB        | Beschrijving                    | Updatetype  | Installatietijd |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3121900 | LSI stuurprogramma en firmware             |  Gewone <br></br>Niet storend      | ~ 20 minuten |


<br></br>
**Als uw apparaat versie 0.2, 0.3, 1.0, 1.1 en 1.2 wordt uitgevoerd**, moet u downloaden en installeren van de Spaceport en het Storport-correctie. Deze zijn al geïnstalleerd als Update 2 wordt uitgevoerd.

| Order  | KB        | Beschrijving                    | Updatetype  | Installatietijd |
|--------|-----------|-------------------------|------------- |-------------|
| 5.      | KB3090322 | Spaceport oplossing </br> Windows Server 2012 R2 |  Gewone <br></br>Niet storend      | ~ 20 minuten |
| 6.      | KB3080728 | Storport-fix </br> Windows Server 2012 R2 |  Gewone <br></br>Niet storend      | ~ 20 minuten |



<br></br>
Mogelijk moet u ook schijf firmware updates te installeren. U kunt controleren of u de schijf firmware-updates door te voeren moet de `Get-HcsFirmwareVersion` cmdlet. Als u werkt met deze firmware-versies: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, dan u niet hoeft om deze updates te installeren.


| Order  | KB        | Beschrijving                    | Updatetype  | Installatietijd |
|--------|-----------|-------------------------|------------- |-------------|
| 7.      | KB3121899 | Schijf-firmware              |  Onderhoud <br></br>Storend      | ~ 30 minuten |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Deze procedure moet slechts één keer worden uitgevoerd om de 2.2-Update toepassen. U kunt de Azure klassieke portal latere updates toe te passen.
> - Als Update 2, wordt de totale installatietijd is bijna 1,5 uur.
> - Voordat u deze procedure voor het toepassen van de update, zorg ervoor dat de apparaatcontrollers on line zijn en de hardware zijn onderdelen in orde.

Voer de volgende stappen uit om te downloaden en installeren van de hotfixes.

[AZURE.INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [versie 2.1 Update](storsimple-update21-release-notes.md).
