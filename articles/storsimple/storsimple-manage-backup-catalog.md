<properties 
   pageTitle="Beheren van uw back-upcatalogus StorSimple | Microsoft Azure"
   description="Hoe de pagina back-upcatalogus StorSimple Manager met de optie en back-upsets voor een volume verwijderen."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>De StorSimple Manager-service gebruiken voor het beheren van uw back-catalogus

## <a name="overview"></a>Overzicht

De StorSimple Manager service **Back-upcatalogus** pagina toont alle reservekopieseries die worden gemaakt wanneer de handmatige of geplande back-ups worden genomen. U kunt deze pagina gebruiken om alle de back-ups weergeven voor een back-up beleid of een volume, selecteren of verwijderen van back-ups of een back-up kunt terugzetten of het klonen van een volume.

In deze zelfstudie wordt uitgelegd hoe u de optie en een back-upset verwijderd. Als u wilt weten hoe u uw apparaat herstellen vanaf back-up, gaat u naar [uw apparaat vanaf een back-upset herstellen](storsimple-restore-from-backup-set.md). Meer informatie over het klonen van een volume, gaat u naar [een volume StorSimple kloon](storsimple-clone-volume.md).

![Back-catalogus](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

De pagina **Back-upcatalogus** biedt de selectie van een query voor het beperken van de back-up instellen. U kunt filteren op de reservekopieseries die worden opgehaald op basis van de volgende parameters:

- **Apparaat** : het apparaat waarop u de back-upset is gemaakt.

- **Back-up beleid of Volume** – de back-up beleid of het volume dat is gekoppeld aan deze back-upset.

- **En** – datum- en tijdbereik wanneer de back-upset is gemaakt.

De gefilterde reservekopieseries worden vervolgens de tabelindeling op basis van de volgende kenmerken:

- **Naam** : de naam van de back-up beleid of het volume dat is gekoppeld aan de back-upset.

- **Grootte** : de werkelijke grootte van de back-upset.

- **Gemaakt op** : de datum en tijd waarop de back-ups zijn gemaakt. 

- **Type** : back-up sets kunnen worden lokale momentopnamen of momentopnamen van de wolk. Een lokale momentopname is een back-up van al uw volumegegevens lokaal opgeslagen op het apparaat dat u een momentopname van een wolk verwijst naar de back-up van het volumegegevens die zich in de cloud. Lokale momentopnamen bieden sneller toegang dat cloud momentopnamen voor meer herstellingsvermogen gegevens worden gekozen.

- **Gestart door** : de back-ups automatisch kunnen worden gestart door een schema of handmatig door een gebruiker. Een beleid voor back-up kunt u back-ups plannen. Ook kunt u de optie **back-up maken** naar een handmatige back-up maken.

## <a name="list-backup-sets-for-a-volume"></a>Lijst met back-up instellen voor een volume
 
Voer de volgende stappen uit om alle de back-ups voor een volume.

#### <a name="to-list-backup-sets"></a>Aan de lijst met back-upsets

1. Klik op het tabblad **back-up-catalogus** op de pagina StorSimple Manager-service.

2. Filter de selecties als volgt:

    1. Selecteer het juiste apparaat.

    2. Kies in de vervolgkeuzelijst een volume om de bijbehorende de back-ups weer te geven.

    3. De periode opgeven.

    4. Klik op het pictogram ![Pictogram](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Deze query uitvoeren.
 
    De back-ups die zijn gekoppeld aan het geselecteerde volume wordt weergegeven in de lijst met back-upsets.

## <a name="select-a-backup-set"></a>Selecteer een back-upset

Voer de volgende stappen uit als u wilt een back-up instellen voor een volume of een back-up beleid.

#### <a name="to-select-a-backup-set"></a>Een back-upset selecteren

1. Klik op het tabblad **back-up-catalogus** op de pagina StorSimple Manager-service.

2. Filter de selecties als volgt:

    1. Selecteer het juiste apparaat.

    2. Kies in de vervolgkeuzelijst het volume of de back-up beleid voor de back-up die u wilt selecteren.

    3. De periode opgeven.

    4. Klik op het pictogram ![Pictogram](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Deze query uitvoeren.

    De back-ups die zijn gekoppeld aan het geselecteerde volume of de back-beleid moet worden weergegeven in de lijst met back-upsets.

3. Selecteer een back-upset en uit. De opties voor **herstellen** en **verwijderen** worden weergegeven onder aan de pagina. U kunt een van deze acties uitvoeren op de back-upset die u hebt geselecteerd.

## <a name="delete-a-backup-set"></a>Een back-upset verwijderen

Een back-up te verwijderen wanneer u niet langer wilt bewaren de gegevens die zijn gekoppeld. Voer de volgende stappen uit om een back-upset verwijderd.

#### <a name="to-delete-a-backup-set"></a>Een back-upset verwijderen

1. Klik op het **tabblad back-up-catalogus**op de pagina StorSimple Manager-service.

2. Filter de selecties als volgt:

    1. Selecteer het juiste apparaat.

    2. Kies in de vervolgkeuzelijst het volume of de back-up beleid voor de back-up die u wilt selecteren.

    3. De periode opgeven.

    4. Klik op het pictogram ![Pictogram](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Deze query uitvoeren.

    De back-ups die zijn gekoppeld aan het geselecteerde volume of de back-beleid moet worden weergegeven in de lijst met back-upsets.

3. Selecteer een back-upset en uit. De opties voor **herstellen** en **verwijderen** worden weergegeven onder aan de pagina. Klik op **verwijderen**.

4. U krijgt bij het verwijderen wordt uitgevoerd en wanneer deze is voltooid. Nadat de verwijdering is voltooid, kunt u de query op deze pagina vernieuwen. De verwijderde back-upset wordt niet langer weergegeven in de lijst met back-upsets.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van de back-catalogus te herstellen van uw apparaat vanaf een back-upset](storsimple-restore-from-backup-set.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
