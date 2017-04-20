<properties 
   pageTitle="Een volume StorSimple terugzetten vanaf back-up | Microsoft Azure"
   description="Hoe de pagina back-upcatalogus StorSimple Manager met een volume StorSimple terugzetten vanaf een back-upset."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Een volume StorSimple terugzetten vanaf een back-upset (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Overzicht

De **Back-upcatalogus** pagina toont alle reservekopieseries die worden gemaakt wanneer de handmatige of automatische back-ups worden genomen. U kunt deze pagina gebruiken om alle de back-ups weergeven voor een back-up beleid of een volume, selecteren of verwijderen van back-ups of een back-up kunt terugzetten of het klonen van een volume.

 ![Cataloguspagina een back-up](./media/storsimple-restore-from-backup-set-u2/restore.png)

In deze zelfstudie wordt uitgelegd hoe de pagina **Back-catalogus** met uw apparaat terugzetten vanaf een back-upset.

U kunt een volume van een lokale of cloud back-up terugzetten. In beide gevallen wordt de bewerking voor terugzetten het volume on line onmiddellijk terwijl de gegevens op de achtergrond worden gedownload. 

Voordat u een terugzetbewerking start, moet u zich bewust zijn van de volgende opties:

- **Dat u moet het volume off line nemen** – het volume off line nemen op de host en het apparaat voordat u tot stand brengen de bewerking voor terugzetten. Hoewel de bewerking voor terugzetten om automatisch het volume on line brengen op het apparaat, moet u handmatig het apparaat on line brengen op de host. U kunt het volume on line brengen op de host als het volume on line op het apparaat is. (U hoeft niet te wachten tot de bewerking voor terugzetten is voltooid.) Voor procedures, gaat u naar [een volume off line nemen](storsimple-manage-volumes-u2.md#take-a-volume-offline).

- **Het type volume na het terugzetten** : verwijderde volumes worden hersteld op basis van het type in de momentopname; dat wil zeggen, volumes die lokaal zijn vastgemaakt worden hersteld als lokaal vastgezette volumes en volumes die trapsgewijs zijn geschakeld als trapsgewijs geschakelde volumes worden hersteld.

    Voor bestaande volumes vervangt het huidige gebruikstype van het volume het type dat is opgeslagen in de momentopname. Bijvoorbeeld als u een volume terugzet vanaf een momentopname die is ondernomen bij het volumetype is doorverbonden en volumetype is nu lokaal vastgehouden (als gevolg van een conversiebewerking die is uitgevoerd), vervolgens het volume teruggezet als een lokaal vaste volume. Op dezelfde manier als een bestaand lokaal vaste volume is uitgebreid en vervolgens hersteld op basis van een momentopname van een oudere genomen wanneer het volume kleiner is, behoudt het teruggezette volume de huidige grootte uitgevouwen.

    U kunt een volume niet converteren van een gelaagde volume naar een lokaal vaste volume of van een lokaal vaste volume naar een gelaagde volume terwijl het volume wordt hersteld. Wacht totdat de bewerking voor terugzetten is voltooid en kunt u het volume naar een ander type converteren. Ga naar [het volumetype wijzigen](storsimple-manage-volumes-u2.md#change-the-volume-type)voor meer informatie over het converteren van een volume. 

- De **grootte van het volume wordt doorgevoerd in het teruggezette volume** – dit is een belangrijke overweging als u een lokaal vaste volume dat is verwijderd (omdat lokaal vastgezette volumes volledig ingericht zijn) terugzet. Zorg ervoor dat u voldoende ruimte hebt voordat u probeert te herstellen van een lokaal vaste volume dat eerder is verwijderd. 

- **Dat u kunt een volume terwijl wordt hersteld niet uitbreiden** – wacht totdat de bewerking voor terugzetten is voltooid voordat u het volume uitbreiden. Ga voor meer informatie over het uitbreiden van een volume wilt [wijzigen van een volume](storsimple-manage-volumes-u2.md#modify-a-volume).

- **Kunt u een back-up terwijl u een lokaal volume terugzetten uitvoeren** – voor procedures gaat u naar [de StorSimple Manager-service voor het beheren van back-beleid gebruiken](storsimple-manage-backup-policies.md).

- **Dat u kunt een bewerking voor terugzetten Annuleren** : als u de terugzettaak wordt het volume annuleert worden, teruggedraaid naar de status waarin deze zich bevond voordat u de bewerking voor terugzetten hebt gestart. Voor procedures, gaat u naar [een taak annuleren](storsimple-manage-jobs-u2.md#cancel-a-job).

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

De pagina **Back-catalogus** kunt u het volume van uw StorSimple een specifieke back-up terugzetten. Houd er echter rekening mee, die herstellen van een volume wordt hersteld als het volume aan de staat waarin het zich bevond toen de back-up is gemaakt. Alle gegevens die zijn toegevoegd nadat de back-upbewerking niet verloren.

> [AZURE.WARNING] Terugzetten vanaf een back-up vervangt de bestaande volumes uit de back-up. Dit kan leiden tot het verlies van alle gegevens die na de back-up is gemaakt.

### <a name="to-restore-your-volume"></a>Het volume herstellen

1. Klik op het tabblad **back-up-catalogus** op de pagina StorSimple Manager-service.

    ![Back-catalogus](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Selecteer een back-up als volgt instellen:
  1. Selecteer het juiste apparaat.
  2. Kies in de vervolgkeuzelijst het volume of de back-up beleid voor de back-up die u wilt selecteren.
  3. De periode opgeven.
  4. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) Deze query uitvoeren.
 
    De back-ups die zijn gekoppeld aan het geselecteerde volume of de back-beleid moet worden weergegeven in de lijst met back-upsets.

3. De back-up ingesteld op de bijbehorende volumes uitbreiden. Deze volumes moeten worden verbroken op de host en het apparaat voordat u deze kunt terugzetten. Toegang tot de volumes op het **Volume Containers** pagina en volg de stappen in [een volume off line nemen](storsimple-manage-volumes-u2.md#take-a-volume-offline) ze off line nemen.

    > [AZURE.IMPORTANT] Zorg ervoor dat u hebt genomen offline de volumes op de host eerst voordat u offline de volumes op het apparaat. Als u de volumes offline op de host onderneemt, kan mogelijk leiden tot gegevensbeschadiging.

4. Ga terug naar het tabblad **Back-catalogus** en selecteert u een back-upset.

5. Klik op **terugzetten** op de onderkant van de pagina.

6. U wordt om bevestiging gevraagd. Controleer de gegevens herstellen en schakel vervolgens het selectievakje bevestiging.

    ![Bevestigingspagina](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Klik op het pictogram voor ![Controleer pictogram](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Dit zal een terugzettaak die u bekijken kunt via de pagina **taken** starten. 

8. Nadat het terugzetten voltooid is, kunt u controleren of dat de inhoud van de volumes worden vervangen door de volumes van de back-up.

![Video beschikbaar](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video beschikbaar**

Om een video waarin wordt gedemonstreerd hoe u kunt de kloon en functies in StorSimple herstellen van verwijderde bestanden terugzetten, klik [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Als het terugzetten is mislukt

U ontvangt een waarschuwing als de herstelbewerking mislukt. Als dit gebeurt, de back-up lijst om te controleren of de back-up nog steeds geldig vernieuwen. Als de back-up geldig is en u uit de cloud terugzetten wilt, vervolgens verbindingsproblemen mogelijk het probleem veroorzaakt. 

Als u de bewerking voor terugzetten wilt het volume off line nemen op de host en probeer de bewerking voor terugzetten. Houd er rekening mee dat elke wijziging van de gegevens die zijn uitgevoerd tijdens het terugzetten op volume verloren zullen gaan.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe u [volumes beheren StorSimple](storsimple-manage-volumes-u2.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
