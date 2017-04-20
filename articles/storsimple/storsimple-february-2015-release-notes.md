<properties 
   pageTitle="StorSimple 8000 Update 0,3 release notes | Microsoft Azure"
   description="Beschrijving van de nieuwe functies en verbeteringen, openstaande kwesties en oplossingen beschikbaar voor de februari 2015 Microsoft Azure StorSimple release (Update 0,3)."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000-serie Update 0,3 releaseopmerkingen - februari 2015

## <a name="overview"></a>Overzicht

De volgende release identificeren de kritieke problemen open voor StorSimple 8000-serie Update uitgebracht in februari 2015 0,3. Ze bevatten ook een lijst van de StorSimple-software en firmware updates zijn opgenomen in deze release. Dit is de derde release nadat de StorSimple 8000-serie versie beschikbaar is gesteld in het algemeen in juli 2014.
  
Deze update wijzigt de softwareversie van het apparaat niet via de update van januari. Het blijft versie 6.3.9600.17312. U kunt bevestigen dat de update is geïnstalleerd aan de hand van de datum **Laatst bijgewerkt** . Als de datum 10-2/2015 of later, en vervolgens de update is geïnstalleerd.  

U wordt aangeraden voor scannen en alle beschikbare updates van toepassing onmiddellijk na de installatie van het apparaat StorSimple. U kunt ook inschakelen Automatische updates downloadt en installeert essentiële updates van Microsoft, zodra ze zijn vrijgegeven. Zie [het StorSimple apparaat bijwerken](storsimple-update-device.md)voor meer informatie.  

Lees de informatie in de release-opmerkingen voordat u de update in de StorSimple oplossing implementeren.  

>[AZURE.IMPORTANT]   
>
> - Gebruik de StorSimple Manager-service en niet Windows PowerShell voor StorSimple de februari-update te installeren.   
> - Het duurt ongeveer een uur om deze update te installeren. Echter, als u cumulatieve updates installeert, het kan duren ongeveer 3 uur te voltooien.  
> - De release van februari van StorSimple bevat geen updates van het virtuele apparaat StorSimple. U kunt alle beschikbare updates voor Windows nog steeds toepassen op het virtuele apparaat, inclusief recente beveiligingsfixes, maar ziet u een wijziging in de versie voor het virtuele apparaat.  

Zorg ervoor dat de volgende voorwaarden wordt voldaan vóór het bijwerken van uw apparaat StorSimple.  

