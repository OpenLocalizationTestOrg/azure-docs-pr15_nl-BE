<properties 
   pageTitle="Lokaal StorSimple vastgemaakt volumes Veelgestelde vragen | Microsoft Azure"
   description="Bevat antwoorden op veelgestelde vragen over volumes StorSimple lokaal vastgemaakt."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>Lokaal StorSimple vastgemaakt volumes: veelgestelde vragen (FAQ)

## <a name="overview"></a>Overzicht

Hieronder vindt u vragen en antwoorden die u hebt als u een lokaal vastgemaakt StorSimple volume maken een gelaagde volume naar een lokaal vaste volume (en vice versa), converteren of back-up en terugzetten van een lokaal vaste volume.

Vragen en antwoorden worden gerangschikt in de volgende categorieën

- Een lokaal vaste volume maken
- Back-up lokaal vastgemaakt
- Een gelaagde volume converteren naar een lokaal vaste volume
- Een lokaal vaste volume herstellen
- Storing worden overgenomen van een lokaal vaste volume

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Vragen over het maken van een lokaal vaste volume

**Q.** Wat is de maximale grootte van een lokaal vaste volume dat ik op apparaten uit de 8000-serie maken kan?

**A** kunt u lokaal vastgezette volumes inrichten 8,5 TB of trapsgewijs geschakelde volumes op het apparaat 8100 200 TB. Op de grotere 8600-apparaat, kunt u lokaal vastgezette volumes inrichten maximaal 22,5 TB of trapsgewijs geschakelde volumes 500 TB.

**Q.** Onlangs mijn 8100 apparaat upgraden naar Update 2 en wanneer ik probeer om een lokaal vaste volume te maken, de maximum beschikbare grootte is slechts 6 TB en niet 8,5 TB. Waarom kan ik een 8,5 TB volume niet maken

**A** kunt u lokaal vastgezette volumes tot 8,5 inrichten TB of doorverbonden volumes op het apparaat 8100 200 TB. Als uw apparaat al is doorverbonden volumes, is de ruimte die beschikbaar is voor het maken van een lokaal vaste volume niet proportioneel lager is dan de maximale limiet. Bijvoorbeeld als 100 TB met trapsgewijs geschakelde volumes hebben al is ingericht op het 8100 apparaat (dat is de helft van de gelaagde capaciteit), vervolgens de maximale grootte van een lokaal volume dat u op het apparaat 8100 maken kunt dienovereenkomstig verlaagd tot 4 TB (ongeveer de helft van de maximale lokaal vastgemaakt volumecapaciteit).

Omdat sommige lokale ruimte op het apparaat wordt gebruikt als host voor de werkset van het trapsgewijs geschakelde volumes, de beschikbare ruimte voor het maken van een lokaal vaste volume verminderd als het apparaat is doorverbonden volumes. Daarentegen vermindert maken van een lokaal vaste volume proportioneel de beschikbare ruimte voor trapsgewijs geschakelde volumes. De volgende tabel worden de beschikbare gelaagde capaciteit op de apparaten 8100 en 8600 als lokaal vastgezette volumes worden gemaakt.

|Lokaal vastgezette volumes ingerichte capaciteit|Beschikbare capaciteit worden ingericht voor gelaagde volumes - 8100|Beschikbare capaciteit worden ingericht voor gelaagde volumes - 8600|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176,5 TB | 477.8 TB|
|4 TB | 105.9 TB | 411.1 TB |
|8,5 TB | 0 TB | 311.1 TB|
|10 TB | NB | 277.8 TB |
|15 TB | NB | 166.7 TB |
|22.5 TB | NB | 0 TB |


**Q.** Waarom is een langdurige bewerking lokaal vaste volume maken? 

**A.** Lokaal vastgezette volumes zijn dik ingericht. Om ruimte te maken op de lokale lagen van het apparaat, kunnen bepaalde gegevens uit een bestaand trapsgewijs geschakelde volumes worden geduwd naar de cloud tijdens het inrichten. En omdat dit afhankelijk van de grootte van het volume wordt ingericht, de bestaande gegevens op het apparaat en de beschikbare bandbreedte naar de cloud is, is de tijd voor het maken van een lokaal volume kan meerdere uren.

