<properties 
   pageTitle="Aanbevolen procedures voor het virtuele matrix StorSimple | Microsoft Azure"
   description="Beschrijving van de best practices voor implementatie en beheer van de virtuele matrix StorSimple."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>Aanbevolen procedures voor StorSimple virtuele matrix

## <a name="overview"></a>Overzicht

Microsoft Azure StorSimple virtuele matrix is een geïntegreerde oplossing die opslagtaken tussen een virtueel apparaat van op gebouwen uitgevoerd in een hypervisor en Microsoft Azure cloud opslag beheert. StorSimple virtuele matrix is een efficiënte, rendabele alternatief voor de fysieke matrix 8000-serie. De virtuele matrix kunt uitvoeren op uw bestaande infrastructuur van hypervisor ondersteunt zowel de iSCSI- en SMB-protocollen en is zeer geschikt voor externe/kantoor office-scenario's. Ga naar [Microsoft Azure StorSimple-overzicht](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx)voor meer informatie over de StorSimple oplossingen.

Dit artikel heeft betrekking op de best practices die tijdens de eerste installatie, implementatie en beheer van de virtuele matrix StorSimple geïmplementeerd. Deze aanbevolen procedures bieden gevalideerde richtlijnen voor de installatie en het beheer van uw virtuele matrix. Dit artikel is gericht op de IT-beheerders die implementeren en beheren van de virtuele matrices in hun datacentra.

Wij raden aan een periodieke evaluatie van de beste praktijken om ervoor te zorgen dat het apparaat nog steeds in overeenstemming is wanneer er wijzigingen zijn aangebracht in de setup- of stroom. U moet of er problemen optreden tijdens het implementeren van deze best practices op uw virtuele matrix, [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md) voor hulp.

## <a name="configuration-best-practices"></a>Aanbevolen procedures voor configuratie 

Deze beste praktijken betrekking hebben op de richtsnoeren die moeten worden gevolgd tijdens de eerste installatie en implementatie van de virtuele-arrays. Deze best practices opnemen die verband houden met het inrichten van de virtuele machine, instellingen voor Groepsbeleid, formaat, het instellen van het netwerk opslag accounts configureren en shares en volumes voor de virtuele matrix maken. 

### <a name="provisioning"></a>Inrichten 

Virtuele StorSimple-matrix is een virtuele machine (VM) ingericht op de hypervisor (Hyper-V of VMware) van uw hostserver. Zorg ervoor dat de host kan voldoende middelen reserveren tijdens het inrichten van de virtuele machine. Voor meer informatie gaat u naar de [minimale resourcevereisten](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) inrichten van een matrix. 

De volgende aanbevolen procedures implementeren tijdens het inrichten van de virtuele matrix:


|                        | Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Virtuele machinetype**   | **Generatie 2** VM voor gebruik met Windows Server 2012 of later en een afbeelding van de *.vhdx* . <br></br> **Generation 1** VM voor gebruik met een Windows Server 2008 of hoger en een *.vhd* -installatiekopie.                                                                                                              | Gebruik virtuele machine versie 8-11 wanneer *.vmdk* afbeelding.                                                                      |
| **Geheugentype**            | Als de **statische geheugen**configureren. <br></br> Gebruik niet de optie **dynamische geheugen** .            |                                                    |
| **Schijf-gegevenstype**         | Als een **dynamisch uitbreidbare**inrichten.<br></br> **Vaste grootte** kan lang duren. <br></br> Gebruik niet de optie **differentiëren** .                                                                                                                   | Gebruik de optie **dunne bepaling** .                                                                                      |
| **Wijziging van de schijf gegevens** | Uitbreiden of verkleinen is niet toegestaan. Een poging om dit te doen, leidt tot het verlies van de lokale gegevens op het apparaat.                       | Uitbreiden of verkleinen is niet toegestaan. Een poging om dit te doen, leidt tot het verlies van de lokale gegevens op het apparaat. |

### <a name="sizing"></a>Grootte aanpassen

Wanneer het formaat van uw virtuele matrix StorSimple rekening houden met de volgende factoren:

- Lokale reservering voor volumes of shares. Ongeveer 12% van de ruimte is gereserveerd op de lokale laag voor elk ingericht gelaagde volume of share. 10% van de ruimte is ongeveer ook gereserveerd voor een lokaal vaste volume voor het bestandssysteem.
- Snapshot-overhead. Ongeveer is 15% ruimte op het lokale niveau gereserveerd voor momentopnamen.
- Noodzaak om te herstellen. Als dit terugzetten als een nieuwe bewerking, grootte moet rekening houden de benodigde ruimte voor terugzetten. Terugzetten is gedaan op een share of -volume van dezelfde grootte of groter.
- Buffer moet worden toegewezen voor een onverwachte groei.

