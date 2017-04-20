<properties 
   pageTitle="Release-opmerkingen voor StorSimple 8000-serie Update 3 | Microsoft Azure"
   description="Beschrijving van de nieuwe functies, problemen en oplossingen voor StorSimple 8000-serie Update 3."
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
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>StorSimple 8000-serie Update 3-release-opmerkingen  

## <a name="overview"></a>Overzicht

Release-opmerkingen worden de nieuwe functies beschreven en de kritieke problemen open voor StorSimple 8000-serie Update 3 bepalen. Ze bevatten ook een lijst met software-updates voor de StorSimple opgenomen in deze release. 

Update 3 kan worden toegepast op elk StorSimple apparaat met Release (GA) of 0,1-Update via de Update 2.2. De apparaatversie van het die met Update 3 is 6.3.9600.17759.

Lees de informatie in de release-opmerkingen voordat u de update in de StorSimple oplossing implementeren.

>[AZURE.IMPORTANT]
> 
> - Update 3 device-software, LSI stuurprogramma en firmware en Storport heeft en Spaceport bijgewerkt. Het duurt ongeveer 1,5 tot 2 uur om deze update te installeren. 

> - Voor de nieuwe releases, kan er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates doen. Wacht een paar dagen en vervolgens zoeken naar updates als deze snel beschikbaar komen.


## <a name="whats-new-in-update-3"></a>Wat is nieuw in Update 3

De volgende belangrijke verbeteringen en bugfixes zijn aangebracht in de Update 3.

 
- **Automatische ruimte terugwinning wijzigingen** – Update 3 begint, de ruimte terugwinning algoritmen uitvoeren van de stand-by-besturing van het systeem waardoor er sneller wordt uitgevoerd. Zie voor meer informatie over de poorten die nodig zijn om te werken met terugwinning van de ruimte, de [StorSimple netwerkvereisten](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Verbeterde prestaties** – Update 3 heeft verbeterde prestaties voor lezen en schrijven naar de cloud.

- **Migratie gerelateerde verbeteringen** – zijn In deze release, verschillende bugfixes en verbeteringen gedaan voor de migratiefunctie van 5000/7000 serie apparaten 8000 serie apparaten. Ga naar de [migratie van 5000/7000 series apparaat 8000-serie apparaat](https://www.microsoft.com/en-us/download/details.aspx?id=47322)voor meer informatie over het gebruik van de migratiefunctie. 

- **Gerelateerde oplossingen monitoring** - zijn In deze release, bugs betreffende bewaking, grafieken, servicedashboard en apparaat dashboard opgelost.


## <a name="issues-fixed-in-update-3"></a>Problemen die zijn opgelost in Update 3

De volgende tabellen wordt een overzicht van de problemen die zijn verholpen in Update 3.    

| Nee | Functie                                    | Probleem                                                                                                                                                                                                                                                                                        | Bij toepassing op het fysieke apparaat. | Van toepassing is op een virtueel apparaat |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Gegevensmigratie van de host-zijde                      | In de eerdere versie is het toestel StorSimple wolk off line gaan tijdens een gegevensmigratie van de host-zijde. Dit probleem is opgelost in deze release.                                                | Nee                        | Ja                        |
| 2  | Lokaal vastgezette volumes                     | In de vorige versie waren er problemen met betrekking tot i/o-fouten, volume conversie storingen, en datapath voor lokaal vastgezette volumes. Deze problemen zijn hoofd veroorzaakt en opgelost in deze release.                | Ja                        | Nee                       |
| 3  | Monitoring                                    | Er zijn meerdere kwesties in verband met eenheden rapportage en monitoring van en het apparaat dashboard grafieken waarbij onjuiste informatie werd weergegeven voor volumes lokaal vastgemaakt. Deze problemen zijn opgelost in deze release. | Ja                         | Nee                       |
| 4  | Zware schrijft I/O                          | Wanneer u de StorSimple voor een standaardwerkbelasting voor zware schrijft, wordt de gebruiker uitgevoerd in een incidentele bug waar de werkset is wordt doorverbonden naar de cloud. Dit probleem is opgelost in deze release. | Ja                        | Ja                       |
| 5  | Back-up                                     | In bepaalde uitzonderlijke gevallen in de vorige versies van software, wanneer de gebruiker een back-up van een externe kloon heeft, in de cloud-fouten zou worden uitgevoerd en de bewerking zou fout uit. Het probleem is opgelost in deze release, en de bewerking met succes is voltooid.             | Ja                        | Ja                       |
| 6  | Back-beleid                                     | In bepaalde uitzonderlijke gevallen, in de oudere versies van software, er is een fout die verband houden met het verwijderen van back-beleid. Dit probleem is opgelost in deze release.       | Ja                        | Ja                       |



## <a name="known-issues-in-update-3"></a>Bekende problemen in Update 3

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
| 20 |Lokaal vastgezette volumes | Als u probeert een gelaagde volume (gemaakt en gekloond met Update 1.2 of eerder) converteren naar een lokaal vaste volume en het apparaat weinig ruimte of er een stroomstoring wolk is, kan de clone(s) is beschadigd.| Dit probleem treedt alleen op bij volumes die gemaakt met gekloonde vóór het bijwerken en 2.1 software zijn. Dit moet een incidentele scenario.|
| 21 | Volumeconversie | De ACRs gekoppeld aan een volume tijdens de volumeconversie van een niet bijwerken (trapsgewijs geschakeld lokaal vastgemaakt of vice versa). De ACRs bijwerken, kan dit leiden tot beschadiging van gegevens. | Indien nodig, werken de ACRs vóór de volumeconversie en breng geen verdere updates ACR tijdens de conversie uitgevoerd wordt. |
| 22 | Updates | Bij het toepassen van Update 3 de pagina **onderhoud** in de klassieke Azure portal verschijnt het volgende bericht gerelateerd aan Update 2 - "StorSimple 8000-serie Update 2 biedt de mogelijkheid voor Microsoft proactief om logboekgegevens te verzamelen van uw apparaat wanneer we potentiële problemen detecteren". Dit is misleidend omdat Hiermee wordt aangegeven dat het apparaat wordt bijgewerkt met Update 2. Nadat het apparaat bijgewerkt met Update 3 succeesfully is, verdwijnt dit bericht. | Dit probleem wordt verholpen in een toekomstige versie. | Ja | Nee |


## <a name="controller-and-firmware-updates-in-update-3"></a>Controller en firmware-updates in de Update 3

Deze release is LSI stuurprogramma en firmware updates. Zie [Update 3 te installeren](storsimple-install-update-3.md) op uw apparaat StorSimple voor meer informatie over het installeren van het stuurprogramma voor LSI en firmware-updates.

 
## <a name="virtual-device-updates-in-update-3"></a>Virtueel apparaatupdates in de Update 3

Deze update kan niet worden toegepast op het toestel in StorSimple wolk (ook bekend als het virtuele apparaat). Nieuwe virtuele apparaten moet worden gemaakt. 


## <a name="next-step"></a>Volgende stap

Meer informatie over het [installeren van Update 3](storsimple-install-update-3.md) op uw apparaat StorSimple.
