<properties 
   pageTitle="Back-uptaken StorSimple Snapshot Manager | Microsoft Azure"
   description="Beschrijft hoe u de module MMC StorSimple Snapshot Manager weergeven en geplande, actieve en voltooide back-uptaken beheren."
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


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>StorSimple Snapshot Manager gebruiken om te bekijken en beheren van back-uptaken

## <a name="overview"></a>Overzicht

Het knooppunt van de **taken** in **het bereikvenster** bevat de **geplande**, **actieve** back-taken die u hebt gestart interactief of met een geconfigureerde beleid en **afgelopen 24 uur**. 

In deze zelfstudie wordt uitgelegd hoe u het knooppunt **taken** weer te geven informatie over geplande, actieve en recente back-uptaken kunt gebruiken. (De lijst met taken en de bijbehorende informatie weergegeven in het deelvenster **resultaten** ). Bovendien kunt u met de rechtermuisknop op een taak weergegeven en contextmenu met beschikbare acties weergegeven.

## <a name="view-scheduled-jobs"></a>Geplande taken weergeven

Gebruik de volgende procedure om geplande back-uptaken weer te geven.

#### <a name="to-view-scheduled-jobs"></a>Geplande taken weergeven

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. Vouw het knooppunt van de **taken** in **het bereikvenster** en op **gepland**. De volgende informatie weergegeven in het deelvenster **resultaten** :

    - **Naam** : de naam van de geplande opname

    - **Volgende uitgevoerd** : de datum en tijd van de volgende geplande momentopname

    - **Laatste start** – de datum en tijd van de meest recente geplande momentopname

    >[AZURE.NOTE] Voor eenmalige alleen momentopnamen, zal de **Volgende** en **Laatste uitgevoerd** hetzelfde zijn. 
 
    ![Geplande back-uptaken](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. Om aanvullende acties uitvoeren op een bepaalde taak, de taaknaam in **het resultatenvenster** met de rechtermuisknop en selecteer in het menuopties.

## <a name="view-recent-jobs"></a>Recente taken weergeven

Gebruik de volgende procedure voor het weergeven van back-up en terugzetten van taken die zijn voltooid in de laatste 24 uur.

#### <a name="to-view-recent-jobs"></a>Recente projecten bekijken

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Vouw het knooppunt van de **taken** in **het bereikvenster** en klikt u op de **laatste 24 uur**. **Het resultatenvenster** ziet u back-uptaken voor de laatste 24 uur (tot een maximum van 64 taken). De volgende informatie weergegeven in het deelvenster **resultaten** , afhankelijk van de opgegeven **weergave** -opties:

    - **Naam** : de naam van de geplande opname.
 
    - **Gestart** : de datum en tijd waarop de opname is begonnen.

    - **Gestopt** – de datum en tijd wanneer de momentopname is voltooid of is beëindigd.

    - Een time-out **verstreken** – de hoeveelheid tijd tussen de **gestart** en **gestopt** .

    - **Status** : de status van de onlangs voltooide afdruktaak. **Succes** geeft aan dat de back-up is gemaakt. **Mislukt** geeft aan dat de taak niet met succes is uitgevoerd.

    - **Informatie** : de reden voor de fout.

    - **Bytes verwerkt (MB)** : de hoeveelheid gegevens uit het volume groep die is verwerkt (in MB). 

    ![Taken die worden uitgevoerd in de afgelopen 24 uur](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. Om aanvullende acties uitvoeren op een bepaalde taak, de taaknaam in **het resultatenvenster** met de rechtermuisknop en selecteer in het menuopties.

    ![Een taak verwijderen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Actieve taken weergeven

Gebruik de volgende procedure om de taken weergeven die momenteel worden uitgevoerd.

#### <a name="to-view-currently-running-jobs"></a>Actieve taken weergeven

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Vouw het knooppunt van de **taken** in **het bereikvenster** en klikt u op **uitvoeren**. Afhankelijk van **de weergaveopties die u opgeeft,** wordt de volgende informatie weergegeven in het deelvenster **resultaten** : 

    - **Naam** : de naam van de geplande opname.

    - **Gestart** : de datum en tijd waarop de opname is begonnen.

    - **Checkpoint** : de huidige actie van de back-up.

    - **Status** : het percentage voltooid werk.
    
    - **Verstreken** – de hoeveelheid tijd die is verstreken sinds het begin van de back-up. 

    - **Gemiddelde doorvoer (MB)** : de verhouding van het aantal bytes aan dat van de totale tijd voor het verwerken van (MBs) verwerkt.

    - **Bytes verwerkt (MB)** : aantal bytes verwerkt (in MB).

    - **Bytes geschreven (MB)** : aantal bytes geschreven (in MB). Het bevat zowel de gegevens als de metagegevens en is daarom meestal groter is dan het aantal Bytes verwerkt.

    ![Taken die momenteel worden uitgevoerd](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. Om aanvullende acties uitvoeren op een bepaalde taak, de taaknaam in **het resultatenvenster** met de rechtermuisknop en selecteer in het menuopties.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple Snapshot Manager voor het beheren van uw oplossing StorSimple](storsimple-snapshot-manager-admin.md).
- Meer informatie over het [gebruik van StorSimple Snapshot Manager voor het beheren van back-upcatalogus](storsimple-snapshot-manager-manage-backup-catalog.md).















            


 

