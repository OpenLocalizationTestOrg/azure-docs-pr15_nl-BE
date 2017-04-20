<properties 
   pageTitle="Release-opmerkingen StorSimple virtuele matrix Updates | Microsoft Azure"
   description="Beschrijving van essentiële openstaande problemen en oplossingen voor de StorSimple virtuele matrix 0,3 Update uitgevoerd."
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
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple virtuele matrix Update 0,3 release-opmerkingen

## <a name="overview"></a>Overzicht

De volgende release-opmerkingen geven de kritieke problemen openen en de problemen opgelost voor updates van Microsoft Azure StorSimple virtuele matrix.

De release-opmerkingen worden voortdurend bijgewerkt en wanneer kritieke problemen waarvoor een oplossing worden ontdekt, ze worden toegevoegd. Voordat u uw virtuele matrix StorSimple implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Update 0,3 correspondeert met de software versie **10.0.10288.0**.

> [AZURE.NOTE] Updates zijn storend en start het apparaat opnieuw. Als u I/O worden uitgevoerd, maakt het apparaat uitvaltijd.


## <a name="whats-new-in-the-update-03"></a>Wat is er nieuw in de Update 0,3

0,3-update is hoofdzakelijk een bug fix build. In deze versie zijn zijn verschillende fouten waardoor de back-fouten in de vorige versie gericht.

## <a name="issues-fixed-in-the-update-03"></a>Problemen die zijn opgelost in de Update 0,3

De volgende tabel bevat een overzicht van de problemen die zijn opgelost in deze release.

| Nr.  | Functie                              | Probleem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Back-ups                                |Een probleem werd gezien in de eerdere versie waar de back-ups wilt uitvoeren voor een gedeeld bestand zou mislukken. Dit probleem is opgetreden, de back-uptaak zou mislukken als een kritieke waarschuwing op de StorSimple Manager-service op de hoogte stellen van de gebruiker is verheven. Dit probleem heeft geen invloed op de gegevens op de aandelen of de toegang tot de gegevens. De hoofdoorzaak is geïdentificeerd en opgelost in deze release. <br></br> De correctie geldt niet met terugwerkende kracht aan de aandelen die dit probleem al zien. Klanten die dit probleem zien moeten eerst 0.3 Update toe te passen en vervolgens contact op met Microsoft Support voor het uitvoeren van een volledige back-up u het probleem kunt oplossen. In plaats van contact opneemt met Microsoft Support, klanten kunnen ook naar een nieuwe share een gezonde back-up terugzetten voor de betrokken aandelen.                                                                                                                                                                                 |
| 2    | iSCSI                         | Een probleem werd gezien in de eerdere versie, waar de volumes verdwijnen zou bij het kopiëren van gegevens op een volume op de virtuele matrix StorSimple. Dit probleem is opgelost in deze release. <br></br> De oplossingen van kracht alleen op nieuw gemaakte volumes. De correcties niet van toepassing met terugwerkende kracht op volumes die dit probleem al zien. Klanten wordt aangeraden te doen om de betreffende volumes on line via de klassieke Azure portal en zet deze volumes aan nieuwe volumes een back-up van deze volumes.                                                               |


## <a name="known-issues-in-the-update-03"></a>Bekende problemen in de Update 0,3

In de volgende tabel bevat een overzicht van bekende problemen voor de StorSimple virtuele matrix en de versie vermeld uit de vorige versies problemen bevat. 


