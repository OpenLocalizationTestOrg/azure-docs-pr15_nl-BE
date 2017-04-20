<properties 
   pageTitle="MPIO configureren op de virtuele matrix StorSimple host | Microsoft Azure"
   description="Beschrijving van het MPIO (Multipath I/O) voor uw StorSimple virtuele matrix die is verbonden met een Windows Server 2012 R2-host configureren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>MPIO configureren op Windows Server host voor de virtuele matrix StorSimple

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe functie MPIO (Multipath I/O) installeren op de Windows Server-host, specifieke configuratie-instellingen voor StorSimple alleen volumes van toepassing en controleer of MPIO voor StorSimple volumes. De procedure wordt ervan uitgegaan dat uw virtuele StorSimple 1200-Array met twee netwerkinterfaces is verbonden met een Windows Server-host met twee netwerkinterfaces. De informatie in dit artikel geldt alleen voor de virtuele matrix. Voor meer informatie over apparaten StorSimple 8000-serie, gaat u naar [MPIO configureren voor StorSimple host](storsimple-configure-mpio-windows-server.md). 

De MPIO-functie in Windows Server helpt build hoge beschikbaarheid en fouttolerante opslagconfiguraties. MPIO fysiek pad redundante onderdelen gebruikt, adapters, kabels en switches, voor het maken van logische paden tussen de server en het opslagapparaat. Als er een onderdeel het laat afweten, veroorzaakt door een logische pad mislukt Multipath-logica een alternatief pad gebruikt voor I/O, zodat toepassingen steeds toegang hun gegevens tot nog. Ook afhankelijk van uw configuratie, MPIO kan ook de prestaties verbeteren door opnieuw de belasting te verdelen over alle deze paden. Zie [overzicht van MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO-overzicht en kenmerken")voor meer informatie.  

Voor de hoge beschikbaarheid van uw oplossing StorSimple, MPIO te configureren op de Windows Server-hosts op uw virtuele StorSimple 1200-matrix (ook bekend als het op ruimten virtueel apparaat) aangesloten. De host-servers kunnen vervolgens een koppeling, een netwerk of een storing in de netwerkinterface tolereren. 

Moet u als volgt te werk om MPIO configureren: 

- Vereisten voor configuratie

- Stap 1: Installeer MPIO op de Windows Server-host

- Stap 2: MPIO configureren voor StorSimple volumes

- Stap 3: Mount StorSimple volumes op de host

Elk van de bovenstaande stappen wordt in de volgende secties besproken.


## <a name="prerequisites"></a>Vereisten

Dit gedeelte wordt ingegaan op de configuratie vereisten voor de Windows Server-host en uw virtuele matrix.

### <a name="on-windows-server-host"></a>Op Windows Server host

-  Zorg ervoor dat uw Windows Server host 2 netwerkinterfaces ingeschakeld.


### <a name="on-storsimple-virtual-array"></a>Op StorSimple virtuele matrix

- De virtuele matrix moet worden geconfigureerd als een iSCSI-server. Voor meer informatie, Zie [virtuele matrix als een iSCSI-server instellen](storsimple-ova-deploy3-iscsi-setup.md). Een of meer netwerkinterfaces moeten zijn ingeschakeld voor de matrix.   

- De netwerkinterfaces op uw virtuele matrix moet bereikbaar zijn vanaf de Windows Server-host.

- Een of meer volumes moeten worden gemaakt op uw virtuele matrix StorSimple. Voor meer informatie, Zie [een volume toevoegen](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) op uw virtuele StorSimple 1200-matrix. In deze procedure wij 3 delen (2 lokaal vastgemaakt en 1 gelaagde volume zoals hieronder) op de virtuele matrix.
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Hardwareconfiguratie voor virtuele matrix StorSimple

In de volgende afbeelding ziet u de hardwareconfiguratie voor maximale beschikbaarheid en Multipath-taakverdeling voor uw Windows Server-host en de StorSimple virtuele matrix gebruikt in deze procedure.  

![MPIO hardwareconfiguratie](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Zoals in de voorgaande afbeelding:

- De StorSimple virtuele matrix ingericht op Hyper-V is een enkel knooppunt actief apparaat geconfigureerd als een iSCSI-server.

- Twee virtuele netwerkinterfaces zijn ingeschakeld op uw array. Controleren of twee netwerkinterfaces worden ingeschakeld door te navigeren naar **Instellingen** , zoals hieronder wordt weergegeven in het lokale web UI van uw virtuele 1200-matrix:

    ![Netwerkinterfaces op 1200 ingeschakeld](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Noteer de IPv4-adressen, van de ingeschakelde netwerkinterfaces (Ethernet, 2 Ethernet-standaard) en opslaan voor toekomstig gebruik op de host.

- Twee netwerkinterfaces zijn ingeschakeld op de Windows Server-host. Als de gekoppelde interfaces voor de host en array in hetzelfde subnet zijn, wordt dan er 4 paden beschikbaar. Dit was het geval in deze procedure. Echter, als elke netwerkinterface op de matrix en de host-interface in een ander IP-subnet (en niet routeerbaar), vervolgens slechts 2 paden is beschikbaar.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Stap 1: Installeer MPIO op de Windows Server-host

MPIO is een optioneel onderdeel in Windows Server en wordt niet standaard geïnstalleerd. Deze moet worden geïnstalleerd als een functie via Serverbeheer. Voer de volgende procedure om deze installeren op de Windows Server-host.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Stap 2: MPIO configureren voor StorSimple volumes

MPIO moet worden geconfigureerd om aan te duiden StorSimple volumes. Voer de volgende stappen uit om te configureren MPIO StorSimple volumes herkent.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Stap 3: Mount StorSimple volumes op de host

Nadat MPIO is geconfigureerd op Windows Server, volume (s) op de array StorSimple gemaakt kan worden gemonteerd en kunnen vervolgens profiteren van MPIO voor redundantie. Om een volume te koppelen, kunt u de volgende stappen uitvoeren.

#### <a name="to-mount-volumes-on-the-host"></a>Volumes koppelen op de host

1. Open het venster **iSCSI Initiator-eigenschappen** op de Server van Windows. Klik op **Server Manager > Dashboard > hulpprogramma's > iSCSI-Initiator**.
2. Klik in het dialoogvenster **iSCSI-Initiator eigenschappen** klikt u op het tabblad detectie en klik op **Doelportal ontdekken**.
3. Klik in het dialoogvenster **Doelportal ontdekken** het volgende doen:
    
    - Voer het IP-adres van de eerste ingeschakelde netwerkinterface op uw virtuele matrix StorSimple. Dit kunnen standaard **Ethernet**. 
    - Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI-Initiator eigenschappen** .

    >[AZURE.IMPORTANT] **Als u een particulier netwerk voor iSCSI-verbindingen gebruikt, voert u het IP-adres van de poort die is verbonden met het particuliere netwerk.**

4. Herhaal stap 2-3 voor een netwerkinterface voor tweede (bijvoorbeeld Ethernet-2) in de matrix. 

5. Selecteer het tabblad **doelen** in het dialoogvenster **iSCSI-Initiator eigenschappen** . Voor uw virtuele matrix ziet u elk oppervlak volume als doel onder **Doelen ontdekt**. In dit geval zou drie doelen (overeenkomend met drie volumes) worden ontdekt.

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Klik op **verbinden** om te maken van een iSCSI-sessie met de matrix StorSimple. Een **verbinding maken met het doel** van het dialoogvenster wordt weergegeven. Schakel het selectievakje **meerdere paden inschakelen** in. Klik op **Geavanceerd**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. Klik in het dialoogvenster **Geavanceerde instellingen** door het volgende te doen:                                       
    -    Selecteer de **Microsoft iSCSI-Initiator**op **Lokale Adapter** in de vervolgkeuzelijst.
    -    Selecteer het IP-adres van de host op de vervolgkeuzelijst **IP Initiator** .
    -    Selecteer in de vervolgkeuzelijst **Doelportal** IP-het OT van matrix-interface.
    -    Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI-Initiator eigenschappen** .

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Klik op **Eigenschappen**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. Klik op **Sessie toevoegen**in het dialoogvenster **Eigenschappen** .

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. Schakel het selectievakje **meerdere paden inschakelen** in het dialoogvenster **verbinding maken met het doel** . Klik op **Geavanceerd**.
11. Klik in het dialoogvenster **Geavanceerde instellingen** :                                        
    -  Selecteer de Microsoft iSCSI-Initiator op **lokale adapter** in de vervolgkeuzelijst.
    -  Selecteer het IP-adres dat overeenkomt met de host op de vervolgkeuzelijst **IP Initiator** . In dit geval aansluit u twee netwerkinterfaces op de array op een enkel netwerkinterface op de host. Deze interface is dus hetzelfde als die voor de eerste sessie.
    -  Selecteer in de vervolgkeuzelijst **Doel Portal IP** het IP-adres voor de tweede data interface is ingeschakeld op de matrix.
    -  Klik op **OK** om terug te keren naar de iSCSI-Initiator in het dialoogvenster. U kunt een tweede sessie hebt toegevoegd aan het doel.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Na het toevoegen van de gewenste sessies (paden) in het dialoogvenster **iSCSI-Initiator eigenschappen** , selecteer het doel en klik op **Eigenschappen**. Opmerking op het tabblad sessies van het dialoogvenster **Eigenschappen van** de vier sessie-id's die met het pad van mogelijke permutaties overeenkomen. Om een sessie te annuleren, selecteert u het selectievakje naast een sessie-id en klik vervolgens op **verbinding verbreken**.
 
    - Om apparaten die zijn ingediend binnen sessies weergeven, selecteer het tabblad **apparaten** . Als u MPIO-beleid voor het geselecteerde apparaat wilt configureren, klikt u op **MPIO**. De **
    -  Details van** in het dialoogvenster wordt weergegeven. Op de **MPIO** tabblad kunt u de juiste **taakverdelingsbeleid** instellingen. U kunt ook bekijken de **actief** of **stand-by ** padtekst.

10. Herhaal stap 8-11 extra sessies (paden) toevoegen aan het doel. U kunt een totaal van vier sessies van elk doel toevoegen met twee interfaces op de host en twee op de virtuele matrix. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. U moet deze stappen herhalen voor elk volume (oppervlakken als doel).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. Open **Computerbeheer** door te navigeren naar **Server Manager > Dashboard > Computerbeheer**. Klik in het linkerdeelvenster op **opslag > Schijfbeheer**. De volumes op de StorSimple virtuele matrix gemaakt die zichtbaar zijn voor deze host worden onder **Schijfbeheer** weergegeven als nieuwe schijven.

13. Initialiseer de schijf en een nieuw volume maakt. Bij de indeling, selecteert u een clustergrootte (AUS) van 64 KB. Herhaal dit proces voor alle beschikbare volumes.

    ![Schijfbeheer](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. Onder **Schijfbeheer**met de rechtermuisknop op de **schijf** en selecteer **Eigenschappen**.

15. Klik op het tabblad **MPIO** in het dialoogvenster **Eigenschappen voor schijf apparaat met meerdere paden** .

    ![Schijfeigenschappen](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. Klik op **Details** en controleer of de parameters worden ingesteld als de standaardparameters in de sectie **Naam DSM** . De standaardparameters zijn:

    - Pad controleren periode = 30
    - Aantal herhaalpogingen = 3
    - Bob periode verwijdert = 20
    - Interval voor opnieuw proberen = 1
    - Pad controleren ingeschakeld = uitgeschakeld.

    >[AZURE.NOTE] **De standaardparameters niet wijzigen.**


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw virtuele matrix StorSimple](storsimple-ova-manager-service-administration.md).
 