**Q.** Hoe lang duurt het maken van een lokaal vaste volume?

**A.** Omdat lokaal vastgezette volumes dik zijn ingericht, sommige bestaande gegevens uit de trapsgewijs geschakelde volumes mogelijk verplaatst naar de cloud tijdens het inrichten. De tijd voor het maken van een lokaal vaste volume is daarom afhankelijk van meerdere factoren, zoals de grootte van het volume, de gegevens op het apparaat en de beschikbare bandbreedte. Op een pas geïnstalleerd apparaat dat geen volumes heeft, is het tijd voor het maken van een lokaal vaste volume ongeveer 10 minuten per terabyte van gegevens. Het maken van lokale volumes nemen echter enkele uren op basis van de factoren die uiteengezet op een apparaat dat wordt gebruikt.

**Q.** Ik wil een lokaal vaste volume maken. Zijn er eventuele aanbevelingen moet ik rekening houden met?

**A.** Lokaal vastgezette volumes zijn geschikt voor de werklast die lokale garanties van gegevens te allen tijde vereist en zijn gevoelig voor cloud vertragingstijden. Bij het beoordelen van het gebruik van lokale volumes voor een van uw werkbelasting zorg rekening met het volgende:

- Lokaal vastgezette volumes dik zijn ingericht en lokale volumes maken van invloed op de beschikbare ruimte voor trapsgewijs geschakelde volumes. Daarom stellen we voor u begint met volumes kleiner en schalen naarmate uw opslag eis toeneemt.

- Inrichten van lokale volumes is een langdurige bewerking die mogelijk betrekking hebben op bestaande gegevens van gelaagde volumes pushen naar de cloud. Als gevolg hiervan kunnen er verminderde prestaties op deze volumes.

- Inrichten van lokale volumes is erg lang duren. De werkelijke tijd die betrokken zijn, is afhankelijk van meerdere factoren: de grootte van het volume wordt ingericht, de gegevens op het apparaat en de beschikbare bandbreedte. Als uw bestaande volumes niet naar de cloud back, is volume maken trager. Wij bevelen dat u momentopnamen wolk van uw bestaande volumes voordat u een lokaal volume inrichten.
 
- Kunt u bestaande trapsgewijze volumes converteren naar lokaal vastgezette volumes en deze omzetting betrekking heeft op het inrichten van de ruimte op het apparaat voor het resulterende lokaal vaste volume (naast waarbij de gelaagde gegevens [eventueel] uit de cloud). Nogmaals, dit is een langdurige bewerking die afhankelijk van factoren die we is hierboven besproken. Het is raadzaam back-up van uw bestaande volumes vóór conversie als het proces zelfs nog trager werken als bestaande volumes zijn niet een back-up. Uw apparaat kan ook optreden verminderde prestaties tijdens dit proces.
    
