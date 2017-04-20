<properties
   pageTitle="Update 2 installeren op uw apparaat StorSimple | Microsoft Azure"
   description="Hoe StorSimple 8000-serie Update 2 installeren op uw apparaat StorSimple 8000-serie."
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

# <a name="install-update-2-on-your-storsimple-device"></a>Update 2 installeren op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u 2 hebt geïnstalleerd op een apparaat StorSimple is uitgevoerd van een eerdere softwareversie via de klassieke Azure portal. De zelfstudie omvat tevens de stappen die nodig zijn voor de update als een gateway voor een netwerkinterface dan 0 gegevens van het apparaat StorSimple is geconfigureerd en u probeert bij te werken vanuit een vooraf Update 1 softwareversie.

Update 2 bevat apparaat software-updates en updates voor stuurprogramma's LSI schijf firmware-updates. De apparaatsoftware LSI updates ononderbroken updates en zijn via de klassieke Azure portal kunnen worden toegepast. De schijf firmware-updates zijn storend updates en kunnen alleen worden toegepast via de Windows PowerShell-interface van het apparaat.

> [AZURE.IMPORTANT]

> -  Er kan geen Update 2 onmiddellijk omdat we een gefaseerde implementatie van de updates doen. Zoeken naar updates in een paar dagen opnieuw als deze Update beschikbaar komen binnenkort.
> - Een aantal handmatige en automatische voorafgaande controles voorafgaand aan de installatie om de toestand van het apparaat in de hardware staat en de netwerkconnectiviteit te worden gedaan. Deze voorafgaande controles worden alleen uitgevoerd als u de updates vanaf de klassieke Azure portal toepassen.
> - Het is raadzaam dat u de software en stuurprogramma-updates via de klassieke Azure-portal installeert. U moet alleen gaat u naar de Windows PowerShell-interface van het apparaat (updates te installeren) als de gateway vóór update controle in de portal mislukt. De updates duurt 4-7 uur te installeren (met inbegrip van de Windows-Updates). Het onderhoud modus-updates moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Onderhoud modus updates updates storend zijn, dit resulteert in een tijd voor uw apparaat.
> - Als de optionele StorSimple Snapshot Manager wordt uitgevoerd, moet u ervoor zorgen u uw Snapshot Manager versie hebt bijgewerkt naar Update 2 voor het bijwerken van het apparaat.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>2 geïnstalleerd via de klassieke Azure portal

Voer de volgende stappen voor het bijwerken van het apparaat naar [2 bijwerken](storsimple-update2-release-notes.md).


> [AZURE.NOTE]
Update 2 zorgt ervoor dat Microsoft om aanvullende diagnostische gegevens van het apparaat. Als gevolg hiervan bij ons team van bewerkingen wordt aangegeven welke apparaten die problemen ondervindt, zijn we beter uitgerust voor het verzamelen van informatie op het apparaat en het vaststellen van problemen. Update 2 accepteert, kunnen wij deze proactieve ondersteuning.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Controleer of het apparaat is **StorSimple 8000 serie Update 2 (6.3.9600.17673)**. De **datum voor het laatst bijgewerkt** moet ook worden gewijzigd. U zult ook zien dat onderhoud modus updates beschikbaar zijn (dit bericht mogelijk nog steeds weergegeven gedurende 24 uur nadat u de updates hebt geïnstalleerd).

    Onderhoud modus updates zijn storend updates die leiden tot uitvaltijd apparaat en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat. In sommige gevallen wanneer u Update 1.2, de firmware van uw schijf mogelijk al up-to-date is, in welk geval u hoeft niet alle onderhoud modus updates te installeren.

13. Download de updates van de modus onderhoud met behulp van de stappen uit [voor het downloaden van hotfixes](#to-download-hotfixes) wilt zoeken en downloaden van KB3121899, welke installaties schijf firmware updates (de andere updates moeten al geïnstalleerd nu).

13. Volg de stappen in het [installeren en onderhoud modus hotfixes controleren of](#to-install-and-verify-maintenance-mode-hotfixes) de modus onderhoud updates te installeren.


## <a name="install-update-2-as-a-hotfix"></a>2 als hotfix geïnstalleerd

Gebruik deze procedure als u het selectievakje gateway niet wanneer u probeert de updates te installeren via de klassieke Azure portal. De controle mislukt als er een gateway is toegewezen aan een niet-0 netwerkinterface en het apparaat een softwareversie dan Update 1 wordt uitgevoerd.

De versies van de software die u kunnen upgraden met behulp van de methode van de hotfix zijn Update 0.1, 0.2, Update en Update 0,3 Update 1, Update 1.1 en Update 1.2. De hotfix-methode bestaat uit de volgende drie stappen:

- De hotfixes downloaden vanaf de Microsoft Update-catalogus.
- Installeren en controleer of de normale modus hotfixes.
- Installeren en controleer of de modus onderhoud hotfix.

Als 2 als hotfix hebt geïnstalleerd, moet u downloaden en installeren van de volgende hotfixes:

| Order  | KB        | Beschrijving                    | Updatetype  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Software-update         |  Gewone     |
| 2      | KB3121900 | LSI-stuurprogramma              |  Gewone     |
| 3      | KB3080728 | Storport-fix </br> Windows Server 2012 R2 |  Gewone     |
| 4      | KB3090322 | Spaceport oplossing </br> Windows Server 2012 R2 |  Gewone     |
| 5      | KB3121899 | Schijf-firmware           | Onderhoud  |


> [AZURE.IMPORTANT]
>
> - Als uw apparaat versie Release (GA), neem contact op met [Microsoft Productondersteuning](storsimple-contact-microsoft-support.md) om u te helpen met de update.
> - Deze procedure moet slechts één keer worden uitgevoerd om de Update 2 van toepassing. U kunt de Azure klassieke portal latere updates toe te passen.
> - Elke hotfix-installatie kan voltooien duren ongeveer 20 minuten. Totale installatietijd is bijna 2 uur.
> - Voordat u deze procedure als u de update toe te passen, zorg ervoor dat beide apparaatcontrollers on line zijn en alle hardwareonderdelen in orde zijn.

Voer de volgende stappen uit om het toepassen van deze update als hotfix.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Update 2-release](storsimple-update2-release-notes.md).
