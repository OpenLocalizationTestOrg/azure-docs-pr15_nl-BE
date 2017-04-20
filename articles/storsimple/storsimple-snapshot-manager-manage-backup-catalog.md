<properties 
   pageTitle="Back-upcatalogus StorSimple Snapshot Manager | Microsoft Azure"
   description="Beschrijft hoe u de module MMC StorSimple Snapshot Manager weergeven en beheren van de back-catalogus."
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

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Gebruik StorSimple Snapshot Manager voor het beheer van de back-catalogus

## <a name="overview"></a>Overzicht

De primaire functie van StorSimple Snapshot Manager is dat u toepassingen consistente back-ups van volumes StorSimple maken in de vorm van momentopnamen. Momentopnamen worden vervolgens weergegeven in een XML-bestand met de naam van een *back-upcatalogus*. De back-upcatalogus organiseert momentopnamen op volume groep en vervolgens op lokale snapshot of momentopname van de wolk. 

In deze zelfstudie wordt beschreven hoe u kunt het knooppunt van de **Catalogus van de back-up van** de volgende taken uitvoeren:

- Een volume terugzetten 
- Klonen van een volume of een volume groep 
- Een back-up verwijderen 
- Een bestand terugzetten
- De Storsimple Snapshot Manager-database terugzetten

U kunt de back-upcatalogus weergeven door het **Back-upcatalogus** knooppunt in het deelvenster **bereik** uit te breiden en vervolgens de groep volume uitbreiden.

- Als u op de naam van het volume, bevat het deelvenster **resultaten** het aantal lokale momentopnamen en cloud snapshots beschikbaar voor de volume-groep. 

