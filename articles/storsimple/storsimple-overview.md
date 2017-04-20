<properties 
   pageTitle="Wat is StorSimple? | Microsoft Azure" 
   description="Beschrijving StorSimple trapsgewijs schakelen, het apparaat virtueel apparaat, services en opslagbeheer en introduceert de belangrijkste termen die worden gebruikt in StorSimple." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000-serie: een hybride cloud-opslagoplossing

## <a name="overview"></a>Overzicht

Welkom bij Microsoft Azure StorSimple, een geïntegreerde opslagoplossing die opslagtaken tussen apparaten op gebouwen en Microsoft Azure cloud opslag beheert. StorSimple is een efficiënte, rendabele en eenvoudig beheerbare SAN (SAN)-oplossing die veel van de problemen en kosten in verband met de onderneming opslag en gegevensbescherming. Het maakt gebruik van de eigen StorSimple 8000-serie apparaat geïntegreerd met cloud services en biedt een aantal hulpprogramma's voor een naadloze weergave van alle enterprise opslag, met inbegrip van cloud opslag. (De StorSimple distributie-informatie gepubliceerd op de website van Microsoft Azure geldt voor StorSimple 8000-serie alleen apparaten. Als u een apparaat StorSimple 5000/7000-serie, gaat u naar [StorSimple Help](http://onlinehelp.storsimple.com/).)

