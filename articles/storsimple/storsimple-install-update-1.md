<properties
   pageTitle="Update 1.2 installeren op uw apparaat StorSimple | Microsoft Azure"
   description="Hoe StorSimple 8000-serie Update 1.2 installeren op uw apparaat StorSimple 8000-serie."
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
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Update 1.2 installeren op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u Update 1.2 installeren op een StorSimple apparaat waarop een softwareversie voordat u Update 1. De zelfstudie omvat ook de extra stappen die nodig zijn voor de update als een gateway voor een netwerkinterface dan 0 gegevens van het apparaat StorSimple is geconfigureerd.

Update 1.2 bevat apparaat software-updates, stuurprogramma-updates van LSI en schijf firmware-updates. De software en updates voor stuurprogramma's LSI ononderbroken updates en via de klassieke Azure portal kunnen worden toegepast. De schijf firmware-updates zijn storend updates en kunnen alleen worden toegepast via de Windows PowerShell-interface van het apparaat.

Afhankelijk van welke versie van het apparaat wordt uitgevoerd, kunt u bepalen als Update 1.2 wordt toegepast. De softwareversie van het apparaat kunt u controleren door naar de sectie **Overzicht** van het apparaat **Dashboard**te gaan.

</br>

| Als softwareversie...   | Wat gebeurt er in de portal?                              |
|---------------------------------|--------------------------------------------------------------|
| Release - NH                    | Als u werkt met versie (GA), niet van toepassing met deze update. Neem [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) om het bijwerken van het apparaat.|
| Update 0,1                      | Portal van Update 1.2 toepassing.                                |
| 0,2 bijwerken                      | Portal van Update 1.2 toepassing.                                |
| 0,3 bijwerken                      | Portal van Update 1.2 toepassing.                                |
| Update 1                        | Deze update is niet beschikbaar.                           |
| Update 1.1                      | Deze update is niet beschikbaar.                           |

</br>

> [AZURE.IMPORTANT]

> -  Er kan geen Update 1.2 onmiddellijk omdat we een gefaseerde implementatie van de updates doen. Zoeken naar updates in een paar dagen opnieuw als deze Update beschikbaar komen binnenkort.
> - Deze update bevat een aantal handmatige en automatische vooraf controleert de status van het apparaat in termen van hardware staat en de netwerkconnectiviteit. Deze voorafgaande controles worden alleen uitgevoerd als u de updates vanaf de klassieke Azure portal toepassen.
> - Het is raadzaam dat u de software en stuurprogramma-updates via de klassieke Azure-portal installeert. U moet alleen gaat u naar de Windows PowerShell-interface van het apparaat (updates te installeren) als de gateway vóór update controle in de portal mislukt. De updates duurt 5-10 uur te installeren (met inbegrip van de Windows-Updates). Het onderhoud modus-updates moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Onderhoud modus updates updates storend zijn, dit resulteert in een tijd voor uw apparaat.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Update 1.2 installeren via de klassieke Azure portal

Voer de volgende stappen voor het bijwerken van het apparaat naar [1.2 bijwerken](storsimple-update1-release-notes.md). Gebruik deze procedure alleen als u een gateway ingesteld op 0 netwerkinterface gegevens op uw apparaat hebt.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Controleer of het apparaat is **StorSimple 8000 serie Update 1.2 (6.3.9600.17584)**. De **datum voor het laatst bijgewerkt** moet ook worden gewijzigd. U zult ook zien dat onderhoud modus updates beschikbaar zijn (dit bericht mogelijk nog steeds weergegeven gedurende 24 uur nadat u de updates hebt geïnstalleerd).

    Onderhoud modus updates zijn storend updates die leiden tot uitvaltijd apparaat en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat.

    ![Pagina Onderhoud] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Pagina Onderhoud")

