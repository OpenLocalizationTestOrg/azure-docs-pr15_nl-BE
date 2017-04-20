<properties 
   pageTitle="Release-opmerkingen voor StorSimple 8000-serie Update 2 | Microsoft Azure"
   description="Beschrijving van de nieuwe functies, problemen en oplossingen voor StorSimple 8000-serie 2."
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

# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000-serie Update 2 release-opmerkingen  

## <a name="overview"></a>Overzicht

De volgende release-opmerkingen worden de nieuwe functies beschreven en identificeren van de essentiële openstaande kwesties voor StorSimple 8000-serie 2. Ze bevatten ook een lijst van de StorSimple software, stuurprogramma- en firmware-updates schijf opgenomen in deze release. 

Update 2 kan worden toegepast op elk StorSimple apparaat met Release (GA) of 0,1-Update via de Update 1.2. De apparaatversie van het die met 2 is 6.3.9600.17673.

Lees de informatie in de release-opmerkingen voordat u de update in de StorSimple oplossing implementeren.

>[AZURE.IMPORTANT]
> 
- Het duurt ongeveer 4-7 uur aan deze update (met inbegrip van de Windows-updates). 
- Update 2 heeft de software en stuurprogramma voor LSI SSD firmware-updates.
- Voor de nieuwe releases, kan er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates doen. Wacht een paar dagen en vervolgens zoeken naar updates als deze snel beschikbaar komen.


## <a name="whats-new-in-update-2"></a>Wat is nieuw in Update 2

Update 2 wordt de volgende nieuwe functies geïntroduceerd.

- **Lokale volumes vastgemaakt** – zijn In eerdere versies van de StorSimple 8000-serie blokken met gegevens doorverbonden naar de cloud op basis van gebruik. Er is geen manier om te garanderen dat de blokken op lokale zou blijven. In de Update 2, bij het maken van een volume, kunt u aangeven een volume als lokaal vastgemaakt en primaire gegevens van het volume zal niet worden doorverbonden naar de cloud. Momentopnamen van lokaal vastgezette volumes nog steeds worden gekopieerd naar de cloud voor back-up, zodat de cloud kan worden gebruikt voor gegevens mobiliteit en disaster recovery. Bovendien kunt u het volumetype (converteren doorverbonden volumes naar een lokaal vaste volumes en -volumes converteren lokaal vastgemaakt aan lagen). 

- **StorSimple virtueel apparaat verbeteringen** – voorheen de StorSimple 8000-serie geplaatst het virtuele apparaat als disaster recovery of ontwikkeling/test oplossing. Er is slechts één model van virtueel apparaat (model 1100). Update 2 introduceert twee modellen van virtuele apparaten: 

     - 8010 (voorheen de 1100) – geen wijziging; heeft een capaciteit van 30 TB en Azure standaardopslag worden gebruikt.
     - 8020 – heeft een capaciteit van 64 TB en Azure Premium-opslagruimte gebruikt voor betere prestaties.

    Er is een enkele VHD voor beide modellen virtueel apparaat (8010/8020). Wanneer u het virtuele apparaat eerst, detecteert de parameters van het platform en de juiste versie van toepassing is.

- **Networking verbeteringen** – Update 2 bevat de volgende verbeteringen voor netwerken:

     - Meerdere netwerkkaarten kunnen worden ingeschakeld voor de wolk dat failover optreden kan als een NIC niet.
     - Verbetering van Routering met vaste metrics voor cloud blokken ingeschakeld.
     - Online opnieuw uitvoeren voordat een failover storingen in bronnen.
     - Nieuwe waarschuwingen voor service mislukt.

- **Verbeteringen bij te werken** : In Update 1.2 en eerder, de StorSimple 8000-serie zijn bijgewerkt via twee kanalen: Windows Update voor clustering, iSCSI, enzovoort, en Microsoft Update voor binaire bestanden en firmware.
    Update 2 gebruikt Microsoft Update voor alle updatepakketten. Dit moet leiden tot minder tijd herstellen of failover doet. 