| Nr. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | De virtuele apparaten gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie van de algemene beschikbaarheid. | Deze virtuele apparaten moeten worden overgenomen voor de algemene beschikbaarheid met behulp van een werkstroom disaster recovery (DR). |
| **2.** | Ingerichte gegevensschijf | Nadat u een schijf met gegevens van een bepaalde grootte opgegeven hebt ingericht en de bijbehorende StorSimple virtueel apparaat gemaakt, moet u niet uitvouwen of verkleinen de gegevensschijf. U probeert te doen leidt tot een verlies van de gegevens in de lokale lagen van het apparaat. |   |
| **3.** | Groepsbeleid | Wanneer een apparaat verbonden met een domein is, kan toepassen van een Groepsbeleid nadelig beïnvloeden de werking van het apparaat. | Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OU) van Active Directory is en geen groepsbeleidsobjecten (GPO) worden toegepast.|
| **4.** | Lokale web UI | Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige lokale web UI's zoals probleemoplossing of onderhoud niet correct werkt. De knoppen op deze pagina's ook werkt niet. | Verbeterde beveiligingsfuncties in Internet Explorer uitschakelen.|
| **5.** | Lokale web UI | In een Hyper-V virtuele machine, de netwerkinterfaces in het web UI worden weergegeven als 10 Gbps interfaces. | Dit gedrag is een afspiegeling van de Hyper-V. Hyper-V bevat altijd 10 Gbps voor virtuele netwerkadapters. |
| **6.** | Trapsgewijs geschakelde volumes of -shares | Bytebereik vergrendelen voor toepassingen die werken met de StorSimple trapsgewijs geschakelde volumes wordt niet ondersteund. Als byte-bereik vergrendelen is ingeschakeld, werkt StorSimple trapsgewijs schakelen niet. | Aanbevolen maatregelen omvatten: <br></br>Bytebereik vergrendelen in uw toepassingslogica uitschakelen.<br></br>Kies de gegevens voor deze toepassing in een lokaal vaste volumes in plaats van trapsgewijs geschakelde volumes plaatsen.<br></br>*Voorbehoud*: wanneer met behulp van lokale volumes vastgemaakt en byte-bereik vergrendelen is ingeschakeld, het lokaal vaste volume on line kan zijn voordat het terugzetten voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet vervolgens u wachten voor het herstellen te voltooien. |
| **7.** | Gelaagde aandelen | Werken met grote bestanden kan leiden tot trage laag uit. | Als u werkt met grote bestanden, wordt aangeraden dat het grootste bestand kleiner dan 3% van de grootte van de share is. |
| **8.** | Gebruikt capaciteit voor aandelen | U ziet mogelijk verbruik delen als er geen gegevens op de share. Dit komt doordat de gebruikte capaciteit voor shares metagegevens bevat. |   |
| **9.** | Noodherstel | U kunt alleen het herstel van een bestandsserver in hetzelfde domein als dat van het bronapparaat uitvoeren. Herstel na noodgevallen met een doelapparaat in een ander domein wordt niet ondersteund in deze release. | Dit is geïmplementeerd in een latere versie. |
| **10.** | Azure PowerShell | De StorSimple virtuele apparaten kunnen niet worden beheerd via de Azure PowerShell in deze release. | Het beheer van de virtuele apparaten moet worden uitgevoerd via de klassieke Azure portal en het lokale web UI. |
| **11.** | Wachtwoord wijzigen | De virtuele matrix-console apparaat accepteert alleen invoer in en-US toetsenbord indeling. |   |
| **12.** | CHAP | CHAP referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Ook als u de referenties CHAP wijzigt, moet u de volumes off line nemen en brengt u deze on line voor de wijziging door te voeren. | Dit probleem is verholpen in een latere versie. |
| **13.** | iSCSI-server  | De 'gebruikt opslag' weergegeven voor een iSCSI-volume zijn verschillend in de StorSimple Manager-service en de iSCSI-host. | De iSCSI-host heeft de weergave van het bestandssysteem.<br></br>Het apparaat ziet de wanneer de maximale grootte van het volume was toegewezen blokken.|
| **14.** | Bestandsserver  | Als een bestand in een map heeft een alternatieve Data Stream (ADS) gekoppeld, de ADVERTENTIES is geen back-up of via noodherstel, klonen en Item herstelt hersteld.| |


## <a name="next-step"></a>Volgende stap

[0,3-Update installeren](storsimple-ova-install-update-01.md) op uw virtuele matrix StorSimple.

## <a name="references"></a>Verwijzingen

Op zoek naar een oudere release-notitie? Ga naar: 

- [StorSimple virtuele matrix Update 0,1-0,2 Release-opmerkingen](storsimple-ova-update-01-release-notes.md)

- [StorSimple virtuele matrix algemene beschikbaarheid Release-opmerkingen](storsimple-ova-pp-release-notes.md)
 

