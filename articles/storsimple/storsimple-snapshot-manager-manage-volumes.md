<properties 
   pageTitle="StorSimple Snapshot Manager en volumes | Microsoft Azure"
   description="Beschrijft hoe u de module MMC StorSimple Snapshot Manager weergeven en beheren van volumes en de back-ups configureren."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>StorSimple Snapshot Manager gebruiken om te bekijken en volumes beheren

## <a name="overview"></a>Overzicht

U kunt het knooppunt StorSimple Snapshot Manager **Volumes** (in **het bereikvenster)** volumes selecteren en weergeven van informatie over deze. De volumes worden gepresenteerd als stations die overeenkomen met de volumes die door de host worden gemonteerd. Het knooppunt **Volumes** worden lokale volumes en volumetypen die worden ondersteund door StorSimple, met inbegrip van volumes met iSCSI en een apparaat wordt gedetecteerd. 

Ga naar [ondersteuning voor meerdere volumetypen](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types)voor meer informatie over ondersteunde volumes.

![Volumelijst in het deelvenster met resultaten](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Het knooppunt **Volumes** kunt u ook scannen of na StorSimple Snapshot Manager deze detecteert volumes verwijderen. 

In deze zelfstudie wordt uitgelegd hoe u kunt koppelen, initialiseren, en volumes formatteren en vervolgens StorSimple Snapshot Manager:

- Informatie over volumes weergeven 
- Volumes verwijderen
- Volumes opnieuw scannen 
- Een standaardvolume configureren en een back-up
- Dynamische mirrored volumes configureren en een back-up

>[AZURE.NOTE] Alle **Volume** knooppunt acties zijn ook beschikbaar in het deelvenster **Acties** .
 
## <a name="mount-volumes"></a>Volumes koppelen

Gebruik de volgende procedure om te koppelen, initialiseren en StorSimple volumes formatteren. Deze procedure maakt gebruik van Schijfbeheer, een hulpprogramma voor het beheer van vaste schijven en de bijbehorende volumes of partities. Ga naar [Schijfbeheer](https://technet.microsoft.com/library/cc770943.aspx) op de Microsoft TechNet-website voor meer informatie over Schijfbeheer.

#### <a name="to-mount-volumes"></a>Volumes koppelen

1. Start de Microsoft iSCSI-initiator op de hostcomputer.

2. Een van de interface IP-adressen als de doelportal of discovery-IP-adres opgeven en verbinding met het apparaat. Zodra het apparaat is aangesloten, worden de volumes toegankelijk is voor uw Windows-systeem. Voor meer informatie over het gebruik van de Microsoft iSCSI-initiator, gaat u naar de sectie 'verbinding maken met een iSCSI-doelapparaat"in het [installeren en configureren van Microsoft iSCSI-Initiator][1].

3. Gebruik een van de volgende opties start Schijfbeheer:

    - Typ Diskmgmt.msc in het vak **uitvoeren** .

    - Start Server Manager, vouw het knooppunt **opslag** en selecteer vervolgens **Schijfbeheer**.

    - Selecteer vervolgens **Schijfbeheer**start, **Systeembeheer**en vouw het knooppunt in **Computerbeheer** . 

    >[AZURE.NOTE] U moet administrator-bevoegdheden voor schijfbeheer uitvoeren.
 
4. Het volume on line nemen:

   1. Klik in Schijfbeheer met de rechtermuisknop op een volume is gemarkeerd als **Offline**.

   2. Klik op **schijf opnieuw activeren**. De schijf moet de status **on line** de schijf opnieuw is geactiveerd.

5. Initialiseren van het volume (s):

   1. Met de rechtermuisknop op de volumes ontdekt.

   2. Selecteer in het menu **Schijf initialiseren**.

   3. Klik in het dialoogvenster **Schijf initialiseren** de schijven die u wilt initialiseren en klik vervolgens op **OK**.

6. Eenvoudige volumes formatteren:

   1. Met de rechtermuisknop op een volume dat u wilt opmaken.

   2. Selecteer in het menu **Nieuw eenvoudig Volume**.

   3. Gebruik de wizard Nieuw eenvoudig Volume om het volume te formatteren:

      - Geef de grootte van het volume.
      - Een stationsletter op te geven.
      - Selecteer het NTFS-bestandssysteem.
      - Geef een clustergrootte van 64 KB.
      - Snel formatteren.

7. Volumes met meerdere partitie formatteren. Ga naar de sectie 'partities en Volumes"in [Uitvoering van Schijfbeheer](https://msdn.microsoft.com/library/dd163556.aspx)voor instructies.

## <a name="view-information-about-your-volumes"></a>Informatie over de volumes weergeven

Gebruik de volgende procedure voor informatie over lokale en Azure StorSimple volumes.

#### <a name="to-view-volume-information"></a>Volume-informatie weergeven

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. Klik op het knooppunt **Volumes** in **het bereikvenster** . Er verschijnt een lijst van lokale en gekoppelde volumes, met inbegrip van alle Azure StorSimple volumes in het deelvenster **resultaten** . De kolommen in het deelvenster **resultaten** worden geconfigureerd. (Klik met de rechtermuisknop op het knooppunt **Volumes** , selecteert u **weergave**en selecteer **Kolommen toevoegen/verwijderen**).

    ![De kolommen configureren](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Kolom | Beschrijving 
    :--------------|:-------------
    Naam           | De kolom **naam** bevat de stationsletter die is toegewezen aan elk volume ontdekt.
    Apparaat         | De kolom **apparaat** bevat het IP-adres van het apparaat is aangesloten op de hostcomputer.
    Volume-apparaatnaam | De **Apparaatnaam Volume** kolom bevat de naam van het apparaat volume waarop het geselecteerde volume behoort. Dit is de naam van het volume in Azure klassieke portal voor dat specifieke volume is gedefinieerd.
    Toegangspaden   | De kolom **Toegangspaden** bevat het access-pad naar het volume. Dit is het station stationsletter of koppelpunt waarmee het volume toegankelijk op de hostcomputer is.
 
## <a name="delete-a-volume"></a>Een volume verwijderen

Gebruik de volgende procedure om een volume uit StorSimple Snapshot Manager verwijderen.

>[AZURE.NOTE] U kunt een volume niet verwijderen als deze deel uitmaakt van een volume groep. (De optie verwijderen is niet beschikbaar voor volumes die deel uitmaken van een volume groep.) De groep van de gehele volume om het volume te verwijderen, moet u verwijderen.


#### <a name="to-delete-a-volume"></a>Een volume verwijderen

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Klik op het knooppunt **Volumes** in **het bereikvenster** . 

3. In **het resultatenvenster** met de rechtermuisknop op het volume dat u wilt verwijderen.

4. Klik op **verwijderen**in het menu. 

    ![Een volume verwijderen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. Het dialoogvenster **Volume verwijderen** wordt weergegeven. **Bevestigen** typt in het tekstvak en klik vervolgens op **OK**.

6. Standaard StorSimple Snapshot Manager een back-up van een volume voordat u het verwijdert. Deze voorzorgsmaatregel kan u beschermen tegen verlies van gegevens als de verwijdering onbedoeld is. StorSimple Snapshot Manager geeft een voortgangsbericht **Automatische momentopname** wanneer wordt een back-up van het volume. 

    ![Automatische momentopname bericht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Volumes opnieuw scannen

Gebruik de volgende procedure de volumes verbonden StorSimple Snapshot Manager opnieuw scannen.

#### <a name="to-rescan-the-volumes"></a>De volumes opnieuw scannen

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. In het deelvenster **bereik** met de rechtermuisknop op de **Volumes**en klik vervolgens op **opnieuw scannen van volumes**.

    ![Volumes opnieuw scannen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Deze procedure synchroniseert de volumelijst StorSimple Snapshot Manager. Wijzigingen, zoals nieuwe volumes of volumes verwijderd, worden doorgevoerd in de resultaten.

## <a name="configure-and-back-up-a-basic-volume"></a>Configureren en een back-up een standaardvolume

Gebruik de volgende procedure om een back-up van een standaardvolume configureren en meteen een back-up of een beleid voor geplande back-ups maken.

### <a name="prerequisites"></a>Vereisten

Voordat u begint:

- Zorg ervoor dat de StorSimple-apparaat en de host-computer correct zijn geconfigureerd. Ga naar [uw apparaat op gebouwen StorSimple implementeren](storsimple-deployment-walkthrough-u2.md)voor meer informatie.

- Installeer en configureer StorSimple Snapshot Manager. Ga naar [StorSimple Snapshot Manager implementeren](storsimple-snapshot-manager-deployment.md)voor meer informatie.

#### <a name="to-configure-backup-of-a-basic-volume"></a>Back-up van een standaardvolume configureren

1. Een standaardvolume op de StorSimple apparaat maken.

2. Koppelen, initialiseren en het volume formatteren als beschreven in [volumes koppelen](#mount-volumes). 

3. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. De StorSimple Snapshot Manager venster verschijnt. 

4. In **het bereikvenster** met de rechtermuisknop op het knooppunt **Volumes** en selecteer vervolgens de **volumes opnieuw scannen**. Wanneer de scan is voltooid, moet een lijst met volumes worden weergegeven in het deelvenster **resultaten** . 

5. In **het resultatenvenster** met de rechtermuisknop op het volume en selecteer vervolgens **Volume groep maken**. 

    ![Volume-groep maken](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. Typ een naam voor de groep van het volume in het dialoogvenster **Volume groep maken** , volumes toe te kennen aan en klik op **OK**.

7. Vouw het knooppunt **Volume groepen** in het deelvenster **bereik** . Het nieuwe volume groep moet worden weergegeven onder het knooppunt **Groepen van Volume** . 

8. Met de rechtermuisknop op de naam van het volume.

    - U start een back-uptaak van interactieve (op verzoek), klikt u op **Back-up maken**. 

    - Als u een automatische back-up plannen, klikt u op **Back-up beleid maken**. Selecteer een groep van het volume in de lijst op de pagina **Algemeen** . Geef de details van het schema op de pagina **planning** . Klik op **OK**als u klaar bent. 

9. Vouw het knooppunt van de **taken** in **het bereikvenster** om te bevestigen dat de back-uptaak is begonnen, en klik vervolgens op het knooppunt **wordt uitgevoerd** . De lijst met actieve taken worden weergegeven in het deelvenster **resultaten** . 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Configureren en een back-up een dynamisch mirrored volume

Voer de volgende stappen uit om de back-up van een dynamisch mirrored volume configureren:

- Stap 1: Gebruik Schijfbeheer om een dynamische mirrored volume maken. 

- Stap 2: StorSimple Snapshot Manager gebruiken voor het configureren van back-up.

### <a name="prerequisites"></a>Vereisten

Voordat u begint:

- Zorg ervoor dat de StorSimple-apparaat en de host-computer correct zijn geconfigureerd. Ga naar [uw apparaat op gebouwen StorSimple implementeren](storsimple-deployment-walkthrough-u2.md)voor meer informatie.

- Installeer en configureer StorSimple Snapshot Manager. Ga naar [StorSimple Snapshot Manager implementeren](storsimple-snapshot-manager-deployment.md)voor meer informatie.

- Twee volumes op de StorSimple apparaat configureren. (In de voorbeelden worden de beschikbare volumes zijn **schijf 1** en **2 van de schijf**). 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Stap 1: Gebruik Schijfbeheer dynamische mirrored volumes maken

Schijfbeheer is een systeemhulpprogramma waarmee u vaste schijven en de volumes of partities op deze schijven beheren. Ga naar [Schijfbeheer](https://technet.microsoft.com/library/cc770943.aspx) op de Microsoft TechNet-website voor meer informatie over Schijfbeheer.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Dynamische mirrored volumes maken

1. Gebruik een van de volgende opties start Schijfbeheer: 

   - Open het **uitvoeren** , typ **Diskmgmt.msc**en druk op Enter.

   - Start Server Manager, vouw het knooppunt **opslag** en selecteer vervolgens **Schijfbeheer**. 

   - Selecteer vervolgens **Schijfbeheer**start, **Systeembeheer**en vouw het knooppunt in **Computerbeheer** . 

2. Zorg ervoor dat er twee volumes beschikbaar op het apparaat StorSimple. (In het voorbeeld wordt de beschikbare volumes zijn **schijf 1** en **2 van de schijf**). 

3. In de rechterkolom van het onderste deelvenster van het venster Schijfbeheer met de rechtermuisknop op de **schijf 1** en selecteer **Nieuwe Mirrored Volume**. 

    ![Nieuwe Mirrored Volume](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. Klik op **volgende**op de pagina van de wizard **Nieuwe Mirrored Volume** .

5. **Schijf 2** in het **geselecteerde** taakvenster selecteren op de pagina **Schijven selecteren** , klikt u op **toevoegen**en klik vervolgens op **volgende**. 

6. Accepteer de standaardinstellingen op de pagina **toegewezen stationsletter of pad** en klik vervolgens op **volgende**. 

7. Selecteer op de pagina **Indeling Volume** in het vak **Grootte van de toewijzingseenheid** **64 K**. Schakel het selectievakje **Snelformatteren** in en klik op **volgende**. 

8. Controleer de instellingen op de pagina **voltooien van het nieuwe Mirrored Volume** en klik op **Voltooien**. 

9. Er verschijnt een bericht om aan te geven dat de standaardschijf naar een dynamische schijf worden geconverteerd. Klik op **Ja**.

    ![Dynamische schijven converteren bericht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. Controleer of dat de schijf 1 en 2 van de schijf als dynamische mirrored volumes worden weergegeven in Schijfbeheer. (**Dynamische** moet worden weergegeven in de statuskolom en de kleur van de balk capaciteit moet veranderen in rood, die een mirrored volume aangeeft.) 

    ![Schijf beheer mirrored dynamische schijven](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Stap 2: Gebruik StorSimple Snapshot Manager back-up configureren

Gebruik de volgende procedure voor het configureren van dynamische mirrored volumes, en meteen een back-up of een beleid voor geplande back-ups maken.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Back-up van een dynamisch mirrored volume configureren

1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. De StorSimple Snapshot Manager venster verschijnt. 

2. Klik met de rechtermuisknop op het knooppunt **Volumes** in **het bereikvenster** en selecteert u **volumes opnieuw scannen**. Wanneer de scan is voltooid, moet een lijst met volumes worden weergegeven in het deelvenster **resultaten** . Dynamische mirrored volume wordt weergegeven als een enkel volume. 

3. In **het resultatenvenster** met de rechtermuisknop op het dynamische mirrored volume en klik vervolgens op **Volume groep maken**. 

4. **In het dialoogvenster **Volume groep maken** , typ een naam voor de groep van het volume en de dynamische mirrored volume toewijzen aan deze groep.** 

5. Vouw het knooppunt **Volume groepen** in het deelvenster **bereik** . Het nieuwe volume groep moet worden weergegeven onder het knooppunt **Groepen van Volume** . 

6. Met de rechtermuisknop op de naam van het volume. 

    - U start een back-uptaak van interactieve (op verzoek), klikt u op **Back-up maken**. 

    - Als u een automatische back-up plannen, klikt u op **Back-up beleid maken**. Selecteer de groep van het volume in de lijst op de pagina **Algemeen** . Geef de details van het schema op de pagina **planning** . Klik op **OK**als u klaar bent. 

7. Als dit wordt uitgevoerd, kunt u de back-uptaak controleren. Vouw het knooppunt **taken** in **het bereikvenster** en vervolgens op **uitvoeren**, de taakdetails worden weergegeven in het deelvenster **resultaten** . Wanneer de back-uptaak is voltooid, worden de details worden overgebracht naar de takenlijst van de **afgelopen 24** uur. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple Snapshot Manager voor het beheren van uw oplossing StorSimple](storsimple-snapshot-manager-admin.md).
- Meer informatie over het [gebruik van Snapshot Manager StorSimple naar volume groepen maken en beheren](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
