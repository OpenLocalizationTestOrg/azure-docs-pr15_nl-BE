<properties 
   pageTitle="StorSimple 8000 Update 0,2 release notes | Microsoft Azure"
   description="Beschrijving van de nieuwe functies en verbeteringen, openstaande kwesties en oplossingen beschikbaar voor het januari 2015 Microsoft Azure StorSimple release (0.2 Update)."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000-serie Update 0,2 Releaseopmerkingen - januari 2015

## <a name="overview"></a>Overzicht

De volgende release identificeren de essentiële openstaande kwesties voor januari 2015 release van Microsoft Azure StorSimple. Ze bevatten ook een lijst van de StorSimple-software en firmware updates zijn opgenomen in deze release. Dit is de tweede versie na de StorSimple 8000-serie versie beschikbaar is gesteld in het algemeen in juli 2014.
  
Deze update wijzigt niet het fysieke apparaat softwareversie van de update van oktober. Het blijft versie 6.3.9600.17312. De afbeelding die wordt gebruikt door de afbeelding virtueel apparaat is gewijzigd in deze release. Daarom wordt alle nieuwe virtuele apparaten gemaakt na 20-1/2015 de versie weergegeven als 6.3.9600.17361.  

Bekijk de volgende informatie in de release-opmerkingen voor de update van januari 2015.

> [AZURE.IMPORTANT]  
>
>- Deze update is niet beschikbaar via Windows Update en net als bij andere updates kan niet worden geïnstalleerd. Het apparaat ontvangt deze update ook als u de updates hebt toegepast met behulp van de klassieke Azure portal. Deze update is alleen van toepassing op virtuele apparaten gemaakt na 20 januari 2015. 
> 
>- De januari-release van StorSimple bevat geen updates van het fysieke apparaat StorSimple. U kunt alle beschikbare updates voor Windows nog steeds toepassen op het virtuele apparaat, inclusief recente beveiligingsfixes, maar ziet u een wijziging in de versie van het fysieke apparaat StorSimple.

## <a name="whats-new-in-the-january-release"></a>Wat is er nieuw in de release van januari