StorSimple [opslag trapsgewijs schakelen](#automatic-storage-tiering) worden beheerd met opgeslagen gegevens over de verschillende opslagmedia. De huidige werkset is opgeslagen op-gebouwen aan solid state harde schijven (SSD), gegevens die minder vaak wordt gebruikt, worden opgeslagen op de vaste-schijfstations (harde schijven) en archivering van gegevens naar de cloud wordt geduwd. Bovendien gebruikt StorSimple deduplication en compressie verminderen de hoeveelheid opslagruimte die de gegevens in beslag. Ga voor meer informatie naar [Deduplication en compressie](#deduplication-and-compression). Ga naar [StorSimple terminologie](#storsimple-terminology) aan het einde van dit artikel voor definities van andere belangrijke termen en begrippen die worden gebruikt in de documentatie StorSimple 8000-serie.

Met 2 StorSimple, kunt u de geschikte volumes identificeren als *lokaal vastgemaakt* om ervoor te zorgen dat de primaire gegevens lokaal op het apparaat blijft en niet trapsgewijs naar de cloud. Hiermee kunt u de werklast die gevoelig voor cloud latentie, zoals SQL en virtuele machine werkbelasting, op lokaal vastgemaakt volumes zijn blijven de cloud gebruiken voor back-up uitvoeren. Zie voor meer informatie over lokaal vastgezette volumes [gebruiken de StorSimple Manager-service voor het beheren van volumes](storsimple-manage-volumes-u2.md). 

Update 2 kunt u StorSimple virtuele apparaten die van de vertragingstijden van lage en hoge prestaties geleverd door Azure premium opslag gebruikmaken maken. Zie voor meer informatie over StorSimple premium virtuele apparaten [implementeren en beheren van een virtueel apparaat StorSimple in Azure](storsimple-virtual-device-u2.md). Ga naar voor meer informatie over opslag Azure premium [Premium-opslag: krachtige opslag voor een standaardwerkbelasting Azure Virtual Machine](../storage/storage-premium-storage.md).

Naast het beheer van opslag, kunnen u op aanvraag maken met functies voor gegevensbeveiliging StorSimple en geplande back-ups en deze lokaal of in de cloud opslaan. Back-ups worden genomen in de vorm van momentopnamen, incrementeel, wat betekent dat ze worden gemaakt en snel hersteld. Wolk momentopnamen is zeer belangrijk bij herstel na storing scenario's omdat ze secundaire opslag-systemen (zoals tape back-up vervangen) en kunnen u gegevens terugzetten op uw datacenter of op andere sites als nodig.

![video-pictogram](./media/storsimple-overview/video_icon.png) Bekijk de video voor een korte inleiding tot Microsoft Azure StorSimple.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>Waarom StorSimple gebruiken?

De volgende tabel worden enkele van de belangrijkste voordelen van Microsoft Azure StorSimple beschreven.

| Functie | Voordeel |
|---------|---------|
|Transparante integratie | Microsoft Azure StorSimple maakt gebruik van het iSCSI-protocol onzichtbaar opslagfaciliteiten gegevens koppelen. Dit zorgt ervoor dat gegevens die zijn opgeslagen in de cloud, op het datacenter, of wordt weergegeven op één locatie worden opgeslagen op externe servers.|
|Minder opslagkosten|Microsoft Azure StorSimple toegewezen voldoende lokaal of cloud opslag om te voldoen aan de huidige vraag en breidt cloud opslag alleen indien nodig. Verder vermindert opslagvereisten en kosten doordat overbodige versies van dezelfde gegevens (deduplication) en door het gebruik van compressie.|
|Vereenvoudigd opslagbeheer|Microsoft Azure StorSimple biedt beheerprogramma's waarmee u kunt configureren en beheren van gegevens die zijn opgeslagen op-locatie, op een externe server en in de cloud. Bovendien kunt u back-up beheren en functies herstellen vanaf een Microsoft Management Console (MMC)-module. StorSimple biedt een aparte optionele interface die u gebruiken kunt om uit te breiden StorSimple beheer- en beveiligingsservices voor inhoud die is opgeslagen op de SharePoint-servers. |
|Verbeterde herstel- en conformiteit|Microsoft Azure StorSimple vereist geen uitgebreide hersteltijd. In plaats daarvan worden hersteld gegevens nodig is. Dit betekent normaal gesproken met een minimale overlast kunnen blijven. Bovendien kunt u beleidsregels als back-up schema's en gegevens wilt opgeven.
|Gegevensmobiliteit|Gegevens geüpload naar Microsoft Azure cloud services toegankelijk vanaf andere sites voor herstel- en migratie. Bovendien kunt u StorSimple StorSimple virtuele apparaten configureren op virtuele machines (VMs) uitgevoerd in Microsoft Azure. Het VMs kunt virtuele apparaten toegang tot opgeslagen gegevens bij wijze van test of herstel.|
|Ondersteuning voor andere cloud-providers |De StorSimple 8000-serie met software-update 1 of hoger ondersteunt Amazon S3-Bronrecords en HP OpenStack cloud services zoals Microsoft Azure. (U moet een account van Microsoft Azure opslag apparaat ten behoeve van.) Ga naar [Wat is er nieuw in Update 1.2](storsimple-update1-release-notes.md#whats-new-in-update-12)voor meer informatie.|
|Bedrijfscontinuïteit | Update 1 of hoger biedt een nieuwe migratiefunctie die StorSimple 5000-7000 series gebruikers hun gegevens migreren naar een apparaat StorSimple 8000-serie.|
|Beschikbaarheid in Azure regering Portal | StorSimple Update 1 of hoger is beschikbaar in de Portal Azure regering. Zie [Deploy uw apparaat op gebouwen StorSimple in de regering Portal](storsimple-deployment-walkthrough-gov.md)voor meer informatie.|
|Gegevensbescherming en beschikbaarheid | De StorSimple 8000-serie met Update 1 of hoger ondersteunt Zone redundante opslag (ZRS), behalve Lokaal redundante opslag (LRS) en Geo-redundante opslag (GRS). Raadpleeg [dit artikel op Azure opslag redundantieopties](https://azure.microsoft.com/documentation/articles/storage-redundancy/) voor ZRS details.|
|Ondersteuning voor essentiële toepassingen. | U kunt met 2 StorSimple, adequate volumes als lokaal vastgemaakt identificeren. Deze functie zorgt ervoor dat de gegevens die nodig zijn voor kritische toepassingen niet naar de cloud is doorverbonden. Lokaal vastgezette volumes zijn niet cloud vertragingstijden of verbindingsproblemen. Zie voor meer informatie over lokaal vastgezette volumes [gebruiken de StorSimple Manager-service voor het beheren van volumes](storsimple-manage-volumes-u2.md).|
|Lage latentie en hoge prestaties | StorSimple Update 2 kunt u virtuele apparaten die van de krachtige functies van opslag Azure premium lage latentie gebruikmaken te maken. Zie voor meer informatie over StorSimple premium virtuele apparaten [implementeren en beheren van een virtueel apparaat StorSimple in Azure](storsimple-virtual-device-u2.md).|

![video pictogram](./media/storsimple-overview/video_icon.png) Bekijk [deze video](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) voor een overzicht van de StorSimple 8000-serie functies en voordelen.

## <a name="storsimple-components"></a>StorSimple onderdelen

De Microsoft Azure StorSimple oplossing omvat de volgende onderdelen:

- **Microsoft Azure StorSimple apparaat** – een matrix op ruimten hybride opslag met SSD en harde schijven, redundante controllers en automatische failover mogelijkheden. De domeincontrollers beheren opslag trapsgewijs schakelen, het plaatsen van momenteel gebruikt (of warme) gegevens op lokale opslag (in het apparaat of op gebouwen servers), terwijl minder vaak gebruikte gegevens te verplaatsen naar de cloud.
- **Virtueel apparaat StorSimple** – ook bekend als het virtuele apparaat StorSimple, dit is een softwareversie van het apparaat StorSimple die worden gerepliceerd van de architectuur en de meeste mogelijkheden van de hybride fysieke opslagapparaat. Het virtuele apparaat StorSimple op één knooppunt in een Azure virtuele machine wordt uitgevoerd. Premium virtuele apparaten die van Azure premium opslag gebruikmaken, zijn beschikbaar in Update 2 en hoger.
- **StorSimple Manager-service** : een uitbreiding van de Azure klassieke portal waarmee u een StorSimple apparaat of een virtueel apparaat StorSimple beheren vanaf een enkele web-interface. U kunt de StorSimple Manager-service maken en services beheren, weergeven en beheren van apparaten, waarschuwingen weergeven, volumes, beheren en weergeven en beheren van back-beleid en de back-catalogus.
- **Windows PowerShell voor StorSimple** -interface met een opdrachtregel die u gebruiken kunt voor het beheren van de StorSimple apparaat. Windows PowerShell voor StorSimple bevat functies waarmee u kunnen u uw apparaat StorSimple registreren, de netwerkinterface op het apparaat te configureren, bepaalde soorten updates installeren, problemen met uw apparaat via de support-sessie en de status van het apparaat wijzigen. U kunt Windows PowerShell openen voor StorSimple aansluiten op de seriële console of met Windows PowerShell remoting.
- **StorSimple van azure PowerShell cmdlets** – een verzameling Windows PowerShell-cmdlets waarmee u kunt het serviceniveau en migratie automatiseren vanaf de opdrachtregel. Ga naar de [cmdlet referentie](https://msdn.microsoft.com/library/dn920427.aspx)voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple.
- **StorSimple Snapshot Manager** – een MMC-module die volume groepen en Windows Volume Shadow Copy-Service gebruikt voor het genereren van toepassing consistent back-ups. Bovendien kunt u StorSimple Snapshot Manager voor het maken van back-up schema's en klonen of volumes herstellen. 
- **StorSimple Adapter voor SharePoint** : een hulpmiddel dat transparant Microsoft Azure StorSimple opslag en gegevensbescherming naar SharePoint Server-farms breidt, terwijl StorSimple opslag zichtbaar en te beheren vanuit Centraal beheer van SharePoint portal.

Het onderstaande diagram wordt een globaal overzicht van de Microsoft Azure StorSimple architectuur en onderdelen.

![StorSimple architectuur](./media/storsimple-overview/overview-big-picture.png)

In de volgende secties worden deze onderdelen nader beschreven en wordt uitgelegd hoe de oplossing worden gegevens gerangschikt, wijst opslagruimte toe en vereenvoudigt het beheer van opslag en gegevensbescherming. De laatste sectie vindt u definities voor enkele van de belangrijke termen en begrippen met betrekking tot StorSimple onderdelen en het beheer ervan.

## <a name="storsimple-device"></a>StorSimple apparaat

Het apparaat Microsoft Azure StorSimple is een op gebouwen hybride opslagarray met primaire opslag en iSCSI-toegang tot gegevens die zijn opgeslagen op deze. Deze communicatie met cloud opslag beheert en waarborgt de veiligheid en vertrouwelijkheid van alle gegevens die zijn opgeslagen op de Microsoft Azure StorSimple oplossing.

Het apparaat StorSimple omvat SSD harde schijven de harde schijven, evenals ondersteuning voor failover clustering en automatische. Het bevat een gedeelde processor en gedeelde opslag twee gespiegelde controllers. Elke domeincontroller biedt het volgende:

- Verbinding met een host-computer
- Maximaal zes netwerkpoorten voor verbinding met het lokale netwerk (LAN)
- Hardware controleren
- Niet-vluchtig RAM-geheugen (NVRAM), die de informatie behouden, zelfs als de stroom wordt onderbroken
- Clusters bijwerken voor het beheren van software-updates op de servers in een failover-cluster zodat de updates bieden echter weinig of geen invloed op de beschikbaarheid van de service
- Cluster-service, dat als een back-end-cluster, leveren van maximale beschikbaarheid en minimaliseren van de schadelijke effecten die optreden kunnen als een harde schijf of SSD uitvalt of off line genomen

Slechts één domeincontroller is actief op elk punt in de tijd. Als de actieve controller uitvalt, actief de tweede controller automatisch. 

Ga naar [StorSimple hardwareonderdelen en de status](storsimple-monitor-hardware-status.md)voor meer informatie.

## <a name="storsimple-virtual-device"></a>StorSimple virtueel apparaat

StorSimple kunt u een virtueel apparaat die de architectuur en de mogelijkheden van het apparaat fysiek hybride opslag repliceert maken. De StorSimple virtueel apparaat (ook wel bekend als de StorSimple virtueel toestel) op een enkel knooppunt in een Azure virtuele machine wordt uitgevoerd. (Een virtueel apparaat kan alleen worden gemaakt op een Azure virtuele machine. U kunt geen maken op een apparaat StorSimple of een server op locatie.) 

Het virtuele apparaat heeft de volgende functies:

- Het gedraagt zich als een fysieke toestel en iSCSI-interface voor virtuele machines in de cloud kan bieden. 
- U kunt een onbeperkt aantal virtuele apparaten maken in de cloud en schakel ze in en uit indien nodig. 
- Het kan helpen simuleren op ruimten omgevingen herstel, ontwikkeling en Testscenario's en kan u helpen met het ophalen van back-ups op itemniveau. 

Met 2 en hoger is het virtuele apparaat StorSimple is beschikbaar in twee modellen: de 8010 (voorheen bekend als het model 1100) en de 8020. Het apparaat 8010 heeft een maximale capaciteit van 30 TB. Het apparaat 8020, en profiteert van Azure premium opslag, heeft een maximale capaciteit van 64 TB. (Lokale lagen, Azure premium opslag gegevens worden opgeslagen op SSD dat standaard opslag gegevens op harde schijven worden opgeslagen.) Houd er rekening mee dat er moet een account premie Azure opslag premium opslag gebruiken. Ga naar voor meer informatie over opslag premium [Premium-opslag: krachtige opslag voor een standaardwerkbelasting Azure Virtual Machine](../storage/storage-premium-storage.md).

Ga voor meer informatie over het virtuele apparaat StorSimple naar [implementeren en beheren van een virtueel apparaat StorSimple in Azure](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>StorSimple Manager-service

Microsoft Azure StorSimple biedt een webgebaseerde gebruikersinterface (de StorSimple Manager service) waarmee u kunt Centraal beheer datacenter en opslag van de wolk. De StorSimple Manager-service kunt u de volgende taken uitvoeren:

- Systeeminstellingen voor StorSimple apparaten configureren.
- Configureren en beheren van beveiligingsinstellingen voor StorSimple apparaten.
- Cloud-referenties en eigenschappen configureren.
- Volumes op een server beheren en configureren.
- Volume groepen configureren.
- Back-up en terugzetten van gegevens.
- Prestaties controleren.
- Instellingen controleren en mogelijke problemen.

U kunt de StorSimple Manager-service alle beheerderstaken met uitzondering van die waarvoor het systeem u tijd, zoals de eerste setup en installeren van updates.

Ga naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md)voor meer informatie.

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell voor StorSimple

Windows PowerShell voor StorSimple biedt een opdrachtregelinterface waarmee u kunt maken en de service Microsoft Azure StorSimple beheren en instellen en StorSimple apparaten controleren. Windows PowerShell gebaseerde interface met een opdrachtregel waarin specifieke cmdlets voor het beheren van uw apparaat StorSimple is. Windows PowerShell voor StorSimple bevat functies waarmee u kunt:

- Een apparaat registreren.
- De netwerkinterface op een apparaat configureren.
- Bepaalde soorten updates te installeren.
- Oplossen van problemen met uw apparaat via de support-sessie.
- Wijzig de status van het apparaat.

U kunt toegang krijgen tot Windows PowerShell voor StorSimple van een seriële console (op een hostcomputer rechtstreeks aangesloten op het apparaat) of op afstand met behulp van Windows PowerShell remoting. Houd er rekening mee dat sommige Windows PowerShell voor StorSimple taken, zoals het eerste apparaatregistratie is alleen mogelijk op de seriële console. 

Ga naar [Windows PowerShell gebruiken voor StorSimple voor het beheren van uw apparaat](storsimple-windows-powershell-administration.md)voor meer informatie.

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple cmdlets

De StorSimple van Azure PowerShell-cmdlets zijn een verzameling Windows PowerShell-cmdlets waarmee u kunt het serviceniveau en migratie automatiseren vanaf de opdrachtregel. Ga naar de [cmdlet referentie](https://msdn.microsoft.com/library/dn920427.aspx)voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple.

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager

StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module die u gebruiken kunt voor het maken van consistente, back-ups van lokale point-in-time en cloud gegevens. De module wordt uitgevoerd op een host op basis van Windows Server. U kunt Snapshot Manager StorSimple te gebruiken:

- Configureren, back-up maken en volumes verwijderen.
- Configureer volume groepen om ervoor te zorgen dat back-ups van gegevens toepassing consistent is.
- Back-beleid beheren, zodat de gegevens een back-up op een vooraf bepaald schema en opgeslagen in een bepaalde locatie (lokaal of in de cloud).
- Volumes en afzonderlijke bestanden terugzetten.

Back-ups worden vastgelegd als snapshots, waarmee alleen de wijzigingen vastgelegd sinds de laatste momentopname is gemaakt en veel minder ruimte in dan een volledige back-ups nodig. U kunt back-up schema's maken of onmiddellijk back-ups wanneer dat nodig is. Bovendien kunt u StorSimple Snapshot Manager bewaarbeleid instellen die bepalen hoeveel momentopnamen worden opgeslagen. Als u later gegevens wilt terugzetten vanaf een back-up en StorSimple Snapshot Manager kunt u uit de catalogus van lokale of momentopnamen van de wolk. 

Als een ramp plaatsvindt of als u gegevens wilt terugzetten om een andere reden, weer StorSimple Snapshot Manager geleidelijk nodig is. Gegevens herstellen is niet vereist dat u het hele systeem afsluiten terwijl u een bestand terugzet, apparatuur vervangen of bewerkingen naar een andere site verplaatsen.

Ga voor meer informatie naar [Wat is StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple Adapter voor SharePoint

Microsoft Azure StorSimple bevat de StorSimple Adapter voor SharePoint, een optioneel onderdeel dat transparant wordt uitgebreid StorSimple opslag- en beveiligingsfuncties voor SharePoint server-farms. De adapter werkt met een externe Blob-opslag (RBS)-provider en de Resourcestructuur van SQL Server-functie, zodat u BLOB's verplaatsen naar een server een back-up van het systeem Microsoft Azure StorSimple. Microsoft Azure StorSimple vervolgens slaat de BLOB-gegevens lokaal of in de cloud, op basis van gebruik.

De StorSimple Adapter voor SharePoint wordt beheerd vanuit de portal voor Centraal beheer van SharePoint. Bijgevolg SharePoint beheren blijft gecentraliseerd en alle opslag lijkt te zijn in de SharePoint-farm.

Ga naar [StorSimple Adapter voor SharePoint](storsimple-adapter-for-sharepoint.md)voor meer informatie. 

## <a name="storage-management-technologies"></a>Technologieën voor het beheer van opslag
 
Naast het apparaat met speciale StorSimple virtueel apparaat en andere onderdelen gebruikt Microsoft Azure StorSimple de volgende softwaretechnologieën bieden snelle toegang tot gegevens en opslag worden verlaagd:

- [Automatische opslag trapsgewijs schakelen](#automatic-storage-tiering) 
- [Thin provisioning](#thin-provisioning) 
- [Deduplication en compressie](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatische opslag trapsgewijs schakelen

Microsoft Azure StorSimple worden automatisch gerangschikt van gegevens in logische lagen op basis van huidige gebruik, de leeftijd en de relatie met andere gegevens. Gegevens die het meest actief is, worden lokaal opgeslagen terwijl er minder actieve en inactieve gegevens worden automatisch gemigreerd naar de cloud. In het volgende diagram ziet u deze benadering voor opslag.
 
![StorSimple opslag lagen](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Voor snelle toegang, slaat StorSimple zeer actieve gegevens (hot) op de SSD in het StorSimple apparaat. Opgeslagen gegevens die soms wordt gebruikt (warme gegevens) op de harde schijven in het apparaat of op servers in het datacenter. Gaat de inactieve gegevens, back-upgegevens en gegevens bewaard voor archivering of naleving doeleinden naar de cloud. 

>[AZURE.NOTE] Met 2 of hoger, kunt u een volume als lokaal vastgemaakt, in welk geval de gegevens op het lokale apparaat blijft en niet doorverbonden naar de cloud. 

StorSimple worden aangepast en de gegevens opnieuw worden geordend en toewijzingen als gebruikspatronen opslag wijzigen. Sommige gegevens kan bijvoorbeeld worden minder actief na verloop van tijd. Als het actief wordt geleidelijk minder, gemigreerde van SSD harde schijven en vervolgens naar de cloud. Als dezelfde gegevens opnieuw geactiveerd wordt, wordt deze gemigreerd naar het opslagapparaat.

De opslag trapsgewijs proces verloopt als volgt:

1. Een systeembeheerder stelt in een account van Microsoft Azure cloud opslag.
2. De beheerder gebruikt de seriële console en de StorSimple Manager-service (in de klassieke Azure portal) voor het configureren van de server apparaat- en volumes en gegevens bescherming beleid maken. Machines op ruimten (zoals bestandsservers) met het Internet Small Computer System Interface (iSCSI) toegang tot het apparaat StorSimple.
3. In eerste instantie StorSimple gegevens worden opgeslagen op de snelle SSD-laag van het apparaat.
4. Als de laag SSD capaciteit nadert, StorSimple deduplicates de oudste gegevensblokken worden gecomprimeerd en verplaatst naar het niveau van de harde schijf.
5. Als de capaciteit van harde schijf laag benaderingen, StorSimple de oudste gegevensblokken worden gecodeerd en veilig verzonden naar de account van de Microsoft Azure opslag via HTTPS.
6. Microsoft Azure maakt meerdere kopieën van de gegevens in het datacenter en in een extern datacenter, ervoor te zorgen dat de gegevens kunnen worden hersteld als er zich een ramp voordoet. 
7. De server vraagt om gegevens die zijn opgeslagen in de cloud, StorSimple naadloos wordt geretourneerd als een kopie opgeslagen op de SSD-laag van het apparaat StorSimple.

### <a name="thin-provisioning"></a>Thin provisioning

Thin provisioning is een virtualisatietechnologie waarin beschikbare opslagruimte dan fysieke bronnen worden weergegeven. In plaats van voldoende opslag van tevoren reserveren, StorSimple gebruikt thin provisioning net voldoende ruimte om te voldoen aan de huidige vereisten toe te wijzen. Het elastische karakter van cloud opslag vergemakkelijkt deze benadering omdat StorSimple kunt vergroten of verkleinen van cloud opslag om te voldoen aan veranderende behoeften. 

>[AZURE.NOTE] Lokaal vastgezette volumes zijn niet dun ingericht. Opslag is toegewezen aan een lokaal volume is in zijn geheel ingericht als het volume wordt gemaakt.

### <a name="deduplication-and-compression"></a>Deduplication en compressie

Microsoft Azure StorSimple maakt gebruik van compressie deduplication en gegevens aan de opslagvereisten te verminderen.

Deduplication vermindert het totale bedrag van de gegevens die zijn opgeslagen door het elimineren van de redundantie in de gegevens opgeslagen. Als de gegevens veranderen, StorSimple negeert de gegevens ongewijzigd en wordt alleen de wijzigingen vastgelegd. Bovendien vermindert StorSimple de hoeveelheid opgeslagen gegevens door het identificeren en verwijderen van overbodige gegevens. 

>[AZURE.NOTE] Gegevens op een lokaal vaste volumes is niet deduplicated of gecomprimeerd. Back-ups van volumes lokaal vastgemaakt zijn echter deduplicated en gecomprimeerd.

## <a name="storsimple-workload-summary"></a>StorSimple werkbelasting samenvatting

Hieronder een overzicht van de ondersteunde StorSimple werkbelasting tabelindeling.

| Scenario                  | Werkbelasting                | Ondersteund |  Beperkingen                                  | Versie              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Samenwerking             | Delen van bestanden            | Ja       |                                                | Alle versies         |
| Samenwerking             | Verspreide bestanden delen| Ja       |                                                | Alle versies         |
| Samenwerking             | SharePoint              | Ja *      |Alleen ondersteund met lokaal vastgezette volumes      | Update 2 en hoger   |
| Archivering                  | Eenvoudig bestanden archiveren   | Ja       |                                                | Alle versies         |
| Virtualisatie            | Virtuele machines        | Ja *      |Alleen ondersteund met lokaal vastgezette volumes      | Update 2 en hoger   |
| Database                  | SQL                     | Ja *      |Alleen ondersteund met lokaal vastgezette volumes      | Update 2 en hoger   |
| Videobewaking        | Videobewaking      | Ja *       |Ondersteund wanneer het apparaat StorSimple is gereserveerd voor de werklast| Update 2 en hoger   |
| Back-up                    | Primaire doel back-up | Ja *      |Ondersteund wanneer het apparaat StorSimple is gereserveerd voor de werklast| Update 3 en hoger |
| Back-up                    | Secundaire doel back-up | Ja *      |Ondersteund wanneer het apparaat StorSimple is gereserveerd voor de werklast| Update 3 en hoger |

*Ja & #42; -Oplossing richtlijnen en beperkingen worden toegepast.*

De volgende werklasten worden niet ondersteund door apparaten StorSimple 8000-serie. Als op de StorSimple geïmplementeerd, resulteert deze werkbelasting in een niet-ondersteunde configuratie.

-  Medische beeldvorming
-  Exchange
-  VDI
-  Oracle
-  SAP
-  BIG data
-  Distributie van inhoud
-  Opstarten van SCSI

Hier volgt een lijst met de onderdelen van de infrastructuur StorSimple ondersteund. 

| Scenario | Werkbelasting      | Ondersteund |  Beperkingen                                 | Versie      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Algemeen  | Route Express | Ja       |                                               |Alle versies |
| Algemeen  | FC DataCore   | Ja *       |Ondersteund door DataCore SANsymphony            | Alle versies |
| Algemeen  | DFSR          | Ja *      |Alleen ondersteund met lokaal vastgezette volumes     | Alle versies |
| Algemeen  | Indexeren      | Ja *       |Voor trapsgewijs geschakelde volumes, alleen metagegevens indexering wordt ondersteund (geen gegevens).<br>Voor lokaal vastgezette volumes, wordt volledige indexering ondersteund.| Alle versies |
| Algemeen  | Antivirusprogramma 's    | Ja *       |Voor trapsgewijs geschakelde volumes, wordt alleen scannen bij openen en sluiten ondersteund.<br> Voor lokaal vastgezette volumes, wordt volledige scan ondersteund.| Alle versies |

*Ja & #42; -Oplossing richtlijnen en beperkingen worden toegepast.*

## <a name="storsimple-terminology"></a>StorSimple-terminologie 

Voordat u uw oplossing Microsoft Azure StorSimple implementeert, is het raadzaam de volgende termen en definities te controleren.

### <a name="key-terms-and-definitions"></a>Belangrijkste termen en definities

| De term (acroniem of de afkorting) | Beschrijving |
| ------------------------------ | ---------------- |
| Access control-record (ACR)    | Een record die is gekoppeld aan een volume op het apparaat met Microsoft Azure StorSimple die bepaalt welke hosts verbinding kunnen maken. De bepaling is gebaseerd op de iSCSI Qualified Name (IQN) (die zich in de ACR) hosts die verbinding met uw apparaat StorSimple maakt.|
| AES-256                        | Een 256-bits Advanced Encryption Standard (AES) algoritme voor het coderen van gegevens naar en van de wolk beweegt. |
| clustergrootte (AUS)     | De kleinste hoeveelheid schijfruimte die kan worden toegewezen voor het opslaan van een bestand van uw Windows-bestandssystemen. Als een bestand niet een veelvoud van de clustergrootte is, extra ruimte moet worden gebruikt voor het opslaan van het bestand (tot het volgende veelvoud van de clustergrootte) wat resulteert in verloren ruimte en fragmentatie van de vaste schijf. <br>De aanbevolen AUS voor Azure StorSimple volumes is 64 KB, omdat het werkt goed met de deduplication algoritmen.|
| geautomatiseerde opslag trapsgewijs schakelen      | Minder actieve gegevens automatisch verplaatsen van SSD harde schijven en vervolgens op een laag in de cloud en het beheer van alle opslag vanuit een centrale gebruikersinterface vervolgens inschakelen.|
| back-catalogus | Een collectie van back-ups, meestal van het toepassingstype waarmee is gekoppeld. Deze collectie wordt weergegeven in de catalogus van de back-up van de service Manager StorSimple UI.|
| back-catalogusbestand             | Een bestand met een lijst van beschikbare momentopnamen die momenteel zijn opgeslagen in de back-updatabase van StorSimple Snapshot Manager. |
| back-beleid                   | Een selectie van volumes, het type back-up en een tijdschema waarmee u back-ups maken op een vooraf bepaald schema.|
| binary large objects (BLOB's)    | Een verzameling binaire gegevens als één enkele entiteit worden opgeslagen in een database managementsysteem. BLOB's worden meestal afbeeldingen, audio of andere multimedia-objecten, hoewel soms binaire uitvoerbare code wordt opgeslagen als een BLOB.|
| Challenge Handshake Authentication Protocol (CHAP) | Een protocol dat wordt gebruikt voor het verifiëren van de peer van een verbinding, gebaseerd op de peer een wachtwoord of geheim delen. CHAP kan eenrichtings- of wederzijdse worden. Het doel geverifieerd met CHAP unilaterale, een initiator. Wederzijdse CHAP is vereist dat het doel de aanvrager geverifieerd en dat de initiator het doel verifieert. | 
| klonen                          | Een kopie van een volume. |
|Cloud als een laag (CaaT)          | Cloud opslag als laag binnen de opslagarchitectuur geïntegreerd zodat alle opslag moet deel uitmaken van één onderneming opslagnetwerk verschijnt.|
| cloud serviceprovider (CSP)   | Een aanbieder van cloud computing services.|
| momentopname van de cloud                 | Een point-in-time-kopie van het volumegegevens die zijn opgeslagen in de cloud. Een momentopname van een wolk is gelijk aan een momentopname van een gerepliceerd op een andere, externe opslagsysteem. Wolk momentopnamen zijn met name handig voor herstel na storing scenario's.|
| coderingssleutel cloud-opslag   | Een wachtwoord of sleutel wordt gebruikt in het StorSimple apparaat toegang tot de gecodeerde gegevens door het apparaat wordt verzonden naar de cloud.|
| clusters bijwerken         | Software-updates op de servers in een failover-cluster te beheren zodat de updates bieden echter weinig of geen invloed op de beschikbaarheid van de service.|
| gegevenspad                       | Een collectie van functionele eenheden die onderling gekoppelde gegevens verwerken bewerkingen uitvoeren.|
| deactiveren                     | Een permanente actie die de verbinding tussen het apparaat StorSimple en de bijbehorende cloud-service. Wolk momentopnamen van het apparaat kunnen na dit proces blijven en worden gekloond of gebruikt voor noodherstel.|
| schijf-mirroring                 | Replicatie van logische schijfvolumes op afzonderlijke vaste schijven in real-time naar continue beschikbaarheid te garanderen.|
| dynamische schijf-mirroring         | Replicatie van de logische schijfvolumes op dynamische schijven.|
| dynamische schijven                  | Een indeling van de schijf volume met de Logical Disk Manager (LDM) voor het opslaan en beheren van gegevens op meerdere fysieke schijven. Dynamische schijven kunnen bieden meer ruimte worden vergroot.|
| Behuizing met uitgebreide bundel van schijven (EBOD) | Een secundaire behuizing van uw apparaat met Microsoft Azure StorSimple met extra harde schijf schijven voor extra opslagruimte.|
| FAT-aanbod               | Een conventionele opslag ingericht in welke opslag ruimte wordt toegewezen op basis van de verwachte behoeften (en is meestal buiten de huidige behoefte). Zie ook *thin provisioning*.|
| harde schijf (HDD)          | Een station dat gebruikmaakt van draaiende platen voor het opslaan van gegevens.|
| hybride cloud-opslag           | Een opslagarchitectuur die gebruikmaakt van lokale en externe bronnen, met inbegrip van cloud opslag.|
| Internet Small Computer System Interface (iSCSI) | Een netwerkstandaard van opslag op basis van IP-Internet-Protocol voor het koppelen van gegevens opslagapparatuur of faciliteiten.|
| iSCSI-initiator                 | Een softwareonderdeel waarmee een hostcomputer met Windows verbinding maken met een netwerk van externe iSCSI-opslag.|
| iSCSI Qualified Name (IQN)      | Een unieke naam waarmee een iSCSI-doel of de initiator.|
| iSCSI-doel                    | Een softwareonderdeel dat gecentraliseerde iSCSI schijfsubsystemen in SAN's biedt.|
| Live archiveren                  | Een opslag aanpak waarin gegevens archiveren toegankelijk voortdurend is (deze niet is opgeslagen op een andere locatie op de band, bijvoorbeeld). Microsoft Azure StorSimple wordt live archiveren.|
|lokaal vaste volume | een volume dat zich bevindt op het apparaat en nooit is doorverbonden naar de cloud. |
| lokale momentopname                  | Een point-in-time-kopie van het volumegegevens die zijn opgeslagen op het apparaat met Microsoft Azure StorSimple.|
| Microsoft Azure StorSimple      | Een krachtige oplossing die bestaat uit een datacenter opslagapparatuur en software waarmee IT-organisaties kunnen gebruikmaken van cloud opslag alsof het datacenter opslag. StorSimple vereenvoudigt de bescherming van gegevens en beheer van de kosten verlagen. De oplossing worden geconsolideerd primaire opslag, archiveren, back-up en disaster recovery (DR) via naadloze integratie met de cloud. StorSimple apparaten inschakelen door een combinatie van SAN cloud opslag en het beheer op een platform enterprise-klasse, snelheid, eenvoud en betrouwbaarheid voor al uw opslag-gerelateerde.|
| Voeding en koeling Module (PCM)  | Hardware-onderdelen van het apparaat StorSimple dat bestaat uit de voedingen en de ventilator, vandaar de naam van de voeding en koeling van de module. De primaire behuizing van het apparaat heeft twee 764W, PCMs, terwijl de behuizing van de EBOD twee 580 w bij PCMs heeft.|
| primaire behuizing               | Belangrijkste behuizing van uw apparaat StorSimple met de domeincontrollers application platform.|
| herstel tijd doelstelling (RTO)   | De maximale hoeveelheid tijd die moet worden opgebruikt voordat het systeem of bedrijfsproces is volledig hersteld na een ramp.| 
|serie aangesloten SCSI (SAS)       | Een type harde schijf (HDD).|
| service gegevens coderingssleutel     | Een sleutel is beschikbaar voor de nieuwe StorSimple apparaten die met de StorSimple Manager-service registreert. De configuratiegegevens overgedragen tussen de StorSimple Manager-service en het apparaat is gecodeerd met een openbare sleutel en kan vervolgens alleen op het apparaat met een persoonlijke sleutel worden ontsleuteld. Service gegevens coderingssleutel kan de service te verkrijgen van deze persoonlijke sleutel voor de decodering.|
| sleutel voor de inschrijving        | Een sleutel die helpt bij het StorSimple apparaat registreren bij de StorSimple Manager-service zodat deze wordt weergegeven in Azure klassieke portal voor verdere acties.|
| Small Computer System Interface (SCSI) | Een set normen voor het fysiek verbinden van computers en doorgeven van gegevens.|
| Solid state harde schijf (SSD)         | Een schijf geen bewegende onderdelen bevat; bijvoorbeeld een flashstation.|
| opslag account                 | Een reeks toegangsreferenties gekoppeld aan uw account opslag voor een bepaalde cloud-serviceprovider.| 
| StorSimple Adapter voor SharePoint| Een onderdeel van Microsoft Azure StorSimple die transparant StorSimple opslag- en beveiliging van SharePoint server-farms.|
| StorSimple Manager-service      | Een uitbreiding van de Azure klassieke portal waarmee u uw Azure StorSimple op gebouwen en virtuele apparaten te beheren.|
| StorSimple Snapshot Manager     | Een Microsoft Management Console (MMC)-module voor het beheren van back-up en bewerkingen voor terugzetten in Microsoft Azure StorSimple.|
| back-up maken                     | Een functie waarmee de gebruiker een interactieve back-up van een volume te krijgen. Het is een alternatieve manier van het nemen van een handmatige back-up van een volume in plaats van een geautomatiseerde back-ups via een gedefinieerd beleid nemen.|
| thin provisioning               | Een methode voor het optimaliseren van de efficiëntie waarmee de beschikbare opslagruimte wordt gebruikt in systemen voor opslag. De opslag is in thin provisioning verdeeld over meerdere gebruikers op basis van de minimale ruimte die is vereist voor elke gebruiker op een bepaald moment. Zie ook *fat inrichten*.|
| trapsgewijs schakelen | Rangschikken van gegevens in logische groepen op basis van huidige gebruik, de leeftijd en de relatie met andere gegevens. StorSimple worden automatisch gegevens in lagen.   |
| volume                          | Logische opslaggebieden aangeboden in de vorm van schijven. Volumes StorSimple overeenkomen met de volumes die door de host, met inbegrip van de ontdekt door het gebruik van iSCSI- en een StorSimple apparaat gemonteerd.|
 | volume-container                | Een groepering van volumes en de instellingen die op hen van toepassing. Alle volumes in uw apparaat StorSimple zijn gegroepeerd in volume containers. Volume-instellingen voor container zijn opslag accounts, instellingen voor codering van gegevens die worden verzonden naar cloud met bijbehorende coderingssleutels en bandbreedte voor operaties met betrekking tot de cloud.|
| volume-groep                    | In StorSimple Snapshot Manager een volume groep is een verzameling van volumes die zijn geconfigureerd voor de verwerking van back-up te vergemakkelijken.|
| Volume Shadow Copy Service (VSS)| Een Windows Server-besturingssysteemservice die zorgt voor een consistente toepassing door te communiceren met de VSS-toepassingen voor het maken van incrementele momentopnamen coördineren. VSS zorgt ervoor dat de toepassingen tijdelijk inactief zijn wanneer momentopnamen worden gemaakt.|
| Windows PowerShell voor StorSimple | Op basis van de Windows PowerShell-interface met een opdrachtregel gebruikt om te werken en het beheren van uw apparaat StorSimple. Deze interface heeft terwijl een aantal essentiële mogelijkheden van Windows PowerShell extra speciale cmdlets die zijn gericht op een apparaat StorSimple beheren.|


## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beveiliging in StorSimple](storsimple-security.md).




 

 
