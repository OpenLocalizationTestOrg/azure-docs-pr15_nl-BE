<properties 
   pageTitle="Beheren van apparaten met StorSimple Snapshot Manager | Microsoft Azure"
   description="Beschrijft hoe u de module MMC StorSimple Snapshot Manager verbinding maken en beheren van apparaten in StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>StorSimple Snapshot Manager gebruiken om te verbinden en StorSimple apparaten beheren

## <a name="overview"></a>Overzicht

U kunt knooppunten in het deelvenster StorSimple Snapshot Manager **Scope** geïmporteerde StorSimple apparaatgegevens verifiëren en vernieuwen van aangesloten opslagapparaten. Wanneer u klikt op het knooppunt **apparaten** , kunt u bovendien een lijst met aangesloten apparaten en de bijbehorende statusinformatie in het deelvenster **resultaten** zien.

![Aangesloten apparaten](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Figuur 1: StorSimple Snapshot Manager verbonden apparaat** 

Afhankelijk van uw selectie **weergeven** ziet **het resultatenvenster** u de volgende informatie over elk apparaat. (Voor meer informatie over het configureren van een weergave, gaat u naar het [menu Beeld](storsimple-use-snapshot-manager.md#view-menu).


| Kolom  |Beschrijving          |
|:----------------|:--------------------| 
| Naam            | De naam van het apparaat zoals ingesteld in de klassieke Azure portal|
| Model           | Het modelnummer van het apparaat|
| Versie         | De versie van de software is geïnstalleerd op het apparaat |
| Status          | Of het apparaat beschikbaar is |
| Laatst gesynchroniseerd.     | Datum en tijd waarop het apparaat voor het laatst is gesynchroniseerd |
| Serienr.      | Het serienummer van het apparaat |
 
Als u met de rechtermuisknop op het knooppunt **apparaten** in het deelvenster **bereik** , kunt u selecteren uit de volgende acties:

- Toevoegen of vervangen van een apparaat 
- Een apparaat aansluit en invoer controleren 
- Aangesloten apparaten vernieuwen 

Als u op het knooppunt **apparaten** en klik vervolgens met de rechtermuisknop op de naam van een apparaat in het deelvenster **resultaten** , kunt u selecteren uit de volgende acties:

- Een apparaat verifiëren 
- Details van apparaat 
- Vernieuwen van een apparaat 
- De apparaatconfiguratie van een verwijderen 
- Het wachtwoord voor een apparaat wijzigen

>[AZURE.NOTE] Al deze acties zijn ook beschikbaar in het deelvenster **Acties** .
 
In deze zelfstudie wordt uitgelegd hoe u StorSimple Snapshot Manager gebruiken om te verbinden en het beheren van apparaten en de volgende taken uitvoeren:

- Toevoegen of vervangen van een apparaat 
- Een apparaat aansluit en invoer controleren 
- Aangesloten apparaten vernieuwen 
- Een apparaat verifiëren 
- Details van apparaat 
- Een afzonderlijk apparaat vernieuwen 
- De apparaatconfiguratie van een verwijderen 
- Een verlopen apparaatwachtwoord wijzigen
- Vervangen van een defecte apparaat

>[AZURE.NOTE] Ga voor algemene informatie over het gebruik van de interface StorSimple Snapshot Manager [StorSimple Snapshot Manager](storsimple-use-snapshot-manager.md)-gebruikersinterface.


## <a name="add-or-replace-a-device"></a>Toevoegen of vervangen van een apparaat

Gebruik de volgende procedure toe te voegen of te vervangen door een StorSimple apparaat.

#### <a name="to-add-or-replace-a-device"></a>Toe te voegen of te vervangen door een apparaat

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Klik met de rechtermuisknop op het knooppunt **apparaten** in het deelvenster **bereik** en klik vervolgens op **een apparaat configureren**. Het dialoogvenster voor **een apparaat configureren** wordt weergegeven.

    ![StorSimple-apparaat configureren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. Selecteer het IP-adres van het apparaat of een virtueel apparaat in de vervolgkeuzelijst **apparaat** . 

4. Typ in het vak **wachtwoord** het wachtwoord StorSimple Snapshot Manager die u hebt gemaakt voor het apparaat in de klassieke Azure portal. Klik op **OK**. StorSimple Snapshot Manager gezocht voor het apparaat dat u hebt bepaald. 

    - Als het apparaat beschikbaar is, voegt StorSimple Snapshot Manager een verbinding. 

    - Als het apparaat voor een of andere reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. Klik op **OK** om het foutbericht te sluiten en klik op **Annuleren** om het dialoogvenster **een apparaat configureren** te sluiten.

## <a name="connect-a-device-and-verify-imports"></a>Een apparaat aansluit en invoer controleren

Gebruik de volgende procedure een StorSimple apparaat aansluit en controleren of dat het bestaande volume groepen die back-ups hebt gekoppeld worden geïmporteerd.

#### <a name="to-connect-a-device-and-verify-imports"></a>Een apparaat aansluit en invoer controleren

1. Verbinding maken met een apparaat StorSimple Snapshot Manager, volg de instructies in het vak toevoegen of vervangen door een apparaat. Wanneer deze verbinding met een apparaat maakt, reageert StorSimple Snapshot Manager als volgt:

    - Als het apparaat voor een of andere reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. 

   - Als het apparaat beschikbaar is, voegt StorSimple Snapshot Manager een verbinding. Wanneer u het apparaat selecteert, wordt deze in het deelvenster met **resultaten weergegeven** en het statusveld geeft aan dat is het apparaat **beschikbaar**. StorSimple Snapshot Manager importeert geen groepen volume is geconfigureerd voor het apparaat, op voorwaarde dat het volume groepen back-ups hebt gekoppeld. Back-beleid worden niet geïmporteerd. Volume groepen waarvoor geen bijbehorende back-ups worden niet geïmporteerd.

2. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

3. Klik met de rechtermuisknop op het bovenste knooppunt in het deelvenster **bereik** en klik op **Weergave van de invoer in-of uitschakelen**.

    ![Weergave van de invoer selecteren in-/ uitschakelen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. Het dialoogvenster **In-/ uitschakelen invoer beeldscherm** wordt weergegeven met de status van de geïmporteerde volume groepen en back-ups. Klik op **OK**. 

Nadat de back-ups en het volume groepen geïmporteerd zijn, kunt u StorSimple Snapshot Manager te beheren, net zoals u zou volume groepen en back-ups die u hebt gemaakt en geconfigureerd met StorSimple Snapshot Manager beheren. 

## <a name="refresh-connected-devices"></a>Aangesloten apparaten vernieuwen

Gebruik de volgende procedure voor het synchroniseren van de aangesloten apparaten met StorSimple StorSimple Snapshot Manager.

####<a name="to-refresh-connected-devices"></a>Aangesloten apparaten vernieuwen

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. In **het bereikvenster** **apparaten**met de rechtermuisknop en klik vervolgens op **Apparaten vernieuwen**. Hiermee synchroniseert de aangesloten apparaten StorSimple Snapshot Manager zodat u kunt het volume groepen en back-ups, met inbegrip van alle recente toevoegingen. 

    ![De StorSimple apparaten vernieuwen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
De actie **Apparaten vernieuwen** haalt nieuwe volume groepen en eventuele bijbehorende back-ups van aangesloten apparaten. In tegenstelling tot de **volumes opnieuw** actie beschikbaar is voor het knooppunt **Volumes** , wordt de reservekopie van het register niet hersteld door **Apparaten vernieuwen** .

## <a name="authenticate-a-device"></a>Een apparaat verifiëren

Gebruik de volgende procedure voor het verifiëren van een apparaat StorSimple StorSimple Snapshot Manager.

#### <a name="to-authenticate-a-device"></a>Een apparaat te verifiëren

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Klik in het deelvenster **bereik** **apparaten**.

3. In **het resultatenvenster** met de rechtermuisknop op de naam van het apparaat en klik op **verifiëren**.

4. Het dialoogvenster **verifiëren** wordt weergegeven. Typ het apparaatwachtwoord en klik vervolgens op **OK**.

    ![In het dialoogvenster verificatie](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Details van apparaat

Gebruik de volgende procedure om de details van een apparaat StorSimple en, indien nodig, opnieuw synchroniseren StorSimple Snapshot Manager van het apparaat.

#### <a name="to-view-and-resynchronize-device-details"></a>Details weergeven en opnieuw synchroniseren apparaat

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten.

2. Klik in het deelvenster **bereik** **apparaten**.

3. In **het resultatenvenster** met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **Details**. 

4. verschijnt **Apparaat** het dialoogvenster. In dit vak worden de naam, model, versie, serienummer, status, iSCSI-doel gekwalificeerde naam (IQN) en laatste synchronisatiedatum en tijd. 

   - Klik op **synchroniseren** om de synchronisatie.

   - Klik op **OK** of op **Annuleren** om het dialoogvenster te sluiten.

    ![Apparaatdetails](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Een afzonderlijk apparaat vernieuwen

Gebruik de volgende procedure als u een afzonderlijk apparaat voor StorSimple StorSimple Snapshot Manager opnieuw synchroniseren.

#### <a name="to-refresh-a-device"></a>Voor het vernieuwen van een apparaat

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. Klik in het deelvenster **bereik** **apparaten**. 

3. In **het resultatenvenster** met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **Vernieuwen apparaat**. Dit StorSimple Snapshot Manager wordt het apparaat gesynchroniseerd. 

## <a name="delete-a-device-configuration"></a>De apparaatconfiguratie van een verwijderen

Gebruik de volgende procedure om een afzonderlijke configuratie StorSimple uit StorSimple Snapshot Manager verwijderen.

#### <a name="to-delete-a-device-configuration"></a>Configuratie van een apparaat verwijderen

1. Klik op het pictogram Bureaublad StorSimple Snapshot Manager starten. 

2. Klik in het deelvenster **bereik** **apparaten**. 

3. In **het resultatenvenster** met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **verwijderen**. 

4. Het volgende bericht wordt weergegeven. Klik op **Ja** om de configuratie te verwijderen of klik op **Nee** om de verwijdering te annuleren.

    ![Configuratie van apparaat verwijderen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Een verlopen apparaatwachtwoord wijzigen

U moet een wachtwoord voor het verifiëren van een apparaat StorSimple StorSimple Snapshot Manager. U kunt dit wachtwoord configureren wanneer u de Windows PowerShell-interface gebruiken om het apparaat in te stellen. Het wachtwoord kan echter verloopt. Als dit gebeurt, kunt u de klassieke Azure portal om het wachtwoord te wijzigen. Vervolgens, omdat het apparaat in StorSimple Snapshot Manager configureren is voordat het wachtwoord is verlopen, moet u het apparaat in StorSimple Snapshot Manager opnieuw worden geverifieerd. 

#### <a name="to-change-the-expired-password"></a>Het verlopen wachtwoord

1. In de klassieke Azure portal, de StorSimple Manager-service te starten.

2. Klik op **apparaten** > **configureren** voor het apparaat.

3. Ga naar de sectie StorSimple Snapshot Manager. Voer een wachtwoord van 14-15 tekens. Zorg ervoor dat het wachtwoord een combinatie van hoofdletters, kleine letters, cijfers en speciale tekens bevat.

4. Voer het wachtwoord ter bevestiging opnieuw op.

5. Klik op **Opslaan** onder aan de pagina.

#### <a name="to-re-authenticate-the-device"></a>Het apparaat opnieuw worden geverifieerd

1. StorSimple Snapshot Manager starten.

2. Klik in het deelvenster **bereik** **apparaten**. Er verschijnt een lijst met geconfigureerde apparaten in het deelvenster **resultaten** . 

3. Selecteer het apparaat, klik met de rechtermuisknop en klik vervolgens op **verifiëren**.

4. Voer het nieuwe wachtwoord in het venster **verifiëren** . 

5. Selecteer het apparaat, klik met de rechtermuisknop en selecteer **vernieuwen apparaat**. Dit StorSimple Snapshot Manager wordt het apparaat gesynchroniseerd. 

## <a name="replace-a-failed-device"></a>Vervangen van een defecte apparaat

Als een apparaat StorSimple mislukt en wordt vervangen door een apparaat op stand-by (failover), gebruikt u de volgende stappen verbinding maken met het nieuwe apparaat en de bijbehorende back-ups weergeven.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Verbinding maken met een nieuw apparaat na een failover

1. De iSCSI-verbinding om het nieuwe apparaat te configureren. Voor meer informatie, Ga naar ' stap 7: Mount, initialiseren en een volume-indeling ' in [uw apparaat op gebouwen StorSimple implementeren](storsimple-deployment-walkthrough-u2.md). 

>[AZURE.NOTE] Als de nieuwe StorSimple apparaat hetzelfde IP-adres als de oude heeft, kunt u mogelijk verbinding maken met de oude configuratie. 

2. Stop de Service Microsoft StorSimple Management:

    1. Start Server Manager.

    2. Selecteer de **Services**op het Dashboard Serverbeheer in het menu **Extra** . 

    3. Selecteer in het venster **Services** de **StorSimple Management-Service van Microsoft**. 

    4. In het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, klikt u op **de service stoppen**. 

3. De configuratie-informatie die betrekking hebben op het oude apparaat verwijderen: 

    1. In de Verkenner, Ga naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. Verwijder de bestanden in de map BACatalog. 

4. Start de Service Microsoft StorSimple Management: 

    1. Selecteer de **Services**op het Dashboard Serverbeheer in het menu **Extra** . 

    2. Selecteer in het venster **Services** de **StorSimple Management-Service van Microsoft**. 

    3. In het rechterdeelvenster, onder **Microsoft StorSimple Management-Service**, klikt u op **de service opnieuw starten**. 

5. StorSimple Snapshot Manager starten. 

6. De nieuwe StorSimple als apparaat wilt configureren, voert u uit stap 2: verbinding maken met een apparaat StorSimple in [StorSimple Snapshot Manager implementeert](storsimple-snapshot-manager-deployment.md). 

7. Klik met de rechtermuisknop op het bovenste knooppunt in het deelvenster **Scope** (Snapshot Manager StorSimple in het voorbeeld) en klik op **Weergave van de invoer in-of uitschakelen**. 

8. Er verschijnt een bericht als de geïmporteerde volume groepen en back-ups in StorSimple Snapshot Manager zijn. Klik op **OK**. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple Snapshot Manager voor het beheren van uw oplossing StorSimple](storsimple-snapshot-manager-admin.md).
- Meer informatie over het [gebruik van Snapshot Manager StorSimple weergeven en beheren van volumes](storsimple-snapshot-manager-manage-volumes.md).

