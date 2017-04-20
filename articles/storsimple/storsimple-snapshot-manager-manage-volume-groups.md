<properties 
   pageTitle="StorSimple Snapshot Manager volume groepen | Microsoft Azure"
   description="Beschrijving van hoe de module MMC StorSimple Snapshot Manager met volume groepen maken en beheren."
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

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>StorSimple Snapshot Manager gebruiken om het volume groepen maken en beheren

## <a name="overview"></a>Overzicht

U kunt het **Volume groepen** knooppunt in **het bereikvenster** volumes volume groepen toewijzen, informatie weergeven over een groep van volume, back-ups plannen en volume groepen bewerken. 

Volume-groepen zijn groepen verwante volumes gebruikt om ervoor te zorgen dat back-ups toepassing consistent zijn. Zie [Volumes en volume groepen](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) en [integratie met Windows Volume Shadow Copy-Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service)voor meer informatie.

>[AZURE.IMPORTANT] 
>
> * Alle volumes in een groep van het volume moeten afkomstig zijn uit een enkele cloud-serviceprovider.
> 
> * Bij het configureren van groepen van volume, niet-gedeelde volumes (CSVs) en CSVs in dezelfde groep volume mengen. StorSimple Snapshot Manager biedt geen ondersteuning voor een combinatie van CSVs en niet-CSVs in de dezelfde momentopname.
 
