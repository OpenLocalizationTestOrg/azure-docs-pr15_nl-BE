<properties
   pageTitle="Terugzetten vanaf een back-up van uw virtuele matrix StorSimple"
   description="Meer informatie over het terugzetten van een back-up van uw virtuele matrix StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Terugzetten vanaf een back-up van uw virtuele matrix StorSimple

## <a name="overview"></a>Overzicht 

Dit artikel is van toepassing op de release van Microsoft Azure StorSimple virtuele matrix (ook bekend als de StorSimple op ruimten virtuele apparaatstuurprogramma of virtueel apparaat StorSimple) uitgevoerd maart 2016 algemene beschikbaarheid (GA) of hoger. In dit artikel wordt stapsgewijs beschreven hoe terugzetten vanaf een back-upset van de aandelen of de volumes op uw virtuele matrix StorSimple. Het artikel ook een gedetailleerd overzicht van de werking van het herstel op itemniveau op uw virtuele StorSimple-matrix die is geconfigureerd als bestandsserver.


## <a name="restore-shares-from-a-backup-set"></a>Aandelen terugzetten vanaf een back-upset


**Voordat u probeert te herstellen van aandelen, moet u ervoor zorgen dat er voldoende ruimte is op het apparaat om deze bewerking te voltooien.** Als u wilt terugzetten vanaf een back-up in de [Azure klassieke portal](https://manage.windowsazure.com/)kunt u de volgende stappen uitvoeren.

#### <a name="to-restore-a-share"></a>Herstellen van een aandeel

1.  Ga naar de **catalogus van de back-up**. Filteren op gewenste apparaat en tijdsbereik om te zoeken naar de back-ups. Klik op het pictogram voor ![](./media/storsimple-ova-restore/image1.png) de query uit te voeren.


1.  In de lijst met back-up wordt weergegeven, klikt u op en selecteert u een specifieke back-up. Vouw de back-up van de verschillende aandelen onder het overzicht. Klik op en selecteer een share die u wilt terugzetten.

2.  Klik op **herstellen als nieuwe**onderaan de pagina.

3.  Dit zal de wizard **terugzetten als een nieuwe share** op te starten. Op de pagina **Geef de naam en locatie** :


    1.  Controleer of de naam van het apparaat. Dit moet het apparaat met de share die u wilt terugzetten. De selectie is niet beschikbaar. Selecteer een ander apparaat, moet u de wizard afsluiten en opnieuw de back-up opnieuw instellen.

    2.  Geef een sharenaam. De share-naam moet 3 tot en met 127 tekens bevatten.

    3.  Bekijk de grootte, het type en de machtigingen die zijn gekoppeld aan de share die u probeert te herstellen. U zult kunnen wijzigen van de eigenschappen voor delen via Windows Verkenner nadat het terugzetten voltooid is.

    4.  Klik op het pictogram voor ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  Nadat het terugzetten voltooid is, wordt het terugzetten wordt gestart en ziet u een andere melding. Klik op **taak weergeven**om de voortgang van terugzetten. Hiervoor gaat u naar de pagina **taken** .

2.  U kunt de voortgang van de terugzettaak bijhouden. Als het herstellen 100% voltooid is, gaat u terug naar de pagina **Shares** op uw apparaat.

3.  U kunt nu de nieuwe share herstelde weergeven in de lijst met shares op uw apparaat. Let op bij het delen van hetzelfde type herstellen wordt uitgevoerd. Een gelaagde share wordt hersteld als lagen en een lokaal vaste delen als een lokaal vaste delen.

U hebt nu de configuratie voltooid en hebt geleerd hoe u een back-up of terugzetten van een share. 


## <a name="restore-volumes-from-a-backup-set"></a>Volumes terugzetten vanaf een back-upset


Als u wilt terugzetten vanaf een back-up in de klassieke Azure portal, kunt u de volgende stappen uitvoeren. Het terugzetten van herstelt de back-up op een nieuw volume op het virtuele apparaat; u kunt niet terugzetten naar een ander apparaat.

#### <a name="to-restore-a-volume"></a>Een volume herstellen

1.  Ga naar de **catalogus van de back-up**. Filteren op gewenste apparaat en tijdsbereik om te zoeken naar de back-ups. Klik op het pictogram voor ![](./media/storsimple-ova-restore/image1.png) de query uit te voeren.

2.  In de lijst met back-up wordt weergegeven, klikt u op en selecteert u een specifieke back-up. Vouw de back-up van de verschillende volumes onder het overzicht. Selecteer het volume dat u wilt terugzetten. 

5.  Klik op **herstellen als nieuwe**onderaan de pagina. De wizard **Nieuw volume terugzetten** wordt gestart.

1.  Op de pagina **Geef de naam en locatie** :


    1.  Controleer of de naam van het apparaat. Dit moet het apparaat met het volume dat u wilt terugzetten. Het apparaat is niet beschikbaar. Selecteer een ander apparaat, moet u de wizard afsluiten en opnieuw de back-up opnieuw instellen.

    2.  Een volumenaam opgeven voor het volume dat als nieuw wordt hersteld. Naam van het volume moet 3 tot en met 127 tekens bevatten.

    3.  Klik op het pijlpictogram.

        ![](./media/storsimple-ova-restore/image12.png)

1.  Selecteer de juiste ACRs in de vervolgkeuzelijst op de pagina **Geef hosts die gebruik van dit volume maken kunnen** .

    ![](./media/storsimple-ova-restore/image13.png)

1.  Klik op het pictogram voor ![](./media/storsimple-ova-restore/image1.png). Dit een terugzettaak wordt gestart en ziet u de volgende melding weergegeven dat de taak uitgevoerd wordt.

2.  Nadat het terugzetten voltooid is, wordt het terugzetten wordt gestart en ziet u een andere melding. Klik op **taak weergeven**om de voortgang van terugzetten. Hiervoor gaat u naar de pagina **taken** .

3.  U kunt de voortgang van de terugzettaak bijhouden. Ga terug naar de pagina van de **Volumes** op uw apparaat.

4.  U kunt nu het nieuwe volume teruggezet in de lijst met volumes weergeven op uw apparaat. Opmerking bij hetzelfde type volume herstellen wordt uitgevoerd. Een gelaagde volume wordt hersteld als lagen en een lokaal vaste volume wordt hersteld als een lokaal vaste volume.

5.  Als het volume wordt weergegeven in de lijst met volumes online, is het volume beschikbaar voor gebruik.  Vernieuw de lijst van doelen in het eigenschappenvenster van iSCSI-initiator op de iSCSI-initiator-host.  Een nieuw doel met de naam van het volume teruggezet moet worden weergegeven als 'inactive' in de statuskolom.

6.  Selecteer het doel en klik op **verbinden**.   Nadat de aanvrager is verbonden met het doel, wijzigt de status in **verbonden**. 

7.  Gekoppelde volumes wordt in het venster **Schijfbeheer** weergegeven zoals in de volgende afbeelding. Klik met de rechtermuisknop op het volume ontdekte (Klik op de schijfnaam van de) en klik vervolgens op **on line**.

> [AZURE.IMPORTANT] Als u een volume of een share terugzetten vanaf een back-up wilt instellen als de opdracht niet werkt, een doelvolume of share kan nog steeds worden gemaakt in de portal. Het is belangrijk dat u dit doelvolume verwijderen of in de portal delen te minimaliseren die voortvloeien uit dit element toekomstige problemen.

## <a name="item-level-recovery-ilr"></a>Herstel op itemniveau (ILR)

Deze release introduceert het herstel op itemniveau (ILR) op een StorSimple virtueel apparaat geconfigureerd als bestandsserver. De functie kunt u granulair herstel van bestanden en mappen vanaf een cloud back-up van alle shares op het apparaat StorSimple doen. Gebruikers kunnen verwijderde bestanden ophalen uit een recente back-ups met behulp van een model zelf.

Elk deel heeft een *.backups* map met de meest recente back-ups. De gebruiker kan navigeren naar de gewenste back-up, relevante bestanden en mappen kopiëren uit de back-up en herstellen. Hierdoor niet aanroepen voor beheerders voor bestanden terugzetten uit back-ups.

1.  Bij het uitvoeren van het ILR, kunt u de back-ups via Windows Verkenner weergeven. Klik op de specifieke delen die u wilt bekijken op de back-up voor. Ziet u een *.backups* -map onder de share waarin u de back-ups gemaakt. Vouw de map *.backups* om de back-ups weer te geven. De map wordt de geëxplodeerde weergave van de volledige back-structuur wordt weergegeven. Deze weergave is op verzoek gemaakt en meestal duurt slechts enkele seconden maken.

    De laatste 5 back-ups op deze manier worden weergegeven en kunnen worden gebruikt voor het uitvoeren van een item herstelt. De 5 recente back-ups worden standaard gepland en de handmatige back-ups bevatten.

    
    -   **Geplande back-ups** met de naam als &lt;apparaatnaam&gt;DailySchedule JJJJMMDD-UUMMSS UTC.

    -   **Handmatige back-ups van** Ad-hoc-JJJJMMDD-UUMMSS-UTC genoemd.
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Geef de back-up met de meest recente versie van het bestand is verwijderd. Hoewel de naam van de map bevat een UTC-tijdstempel in elk van de bovenstaande gevallen, is de tijd waarop de map is gemaakt het feitelijke apparaat tijd waarop de back-up wordt gestart. De tijdstempel van de map te zoeken en identificeren van de back-ups gebruiken.

2.  Zoek de map of het bestand dat u wilt terugzetten in de back-up die u hebt opgegeven in de vorige stap. Opmerking dat u kunt alleen bestanden of mappen die u machtigingen hebt voor weergeven. Als u geen toegang tot bepaalde bestanden of mappen, moet u contact opnemen met de beheerder van een share die met Windows Verkenner voor het bewerken van de share-machtigingen en hebt u toegang tot specifieke bestanden of mappen. Het is best aanbevolen dat de beheerder delen een groep in plaats van één gebruiker zijn.

3.  Kopieer het bestand of de map naar de juiste share op uw server StorSimple.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **Video beschikbaar**

Bekijk de video om te zien hoe u kunt shares maken, back-up van aandelen en gegevens terugzetten op een virtuele matrix StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple Virtual Array met behulp van het lokale web UI](storsimple-ova-web-ui-admin.md).