- **Firmware-updates** : de volgende firmware-updates zijn opgenomen:
    - LSI: lsi_sas2.sys versie 2.00.72.10
    - Alleen SSD (geen updates van de harde schijf): XMGG, XGEG, KZ50, F6C2 en VR08

- **Proactieve ondersteuning** – Update 2 maakt Microsoft om aanvullende diagnostische gegevens van het apparaat. Bij ons team van bewerkingen wordt aangegeven welke apparaten die problemen ondervindt, zijn we beter uitgerust om het verzamelen van informatie op het apparaat en het vaststellen van problemen. **Update 2, accepteert kunnen we deze proactieve ondersteuning bieden**.    
 

## <a name="issues-fixed-in-update-2"></a>Problemen die zijn opgelost in Update 2

De volgende tabellen wordt een overzicht van de problemen die in 2 Updates verholpen zijn.    

| Nr. | Functie | Probleem | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Netwerkinterfaces | Na een upgrade naar Update 1, de StorSimple Manager-service heeft gerapporteerd dat de bestand2 en Data3 poorten niet op een domeincontroller. Dit probleem is opgelost. | Ja | Nee |
| 2 | Updates | Na een upgrade naar Update 1 hoorbaar alarm waarschuwingen opgetreden in de Azure klassieke portal op verschillende apparaten. Dit probleem is opgelost. | Ja | Nee |
| 3 | Openstack verificatie | Wanneer u Openstack als uw cloud-serviceprovider, kan een foutbericht dat de wolk verificatie-tekenreeks te lang is. Dit probleem is opgelost. | Ja | Nee |


## <a name="known-issues-in-update-2"></a>Bekende problemen in Update 2

De volgende tabel bevat een overzicht van bekende problemen in deze release.

