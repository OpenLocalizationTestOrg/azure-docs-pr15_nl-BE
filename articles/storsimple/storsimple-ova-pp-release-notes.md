<properties 
   pageTitle="Release-opmerkingen voor virtuele matrix StorSimple | Microsoft Azure"
   description="Beschrijving van essentiële openstaande problemen en oplossingen voor de virtuele matrix StorSimple."
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
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>Virtuele matrix StorSimple release-opmerkingen

## <a name="overview"></a>Overzicht

De volgende release-opmerkingen geven de essentiële openstaande kwesties voor de release van maart 2016 algemene beschikbaarheid (GA) van de Microsoft Azure StorSimple virtuele matrix (ook bekend als het virtuele apparaat voor StorSimple in ruimten of het virtuele apparaat StorSimple). Deze versie overeenkomt met softwareversie 10.0.10271.0.

De release-opmerkingen worden voortdurend bijgewerkt en wanneer kritieke problemen waarvoor een oplossing worden ontdekt, ze worden toegevoegd. Voordat u het virtuele apparaat StorSimple implementeert, zorgvuldig door de informatie in de release-opmerkingen. 

De volgende tabel bevat een overzicht van bekende problemen in deze release.


| Nr. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | De virtuele apparaten gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie van de algemene beschikbaarheid. | Deze virtuele apparaten moeten worden overgenomen voor de algemene beschikbaarheid met behulp van een werkstroom disaster recovery (DR). |
| **2.** | Ingerichte gegevensschijf | Nadat u een schijf met gegevens van een bepaalde grootte opgegeven hebt ingericht en de bijbehorende StorSimple virtueel apparaat gemaakt, moet u niet uitvouwen of verkleinen de gegevensschijf. Tracht te doen leidt tot een verlies van de gegevens in de lokale lagen van het apparaat. |   |
| **3.** | Groepsbeleid | Wanneer een apparaat verbonden met een domein is, kan toepassen van een Groepsbeleid nadelig beïnvloeden de werking van het apparaat. | Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OU) van Active Directory is en geen groepsbeleidsobjecten (GPO) worden toegepast.|
| **4.** | Lokale web UI | Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige lokale web UI's zoals probleemoplossing of onderhoud niet correct werkt. De knoppen op deze pagina's ook werkt niet. | Verbeterde beveiligingsfuncties in Internet Explorer uitschakelen.|
| **5.** | Lokale web UI | In een Hyper-V virtuele machine, de netwerkinterfaces in het web UI worden weergegeven als 10 Gbps interfaces. | Dit gedrag is een afspiegeling van de Hyper-V. Hyper-V bevat altijd 10 Gbps voor virtuele netwerkadapters. |
| **6.** | Trapsgewijs geschakelde volumes of -shares | Bytebereik vergrendelen voor toepassingen die werken met de StorSimple trapsgewijs geschakelde volumes wordt niet ondersteund. Als byte-bereik vergrendelen is ingeschakeld, werkt StorSimple trapsgewijs schakelen niet. | Aanbevolen maatregelen omvatten: <br></br>Bytebereik vergrendelen in uw toepassingslogica uitschakelen.<br></br>Kies de gegevens voor deze toepassing in een lokaal vaste volumes in plaats van trapsgewijs geschakelde volumes plaatsen.<br></br>*Voorbehoud*: als volumes met behulp van lokaal vastgemaakt en byte-bereik vergrendelen is ingeschakeld, moet u zich realiseren dat het lokaal vaste volume kunt online zelfs voordat de herstelbewerking voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet vervolgens u wachten voor het herstellen te voltooien. |
| **7.** | Gelaagde aandelen | Werken met grote bestanden kan leiden tot trage laag uit. | Als u werkt met grote bestanden, wordt aangeraden dat het grootste bestand kleiner dan 3% van de grootte van de share is. |
| **8.** | Gebruikt capaciteit voor aandelen | U ziet mogelijk verbruik in de afwezigheid van alle gegevens op de share delen. Dit komt doordat de gebruikte capaciteit voor shares metagegevens bevat. |   |
| **9.** | Noodherstel | U kunt alleen het herstel van een bestandsserver in hetzelfde domein als dat van het bronapparaat uitvoeren. Herstel na noodgevallen met een doelapparaat in een ander domein wordt niet ondersteund in deze release. | Dit zal worden geïmplementeerd in een latere versie. |
| **10.** | Azure PowerShell | De StorSimple virtuele apparaten kunnen niet worden beheerd via de Azure PowerShell in deze release. | Het beheer van de virtuele apparaten moet worden uitgevoerd via de klassieke Azure portal en het lokale web UI. |
| **11.** | Wachtwoord wijzigen | De virtuele matrix-console apparaat accepteert alleen invoer in en-US toetsenbord indeling. |   |
| **12.** | CHAP | CHAP referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Ook als u de referenties CHAP wijzigt, moet u de volumes off line nemen en brengt u deze on line voor de wijziging door te voeren. | Dit wordt opgelost in een latere versie. |
| **13.** | iSCSI-server  | De 'gebruikt opslag' weergegeven voor een iSCSI-volume zijn verschillend in de StorSimple Manager-service en de iSCSI-host. | De iSCSI-host heeft de weergave van het bestandssysteem.<br></br>Het apparaat ziet de wanneer de maximale grootte van het volume was toegewezen blokken.|