![Volume groepen knooppunt](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Figuur 1: StorSimple Snapshot Manager Volume groepen knooppunt** 

In deze zelfstudie wordt uitgelegd hoe u StorSimple Snapshot Manager kunt gebruiken:

- Informatie over uw volume groepen weergeven 
- Een groep van het volume maken
- Back-up van een volume groep
- Een volume groep bewerken
- Een volume groep verwijderen

Al deze acties zijn ook beschikbaar in het deelvenster **Acties** .
 
## <a name="view-volume-groups"></a>Volume-groepen weergeven

Als u op het knooppunt **Groepen van Volume** , ziet **het resultatenvenster** u de volgende informatie over elke groep volume, afhankelijk van de selecties van de kolom die u maakt. (De kolommen in het deelvenster **resultaten** worden geconfigureerd. Met de rechtermuisknop op het knooppunt **Volumes** , selecteert u **weergave**en klik vervolgens op **Kolommen toevoegen/verwijderen**.)

Kolom | Beschrijving 
:--------------|:------------ 
Naam           | De kolom **naam** bevat de naam van de groep van het volume.
Toepassing    | De kolom **toepassingen** bevat het nummer van de VSS-schrijvers geïnstalleerd en uitgevoerd op de Windows-host.
Geselecteerd       | De **geselecteerde** kolom bevat het nummer van de volumes die zijn opgenomen in de groep van het volume. Nul (0) geeft aan dat geen toepassing gekoppeld aan de volumes in de groep van het volume is.
Geïmporteerd       | De **ingevoerde** kolom bevat het nummer van de ingevoerde hoeveelheden. Als de waarde **True**is, deze kolom geeft aan dat een groep van het volume is geïmporteerd uit de klassieke Azure portal StorSimple Snapshot Manager is niet gemaakt.
 
>[AZURE.NOTE] StorSimple Snapshot Manager volume groepen worden ook weergegeven op het tabblad **Back-up beleid** in de klassieke Azure portal.
 
## <a name="create-a-volume-group"></a>Een groep van het volume maken

Gebruik de volgende procedure voor het maken van een volume groep.

#### <a name="to-create-a-volume-group"></a>Een groep van het volume maken

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. In **het bereikvenster** **Volume groepen**met de rechtermuisknop en klik vervolgens op **Volume groep maken**. 

    ![Volume-groep maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    Het dialoogvenster **een groep volume maken** wordt weergegeven. 

    ![Een dialoogvenster volume maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Voer de volgende gegevens: 

    1. Typ in het vak **naam** een unieke naam voor het nieuwe volume groep. 

    2. Selecteer in het vak **toepassingen** toepassingen die is gekoppeld aan de volumes die u aan de groep van het volume toevoegen wilt. 

        De **toepassingen** lijsten alleen die toepassingen die werken met volumes van StorSimple en VSS-schrijvers hebben ingeschakeld voor hen. Een VSS-schrijver is alleen beschikbaar als alle volumes die op de hoogte van de schrijver StorSimple volumes zijn. Als het vak toepassingen leeg is, worden geen toepassingen die gebruikmaken van Azure StorSimple volumes en VSS-schrijvers hebben ondersteund geïnstalleerd. (Momenteel Azure StorSimple ondersteunt Microsoft Exchange en SQL Server.) Zie [integratie met Volume Shadow Copy-Service Windows](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service)voor meer informatie over de VSS-schrijvers.

        Als u een toepassing selecteert, worden alle volumes die zijn gekoppeld aan deze automatisch geselecteerd. Omgekeerd, als u volumes die zijn gekoppeld aan een specifieke toepassing, de toepassing wordt automatisch geselecteerd in het vak **toepassingen** . 

    3. Selecteer in het vak **Volumes** StorSimple volumes toevoegen aan de groep van het volume. 

      - U kunt volumes met één of meerdere partities bevatten. (Meerdere volumes van partitie kunnen worden dynamische schijven of standaardschijven met meerdere partities.) Een volume met meerdere partities wordt behandeld als één eenheid. Dus als u slechts één van de partities aan een groep van het volume toevoegt, worden de partities automatisch toegevoegd aan die groep volume tegelijkertijd. Nadat u een volume met meerdere partitie aan de groep van een volume toevoegen, blijft het volume met meerdere partitie worden behandeld als één eenheid.

      - Lege volume groepen kunt u niet de volumes door aan te wijzen ze maken. 

      - Meng niet-gedeelde volumes (CSVs) en CSVs in dezelfde groep van het volume. StorSimple Snapshot Manager biedt geen ondersteuning voor een combinatie van CSV-volumes en niet-CSV-volumes in de dezelfde momentopname. 

4. Klik op **OK** als het volume groep wilt opslaan.

## <a name="back-up-a-volume-group"></a>Back-up van een volume groep

Gebruik de volgende procedure om een back-up van een volume groep.

#### <a name="to-back-up-a-volume-group"></a>Back-up van een volume groep

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Vouw het knooppunt **Groepen van Volume** in **het bereikvenster** met de rechtermuisknop op de naam van een volume groep en klik vervolgens op **Back-up maken**. 

    ![Onmiddellijk een back-up volume groep](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. Selecteer **Lokale Snapshot** of **Momentopname van de wolk**in het dialoogvenster **Back-up maken** en klik op **maken**. 

    ![Dialoogvenster voor reservekopieën maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. Vouw het knooppunt **taken** uit om te controleren of de back-up wordt uitgevoerd, en klik vervolgens op **uitvoeren**. De back-up moet worden vermeld.

5. Als u wilt weergeven in de voltooide momentopname, vouw het knooppunt van de **Catalogus van de back-up** , vouw de naam van het volume en klik vervolgens op **Lokale Snapshot** of **Momentopname van de wolk**. De back-up worden weergegeven als deze is voltooid. 

## <a name="edit-a-volume-group"></a>Een volume groep bewerken

Gebruik de volgende procedure voor het bewerken van een volume groep.

#### <a name="to-edit-a-volume-group"></a>Voor het bewerken van een volume groep

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Vouw het knooppunt **Groepen van Volume** in **het bereikvenster** , met de rechtermuisknop op de naam van een volume groep en klik vervolgens op **bewerken**. 

3. Het dialoogvenster **een groep volume maken **wordt weergegeven. U kunt de gegevens **, **toepassingen**en **Volumes** **wijzigen. 

4. Klik op **OK** om uw wijzigingen te slaan.

## <a name="delete-a-volume-group"></a>Een volume groep verwijderen

Gebruik de volgende procedure voor het verwijderen van een volume groep. 

>[AZURE.WARNING] Hiermee verwijdert u ook alle back-ups die zijn gekoppeld aan de groep van het volume.

#### <a name="to-delete-a-volume-group"></a>Een volume groep verwijderen

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. Vouw het knooppunt **Groepen van Volume** in **het bereikvenster** met de rechtermuisknop op de naam van een volume groep en klik vervolgens op **verwijderen**. 

3. Het dialoogvenster **Groep van Volume verwijderen** wordt weergegeven. **Bevestigen** typt in het tekstvak en klik vervolgens op **OK**. 

    Het verwijderde volume groep verdwijnt uit de lijst in het deelvenster met **resultaten** en alle back-ups die gekoppeld aan die groep volume zijn worden verwijderd uit de back-catalogus.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple Snapshot Manager voor het beheren van uw oplossing StorSimple](storsimple-snapshot-manager-admin.md).
- Meer informatie over het [gebruik van Snapshot Manager StorSimple maken en beheren van back-beleid](storsimple-snapshot-manager-manage-backup-policies.md).
