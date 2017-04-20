<properties
   pageTitle="Overzicht virtuele matrix StorSimple | Microsoft Azure"
   description="Beschrijving van de StorSimple virtuele matrix, een geïntegreerde opslagoplossing die opslagtaken tussen een virtueel apparaat op gebouwen en Microsoft Azure cloud opslag beheert."
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
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="introduction-to-the-storsimple-virtual-array"></a>Inleiding tot de virtuele matrix StorSimple

## <a name="overview"></a>Overzicht

Welkom bij de Microsoft Azure StorSimple virtuele matrix, een geïntegreerde opslagoplossing die opslagtaken tussen een virtueel apparaat van op gebouwen uitgevoerd in een hypervisor en Microsoft Azure cloud opslag beheert. De virtuele matrix (ook bekend als de StorSimple op ruimten virtueel apparaat) is een efficiënte, rendabele en eenvoudig beheerbare bestandsserver of iSCSI-serveroplossing die veel van de problemen en kosten in verband met de onderneming opslag en gegevensbescherming. De virtuele matrix is met name geschikt voor de scenario's voor RAS/kantoor office (ROBO).

Dit overzicht is gericht op de virtuele matrix. 

- Voor een overzicht van de StorSimple 8000-serie, gaat u naar [StorSimple 8000-serie: een hybride cloud-oplossing](storsimple-overview.md). 

