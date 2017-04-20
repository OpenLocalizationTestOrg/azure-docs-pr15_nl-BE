<properties 
   pageTitle="Virtuele matrix StorSimple iSCSI-server setup | Microsoft Azure"
   description="Beschrijving van het eerste setup uitvoeren en voltooien van setup voor apparaat uw StorSimple iSCSI-server registreren."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Implementeren van StorSimple virtuele matrix – het virtuele apparaat als een iSCSI-server instellen

![iSCSI-processtroom](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Overzicht

Deze zelfstudie implementatie is van toepassing op de algemene beschikbaarheid (GA) release voor Microsoft Azure StorSimple virtuele matrix (ook bekend als het virtuele apparaat voor StorSimple in ruimten of het virtuele apparaat StorSimple) uitgevoerd maart 2016. In deze zelfstudie wordt beschreven hoe u installatie uitvoeren, uw StorSimple iSCSI-server registreren, het apparaat instellen te voltooien en vervolgens maken, koppelen, initialiseren en formatteer de volumes op uw server StorSimple virtueel apparaat iSCSI. De StorSimple installatie-informatie in dit artikel alleen van toepassing op virtuele StorSimple-Arrays. 

De beschreven procedures duurt ongeveer 30 minuten hier op 1 uur te voltooien. De informatie in dit artikel alleen van toepassing op virtuele StorSimple-Arrays.

## <a name="setup-prerequisites"></a>Setup-vereisten

Voordat u configureren en u het virtuele apparaat StorSimple stelt, zorg ervoor dat:

- U hebt een virtueel apparaat ingericht en aangesloten zoals beschreven in het [Implementeren van StorSimple virtuele matrix - voorziening een virtuele matrix in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) of [Implementeren StorSimple virtuele matrix - bepaling een virtuele matrix in VMware](storsimple-ova-deploy2-provision-vmware.md).

- U hebt de sleutel voor de registratie van de StorSimple Manager-service die u hebt gemaakt voor het beheren van virtuele apparaten StorSimple. Zie voor meer informatie **stap 2: de registratie sleutel ophalen** in [Implementeren StorSimple virtuele matrix - voorbereiden van de portal](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

- Als dit de tweede of volgende virtueel apparaat dat u in een bestaande StorSimple Manager-service registreren wilt, moet u de sleutel data service hebben. Deze sleutel is gegenereerd nadat het eerste apparaat met deze service is geregistreerd. Als u deze sleutel hebt verloren, Zie **ophalen van de sleutel data service** gebruikt [De on line gebruikersinterface voor het beheer van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Stapsgewijze installatie 

Stapsgewijze instructies voor het instellen en configureren van het virtuele apparaat StorSimple gebruiken:

-  [Stap 1: Het lokale web UI-installatie is voltooid en het apparaat registreren](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Stap 2: Vul de vereiste apparaat instellen](#step-2-complete-the-required-device-setup)
-  [Stap 3: Een volume toevoegen](#step-3-add-a-volume)
-  [Stap 4: Koppel, initialiseren en formatteren van een volume](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Stap 1: Het lokale web UI-installatie is voltooid en het apparaat registreren 

#### <a name="to-complete-the-setup-and-register-the-device"></a>De installatie is voltooid en het apparaat registreren

1. Open een browservenster en verbinding maken met het web UI door te typen:

    `https://<ip-address of network interface>`

    Gebruik de verbindings-URL in de vorige stap hebt genoteerd. Er verschijnt een melding dat er een probleem met het beveiligingscertificaat van de website is fout. Klik op **Doorgaan naar deze webpagina**.

    ![Fout in beveiligingscertificaat](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Aanmelden bij de on line gebruikersinterface van het virtuele apparaat als **StorSimpleAdmin**. Geef het administrator-wachtwoord voor het apparaat dat u hebt gewijzigd in stap 3: het virtuele apparaat starten in [Implementeren StorSimple virtuele matrix - voorziening een virtueel apparaat in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) of [Implementeren StorSimple virtuele matrix - voorziening een virtueel apparaat in VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![-In pagina](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. U zal worden genomen op **de introductiepagina** . Deze pagina beschrijft de verschillende instellingen die zijn vereist voor het configureren en het virtuele apparaat registreren bij de StorSimple Manager-service. Houd er rekening mee dat de **instellingen van het netwerk**, **Web proxy-instellingen**en **tijdinstellingen** optioneel zijn. De enige vereiste instellingen zijn **Instellingen** en **instellingen van de wolk**.

    ![Startpagina](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. Klik op de pagina **netwerkinstellingen** onder **Netwerkinterfaces**0 gegevens automatisch geconfigureerd voor u. Elke netwerkinterface is standaard ingesteld om automatisch een IP-adres (DHCP). Daarom wordt een IP-adres, het subnetmasker en de gateway automatisch toegewezen (voor zowel IPv4 als IPv6).

    Als u van plan bent het apparaat implementeren als een iSCSI-server (blok opslagruimte creëren), wordt aangeraden dat u de optie **Get IP-adres automatisch** uitschakelen en configureren van statische IP-adressen.

    ![De pagina met netwerkinstellingen](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Als u meer dan één netwerkinterface toegevoegd tijdens het inrichten van het apparaat, kunt u deze hier. Opmerking dat u kunt de netwerkinterface alleen of als zowel IPv4 als IPv6 als IPv4 configureren. Alleen IPv6-configuraties worden niet ondersteund.

5. DNS-servers zijn vereist omdat ze worden gebruikt wanneer uw apparaat probeert te communiceren met uw cloud storage service-providers of uw apparaat oplossen door de naam als deze is geconfigureerd als bestandsserver. Op de pagina **netwerkinstellingen** onder de **DNS-servers**:

    1. Een primaire en secundaire DNS-server automatisch geconfigureerd. Als u statische IP-adressen configureren, kunt u DNS-servers. Voor hoge beschikbaarheid, raden wij u aan dat u een primaire en een secundaire DNS-server configureren.

    2. Klik op **toepassen**. Dit toepassen en valideren van de netwerkinstellingen.

6. Op de pagina **Instellingen voor** :

    1. Een unieke **naam** toewijzen aan het apparaat. Deze naam kan 1-15 tekens en letter, cijfers en koppelstreepjes kan bevatten.

    2. Klik op het pictogram van de **iSCSI-server** ![pictogram voor iSCSI-server](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) voor het **Type** apparaat dat u wilt maken. Een iSCSI-server, kunt u blokkeren om opslagruimte te creëren.

    3. Geef als u dit apparaat te worden van een domein behoren. Als uw apparaat een iSCSI-server is, is lid worden van het domein optioneel. Als u uw iSCSI-server niet toevoegen aan een domein, klikt u op **toepassen**, wacht totdat de instellingen worden toegepast en gaat u verder met de volgende stap.

        Als u het apparaat toevoegen aan een domein. Geef een **domeinnaam**op en klik vervolgens op **toepassen**.

        > [AZURE.NOTE] Als u uw iSCSI-server toevoegen aan een domein, zorg ervoor dat uw virtuele array in de eigen organisatie-eenheid (OU) voor Microsoft Azure Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast.

    5. Er wordt een dialoogvenster weergegeven. Voer uw referenties voor het domein in de opgegeven indeling. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Referenties voor het domein worden gecontroleerd. Er wordt een foutbericht weergegeven als de referenties onjuist zijn.

        ![referenties](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Klik op **toepassen**. Dit zal toepassen en valideren van de instellingen.
 
7. De web proxy-server (optioneel) configureren. Hoewel de web proxy-configuratie is optioneel, houd er rekening mee dat als u een webproxy gebruikt, u alleen deze hier configureren kunt worden.

    ![webproxy configureren](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    Op de pagina **Web-proxy** :

    1. De **Web proxy-URL** in de volgende notatie: *FDQN:Port-nummer*of *adres http://host-IP* . Opmerking de HTTPS-URL's worden niet ondersteund.

    2. **Verificatie** opgeven als **standaard** of **geen**.

    3. Als u verificatie gebruikt, moet u ook een **gebruikersnaam** en **wachtwoord**.

    4. Klik op **toepassen**. Zo valideren en past u de geconfigureerde web proxy-instellingen.
 
8. De instellingen voor uw apparaat, zoals de tijdzone en de primaire en secundaire NTP-servers (optioneel) configureren. NTP-servers zijn vereist omdat het apparaat tijd synchroniseren moet zodat deze met uw cloud-aanbieders verifiëren kan.

    ![Tijd instellen](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    Op de pagina **instellingen van tijd** :

    1. Selecteer in de vervolgkeuzelijst de **tijdzone** op basis van de geografische locatie waar het apparaat wordt ingezet. De standaardtijdzone voor uw apparaat is een PST-bestand. Het apparaat wordt deze tijdzone gebruiken voor alle geplande activiteiten.

    2. Geef een **primaire NTP-server** voor uw apparaat of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat uw netwerk NTP-verkeer staat voor het doorsturen van uw datacenter met het Internet.

    3. Geef desgewenst een **secundaire NTP-server** voor uw apparaat.

    4. Klik op **toepassen**. Dit zal valideren en de geconfigureerde instellingen toepassen.

9. Configureer de instellingen van de wolk voor uw apparaat. In deze stap maakt u voltooit de lokale configuratie en registreert u het apparaat in uw StorSimple Manager-service.

    1. Voer de **registratie sleutel** die u hebt gekregen in **stap 2: ophalen van de sleutel voor de registratie van** in [Implementeren StorSimple virtuele matrix - voorbereiden van de Portal](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

    2. Als dit niet het eerste apparaat dat u met deze service registreren wilt moet u de **sleutel data codering**opgeven. Deze sleutel is vereist bij de registratie sleutel extra apparaten registreren bij de StorSimple Manager-service. Raadpleeg voor meer informatie, ophalen van [de sleutel data service](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) op het lokale web UI.

    3. Klik op **registreren**. Hiermee wordt het apparaat opnieuw gestart. Wellicht moet u wachten tot 2-3 minuten voordat het apparaat correct is geregistreerd. Nadat u het apparaat opnieuw is opgestart, wordt u naar de aanmeldingspagina worden genomen.

       ![Apparaat registreren](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Terug naar de klassieke Azure portal. Controleer of dat het apparaat is met de service verbonden door te zoeken naar de status op de pagina **apparaten** . Status van het apparaat moet **actief**zijn.

    ![Pagina apparaten](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Stap 2: Vul de vereiste apparaat instellen

Als u de configuratie van uw apparaat StorSimple wilt moet u:

- Selecteer een opslag-account koppelen aan uw apparaat.

- Kies instellingen voor codering voor de gegevens die worden verzonden naar de cloud.

De volgende stappen uitvoeren in de Azure klassieke portal de installatie vereist apparaat te voltooien.

#### <a name="to-complete-the-minimum-device-setup"></a>De minimaal vereiste apparaatstuurprogramma installatie te voltooien

1. Selecteer op de pagina **apparaten** het apparaat dat u zojuist hebt gemaakt. Dit apparaat zou weergegeven als **actief**. Klik op de pijl naast de naam van het apparaat en klik vervolgens op **Snel starten**.

    ![Pagina apparaten](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Klik op **volledige apparaat instellen** om het apparaat configureren wizard te starten.

    ![Wizard apparaat configureren](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. In de wizard configureren apparaat op de pagina **Algemene instellingen** het volgende doen:

   1. Geef een opslag-account moet worden gebruikt bij het apparaat. In dit abonnement, kunt u een bestaande account voor opslag in de lijst selecteren of kunt u **meer toevoegen** om een account uit een ander abonnement.

   2. Definieer de versleutelingsinstellingen voor de gegevens in rust die worden verzonden naar de cloud. (StorSimple gebruikt AES-256 versleuteling). Schakel het selectievakje **Enable cloud opslag codering** voor het coderen van uw gegevens. Voer een cloud opslag codering die 32 tekens bevat. Opnieuw op de toets om het te bevestigen.

   3. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Basisinstellingen](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    De instellingen wordt nu bijgewerkt. Nadat de instellingen zijn bijgewerkt, is de knop volledige apparaat instellingen niet beschikbaar. U gaat terug naar de pagina **Quick Start** van apparaat.                                                        

>[AZURE.NOTE]U kunt alle andere apparaatinstellingen op elk gewenst moment wijzigen via de pagina **configureren** .

## <a name="step-3-add-a-volume"></a>Stap 3: Een volume toevoegen

De volgende stappen uitvoeren in de Azure klassieke portal om een volume te maken.

#### <a name="to-create-a-volume"></a>Een volume maken

1. Klik op de pagina apparaat **Snel starten** **toevoegen van een volume**. Hiermee start u de wizard volume toevoegen.

2. In de wizard volume, onder **Basisinstellingen**toevoegen door het volgende te doen:

    1. Geef een unieke naam voor het volume. De naam moet een tekenreeks met 3 tot en met 127 tekens.

    2. Geef een beschrijving voor het volume. De beschrijving helpt identificeren van de eigenaars van het volume.

    3. Selecteer een gebruikstype voor het volume. Het gebruikstype **Tiered volume** kan worden of **lokaal volume vastgemaakt.** (**Tiered volume** is de standaardinstelling). Selecteer voor werkbelasting waarvoor lokale garanties, lage vertragingstijden en hogere prestaties, **vastgemaakt lokaal** **volume**. Selecteer **Tiered** - **volume**voor andere gegevens.

        Een lokaal vaste volume dik is ingericht en zorgt ervoor dat de primaire gegevens in het volume op het apparaat blijft en Mors geen naar de cloud. Als u een lokaal vaste volume maakt, wordt het apparaat gecontroleerd op beschikbare ruimte op de lokale niveaus van de aangevraagde grootte volumes creëren. Maken van een lokaal vaste volume mogelijk bestaande gegevens van het apparaat naar de cloud te lopen en de tijd die nodig is om het volume te maken is lang. De totale tijd is afhankelijk van de grootte van het volume ingerichte beschikbare netwerkbandbreedte en de gegevens op uw apparaat.

        Een gelaagde volume aan de andere kant dun is ingericht en zeer snel kan worden gemaakt. Bij het maken van een gelaagde volume ongeveer 10% van de ruimte is ingericht op het lokale niveau en 90% van de ruimte is ingericht in de cloud. Bijvoorbeeld als u een volume van 1 TB ingericht, 100 GB zou zich bevinden in de lokale ruimte en 900 GB in de cloud zou worden gebruikt wanneer de niveaus van de gegevens. Dit betekent op zijn beurt is dat als u geen lokale ruimte op het apparaat u kan geen gegevens leveren een gelaagde share (omdat de 10% niet beschikbaar is).

    4. Geef de ingerichte capaciteit voor het volume. Houd er rekening mee dat de opgegeven capaciteit kleiner zijn dan de beschikbare capaciteit moet. Als u een trapsgewijze volume maakt, wordt de grootte moet liggen tussen 500 GB en 5 TB. Geef de grootte van een volume tussen 50 en 500 GB voor een lokaal vaste volume. De beschikbare capaciteit gebruiken als leidraad voor het inrichten van een volume. Als de beschikbare lokale capaciteit 0 GB, wordt u niet toegestaan lokaal vastgemaakt of gelaagde volume inrichten.

        ![Basisinstellingen](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) Ga naar de volgende pagina.

3. Op de pagina **Extra instellingen** een nieuwe access control record toevoegen (ACR):

    1. Geef een **naam** voor uw ACR.

    2. Onder de **iSCSI-initiatornaam**, bieden de iSCSI Qualified Name (IQN) van de Windows-host. Als u niet de IQN hebt, gaat u naar de [bijlage A: Get de IQN van een Windows Server-host](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. Wij raden aan dat u een standaard back-up inschakelen door het selectievakje **inschakelt van een standaard back-up voor dit volume** . De standaard back-up wordt gemaakt van een beleid dat wordt uitgevoerd om 22:30 elke dag (apparaat-tijd) en maakt u een momentopname van een wolk van dit volume.

        ![Extra instellingen](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Hiermee start u de taak volume maken. U ziet een bericht Bezig met de volgende strekking.

        ![voortgangsbericht](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        Een volume wordt gemaakt met de opgegeven instellingen. Standaard zal controle en back-up worden ingeschakeld voor het volume.

    5. Om te bevestigen dat het volume is gemaakt, gaat u naar de pagina **Volumes** . Hier ziet u het volume weergegeven.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Stap 4: Koppel, initialiseren en formatteren van een volume

Voer de volgende stappen uit als u wilt koppelen, initialiseren en formatteren uw StorSimple volumes op een Windows Server-host.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Als u wilt koppelen, initialiseren en formatteren van een volume

1. Start de Microsoft iSCSI-initiator.

2. Klik in het venster **iSCSI Initiator-eigenschappen** op het tabblad **detectie** **Portal ontdekken**.

    ![Ontdek portal](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. Geef het IP-adres van uw iSCSI-netwerk-interface in het dialoogvenster **Ontdekken doelportal** en klik op **OK**.

    ![IP-adres](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. Zoek in het venster **iSCSI Initiator-eigenschappen** op het tabblad **doelen** de **doelen Discovered**. (Elk volume worden ontdekt doel.) Status van het apparaat moet worden weergegeven als **niet-actief**.

    ![ontdekte doelen](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Selecteer een doelapparaat en klik vervolgens op **verbinden**. Nadat het apparaat is aangesloten, moet de status wijzigen naar **verbonden**. (Zie voor meer informatie over het gebruik van de Microsoft iSCSI-initiator, [installeren en configureren van Microsoft iSCSI-Initiator] [1].

    ![doelapparaat](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. Druk op de toets met het Windows-Logo + X op de Windows-host, en klik vervolgens op **uitvoeren**.

7. Typ in het dialoogvenster **uitvoeren** **Diskmgmt.msc**. Klik op **OK**en het dialoogvenster **Schijfbeheer** wordt weergegeven. Het rechterdeelvenster worden de volumes weergegeven op de host.

8. Gekoppelde volumes wordt in het venster **Schijfbeheer** weergegeven zoals in de volgende afbeelding. Klik met de rechtermuisknop op het volume ontdekte (Klik op de schijfnaam van de) en klik vervolgens op **on line**.

    ![Schijfbeheer](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Klik met de rechtermuisknop en selecteer **Schijf initialiseren**.

    ![Initialiseer de schijf 1](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. In het dialoogvenster Selecteer de schijven die u wilt initialiseren en klik vervolgens op **OK**.

    ![Initialiseer de schijf 2](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. Hiermee start u de wizard Nieuw eenvoudig Volume. Selecteer een grootte van de schijf en klik vervolgens op **volgende**.

    ![wizard Nieuw volume 1](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Een stationsletter toewijzen aan het volume en klik op **volgende**.

    ![wizard Nieuw volume 2](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Geef de parameters om het volume te maken. **Alleen NTFS in Windows Server wordt ondersteund.** Stel de AUS tot 64 kB. Bieden een label voor het volume. Het is een aanbevolen beste praktijken voor deze naam identiek zijn aan de naam van het volume dat u hebt opgegeven op het virtuele apparaat StorSimple. Klik op **volgende**.

    ![wizard Nieuw volume 3](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Controleer de waarden voor het volume en klik vervolgens op **Voltooien**.

    ![wizard Nieuw volume 4](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    De volumes worden weergegeven als **Online** op de pagina met **Schijfbeheer** .

    ![volumes on line](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van het lokale web UI voor het [beheer van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Bijlage A: de IQN van een Windows Server-host ophalen

Doe het volgende als u de iSCSI Qualified Name (IQN) van een Windows-host met Windows Server 2012.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>De IQN van een Windows-host ophalen

1. Start de Microsoft iSCSI-initiator op de Windows-host.

2. In het venster **iSCSI Initiator-eigenschappen** op het tabblad **configuratie** en selecteer en kopieer de tekenreeks uit het veld **Naam van de Initiator** .

    ![iSCSI-initiator eigenschappen](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Deze tekenreeks wordt opgeslagen.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