13. Download de updates van de modus onderhoud met behulp van de stappen uit [voor het downloaden van hotfixes]( #to-download-hotfixes) wilt zoeken en downloaden van KB3063416, welke installaties schijf firmware updates (de andere updates moeten al geïnstalleerd nu).

13. Volg de stappen in het [installeren en onderhoud modus hotfixes controleren of](#to-install-and-verify-maintenance-mode-hotfixes) de modus onderhoud updates te installeren.

14. In de klassieke Azure portal, navigeer naar de pagina **onderhoud** en onderaan de pagina, klikt u op **Updates scannen** om te controleren of er updates voor Windows en klik vervolgens op **Updates installeren**. U hebt nadat alle updates zijn geïnstalleerd.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Update 1.2 installeren op een apparaat met een gateway voor een non-DATA 0 netwerkinterface is geconfigureerd

Moet u deze procedure alleen als u het selectievakje gateway niet wanneer u probeert de updates te installeren via de klassieke Azure portal. De controle mislukt als er een gateway is toegewezen aan een niet-0 netwerkinterface en het apparaat een softwareversie dan Update 1 wordt uitgevoerd. Als uw apparaat heeft geen gateway op een niet-0 netwerkinterface, kunt u het apparaat direct vanaf de portal voor Azure klassieke bijwerken. Zie [Install 1.2 via de klassieke Azure portal bijwerken](#install-update-1.2-via-the-azure-classic-portal).

De versies van de software die u kunnen upgraden met behulp van deze methode zijn updates Update 0.1, 0.2, en 0,3.


> [AZURE.IMPORTANT]
>
> - Als uw apparaat versie Release (GA), neem contact op met [Microsoft Productondersteuning](storsimple-contact-microsoft-support.md) om u te helpen met de update.
> - Deze procedure moet slechts één keer worden uitgevoerd voor het toepassen van Update 1.2. U kunt de Azure klassieke portal latere updates toe te passen.

Als het apparaat 1 vóór Update-software wordt uitgevoerd en er een gateway voor een netwerkinterface dan gegevens 0 is ingesteld, kunt u de Update 1.2 toepassen op de volgende twee manieren:

- **Optie 1**: de update downloaden en toepassen met behulp van de `Start-HcsHotfix` cmdlet uit de Windows PowerShell-interface van het apparaat. Dit is de aanbevolen methode. **Gebruik deze methode niet toepassen van Update 1.2 als uw apparaat Update 1.0 of 1.1 van de Update wordt uitgevoerd.**

- **Optie 2**: de configuratie van de standaardgateway verwijderen en installeren van de update rechtstreeks vanuit de klassieke Azure portal.


In de volgende secties vindt u gedetailleerde instructies voor elk van deze.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Optie 1: Gebruik Windows PowerShell voor StorSimple Update 1.2 als hotfix toepassen

U moet deze procedure alleen gebruiken als u Update 0.1, 0.2, 0.3, en als uw gateway-controle is mislukt wanneer u probeert updates te installeren vanaf de klassieke Azure portal wordt uitgevoerd. Als u software Release (GA) uitvoert, kunt u [Microsoft-ondersteuning](storsimple-contact-microsoft-support.md) voor het bijwerken van het apparaat.

Update 1.2 als hotfix installeren, moet u downloaden en installeren van de volgende hotfixes:

| Order  | KB        | Beschrijving             | Updatetype  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Software-update         |  Gewone     |
| 2      | KB3043005 | LSI SAS-controller-update |  Gewone     |
| 3      | KB3063416 | Schijf-firmware           | Onderhoud  |

Voordat u deze procedure als u de update toe te passen, zorg ervoor dat:

- Beide apparaatcontrollers zijn on line.

Voer de volgende stappen uit om de Update 1.2 van toepassing. **De updates krijgen ongeveer 2 uur te voltooien (ongeveer 30 minuten voor software, 30 minuten voor de bestuurder, 45 minuten voor de firmware van de schijf).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Optie 2: Gebruik de Azure klassieke portal 1.2-Update toepassen na het verwijderen van de configuratie van de standaardgateway

Deze procedure geldt alleen voor StorSimple-apparaten die een versie van de software voordat u Update 1 en een gateway instellen op een netwerkinterface dan gegevens 0 hebben. U moet de gateway-instellingen voordat u de update toepast.

De update kan enkele uren duren. Als uw hosts zich in verschillende subnetten bevinden, verwijdert de configuratie van de standaardgateway op de iSCSI-interfaces kan leiden tot uitvaltijd. Wij raden aan 0 voor iSCSI-verkeer gegevens om de uitvaltijd te configureren.

Voer de volgende stappen uit om de netwerkinterface met de gateway uitschakelen en vervolgens de update toepast.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Update 1.2-release](storsimple-update1-release-notes.md).