- Als u op **Lokale Snapshot** of **Momentopname van de wolk**, ziet **het resultatenvenster** u de volgende informatie over elke back-snapshot (afhankelijk van de **weergave** -instellingen): 

    - **Naam** : de tijd dat de momentopname is gemaakt. 

    - **Type** : of dit een momentopname van een lokale of een momentopname van een wolk is. 

    - **Eigenaar** : de eigenaar van de inhoud. 

    - **Beschikbare** – of de momentopname die momenteel beschikbaar is. **True** geeft aan dat de momentopname beschikbaar is en kan worden hersteld; **False** geeft aan dat de momentopname niet langer beschikbaar is. 

    - **Ingevoerde** – of de back-up is geïmporteerd. **True** geeft aan dat de back-up is geïmporteerd uit de StorSimple Manager-service op het moment dat het apparaat is geconfigureerd in StorSimple Snapshot Manager; **False** geeft aan dat het niet is geïmporteerd, maar is gemaakt door StorSimple Snapshot Manager. (U kunt gemakkelijk herkennen een geïmporteerde volume groep omdat wordt een achtervoegsel toegevoegd die aangeeft dat het apparaat waaruit de groep volume is geïmporteerd.)

    ![Back-catalogus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Als u **Lokale Snapshot** of **Momentopname wolk**vouwen en klik vervolgens op de naam van een afzonderlijke snapshot, ziet **het resultatenvenster** u de volgende informatie over de momentopname die u hebt geselecteerd:

    - **Naam** : het volume dat wordt aangeduid met een stationsletter. 

    - **Lokale naam** : de lokale naam van het station (indien beschikbaar). 

    - **Apparaat** : de naam van het apparaat op het volume zich bevindt. 

    - **Beschikbare** – of de momentopname die momenteel beschikbaar is. **True** geeft aan dat de momentopname beschikbaar is en kan worden hersteld; **False** geeft aan dat de momentopname niet langer beschikbaar is. 


## <a name="restore-a-volume"></a>Een volume terugzetten

Gebruik de volgende procedure om een volume te herstellen vanaf back-up.

#### <a name="prerequisites"></a>Vereisten

Als u dit nog niet hebt gedaan, maakt u een volume en het volume groep en vervolgens het volume verwijderen. Standaard StorSimple Snapshot Manager een back-up van een volume voordat deze worden verwijderd. Deze voorzorgsmaatregel kan voorkomen dat gegevens verloren gaan als het volume per ongeluk is verwijderd of als de gegevens moeten worden hersteld om welke reden. 

StorSimple Snapshot Manager het volgende bericht wordt weergegeven terwijl de voorzorg back-up wordt gemaakt.

![Automatische momentopname bericht](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]U kunt een volume dat deel uitmaakt van een groep van het volume niet verwijderen. De optie verwijderen is niet beschikbaar. <br>

#### <a name="to-restore-a-volume"></a>Een volume herstellen

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. Vouw het knooppunt van de **Catalogus van de back-up** in **het bereikvenster** , een volume groep uitvouwen en klik op **Lokale momentopnamen** of **Momentopnamen die wolk**. Een lijst van momentopnamen voor back-up wordt weergegeven in het deelvenster **resultaten** . 

3. Zoek de back-up die u terugzetten wilt, klik met de rechtermuisknop, en klik op **herstellen**. 

    ![Back-upcatalogus herstellen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. **Op de bevestigingspagina van de details en **bevestigen**te typen.** De back-up StorSimple Snapshot Manager gebruikt om het volume te herstellen. 

    ![Bevestigingsbericht herstellen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Als dit wordt uitgevoerd, kunt u de actie VorigFormaat controleren. Vouw het knooppunt van de **taken** in **het bereikvenster** en klik vervolgens op **uitvoeren**. De taakdetails worden weergegeven in het deelvenster **resultaten** . Wanneer het terugzetten is voltooid, worden de taakdetails aan de lijst van de **afgelopen 24 uur** overgebracht.

## <a name="clone-a-volume-or-volume-group"></a>Klonen van een volume of een volume groep

Gebruik de volgende procedure een kopie van (kopie) van een volume of het volume groep te maken.

#### <a name="to-clone-a-volume-or-volume-group"></a>Kopie van een volume of een volume groep

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Vouw het knooppunt van de **Catalogus van de back-up** in **het bereikvenster** , een volume groep uitvouwen en klik op **Wolk momentopnamen**. Er verschijnt een lijst met back-ups in het deelvenster **resultaten** .

3. Het volume of de volume-groep die u wilt klonen en **klonen**Klik met de rechtermuisknop op het volume of de naam van volume vinden. Het dialoogvenster **Kloon wolk momentopname** wordt weergegeven.

    ![Een momentopname van een wolk klonen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Voer in het dialoogvenster **Momentopname van de wolk kloon** als volgt: 

    1. Typ in het tekstvak **naam** een naam voor het gekloonde volume. Deze naam wordt weergegeven in het knooppunt **Volumes** . 

    2. Selecteer **station**(optioneel) en selecteer een stationsletter in de vervolgkeuzelijst. 

    3. (Optioneel) Selecteer **Map (NTFS)**, typt u het pad naar een map of klikt u op Bladeren en selecteer een locatie voor de map. 

    4. Klik op **maken**.

5. Wanneer het klonen proces is voltooid, moet u het volume van de gekloonde initialiseren. Start Server Manager en vervolgens Schijfbeheer start. Zie voor gedetailleerde instructies [volumes koppelen](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Wanneer deze wordt geïnitialiseerd, wordt het volume weergegeven onder het knooppunt **Volumes** in **het bereikvenster** . Als u het volume vermeld niet ziet, vernieuwt u de lijst met volumes (Klik met de rechtermuisknop op het knooppunt **Volumes** en klik vervolgens op **vernieuwen**).

## <a name="delete-a-backup"></a>Een back-up verwijderen

Gebruik de volgende procedure om een momentopname te verwijderen uit de catalogus van de back-up. 

>[AZURE.NOTE]Een momentopname verwijdert, wordt de back-ups van gegevens die zijn gekoppeld aan de momentopname. Het proces van het opschonen van gegevens uit de cloud kan echter enige tijd duren.<br>
 
#### <a name="to-delete-a-backup"></a>Een back-up verwijderen

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Vouw het knooppunt van de **Catalogus van de back-up** in **het bereikvenster** , een volume groep uitvouwen en klik op **Lokale momentopnamen** of **Momentopnamen die wolk**. Een lijst met opnamen wordt weergegeven in het deelvenster **resultaten** . 

3. Klik met de rechtermuisknop op de momentopname die u wilt verwijderen en klik vervolgens op **verwijderen**.

4. Klik op **OK**als het bevestigingsbericht verschijnt. 

## <a name="recover-a-file"></a>Een bestand terugzetten

Als u een bestand per ongeluk is verwijderd van een volume, kunt u het bestand herstellen door een momentopname waarin datums vóór de verwijdering ophalen, met behulp van de momentopname te maken van het volume en vervolgens kopieert van de gekloonde volume aan het oorspronkelijke volume.

#### <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat er een actuele back-up van de groep van het volume. Vervolgens een bestand opgeslagen op een van de volumes in de groep volume verwijderen. Ten slotte de volgende stappen gebruiken om verwijderde bestanden terugzetten vanaf de back-up. 

#### <a name="to-recover-a-deleted-file"></a>Een verwijderd bestand terugzetten

1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het venster StorSimple Snapshot Manager-console wordt weergegeven. 

2. Vouw het knooppunt van de **Catalogus van de back-up** in **het bereikvenster** en blader naar een momentopname waarin het verwijderde bestand. Meestal moet u een momentopname die is gemaakt voordat de verwijdering. 

3. Zoeken naar het volume dat u kopiëren wilt, klik met de rechtermuisknop, en klik op **kopiëren**. Het dialoogvenster **Kloon wolk momentopname** wordt weergegeven.

    ![Een momentopname van een wolk klonen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Voer in het dialoogvenster **Momentopname van de wolk kloon** als volgt: 

   1. Typ in het tekstvak **naam** een naam voor het gekloonde volume. Deze naam wordt weergegeven in het knooppunt **Volumes** . 

   2. (Optioneel) **Station**selecteren en selecteer een stationsletter in de vervolgkeuzelijst. 

   3. (Optioneel) Selecteer de **Map (NTFS)**, en typ het pad naar een map of klik op **Bladeren selecteert u een locatie voor de map.** 

   4. Klik op **maken**. 

5. Wanneer het klonen proces is voltooid, moet u het volume van de gekloonde initialiseren. Start Server Manager en vervolgens Schijfbeheer start. Zie voor gedetailleerde instructies [volumes koppelen](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Wanneer deze wordt geïnitialiseerd, wordt het volume weergegeven onder het knooppunt **Volumes** in **het bereikvenster** . 

    Als u het volume vermeld niet ziet, vernieuwt u de lijst met volumes (Klik met de rechtermuisknop op het knooppunt **Volumes** en klik vervolgens op **vernieuwen**).

6. Open de NTFS-map met het gekloonde volume, vouw het knooppunt **Volumes** en open vervolgens het gekloonde volume. Zoek het bestand dat u wilt herstellen en kopieer het naar het primaire volume.

7. Nadat u het bestand wilt terugzetten, kunt u de NTFS-map waarin het gekloonde volume verwijderen.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>De StorSimple Snapshot Manager-database terugzetten

U dient regelmatig back-ups de StorSimple Snapshot Manager-database op de hostcomputer. Als een ramp plaatsvindt of de host-computer mislukt, kunt u deze terugzetten vanaf de back-up. Maken van back-up van de database is een handmatig proces.

#### <a name="to-back-up-and-restore-the-database"></a>Back-up maken en terugzetten van de database

1. Stop de Service Microsoft StorSimple Management:

    1. Start Server Manager.

    2. Selecteer de **Services**op het dashboard Serverbeheer in het menu **Extra** .

    3. Selecteer in het venster **Services** de **StorSimple Management-Service van Microsoft**.

    4. In het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, klikt u op **de service stoppen**.

2. Ga naar C:\ProgramData\Microsoft\StorSimple\BACatalog op de hostcomputer. 

    >[AZURE.NOTE] ProgramData is een verborgen map.
 
3. Het XML-catalogusbestand zoeken, het bestand kopiëren en de kopie opslaan op een veilige plaats of in de cloud. Als de host uitvalt, kunt u deze back-up te herstellen van de back-beleid dat u hebt gemaakt in StorSimple Snapshot Manager.

    ![Azure StorSimple back-catalogusbestand](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. Start de Service Microsoft StorSimple Management: 

    1. Selecteer de **Services**op het dashboard Serverbeheer in het menu **Extra** .
    
    2. Selecteer in het venster **Services** de **StorSimple Management-Service van Microsoft**.

    3. In het rechterdeelvenster, onder **Microsoft StorSimple Management-Service**, klikt u op **de service opnieuw starten**.

5. Ga naar C:\ProgramData\Microsoft\StorSimple\BACatalog op de hostcomputer. 

6. Het XML-catalogusbestand verwijderen en vervangen door de back-up die u hebt gemaakt. 

7. Klik op het pictogram Bureaublad StorSimple Snapshot Manager StorSimple Snapshot Manager starten. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple Snapshot Manager voor het beheren van uw oplossing StorSimple](storsimple-snapshot-manager-admin.md).
- Meer informatie over [StorSimple Snapshot Manager taken en werkstromen](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
