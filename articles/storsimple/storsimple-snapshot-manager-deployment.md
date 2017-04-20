<properties 
   pageTitle="Implementeren van StorSimple Snapshot Manager | Microsoft Azure"
   description="Informatie over het downloaden en installeren van de StorSimple Snapshot Manager, een MMC-module voor het beheren van beveiliging en back-up functies StorSimple."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>De StorSimple momentopname MMC-module beheer implementeren

## <a name="overview"></a>Overzicht

De StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module die de bescherming van gegevens en back-up beheren in een omgeving met Microsoft Azure StorSimple vereenvoudigt. Met StorSimple Snapshot Manager beheren Microsoft Azure StorSimple op gebouwen en cloud opslag alsof het een volledig geïntegreerde opslagsysteem, waardoor de back-up en terugzetten van processen te vereenvoudigen en kosten te verlagen. 

In deze zelfstudie wordt beschreven configuratievereisten, alsmede procedures voor het installeren, verwijderen en bijwerken StorSimple Snapshot Manager.

>[AZURE.NOTE] 
>
>- U kunt StorSimple Snapshot Manager niet gebruiken voor het beheren van Microsoft Azure StorSimple virtuele matrices (ook bekend als StorSimple op-premises virtuele apparaten).
>
>- Als u van plan bent om StorSimple 2 hebt geïnstalleerd op uw apparaat StorSimple, moet u de meest recente versie van StorSimple Snapshot Manager te downloaden en installeren **voordat u StorSimple 2 hebt geïnstalleerd**. De meest recente versie van StorSimple Snapshot Manager is achterwaarts compatibel en werkt met alle versies van Microsoft Azure StorSimple. Als u de vorige versie van StorSimple Snapshot Manager gebruikt, moet u te werken (u hoeft niet naar de vorige versie verwijderen voordat u de nieuwe versie installeert).

## <a name="storsimple-snapshot-manager-installation"></a>Installatie StorSimple Snapshot Manager

StorSimple Snapshot Manager kan worden geïnstalleerd op computers waarop het Windows Server 2008 R2 SP1, Windows Server 2012 R2-besturingssysteem of Windows Server 2012. Op servers waarop Windows 2008 R2 wordt uitgevoerd, moet u ook Windows Server 2008 SP1 en Windows Management Framework 3.0 installeren. 

Voordat het installeren of bijwerken van de StorSimple Snapshot Manager-module voor de Microsoft Management Console (MMC), ervoor zorgen dat de Microsoft Azure StorSimple apparaat en host-server correct zijn geconfigureerd. 

## <a name="configure-prerequisites"></a>Vereisten configureren

De volgende stappen geven een overzicht van configuratietaken die u uitvoeren moet voordat u de StorSimple Snapshot Manager installeren. Zie [uw apparaat op gebouwen StorSimple implementeren](storsimple-deployment-walkthrough.md)voor volledige Microsoft Azure StorSimple configuratie en setup-informatie, met inbegrip van de systeemvereisten en stapsgewijze instructies.

