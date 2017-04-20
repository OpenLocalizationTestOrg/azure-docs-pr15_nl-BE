<properties 
   pageTitle="Release-opmerkingen voor StorSimple 8000-serie Update 1.2 | Microsoft Azure"
   description="Beschrijving van de nieuwe functies, problemen en oplossingen voor StorSimple 8000-serie Update 1.2."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>StorSimple 8000-serie Update 1.2-release-opmerkingen  

## <a name="overview"></a>Overzicht

De volgende release-opmerkingen worden de nieuwe functies beschreven en de essentiële openstaande kwesties voor StorSimple 8000-serie Update 1.2 identificeren. Ze bevatten ook een lijst van de StorSimple software, stuurprogramma- en firmware-updates schijf opgenomen in deze release. 

Update 1.2 kan worden toegepast op elk apparaat StorSimple Release (GA), 0,1 Update, Update 0,2 of 0,3 Update-software. Update 1.2 is niet beschikbaar als het apparaat met Update 1 of Update 1.1. Als uw apparaat kan vrijgeven (GA) wordt uitgevoerd, kunt u [contact opnemen met Microsoft Productondersteuning](storsimple-contact-microsoft-support.md) om u te helpen bij het installeren van deze update.

De volgende tabel worden de versies van het apparaat software overeenkomt met update 1, 1.1 en 1.2.

| Als de update uitgevoerd... | Dit is uw softwareversie van het apparaat. |
|---------------------|---------------------------------------|
| Update 1.2          | 6.3.9600.17584                        |
| Update 1.1          | 6.3.9600.17521                        |
| Update 1.0          | 6.3.9600.17491                        |

Lees de informatie in de release-opmerkingen voordat u de update in de StorSimple oplossing implementeren. Zie voor meer informatie het [installeren van Update 1.2 op uw apparaat StorSimple](storsimple-install-update-1.md). 

>[AZURE.IMPORTANT]
> 
- Het duurt ongeveer 5-10 uur voor de installatie van deze update (met inbegrip van de Windows-Updates). 
- Update 1.2 heeft software, LSI stuurprogramma en firmware-updates voor schijf. Als u wilt installeren, volg de instructies in het [installeren van Update 1.2 op uw apparaat StorSimple](storsimple-install-update-1.md).
- Voor de nieuwe releases, kan er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates doen. Zoeken naar updates in een paar dagen opnieuw als deze beschikbaar komen binnenkort.


## <a name="whats-new-in-update-12"></a>Wat is nieuw in Update 1.2

Deze functies zijn voor het eerst uitgebracht met Update 1 die aan een beperkt aantal gebruikers beschikbaar is gesteld. Met de release Update 1.2, de meeste van de gebruikers StorSimple ziet de volgende nieuwe functies en verbeteringen:

