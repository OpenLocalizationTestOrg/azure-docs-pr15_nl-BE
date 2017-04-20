<properties 
   pageTitle="Release-opmerkingen StorSimple virtuele matrix Updates | Microsoft Azure"
   description="Beschrijving van essentiële openstaande problemen en oplossingen voor de StorSimple virtuele matrix met Update 0,2 en 0,1."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple virtuele matrix Update 0,2 en 0.1 release-opmerkingen

## <a name="overview"></a>Overzicht

De volgende release-opmerkingen geven de kritieke problemen openen en de problemen opgelost voor updates van Microsoft Azure StorSimple virtuele matrix. (Microsoft Azure StorSimple virtuele matrix is ook bekend als het virtuele apparaat voor StorSimple in ruimten of het virtuele apparaat StorSimple.) 

De release-opmerkingen worden voortdurend bijgewerkt en wanneer kritieke problemen waarvoor een oplossing worden ontdekt, ze worden toegevoegd. Voordat u het virtuele apparaat StorSimple implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Update 0.2 correspondeert met de software versie **10.0.10280.0**; Update 0,1 is versie **10.0.10279.0**. In de volgende secties worden de wijzigingen voor elke update vermeld. 

> [AZURE.NOTE] Updates zijn storend en het apparaat opnieuw gestart. Als I/O uitgevoerd worden, wordt het apparaat uitvaltijd oplopen.

## <a name="issues-fixed-in-the-update-02"></a>Problemen die zijn opgelost in de Update van 0,2
0,2 update bevat alle wijzigingen van 0,1 Update behalve de correctie die is beschreven in de volgende tabel:

Functie                              | Probleem                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Updates                                 | In de laatste versie zijn niet updates automatisch gedetecteerd in de klassieke Azure portal, zodat u moest het lokale Web UI gebruiken om updates te installeren. Dit probleem is opgelost in deze release. Na het installeren van Update 0,2 kunt u met de klassieke Azure portal van toekomstige updates installeren.                       

## <a name="whats-new-in-the-update-01"></a>Wat is er nieuw in de Update van 0,1

0,1 update bevat de volgende correcties en verbeteringen. 

- **Verbeterde herstellingsvermogen voor cloud storingen**: deze versie heeft verscheidene bugfixes rond noodherstel, back-up, terugzetten en trapsgewijs schakelen in geval van een verstoring van de wolk connectiviteit. 

- **Verbeterde prestaties herstellen**: deze release bevat correcties die u hebt het tijdstip van de voltooiing van de terugzettaken aanzienlijk verminderen.

- **Automatische ruimte terugwinning optimalisatie**: wanneer gegevens worden verwijderd op dun ingerichte volumes, de opslag van ongebruikte blokken moeten terug te vorderen. Deze release is de ruimte regeneratie proces uit de cloud waardoor de ongebruikte ruimte wordt sneller beschikbaar in vergelijking met de vorige versies worden verbeterd.

- **Nieuwe virtuele schijf afbeeldingen**: nieuwe VHD VHDX en VMDK zijn nu beschikbaar via de klassieke Azure-portal. U kunt deze afbeeldingen te creëren nieuwe Update 0,1 apparaten downloaden.

- **Verbetering van de nauwkeurigheid van de status van taken in de portal**: In de eerdere versie van de software, taakstatus melden in de portal is niet nauwkeurig. Dit probleem is opgelost in deze release.

- **Aan domein toevoegen ondervinden**: correcties in verband met lid worden van een domein en naam van het apparaat.


## <a name="issues-fixed-in-the-update-01"></a>Problemen die zijn opgelost in de Update van 0,1

De volgende tabel bevat een overzicht van de problemen die zijn opgelost in deze release.

| Nr.  | Functie                              | Probleem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | In sommige versies van VMware, is de schijf OS gezien als sparse waarschuwingen veroorzaken en die de normale werking te onderbreken. Dit is opgelost in deze release.                                                                                                                                                                                    |
| 2    | iSCSI-server                         | De gebruiker is in de laatste versie vereist een gateway voor elke ingeschakelde netwerkinterface van uw StorSimple virtueel apparaat opgeven. Dit gedrag is gewijzigd in deze release, zodat de gebruiker heeft ten minste één gateway voor alle ingeschakelde netwerkinterfaces configureren.                                                                              |
| 3    | Support-pakket                      | Ondersteuning in de eerdere versie van de software pakket collectie is mislukt wanneer de grootte van het pakket groter dan 1 GB is. Dit probleem is opgelost in deze release.                                                                                                                                                                               |
| 4    | Toegang tot de cloud                         |  In de laatste versie als de virtuele matrix StorSimple heeft geen verbinding met het netwerk opnieuw is gestart, er de gebruikersinterface van lokale verbindingsproblemen. Dit probleem is opgelost in deze release.                                                                                                                            |
| 5    | Monitoring van grafieken                    | De cloud capaciteit gebruik grafieken weergegeven in de eerdere versie, na een failover apparaat onjuiste waarden in de klassieke Azure portal. Dit probleem wordt opgelost in de huidige versie.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Bekende problemen in de Update van 0,1

In de volgende tabel bevat een overzicht van bekende problemen voor de StorSimple virtuele matrix en de versie vermeld uit de vorige versies problemen bevat. **De release van de problemen vermeld in deze release zijn gemarkeerd met een sterretje. Bijna alle problemen in deze lijst hebben overgedragen van de NH-release van virtuele matrix StorSimple.**


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
| **14.** | Bestand server *  | Als een bestand in een map heeft een alternatieve Data Stream (ADS) gekoppeld, de ADVERTENTIES is geen back-up of via noodherstel, klonen en Item herstelt hersteld.| |


## <a name="next-step"></a>Volgende stap

[Updates installeren](storsimple-ova-install-update-01.md) op uw virtuele matrix StorSimple.
