<properties 
   pageTitle="StorSimple systeem grenzen | Microsoft Azure"
   description="Beschrijving van systeem grenzen en aanbevolen formaten voor StorSimple 8000-serie onderdelen en verbindingen."
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

# <a name="storsimple-system-limits"></a>StorSimple systeem grenzen

## <a name="overview"></a>Overzicht

StorSimple biedt schaalbare en flexibele opslag voor uw datacenter. Er zijn echter enkele beperkingen die Houd er rekening mee als u plannen, distribueren en gebruiken van uw oplossing StorSimple. De volgende tabel worden deze limieten beschreven en enkele aanbevelingen zodat u uit uw StorSimple oplossing halen kunt.

| Limiet-ID | Limiet | Opmerkingen |
|----------------- | ------|--------- |
| Maximum aantal opslag referenties | 64 | |
| Maximum aantal containers volume | 64 | |
| Maximum aantal volumes | 255 | |
| Maximum aantal volumes lokaal vastgemaakt | 32 | |
| Maximum aantal schema's per sjabloon bandbreedte | 168 | Een schema voor elk uur, elke dag van de week (24 * 7). |
| Maximale grootte van het volume van een gelaagde op fysieke apparaten | 64 TB voor 8100 en 8600 | 8100 en 8600 zijn fysieke apparaten. |
| Maximale grootte van het volume van een gelaagde op virtuele apparaten in Azure | 30 TB voor 8010 <br></br> 64 TB voor 8020 | 8010 en 8020 zijn virtuele apparaten in Azure, die respectievelijk gebruikmaken van standaardopslag en opslag van de premie. |
| Maximale grootte van het volume van een lokaal vastgehouden op fysieke apparaten | 8,5 TB voor 8100 <br></br> 22,5 TB voor 8600 | 8100 en 8600 zijn fysieke apparaten. |
| Maximum aantal iSCSI-verbindingen | 512 | |
| Maximum aantal verbindingen iSCSI-initiators | 512 | |
| Maximum aantal records uit access control per apparaat | 64 | |
| Maximum aantal volumes per back-beleid | 24 | |
| Maximum aantal back-ups bewaard per schema (in een back-up beleid) | 64 | |
| Maximum aantal schema's per back-beleid | 10 | |
| Maximum aantal momentopnamen van elk type dat per volume kunnen worden bewaard | 256 | Dit aantal bevat ook lokale momentopnamen en momentopnamen van de wolk. |
| Maximum aantal momentopnamen die kunnen voorkomen in elk apparaat | 10.000 | |
| Maximum aantal volumes die kunnen worden verwerkt voor back-up en terugzetten of klonen | 16 |<ul><li>Als er meer dan 16 volumes, ze sequentieel verwerkt verwerking sleuven beschikbaar komen.</li><li>Nieuwe back-ups van een gekloonde of een gelaagde teruggezette volume kan niet plaatsvinden totdat de bewerking is voltooid. Voor een lokaal volume zijn back-ups echter toegestaan nadat het volume on line is.</li></ul>|
| Terugzetten en klonen herstellen voor trapsgewijs geschakelde volumes | < 2 minuten | <ul><li>Het volume wordt beschikbaar gesteld binnen twee minuten na de bewerking voor terugzetten of klonen, ongeacht de grootte van het volume.</li><li>Het volume kan in eerste instantie worden langzamer dan normaal, zoals de meeste van de gegevens en metagegevens woont nog steeds in de cloud. Prestaties kan als gegevensstromen naar het apparaat StorSimple vanuit de cloud verhogen.</li><li>De totale tijd voor het downloaden van metagegevens is afhankelijk van de grootte van het volume toegewezen. Metagegevens in het apparaat op de achtergrond met een snelheid van 5 minuten per TB aan volumegegevens toegewezen automatisch binnengehaald. Dit percentage kan worden beïnvloed door de Internet-bandbreedte naar de cloud.</li><li>Het terugzetten of de kopieerbewerking is voltooid wanneer alle metagegevens op het apparaat is.</li><li>Pas het herstellen van de back-upbewerkingen uitvoeren of klonen volledig voltooid is.|
| Terugzetten tijd voor lokaal vastgezette volumes herstellen | < 2 minuten | <ul><li>Het volume wordt beschikbaar gesteld binnen twee minuten na het terugzetten, ongeacht de grootte van het volume.</li><li>Het volume kan in eerste instantie worden langzamer dan normaal, zoals de meeste van de gegevens en metagegevens woont nog steeds in de cloud. Prestaties kan als gegevensstromen naar het apparaat StorSimple vanuit de cloud verhogen.</li><li>De totale tijd voor het downloaden van metagegevens is afhankelijk van de grootte van het volume toegewezen. Metagegevens in het apparaat op de achtergrond met een snelheid van 5 minuten per TB aan volumegegevens toegewezen automatisch binnengehaald. Dit percentage kan worden beïnvloed door de Internet-bandbreedte naar de cloud.</li><li>In tegenstelling tot de trapsgewijs geschakelde volumes, voor lokaal vastgezette volumes, het volumegegevens worden ook gedownload lokaal op het apparaat. De bewerking voor terugzetten is voltooid wanneer alle gegevens op het volume is ingesteld op het apparaat.</li><li>De bewerking voor terugzetten is lang. De totale tijd voor het voltooien van de terugzetbewerking is afhankelijk van de grootte van het ingerichte lokale volume, de Internet-bandbreedte en de bestaande gegevens op het apparaat. Back-ups op de lokale vaste volume zijn toegestaan tijdens de bewerking voor terugzetten.|
|Snelheid verwerking voor momentopnamen van de cloud| 15 minuten/TB | <ul><li>Minimale tijd om aan de cloud snapshot gereed voor uploaden per TB aan volumegegevens in de back-up toegewezen. </li><li> Totale cloud momentopname tijd wordt berekend door deze tijd toe te voegen aan de momentopname uploadtijd wordt beïnvloed door de Internet-bandbreedte cloud.
| Maximum aantal clients lezen/schrijven doorvoer (van de SSD laag bediend) * | 920/720 MB/s met een enkele 10 GbE-netwerkinterface | Tot 2 x met MPIO en twee netwerkinterfaces. |
| Maximum aantal clients lezen/schrijven doorvoer (van de harde schijf laag bediend) * | 120/250 MB/s |
| Maximum aantal clients lezen/schrijven doorvoer (van de wolk laag bediend)* Update 3 en hoger** | 40/60 MB/s voor gelaagde volumes<br><br>60/80 MB/s voor volumes doorverbonden met archivering optie geselecteerd tijdens het maken van een volume | Lezen doorvoer hangt af van de clients genereren en bijhouden van voldoende i/o-wachtrijdiepte. <br><br>Snelheid is bereikt, is afhankelijk van de snelheid van de onderliggende opslag account gebruikt. | 

& #42; Maximale doorvoer per type i/o-werd gemeten met 100 procent lezen en schrijven van 100 procent-scenario's. Werkelijke doorvoer mogelijk lager en afhankelijk van I/O mengen en voorwaarden het netwerk.

& #42; & #42; Prestatiegegevens voor Update 3 mogelijk lager.

## <a name="next-steps"></a>Volgende stappen

Controleer de [systeemvereisten voor StorSimple](storsimple-system-requirements.md). 
