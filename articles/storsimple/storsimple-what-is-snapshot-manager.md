<properties 
   pageTitle="Wat is StorSimple Snapshot Manager? | Microsoft Azure"
   description="Beschrijving van de StorSimple Snapshot Manager, de architectuur en de functies."
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

# <a name="what-is-storsimple-snapshot-manager"></a>Wat is StorSimple Snapshot Manager?

## <a name="overview"></a>Overzicht

StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module die de bescherming van gegevens en back-up beheren in een omgeving met Microsoft Azure StorSimple vereenvoudigt. Met StorSimple Snapshot Manager beheren Microsoft Azure StorSimple data in het datacenter en de cloud als een geïntegreerde opslagoplossing één dus back-upprocessen te vereenvoudigen en de kosten te verlagen.

Dit overzicht introduceert de StorSimple Snapshot Manager, worden de functies beschreven en zijn rol in Microsoft Azure StorSimple wordt uitgelegd. 

Zie voor een overzicht van het volledige Microsoft Azure StorSimple systeem, met inbegrip van het apparaat StorSimple, StorSimple Manager-service StorSimple Snapshot Manager en StorSimple Adapter voor SharePoint, [StorSimple 8000-serie: een hybride cloud storage oplossing](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- U kunt StorSimple Snapshot Manager niet gebruiken voor het beheren van Microsoft Azure StorSimple virtuele matrices (ook bekend als StorSimple op-premises virtuele apparaten).
>
>- Als u van plan bent om StorSimple 2 hebt geïnstalleerd op uw apparaat StorSimple, moet u de meest recente versie van StorSimple Snapshot Manager te downloaden en installeren **voordat u StorSimple 2 hebt geïnstalleerd**. De meest recente versie van StorSimple Snapshot Manager is achterwaarts compatibel en werkt met alle versies van Microsoft Azure StorSimple. Als u de vorige versie van StorSimple Snapshot Manager gebruikt, moet u te werken (u hoeft niet naar de vorige versie verwijderen voordat u de nieuwe versie installeert).

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>Doel van de StorSimple Snapshot Manager en architectuur

StorSimple Snapshot Manager biedt een centrale beheerconsole, die u gebruiken kunt voor het maken van consistente, back-ups van lokale point-in-time en cloud gegevens. U kunt bijvoorbeeld de console te gebruiken:

- Configureren, back-up maken en volumes verwijderen.
- Configureer volume groepen om ervoor te zorgen dat back-ups van gegevens toepassing consistent is.
- Back-beleid beheren zodat u de back-up op een vooraf bepaald schema.
- Maak lokale en momentopnamen die kunnen worden opgeslagen in de cloud en gebruikt voor het herstel van de wolk.

De StorSimple Snapshot Manager ophaalt uit de lijst met toepassingen die zijn geregistreerd met de VSS-provider op de host. Vervolgens toepassing consistent back-ups maken, controleert de volumes die door een toepassing worden gebruikt en stelt volume groepen configureren. Deze groepen volume StorSimple Snapshot Manager gebruikt voor het genereren van back-ups die consistent zijn met toepassing. (Toepassing consistentie bestaat wanneer alle bestanden gerelateerde en databases worden gesynchroniseerd en de werkelijke status van de toepassing op een bepaald punt in de tijd vertegenwoordigen.) 

Back-ups StorSimple Snapshot Manager de vorm aannemen van incrementele snapshots, waarmee alleen de wijzigingen vast te sinds de laatste back-up leggen. Als gevolg hiervan kunnen back-ups minder opslagruimte nodig en worden gemaakt en snel hersteld. Windows Volume Shadow Copy Service (VSS) StorSimple Snapshot Manager gebruikt om ervoor te zorgen dat momentopnamen toepassing consistente gegevens vastleggen. (Voor meer informatie gaat u naar de integratie met Volume Shadow Copy-Service Windows-sectie.) Met StorSimple Snapshot Manager kunt u back-up schema's maken of direct back-ups uitvoeren indien nodig. Als u gegevens wilt terugzetten vanaf een back-up en StorSimple Snapshot Manager kunt u uit een catalogus van lokale of momentopnamen van de wolk. Azure StorSimple herstelt alleen de gegevens die nodig zijn als nodig is, die voorkomt vertragingen in de beschikbaarheid van gegevens tijdens de bewerking voor terugzetten dat).

![StorSimple Snapshot Manager architectuur](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot Manager architectuur** 

## <a name="support-for-multiple-volume-types"></a>Ondersteuning voor meerdere volumetypen

U kunt de StorSimple Snapshot Manager configureren en een back-up van de volgende soorten volumes: 

- **Standaardvolumes** – een standaardvolume is één partitie op een standaardschijf. 

- **Eenvoudige volumes** – een eenvoudig volume is een dynamisch volume met de schijfruimte op één dynamische schijf. Een eenvoudig volume bestaat uit een enkele regio op een schijf of meerdere regio's die aan elkaar zijn gekoppeld op dezelfde schijf. (Kunt u eenvoudige volumes alleen op dynamische schijven.) Eenvoudige volumes zijn niet fouttolerant.

- **Dynamische volumes** : een dynamisch volume is een volume op een dynamische schijf worden gemaakt. Een database dynamische schijven gebruiken voor het bijhouden van informatie over volumes die zich bevinden op dynamische schijven in een computer. 

- **Dynamische volumes spiegelen** : dynamische volumes spiegelen zijn op de RAID 1-architectuur gebouwd. Met RAID-1 geschreven identieke gegevens op twee of meer schijf, een gespiegelde set produceren. Een leesbevestiging kan vervolgens worden verwerkt door een schijf met de gevraagde gegevens.

- **Cluster gedeelde clustervolumes** – met gedeelde volumes (CSVs), meerdere knooppunten in een failover-cluster kunnen gelijktijdig lezen of te schrijven op dezelfde schijf. Failover van één knooppunt naar een ander knooppunt kan snel optreden zonder dat een wijziging in de eigendom van station montage, ontkoppelen of verwijderen van een volume. 

>[AZURE.IMPORTANT] Niet mengen met CSVs en CSVs in de dezelfde momentopname. CSVs en CSVs in een momentopname van een vermenging wordt niet ondersteund. 
 
StorSimple Snapshot Manager kunt u groepen van gehele volume terugzetten of afzonderlijke volumes klonen en afzonderlijke bestanden herstellen.

- [Volumes en volume groepen](#volumes-and-volume-groups) 
- [Typen back-up en back-beleid](#backup-types-and-backup-policies) 

Zie voor meer informatie over StorSimple Snapshot Manager-functies en hoe ze te gebruiken [gebruikersinterface StorSimple Snapshot Manager](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Volumes en volume groepen

Met StorSimple Snapshot Manager, volumes maken en deze vervolgens in volume groepen configureren. 

Volume groepen StorSimple Snapshot Manager gebruikt voor het maken van back-ups die toepassing consistent zijn. Toepassing consistentie bestaat wanneer alle bestanden gerelateerde en databases worden gesynchroniseerd en de werkelijke status van een toepassing op een bepaald punt in de tijd vertegenwoordigen. Volume groepen (die ook wel bekend als *groepen consistentie*) vormen de basis van een back-up of terugzetten.

Volume groepen zijn niet hetzelfde als containers voor volume. Een container volume bevat een of meer volumes die een cloud opslag en andere kenmerken, zoals codering en bandbreedte verbruik delen. Een container één volume kan maximaal 256 dun ingerichte StorSimple volumes bevatten. Voor meer informatie over het volume, containers, gaat u naar [de volume-containers beheren](storsimple-manage-volume-containers.md). Volume groepen zijn verzamelingen van volumes die u kunt configureren om back-ups vergemakkelijken. Als u twee volumes die behoren tot een ander volume, containers en maak vervolgens een back-up beleid voor die groep volume plaatst in een enkel volume groep selecteert, wordt elk volume reservekopie in het juiste volume-container, met behulp van de account de juiste opslag.

>[AZURE.NOTE] Alle volumes in een groep van het volume moeten afkomstig zijn uit een enkele cloud-serviceprovider.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integratie met Windows Volume Shadow Copy-Service

StorSimple Snapshot Manager gebruikt Windows Volume Shadow Copy Service (VSS) toepassing consistente gegevens vastleggen. VSS zorgt voor een consistente toepassing door te communiceren met de VSS-toepassingen voor het maken van incrementele momentopnamen coördineren. VSS zorgt ervoor dat de toepassingen tijdelijk inactief of quiescent, wanneer momentopnamen worden gemaakt. 

De uitvoering van de StorSimple Snapshot Manager van VSS werkt met SQL Server en algemene NTFS-volumes. Het proces is als volgt: 

1. Een aanvrager, die meestal een gegevensbeheer en gegevensbescherming (zoals StorSimple Snapshot Manager) of een back-uptoepassing, roept VSS en wordt u gevraagd deze informatie te verzamelen van de software van de schrijver in de doeltoepassing.

2. VSS contact op met de schrijver-component voor het ophalen van een beschrijving van de gegevens. De schrijver resulteert in de beschrijving van de gegevens op de back-up worden gemaakt. 

3. VSS geeft de schrijver aan de voorbereiding van de toepassing voor back-up. De schrijver zorgt ervoor dat de gegevens voor de back-up door openstaande transacties bijwerken van transactielogboeken, enzovoort, en wordt een bericht verzonden VSS.

4. VSS geïnstrueerd de schrijver tijdelijk stoppen van de opgeslagen gegevens van de toepassing en zorg ervoor dat er geen gegevens worden weggeschreven naar het volume terwijl de schaduwkopie wordt gemaakt. Deze stap zorgt voor consistentie van de gegevens en niet langer duurt dan 60 seconden.

5. VSS Hiermee geeft u de voorziening voor het maken van de schaduwkopie. Leveranciers van software of hardware gebaseerde, kunnen zijn beheren de volumes die momenteel actief zijn en maken van kopieën van deze op aanvraag. De provider de schaduwkopie wordt gemaakt en VSS wordt gewaarschuwd wanneer deze is voltooid.

6. VSS contact op met de schrijver op de hoogte stellen van de toepassing die I/O kan hervatten en om te bevestigen dat I/O is onderbroken tijdens het maken van schaduwkopieën. 

7. Als de kopie gelukt is, retourneert VSS locatie van de kopie voor de aanvrager. 

8. Als gegevens is geschreven terwijl de schaduwkopie is gemaakt, wordt de back-up niet consistent zijn. VSS de schaduwkopie wordt verwijderd en wordt de aanvrager. De aanvrager kan het back-upproces automatisch herhalen of contact met de netwerkbeheerder om te proberen deze op een later tijdstip te.

Zie de volgende afbeelding.

![VSS-proces](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Volume Shadow Copy-Service Windows-proces** 

## <a name="backup-types-and-backup-policies"></a>Typen back-up en back-beleid

Met StorSimple Snapshot Manager kunt u een back-up van gegevens en sla deze lokaal en in de cloud. U kunt StorSimple Snapshot Manager onmiddellijk een back-up van gegevens, of kunt u een beleid voor back-up maken van een schema voor het automatisch maken van back-ups. Back-beleid kunnen u opgeven hoeveel momentopnamen wordt bewaard. 

### <a name="backup-types"></a>Back-uptypen

StorSimple Snapshot Manager kunt u de volgende soorten back-ups te maken:

- **Lokale momentopnamen** – lokale momentopnamen zijn point-in-time kopieën van gegevens die zijn opgeslagen op het apparaat StorSimple op volume. Doorgaans worden dit type back-up gemaakt en snel hersteld. Als u een lokale back-up kunt u een momentopname van een lokale gebruiken.

- **Momentopnamen van de wolk** – Cloud momentopnamen zijn point-in-time kopieën van volumegegevens die zijn opgeslagen in de cloud. Een momentopname van een wolk is gelijk aan een momentopname van een gerepliceerd op een andere, externe opslagsysteem. Wolk momentopnamen zijn met name handig voor herstel na storing scenario's.

### <a name="on-demand-and-scheduled-backups"></a>Op verzoek en geplande back-ups

U kunt eenmalige back-up wordt gemaakt onmiddellijk starten met StorSimple Snapshot Manager of kunt u een back-up beleid periodiek back-ups plannen.

Een back-up beleid is een reeks geautomatiseerde regels die u gebruiken kunt om regelmatig back-ups. Een back-up beleid kunt u de frequentie en de parameters voor het maken van momentopnamen van een specifiek volume groep definiëren. U kunt beleid opgeven en vervaldatum, tijden, frequenties en bewaarplicht van lokale en momentopnamen van de wolk. Een beleid wordt toegepast, onmiddellijk nadat u deze hebt gedefinieerd. 

StorSimple Snapshot Manager kunt u configureren of opnieuw configureren van back-up beleid indien nodig. 

U configureert de volgende informatie voor elk back-beleid dat u maakt:

- **Naam** : de unieke naam van het geselecteerde back-beleid.

- **Type** – het type back-up beleid; lokale snapshot of momentopname van de wolk.

- **Volume groep** – de groep van het volume waaraan het geselecteerde back-beleid is toegewezen.

- **Bewaren** : het aantal back-ups te behouden. Als u het selectievakje **alle** , worden alle back-ups bewaard totdat het maximum aantal back-up kopieën per volume is bereikt, waarna het beleid voor en wordt er een foutbericht weergegeven. U kunt ook opgeven dat een aantal back-ups te behouden (tussen 1 en 64).

- **Datum** : de datum waarop het beleid van de back-up is gemaakt.

Ga voor meer informatie over het configureren van back-beleid naar [Gebruik StorSimple Snapshot Manager maken en beheren van back-beleid](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Beheer en bewaking van de back-uptaak

U kunt de StorSimple Snapshot Manager bewaken en beheren van back-uptaken voor aanstaande gepland en voltooid. Bovendien biedt StorSimple Snapshot Manager een catalogus van maximaal 64 voltooide back-ups. U kunt de catalogus zoeken en herstellen van volumes of afzonderlijke bestanden. 

Ga voor meer informatie over het controleren van back-uptaken [Gebruik StorSimple Snapshot](storsimple-snapshot-manager-manage-backup-jobs.md)Manager weergeven en beheren van back-uptaken.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple Snapshot Manager voor het beheren van uw oplossing StorSimple](storsimple-snapshot-manager-admin.md).

- [StorSimple-Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220)downloaden.
