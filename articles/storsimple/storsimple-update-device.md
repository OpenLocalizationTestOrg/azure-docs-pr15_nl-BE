<properties
   pageTitle="Bijwerken van uw apparaat StorSimple | Microsoft Azure"
   description="Hoe normaal en onderhoud modus updates en hotfixes installeren met de functie StorSimple update."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Bijwerken van uw apparaat StorSimple 8000-serie

## <a name="overview"></a>Overzicht

De StorSimple updates kunt u gemakkelijk uw apparaat StorSimple om up-to-date te houden. Afhankelijk van het updatetype kunt u updates toepassen op het apparaat via de klassieke Azure portal of via de interface van Windows PowerShell. In deze zelfstudie wordt beschreven welke typen update en het installeren van elk van hen.

U kunt twee soorten apparaatupdates toepassen: 

- Regelmatige (of de normale modus) updates
- Onderhoud modus updates

U kunt regelmatig updates via de Windows PowerShell; of Azure klassieke portal installeren echter, moet u Windows PowerShell onderhoud modus updates te installeren. 

Elk updatetype wordt afzonderlijk, hieronder beschreven.

### <a name="regular-updates"></a>Regelmatige updates

Regelmatige updates zijn ononderbroken updates die kunnen worden geïnstalleerd wanneer het apparaat in de normale modus is. Deze updates worden toegepast via de Microsoft Update-website op elk apparaat-controller. 

> [AZURE.IMPORTANT] Controller failover kan optreden tijdens het bijwerken. Maar dit heeft geen invloed op beschikbaarheid van het systeem of de bewerking.

- Zie voor meer informatie over het installeren van regelmatige updates via de klassieke Azure portal [Installeer regelmatig updates via de Azure klassieke portal(#install-regular-updates-via-the-azure-classic-portal).

- Ook kunt u regelmatig updates via Windows PowerShell installeren voor StorSimple. Zie de [regelmatige updates via Windows PowerShell voor StorSimple installeren](#install-regular-updates-via-windows-powershell-for-storsimple)voor meer informatie.

### <a name="maintenance-mode-updates"></a>Onderhoud modus updates

Onderhoud modus updates zijn storend updates zoals schijf-firmware-upgrades. Deze updates moet het apparaat in de onderhoudsmodus moet worden geplaatst. Voor meer informatie, Zie [stap 2: Voer onderhoud modus](#step2). U niet de Azure klassieke portal gebruiken om onderhoud modus updates te installeren. In plaats daarvan moet u Windows PowerShell gebruiken voor StorSimple. 

Zie voor meer informatie over het installeren van updates voor onderhoud modus [installeren onderhoud modus updates via Windows PowerShell voor StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Onderhoud modus updates moeten afzonderlijk worden toegepast op elke domeincontroller. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Installeer regelmatig updates via de klassieke Azure portal

De klassieke Azure portal kunt u updates op uw apparaat StorSimple toepassen.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Regelmatige updates via Windows PowerShell voor StorSimple installeren

U kunt ook Windows PowerShell voor StorSimple (normale modus) regelmatige updates toe te passen.

> [AZURE.IMPORTANT] U kunt regelmatig updates met Windows PowerShell voor StorSimple installeren, wordt aangeraden dat u regelmatig updates via de klassieke Azure portal installeert. Beginnen met Update 1, moet vooraf worden gecontroleerd voordat u updates installeert vanaf de portal. Deze controles vooraf wordt voorrang nemen op fouten en een soepeler ervaring zorgen. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Onderhoud modus updates via Windows PowerShell voor StorSimple installeren

U gebruikt Windows PowerShell voor StorSimple onderhoud modus updates toepassen op uw apparaat StorSimple. In deze modus worden alle i/o-aanvragen onderbroken. Services zoals niet-vluchtige RAM-geheugen (NVRAM) of de clustering-service worden ook gestopt. Beide controllers worden opnieuw gestart wanneer u deze modus af te sluiten of. Wanneer u deze modus afsluit, worden alle services wordt hervat en moeten gezond. (Dit kan enkele minuten duren).

Als u onderhoud modus updates toe te passen, ontvangt u een melding via de klassieke Azure portal dat er updates moeten worden geïnstalleerd. Deze waarschuwing bevat instructies voor het gebruik van Windows PowerShell voor StorSimple om de updates te installeren. Nadat u het apparaat hebt bijgewerkt, wordt dezelfde procedure gebruiken om te wijzigen in de normale modus. Zie voor stapsgewijze instructies [stap 4: Exit onderhoudsmodus](#step4).

> [AZURE.IMPORTANT] 
> 
> - Voordat u de onderhoudsmodus invoert, controleert u of beide apparaatcontrollers gezond door het controleren van de **Status van de Hardware** op de pagina **onderhoud** in de klassieke Azure portal. Als de controller niet in orde is, neem dan contact op met Microsoft Support voor de volgende stappen. Ga naar contact met Microsoft ondersteuning voor meer informatie. 
> - Wanneer u zich in de onderhoudsmodus, moet u eerst de update toepassen op een domeincontroller en klik vervolgens op de andere controller.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Stap 1: Maak verbinding met de seriële console<a name="step1">

Gebruik een toepassing zoals stopverf eerst toegang te krijgen tot de seriële console. De volgende procedure wordt uitgelegd hoe met stopverf verbinding maken met de seriële console.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Stap 2: Geef de onderhoudsmodus<a name="step2">

Nadat u verbinding met de console, bepalen of er updates installeren en onderhoud modus te installeren.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Stap 3: De updates installeren<a name="step3">

Vervolgens de updates te installeren.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Stap 4: Exit onderhoudsmodus<a name="step4">

Ten slotte afsluiten in de onderhoudsmodus.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Installeren van hotfixes via Windows PowerShell voor StorSimple

In tegenstelling tot de updates voor Microsoft Azure StorSimple zijn hotfixes geïnstalleerd vanuit een gedeelde map. Er zijn twee soorten hotfixes zoals bij updates: 

- Regelmatig hotfixes 
- Onderhoud modus hotfixes  

De volgende procedures wordt uitgelegd hoe reguliere en onderhoud modus hotfixes installeren met Windows PowerShell voor StorSimple.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Wat gebeurt er met de updates als u een factory reset van het apparaat uitvoeren?

Als een apparaat wordt opnieuw ingesteld op de fabrieksinstellingen, zijn alle updates gaan verloren. Nadat het apparaat factory-reset is ingeschreven en geconfigureerd, moet u handmatig updates te installeren via de portal voor Azure klassieke en/of Windows PowerShell voor StorSimple. Opnieuw instellen, raadpleegt u [het apparaat standaardinstellingen opnieuw instellen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings)voor meer informatie over de fabriek.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Windows PowerShell voor StorSimple voor het beheren van uw apparaat StorSimple](storsimple-windows-powershell-administration.md).
- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