- Zorg ervoor dat beide apparaatcontrollers voordat u naar updates zoekt worden uitgevoerd. Als de controller niet wordt uitgevoerd, mislukt de scan. Als u wilt controleren of de domeincontrollers zijn in orde zijn, gaat u naar de **Status van de Hardware** onder de pagina **onderhoud** . Er zijn onderdelen die **de aandacht nodig**, contact op met Microsoft Support voordat u verdergaat.
- Ervoor zorgen dat vaste IP-adressen voor controller 0 en 1 controller routeerbaar en verbinding kan maken met het Internet zoals deze worden gebruikt voor het onderhoud van de updates voor het apparaat. U kunt de [verbinding testen cmdlet](https://technet.microsoft.com/library/hh849808.aspx) ping een bekend adres buiten het netwerk, zoals outlook.com, om te controleren of de controller heeft verbinding met het externe netwerk.
- Zorg ervoor dat poorten 80 en 443 beschikbaar op uw apparaat StorSimple voor uitgaande communicatie zijn. Zie de [vereisten voor uw apparaat StorSimple toegang](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)voor meer informatie.
- Als de softwareversie van het apparaat ouder is dan 6.3.9600.17312 (update oktober 2014), de gegevens 2 en 3 van de Data-poorten uitschakelen als ingeschakeld voordat u begint met de update. Zeehavens van de Data 2 of 3 gegevens ingeschakeld wanneer u de update hebt toegepast, is het mogelijk dat de controller apparaat in de herstelmodus. Houd er rekening mee dat wanneer u de netwerkinterfaces uitschakelt, gekoppelde volumes worden off line worden genomen en de i/o voor de duur van de update zal worden verstoord.  
  
## <a name="whats-new-in-the-february-release"></a>Wat is er nieuw in de release van februari

Deze update bevat een correctie voor de factory probleem dat is opgetreden reset op apparaten die waren van het NH is bijgewerkt naar de versie van oktober 2014 vrijgeven. Zie voor meer informatie [problemen opgelost in deze release](#issues-fixed-in-the-february-release).   

Deze update bevat geen nieuwe functies of functionaliteit.  

## <a name="issues-fixed-in-the-february-release"></a>Problemen die zijn opgelost in de versie van februari

De volgende tabel beschrijft het probleem dat is opgelost in deze update.  
 
| Nr. | Functie | Probleem | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Factory reset | U probeert uit te voeren een factory reset op een apparaat dat oorspronkelijk was de NH-versie (versie 6.3.9600.17215) geïnstalleerd, maar is bijgewerkt naar de oktober release (versie 6.3.9600.17312). De fabriek opnieuw mislukt en het apparaat wordt instabiel. | Ja | Nee |


## <a name="known-issues-in-the-february-release"></a>Bekende problemen in de release van februari

De volgende tabel bevat een overzicht van bekende problemen in deze release.
 
| Nr. | Functie | Probleem | Opmerkingen/oplossing | Bij toepassing op het fysieke apparaat.  | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Factory reset | In sommige gevallen, wanneer u een factory-reset het apparaat StorSimple vastgelopen en dit bericht weergeven: **opnieuw instellen van de fabriek wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt terwijl de cmdlet uitgevoerd wordt. | Druk CTRL + C niet op na een factory reset wordt gestart. Bent u al in deze toestand, neem u contact op met Microsoft Support voor volgende stappen. | Ja | Nee |
| 2 | Quorum schijf | In uitzonderlijke gevallen, als de meerderheid van de schijven in de behuizing van de EBOD van een 8600device wordt verbroken, waardoor er geen quorum schijf vervolgens de opslaggroep niet off line. Het wordt off line blijven, zelfs als de schijven opnieuw zijn verbonden. | Moet u het apparaat opnieuw wordt opgestart. Als het probleem zich blijft voordoen, neem u contact op met Microsoft Support voor volgende stappen. | Ja | Nee |
| 3 | Wolk momentopname storingen | In zeldzame gevallen kan een momentopname van een wolk mislukken met de fout **Maximum back-limiet is bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat uit hetzelfde oorspronkelijke volume dat is verwijderd. |  | Ja | Ja |
| 4 | Onjuiste besturings-ID | Als een controller vervanging wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de controller vervangen, wanneer de afbeelding wordt geladen vanuit het knooppunt op hetzelfde niveau, kunt de besturings-ID weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan dit gedrag ook nadat het systeem opnieuw moet worden gezien. | Er is geen gebruikersactie vereist. Deze situatie oplossen zelf na de vervanging van de domeincontroller voltooid is. | Ja | Nee |
| 5 | Grafieken controle apparaat | In de service Manager StorSimple grafieken controle apparaat werken niet als Basic of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. | Wijzig de web proxy-configuratie voor het apparaat geregistreerd bij uw StorSimple Manager-service zodanig dat verificatie is ingesteld op geen. Voer hiervoor de de Windows PowerShell voor de cmdlet Set-HcsWebProxy van StorSimple. | Ja | Ja |
| 6 | Opslag-accounts | Met behulp van de Storage-service te verwijderen van de account van de opslag is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. |  | Ja | Ja |
| 7 | Failover van apparaat | Meerdere failovers van een container volume van het bronapparaat met dezelfde naar andere doelapparaten wordt niet ondersteund.  Failover van één dode apparaat met meerdere apparaten maakt de containers volume op het eerste apparaat failover geen eigenaar van de gegevens. Deze containers volume wordt na een failover worden weergegeven of anders werken wanneer u ze in de klassieke Azure portal weergeeft. |   | Ja | Nee |
| 8 | Installatie | Tijdens de StorSimple Adapter voor SharePoint-installatie moet u een IP apparaat in de volgorde voor de installatie is voltooid. |  | Ja | Nee |
| 9 | Webproxy | Als de web proxy-configuratie HTTPS als het opgegeven protocol heeft, de communicatie-apparaat te onderhouden worden beïnvloed en het apparaat off. Support pakketten worden ook in het proces, belangrijke bronnen op uw apparaat gegenereerd. | Zorg ervoor dat de URL van de web proxy HTTP als het opgegeven protocol. Meer informatie over het [web-proxy configureren voor uw apparaat](storsimple-configure-web-proxy.md). | Ja | Nee |
| 10 | Webproxy | Als u configureert en webproxy op een geregistreerde apparaat inschakelt, moet u de active controller op uw apparaat opnieuw. |  | Ja | Nee |
| 11 | Wolk hoge latentie en hoge i/o-belasting | Wanneer het apparaat StorSimple een combinatie van zeer hoge wolk vertragingstijden (volgorde van seconden) en hoge i/o-belasting tegenkomt, de volumes apparaat in een slechtere toestand en de i/o kan mislukken met een fout 'apparaat niet gereed'. | U moet handmatig opnieuw opstarten van de Apparaatcontrollers of uitvoeren van failover apparaat deze situatie te herstellen. | Ja | Nee |

## <a name="physical-device-updates-in-the-february-release"></a>Updates in de februari fysiek apparaat vrijgeven

Deze update corrigeert de factory reset probleem dat is opgetreden op apparaten die waren van het NH is bijgewerkt naar de versie van oktober 2014 vrijgeven. Het bevat geen andere updates van de StorSimple apparaat.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Serial attached SCSI (SAS) controller-en firmware-updates in februari

Deze release bevat geen updates van de serial attached SCSI (SAS) controller of de firmware. De stuurprogrammaupdate is in de oktober 2014 release.  

## <a name="virtual-device-updates-in-the-february-release"></a>Updates voor virtueel apparaatstuurprogramma in februari vrijgeven

Deze release bevat geen updates voor het virtuele apparaat. Versie van de software van een virtueel apparaat wordt niet gewijzigd als u deze update toepast.
 