| Nr. | Functie | Probleem | Opmerkingen / oplossing | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Quorum schijf | In uitzonderlijke gevallen, als de meerderheid van de schijven in de behuizing van de EBOD van een 8600 apparaat wordt verbroken, waardoor er geen quorum schijf gaat vervolgens de opslaggroep offline. Het wordt off line blijven, zelfs als de schijven opnieuw zijn verbonden. | Moet u het apparaat opnieuw wordt opgestart. Als het probleem zich blijft voordoen, neem u contact op met Microsoft Support voor volgende stappen. | Ja | Nee |
| 2 | Onjuiste besturings-ID | Als een controller vervanging wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de controller vervangen, wanneer de afbeelding wordt geladen vanuit het knooppunt op hetzelfde niveau, kunt de besturings-ID weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan dit gedrag ook nadat het systeem opnieuw moet worden gezien. | Er is geen gebruikersactie vereist. Deze situatie oplossen zelf na de vervanging van de domeincontroller voltooid is. | Ja | Nee |
| 3 | Opslag-accounts | Met behulp van de Storage-service te verwijderen van de account van de opslag is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald.|  | Ja | Ja |
| 4 | Failover van apparaat | Meerdere failovers van een container volume van het bronapparaat met dezelfde naar andere doelapparaten wordt niet ondersteund. Failover van één dode apparaat met meerdere apparaten maakt de containers volume op het eerste apparaat failover geen eigenaar van de gegevens. Deze containers volume wordt na een failover worden weergegeven of anders werken wanneer u ze in de klassieke Azure portal weergeeft. | | Ja | Nee |
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
| 15 | Azure PowerShell-cmdlets en lokaal vastgezette volumes | U kunt een lokaal vaste volume via Azure PowerShell-cmdlets maken. (Alle volumes die u via Azure PowerShell maakt worden doorverbonden.) |Gebruik altijd de StorSimple Manager-service lokaal vastgezette volumes configureren.| Ja | Nee |
| 16 |Ruimte voor lokaal vastgezette volumes | Als u een lokaal vaste volume verwijdert, wordt de ruimte die beschikbaar is voor nieuwe volumes niet onmiddellijk bijgewerkt. De StorSimple Manager-service werkt bij de lokale schijfruimte ongeveer eenmaal per uur.| Wacht een uur voordat u het nieuwe volume. | Ja | Nee |
| 17 | Lokaal vastgezette volumes | De terugzettaak beschrijft de momentopname van tijdelijke back-up in de back-catalogus, maar alleen voor de duur van de terugzettaak. Bovendien stelt een virtuele schijfgroep met het voorvoegsel **tmpCollection** op de pagina **Back-up beleid** , maar alleen voor de duur van de taak voor terugzetten beschikbaar. | Dit probleem kan optreden als uw terugzettaak alleen lokaal volumes of een combinatie van lokaal vastgemaakt en trapsgewijs geschakelde volumes vastgemaakt is. Als de terugzettaak alleen trapsgewijs geschakelde volumes bevat, zal niet dit probleem optreden. Er is geen tussenkomst van de gebruiker vereist. | Ja | Nee |
| 18 | Lokaal vastgezette volumes | Als u een terugzettaak en een controller failover annuleert plaatsvindt onmiddellijk daarna de terugzettaak **mislukt** **geannuleerd**wordt weergegeven. Als een taak herstellen mislukt en een controller failover plaatsvindt onmiddellijk daarna de terugzettaak **geannuleerd** in plaats van **mislukt**wordt weergegeven. | Dit probleem kan optreden als uw terugzettaak alleen lokaal volumes of een combinatie van lokaal vastgemaakt en trapsgewijs geschakelde volumes vastgemaakt is. Als de terugzettaak alleen trapsgewijs geschakelde volumes bevat, zal niet dit probleem optreden. Er is geen tussenkomst van de gebruiker vereist. | Ja | Nee |
| 19 |Lokaal vastgezette volumes | Als u een opdracht annuleren of een herstelbewerking mislukt en wordt vervolgens een controller failover plaatsvindt, wordt een extra terugzettaak weergegeven op de pagina **taken** . | Dit probleem kan optreden als uw terugzettaak alleen lokaal volumes of een combinatie van lokaal vastgemaakt en trapsgewijs geschakelde volumes vastgemaakt is. Als de terugzettaak alleen trapsgewijs geschakelde volumes bevat, zal niet dit probleem optreden. Er is geen tussenkomst van de gebruiker vereist. | Ja | Nee |
| 20 |Lokaal vastgezette volumes | Als u probeert een gelaagde volume (gemaakt en gekloond met Update 1.2 of eerder) converteren naar een lokaal vaste volume en het apparaat weinig ruimte of er een stroomstoring wolk is, kan de clone(s) is beschadigd.| Dit probleem treedt alleen op bij volumes die gemaakt en met gekloonde vóór Update 2-software zijn. Dit moet een incidentele scenario.|
| 21 | Volumeconversie | De ACRs gekoppeld aan een volume tijdens de volumeconversie van een niet bijwerken (trapsgewijs geschakeld lokaal vastgemaakt of vice versa). De ACRs bijwerken, kan dit leiden tot beschadiging van gegevens. | Indien nodig, werken de ACRs vóór de volumeconversie en breng geen verdere updates ACR tijdens de conversie uitgevoerd wordt. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Controller en firmware-updates in de Update 2

Deze versie werkt u het stuurprogramma en firmware van de schijf op uw apparaat.
 
- Voor meer informatie over de firmware LSI, raadpleegt u Microsoft Knowledge base-artikel 3121900. 
- Update, Zie Microsoft Knowledge base-artikel 3121899 voor meer informatie over de firmware van de schijf.
 
## <a name="virtual-device-updates-in-update-2"></a>Virtueel apparaatupdates in de Update 2

Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap

Meer informatie over [Update 2 installeren](storsimple-install-update-2.md) op uw apparaat StorSimple.
