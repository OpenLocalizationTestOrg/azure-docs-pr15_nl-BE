<properties
   pageTitle="Beheren van uw StorSimple volumes (U2) | Microsoft Azure"
   description="Legt uit hoe toevoegen, wijzigen, controleren en StorSimple volumes verwijderen, en hoe deze off line nemen indien nodig."
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
   ms.workload="NA"
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>De StorSimple Manager-service gebruiken voor het beheren van volumes (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u met de StorSimple Manager-service kunt maken en beheren van volumes op de StorSimple-apparaat en de StorSimple virtueel apparaat met Update 2 is geïnstalleerd.

De StorSimple Manager-service is een uitbreiding in Azure klassieke portal waarmee u uw oplossing StorSimple beheren vanaf een enkele web-interface. Naast het beheren van volumes, kunt u de StorSimple Manager-service maken en StorSimple services beheren, weergeven en beheren van apparaten, weergeven van waarschuwingen, en weergeven en beheren van back-beleid en de back-upcatalogus.

## <a name="volume-types"></a>Volumetypen

StorSimple volumes zijn:

- **Lokaal vastgemaakt volumes**: gegevens in deze volumes blijft op het lokale apparaat voor StorSimple te allen tijde.
- **Tiered volumes**: gegevens in deze volumes kunt Mors naar de cloud.

Een volume archivering is een gelaagde volume. De deduplication chunk groter gebruikt voor archivering volumes kan het apparaat groter segmenten van de gegevens overbrengen naar de cloud. 

Indien nodig kunt u het volume van lokale doorverbonden of uit geschakeld naar lokale. Ga naar [het volumetype wijzigen](#change-the-volume-type)voor meer informatie.

### <a name="locally-pinned-volumes"></a>Lokaal vastgezette volumes

Lokaal vastgezette volumes zijn volledig ingerichte volumes die geen gegevens van laag naar de cloud doen, opdat de lokale garandeert voor primaire gegevens onafhankelijk van cloud-connectiviteit. Gegevens op een lokaal vaste volumes is niet deduplicated en gecomprimeerd. momentopnamen van lokaal vastgezette volumes zijn echter deduplicated. 

Lokaal vastgezette volumes zijn volledig ingericht; Daarom moet er voldoende ruimte op het apparaat wanneer u deze maakt. U kunt lokaal vastgezette volumes tot maximaal 8 TB op het apparaat StorSimple 8100 en 20 TB op het apparaat 8600 inrichten. StorSimple behoudt zich alle lokale ruimte op het apparaat voor momentopnamen, metagegevens en gegevens verwerken. U kunt de tekengrootte van een lokaal vaste volume aan hoeveel ruimte maximaal beschikbaar, maar u kunt de grootte van een volume eenmaal is gemaakt niet verlagen.

Als u een lokaal vaste volume maakt, wordt de beschikbare ruimte voor het maken van volumes gelaagde verminderd. Het omgekeerde geldt ook: hebt u een bestaand trapsgewijs geschakelde volumes, de ruimte die beschikbaar is voor het maken van lokale volumes vastgemaakt is lager dan de hierboven vermelde maximumgehalten. Raadpleeg de [Veelgestelde vragen op een lokaal vaste volumes](storsimple-local-volume-faq.md)voor meer informatie op lokale volumes.   

### <a name="tiered-volumes"></a>Trapsgewijs geschakelde volumes

Trapsgewijs geschakelde volumes zijn dun ingerichte volumes waarin veelgebruikte gegevens lokaal op het apparaat blijft en minder vaak gebruikte gegevens automatisch wordt geschakeld naar de cloud. Thin provisioning is een virtualisatietechnologie waarin beschikbare opslagruimte dan fysieke bronnen worden weergegeven. In plaats van voldoende opslag van tevoren reserveren, StorSimple gebruikt thin provisioning net voldoende ruimte om te voldoen aan de huidige vereisten toe te wijzen. Het elastische karakter van cloud opslag vergemakkelijkt deze benadering omdat StorSimple kunt vergroten of verkleinen van cloud opslag om te voldoen aan veranderende behoeften.

Als u het trapsgewijs geschakelde volume wordt gebruikt voor het archiveren van gegevens, verandert het selectievakje **dit volume voor de minder vaak gebruikte gegevens archiveren gebruiken** in de grootte van de chunk deduplication voor het volume 512 kB. Als u deze optie niet selecteert, wordt de bijbehorende gelaagde volume een chunkgrootte van 64 KB gebruiken. Een groter deel van deduplication kan het apparaat versnellen de overdracht van grote archivering gegevens naar de cloud.

>[AZURE.NOTE] Archivering volumes die zijn gemaakt met een voorlopige Update 2-versie van StorSimple moeten worden geïmporteerd als doorverbonden met het selectievakje sleutelarchivering is ingeschakeld.

### <a name="provisioned-capacity"></a>Ingerichte capaciteit

Raadpleeg de volgende tabel voor ingerichte maximumcapaciteit voor elk type apparaat en het volume. (Opmerking die lokaal vastgemaakt volumes zijn niet beschikbaar op een virtueel apparaat).

|             | Maximale volumegrootte voor gelaagde | Lokaal maximale vastgemaakt volumegrootte |
|-------------|----------------------------|------------------------------------|
| **Fysieke apparaten** |       |       |
| 8100                 | 64 TB | 8 TB |
| 8600                 | 64 TB | 20 TB |
| **Virtuele apparaten**  |       |       |
| 8010                | 30 TB | N.V.T.   |
| 8020               | 64 TB | N.V.T.   |

## <a name="the-volumes-page"></a>De pagina Volumes

Op de pagina **Volumes** kunt u beheren de opslagvolumes die zijn ingericht op het apparaat met Microsoft Azure StorSimple voor de initiators (servers). De lijst met volumes wordt weergegeven op het apparaat StorSimple.

 ![Volumes pagina](./media/storsimple-manage-volumes-u2/VolumePage.png)

Een volume bestaat uit een reeks kenmerken:

- Het volume identificeren **Volumenaam** : een beschrijvende naam die uniek moet zijn en helpt Deze naam wordt ook gebruikt in rapporten controleren als u op een bepaald volume filteren.

- **Status** : kan on line of off line zijn. Als een volume off line is, is het niet zichtbaar voor initiators (servers) die toegang hebben tot het volume.

- **Capaciteit** : Hiermee geeft u de totale hoeveelheid gegevens die kunnen worden opgeslagen door de initiator (server). Volumes lokaal vastgemaakt zijn volledig ingericht en bevinden zich op het apparaat StorSimple. Trapsgewijs geschakelde volumes dun zijn ingericht en de gegevens is deduplicated. Met dun ingerichte volumes, het apparaat niet vooraf fysieke opslagcapaciteit intern of toewijzen van de wolk naar geconfigureerde volumecapaciteit. De volumecapaciteit is toegewezen en verbruikt op de vraag.

- **Type** : geeft aan of het volume is **Tiered** (de standaardinstelling) of **lokaal vastgemaakt**.

- **Back-up** : geeft aan of een standaard back-beleid bestaat voor het volume.

- **Toegang** : Hiermee geeft u de initiators die toegang hebben tot dit volume (servers). Initiators die geen deel uitmaken van access control-record (ACR) die is gekoppeld aan het volume wordt het volume niet weergegeven.

- **Monitoring** : geeft aan of er een volume wordt gecontroleerd. Een volume hebt controle standaard ingeschakeld wanneer deze wordt gemaakt. Voor het klonen van een volume worden uitgeschakeld, wordt echter controle. Als u wilt inschakelen voor een volume controle, volg de instructies in de [Monitor een volume](#monitor-a-volume). 

Volg de instructies in deze handleiding de volgende taken uitvoeren:

- Een volume toevoegen 
- Een volume wijzigen 
- Het volume wijzigen
- Een volume verwijderen 
- Een volume off line nemen 
- Een volume controleren 

## <a name="add-a-volume"></a>Een volume toevoegen

Tijdens de implementatie van uw oplossing StorSimple u [een volume gemaakt](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) . Toevoegen van een volume is een soortgelijke procedure.

#### <a name="to-add-a-volume"></a>Een volume toevoegen

1. Selecteert u het apparaat op de pagina **apparaten** , dubbelklikt u erop en klikt u vervolgens op het tabblad **Volume Containers** .

2. Een container van het volume in de lijst selecteren en dubbelklik erop om de volumes die zijn gekoppeld aan de container.

3. Klik op **toevoegen** onder aan de pagina. Hiermee start u een wizard volume toevoegen.

     ![Wizard volume basisinstellingen toevoegen](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. In de wizard volume, onder **Basisinstellingen**toevoegen door het volgende te doen:

  1. Geef een **naam** op voor het volume.
  2. Selecteer een **Soort Gebruik** in de vervolgkeuzelijst. Voor een standaardwerkbelasting waarvoor gegevens worden lokaal op het apparaat beschikbaar te allen tijde, selecteert u **Lokaal vastgemaakt**. Selecteer **Tiered**voor andere typen gegevens. (**Tiered** is de standaardinstelling).
  3. Als u **Tiered** in stap 2 hebt geselecteerd, kunt u het selectievakje **dit volume voor de minder vaak gebruikte gegevens archiveren gebruiken** voor het configureren van een volume van archivering.
  4. Voer de **Capaciteit ingericht** voor het volume in GB of TB. Zie [Provisioned capaciteit](#provisioned-capacity) voor de maximale grootte voor elk type apparaat en het volume. Bekijk de **Beschikbare capaciteit** om te bepalen hoeveel opslagruimte op uw apparaat daadwerkelijk beschikbaar is.

5. Klik op het pijlpictogram![Pictogram pijl](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Als u een lokaal vaste volume configureert, ziet u het volgende bericht weergegeven.

    ![Volume type bericht wijzigen](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. Klik op het pijlpictogram ![pictogram pijl](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)opnieuw om te gaan naar de pagina **Extra instellingen** .

    ![Extra instellingen van de wizard Volume toevoegen](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. Onder **Extra instellingen**een nieuwe access control record toevoegen (ACR):
  
  1. Selecteer record (ACR) voor een besturingselement in de vervolgkeuzelijst. U kunt ook een nieuwe ACR toevoegen. ACRs bepalen welke hosts toegang tot uw volumes door de host IQN overeenkomen met die vermeld in de record. Als u een ACR niet opgeeft, verschijnt het volgende bericht weergegeven.

        ![ACR opgeven](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. Wij raden aan dat u het selectievakje **inschakelt van een standaard back-up voor dit volume** .
  3. Klik op het pictogram ![Pictogram](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) het volume te maken met de opgegeven instellingen.

Het nieuwe volume is nu klaar voor gebruik.

>[AZURE.NOTE] Als u een lokaal vaste volume maken en maak vervolgens een ander lokaal vaste volume onmiddellijk daarna het volume maken taken na elkaar uitgevoerd. De eerste taak voor het maken van de volume moet zijn voltooid voordat de volgende volume maken taak kan beginnen.

## <a name="modify-a-volume"></a>Een volume wijzigen

Een volume wijzigen wanneer u wilt uitvouwen of de hosts die toegang hebben tot het volume wijzigen.

> [AZURE.IMPORTANT] 
>
> - Als u de grootte van het volume op het apparaat hebt gewijzigd, moet de grootte van het volume op de host ook worden gewijzigd. 
> - De hier beschreven stappen voor de host-zijde zijn voor Windows Server 2012 (2012R2). Procedures voor Linux of andere besturingssystemen host zullen afwijken. Raadpleeg de instructies van uw host-besturingssysteem bij het wijzigen van het volume op een host met een ander besturingssysteem. 

#### <a name="to-modify-a-volume"></a>Voor het wijzigen van een volume

1. Selecteert u het apparaat op de pagina **apparaten** , dubbelklikt u erop en klikt u vervolgens op het tabblad **Volume Containers** .

2. Een container van het volume in de lijst selecteren en dubbelklik erop om de volumes die zijn gekoppeld aan de container weer te geven.

3. Selecteer een volume en onderaan op de pagina, klikt u op **wijzigen**. Wijzigen volume wordt de wizard gestart.

4. In de wizard wijzigen volume onder **Algemene instellingen**kunt u het volgende doen:

  - De **naam**bewerken.
  - Converteren van het **Soort Gebruik** van lokaal vastgemaakt lagen aan of uit geschakeld naar lokaal vastgezet (Zie [het volumetype wijzigen](#change-the-volume-type) voor meer informatie).
  - Vergroot de **capaciteit ingericht**. De **Capaciteit ingericht** kan alleen worden verhoogd. U kunt een volume niet verkleinen nadat deze is gemaakt.

5. Onder **Aanvullende instellingen**kunt u de ACR, op voorwaarde dat het volume off line is. Als het volume on line is, moet u eerst off line nemen. Zie de stappen in [een volume off line nemen](#take-a-volume-offline) voor het wijzigen van de ACR.

    > [AZURE.NOTE] U kunt de optie **een standaard back-up** van het volume niet wijzigen.

6. Sla uw wijzigingen op door te klikken op het pictogram ![pictogram](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). De klassieke Azure portal weergegeven een bijwerken volume bericht. Er wordt een bericht weergegeven wanneer het volume is bijgewerkt.

7. Als u een volume uitbreidt, de volgende stappen op uw computer Windows-host:

   1. Ga naar **Computer Management** ->**Schijfbeheer**.
   2. **Schijfbeheer** met de rechtermuisknop en selecteer de **Schijven opnieuw controleren**.
   3. Selecteer in de lijst met schijven, het volume dat u hebt bijgewerkt, klik met de rechtermuisknop en klik vervolgens op **Volume uitbreiden**. Hiermee start u de wizard Volume uitbreiden. Klik op **volgende**.
   4. Voltooi de wizard, de standaardwaarden te accepteren. Nadat de wizard voltooid is, moet het volume worden de grotere grootte weergegeven.

    >[AZURE.NOTE] Als u een lokaal vaste volume uitbreiden en klik op vastgemaakt een ander lokaal volume onmiddellijk daarna de expansie volume taken opeenvolgend uitgevoerd. De eerste volume Uitbreidingstaak moet zijn voltooid voordat de volgende volume Uitbreidingstaak kan beginnen.

![Video beschikbaar](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video beschikbaar**

Om een video die laat zien hoe u een volume uitbreiden, klik [hier](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Het volume wijzigen

U kunt het volumetype van doorverbonden naar lokaal vastgemaakt of van lokaal vastgemaakt aan doorverbonden. Deze conversie moet echter niet een frequent voorkomt. Enkele redenen voor het converteren van een volume uit zijn geschakeld naar lokaal vastgemaakt:

- Lokale garanties met betrekking tot de beschikbaarheid van gegevens en prestaties
- Afschaffing van de wolk vertragingstijden en problemen met de cloud.

Dit zijn kleine bestaande volumes die u wilt benaderen regelmatig. Een lokaal vaste volume is volledig ingericht, wanneer deze wordt gemaakt. Als u een trapsgewijze volume naar een lokaal vaste volume converteert, StorSimple wordt gecontroleerd of er voldoende ruimte op het apparaat voordat de conversie is gestart. Als er onvoldoende ruimte is, krijgt u een foutmelding en wordt de bewerking geannuleerd. 

> [AZURE.NOTE] Voordat u de conversie van een trapsgewijs geschakeld naar lokaal vastgemaakt, zorg dat u rekening houden met de vereisten van uw andere werkbelasting. 

U kunt een lokaal vaste volume wijzigen in een gelaagde volume als u extra ruimte inrichten van andere volumes nodig. Als u converteert het volume lokaal vastgezette doorverbonden, de beschikbare capaciteit op de verhogingen van het apparaat door de grootte van de capaciteit van de vrijgegeven. Als u verbindingsproblemen te voorkomen dat de conversie van een volume van het type lokale naar het type trapsgewijze, vertoont het lokale volume eigenschappen van een gelaagde volume totdat de conversie is voltooid. Dit komt omdat sommige gegevens mogelijk naar de cloud terechtgekomen. Deze gemorst gegevens nog steeds lokale inneemt op het apparaat dat niet kan worden vrijgemaakt, totdat de bewerking is gestart en voltooid.

>[AZURE.NOTE] Converteren van een volume kan enige tijd duren en een conversie kan niet worden geannuleerd nadat deze is gestart. Het volume on line blijft tijdens de conversie, en u kunt back-ups uitvoeren, maar u kunt uitbreiden of het volume terugzet terwijl de conversie plaatsvindt.  

Conversie van een trapsgewijs geschakeld naar een lokaal vaste volume kan Apparaatprestaties nadelig beïnvloeden. Ook kunnen de volgende factoren het langer duren die nodig is om de conversie te voltooien:

- Er is onvoldoende bandbreedte.

- Er is geen recente back-up.

Minimaliseren van de effecten die deze factoren kunnen hebben:

- Beleid voor bandbreedteregeling controleren en zorg ervoor dat een speciale 40 Mbps-bandbreedte beschikbaar is.
- De conversie daluren.
- Een momentopname van een wolk nemen voordat u de conversie.

Als u meerdere volumes (ondersteuning voor verschillende werkbelastingen) converteren wilt, moet u de volumeconversie prioriteit dat hogere prioriteit volumes eerst worden geconverteerd. Bijvoorbeeld, moet u volumes met virtuele machines (VMs) of volumes met SQL werklasten converteren voordat u volumes met share-werklasten bestand converteren.

#### <a name="to-change-the-volume-type"></a>Het volumetype wijzigen

1. Selecteert u het apparaat op de pagina **apparaten** , dubbelklikt u erop en klikt u vervolgens op het tabblad **Volume Containers** .

2. Een container van het volume in de lijst selecteren en dubbelklik erop om de volumes die zijn gekoppeld aan de container weer te geven.

3. Selecteer een volume en onderaan op de pagina, klikt u op **wijzigen**. Wijzigen volume wordt de wizard gestart.

4. Op de pagina **Basisinstellingen** wijzigen de soort gebruik door het nieuwe type selecteren in de vervolgkeuzelijst **Type gebruik** .

    - Als u het type in **lokaal vastgemaakt wijzigt**, wordt StorSimple gecontroleerd of er voldoende capaciteit is.
    - Als u het type in **Tiered wijzigt** en dit volume wordt gebruikt voor het archiveren van gegevens, selecteert u het selectievakje **dit volume voor de minder vaak gebruikte gegevens archiveren** .

        ![Archief checkbox](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. Klik op het pijlpictogram ![pictogram pijl](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) naar de pagina **Extra instellingen** te gaan. Als u een lokaal vaste volume configureert, wordt het volgende bericht weergegeven.

    ![Volume type bericht wijzigen](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) opnieuw om door te gaan.

7. Klik op het pictogram ![Pictogram](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) om te starten tijdens de conversie. De portal Azure weergegeven een bijwerken volume bericht. Er wordt een bericht weergegeven wanneer het volume is bijgewerkt.

## <a name="take-a-volume-offline"></a>Een volume off line nemen

Wellicht moet u een volume off line nemen als u een planning wilt wijzigen of verwijderen. Wanneer een volume off line is, is het niet beschikbaar voor lezen / schrijven-toegang. U moet het volume off line nemen op de host en klik op het apparaat. 

#### <a name="to-take-a-volume-offline"></a>Een volume off line nemen

1. Zorg ervoor dat het betreffende volume niet gebruikt wordt voor off line nemen.

2. Het volume off line op de host eerste nemen. Hierdoor wordt alle potentiële gevaar voor beschadiging van gegevens op het volume. Raadpleeg de instructies voor uw hostbesturingssysteem voor specifieke stappen.

3. Nadat de host off line is, neemt het volume op het apparaat offline door de volgende stappen uit te voeren:

  1. Selecteert u het apparaat op de pagina **apparaten** , dubbelklikt u erop en klikt u vervolgens op het tabblad **Volume Containers** . Het tabblad **Volume Containers** worden in tabelvorm alle volume-containers die gekoppeld aan het apparaat zijn.
  2. Selecteer een container met volume en klik erop om de lijst van alle volumes in de container.
  3. Selecteer een volume en klik op **off line nemen**.
  4. Klik op **Ja**ter bevestiging. Het volume moet nu zijn off line.

    Nadat u een volume is off line, wordt **On line brengen** de beschikbaar.

> [AZURE.NOTE] De opdracht **Off line nemen** verzendt een verzoek naar het apparaat het volume off line nemen. Als het volume nog steeds door de hosts gebruikt wordt, hierdoor verbroken verbindingen, maar niet off line nemen van het volume zal mislukken. 

## <a name="delete-a-volume"></a>Een volume verwijderen

> [AZURE.IMPORTANT] U kunt een volume alleen verwijderen als deze off line is.

Voer de volgende stappen uit om een volume te verwijderen.

#### <a name="to-delete-a-volume"></a>Een volume verwijderen

1. Selecteert u het apparaat op de pagina **apparaten** , dubbelklikt u erop en klikt u vervolgens op het tabblad **Volume Containers** .

2. Selecteer de container volume met het volume dat u wilt verwijderen. Klik op de container volume toegang tot de pagina **Volumes** .

3. Alle volumes die zijn gekoppeld aan deze container worden weergegeven in tabelvorm. Controleer de status van het volume dat u wilt verwijderen. Als het volume dat u wilt verwijderen niet off line is, het offline halen eerst de stappen in [een volume off line nemen](#take-a-volume-offline).

4. Nadat het volume off line is, klikt u op **verwijderen** onder aan de pagina.

5. Klik op **Ja**ter bevestiging. Het volume worden nu verwijderd en de **Volumes** ziet u de bijgewerkte lijst met volumes in de container.

    >[AZURE.NOTE]Als u een lokaal vaste volume verwijdert, wordt de ruimte die beschikbaar is voor nieuwe volumes niet onmiddellijk bijgewerkt. De lokale schijfruimte door de Service StorSimple Manager regelmatig bijgewerkt. Wij bevelen dat u wacht een paar minuten voordat u het nieuwe volume.<br> Ook als u een lokaal vaste volume verwijderen en verwijder een ander lokaal vastgemaakt volume onmiddellijk daarna het volume verwijderen taken na elkaar uitgevoerd. De eerste taak volume moet zijn voltooid voordat de volgende volume taak kan beginnen.
 
## <a name="monitor-a-volume"></a>Een volume controleren

Controle, kunt u voor het verzamelen van I/O-gegevens voor een volume. Controle is standaard ingeschakeld voor de eerste 32 volumes die u maakt. Bewaking van extra volumes is standaard uitgeschakeld. Controle van de gekloonde volumes is ook standaard uitgeschakeld.

Voer de volgende stappen uit als u wilt in- of uitschakelen voor een volume controle.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Of controle uit te schakelen

1. Selecteert u het apparaat op de pagina **apparaten** , dubbelklikt u erop en klikt u vervolgens op het tabblad **Volume Containers** .

2. Selecteer de container van het volume waarop het volume zich bevindt en klik vervolgens op de toegang tot de pagina **Volumes** volume-container.

3. Alle volumes die zijn gekoppeld aan deze container worden weergegeven in de weergave in tabelvorm. Klik op en selecteer het volume of de volume-kloon.

4. Onderaan op de pagina, klikt u op **wijzigen**.

5. Selecteer in de wizard Volume wijzigen onder **Algemene instellingen** **in- of **uitschakelen** ** in de lijst **controle** .

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe u [een volume StorSimple kloon](storsimple-clone-volume.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).

 
