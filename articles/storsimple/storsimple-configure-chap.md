<properties 
   pageTitle="CHAP configureren voor uw apparaat StorSimple | Microsoft Azure"
   description="Beschrijving van het Challenge Handshake Authentication Protocol (CHAP) op een StorSimple apparaat configureren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>CHAP configureren voor uw apparaat StorSimple

In deze zelfstudie wordt uitgelegd hoe u CHAP configureren voor uw apparaat StorSimple. De in dit artikel beschreven procedure is van toepassing op StorSimple 8000-serie, alsmede StorSimple 1200-apparaten.

CHAP staat voor Challenge Handshake Authentication Protocol. Het is een verificatiemethode waarmee servers valideren de identiteit van de RAS-clients. De controle is gebaseerd op een gedeelde wachtwoord of geheim. CHAP is eenrichtingsverkeer (Unidirectioneel) of onderlinge (bidirectioneel). Unilaterale CHAP is wanneer het doel een initiator verifieert. Wederzijdse of omgekeerde CHAP, aan de andere kant vereist dat het doel de initiator verifiëren en vervolgens de initiator het doel te verifiëren. Initiator verificatie kan worden uitgevoerd zonder doel verificatie. Doel-verificatie kan echter alleen als initiator-verificatie ook geïmplementeerd worden geïmplementeerd. 

Beste, is het raadzaam u CHAP gebruikt voor het verbeteren van de iSCSI-beveiliging.

>[AZURE.NOTE] Houd er rekening mee dat IPSEC wordt momenteel niet ondersteund op apparaten StorSimple.

Het CHAP-instellingen op de StorSimple-apparaat kunnen op de volgende manieren worden geconfigureerd:

- Unidirectioneel of eenrichtings-verificatie

- Bidirectionele of reverse- of wederzijdse verificatie

In elk van deze gevallen moet de portal voor het apparaat en de iSCSI-initiatorsoftware van server worden geconfigureerd. De gedetailleerde stappen voor deze configuratie worden in de volgende zelfstudie beschreven.

## <a name="unidirectional-or-one-way-authentication"></a>Unidirectioneel of eenrichtings-verificatie

Unidirectioneel verificatie verifieert het doel de initiator. Deze verificatie is vereist dat u de CHAP initiator-instellingen op de StorSimple-apparaat en de iSCSI-initiatorsoftware op de host configureren. De gedetailleerde procedures voor het StorSimple apparaat en de Windows-host worden nu beschreven.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Het apparaat voor eenrichtingsverificatie configureren

1. Klik op het tabblad **configureren** in de Azure klassieke portal op de pagina **apparaten** .

    ![CHAP Initiator](./media/storsimple-configure-chap/IC740943.png)

2. Schuif naar beneden op deze pagina en klik in de sectie **CHAP Initiator** :
                                                    
    1. Geef een gebruikersnaam voor uw CHAP initiator.

    2. Een wachtwoord opgeven voor de CHAP initiator.

         > [AZURE.IMPORTANT] De CHAP-gebruikersnaam moet minder dan 233 tekens bevatten. Het CHAP-wachtwoord moet tussen 12 en 16 tekens bevatten. Een langere gebruikersnaam of wachtwoord leidt tot een verificatiefout op de Windows-host.
    
    3. Bevestig het wachtwoord.

4. Klik op **Opslaan**. Een bevestigingsbericht verschijnt. Klik op **OK** om de wijzigingen opslaan.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Eenrichtingsverificatie configureren op de Windows-host-server

1. Start de iSCSI-Initiator op de host-server van Windows.

2. In het venster **iSCSI Initiator-eigenschappen** kunt u de volgende stappen uitvoeren:
                                                    
    1. Klik op het tabblad **detectie** .

        ![iSCSI-initiator eigenschappen](./media/storsimple-configure-chap/IC740944.png)

    2. Klik op de **Portal te ontdekken**.

3. Klik in het dialoogvenster **Doelportal ontdekken** :
                                                    
    1. Geef het IP-adres van het apparaat.

    3. Klik op **Geavanceerd**.

        ![Ontdek de doelportal](./media/storsimple-configure-chap/IC740945.png)

4. Klik in het dialoogvenster **Geavanceerde instellingen** :
                                                    
    1. Schakel het selectievakje **aanmelden CHAP inschakelen** in.

    2. Geef de gebruikersnaam die u hebt opgegeven voor de CHAP Initiator in de klassieke portal in het veld **naam** .

    3. In het veld **Doelgeheim** wachtwoord die u hebt opgegeven voor de CHAP Initiator in de klassieke portal opgeven.

    4. Klik op **OK**.

        ![Geavanceerde instellingen-algemeen](./media/storsimple-configure-chap/IC740946.png)

