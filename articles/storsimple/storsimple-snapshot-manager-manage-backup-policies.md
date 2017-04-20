<properties 
   pageTitle="Back-up beleid StorSimple Snapshot Manager | Microsoft Azure"
   description="Beschrijft hoe u de module MMC StorSimple Snapshot Manager maken en de back-up beleid waarmee de geplande back-ups beheren."
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
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>StorSimple Snapshot Manager gebruiken voor het maken en beheren van back-beleid

## <a name="overview"></a>Overzicht

Een back-up beleid maakt een schema voor de back-ups van gegevens op volume lokaal of in de cloud. Wanneer u een back-up beleid maakt, kunt u ook een bewaarbeleid. (U kunt maximaal 64 momentopnamen behouden.) Zie voor meer informatie over back-beleid [typen back-up](storsimple-what-is-snapshot-manager.md#backup-type) van [StorSimple 8000-serie: een hybride cloud-oplossing](storsimple-overview.md).

In deze zelfstudie wordt uitgelegd hoe u kunt:

- Maak een back-up beleid 
- Een back-up beleid bewerken 
- Een back-beleid verwijderen 

## <a name="create-a-backup-policy"></a>Maak een back-up beleid

Gebruik de volgende procedure voor het maken van een nieuwe back-beleid.

#### <a name="to-create-a-backup-policy"></a>Een back-up beleid maken

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. In **het bereikvenster** met de rechtermuisknop op het **Beleid van de back-up**en klik op **Back-up beleid maken**.

    ![Maak een back-up beleid](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    In het dialoogvenster **een beleid maken** wordt weergegeven. 

    ![Maak een beleid - tabblad Algemeen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Vul op het tabblad **Algemeen** de volgende gegevens:

   1. Typ in het tekstvak **naam** een naam voor het beleid.

   2. Typ in het tekstvak **Volume groep** de naam van de groep van het volume dat is gekoppeld aan het beleid.

   3. Selecteer **lokale Snapshot** of **momentopname van de wolk**.

   4. Selecteer het aantal momentopnamen te behouden. Als u **alle**selecteert, worden 64 snapshots (maximaal) behouden. 

4. Klik op het tabblad **planning** .

    ![Maak een beleid - tabblad Planning](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Vul de volgende gegevens op het tabblad **planning** : 

   1. Klik op het selectievakje **inschakelen** als u de volgende back-up plannen.

   2. Schakel onder **Instellingen** **eenmalig**, **dagelijks**, **Wekelijks**of **maandelijks**. 

   3. Klik op het pictogram van de kalender en selecteer een begindatum in het tekstvak **Start** .

   4. Klik onder **Geavanceerde instellingen**kunt u optionele herhalen planningen en een einddatum instellen.

   5. Klik op **OK**.

Nadat u een back-up beleid hebt gemaakt, wordt de volgende informatie weergegeven in het deelvenster **resultaten** :

- **Naam** : de naam van de back-beleid.

- **Type** : lokale snapshot of momentopname van de wolk.

- **Volume-groep** : de volume-groep die is gekoppeld aan het beleid.

- **Bewaren** : het aantal momentopnamen behouden; het maximum is 64.

- **Gemaakt** : de datum waarop dit beleid is gemaakt.

- **Ingeschakeld** : of het beleid is actief: **de waarde True** geeft aan dat deze van kracht; **False** geeft aan dat het niet van kracht is. 

## <a name="edit-a-backup-policy"></a>Een back-up beleid bewerken

Gebruik de volgende procedure voor het bewerken van een bestaand back-beleid.

#### <a name="to-edit-a-backup-policy"></a>Een back-up beleid bewerken

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. Klik op het knooppunt **Back-up beleid** in **het bereikvenster** . De back-up beleid weergegeven in het deelvenster **resultaten** . 

3. Klik met de rechtermuisknop op het beleid dat u wilt bewerken en klik vervolgens op **bewerken**. 

    ![Een back-up beleid bewerken](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Wanneer u **een beleid maken** verschijnt, typt u de wijzigingen en klik op **OK**. 

## <a name="delete-a-backup-policy"></a>Een back-beleid verwijderen

Gebruik de volgende procedure als u een back-up beleid verwijderen.

#### <a name="to-delete-a-backup-policy"></a>Een back-beleid verwijderen

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. Klik op het knooppunt **Back-up beleid** in **het bereikvenster** . De back-up beleid weergegeven in het deelvenster **resultaten** . 

3. Klik met de rechtermuisknop op het back-beleid dat u wilt verwijderen en klik vervolgens op **verwijderen**.

4. Klik op **Ja**in het bevestigingsbericht.

    ![Bevestiging van de back-beleid verwijderen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple Snapshot Manager voor het beheren van uw oplossing StorSimple](storsimple-snapshot-manager-admin.md).
- Meer informatie over het [gebruik van Snapshot Manager StorSimple weergeven en beheren van back-uptaken](storsimple-snapshot-manager-manage-backup-jobs.md).