- **Migratie van 5000-7000 series naar 8000 serie apparaten** – deze versie bevat een nieuwe migratiefunctie waarmee de StorSimple 5000-7000 series toestel gebruikers hun gegevens migreren naar een fysieke StorSimple 8000-serie-toestel of een virtueel toestel. De migratiefunctie heeft twee belangrijke proposities:                                                                  

    - **Business continuity**, doordat de migratie van bestaande gegevens op 5000-7000 series toestellen op 8000-serie toestellen.
    - **Verbeterde functie voor aanbiedingen van de 8000-serie toestellen**, zoals efficiënte gecentraliseerd beheer van meerdere apparaten via StorSimple Manager service, betere categorie hardware en firmware, virtuele apparaten, gegevensmobiliteit en functies in de toekomstige roadmap bijgewerkt.

    Raadpleeg de [gids](http://www.microsoft.com/download/details.aspx?id=47322) voor meer informatie over het migreren van een StorSimple 5000-7000-serie met een apparaat dat 8000-serie. 

- **De beschikbaarheid in de Portal Azure overheid** – StorSimple is nu beschikbaar in de portal Azure regering. Zie het [implementeren van een apparaat StorSimple in Azure regering Portal](storsimple-deployment-walkthrough-gov.md).

- **Ondersteuning voor andere cloud-providers** – de andere cloud providers ondersteund zijn Amazon S3, Amazon S3 met Bronrecords, HP en OpenStack (beta).

- **Update naar de nieuwste Storage API's** – is met deze release StorSimple bijgewerkt naar de nieuwste Azure Storage service API's. StorSimple 8000-serie apparaten met software-versies vóór Update 1 (Release, 0,1 0,2 en 0,3) met een versie van de Azure Storage Service API's ouder dan 17 juli 2009. Zoals vermeld in de [aankondiging over het verwijderen van de versies van opslag service](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)bijgewerkt op 1 augustus 2016 wordt deze API's worden afgeschaft. Het is noodzakelijk het StorSimple 8000-serie 1 voor 1 augustus 2016 toe te passen. Als u dit niet, stopt StorSimple apparaten correct functioneren.

- **Ondersteuning voor Zone redundante opslag (ZRS)** – met de upgrade naar de nieuwste versie van de API's voor opslag, de StorSimple 8000-serie ondersteunt Zone redundante opslag (ZRS) lokaal redundante opslag (LRS) en Geo-redundante opslag (GRS). Raadpleeg dit [artikel op Azure opslag redundantieopties](../storage/storage-redundancy.md) voor details van de ZRS.

- **Uitgebreide initiële implementatie- en ervaring** – zijn In deze release, de installatie en update processen verbeterd. De installatie via de wizard setup is verbeterd als u feedback wilt aan de gebruiker als de netwerkconfiguratie en de firewall-instellingen onjuist zijn. Aanvullende diagnostische cmdlets zijn gesteld om u te helpen bij het oplossen van het netwerk van het apparaat. Zie de [deployment artikel oplossen](storsimple-troubleshoot-deployment.md) voor meer informatie over de nieuwe diagnostische cmdlets die wordt gebruikt voor het oplossen van problemen.

## <a name="issues-fixed-in-update-12"></a>Problemen die zijn opgelost in Update 1.2

De volgende tabel bevat een overzicht van de problemen die zijn opgelost in de Updates 1.2, 1.1 en 1.    


| Nr. | Functie | Probleem | Opgelost met Update | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Windows PowerShell voor StorSimple | Wanneer een gebruiker op afstand toegang krijgen de StorSimple-apparaat tot met behulp van Windows PowerShell voor StorSimple en vervolgens de wizard setup is gestart, wordt een crash spoedig gegevensinvoer 0 IP is opgetreden. Deze fout is nu opgelost met Update 1. | Update 1 | Ja | Ja |
| 2 | Factory reset | In sommige gevallen, bij een factory-reset het apparaat StorSimple werd blijven hangen en dit bericht wordt weergegeven: **opnieuw instellen van de fabriek wordt uitgevoerd (fase 8)**. Dit is het geval als u op CTRL + C drukt terwijl de cmdlet werd uitgevoerd. Dit probleem is nu opgelost.| Update 1 | Ja | Nee |
| 3 | Factory reset | Na een mislukte twee controllers factory reset, konden u doorgaan met het apparaat. Dit resulteerde in een niet-ondersteunde configuratie. In Update 1, wordt een foutbericht weergegeven en inschrijving is geblokkeerd op een apparaat dat is een mislukte factory reset. | Update 1 | Ja | Nee |
| 4 | Factory reset | In sommige gevallen zijn incompatibel met false positieve signalen verhoogd. Probleem met onjuiste meldingen worden niet langer gegenereerd op apparaten met Update 1. | Update 1 | Ja | Nee |
| 5 | Factory reset | Als een fabriek reset is onderbroken voordat deze is voltooid, wordt het apparaat de herstelmodus en staat niet toe dat u toegang tot Windows PowerShell voor StorSimple. Dit probleem is nu opgelost. | Update 1 | Ja | Nee |
| 6 | Noodherstel | Disaster recovery (DR) fout is opgelost waarbij DR zou mislukken tijdens het opsporen van back-ups op het doelapparaat. | Update 1 | Ja | Ja |
| 7 | Controle LED 's | In bepaalde gevallen wordt monitoring LED's aan de achterkant van toestel niet gespecificeerd correcte status. De blauwe LED is uitgeschakeld. Zelfs wanneer deze interfaces zijn niet geconfigureerd zijn 0-gegevens en gegevens 1 LED's knipperen. Het probleem is opgelost en controle LED's geven nu de juiste status.  | Update 1 | Ja | Nee |
| 8 | Controle LED 's | In bepaalde gevallen, na het toepassen van Update 1, het blauwe lampje op de actieve domeincontroller uitgeschakeld waardoor het moeilijk te identificeren, de actieve domeincontroller. Dit probleem is opgelost in deze versie van de patch.| Update 1.2 | Ja | Nee |
| 9 | Netwerkinterfaces | In vorige versies kan een StorSimple apparaat geconfigureerd met een niet-routeerbaar gateway off line gaan. In deze release, de routeringsmetric voor gegevens 0 is gemaakt van de laagste; Daarom, zelfs als andere netwerkinterfaces wolk ingeschakeld, alle verkeer van een wolk van het apparaat worden gerouteerd via Data 0. | Update 1 | Ja | Ja | 
| 10 | Back-ups | Een fout in Update 1, waardoor de back-ups mislukken na 24 dagen is opgelost in de versie van de patch Update 1.1. | Update 1.1 | Ja | Ja |
| 11 | Back-ups | Een fout in de vorige versies resulteerde in slechte prestaties voor momentopnamen van de wolk met wijzigen van lage tarieven. Deze fout is verholpen in de release van deze patch.| Update 1.2 | Ja | Ja |
| 12 | Updates | Een fout in de Update 1 die een mislukte upgrade aangegeven en veroorzaakt de controllers in de herstelmodus, is opgelost in deze versie van de patch.| Update 1.2 | Ja | Ja |


## <a name="known-issues-in-update-12"></a>Bekende problemen in Update 1.2

De volgende tabel bevat een overzicht van bekende problemen in deze release.

| Nr. | Functie | Probleem | Opmerkingen/oplossing | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Quorum schijf | In uitzonderlijke gevallen, als het merendeel van de schijven in de behuizing van de EBOD van een 8600 apparaat wordt verbroken, waardoor er geen quorum schijf vervolgens de groep worden off line. Het wordt off line blijven, zelfs als de schijven opnieuw zijn verbonden. | Moet u het apparaat opnieuw wordt opgestart. Als het probleem zich blijft voordoen, neem u contact op met Microsoft Support voor volgende stappen. | Ja | Nee |
| 2 | Onjuiste besturings-ID | Als een controller vervanging wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de controller vervangen, wanneer de afbeelding wordt geladen vanuit het knooppunt op hetzelfde niveau, kunt de besturings-ID weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan dit gedrag ook nadat het systeem opnieuw moet worden gezien. | Er is geen gebruikersactie vereist. Deze situatie oplossen zelf na de vervanging van de domeincontroller voltooid is. | Ja | Nee |
| 3 | Opslag-accounts | Met behulp van de Storage-service te verwijderen van de account van de opslag is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | Ja | Ja |
| 4 | Failover van apparaat | Meerdere failovers van een container volume van het bronapparaat met dezelfde naar andere doelapparaten wordt niet ondersteund. Apparaat failover van één dode apparaat met meerdere apparaten maakt de containers volume op het eerste apparaat failover geen eigenaar van de gegevens. Deze containers volume wordt na een failover worden weergegeven of anders werken wanneer u ze in de klassieke Azure portal weergeeft. | | Ja | Nee |
| 5 | Installatie | Tijdens de StorSimple Adapter voor SharePoint-installatie moet u een IP apparaat in de volgorde voor de installatie is voltooid.    | | Ja | Nee |
| 6 | Webproxy | Als de web proxy-configuratie HTTPS als het opgegeven protocol heeft, de communicatie-apparaat te onderhouden worden beïnvloed en het apparaat off. Support pakketten worden ook in het proces, belangrijke bronnen op uw apparaat gegenereerd. | Zorg ervoor dat de URL van de web proxy HTTP als het opgegeven protocol. Ga naar [web-proxy configureren voor uw apparaat](storsimple-configure-web-proxy.md)voor meer informatie. | Ja | Nee |
| 7 | Webproxy | Als u configureert en webproxy op een geregistreerde apparaat inschakelt, moet u de active controller op uw apparaat opnieuw. | | Ja | Nee |
| 8 | Wolk hoge latentie en hoge i/o-belasting | Wanneer het apparaat StorSimple een combinatie van zeer hoge wolk vertragingstijden (volgorde van seconden) en hoge i/o-belasting tegenkomt, de volumes apparaat in een slechtere toestand en de i/o kan mislukken met een fout 'apparaat niet gereed'. | U moet handmatig opnieuw opstarten van de Apparaatcontrollers of uitvoeren van failover apparaat deze situatie te herstellen. | Ja | Nee |
| 9 | Azure PowerShell | Wanneer u de StorSimple cmdlet Get-AzureStorSimpleStorageAccountCredential- **& #124; Select-Object - Wacht eerst 1 -** voor het eerste object selecteert zodat u een nieuw **VolumeContainer** -object kunt maken, de cmdlet retourneert alle objecten. | De cmdlet als volgt tussen haakjes plaatsen: **(Get Azure StorSimpleStorageAccountCredential) & #124; Select-Object - Wacht eerst 1 -** | Ja | Ja |
| 10| Migratie | Wanneer meerdere volume containers worden doorgegeven voor migratie, wordt de ETA voor de meest recente back-up alleen voor de eerste volume-container. Bovendien begint parallelle migratie nadat de eerste 4 back-ups in de eerste volume container worden gemigreerd. | Wij raden u aan één volume container tegelijk migreren. | Ja | Nee |
| 11| Migratie | Na het herstellen, worden volumes niet toegevoegd aan het back-beleid of de virtuele schijfgroep. | U moet deze volumes toevoegen aan een back-up beleid om back-ups maken. | Ja | Ja |
| 12| Migratie | Nadat de migratie voltooid is, moet het apparaat 5000/7000 series geen toegang tot de gemigreerde gegevenscontainers. | Wij raden aan dat u de containers voor de gemigreerde gegevens verwijderen nadat de migratie voltooid en de toegezegde is. | Ja | Nee |
| 13| Klonen en DR | Een StorSimple apparaat met Update 1 kan niet klonen of herstel naar een apparaat met 1 vóór update-software uit te voeren. | U moet het doelapparaat op 1 bijwerken zodat deze bewerkingen bijwerken | Ja | Ja |
| 14 | Migratie | Back-up voor de migratie mislukt op een 5000-7000 series apparaat wanneer er geen gekoppelde volumes volume groepen. | De lege volume groepen met geen gekoppelde volumes verwijderen en voer de back-up opnieuw uit.| Ja | Nee |

## <a name="physical-device-updates-in-update-12"></a>Updates in de Update 1.2 fysiek apparaat

Als patch update 1.2 wordt toegepast op een fysiek apparaat (versies van vóór de Update 1 wordt uitgevoerd), versie van de software wordt gewijzigd in 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Controller en firmware-updates in de Update 1.2

Deze versie werkt u het stuurprogramma en firmware van de schijf op uw apparaat.
 
- Zie voor meer informatie over de SAS-controller-update [1 voor LSI SAS-controllers in Microsoft Azure StorSimple toestel bijwerken](https://support.microsoft.com/kb/3043005). 

- Voor meer informatie over de schijf firmware update, Zie [schijf firmware Update 1 voor Microsoft Azure StorSimple toestel](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Virtueel apparaatupdates in de Update 1.2

Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-steps"></a>Volgende stappen

- [Update 1.2 installeren op uw apparaat](storsimple-install-update-1.md).
 