>[AZURE.IMPORTANT]Voordat u begint, bekijk de [implementatie, controlelijst](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) en en [vereisten voor implementatie](storsimple-deployment-walkthrough.md#deployment-prerequisites) in [uw apparaat op gebouwen StorSimple implementeren](storsimple-deployment-walkthrough.md).
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>Voordat u StorSimple Snapshot Manager installeren

1. Uitpakken, koppelen en sluit u het Microsoft Azure StorSimple apparaat zoals beschreven in de [installatie van het apparaat StorSimple 8100](storsimple-8100-hardware-installation.md) of [uw apparaat StorSimple 8600](storsimple-8600-hardware-installation.md).

2. Zorg ervoor dat de hostcomputer een van de volgende besturingssystemen wordt uitgevoerd:

    - Windows Server 2008 R2 (op servers waarop Windows 2008 R2 wordt uitgevoerd, moet u ook installeren SP1 voor Windows Server 2008 en Windows Management Framework 3.0)
    - Windows Server 2012
    - Windows Server 2012 R2
 
    Een virtueel apparaat StorSimple moet de host een virtuele Machine van Microsoft Azure. 

3. Zorg ervoor dat u voldoet aan de vereisten voor de configuratie van Microsoft Azure StorSimple. Voor meer informatie gaat u naar de [vereisten voor implementatie](storsimple-deployment-walkthrough.md#deployment-prerequisites).

4. Het apparaat aansluiten op de host en de eerste configuratie uitvoeren. Voor meer informatie gaat u naar de [stappen voor implementatie](storsimple-deployment-walkthrough.md#deployment-steps).

5. Volumes maken op het apparaat en wijst deze toe aan de host controleren dat de host te koppelen en deze gebruiken. StorSimple Snapshot Manager ondersteunt de volgende soorten volumes: 

    - Standaardvolumes
    - Eenvoudige volumes
    - Dynamische volumes
    - Mirrored dynamische volumes (RAID 1)
    - Cluster gedeelde clustervolumes
 
    Raadpleeg voor meer informatie over het maken van volumes op de StorSimple-apparaat of een virtueel apparaat StorSimple [stap 6: maken van een volume](storsimple-deployment-walkthrough.md#step-6-create-a-volume), in [uw apparaat op gebouwen StorSimple implementeren](storsimple-deployment-walkthrough.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Een nieuwe StorSimple Snapshot Manager installeren

Voordat u StorSimple Snapshot Manager installeert, controleren of de volumes u gemaakt op het apparaat StorSimple of StorSimple virtueel apparaat zijn gekoppeld, geïnitialiseerd en geformatteerd zoals beschreven in [vereisten configureren](#configure-prerequisites).

>[AZURE.IMPORTANT]
>
>- Een virtueel apparaat StorSimple moet de host een virtuele Machine van Microsoft Azure. 
>
>- De host moet Windows 2008 R2, Windows Server 2012 of Windows Server 2012 R2 worden uitgevoerd. Als uw server Windows Server 2008 R2 wordt uitgevoerd, moet u ook Windows Server 2008 SP1 en Windows Management Framework 3.0 installeren.
>
>- U moet een iSCSI-verbinding van de host naar het apparaat StorSimple configureren voordat u het apparaat StorSimple Snapshot Manager kunt verbinden.

Volg deze stappen om een nieuwe installatie van StorSimple Snapshot Manager. Als u een upgrade installeert, gaat u naar [bijwerken of opnieuw installeren van StorSimple Snapshot Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Stap 1: StorSimple Snapshot Manager installeren 
- Stap 2: Verbinding StorSimple Snapshot Manager met een apparaat 
- Stap 3: Controleer of de verbinding met het apparaat 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Stap 1: StorSimple Snapshot Manager installeren

Gebruik de volgende stappen StorSimple Snapshot Manager te installeren.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager installeren

1. (Ga naar [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) in het Microsoft Download Center) van de StorSimple Snapshot Manager-software downloaden en lokaal opslaan op de host.

2. In File Explorer, klik met de rechtermuisknop op de gecomprimeerde map en klik vervolgens op **Alles uitpakken**.

3. Typ in het venster **mappen uitpakken van gecomprimeerde (Zipped)** in het vak **Selecteer een bestemming en uitpakken van bestanden** of blader naar het pad waar u het bestand wilt moet worden geëxtraheerd. 

       >[AZURE.IMPORTANT] U moet de StorSimple Snapshot Manager installeren op station C:.
 
4. Schakel het selectievakje **uitgepakte bestanden als voltooid weergeven** in en klik op **uitpakken**.

    ![Het dialoogvenster bestanden uitpakken](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. Wanneer het uitpakken is voltooid, opent u de doelmap. Dubbelklik op het pictogram van de toepassing setup dat wordt weergegeven in de doelmap.

5. Wanneer het bericht **Installatie voltooid** verschijnt, klikt u op **sluiten**. U ziet het pictogram StorSimple Snapshot Manager op uw bureaublad.

    ![pictogram op het bureaublad](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Stap 2: Verbinding StorSimple Snapshot Manager met een apparaat

Gebruik de volgende stappen StorSimple Snapshot Manager met een StorSimple apparaat verbinding te maken.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>StorSimple Snapshot Manager verbinding kunnen maken met een apparaat

1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. De StorSimple Snapshot Manager venster verschijnt. Het venster bevat een **Scope** -deelvenster, deelvenster met **resultaten** en een deelvenster met **Acties** . 

    ![De gebruikersinterface StorSimple Snapshot Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - Het deelvenster **bereik** (het linkerdeelvenster) bevat een lijst met knooppunten georganiseerd in een boomstructuur. U kunt sommige knooppunten in een weergave of specifieke gegevens in verband met dat knooppunt uitvouwen. Klik op het pijlpictogram wilt uitvouwen of samenvouwen van een knooppunt. Met de rechtermuisknop op een item in het deelvenster **bereik** voor een overzicht van de beschikbare acties voor dat item. 

    - **Het resultatenvenster (middenvenster)** bevat gedetailleerde informatie over het knooppunt, de weergave of de gegevens die u hebt geselecteerd in **het bereikvenster** .

    - Het deelvenster **Acties** worden de bewerkingen vermeld die u kunt uitvoeren op knooppunt, weergave of door de gegevens die u hebt geselecteerd in **het bereikvenster** .

    Zie voor een volledige beschrijving van de gebruikersinterface StorSimple Snapshot Manager [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).

2. Klik met de rechtermuisknop op het knooppunt **apparaten** in het deelvenster **bereik** en klik vervolgens op **een apparaat configureren**. Het dialoogvenster voor **een apparaat configureren** wordt weergegeven.

    ![Een apparaat configureren](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. Selecteer het IP-adres van de Microsoft Azure StorSimple apparaat of virtueel apparaat in de lijst **apparaat** . Typ in het vak **wachtwoord** het wachtwoord StorSimple Snapshot Manager die u hebt gemaakt voor het apparaat in de klassieke Azure portal. Klik op **OK**.

4. StorSimple Snapshot Manager gezocht voor het apparaat dat u hebt bepaald. Als het apparaat beschikbaar is, voegt StorSimple Snapshot Manager een verbinding. U kunt [controleren of de verbinding met het apparaat](#to-verify-the-connection) om te bevestigen dat de verbinding is toegevoegd.

    Als het apparaat voor een of andere reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. Klik op **OK** om het foutbericht te sluiten en klik op **Annuleren** om het dialoogvenster **een apparaat configureren** te sluiten.

5. Wanneer deze verbinding met een apparaat maakt, importeert StorSimple Snapshot Manager elke groep volume is geconfigureerd voor het apparaat, op voorwaarde dat het volume groep Back-ups is gekoppeld. Volume groepen waarvoor geen bijbehorende back-ups worden niet geïmporteerd. Bovendien worden back-beleidsregels die zijn gemaakt voor een groep van het volume niet geïmporteerd. Overzicht van de geïmporteerde groepen met de rechtermuisknop op het **Volume groepen** bovenste knooppunt in **het bereikvenster** en klikt u op **in-/ uitschakelen groepen geïmporteerd**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Stap 3: Controleer of de verbinding met het apparaat

Gebruik de volgende stappen uit om te controleren of de StorSimple Snapshot Manager is aangesloten op het apparaat StorSimple.

#### <a name="to-verify-the-connection"></a>Om te controleren of de verbinding

1. Klik op het knooppunt **apparaten** in het deelvenster **bereik** .

    ![Apparaatstatus StorSimple Snapshot Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Controleer in het deelvenster met **resultaten** : 

   - Het apparaat is aangesloten als een groene indicator weergegeven op het pictogram **beschikbaar** wordt weergegeven in de kolom **Status** . 

   - Als u op het pictogram een rode indicator wordt weergegeven en niet beschikbaar weergegeven in de kolom **Status** , vervolgens het apparaat is niet aangesloten. 

   - Als het **vernieuwen** wordt weergegeven in de kolom **Status** , klik ophalen StorSimple Snapshot Manager volume groepen en de bijbehorende back-ups voor een aangesloten apparaat.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Upgraden of installeren StorSimple Snapshot Manager

U moet volledig StorSimple Snapshot Manager verwijderen voordat u een upgrade uitvoert of installeer de software opnieuw. 

Voordat u opnieuw installeert StorSimple Snapshot Manager, back-up van de bestaande StorSimple Snapshot Manager-database op de hostcomputer. Dit bespaart de back-up beleid en configuratie-informatie, zodat u deze gegevens gemakkelijk vanaf de back-up terugzetten kunt.

Als volgt te werk als u een upgrade uitvoert of StorSimple Snapshot Manager opnieuw te installeren:

- Stap 1: StorSimple Snapshot Manager verwijderen 
- Stap 2: Back-up van de database StorSimple Snapshot Manager 
- Stap 3: Installeer StorSimple Snapshot Manager en de database terugzetten 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Stap 1: StorSimple Snapshot Manager verwijderen

Gebruik de volgende stappen uit om StorSimple Snapshot Manager te verwijderen.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager verwijderen

1. Op de hostcomputer, opent u het **Configuratiescherm**, klik op **programma's**en klik op **programma's en onderdelen**.

2. Klik in het linkerdeelvenster op **een programma verwijderen of wijzigen**.

3. **StorSimple Snapshot Manager**met de rechtermuisknop en klik vervolgens op **verwijderen**.

4. Hiermee start u het programma Setup StorSimple Snapshot Manager. Klik op **Instellingen wijzigen**en klik vervolgens op **verwijderen**.

    >[AZURE.NOTE] Als er een MMC-processen worden uitgevoerd op de achtergrond, zoals StorSimple Snapshot Manager of Schijfbeheer, verwijderen van de installatie mislukt en ontvangt u een bericht naar alle exemplaren van MMC sluiten voordat u probeert om het programma te verwijderen. Selecteer **toepassingen automatisch sluiten en opnieuw openen nadat setup voltooid is**en klik vervolgens op **OK**.
 
5. Als het verwijderingsproces is voltooid, verschijnt een bericht is **Setup voltooid** . Klik op **sluiten**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Stap 2: Back-up van de database StorSimple Snapshot Manager

Gebruik de volgende stappen voor het maken en opslaan van een kopie van de StorSimple Snapshot Manager-database.

#### <a name="to-back-up-the-database"></a>Back-up van de database

1. Stop de Service Microsoft StorSimple Management:

   1. Start Server Manager.

   2. Selecteer de **Services**op het Dashboard Serverbeheer in het menu **Extra** .

   3. Selecteer op de pagina **Services** **Microsoft StorSimple Management-Service**.

   4. In het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, klikt u op **de service stoppen**.

        ![De StorSimple Manager-service stoppen](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Ga naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData is een verborgen map.

3. Het XML-catalogusbestand zoeken, het bestand kopiëren en de kopie opslaan op een veilige plaats of in de cloud.

    ![StorSimple back-upcatalogus bestand](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Start de Service Microsoft StorSimple Management: 

    1. Selecteer de **Services**op het Dashboard Serverbeheer in het menu **Extra** .

    2. Selecteer op de pagina **Services** de **StorSimple Management Servic**e.

    3. In het rechterdeelvenster, onder **Microsoft StorSimple Management-Service**, klikt u op **de service opnieuw starten**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Stap 3: Installeer StorSimple Snapshot Manager en de database terugzetten

Als u wilt StorSimple Snapshot Manager, de stappen in [een nieuwe StorSimple Snapshot Manager installeren](#install-a-new-storsimple-snapshot-manager). Vervolgens gebruikt u de volgende procedure de StorSimple Snapshot Manager-database terugzetten.

#### <a name="to-restore-the-database"></a>De database terugzetten

1. Stop de Service Microsoft StorSimple Management:

    1. Start Server Manager.

    2. Selecteer de **Services**op het Dashboard Serverbeheer in het menu **Extra** .

    3. Selecteer op de pagina **Services** **Microsoft StorSimple Management-Service**.

    4. In het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, klikt u op **de service stoppen**.

2. Ga naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] ProgramData is een verborgen map.

3. Het XML-catalogusbestand verwijderen en vervangen door de versie die u eerder hebt opgeslagen.

4. Start de Service Microsoft StorSimple Management: 

    1. Selecteer de **Services**op het Dashboard Serverbeheer in het menu **Extra** .

    2. Selecteer op de pagina **Services** **Microsoft StorSimple Management-Service**.

    3. In het rechterdeelvenster, onder **Microsoft StorSimple Management-Service**, klikt u op **de service opnieuw starten**.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over StorSimple Snapshot Manager, gaat u naar [Wat is StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md).

- Voor meer informatie over de gebruikersinterface StorSimple Snapshot Manager, gaat u naar [de gebruikersinterface StorSimple Snapshot Manager](storsimple-use-snapshot-manager.md).

- Voor meer informatie over het gebruik van StorSimple Snapshot Manager, gaat u naar de [Manager voor StorSimple momentopname gebruiken voor het beheren van uw oplossing StorSimple](storsimple-snapshot-manager-admin.md).