Op basis van de bovenstaande factoren, worden de eisen van de formaatgrepen weergegeven door de volgende vergelijking:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


De volgende voorbeelden illustreren hoe u kunt het formaat van een virtuele matrix op basis van uw vereisten.

#### <a name="example-1"></a>Voorbeeld 1:
Uw virtuele matrix wilt kunnen 

- inrichten van een 2 TB gelaagde volume of share.
- inrichten van een 1 TB gelaagde volume of share.
- inrichten van een lokaal vaste volume van 300 GB of delen.


Voor de voorgaande volumes of shares laat ons berekenen de schijfruimte op het lokale niveau. 

Ten eerste voor elk trapsgewijs geschakeld volume/share zijn lokale reservering gelijk aan 12% van de grootte van de volume/share. Lokale reservering is voor een lokaal vaste volume/share, 10% van de grootte van het lokaal vaste volume/share (naast de ingerichte grootte). In dit voorbeeld moet u

- 240 GB lokale reservering (trapsgewijs opgesteld volume/share voor een 2 TB)
- Lokale reservering 120 GB (trapsgewijs opgesteld volume/share voor een 1 TB)
- 330 GB voor lokaal vaste volume of delen (10% van de lokale reservering toevoegen aan de grootte van 300 GB ingericht)

De totale ruimte vereist op de lokale laag tot nu toe is: 240 GB + 120 GB + 330 GB = 690 GB.

Ten tweede moeten we ten minste zo veel mogelijk ruimte op het lokale niveau als de grootste één reservering. Dit extra bedrag wordt gebruikt voor het geval u nodig hebt om te herstellen van een momentopname van een wolk. In dit voorbeeld is de grootste lokale reservering 330 GB (met inbegrip van de reservering voor het bestandssysteem), zodat u dat de 690 GB: 690 GB + 330 GB = 1020 GB toevoegen.
Als we herstelt de volgende aanvullende uitgevoerd, kunnen we altijd de ruimte van de vorige bewerking voor terugzetten gratis.

Ten derde moeten we 15% van de totale lokale opslagruimte tot nu toe voor lokale snapshots, zodat slechts 85% van het beschikbaar is. In dit voorbeeld, dat is rond 1020 GB = 0.85&ast;ingerichte gegevens TB schijf. Dus de gegevensschijf ingericht zou zijn (1020&ast;(1/0.85)) = 1200 GB = 1,20 TB ~ 1,25 TB (afronden op het dichtstbijzijnde kwartiel)

Factoring in onverwachte groei en nieuwe herstelt, moet u een lokale schijf van rond inrichten 1,25-1,5 TB.

> [AZURE.NOTE] Ook aangeraden dat het lokale schijf dun is ingericht. Deze aanbeveling is, omdat de ruimte voor terugzetten is alleen nodig als u gegevens wilt terugzetten die ouder is dan vijf dagen. Op itemniveau herstel kunt u voor het terugzetten van gegevens voor de laatste vijf dagen zonder extra ruimte voor terugzetten.

#### <a name="example-2"></a>Voorbeeld 2: 
Uw virtuele matrix wilt kunnen 

- inrichten van een 2 TB doorverbonden volume
- 300 GB lokaal vastgemaakt volumes creëren

Op basis van 12% van de lokale ruimte reserveren voor trapsgewijs geschakelde volumes/aandelen en 10% voor lokaal vastgezette volumes/aandelen, moeten we

- 240 GB lokale reservering (trapsgewijs opgesteld volume/share voor 2 TB)
- 330 GB voor lokaal vaste volume of delen (10% van de lokale reservering toevoegen aan de ruimte 300 GB ingericht)

De totale ruimte vereist op de lokale laag is: 240 GB + 330 GB = 570 GB

De minimale lokale benodigde ruimte voor terugzetten is 330 GB. 

15% van de totale schijfruimte wordt gebruikt om de momentopnamen opgeslagen, zodat alleen 0.85 beschikbaar is. Dus de grootte van de schijf is (900&ast;(1/0.85)) = 1,06 TB ~ 1,25 TB (afronden op het dichtstbijzijnde kwartiel) 

In een onverwachte groei factoring, kunt u een lokale schijf 1,25-1,5 TB inrichten.


### <a name="group-policy"></a>Groepsbeleid