Deze update bevat een hotfix met betrekking tot de volumes op het virtuele apparaat off line gaan. (Zie [problemen opgelost in deze release](#issues-fixed-in-the-january-release)).  

De update bevat geen nieuwe functies of functionaliteit.  

## <a name="issues-fixed-in-the-january-release"></a>Problemen die zijn opgelost in de versie van januari

De volgende tabel beschrijft het probleem dat is opgelost in deze update.

|Nr.|Functie|Probleem|Bij toepassing op het fysieke apparaat.|Van toepassing is op een virtueel apparaat 
|---|-------|-----|--------------------------|-------------------------
|1|Volumes verbreken|Wanneer een hoge wolk vertragingstijden aanhouden gedurende enkele minuten, de volumes StorSimple virtueel apparaat off line gaan op de hosts. Deze correctie wordt de drempelwaarde voor cloud vertragingstijden, waardoor de omstandigheden waardoor de volumes om offline te gaan op de hosts te minimaliseren.|Nee|Ja  

## <a name="known-issues-in-the-january-release"></a>Bekende problemen in de release van januari

De volgende tabel bevat een overzicht van bekende problemen in deze release.
 
|Nr.|Functie|Probleem|Opmerkingen/oplossing|Bij toepassing op het fysieke apparaat.|Van toepassing is op een virtueel apparaat 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Factory reset|  In sommige gevallen, bij het uitvoeren van een factory-reset het apparaat StorSimple vastgelopen en dit bericht wordt weergegeven: **opnieuw instellen van de fabriek wordt uitgevoerd (8 fase).** Dit gebeurt als u op CTRL + C drukt terwijl de cmdlet uitgevoerd wordt.| Druk CTRL + C niet op na een factory reset wordt gestart. Bent u al in deze toestand, neem u contact op met Microsoft Support voor volgende stappen.|Ja|Nee|
|2|Quorum schijf| In uitzonderlijke gevallen, als het merendeel van de schijven in de behuizing van de EBOD van een 8600 apparaat wordt verbroken, waardoor er geen quorum schijf vervolgens de groep worden off line. Het wordt off line blijven, zelfs als de schijven opnieuw zijn verbonden.|Moet u het apparaat opnieuw wordt opgestart. Als het probleem zich blijft voordoen, neem u contact op met Microsoft Support voor volgende stappen.|Ja |Nee
|3|Wolk momentopname storingen|In zeldzame gevallen kan een momentopname van een wolk mislukken met de fout **Maximum back-limiet is bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat uit hetzelfde oorspronkelijke volume dat is verwijderd.||Ja|Ja
|4|Onjuiste besturings-ID|Als een controller vervanging wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de controller vervangen, wanneer de afbeelding wordt geladen vanuit het knooppunt op hetzelfde niveau, kunt de besturings-ID weergegeven in eerste instantie als peer-controller-ID.  In zeldzame gevallen kan dit gedrag ook nadat het systeem opnieuw moet worden gezien.|Er is geen gebruikersactie vereist. Deze situatie oplossen zelf na de vervanging van de domeincontroller voltooid is.|Ja|Nee 
|5| Grafieken controle apparaat|In de service Manager StorSimple grafieken controle apparaat werken niet als Basic of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat.|Wijzig de web proxy-configuratie voor het apparaat geregistreerd bij uw StorSimple Manager-service zodanig dat verificatie is ingesteld op geen. Voer hiervoor de de Windows PowerShell voor de cmdlet Set-HcsWebProxy van StorSimple.|Ja|Ja
|6| Opslag-accounts|Met behulp van de Storage-service te verwijderen van de account van de opslag is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald.|| Ja |  Ja
|7|Failover van apparaat| Meerdere failovers van een container volume van het bronapparaat met dezelfde naar andere doelapparaten wordt niet ondersteund.| Failover van één dode apparaat met meerdere apparaten maakt de containers volume op het eerste apparaat failover geen eigenaar van de gegevens. Deze containers volume wordt na een failover worden weergegeven of anders werken wanneer u ze in de klassieke Azure portal weergeeft.|Ja|Nee
|8| Installatie|Tijdens de StorSimple Adapter voor SharePoint-installatie moet u een IP apparaat in de volgorde voor de installatie is voltooid.||Ja|Nee
|9| Webproxy|Als de web proxy-configuratie HTTPS als het opgegeven protocol heeft, de communicatie-apparaat te onderhouden worden beïnvloed en het apparaat off. Support pakketten worden ook in het proces, belangrijke bronnen op uw apparaat gegenereerd.|Zorg ervoor dat de URL van de web proxy HTTP als het opgegeven protocol. Zie meer informatie over het [web-proxy configureren voor uw apparaat](storsimple-configure-web-proxy.md).|Ja |Nee
|10|Webproxy|  Als u configureert en webproxy op een geregistreerde apparaat inschakelt, moet u de active controller op uw apparaat opnieuw.|| Ja |Nee
|11|Wolk hoge latentie en hoge i/o-belasting|Wanneer het apparaat StorSimple een combinatie van zeer hoge wolk vertragingstijden (volgorde van seconden) en hoge i/o-belasting tegenkomt, de volumes apparaat in een slechtere toestand en de i/o kan mislukken met een fout 'apparaat niet gereed'.|U moet handmatig opnieuw opstarten van de Apparaatcontrollers of uitvoeren van failover apparaat deze situatie te herstellen.|Ja|Nee

## <a name="physical-device-updates-in-the-january-release"></a>Laat u fysiek apparaatupdates in januari

Deze update bevat geen andere wijzigingen aan het apparaat StorSimple.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Serial attached SCSI (SAS) controller-en firmware-updates in januari

Deze release bevat geen updates van de serial attached SCSI (SAS) controller of de firmware. De stuurprogrammaupdate is in de oktober 2014 release. 

## <a name="virtual-device-updates-in-the-january-release"></a>Laat u updates voor virtueel apparaatstuurprogramma in januari

Deze versie bevat een bijgewerkte afbeelding voor het virtuele apparaat. De virtuele apparaten gemaakt na 20 januari 2015 wordt de versie van de software weergegeven als 6.3.9600.17361.

 