Meer informatie over het [maken van een lokaal vaste volume](storsimple-manage-volumes-u2.md#add-a-volume)

**Q.** Kan ik meerdere lokale vaste volumes maken op hetzelfde moment?

**A.** Ja, maar een lokaal vaste volume aanmaken en uitbreiden taken sequentieel verwerkt.

Lokaal vastgezette volumes dik zijn ingericht en dit vereist het maken van lokale ruimte op het apparaat (dit kan ertoe leiden dat bestaande gegevens uit de trapsgewijs geschakelde volumes moet tijdens het inrichtingsproces worden gepusht naar de cloud). Dus als een provisioning uitgevoerd wordt, andere taken lokaal volume maken zal worden in de wachtrij totdat die taak is voltooid.

Op dezelfde manier als een bestaand lokaal volume wordt uitgebreid of een gelaagde volume wordt geconverteerd naar een lokaal vaste volume, vervolgens het maken van een nieuw lokaal vaste volume is in de wachtrij geplaatst totdat de vorige taak is voltooid. Het vergroten van een lokaal vaste volume heeft betrekking op de uitbreiding van de bestaande lokale ruimte voor het volume. De conversie van een gelaagde aan vastgemaakt lokaal volume ook betrekking heeft op het maken van lokale ruimte voor de resulterende lokaal volume vastgemaakt. In zowel van deze bewerkingen, maken of uitbreiding van de lokale ruimte wordt een Long-waarde uitgevoerd project.

U kunt deze taken weergeven op de pagina **taken** van de Azure StorSimple Manager-service. De taak die actief wordt verwerkt wordt voortdurend bijgewerkt met de voortgang van de ruimte wordt ingericht. De resterende taken voor lokaal vaste volume is gemarkeerd als voorlopig, maar hun voortgang is opgehouden en zij worden opgehaald in de volgorde waarin dat ze werden in de wachtrij geplaatst.

**Q.** Ik heb verwijderd een lokaal vaste volume. Waarom Zie de geregenereerde ruimte in de beschikbare ruimte weergegeven wanneer ik een nieuw volume niet? 

**A.** Als u een lokaal vaste volume verwijdert, wordt de ruimte die beschikbaar is voor nieuwe volumes niet onmiddellijk bijgewerkt. De StorSimple Manager-Service werkt bij de lokale schijfruimte ongeveer eenmaal per uur. Wij bevelen dat u wacht een uur voordat u het nieuwe volume.

**Q.** Ondersteunt het toestel wolk lokaal vastgezette volumes?

**A.** Lokaal vastgezette volumes worden niet ondersteund op het toestel cloud (8010 en 8020 apparaten voorheen bekend als het virtuele apparaat StorSimple).

**Q.** Kan ik de Azure PowerShell-cmdlets gebruiken maken en lokaal vastgezette volumes beheren 

**A.** Nee, u lokaal vastgezette volumes via Azure PowerShell cmdlets (alle volumes die u via Azure PowerShell maakt is doorverbonden) niet maken. Wij bevelen ook gebruiken de Azure PowerShell-cmdlets niet alle eigenschappen van een lokaal vaste volume wijzigen als de ongewenste gevolgen van het wijzigen van het volumetype heeft tot doorverbonden.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Vragen over back-ups van een lokaal vaste volume

**Q.** Zijn lokale momentopnamen van lokaal vastgezette volumes ondersteund?

**A.** Ja, u kunt momentopnamen lokale van uw lokaal vastgezette volumes. Echter, het beste regelmatig back-up van uw lokaal vastgezette volumes met cloud-momentopnamen om ervoor te zorgen dat uw gegevens zijn beveiligd in de deze van een ramp.

**Q.** Bestaan er richtlijnen voor het beheren van lokale momentopnamen voor lokaal vastgezette volumes?

**A.** Vaak lokale snapshots samen met een hoge frequentie van lospeuteren van gegevens in een lokaal vaste volume mogelijk lokale ruimte op het apparaat snel worden verbruikt en er gegevens van gelaagde volumes wordt geduwd naar de cloud. Daarom stellen we voor dat u het aantal lokale momentopnamen minimaliseren.

**Q.** Ik krijg een waarschuwing met de melding dat mijn lokale momentopnamen van lokaal vastgezette volumes kunnen ongeldig worden gemaakt. Wanneer kan dit gebeuren?

**A.** Vaak lokale snapshots samen met een hoge frequentie van lospeuteren van gegevens in een lokaal vaste volume mogelijk lokale ruimte op het apparaat snel worden verbruikt. Als de lokale lagen van het apparaat intensief gebruikt worden, een uitgebreide wolk storing kan ertoe leiden dat het apparaat vol raken en binnenkomende schrijfbewerkingen naar het volume kunnen leiden tot ongeldigmaking van de momentopnamen (als er geen schijfruimte beschikbaar is om te werken de momentopnamen om te verwijzen naar de oudere blokken gegevens overschreven). In een dergelijke situatie schrijfbewerkingen naar het volume zal blijven worden aangeboden, maar de lokale momentopnamen is mogelijk ongeldig. Er is geen invloed op uw bestaande wolk momentopnamen.

De waarschuwing van de waarschuwing is om u te informeren dat een dergelijke situatie kan zich voordoen en dat hetzelfde adres tijdig door lokale momentopnamen planningen om minder vaak lokale momentopnamen te bekijken of verwijderen van oudere lokale momentopnamen die niet langer nodig zijn.

Als de lokale momentopnamen ongeldig zijn geworden, ontvangt u een informatie-waarschuwing waarschuwen dat de lokale momentopnamen voor de specifieke back-beleid zijn ongeldig gemaakt naast de lijst van de tijdstempel van de lokale momentopnamen die ongeldig zijn gemaakt. Deze momentopnamen worden automatisch verwijderd en is niet langer in de **Catalogussen back-up** -pagina in de portal voor Azure klassieke weergeven.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Vragen over het converteren van een gelaagde volume naar een lokaal vaste volume

**Q.** Ik heb sommige traagheid op het apparaat waar te nemen bij het converteren van een gelaagde volume naar een lokaal vaste volume. Hoe komt dat? 

**A.** Het conversieproces verloopt in twee stappen: 

  1. Inrichten van de ruimte op het apparaat voor het snel-te-worden-geconverteerd lokaal vastgemaakt volume.
  2. Gelaagde gegevens downloaden vanuit de cloud om lokale garandeert.

Beide stappen zijn lang met bewerkingen die afhankelijk van de grootte van het volume wordt geconverteerd zijn, gegevens op het apparaat en de beschikbare bandbreedte. Als bepaalde gegevens uit een bestaand trapsgewijs geschakelde volumes kunnen als onderdeel van het inrichtingsproces Mors naar de cloud, kan het apparaat verminderde prestaties tijdens deze periode kan optreden. Bovendien tijdens de conversie duurt mogelijk langer als:

- Bestaande volumes is geen reservekopie naar de cloud; dus het is raadzaam back-up van de volumes alvorens tot een conversie.

- Bandbreedte bandbreedteregeling beleid zijn toegepast, die mogelijk beperken de beschikbare bandbreedte naar de cloud; Daarom raden we aan er een speciale 40 Mbps of meer verbindingen naar de cloud.

- Tijdens het conversieproces kunt nemen enkele uren als gevolg van meerdere factoren uiteengezet; Daarom stellen we voor dat u deze bewerking niet pieken momenten of op een weekend om te voorkomen dat de gevolgen voor het einde consumenten.

Meer informatie over het [converteren van een gelaagde volume naar een lokaal vaste volume](storsimple-manage-volumes-u2.md#change-the-volume-type)

**Q.** Kan ik de conversiebewerking volume annuleren?

**A.** Nee, u kunt niet de Annuleerknop de conversiebewerking eenmaal is gestart. Zoals besproken in de vorige vraag, moet je erop letten van potentiële prestatieproblemen die mogelijk tijdens het proces optreden en volg de aanbevolen procedures wanneer u van plan uw conversie bent bovenstaande.

**Q.** Wat gebeurt er met mijn volume als de conversie mislukt?

**A.** Volumeconversie kan mislukken als gevolg van problemen met de cloud. Het apparaat niet meer tijdens het conversieproces uiteindelijk na een aantal mislukte pogingen om de doorverbonden gegevens vanuit de cloud. In een dergelijke situatie, het volumetype blijft het brontype volume vóór conversie en:

- Een kritieke waarschuwing zal worden verhoogd om te sturen over de fout bij het converteren van het volume. Meer informatie over [waarschuwingen betrekking heeft op een lokaal vaste volumes](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Als u een trapsgewijs geschakeld naar een lokaal vaste volume converteert, blijft het volume vertonen eigenschappen van een gelaagde volume als de gegevens nog steeds op de cloud kan staan. Wij stellen voor dat u problemen met het en probeer de conversiebewerking.
 
- Ook wanneer de conversie van een lokaal vastgemaakt aan een gelaagde volume uitvalt, hoewel het volume wordt gemarkeerd als een lokaal vaste volume, werkt het als een gelaagde volume (omdat de gegevens kan zich hebben verspreid naar de cloud). Het blijft echter nemen ruimte op de lokale lagen van het apparaat. Deze ruimte is alleen beschikbaar voor andere volumes lokaal vastgemaakt. Wij stellen voor dat u opnieuw deze bewerking proberen om ervoor te zorgen dat de volumeconversie voltooid is en de lokale ruimte op het apparaat kan komen.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Vragen over het herstellen van een lokaal vaste volume

**Q.** Lokaal vastgezette volumes direct hersteld worden?

**A.** Ja, lokaal vastgezette volumes worden direct hersteld. Zodra u de metagegevens voor het volume wordt opgehaald uit de cloud als onderdeel van de bewerking voor terugzetten, wordt het volume on line wordt gebracht en kan worden benaderd door de host. Evenwel verminderd lokale garanties voor het volume gegevens niet meer aanwezig totdat u alle gegevens uit de cloud is gedownload en kunnen zich op deze volumes voor de duur van het herstellen van de prestaties.

**Q.** Hoe lang duurt het voordat een lokaal vaste volume herstellen?

**A.** Lokaal vastgezette volumes zijn direct hersteld en on line worden gebracht als de metagegevens van het volume wordt opgehaald uit de cloud, terwijl de gegevens op het volume blijft op de achtergrond worden gedownload. Dit laatste deel van de bewerking voor terugzetten--ophalen terug van de lokale garanties voor gegevens op het volume--is een langdurige bewerking en duurt enkele uren voor de gegevens lokaal opnieuw worden gemaakt. De tijd voor het voltooien van dezelfde hangt af van meerdere factoren, zoals de grootte van het volume wordt hersteld en de beschikbare bandbreedte. Als het oorspronkelijke volume dat wordt hersteld is verwijderd, wordt extra tijd nemen de lokale om ruimte te maken op het apparaat als onderdeel van de bewerking voor terugzetten.

**Q.** Ik wil mijn bestaande lokaal vaste volume terugzetten naar een oudere momentopname (als het volume is doorverbonden). Het volume teruggezet zoals in dit geval worden doorverbonden?

**A.** Nee, het volume wordt hersteld als een lokaal vaste volume. Hoewel de momentopname datums en de tijd waarop het volume is trapsgewijs geschakeld bij het herstellen van bestaande volumes StorSimple altijd gebruik van het type volume op de schijf zoals het momenteel is.

**Q.** Ik heb onlangs mijn lokaal vaste volume uitgebreid, maar nu moet ik de gegevens terugzetten op een tijdstip wanneer het volume kleiner was. Wordt herstellen de grootte van het huidige volume en moet ik de grootte van het volume uitbreiden nadat het terugzetten is voltooid?

**A.** Ja, het terugzetten wordt de grootte van het volume en moet u de grootte van het volume uitbreiden nadat het terugzetten is voltooid.

**Q.** Kan ik het type van een volume tijdens het terugzetten wijzigen?

**A.** Nee, kunt u de volumetype niet wijzigen tijdens het terugzetten.

- Volumes die zijn verwijderd, hersteld als het type dat is opgeslagen in de momentopname.

- Bestaande volumes worden hersteld op basis van het huidige type, ongeacht het type in de momentopname is opgeslagen (Zie de vorige twee vragen).
 
**Q.** Ik wil mijn lokaal vaste volume herstellen, maar ik heb een onjuist point in time momentopname. Kan ik de huidige bewerking voor terugzetten Annuleren?

**A.** Ja, kunt u een doorlopende bewerking annuleren. De status van het volume wordt teruggedraaid naar de staat aan het begin van het terugzetten. Alle schrijfbewerkingen die zijn aangebracht aan het volume terwijl de herstelbewerking werd uitgevoerd niet verloren.

**Q.** Ik begon een bewerking voor terugzetten op een van mijn lokaal vastgezette volumes en nu een momentopname wordt weergegeven in de catalogus van mijn reserve die ik niet nu maken. Waar wordt dit voor gebruikt?

**A.** Dit is de tijdelijke momentopname die is gemaakt voordat de bewerking voor terugzetten en voor ongedaan maken wordt gebruikt wanneer de herstelbewerking is geannuleerd of mislukt. Verwijder niet deze momentopname; Deze worden automatisch verwijderd als de herstelbewerking voltooid is. Dit probleem kan optreden als uw terugzettaak alleen lokaal volumes of een combinatie van lokaal vastgemaakt en trapsgewijs geschakelde volumes vastgemaakt is. Als de terugzettaak alleen trapsgewijs geschakelde volumes bevat, zal niet dit probleem optreden.

**Q.** Kan ik een lokaal vaste volume klonen?

**A.** Ja, u kunt. Het lokaal vaste volume wordt echter als een volume van gelaagde standaard worden gekloond. Meer informatie over het [klonen van een lokaal vaste volume](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Vragen over storing worden overgenomen van een lokaal vaste volume

**Q.** Ik wil mijn apparaat naar een ander fysiek apparaat failover. Mijn lokaal vastgezette volumes mislukt dan lokaal vastgemaakt of doorverbonden?

**A.** Afhankelijk van de versie van de software van het doelapparaat, lokaal vastgezette volumes heeft overgenomen als:

- Lokaal vastgehouden als het doelapparaat StorSimple 8000 bijwerken series 2
- Als het doelapparaat StorSimple doorverbonden bijwerken 8000-serie 1.x
- Als het doelapparaat het toestel wolk is doorverbonden (softwareupdate van versie 2 of 1.x bijwerken)

Meer informatie over [failover en DR van lokale volumes in versies vastgemaakt](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**Q.** Lokaal vastgezette volumes direct hersteld tijdens een noodherstel (DR)?

**A.** Ja, lokaal vastgezette volumes hersteld onmiddellijk tijdens failover. Zodra u de metagegevens voor het volume wordt opgehaald uit de cloud als onderdeel van de failover-bewerking, wordt het volume on line wordt gebracht op het doelapparaat en toegankelijk is voor de host. Ondertussen de volumegegevens nog steeds op de achtergrond gedownload en verminderde prestaties op deze volumes voor de duur van de failover kunnen optreden.

**Q.** Ik zie de failover-taak is voltooid, hoe kan ik de voortgang bijhouden van lokaal vaste volume dat wordt hersteld op het doelapparaat?

**A.** Tijdens een failover-bewerking is de failover-taak gemarkeerd als voltooid zodra alle volumes in de failover-set zijn direct hersteld en on line worden gebracht op het doelapparaat. Dit omvat alle lokaal vastgezette volumes die mogelijk is failover; echter, lokale garanties van de gegevens alleen beschikbaar zal zijn wanneer de gegevens voor het volume is gedownload. Dit proces voor elk lokaal vaste volume dat is mislukt door het toezicht op de bijbehorende terugzettaken die zijn gemaakt als onderdeel van de failover-functie kunt u bijhouden. Deze afzonderlijke terugzettaken wordt alleen voor lokaal vastgezette volumes worden gemaakt.

**Q.** Kan ik het type van een volume wijzigen tijdens een overname?

**A.** Nee, kunt u het volumetype tijdens een failover veranderen. Als het u lukt niet via een ander fysiek apparaat waarop StorSimple 8000 serie update 2, de volumes wordt failover uitgevoerd op basis van het volume in de momentopname wordt opgeslagen. Als u niet een andere apparaatversie, raadpleegt u de vraag boven op het volumetype na een failover.

**Q.** Kan ik een container volume met lokaal vastgezette volumes op het toestel wolk failover?

**A.** Ja, u kunt. De volumes lokaal vastgemaakt kan niet als trapsgewijs geschakelde volumes. Meer informatie over [failover en DR van lokale volumes in versies vastgemaakt](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