Groepsbeleid is een infrastructuur die het mogelijk maakt voor de uitvoering van specifieke configuraties voor gebruikers en computers. De groepsbeleidsinstellingen bevinden zich in groepsbeleidsobjecten (GPO's) die zijn gekoppeld aan de volgende containers van Active Directory Domain Services (AD DS): sites, domeinen of organisatie-eenheden (OU's). 

Als uw virtuele matrix een domein behoren is, kunnen groepsbeleidsobjecten worden toegepast. Deze groepsbeleidsobjecten kunnen installeren van toepassingen, zoals een antivirusprogramma dat nadelig effect op de werking van de virtuele matrix StorSimple hebben kan.

Daarom raden wij u:

-   Zorg ervoor dat uw virtuele array in de eigen organisatie-eenheid (OU) van Active Directory. 

-   Zorg ervoor dat er geen groepsbeleidsobjecten (GPO's) worden toegepast op uw virtuele matrix. U kunt beleidsovername om ervoor te zorgen dat de virtuele matrix (onderliggend knooppunt) automatisch geen groepsbeleidsobjecten van de bovenliggende site neemt. Ga naar [overname blokkeren](https://technet.microsoft.com/library/cc731076.aspx)voor meer informatie.


### <a name="networking"></a>Netwerken

De configuratie voor uw virtuele matrix gebeurt door middel van het lokale web UI. Een virtuele netwerkinterface is ingeschakeld via de hypervisor waarin de virtuele matrix is ingericht. Gebruik de pagina [Netwerkinstellingen](storsimple-ova-deploy3-fs-setup.md) configureren van de virtuele interface IP-adres, subnetmasker en gateway.  U kunt ook de primaire en secundaire DNS-server, instellingen voor tijd en optionele proxy-instellingen configureren voor uw apparaat. De meeste van de netwerkconfiguratie is een eenmalige installatie. Bekijk de [netwerkvereisten StorSimple](storsimple-ova-system-requirements.md#networking-requirements) vóór de implementatie van de virtuele matrix.

Wanneer u uw virtuele matrix implementeert, raden we u aan deze aanbevolen procedures te volgen:

-   Zorg ervoor dat het netwerk waarin de virtuele matrix altijd geïmplementeerd de capaciteit te reserveren bandbreedte van 5 Mbps Internet (of meer). 

    -   Internet bandbreedte nodig is afhankelijk van de kenmerken van uw werkbelasting en de snelheid van de gegevenswijziging.

    -   De gegevenswijziging die kan worden verwerkt is rechtevenredig met de Internet-bandbreedte. Als u bijvoorbeeld bij het nemen van een back-up een 5 Mbps bandbreedte geschikt voor een gegevenswijziging van ongeveer 18 GB in 8 uur. Met vier keer meer bandbreedte (20 Mbps), kunt u vier keer meer gegevens wijzigen (72 GB) verwerken. 

-   Controleer de verbinding met het Internet altijd beschikbaar is. Enkel geval of onbetrouwbare Internet-verbindingen met de apparaten kunnen leiden tot verlies van toegang tot gegevens in de cloud en kunnen resulteren in een niet-ondersteunde configuratie.

-   Als u van plan bent het apparaat als een iSCSI-server implementeren: 
    -   Wij raden aan dat u de optie **Get IP-adres automatisch** (DHCP) uitschakelen. 
    -   Statische IP-adressen configureren. U moet een primaire en een secundaire DNS-server configureren.

    -   Als meerdere netwerkinterfaces op uw virtuele definieert een matrix, het eerste netwerkinterface (deze interface is standaard **Ethernet**) de cloud kunt bereiken. Om te bepalen welk type verkeer, kunt u meerdere virtuele netwerkinterfaces op uw virtuele matrix (geconfigureerd als een server met iSCSI) maken en die interfaces naar andere subnetten verbinden.

-   De wolk bandbreedte alleen beperken (gebruikt door de virtuele matrix), beperking van de router of de firewall configureren. Als u in de hypervisor beperking definieert, wordt deze alle protocollen zoals iSCSI- en SMB-in plaats van alleen de wolk bandbreedte beperken. 

-   Zorg ervoor dat wanneer de synchronisatie voor hypervisors is ingeschakeld. Als uw virtuele matrix met Hyper-V in de Hyper-V-beheer is geselecteerd, gaat u naar **instellingen &gt; Integration Services**, en ervoor te zorgen dat de **synchronisatie** is ingeschakeld.

### <a name="storage-accounts"></a>Opslag-accounts

StorSimple Virtual Array kunnen worden gekoppeld aan een account één opslag. Deze account opslag kan een automatisch gegenereerde opslag account, een account in het abonnement hetzelfde als de service, of een opslag-account is gekoppeld aan een ander abonnement. Zie voor meer informatie, het [beheren van accounts voor uw array virtuele opslag](storsimple-ova-manage-storage-accounts.md).

Gebruik de volgende aanbevelingen voor opslag accounts die zijn gekoppeld aan uw virtuele matrix.

-   Factor in de maximale capaciteit (64 TB) voor een virtuele matrix en de maximale grootte (500 TB) voor een account voor de opslag bij het koppelen van meerdere virtuele matrices met een opslag voor één account. Dit beperkt het aantal grote virtuele arrays die gekoppeld aan die account opslag op ongeveer 7 worden kunnen.

-   Bij het maken van een nieuwe account voor opslag
    -   We raden aan dat u deze maakt in de regio die het dichtst bij de externe office/filiaal waar uw virtuele matrix StorSimple vertragingstijden minimaliseren is geïmplementeerd.

    -   Houd er rekening mee dat u een opslag-account niet tussen de verschillende regio's verplaatsen. U kunt een service ook niet verplaatsen over abonnementen.

    -   Een opslag gebruiken die redundantie tussen de datacenters implementeert. Geo-redundante opslag (GRS), redundante opslag (ZRS) Zone en lokaal redundante opslag (LRS) alle ondersteund voor gebruik met uw virtuele matrix. Ga naar [Azure storage replicatie](../storage/storage-redundancy.md)voor meer informatie over de verschillende soorten rekeningen voor opslag.


### <a name="shares-and-volumes"></a>Aandelen en volumes

U kunt voor uw virtuele matrix StorSimple aandelen inrichten als deze is geconfigureerd als een bestandsserver en volumes wanneer een iSCSI-server is geconfigureerd. De aanbevolen procedures voor het maken van shares en volumes hebben betrekking op de grootte en het type dat is geconfigureerd.

#### <a name="volumeshare-size"></a>Grootte van de volume/aandeel

U kunt op uw virtuele matrix aandelen inrichten wanneer deze is geconfigureerd als een bestandsserver en volumes wanneer een iSCSI-server is geconfigureerd. De aanbevolen procedures voor het maken van shares en volumes hebben betrekking op de grootte en het type dat is geconfigureerd. 

Houd in gedachten de volgende aanbevolen procedures bij het inrichten van aandelen of de volumes op het virtuele apparaat.

-   De grootte van de ingerichte grootte van een gelaagde share kunnen invloed hebben op de prestaties van trapsgewijs schakelen. Werken met grote bestanden kan leiden tot een langzame laag uit. Als u werkt met grote bestanden, wordt aangeraden dat het grootste bestand kleiner dan 3% van de grootte van de share is.

-   Maximaal 16 volumes/aandelen kan worden gemaakt op de virtuele matrix. Als lokaal wordt vastgehouden, worden de volumes/aandelen tussen 50 GB tot 2 TB. Als geschakeld, zijn de volumes/aandelen tussen 500 GB tot 20 TB. 

-   Bij het maken van een volume, factor in de verwachte gegevens verbruik als toekomstige groei. Terwijl het volume kan niet later worden uitgebreid, kunt u altijd een groter volume herstellen.

-   Wanneer u het volume hebt gemaakt, kunt u de grootte van het volume op de StorSimple niet verkleinen.
   
-   Bij het schrijven naar een gelaagde volume op StorSimple, als het volumegegevens een bepaalde drempel (ten opzichte van de lokale ruimte gereserveerd voor het volume) bereikt, is de snelheid van de i/o. Door te gaan naar het volume schrijven vertraagt de i/o aanzienlijk. Hoewel u kunt schrijven naar een gelaagde volume dan de ingerichte capaciteit (we niet actief stopt de gebruiker schrijven de ingerichte capaciteit), ziet u een waarschuwing dat u hebt oversubscribed. Nadat u de waarschuwing ziet, is het absoluut noodzakelijk dat u nemen herstelmaatregelen verwijderen van gegevens op het volume of het volume naar een groter volume terugzet (volume expansion wordt momenteel niet ondersteund).

-   Voor disaster recovery use-cases, zoals het aantal toegestane aandelen/volumes 16 is en het maximum aantal aandelen/volumes die kunnen worden verwerkt parallel ook 16 is, heeft het aantal aandelen/volume geen invloed op de vrijgegeven Productieorder en RTOs. 

#### <a name="volumeshare-type"></a>Type volume/aandeel

StorSimple ondersteunt twee volume/share op basis van het verbruik: lokaal vastgemaakt en lagen. Lokaal vastgezette volumes/aandelen zijn dik ingericht dat de trapsgewijs geschakelde volumes/aandelen dun zijn ingericht. 

Wij raden u aan de volgende best practices te implementeren, bij het configureren van StorSimple volumes/aandelen:

-   Geef het volume dat op basis van de werklast die u implementeren wilt voordat u een volume maken. Lokaal gebruik vastgemaakt volumes voor werkbelasting die lokale garanties van gegevens vereist (zelfs tijdens een stroomstoring wolk) en die laag wolk vertragingstijden vereist. Wanneer u een volume op uw virtuele matrix hebt gemaakt, kunt u het volumetype niet wijzigen vanuit lokaal vastgemaakt aan gelaagde of *omgekeerd*. Als u bijvoorbeeld lokaal vastgezette volumes maken bij de implementatie van SQL-werklasten of werklasten host voor virtuele machines (VMs); bestand delen werklasten trapsgewijs geschakelde volumes gebruiken.

-   Schakel het selectievakje voor de minder vaak gebruikte gegevens archiveren bij het omgaan met grote bestanden. Deduplication chunk groter van 512 K wordt gebruikt wanneer deze optie is ingeschakeld voor het vergemakkelijken van de overdracht van gegevens naar de cloud.

#### <a name="volume-format"></a>Volume-indeling

Nadat u StorSimple volumes op uw iSCSI-server maken, die u wilt initialiseren en formatteren van volumes koppelen. Deze bewerking wordt uitgevoerd op de host die is aangesloten op uw apparaat StorSimple. Volgende procedures worden aanbevolen als montage en formatteren van volumes op de host StorSimple.

-   Een snelle formattering uitvoert op alle StorSimple volumes.

-   Bij het opmaken van een volume StorSimple, gebruik een clustergrootte (AUS) van 64 KB (de standaardwaarde is 4 KB). De 64 KB AUS is gebaseerd op tests uitgevoerd binnen het bedrijf voor algemene StorSimple werkbelasting en andere werkbelasting.

-   Wanneer u de StorSimple virtuele matrix geconfigureerd als een iSCSI-server, gebruik geen spanned volumes en dynamische schijven als deze volumes of schijven worden niet ondersteund door StorSimple.

#### <a name="share-access"></a>Gedeelde toegang

Bij het maken van shares op de server van uw virtuele matrix, volg de volgende richtlijnen:

-   Bij het maken van een share toewijzen een gebruikersgroep als beheerder van een share in plaats van één gebruiker.

-   U kunt de NTFS-machtigingen beheren nadat de share is gemaakt door het bewerken van de aandelen in Windows Verkenner.

#### <a name="volume-access"></a>Tot een volume

Bij het configureren van de iSCSI-volumes op uw virtuele Array StorSimple, is het belangrijk dat u waar nodig de toegang beheren. Om te bepalen welke hostservers volumes kunnen openen, maken en records uit access control (ACRs) koppelen aan StorSimple volumes.

Gebruik de volgende aanbevolen procedures wanneer ACRs StorSimple volumes configureren:

-   Altijd ten minste één ACR aan een volume koppelen.

-   Meerdere ACRs alleen definiëren in een geclusterde omgeving.

-   Wanneer meer dan één ACR toe te wijzen aan een volume, ervoor te zorgen dat het volume niet beschikbaar is op een manier waarbij tegelijkertijd door meer dan één niet-geclusterde host kan worden geopend. Als u meerdere ACRs hebt toegewezen aan een volume, verschijnt verschijnt er een waarschuwingsbericht voor u uw configuratie controleren.

### <a name="data-security-and-encryption"></a>Beveiliging en codering

Uw virtuele matrix StorSimple heeft beveiligings- en functies die de vertrouwelijkheid en integriteit van uw gegevens te waarborgen. Als u deze functies gebruikt, kunt u het beste deze aanbevolen procedures te volgen: 

-   Definieer een cloud opslag coderingssleutel voor het genereren van AES-256 versleuteling voordat de gegevens van uw virtuele array wordt verzonden naar de cloud. Deze sleutel is niet vereist als uw gegevens worden gecodeerd, beginnen. De sleutel kan worden gegenereerd en veilig gebruik van een systeem voor sleutelbeheer zoals [Azure sleutel kluis](../key-vault/key-vault-whatis.md)bewaard.

-   Wanneer u de account opslag via de StorSimple Manager-service configureert, moet het SSL-modus voor het maken van een beveiligd kanaal voor communicatie tussen het apparaat StorSimple en de cloud in te schakelen.

-   Opnieuw genereren de sleutels voor uw opslag rekeningen (toegang verkrijgen tot de Azure Storage-service) periodiek aan de account voor toegang tot wijzigingen op basis van de gewijzigde lijst met beheerders.

-   Gegevens op uw virtuele array wordt gecomprimeerd en deduplicated voordat het wordt verzonden naar Azure. Gebruik de functieservice Data Deduplication op de Windows Server-host aanbevolen niet.


## <a name="operational-best-practices"></a>Aanbevolen procedures

De aanbevolen procedures zijn richtlijnen die moeten worden gevolgd tijdens het dagelijks beheer of de werking van de virtuele array. Deze praktijken betrekking hebben op specifieke beheertaken uitvoeren zoals het maken van back-ups terugzetten vanaf een back-upset een failover uitvoeren, uitschakelen en verwijderen van de matrix, het toezicht op het gebruik en de gezondheid en het virus wordt uitgevoerd worden op uw virtuele matrix.

### <a name="backups"></a>Back-ups

De gegevens op uw virtuele array is een back-up naar de cloud op twee manieren, een standaard automatische dagelijkse back-up van het hele apparaat op 22:30 of via een handmatige back-up op verzoek wordt gestart. Het apparaat maakt automatisch dagelijkse wolk momentopnamen van alle gegevens die zich op deze standaard. Ga voor meer informatie naar [back-up van uw virtuele matrix StorSimple](storsimple-ova-backup.md).

De frequentie en vasthouden die is gekoppeld aan de standaard back-ups kunnen niet worden gewijzigd, maar kunt u de tijd waarop de dagelijkse back-ups elke dag worden geïnitieerd. Bij het configureren van de begintijd voor de geautomatiseerde back-ups wordt aangeraden:

-   Uw back-ups plannen voor daluren. Back-begintijd moet niet samenvallen met talrijke host IO.

-   Een handmatige back-up op verzoek starten bij het plannen van een failover-apparaat of voorafgaand aan het onderhoudsvenster ter bescherming van de gegevens op uw virtuele matrix uit te voeren.

### <a name="restore"></a>Herstellen

U kunt terugzetten vanaf een back-up op twee manieren instellen: herstellen naar een ander volume of share of een item herstelt (alleen beschikbaar in een virtuele matrix die is geconfigureerd als bestandsserver) uit te voeren. Op itemniveau herstel kunt u een granulair herstel van bestanden en mappen vanaf een cloud back-up van alle shares op het apparaat StorSimple doen. Voor meer informatie gaat u naar [een back-up terugzetten](storsimple-ova-restore.md).

Wanneer u een herstelbewerking uitvoert, houd de volgende richtlijnen:

-   Uw virtuele StorSimple-Array biedt geen ondersteuning voor op de plaats terugzetten. Dit kan echter gemakkelijk worden bereikt door een proces: de ruimte op de virtuele array en zet naar een ander volume/share maken.

-   Wanneer het terugzetten vanaf een lokaal volume moet u rekening houden zijn het herstellen een langdurige bewerking. Hoewel het volume kan snel on line worden gebracht, blijft de gegevens op de achtergrond worden gehydrateerd.

-   Het volumetype blijft ongewijzigd tijdens het terugzetten. Een gelaagde volume wordt hersteld naar een ander volume van gelaagde en een lokaal vaste volume naar een ander lokaal volume vastgemaakt.

-   Als u een volume of een share terugzetten vanaf een back-up wilt instellen als de opdracht niet werkt, een doelvolume of share kan nog steeds worden gemaakt in de portal. Het is belangrijk dat u dit doelvolume niet-gebruikte verwijderen of in de portal delen te minimaliseren die voortvloeien uit dit element toekomstige problemen.

### <a name="failover-and-disaster-recovery"></a>Failover en noodherstel

Een failover van het apparaat kunt u uw gegevens migreren van een *bron* -apparaat in het datacenter naar *een ander apparaat zich bevindt in dezelfde of een andere geografische locatie* . De overname van het apparaat is voor het gehele apparaat. Tijdens een overname, de cloud-gegevens voor de Bronapparaat verandert eigendom van het doelapparaat.

Voor uw virtuele matrix StorSimple kunt u alleen failover uitvoeren naar een andere virtuele matrix beheerd door dezelfde StorSimple Manager-service. Een failover naar een apparaat 8000-serie of een matrix die wordt beheerd door een andere service Manager StorSimple (dan die voor het bronapparaat) is niet toegestaan. Meer informatie over overwegingen met betrekking tot de failover, gaat u naar de [vereisten voor de overname van het apparaat](storsimple-ova-failover-dr.md).

Bij het uitvoeren van een fail over voor uw virtuele matrix, houd het volgende in gedachten:

-   Het is aanbevolen best practice te nemen van alle volumes/aandelen offline alvorens tot failover voor een geplande failover. Volg de instructies van het besturingssysteem-specifieke eerste volumes/aandelen off line nemen op de host en vervolgens de off line op het virtuele apparaat.

-   Voor een bestand server disaster recovery (DR) raden wij u aan het doelapparaat naar hetzelfde domein als de bron te koppelen zodat de sharemachtigingen automatisch opgelost worden. Failover naar een doelapparaat in hetzelfde domein wordt ondersteund in deze release.

-   Zodra de DR is voltooid, wordt het bronapparaat automatisch verwijderd. Als het apparaat niet meer beschikbaar is, wordt de virtuele machine die u op het hostsysteem ingericht nog bronnen verbruiken. Wij raden u aan dat deze virtuele machine te verwijderen van uw hostsysteem om te voorkomen dat eventuele toeslagen toerekenen.

-   Merk op dat zelfs als de failover-functie mislukt is, **de gegevens altijd veilig in de cloud is**. Houd rekening met de volgende drie scenario's waarin de overname is niet voltooid:

    -   Er is een fout opgetreden in de eerste stadia van de failover, zoals wanneer de DR voorafgaande controles worden uitgevoerd. In dit geval is het doelapparaat nog steeds bruikbaar. U kunt opnieuw proberen de failover-functie op hetzelfde doelapparaat.

    -   Er is een fout opgetreden tijdens het werkelijke failover. In dit geval is het doelapparaat gemarkeerd als onbruikbaar. U moet inrichten en virtuele matrix van een ander doel configureren en gebruiken die voor failover.

    -   Failover is volledige waarvan de Bronapparaat is verwijderd, maar het doelapparaat problemen heeft en u geen toegang tot gegevens. De gegevens in de wolk nog steeds veilig is en gemakkelijk kan worden opgehaald door een andere virtuele matrix maken en vervolgens wordt gebruikt als een apparaat voor de DR.

### <a name="deactivate"></a>Deactiveren

Als u een virtuele matrix StorSimple deactiveert, verbreekt u de verbinding tussen het apparaat en de bijbehorende StorSimple Manager-service. Deactiveren niet **ongedaan** maken en kan niet ongedaan worden gemaakt. Een gedeactiveerde apparaat kan niet worden geregistreerd in de StorSimple Manager-service opnieuw. Ga naar [deactiveren en uw virtuele matrix StorSimple verwijderen](storsimple-deactivate-and-delete-device.md)voor meer informatie.

Houd de volgende richtlijnen bij het deactiveren van uw virtuele array:

-   Een wolk momentopname van de gegevens voor het deactiveren van een virtueel apparaat. Als u een virtuele matrix deactiveert, zijn alle gegevens van het lokale apparaat gaat verloren. Een momentopname van de wolk, kunt u gegevens te herstellen in een later stadium.

-   Voordat u een virtuele matrix StorSimple deactiveert, moet stoppen of verwijderen van clients en hosts die afhankelijk van dat apparaat zijn.

-   Een gedeactiveerde apparaat verwijderen als u niet meer gebruikt, zodat deze kosten niet toenemen.

### <a name="monitoring"></a>Monitoring

Om ervoor te zorgen dat uw virtuele matrix StorSimple in orde zijn doorlopend is, moet u de matrix bewaken en ervoor te zorgen dat u informatie uit het systeem inclusief waarschuwingen ontvangen. Implementeren van de volgende aanbevolen procedures voor het controleren van de algehele gezondheid van de virtuele matrix:

- Controle voor het bijhouden van het schijfgebruik van uw data matrix virtuele schijf als de schijf OS configureren Als Hyper-V wordt uitgevoerd, kunt u een combinatie van System Center Virtual Machine Manager (SCVMM) en System Center Operations Manager (SCOM) voor het controleren van uw virtualisatie-hosts.   

- E-mailmeldingen configureren op uw virtuele matrix om waarschuwingen te verzenden op bepaalde niveaus gebruik.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Zoeken op index en virus scan toepassingen

Een virtuele matrix StorSimple kunt automatisch trapsgewijs gegevens uit de lokale laag naar de cloud met Microsoft Azure. Wanneer een toepassing als een zoekactie in de index of een virusscan voor het scannen van de gegevens op de StorSimple wordt gebruikt, moet u ervoor te zorgen dat de wolkengegevens niet ophalen benaderd en terug naar de lokale laag opgehaald.

Het is raadzaam implementeren waarmee u de volgende richtlijnen bij het configureren van de index zoeken of virus scan op uw virtuele array:

-   Alle bewerkingen automatisch geconfigureerde volledige scan uit te schakelen.

-   Voor trapsgewijs geschakelde volumes, configureert u de index zoeken of virus scan toepassing een incrementele scan uit te voeren. Dit zou scannen alleen de nieuwe gegevens waarschijnlijk die zich op het lokale niveau. De gegevens die naar de cloud is doorverbonden is niet toegankelijk tijdens een incrementele.

-   Ervoor te zorgen de juiste zoekfilters en instellingen zijn zo geconfigureerd dat de beoogde soorten bestanden ophalen gescand. Bijvoorbeeld, image-bestanden (JPEG-, GIF- en TIFF-bestand) en technische tekeningen moet niet worden gescand tijdens de incrementele of volledige index opnieuw maken.

Als Windows indexing proces, volg dan deze richtlijnen:

-   Gebruik niet de indexeerfunctie van de Windows voor trapsgewijs geschakelde volumes als er grote hoeveelheden gegevens (TBs) uit de cloud terughalen als de index moet regelmatig opnieuw worden opgebouwd. De index opnieuw ophalen van alle bestandstypen voor het indexeren van de inhoud ervan.

-   Gebruik de Windows indexing proces voor lokaal vastgezette volumes als dit alleen toegang gegevens op de lokale niveaus index tot zou (de wolkengegevens wordt niet geopend) te maken.

### <a name="byte-range-locking"></a>Byte-bereik vergrendelen

Toepassingen kunnen een opgegeven aantal bytes in bestanden vergrendelen. Als byte-bereik vergrendelen is ingeschakeld op de toepassingen die aan uw StorSimple schrijft, werkt vervolgens trapsgewijs schakelen niet op uw virtuele matrix. Voor het trapsgewijs schakelen als u wilt werken, moeten alle gebieden van de bestanden geopend worden ontgrendeld. Byte-bereik vergrendelen wordt met trapsgewijs geschakelde volumes op uw virtuele matrix niet ondersteund.

Aanbevolen maatregelen om het verlichten van de vergrendeling van de byte-bereik omvatten:

-   Bytebereik vergrendelen in uw toepassingslogica uitschakelen.

-   Gebruik lokale volumes vastgehouden (in plaats van trapsgewijs opgesteld) voor de gegevens die aan deze toepassing is gekoppeld. Lokaal vastgezette volumes niet trapsgewijs in de cloud.

-   Wanneer u met behulp van lokale volumes vastgezet met byte-bereik vergrendelen is ingeschakeld, kan het volume on line worden gebracht voordat de herstelbewerking voltooid is. In dergelijke gevallen kunt moet u wachten tot het herstellen volledig.

## <a name="multiple-arrays"></a>Meerdere arrays

Meerdere virtuele matrices moet worden geïmplementeerd voor een groeiende werkset van gegevens die naar de cloud dus invloed op de prestaties van het apparaat kan Mors. In deze gevallen is het beste schaal apparaten als de werkset groeit. Hiervoor moet een of meer apparaten moeten worden toegevoegd in het datacenter van gebouwen. U kunt tijdens het toevoegen van de apparaten:

-   De huidige set gegevens splitsen.
-   Nieuwe werklasten implementeren op nieuwe apparaten.
-   Als u meerdere virtuele arrays, raden we van netwerktaakverdeling perspectief, de matrix verdelen over verschillende hypervisor hosts.

-  Meerdere virtuele matrices (indien geconfigureerd als een bestandsserver of een iSCSI-server) kunnen worden geïmplementeerd in een verspreid bestand systeem Namespace. Ga naar een [Verspreid bestand System Namespace-oplossing met hybride Cloud Storage Deployment Guide](https://www.microsoft.com/download/details.aspx?id=45507)voor gedetailleerde stappen. Distributed File System Replication is momenteel niet aanbevolen voor gebruik met de virtuele matrix. 


## <a name="see-also"></a>Zie ook
Meer informatie over het [beheren van uw virtuele matrix StorSimple](storsimple-ova-manager-service-administration.md) via de StorSimple Manager-service.
