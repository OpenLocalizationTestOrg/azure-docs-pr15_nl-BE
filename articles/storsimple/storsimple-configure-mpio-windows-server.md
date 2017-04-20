<properties 
   pageTitle="MPIO configureren voor uw apparaat StorSimple | Microsoft Azure"
   description="Beschrijving van het MPIO (Multipath I/O) voor uw StorSimple-apparaat aangesloten op een Windows Server 2012 R2-host configureren."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>MPIO configureren voor uw apparaat StorSimple

Microsoft ondersteuning voor MPIO (Multipath I/O) in Windows Server help build hoge beschikbaarheid en fouttolerante SAN-configuraties functie ingebouwd. MPIO fysiek pad redundante onderdelen gebruikt, adapters, kabels en switches, voor het maken van logische paden tussen de server en het opslagapparaat. Als er een onderdeel het laat afweten, veroorzaakt door een logische pad mislukt Multipath-logica een alternatief pad gebruikt voor I/O, zodat toepassingen steeds toegang hun gegevens tot nog. Ook afhankelijk van uw configuratie, MPIO kan ook de prestaties verbeteren door opnieuw de belasting te verdelen over alle deze paden. Zie [overzicht van MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO-overzicht en kenmerken")voor meer informatie.  

Voor de hoge beschikbaarheid van uw oplossing StorSimple, moet op uw apparaat StorSimple MPIO worden geconfigureerd. Als u MPIO is geïnstalleerd op de hostservers met Windows Server 2012 R2, kunnen vervolgens de servers tolereren een koppeling, een netwerk of een storing in de netwerkinterface. 

MPIO is een optioneel onderdeel in Windows Server en wordt niet standaard geïnstalleerd. Deze moet worden geïnstalleerd als een functie via Serverbeheer. Dit onderwerp beschrijft de stappen die u moet volgen om te installeren en gebruiken van de MPIO-voorziening op een host met Windows Server 2012 R2 en verbonden met een fysiek apparaat StorSimple.

>[AZURE.NOTE] **Deze procedure is van toepassing voor alleen StorSimple 8000-serie. MPIO wordt momenteel niet ondersteund op een virtueel apparaat StorSimple.**

Moet u als volgt te werk om MPIO configureren op uw apparaat StorSimple:

- Stap 1: Installeer MPIO op de Windows Server-host

- Stap 2: MPIO configureren voor StorSimple volumes

- Stap 3: Mount StorSimple volumes op de host

- Stap 4: MPIO configureren voor maximale beschikbaarheid en taakverdeling

Elk van de bovenstaande stappen wordt in de volgende secties besproken.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Stap 1: Installeer MPIO op de Windows Server-host

Voer de volgende procedure om deze installeren op de Windows Server-host.

#### <a name="to-install-mpio-on-the-host"></a>MPIO installeren op de host

1. Open Serverbeheer op de Server Windows-host. Server Manager wordt standaard gestart wanneer een lid van de groep Administrators zich aanmeldt bij een computer met Windows Server 2012 R2 of Windows Server 2012. Als de Server Manager nog niet is geopend, klikt u op **Start > Server Manager**.
![Serverbeheer](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Klik op **Server Manager > Dashboard > functies en onderdelen toevoegen**. Hiermee start u de wizard **functies toevoegen en onderdelen** .
![Rollen en functies Wizard 1 toevoegen](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. In de wizard **functies toevoegen en onderdelen** als volgt weergeven:

    - Klik op **volgende**op de pagina **voordat u begint** .
    - Accepteer de standaardinstelling van de **rol of functie** installatie op de pagina **installatietype selecteren** . Klik op **volgende**. ![Toevoegen, rollen en functies Wizard 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - Kies **een server uit de server-groep selecteren**op de pagina **Selecteer de doelserver** . De hostserver moet automatisch worden opgespoord. Klik op **volgende**.
    - Klik op **volgende**op de pagina **Serverrollen selecteren** .
    - Op de pagina **functies selecteren** selecteert u **MPIO**en klik op **volgende**. ![Toevoegen, rollen en functies Wizard 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - Bevestig de selectie en selecteer **de doelserver opnieuw automatisch indien nodig**, zoals hieronder wordt weergegeven op de pagina **installatie bevestigen zijn geselecteerd** . Klik op **installeren**. ![Toevoegen, rollen en functies Wizard 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - U krijgt als de installatie voltooid is. Klik op **sluiten** om de wizard te sluiten. ![Toevoegen, rollen en functies Wizard 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Stap 2: MPIO configureren voor StorSimple volumes

MPIO moet worden geconfigureerd om aan te duiden StorSimple volumes. Voer de volgende stappen uit om te configureren MPIO StorSimple volumes herkent.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>MPIO voor StorSimple volumes configureren

1. Open de **MPIO-configuratie**. Klik op **Server Manager > Dashboard > hulpprogramma's > MPIO**.

2. Selecteer het tabblad **Multipaden** in het dialoogvenster **Eigenschappen voor MPIO** .

3. **Ondersteuning toevoegen voor iSCSI-apparaten**selecteren en klik vervolgens op **toevoegen**.  
![Eigenschappen voor MPIO ontdekken meerdere paden](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. De server wanneer u wordt gevraagd opnieuw opstarten.
5. Klik op het tabblad **MPIO-apparaten** in het dialoogvenster **Eigenschappen voor MPIO** . Klik op **toevoegen**.
    </br>![MPIO eigenschappen voor MPIO-apparaten](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. Voer het serienummer van uw apparaat in **MPIO-ondersteuning toevoegen** in het dialoogvenster **Hardware-ID apparaat**. U kunt het serienummer van het apparaat opvragen door toegang tot uw service Manager StorSimple en navigeren naar **apparaten > Dashboard**. Het serienummer van het apparaat wordt weergegeven in het rechter deelvenster van het dashboard apparaat **Snel bekijken** .
    </br>![MPIO-ondersteuning toevoegen](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. De server wanneer u wordt gevraagd opnieuw opstarten.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Stap 3: Mount StorSimple volumes op de host

Nadat MPIO is geconfigureerd op Windows Server, volume (s) op het apparaat StorSimple gemaakt kan worden gemonteerd en kunnen vervolgens profiteren van MPIO voor redundantie. Om een volume te koppelen, kunt u de volgende stappen uitvoeren.

#### <a name="to-mount-volumes-on-the-host"></a>Volumes koppelen op de host

1. Open het venster **iSCSI Initiator-eigenschappen** op de Server van Windows. Klik op **Server Manager > Dashboard > hulpprogramma's > iSCSI-Initiator**.
2. Klik in het dialoogvenster **iSCSI-Initiator eigenschappen** klikt u op het tabblad detectie en klik op **Doelportal ontdekken**.
3. Klik in het dialoogvenster **Doelportal ontdekken** het volgende doen:
    
    - Geef het IP-adres van de poort van uw apparaat StorSimple (bijvoorbeeld gegevensinvoer 0).
    - Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI-Initiator eigenschappen** .

    >[AZURE.IMPORTANT] **Als u een particulier netwerk voor iSCSI-verbindingen gebruikt, voert u het IP-adres van de poort die is verbonden met het particuliere netwerk.**

4. Herhaal stap 2-3 voor een tweede (bijvoorbeeld gegevens 1) netwerkinterface op uw apparaat. Houd er rekening mee dat deze interfaces voor iSCSI-moeten worden ingeschakeld. Voor meer informatie hierover, Zie [netwerkinterfaces wijzigen](storsimple-modify-device-config.md#modify-network-interfaces).
5. Selecteer het tabblad **doelen** in het dialoogvenster **iSCSI-Initiator eigenschappen** . Hier ziet u de StorSimple apparaat doel IQN onder **Doelen ontdekt**.
 ![iSCSI-Initiator Eigenschappen-tabblad doelen](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Klik op **verbinden** om een iSCSI-sessie tot stand brengen met uw apparaat StorSimple. Een **verbinding maken met het doel** van het dialoogvenster wordt weergegeven.

7. Schakel het selectievakje **meerdere paden inschakelen** in het dialoogvenster **verbinding maken met het doel** . Klik op **Geavanceerd**.

8. Klik in het dialoogvenster **Geavanceerde instellingen** door het volgende te doen:                                       
    -    Selecteer de **Microsoft iSCSI-Initiator**op **Lokale Adapter** in de vervolgkeuzelijst.
    -    Selecteer het IP-adres van de host op de vervolgkeuzelijst **IP Initiator** .
    -    Selecteer in de vervolgkeuzelijst **Doelportal** IP-het OT van device interface.
    -    Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI-Initiator eigenschappen** .

9. Klik op **Eigenschappen**. Klik op **Sessie toevoegen**in het dialoogvenster **Eigenschappen** .
10. Schakel het selectievakje **meerdere paden inschakelen** in het dialoogvenster **verbinding maken met het doel** . Klik op **Geavanceerd**.
11. Klik in het dialoogvenster **Geavanceerde instellingen** :                                        
    -  Selecteer de Microsoft iSCSI-Initiator op **lokale adapter** in de vervolgkeuzelijst.
    -  Selecteer het IP-adres dat overeenkomt met de host op de vervolgkeuzelijst **IP Initiator** . In dit geval aansluit u twee netwerkinterfaces op het apparaat op een enkel netwerkinterface op de host. Deze interface is dus hetzelfde als die voor de eerste sessie.
    -  Selecteer het IP-adres voor de tweede data interface is ingeschakeld op het apparaat in de vervolgkeuzelijst **Doel Portal IP** .
    -  Klik op **OK** om terug te keren naar de iSCSI-Initiator in het dialoogvenster. U kunt een tweede sessie hebt toegevoegd aan het doel.

12. Open **Computerbeheer** door te navigeren naar **Server Manager > Dashboard > Computerbeheer**. Klik in het linkerdeelvenster op **opslag > Schijfbeheer**. De volumes op de StorSimple apparaat gemaakt die zichtbaar zijn voor deze host worden onder **Schijfbeheer** weergegeven als nieuwe schijven.

13. Initialiseer de schijf en een nieuw volume maakt. Bij de indeling, selecteer een blokgrootte van 64 KB.
![Schijfbeheer](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. Onder **Schijfbeheer**met de rechtermuisknop op de **schijf** en selecteer **Eigenschappen**.
15. In het StorSimple Model ### **Eigenschappen van schijf apparaat met meerdere paden** dialoogvenster op het tabblad **MPIO** .
![StorSimple 8100 multipath-schijf DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. Klik op **Details** en controleer of de parameters worden ingesteld als de standaardparameters in de sectie **Naam DSM** . De standaardparameters zijn:

    - Pad controleren periode = 30
    - Aantal herhaalpogingen = 3
    - Bob periode verwijdert = 20
    - Interval voor opnieuw proberen = 1
    - Pad controleren ingeschakeld = uitgeschakeld.


>[AZURE.NOTE] **De standaardparameters niet wijzigen.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Stap 4: MPIO configureren voor maximale beschikbaarheid en taakverdeling

Voor meerdere paden op basis van beschikbaarheid en taakverdeling, moeten meerdere sessies handmatig worden toegevoegd om aan te geven van de verschillende beschikbare paden. Bijvoorbeeld, als de host heeft twee interfaces die zijn verbonden met SAN en het apparaat heeft twee interfaces die zijn verbonden met SAN, moet u vier sessies die zijn geconfigureerd met het juiste pad permutaties (slechts twee sessies worden vereist als elk DATA-interface en de hostinterface op een ander IP-subnet en niet routeerbaar is).

>[AZURE.IMPORTANT] **Het is raadzaam niet 1 GbE en 10 GbE-netwerkinterfaces te mengen. Als u twee netwerkinterfaces, moeten beide interfaces het hetzelfde type.**

De volgende procedure wordt beschreven hoe u toe te voegen wanneer een apparaat StorSimple met twee netwerkinterfaces is verbonden met een host met twee netwerkinterfaces sessies.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>MPIO configureren voor maximale beschikbaarheid en taakverdeling

1. Het doel te doorzoeken: Klik in het dialoogvenster **iSCSI Initiator-eigenschappen** op het tabblad **detectie** **Portal ontdekken**.
2. Voer in het dialoogvenster **verbinding maken met het doel** het IP-adres van een van de netwerkinterfaces van het apparaat.
3. Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI-Initiator eigenschappen** .

4. Klik op het tabblad **doelen** in het dialoogvenster **iSCSI-Initiator eigenschappen** markeren het ontdekte doel en klik vervolgens op **verbinden**. Het dialoogvenster **verbinding maken met het doel** wordt weergegeven.

5. Klik in het dialoogvenster **verbinding maken met het doel** :
    
    - Laat de standaard geselecteerde doel instellen voor **deze verbinding toevoegen** aan de lijst met favoriete doelen. Dit maakt het apparaat automatisch geprobeerd de verbinding opnieuw starten telkens wanneer deze computer wordt opnieuw opgestart.
    - Schakel het selectievakje **meerdere paden inschakelen** in.
    - Klik op **Geavanceerd**.

6. Klik in het dialoogvenster **Geavanceerde instellingen** :
    - Selecteer de **Microsoft iSCSI-Initiator**op **Lokale Adapter** in de vervolgkeuzelijst.
    - Selecteer het IP-adres van de host op de vervolgkeuzelijst **IP Initiator** .
    - Selecteer in de vervolgkeuzelijst **Doel Portal IP** het IP-adres van de interface van de gegevens op het apparaat is ingeschakeld.
    - Klik op **OK** om terug te keren naar de iSCSI-Initiator in het dialoogvenster.

7. Klik op **Eigenschappen**en klik op **Sessie toevoegen**in het dialoogvenster **Eigenschappen** .

8. Schakel het selectievakje **meerdere paden inschakelen** in en klikt u op **Geavanceerd**in het dialoogvenster **verbinding maken met het doel** .

9. Klik in het dialoogvenster **Geavanceerde instellingen** :
    1. Selecteer de **Microsoft iSCSI-Initiator**op **lokale adapter** in de vervolgkeuzelijst.
    2. Selecteer het IP-adres dat overeenkomt met de tweede op de host-interface op de vervolgkeuzelijst **IP Initiator** .
    3. Selecteer het IP-adres voor de tweede data interface is ingeschakeld op het apparaat in de vervolgkeuzelijst **Doel Portal IP** .
    4. Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI-Initiator eigenschappen** . U hebt nu een tweede sessie toegevoegd aan het doel.

10. Herhaal stap 8-10 extra sessies (paden) toevoegen aan het doel. U kunt een totaal van vier sessies toevoegen met twee interfaces op de host en twee op het apparaat.

11. Na het toevoegen van de gewenste sessies (paden) in het dialoogvenster **iSCSI-Initiator eigenschappen** , selecteer het doel en klik op **Eigenschappen**. Opmerking op het tabblad sessies van het dialoogvenster **Eigenschappen van** de vier sessie-id's die met het pad van mogelijke permutaties overeenkomen. Om een sessie te annuleren, selecteert u het selectievakje naast een sessie-id en klik vervolgens op **verbinding verbreken**.

12. Om apparaten die zijn ingediend binnen sessies weergeven, selecteer het tabblad **apparaten** . Als u MPIO-beleid voor het geselecteerde apparaat wilt configureren, klikt u op **MPIO**. Het dialoogvenster **Details apparaat** wordt weergegeven. Klik op het tabblad **MPIO** kunt u de juiste instellingen voor **Beleid voor taakverdeling** . U kunt ook het pad **actief** of **stand-by-** type weergeven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de service Manager StorSimple te wijzigen van de apparaatconfiguratie StorSimple](storsimple-modify-device-config.md).
 
