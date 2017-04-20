<properties 
    pageTitle="StorSimple 8000 Update 0,1 release notes | Microsoft Azure"
    description="Beschrijving van de nieuwe functies en verbeteringen, openstaande kwesties en oplossingen beschikbaar voor de 2014 oktober release van Microsoft Azure StorSimple (Update 0,1)."
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
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000-serie Update 0,1 release-opmerkingen: oktober 2014  

## <a name="overview"></a>Overzicht

De volgende release identificeren de kritieke problemen open voor StorSimple 8000-serie Update 0.1 uitgebracht in oktober 2014. Ze bevatten ook een lijst van de StorSimple-software en firmware updates zijn opgenomen in deze release. Dit is de eerste release nadat de StorSimple 8000-serie versie beschikbaar is gesteld in het algemeen in juli 2014 en komt met de softwareversie 6.3.9600.17312 overeen.  

U wordt aangeraden voor scannen en alle beschikbare updates van toepassing onmiddellijk na de installatie van het apparaat. U kunt ook inschakelen Automatische updates downloadt en installeert essentiële updates van Microsoft, zodra ze zijn vrijgegeven. Zie voor meer informatie het [bijwerken van uw apparaat StorSimple](storsimple-update-device.md).  

Lees de informatie in de release-opmerkingen voordat u de updates in de StorSimple oplossing implementeren.  

>[AZURE.IMPORTANT]
> 
-   Gebruik de StorSimple Manager-service en niet Windows PowerShell voor StorSimple de oktober updates te installeren.  
-   De updates worden meestal ongeveer 3 uur te voltooien.  
-   De oktober release van StorSimple bevat geen updates van het virtuele apparaat StorSimple. U kunt nog steeds toepassen beschikbare Windows updates, inclusief beveiligingsfixes recente, maar ziet u een wijziging in de versie voor het virtuele apparaat.  

Zorg ervoor dat de volgende voorwaarden wordt voldaan vóór het bijwerken van uw apparaat StorSimple.  