- Ga naar het [StorSimple on line Help](http://onlinehelp.storsimple.com/)voor meer informatie over het apparaat StorSimple 5000/7000-serie.

De virtuele matrix ondersteunt iSCSI of Server Message Block (SMB)-protocol. Het wordt uitgevoerd op uw bestaande infrastructuur van hypervisor en biedt trapsgewijs schakelen naar de cloud, cloud back-up, snel terugzetten, herstel op itemniveau en disaster recovery-functies.

De volgende tabel bevat een overzicht van de belangrijkste functies van de virtuele matrix.

| Functie | Virtuele matrix |
| ------- | ------------- |
|Vereisten voor de installatie | Maakt gebruik van virtualisatie infrastructuur (Hyper-V of VMware)|
| Beschikbaarheid | Één knooppunt |
| Totale capaciteit (met inbegrip van de wolk) |Maximaal 64 TB bruikbare capaciteit per virtueel apparaat |
| Lokale capaciteit | 390 GB tot bruikbare capaciteit van 6,4 TB per virtueel apparaat (moet inrichten tot 8 TB 500 GB schijfruimte)|
| Native protocollen | iSCSI- of SMB |
| Herstel tijd doelstelling (RTO) | iSCSI: minder dan 2 minuten, ongeacht de grootte |
| Herstel punt doelstelling (vrijgegeven Productieorder) | Dagelijkse back-ups en back-ups op aanvraag |
| Opslag trapsgewijs schakelen | Gebruik warmte toewijzen om te bepalen welke gegevens moeten worden doorverbonden in of uit |
| Ondersteuning | Virtualisatie infrastructuur, ondersteund door de leverancier |
| Prestaties | Is afhankelijk van de onderliggende infrastructuur |
| Gegevensmobiliteit | Het apparaat kunt herstellen of op itemniveau herstel (bestandsserver) |
| Opslag-lagen | Opslag van lokale hypervisor en cloud |
| Delen van grootte |Lagen: tot 20 TB; lokaal vastgezet: 2 TB |
| Volumegrootte |Lagen: tot 5 TB; lokaal vastgezet: 500 GB |
| Momentopnamen | Crashconsistent |
| Herstel op itemniveau | Ja; gebruikers kunnen herstellen van aandelen |

## <a name="why-use-storsimple"></a>Waarom StorSimple gebruiken?

StorSimple verbonden gebruikers en servers met Azure opslag in minuten met geen wijzigingen in de toepassing.

In de volgende tabel worden enkele van de belangrijkste voordelen van de oplossing met virtuele matrix beschreven.

| Functie | Voordeel |
|---------|---------|
| Transparante integratie | De virtuele matrix ondersteunt de iSCSI- of het SMB-protocol. De verplaatsing van gegevens tussen het lokale niveau en het niveau van de wolk is naadloos en transparant voor de gebruiker.|
| Minder opslagkosten | Met StorSimple, moet u voldoende lokale opslag om te voldoen aan de huidige vraag voor de meest gebruikte gegevens hot inrichten. Opslag behoeften groeien, cloud StorSimple lagen koude gegevens rendabele opslag. De gegevens worden deduplicated en gecomprimeerd voordat u naar de cloud opslagvereisten en kosten verder te verlagen.|
| Vereenvoudigd opslagbeheer | StorSimple biedt u gecentraliseerd beheer in de cloud StorSimple Manager gebruiken voor het beheren van meerdere apparaten.| 
| Verbeterde herstel- en conformiteit | StorSimple vereenvoudigt noodherstel sneller door onmiddellijk herstel van de metagegevens en de gegevens zo nodig terugzetten. Dit betekent normaal gesproken met een minimale overlast kunnen blijven.|
| Gegevensmobiliteit | Gegevens worden doorverbonden naar de cloud toegankelijk is vanaf andere sites voor herstel- en migratie. Opmerking u kunt gegevens alleen aan de oorspronkelijke virtuele matrix terugzetten. Echter met disaster recovery-functies kunt u de hele virtuele matrix terugzetten naar een andere virtuele matrix.|

## <a name="storsimple-workload-summary"></a>StorSimple werkbelasting samenvatting

Hieronder een overzicht van de ondersteunde StorSimple werkbelasting tabelindeling.

| Scenario                | Werkbelasting              | Ondersteund |  Beperkingen                                  | Versie              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| ROBO-samenwerking      | Delen van bestanden          | Ja       | Zie de [maximale grenzen voor bestandsserver](storsimple-ova-limits.md). <br>Zie [de systeemvereisten voor ondersteunde versies van SMB](storsimple-ova-system-requirements.md).   | Alle versies      |


## <a name="workflows"></a>Workflows

De virtuele matrix StorSimple is bijzonder geschikt voor de volgende workflows:

- [Beheer cloud-gebaseerde opslag](#cloud-based-storage-management)
- [Locatie-onafhankelijke back-up](#location-independent-backup)
- [Bescherming en herstel na storing herstel van gegevens](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Beheer cloud-gebaseerde opslag

De StorSimple Manager service wordt uitgevoerd in de klassieke Azure portal kunt u gegevens die zijn opgeslagen op meerdere apparaten en op meerdere vestigingen beheren. Dit is vooral nuttig in scenario's van gedistribueerde tak. Houd er rekening mee dat u afzonderlijke exemplaren van de service Manager StorSimple matrices virtuele en fysieke StorSimple apparaten beheren moet maken. 

![beheer cloud-gebaseerde opslag](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Locatie-onafhankelijke back-up

Met de virtuele matrix bieden cloud momentopnamen een locatie-onafhankelijke point-in-time kopie van een volume of -share. Wolk momentopnamen zijn standaard ingeschakeld en kunnen niet worden uitgeschakeld. Alle volumes en gedeelde back-up worden tegelijkertijd door middel van een dagelijkse back-up beleid en u kunt nemen meer ad hoc back-ups wanneer nodig.

### <a name="data-protection-and-disaster-recovery"></a>Bescherming en herstel na storing herstel van gegevens

De virtuele Array ondersteunt de volgende gegevens bescherming en disaster recovery scenario's:

- **Volume of share herstellen** – gebruik het terugzetten als nieuwe werkstroom te herstellen van een volume of -share. Deze methode gebruiken voor het herstellen van het volledige volume of de share.
- **Herstel op itemniveau** – Shares toestaan vereenvoudigde toegang tot recente back-ups. U kunt eenvoudig een afzonderlijk bestand herstellen uit een map speciaal .backup beschikbaar in de cloud. Deze terugzetten gebruiker aangestuurde is en geen tussenkomst is vereist.
- **Disaster recovery** – gebruik de uitvalbeveiligingsmogelijkheden herstellen alle volumes of -shares op een nieuwe virtuele matrix. U de nieuwe virtuele matrix maken en registreren met de StorSimple Manager-service, waarna de oorspronkelijke virtuele matrix failover. De nieuwe virtuele matrix krijgt vervolgens de ingerichte bronnen. 

## <a name="virtual-array-components"></a>Onderdelen van virtuele matrix

De virtuele matrix bevat de volgende onderdelen:

- [Virtuele matrix](#virtual-array) – een hybride cloud-opslagapparaat op basis van een virtuele machine in uw gevirtualiseerde omgeving of hypervisor ingericht.  
- Een of meer StorSimple apparaten beheren [StorSimple Manager-service](#storsimple-manager-service) : een uitbreiding van de Azure klassieke portal waarmee u kunt vanaf een enkele web-interface die u kunt openen vanuit verschillende geografische locaties. U kunt de StorSimple Manager-service maken en beheren van services weergeven en apparaten en waarschuwingen beheren en beheren van volumes, aandelen en bestaande momentopnamen.
- [Gebruikersinterface van lokale web](#local-web-user-interface) – een web gebaseerde gebruikersinterface die wordt gebruikt voor het apparaat te configureren, zodat er verbinding met het lokale netwerk en het apparaat vervolgens met de StorSimple Manager-service registreren. 
- [Interface met opdrachtregel](#command-line-interface) : een interface voor Windows PowerShell kunt u een ondersteuningssessie te starten op de virtuele matrix.
In de volgende secties worden deze onderdelen nader beschreven en wordt uitgelegd hoe de oplossing worden gegevens gerangschikt, wijst opslagruimte toe en vereenvoudigt het beheer van opslag en gegevensbescherming.

### <a name="virtual-array"></a>Virtuele matrix

De virtuele matrix is een opslagoplossing met één knooppunt biedt primaire opslag, communicatie met cloud opslag beheert en waarborgt de veiligheid en vertrouwelijkheid van alle gegevens die zijn opgeslagen op het apparaat.

De virtuele matrix is beschikbaar in een model dat kan worden gedownload. De opslagarray heeft een maximale capaciteit van 6,4 TB op het apparaat (met een onderliggende benodigde opslagruimte van 8 TB) en met inbegrip van 64 TB cloud opslag. 

De virtuele Array heeft de volgende functies:

- Het is goedkoper. Het maakt gebruik van uw bestaande infrastructuur van virtualisatie en op uw bestaande Hyper-V of VMware hypervisor kunnen worden ingezet.
- Het bevindt zich in het datacenter en kan worden geconfigureerd als een iSCSI-server of een bestandsserver. 
- Het is geïntegreerd met de cloud.
- Back-ups worden opgeslagen in de cloud, die kan vergemakkelijken noodherstel en itemniveau recovery (ILR) vereenvoudigen. 
- U kunt updates toepassen op de virtuele matrix, net zoals u deze op een fysiek apparaat toepassen wilt.

>[AZURE.NOTE] Een virtuele matrix kan niet worden uitgebreid. Daarom is het belangrijk om voldoende opslagruimte te creëren bij het maken van het virtuele apparaat. 

### <a name="storsimple-manager-service"></a>StorSimple Manager-service

Microsoft Azure StorSimple biedt een webgebaseerde gebruikersinterface (de StorSimple Manager service) waarmee u kunt Centraal beheer datacenter en opslag van de wolk. De StorSimple Manager-service kunt u de volgende taken uitvoeren:

- Meerdere virtuele StorSimple-Arrays beheren vanaf een enkele service. 
- Configureren en beheren van beveiligingsinstellingen voor StorSimple apparaten. (Codering in de cloud is afhankelijk van de API's van Microsoft Azure.)
- De referenties van opslag en eigenschappen configureren.
- Configureren en beheren voor volumes of shares.
- Back-up en herstel van gegevens op volumes of -shares.
- Prestaties controleren.
- Instellingen controleren en mogelijke problemen.

Met de StorSimple Manager-service kunt u dagelijks beheer van uw virtuele matrix uitvoeren.

Ga naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md)voor meer informatie.

### <a name="local-web-user-interface"></a>Gebruikersinterface van lokale web

De virtuele matrix bevat een web gebaseerde gebruikersinterface die wordt gebruikt voor eenmalige configuratie en de registratie van het apparaat met de StorSimple Manager-service. U kunt het afsluiten en opnieuw de virtuele matrix, diagnostische tests uitvoeren-software bijwerken, het apparaat administrator-wachtwoord wijzigen, systeemlogboeken bekijken en contact opnemen met Microsoft Support service aanvragen. 

Ga voor meer informatie over het gebruik van het web gebaseerde gebruikersinterface tot het [gebruik van de webgebaseerde gebruikersinterface voor het beheer van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Interface met opdrachtregel

De opgenomen interface voor Windows PowerShell kunt u een ondersteuningssessie met Microsoft Support voor starten zodat deze helpt u bij het oplossen van problemen die op het virtuele apparaat optreden kunnen.

## <a name="storage-management-technologies"></a>Technologieën voor het beheer van opslag

Naast de virtuele matrix en andere onderdelen wordt de StorSimple oplossing met de volgende softwaretechnologieën snel toegang bieden tot belangrijke gegevens, opslag, verbruik te verlagen en beveiligen van gegevens die zijn opgeslagen op uw virtuele Array:

- [Automatische opslag trapsgewijs schakelen](#automatic-storage-tiering) 
- [Lokaal vastgezette aandelen en volumes](#locally-pinned-shares-and-volumes)
- [Deduplication en compressie van gegevens trapsgewijs geschakeld of een back-up naar de cloud](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [Back-ups van de geplande en on-demand](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatische opslag trapsgewijs schakelen

Een nieuw mechanisme voor trapsgewijs schakelen de virtuele matrix gebruikt voor het beheren van opgeslagen gegevens over het virtuele matrix en de cloud. Er zijn slechts twee lagen: de lokale virtuele matrix en Azure cloud opslag. De virtuele matrix StorSimple rangschikt gegevens automatisch in de lagen op basis van een warmte-kaart die het huidige gebruik, de leeftijd en de relaties met andere gegevens worden bijgehouden. Gegevens die het meest actief zijn (beste) lokaal is opgeslagen, terwijl minder actieve en inactieve gegevens worden automatisch gemigreerd naar de cloud. (Alle back-ups worden opgeslagen in de cloud). StorSimple worden aangepast en de gegevens opnieuw worden geordend en toewijzingen als gebruikspatronen opslag wijzigen. Sommige gegevens kan bijvoorbeeld worden minder actief na verloop van tijd. Als het actief wordt geleidelijk minder, is het van doorverbonden naar de cloud. Als dezelfde gegevens opnieuw geactiveerd wordt, wordt het trapsgewijs geschakeld naar de opslagmatrix.

Gegevens voor een bepaalde gelaagde delen of het volume is gegarandeerd een eigen lokale tier ruimte. (ongeveer 10% van de totale ingerichte ruimte voor dat aandeel of volume). Terwijl dit wordt beperkt door de beschikbare ruimte op het virtuele apparaat voor delen of het volume, zorgt u ervoor dat trapsgewijs schakelen van een share of het volume niet worden beïnvloed door de trapsgewijs behoeften van andere aandelen of volumes. Een zeer druk bezet werkbelasting op een share of het volume dwingen niet dus andere werkbelasting op de cloud. 

![automatische opslag trapsgewijs schakelen](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] Kunt u een volume als lokaal vastgemaakt, in welk geval de gegevens op de virtuele matrix blijft en nooit wordt doorverbonden naar de cloud. Ga naar het [lokaal vastgemaakt aandelen en volumes](#locally-pinned-shares-and-volumes)voor meer informatie.

### <a name="locally-pinned-shares-and-volumes"></a>Lokaal vastgezette aandelen en volumes

Kunt u geschikte aandelen en volumes als lokaal vastgemaakt. Deze functie zorgt ervoor dat gegevens die nodig zijn voor kritische toepassingen blijft in de virtuele matrix is nooit doorverbonden naar de cloud. Lokaal vastgezette aandelen en volumes hebben de volgende functies: 

- Ze zijn niet cloud vertragingstijden of verbindingsproblemen.
- Ze blijven profiteren van de StorSimple cloud backup en disaster recovery-functies.

U kunt een lokaal vaste delen of volume als lagen of op een gelaagde of herstellen als lokaal volume vastgemaakt. 

Ga naar [de StorSimple Manager-service voor het beheren van volumes gebruiken](storsimple-manage-volumes-u2.md)voor meer informatie over lokaal vastgezette volumes.

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Deduplication en compressie van gegevens trapsgewijs geschakeld of een back-up naar de cloud

Opslagvereisten voor in de cloud te verminderen wordt StorSimple deduplication en gegevens comprimeren. Deduplication vermindert het totale bedrag van de gegevens die zijn opgeslagen door het elimineren van de redundantie in de gegevens opgeslagen. Als de gegevens veranderen, StorSimple negeert de gegevens ongewijzigd en wordt alleen de wijzigingen vastgelegd. Bovendien vermindert StorSimple de hoeveelheid opgeslagen gegevens door het identificeren en verwijderen van dubbele gegevens. 

>[AZURE.NOTE] Gegevens die zijn opgeslagen op de virtuele matrix is niet deduplicated of gecomprimeerd. Alle deduplication en compressie vindt plaats vlak voordat de gegevens worden verzonden naar de cloud.

### <a name="scheduled-and-on-demand-backups"></a>Back-ups van de geplande en on-demand

Functies voor gegevensbeveiliging StorSimple kunnen u back-ups op verzoek maken. Bovendien een standaard back-upschema zorgt ervoor dat de back-up dagelijks. Back-ups worden genomen in de vorm van incrementele momentopnamen die zijn opgeslagen in de cloud. -Snapshots, waarmee alleen de wijzigingen sinds de laatste back-up opneemt, worden gemaakt en snel hersteld. Deze momentopnamen is zeer belangrijk bij herstel na storing scenario's omdat ze secundaire opslag-systemen (zoals tape back-up vervangen) en kunnen u gegevens terugzetten op uw datacenter of op andere sites als nodig is.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [voorbereiden van de virtuele matrix-portal](storsimple-ova-deploy1-portal-prep.md).