5. Status van het apparaat moet op het tabblad **doelen** van de **iSCSI-Initiator eigenschappen** -venster weergegeven als **verbonden**. Als u een apparaat StorSimple 1200 wordt vervolgens elk volume gekoppeld als een iSCSI-doel zoals hieronder wordt weergegeven. Stap 3 en 4 moet derhalve worden herhaald voor elk volume.

    ![Volumes gekoppeld als afzonderlijke doelen](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Als u de iSCSI-naam wijzigt, wordt de nieuwe naam voor de nieuwe iSCSI-sessies worden gebruikt. Nieuwe instellingen niet worden gebruikt voor bestaande sessies totdat u zich afmeldt en aanmeldt opnieuw.

Ga naar [aanvullende overwegingen](#additional-considerations)voor meer informatie over het configureren van CHAP op de Windows-host-server.


## <a name="bidirectional-or-mutual-authentication"></a>Bidirectionele of onderlinge verificatie

Het doel verifieert de initiator in bidirectionele verificatie, en vervolgens de initiator het doel verifieert. Hiervoor moet de gebruiker de instellingen van CHAP initiator, als de omgekeerde CHAP-instellingen configureren op het apparaat en de iSCSI-initiatorsoftware op de host. De volgende procedures worden de stappen beschreven om wederzijdse verificatie te configureren op het apparaat en op de Windows-host.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Het apparaat voor wederzijdse verificatie configureren

1. Klik op het tabblad **configureren** in de Azure klassieke portal op de pagina **apparaten** .

    ![CHAP target](./media/storsimple-configure-chap/IC740948.png)

2. Schuif naar beneden op deze pagina en klik in de sectie **CHAP Target** :
                                                    
    1. Geef een **omgekeerde CHAP-gebruikersnaam** voor uw apparaat.

    2. Een **omgekeerde CHAP wachtwoord** voor het apparaat opgeven.

    3. Bevestig het wachtwoord.

3. In de sectie **CHAP Initiator** :
                                                
    1. Geef een **gebruikersnaam** voor uw apparaat.

    1. Een **wachtwoord** opgeven voor het apparaat.

    3. Bevestig het wachtwoord.

4. Klik op **Opslaan**. Een bevestigingsbericht verschijnt. Klik op **OK** om de wijzigingen opslaan.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Bidirectionele verificatie op de Windows-host-server configureren

1. Start de iSCSI-Initiator op de host-server van Windows.

2. Klik op het tabblad **configuratie** in het venster **iSCSI Initiator-eigenschappen** .

3. Klik op **CHAP**.

4. Klik in het dialoogvenster **iSCSI-Initiator wederzijdse CHAP-geheim** :
                                                    
    1. Typ het **Omgekeerde CHAP wachtwoord** die u hebt geconfigureerd in de klassieke Azure portal.

    2. Klik op **OK**.

        ![iSCSI-initiator wederzijdse CHAP-geheim](./media/storsimple-configure-chap/IC740949.png)

5. Klik op het tabblad **doelen** .

6. Klik op de knop **verbinden** . 

7. Klik op **Geavanceerd**in het dialoogvenster **Verbinding maken naar doel** .

8. Klik in het dialoogvenster **Geavanceerde eigenschappen** :
                                                    
    1. Schakel het selectievakje **aanmelden CHAP inschakelen** in.

    2. Geef de gebruikersnaam die u hebt opgegeven voor de CHAP Initiator in de klassieke portal in het veld **naam** .

    3. In het veld **Doelgeheim** wachtwoord die u hebt opgegeven voor de CHAP Initiator in de klassieke portal opgeven.

    4. Schakel het selectievakje **wederzijdse verificatie uitvoeren** .

        ![Geavanceerde instellingen voor wederzijdse verificatie](./media/storsimple-configure-chap/IC740950.png)

    5. Klik op **OK** om de configuratie van CHAP te voltooien
     
Ga naar [aanvullende overwegingen](#additional-considerations)voor meer informatie over het configureren van CHAP op de Windows-host-server.

## <a name="additional-considerations"></a>Aanvullende overwegingen

De functie **Snelle verbinding** biedt geen ondersteuning voor verbindingen waarvoor CHAP is ingeschakeld. Als CHAP is ingeschakeld, moet u ook de knop **Connect** op het tabblad **doelen** verbinding maken met een doel.

![Verbinding maken met het doel](./media/storsimple-configure-chap/IC740947.png)

Schakel het selectievakje **toevoegen aan de lijst met favoriete doelen** in het dialoogvenster **verbinding maken met het doel** dat wordt aangeboden. Dit zorgt ervoor dat elke keer dat de computer opnieuw is opgestart, wordt geprobeerd de verbinding te herstellen met de favoriete iSCSI-doelen.

## <a name="errors-during-configuration"></a>Fouten tijdens de configuratie

Als de CHAP-configuratie onjuist is, vervolgens bent u waarschijnlijk een foutbericht voor de **verificatie mislukt** .

## <a name="verification-of-chap-configuration"></a>Controle van de CHAP-configuratie

U kunt controleren of CHAP wordt gebruikt door de volgende stappen uit te voeren.

#### <a name="to-verify-your-chap-configuration"></a>Het CHAP-configuratie controleren

1. Klik op **favoriete doelen**.

2. Selecteer het doel waarvoor u verificatie ingeschakeld.

3. Klik op **Details**.

    ![iSCSI-initiator eigenschappen favoriete doelen](./media/storsimple-configure-chap/IC740951.png)

4. Noteer de vermelding in het veld **verificatie** in het dialoogvenster **Details van favoriete** . Als de configuratie voltooid is, moet er **CHAP**.

    ![Details van favoriete](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [beveiliging van StorSimple](storsimple-security.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