- Zorg ervoor dat beide apparaatcontrollers voordat u naar updates zoekt worden uitgevoerd. Als de controller niet wordt uitgevoerd, mislukt de scan. Als u wilt controleren of de domeincontrollers zijn in orde zijn, gaat u naar de **Status van de Hardware** onder de pagina **onderhoud** . Er zijn onderdelen die **de aandacht nodig**, contact op met Microsoft Support voordat u verdergaat.  
- Ervoor zorgen dat vaste IP-adressen voor beide Controller 0 en 1 Controller routeerbaar en verbinding met het Internet maken kan zoals deze worden gebruikt voor het onderhoud van de updates voor het apparaat. U kunt de [verbinding testen cmdlet](https://technet.microsoft.com/library/hh849808.aspx) ping een bekend adres buiten het netwerk zoals outlook.com, om te controleren of de controller heeft verbinding met het externe netwerk.  
- Zorg ervoor dat de vereiste uitgaande poorten beschikbaar op uw apparaat StorSimple voor uitgaande communicatie zijn. Zie de [vereisten voor uw apparaat StorSimple toegang](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)voor meer informatie.  
- Als de softwareversie van het apparaat ouder is dan 6.3.9600.17312 (update oktober 2014), de gegevens 2 en 3 van de Data-poorten uitschakelen als ingeschakeld voordat u begint met de update. Als u de gegevens 2 of 3 gegevens poorten ingeschakeld laat wanneer u de update toepast, is het mogelijk dat de spelbesturing apparaat in de herstelmodus. Opmerking: wanneer u de netwerkinterfaces uitschakelt, alle gekoppelde volumes worden off line worden genomen en de I/O voor de duur van de update zal worden verstoord.  

## <a name="whats-new-in-the-october-release"></a>Wat is er nieuw in de release van oktober

Deze update bevat de volgende verbeteringen:

- Nu kunt u de service Manager StorSimple gebruikersinterface voor het beheren van uw apparaatcontrollers. Het beheer van acties opnieuw starten, afsluiten, of een domeincontroller inschakelen. Ga naar [de apparaatcontrollers StorSimple beheren](storsimple-manage-device-controller.md)voor meer informatie.  
- U kunt de WAN-bandbreedte worden toegewezen volgens combinaties van dag van de week en tijd van de dag plannen. Hierdoor kunt u beter gebruikmaken van WAN-bandbreedte tijdens rustige uren. Bandbreedte van verschillende sjablonen zijn voor verschillende volume containers toegestaan. Ga voor meer informatie naar [uw bandbreedte StorSimple sjablonen beheren](storsimple-manage-bandwidth-templates.md).  
- U kunt e-mailberichten te informeren de Administrator (s) en andere bestaande of mogelijk toekomstige problemen. Ga naar [Instellingen voor waarschuwingen configureren](storsimple-manage-alerts.md#configure-alert-settings)voor meer informatie.  

## <a name="issues-fixed-in-the-october-release"></a>Problemen die zijn opgelost in de versie van oktober


De volgende tabel bevat een overzicht van de problemen die in deze update zijn opgelost.  

| Nr. | Functie | Probleem | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Netwerkinterfaces | De netwerkinterfaces gegevens 2 en 3 van de gegevens in de vorige versie zijn omgewisseld in de software. Dit probleem is opgelost in deze update. Schakel de instellingen en deze netwerkinterfaces uitschakelen voordat u de update installeert. U moet deze interfaces configureren na de installatie van de update. | Ja | Nee |
| 2 | Support-pakket | In de vorige versie als u de Windows PowerShell- **Export-HcsSupportPackage** -cmdlet voor het ophalen van de Baseboard Management Controller (BMC) zich aanmeldt, de bewerking is mislukt met de volgende waarschuwing weergegeven: ' de bewerking is geslaagd op deze domeincontroller, maar is mislukt vanwege de volgende fout(en) peer controller. Controleer of de peer in orde is en of het huidige knooppunt verbinding kan maken met de peer.' Dit probleem is nu opgelost. | Ja | Nee |
| 3 | Failover van apparaat | In de vorige versie is er een kans op inconsistenties in de gegevens als een taak voor het **ontdekken van de back-up** is mislukt tijdens een failover van het apparaat. Dit probleem is nu opgelost. | Ja | Nee |
| 4 | Failover van apparaat | Back-ups zijn zichtbaar in de eerdere versie, na een failover apparaat, maar de bijbehorende volume-container is niet aanwezig op het doelapparaat. Dit probleem is nu opgelost. | Ja | Nee |
| 5 | Failover van apparaat | In de vorige versie was een bug in de opsomming van cloud back-ups tijdens het register terugzetten dat tot inconsistenties in de gegevens leiden kan als er problemen met de cloud. | Ja | Nee |
| 6 | Firmware-update | In de eerdere versie, de taak apparaat firmware bijwerken is mislukt en een fout die vermeld waren de cmdlets niet herkend en de update mislukt als gevolg hiervan wordt weergegeven. De controller is herstel. Dit probleem is nu opgelost. | Ja | Nee |
| 7 | Installatie | Fouten als gevolg van het apparaat niet wordt replicatie goed tijdens de installatie zijn vastgesteld. | Ja | Nee |
| 8 | Factory reset | U kunt nu desgewenst de firmware controleren op factory reset overslaan. Dit is een wijziging van de vorige release. | Ja | Nee |
| 9 | Factory reset | In de vorige versie wordt tijdens het uitvoeren van een fabriek reset cmdlet, firmware versie controles aangebracht alleen voor bepaalde hardware. Firmware extra controles zijn aangebracht nadat de eerste herstart in het proces, waardoor het herstellen mislukt. Deze oplossing zorgt ervoor dat alle de firmware wordt gecontroleerd wanneer de factory cmdlet reset wordt uitgevoerd voordat de eerste systeem opnieuw op te starten. | Ja | Nee |
| 10 | Opslag account key rotatie | De **Invoke-HcsmServiceDataEncryptionKeyChange** -cmdlet gebruikt om te draaien de sleutels opslag account nu vraagt de gebruiker de sleutel voor de codering gegevens invoeren. Dit is een wijziging van de vorige release waarin de sleutel data service is doorgegeven als een parameter in line. | Ja | Nee |
| 11 | Failback binnen 24 uur | Het opruimen van het bronapparaat hebben bij een noodherstel, duidelijke, waardoor failback worden uitgevoerd niet plaatsgevonden. Dit probleem is opgelost in deze release. | Ja | Nee |

## <a name="known-issues-in-the-october-release"></a>Bekende problemen in de release van oktober

De volgende tabel bevat een overzicht van bekende problemen in deze release.

| Nr. | Functie | Probleem | Opmerkingen/oplossing | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory reset | In sommige gevallen, wanneer u een factory-reset het apparaat StorSimple vastgelopen en dit bericht weergeven: **opnieuw instellen van de fabriek wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt terwijl de cmdlet uitgevoerd wordt. | Druk CTRL + C niet op na een factory reset wordt gestart. Bent u al in deze toestand, neem u contact op met Microsoft Support voor volgende stappen. | Ja | Nee |
| 2 | Factory reset | Geen factory reset doet een StorSimple-apparaat dat is bijgewerkt van NH tot oktober 2014 vrijgeven. | Deze bewerking werkt alleen als een patch is geïnstalleerd. Neem contact op met Microsoft Support als u deze patch vereist. | Ja | Nee | 
| 3 | Quorum schijf | In uitzonderlijke gevallen, als het merendeel van de schijven in de behuizing van de EBOD van een 8600 apparaat wordt verbroken, waardoor er geen quorum schijf vervolgens de groep worden off line. Het wordt off line blijven, zelfs als de schijven opnieuw zijn verbonden. | Moet u het apparaat opnieuw wordt opgestart. Als het probleem zich blijft voordoen, neem u contact op met Microsoft Support voor volgende stappen. | Ja | Nee |
| 4 | Wolk momentopname storingen | In zeldzame gevallen kan een momentopname van een wolk mislukken met de fout **Maximum back-limiet is bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat uit hetzelfde oorspronkelijke volume dat is verwijderd. | | Ja | Ja |
| 5 | Onjuiste besturings-ID | Als een controller vervanging wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de controller vervangen, wanneer de afbeelding wordt geladen vanuit het knooppunt op hetzelfde niveau, kunt de besturings-ID weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan dit gedrag ook nadat het systeem opnieuw moet worden gezien. |Er is geen gebruikersactie vereist. Deze situatie oplossen zelf na de vervanging van de domeincontroller voltooid is. | Ja | Nee |
| 6 | Grafieken controle apparaat | In de service Manager StorSimple grafieken controle apparaat werken niet als Basic of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. | Wijzig de web proxy-configuratie voor het apparaat geregistreerd bij uw StorSimple Manager-service zodanig dat verificatie is ingesteld op geen. Voer hiervoor de de Windows PowerShell voor de cmdlet Set-HcsWebProxy van StorSimple. | Ja | Ja |
| 7 | Opslag-accounts | Met behulp van de Storage-service te verwijderen van de account van de opslag is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | | Ja | Ja |
| 8 | Failover van apparaat | Meerdere failovers van een container volume van het bronapparaat met dezelfde naar andere doelapparaten wordt niet ondersteund. | Failover van één dode apparaat met meerdere apparaten maakt de containers volume op het eerste apparaat failover geen eigenaar van de gegevens. Deze containers volume wordt na een failover worden weergegeven of anders werken wanneer u ze in de klassieke Azure portal weergeeft. | Ja | Nee |
| 9 | Installatie | Tijdens de StorSimple Adapter voor SharePoint-installatie moet u een IP apparaat in de volgorde voor de installatie is voltooid.    | | Ja | Nee |
| 10 | Webproxy | Als de web proxy-configuratie HTTPS als het opgegeven protocol heeft, de communicatie-apparaat te onderhouden worden beïnvloed en het apparaat off. Support pakketten worden ook in het proces, belangrijke bronnen op uw apparaat gegenereerd. | Zorg ervoor dat de URL van de web proxy HTTP als het opgegeven protocol. Meer informatie over het [web-proxy configureren voor uw apparaat](storsimple-configure-web-proxy.md). | Ja | Nee |
| 11 | Webproxy | Als u configureert en webproxy op een geregistreerde apparaat inschakelt, moet u de active controller op uw apparaat opnieuw. | | Ja | Nee |
| 12 | Wolk hoge latentie en hoge i/o-belasting | Wanneer het apparaat StorSimple een combinatie van zeer hoge wolk vertragingstijden (volgorde van seconden) en hoge i/o-belasting tegenkomt, de volumes apparaat in een slechtere toestand en de i/o kan mislukken met een fout 'apparaat niet gereed'. | U moet handmatig opnieuw opstarten van de Apparaatcontrollers of uitvoeren van failover apparaat deze situatie te herstellen. | Ja | Nee |

## <a name="physical-device-updates-in-the-october-release"></a>Fysiek apparaatupdates in de oktober release

Wanneer deze updates zijn toegepast op een fysiek apparaat, verandert versie van de software in een 6.3.9600.17312. Tenzij anders vermeld, worden deze releaseopmerkingen van toepassing op alle modellen van de StorSimple apparaat. Zie voor meer informatie over deze updates [oktober 2014 fysieke toestelsoftware-update voor Microsoft Azure StorSimple toestel](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Serial attached SCSI (SAS) controller en firmware-updates in de oktober release

Deze versie werkt u het stuurprogramma en de firmware van de SAS-controller van het fysieke apparaat. Voor meer informatie over de SAS-controller, raadpleegt u [oktober 2014 voor LSI SAS-controllers in Microsoft Azure StorSimple toestel bijwerken](http://support.microsoft.com/kb/2987020).   

Deze release geldt ook een cumulatieve firmware-update die betrouwbaarheidsproblemen met hardwareonderdelen van het apparaat verhelpt. Zie voor meer informatie over de firmware-update, [oktober 2014 firmware-update voor Microsoft Azure StorSimple toestel](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Virtueel apparaatupdates in de oktober release

Deze release bevat geen updates voor het virtuele apparaat. Versie van de software van een virtueel apparaat wordt niet gewijzigd als u deze update toepast.
 
