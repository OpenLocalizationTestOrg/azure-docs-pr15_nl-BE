<properties 
   pageTitle="Een volume StorSimple terugzetten vanaf back-up | Microsoft Azure"
   description="Hoe de pagina back-upcatalogus StorSimple Manager met een volume StorSimple terugzetten vanaf een back-upset."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Een volume StorSimple terugzetten vanaf een back-upset

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Overzicht

De **Back-upcatalogus** pagina toont alle reservekopieseries die worden gemaakt wanneer de handmatige of automatische back-ups worden genomen. U kunt deze pagina gebruiken om alle de back-ups weergeven voor een back-up beleid of een volume, selecteren of verwijderen van back-ups of een back-up kunt terugzetten of het klonen van een volume.

 ![Cataloguspagina een back-up](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

In deze zelfstudie wordt uitgelegd hoe u een volume op het apparaat terugzetten vanaf een back-upset met de pagina **Back-catalogus** .

## <a name="how-to-use-the-backup-catalog"></a>Het gebruik van de back-catalogus 

De pagina **Back-upcatalogus** biedt de selectie van een query die u helpt bij het beperken van de back-up instellen. U kunt filteren op de reservekopieseries die worden opgehaald op basis van de volgende parameters:

- **Apparaat** : het apparaat waarop u de back-upset is gemaakt.
- **Back-up beleid** of **volume** – de back-up beleid of het volume dat is gekoppeld aan deze back-upset.
- **Van** en **naar** – de datum- en tijdbereik wanneer de back-upset is gemaakt.

De gefilterde reservekopieseries worden vervolgens de tabelindeling op basis van de volgende kenmerken:

- **Naam** : de naam van de back-up beleid of het volume dat is gekoppeld aan de back-upset.
- **Grootte** : de werkelijke grootte van de back-upset.
- **Gemaakt op** : de datum en tijd waarop de back-ups zijn gemaakt. 
- **Type** : back-up sets kunnen worden lokale momentopnamen of momentopnamen van de wolk. Een lokale momentopname is een back-up van al uw volumegegevens lokaal opgeslagen op het apparaat dat u een momentopname van een wolk verwijst naar de back-up van het volumegegevens die zich in de cloud. Lokale momentopnamen bieden sneller toegang dat cloud momentopnamen voor meer herstellingsvermogen gegevens worden gekozen.
- **Gestart door** : de back-ups automatisch kunnen worden gestart volgens een schema of handmatig door een gebruiker. (U kunt een back-up beleid back-ups plannen. Ook kunt u de optie voor het **nemen van back-up** te nemen van een interactieve back-up.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Het volume van uw StorSimple terugzetten vanaf een back-up

De pagina **Back-catalogus** kunt u het volume van uw StorSimple een specifieke back-up terugzetten. 

> [AZURE.WARNING] Terugzetten vanaf een back-up vervangt de bestaande volumes uit de back-up. Dit kan leiden tot het verlies van alle gegevens die na de back-up is gemaakt.

Voordat u een terugzetten op een volume start, ervoor te zorgen dat het volume off line is. U moet eerst het volume off line nemen op de host en klik vervolgens op het apparaat. Volg de stappen in [een volume off line nemen](storsimple-manage-volumes.md#take-a-volume-offline). Voer de volgende stappen uit om een volume terugzetten vanaf een back-upset.

### <a name="to-restore-from-a-backup-set"></a>Terugzetten vanaf een back-upset

1. Klik op het tabblad **back-up-catalogus** op de pagina StorSimple Manager-service.

    ![Back-catalogus](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Selecteer een back-up als volgt instellen:
  1. Selecteer het juiste apparaat.
  2. Kies in de vervolgkeuzelijst het volume of de back-up beleid voor de back-up die u wilt selecteren.
  3. De periode opgeven.
  4. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) Deze query uitvoeren.
 
    De back-ups die zijn gekoppeld aan het geselecteerde volume of de back-beleid moet worden weergegeven in de lijst met back-upsets.

3. De back-up ingesteld op de bijbehorende volumes uitbreiden. Deze volumes moeten worden verbroken op de host en het apparaat voordat u deze kunt terugzetten. Volg de stappen in [een volume off line nemen](storsimple-manage-volumes.md#take-a-volume-offline).

    >  [AZURE.IMPORTANT] Zorg ervoor dat u hebt genomen offline de volumes op de host eerst voordat u offline de volumes op het apparaat. Als u de volumes offline op de host onderneemt, kan mogelijk leiden tot gegevensbeschadiging.

4. Selecteer een back-upset. Klik op **terugzetten** op de onderkant van de pagina.

6. U wordt om bevestiging gevraagd. 

    ![Bevestigingspagina](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Bekijk de informatie herstellen en klik op het pictogram voor ![Controleer pictogram](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Dit zal een terugzettaak die u bekijken kunt via de pagina **taken** starten. 

8. Nadat het terugzetten voltooid is, kunt u controleren of dat de inhoud van de volumes worden vervangen door de volumes van de back-up.

![Video beschikbaar](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video beschikbaar**

Om een video waarin wordt gedemonstreerd hoe u kunt de kloon en functies in StorSimple herstellen van verwijderde bestanden terugzetten, klik [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe u [volumes beheren StorSimple](storsimple-manage-volumes.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
