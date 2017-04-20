<properties
    pageTitle="Met behulp van importeren/exporteren om gegevens te verzenden naar een Blob-opslag | Microsoft Azure"
    description="Informatie over het maken, importeren en exporteren van projecten in de portal klassieke Azure blob-opslag gegevens overbrengen."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>De Service Microsoft Azure importeren/exporteren gebruiken om gegevens te verzenden naar een Blob-opslag

## <a name="overview"></a>Overzicht

Azure Service voor importeren/exporteren kunt u grote hoeveelheden gegevens veilig overdragen naar Azure blobopslag door verzending van harde schijven tot een Azure Datacenter. U kunt deze service ook gegevens van Azure blob-opslag overbrengen naar de vaste schijven en verzenden naar uw site op gebouwen. Deze service is geschikt in situaties waar u wilt overbrengen van verschillende TBs van gegevens van of Azure, maar uploaden of downloaden via het netwerk niet haalbaar is vanwege de beperkte bandbreedte of met hoge netwerkkosten.

De service is vereist dat vaste-schijfstations moet BitLocker gecodeerd voor de beveiliging van uw gegevens. De service ondersteunt klassieke opslag rekeningen aanwezig in alle regio's van openbare Azure. U kunt vaste schijven op een van de ondersteunde locaties die verderop in dit artikel moet verzenden.

In dit artikel leert u meer over de service Azure importeren/exporteren en het verzenden van stations voor het kopiëren van gegevens naar en van Azure Blob-opslag.

> [AZURE.IMPORTANT] U kunt maken en beheren, importeren en exporteren van projecten voor klassieke opslag via de portal van de klassieke of de [Import/Export service REST API's](http://go.microsoft.com/fwlink/?LinkID=329099). Resource Manager opslag accounts worden niet ondersteund op dit moment.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Wanneer moet ik de service Azure importeren/exporteren gebruiken?

Kunt u overwegen Azure Import/Export service tijdens het uploaden of downloaden van gegevens via het netwerk is te langzaam of verkrijgen van extra netwerkbandbreedte is veel te duur.

U kunt deze service gebruiken in scenario's zoals:

- Gegevens migreren naar de cloud: snel grote hoeveelheden gegevens gaan naar Azure en efficiënter.
- Distributie van inhoud: snel gegevens verzenden naar de sites van uw klant.
- Back-up: Back-ups van uw gegevens op de locatie op te slaan in Azure blob-opslag nemen.
- Herstel van gegevens: bezorgd bij uw locatie op gebouwen en herstellen van de grote hoeveelheid gegevens die zijn opgeslagen in de blobopslag.

## <a name="pre-requisites"></a>Minimumvereisten

In deze sectie, hebben we de noodzakelijke voorwaarden, vereist voor het gebruik van deze service weergegeven. Controleer deze zorgvuldig voordat u de schijven levert.

### <a name="storage-account"></a>Opslag account

U hebt een abonnement Azure en één of meer **klassieke** opslag rekeningen om de Import/Export-service te gebruiken. Elke taak kan worden gebruikt voor gegevensoverdracht of naar slechts één account van klassieke opslag. Een taak één importeren/exporteren kan niet met andere woorden, meerdere accounts voor opslag overspannen. Zie voor informatie over het maken van een nieuwe account voor de opslag, [het maken van een Account voor opslag](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>BLOB-typen

Service Azure importeren/exporteren kunt u gegevens kopieert naar een BLOB's **blokkeren** of BLOB's **pagina** . Daarentegen kunt u alleen **blok** BLOB's, **pagina** BLOB's of **toevoegquery** BLOB's exporteren vanuit Azure opslag met behulp van deze service.

### <a name="job"></a>Taak

Om te beginnen met het proces van het importeren of exporteren van Blob-opslag, moet u eerst een taak maken. Een taak kan bestaan uit een importeren of een taak exporteren:

- Maak een taak importeren als u overbrengen van gegevens wilt over lokalen om BLOB's in uw account Azure opslag.
- Maak een taak voor het exporteren als u wilt overbrengen van gegevens opgeslagen als BLOB's in uw account opslag voor harde schijven die aan u zijn verzonden.

Wanneer u een taak maakt, melden je bij de Import/Export-service dat u een of meer vaste schijven naar een Azure Datacenter wordt verzonden.

- Voor een taak importeren zal u harde schijven met de gegevens verzenden.
- Voor een taak voor het exporteren, wordt u lege harde schijven verzenden.
- U kunt maximaal 10 harde schijven per project leveren.

U kunt een import maken of taak met de [klassieke portal](https://manage.windowsazure.com/) of de [REST API voor Azure opslag Import/Export](http://go.microsoft.com/fwlink/?LinkID=329099)exporteren.

### <a name="client-tool"></a>Clienthulpprogramma voor

De eerste stap bij het maken van een taak **importeren** is het voorbereiden van het station die worden verzonden voor importeren. Om voor te bereiden op de schijf, moet u het aansluit op een lokale server en het hulpprogramma Azure Import/Export-Client uitvoeren op de lokale server. Dit clienthulpprogramma vereenvoudigt het kopiëren van uw gegevens naar het station, de gegevens op de schijf met BitLocker coderen en genereren bestanden van het station.

De journaal-bestanden bevatten de basisinformatie over de taak en het station bijvoorbeeld station serienummer en de naam van opslag. Deze journal-bestand is niet opgeslagen op het station. Het wordt gebruikt tijdens het maken van de taak importeren. Stap voor stap vindt meer informatie over het maken van taken verderop in dit artikel.

Het clienthulpprogramma is alleen compatibel met 64-bits Windows-besturingssysteem. Zie de sectie [Operating System](#operating-system) voor bepaalde OS-versies die worden ondersteund.

Download de meest recente versie van het [hulpprogramma voor importeren/exporteren van Azure-client](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409). Zie de [Naslaginformatie Azure importeren/exporteren](http://go.microsoft.com/fwlink/?LinkId=329032)voor meer informatie over het hulpprogramma Azure importeren/exporteren.

### <a name="hard-disk-drives"></a>Harde schijven

Slechts 3,5-inch SATA II/III interne harde schijven worden ondersteund voor gebruik met de service voor importeren/exporteren. Harde schijven kunt u maximaal 10TB.
Voor het importeren van taken, worden alleen de eerste gegevensvolume op de schijf verwerkt. Het gegevensvolume moet worden geformatteerd met NTFS.
Bij het kopiëren van gegevens naar de vaste schijf, kunt u rechtstreeks met een SATA-connector koppelen of extern met een externe SATA II/III-USB-adapter kunt u koppelen. Wij raden u aan met een van de volgende externe SATA II/III USB-adapters:

- Anker 68UPSATAA 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon QuickPort XT HC

Als er een converter die hierboven niet wordt vermeld, kunt u proberen het uitvoeren Azure importeren/exporteren met behulp van het conversieprogramma voor het voorbereiden van het station en zien als het werkt voordat u een ondersteunde conversieprogramma aanschaft.

> [AZURE.IMPORTANT] Externe harde schijven die worden geleverd met een ingebouwde USB-adapter worden niet ondersteund door deze service. Ook kan niet de schijf in de behuizing van een externe harde schijf worden gebruikt; Stuur geen externe harde schijven.

### <a name="encryption"></a>Codering

De gegevens op het station moet worden gecodeerd met BitLocker-stationsversleuteling. Zo beveiligt u uw gegevens terwijl deze in transit is.

Er zijn twee manieren voor het uitvoeren van de codering voor het importeren van taken. De eerste manier is via de / parameter coderen bij het uitvoeren van het clienthulpprogramma tijdens de voorbereiding van het station. De tweede manier is het inschakelen van BitLocker-versleuteling handmatig op het station en de coderingssleutel opgeven op de opdrachtregel client tool tijdens het voorbereiden van het station.

Voor het exporteren, nadat de gegevens worden gekopieerd naar de stations, de service wordt het station versleutelen met BitLocker voordat het systeem aan u. De coderingssleutel krijgt u via de klassieke portal.  

### <a name="operating-system"></a>Besturingssysteem

U kunt een van de volgende 64-bits besturingssystemen voorbereiden van de vaste schijf met het hulpprogramma voor het importeren/exporteren van Azure voordat u het station naar Azure levert:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Deze besturingssystemen voor BitLocker-stationsversleuteling.

> [AZURE.IMPORTANT] <sup>1</sup> Download de meest recente versie van het hulpprogramma Azure importeren/exporteren als u een Windows 10 machine voor het voorbereiden van de vaste schijf.

### <a name="locations"></a>Locaties

De Azure Import/Export service ondersteunt het kopiëren van gegevens naar en van alle openbare Azure opslag rekeningen. U kunt vaste schijven op een van de volgende locaties op te leveren. Als je account voor opslag in een openbare Azure locatie die hier niet wordt opgegeven, wordt een alternatieve verzendlocatie verstrekt bij het maken van de taak met de klassieke portal of de Import/Export REST API.

Verzendlocaties ondersteund:

- Oost-Verenigde Staten

- West-Verenigde Staten

- Oost-Verenigde Staten 2

- Centrale VS

- Centrale Noord-Amerikaanse

- Zuid-centraal-Verenigde Staten

- Noord-Europa

- West-Europa

- Oost-Azië

- Zuidoost-Azië

- Australië, Oost

- Zuidoost Australië

- West Japan

- East Japan

- Centraal India


### <a name="shipping"></a>Verzendkosten

**Verzending stations tot het datacenter:**

Bij het maken van een taak importeren of exporteren, krijgt u een verzendadres van een van de ondersteunde locaties op uw schijven verzenden. Het opgegeven verzendadres hangt af van de locatie van uw account voor de opslag, maar deze mogelijk niet hetzelfde als de locatie van uw account.

Vervoerders zoals FedEx, DHL, UPS of de US Postal Service kunt u uw schijven naar het verzendadres verzenden.

**Verzending van het datacenter-stations:**

Bij het maken van een taak importeren of exporteren, moet u opgeven dat een retouradres voor Microsoft levert de stations terug nadat de afdruktaak voltooid is. Zorg ervoor dat u opgeeft dat een geldig adres om te voorkomen dat vertragingen in de verwerking.

U moet ook beschikken over een geldig FedEx of DHL carrier rekeningnummer door Microsoft worden gebruikt voor het verzenden van de schijven terug. Een rekeningnummer FedEx is vereist voor het verzenden van stations van de locaties van de Verenigde Staten en Europa. Een rekeningnummer DHL is vereist voor het verzenden van stations van de locaties van Azië en Australië. U kunt een [FedEx](http://www.fedex.com/us/oadr/) (voor de Verenigde Staten en Europa) of [DHL](http://www.dhl.com/) (Azië en Australië) vervoerder account maken als u niet hebt. Als u al een nummer voor de vervoerder, Controleer of deze geldig is.

In de pakketten verzendt, moet u de voorwaarden op [Microsoft Azure servicevoorwaarden](https://azure.microsoft.com/support/legal/services-terms/)volgen.

> [AZURE.IMPORTANT] Houd er rekening mee dat het fysieke medium dat u levert wellicht meerdere internationale grenzen heen. U bent verantwoordelijk om ervoor te zorgen dat het fysieke medium en de gegevens worden geïmporteerd en/of geëxporteerd volgens de van toepassing zijnde wetten. Neem contact op met uw adviseurs om te verifiëren dat uw media en legaal kan worden verzonden naar het midden van de geïdentificeerde gegevens vóór verzending van het fysieke medium. Dit helpt ervoor te zorgen dat Microsoft heeft bereikt in een tijdige wijze. Elk pakket dat internationale grenzen heen snijden moet bijvoorbeeld een handelsfactuur die gepaard gaan met het pakket (behalve als kruisende randen binnen de Europese Unie). U kan een gevulde kopie van de handelsfactuur van vervoerder website afdrukken. Voorbeeld van commerciële factuur zijn [DHL handelsfactuur] (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) of [FedEx handelsfactuur](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Zorg ervoor dat Microsoft niet is opgegeven als de exporteur.

## <a name="how-does-the-azure-importexport-service-work"></a>Hoe werkt de service Azure importeren/exporteren

U kunt gegevens uitwisselen tussen uw site op gebouwen en Azure blob-opslag met behulp van de service Azure importeren/exporteren door taken te maken en verzenden van harde schijven tot een Azure Datacenter. Elke vaste schijf die u verzendt, is gekoppeld aan één taak. Elke taak is gekoppeld aan een account één opslag. Raadpleeg de [minimumvereisten sectie](#pre-requisites) zorgvuldig door voor meer informatie over de specifieke kenmerken van deze service ondersteund, zoals blob typen schijftypen, locaties en verzending.

In dit gedeelte wordt beschreven op een hoog niveau van de stappen voor importeren en exporteren van taken. We bieden stapsgewijze instructies voor het maken van een taak exporteren verderop in de [sectie snel starten](#quick-start).

### <a name="inside-an-import-job"></a>Binnen een project importeren

Op een hoog niveau omvat een taak importeren de volgende stappen:

- De gegevens worden ingevoerd en het aantal schijven u moet, bepalen.
- Identificeer de blobs bestemming voor de gegevens in de Blob-opslag.
- De gebruiken Azure importeren/exporteren om uw gegevens in een of meer vaste schijven kopiëren en ze te versleutelen met BitLocker.  
- Maak een taak importeren in de doelaccount klassieke opslag via de portal van de klassieke of de Import/Export REST API. Als de klassieke portal, upload bestanden van het station.
- Het retouradres en het rekeningnummer van de vervoerder moet worden gebruikt voor het verzenden van de stations aan u leveren.
- De vaste-schijfstations voor het verzendadres is opgegeven tijdens het maken van taken verzenden.
- De levering is het volgnummer in de details van de taak importeren bijwerken en indienen van de taak importeren.
- Stations worden ontvangen en verwerkt op de Azure Datacenter.
- Schijven worden geleverd met uw account carrier aan het adres van de afzender die in het project importeren.

    ![Figuur 1:Import taak stroom](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>In een taak exporteren

Op een hoog niveau omvat een taak exporteren de volgende stappen:

- De gegevens worden geëxporteerd en het aantal schijven u moet, bepalen.
- De paden van de gegevens in de Blob-opslag container of bron BLOB's identificeren.
- Maak een taak voor het exporteren in de bronaccount opslag via de portal van de klassieke of de Import/Export REST API.
- Geef de bron BLOB's of container paden van de gegevens in het project exporteren.
- Het retouradres en het rekeningnummer voor vervoerder moet worden gebruikt voor het verzenden van de stations aan u leveren.
- De vaste-schijfstations voor het verzendadres is opgegeven tijdens het maken van taken verzenden.
- De levering is het volgnummer in de details van de export bijwerken en indienen van de taak voor het exporteren.
- De stations worden ontvangen en verwerkt op de Azure Datacenter.
- De stations zijn versleuteld met BitLocker; de toetsen zijn beschikbaar via de klassieke portal.  
- De schijven worden geleverd met uw account carrier aan het adres van de afzender die in het project importeren.

    ![Figuur 2:Export taak stroom](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>De status van uw project weergeven

U kunt bijhouden van de status van de importbewerking of taken exporteren uit de Classic-portal. Ga naar uw account voor opslag in de klassieke portal en klik op het tabblad **Importeren/exporteren** . Een lijst van uw taken wordt op de pagina weergegeven. U kunt de lijst op de taakstatus, naam, taaktype of volgnummer filteren.

Ziet u een van de volgende statussen taak waarbij het station is in het proces.

| Taakstatus   | Beschrijving                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Maken     | De taak is gemaakt, maar u hebt nog niet voorzien van uw verzendgegevens.                                                                                                                                                                                                                                                                                                |
| Verzendkosten     | De taak is gemaakt en u hebt uw verzendgegevens verstrekt. **Opmerking**: als het station wordt bezorgd in het datacenter Azure, de status nog steeds 'Verzendkosten' voor enige tijd. Zodra de service wordt gestart voor het kopiëren van uw gegevens, wordt de status gewijzigd in "Overdragen". Als u meer specifieke status van de schijf wilt, kunt u de REST-API voor importeren/exporteren. |
| Overbrengen | Uw gegevens worden overgebracht van de vaste schijf (voor een taak importeren) of op de vaste schijf (voor een taak exporteren).                                                                                                                                                                                                                                                                 |
| Verpakking    | De overdracht van uw gegevens is voltooid en de vaste schijf wordt voorbereid voor verzending aan u.                                                                                                                                                                                                                                                                             |
| Voltooien     | De vaste schijf is naar u verzonden.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Tijd aan project proces

De hoeveelheid tijd die nodig is om het proces een taak importeren/exporteren, afhankelijk van verschillende factoren zoals de verzendtijd is, type, type en grootte van de gegevens worden gekopieerd en de omvang van de schijven die taak. De Import/Export-Service beschikt niet over een SLA. De REST-API kunt u de voortgang van het project beter bijhouden. Er is een parameter voor percentage voltooid in de lijst taken bewerking een indicatie van de kopie geeft. Bereiken ons als u een raming voor het voltooien van een taak tijd kritiek importeren/exporteren.

### <a name="pricing"></a>Prijzen

**Station-en verpakkingskosten**

Er is een station verzendkosten voor elk station verwerkt als onderdeel van het importeren of exporteren van de taak. Zie de details over de [Prijzen van Azure importeren/exporteren](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Verzendkosten**

Wanneer u stations Azure verzendt, kunt u de verzendkosten betalen aan de vervoerder. Wanneer Microsoft de stations aan u retourneert, de verzendkosten in rekening gebracht aan de vervoerder-account die u hebt opgegeven op het moment van de schepping van werkgelegenheid.

**Transactiekosten**

Er zijn geen transactiekosten bij het importeren van gegevens in de blob-opslag. De standaard egress toeslagen zijn van toepassing wanneer de gegevens worden geëxporteerd uit de blobopslag. Zie voor meer informatie over de transactiekosten, [gegevensoverdracht prijzen.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Snel starten

In deze sectie bieden we Stapsgewijze instructies voor het maken van een import- en een taak voor het exporteren. Zorg ervoor dat u aan alle van de [noodzakelijke voorwaarden](#pre-requisites) voldoen voordat u verdergaat.

## <a name="how-to-create-an-import-job"></a>Het maken van een taak importeren?

Maak een taak importeren om gegevens te kopiëren naar uw rekening Azure opslag van harde schijven door verzending van een of meer stations met gegevens die aan de opgegeven Datacenter. De taak importeren geeft informatie over vaste schijven, gegevens worden gekopieerd, opslag account en verzendgegevens naar de service Azure importeren/exporteren. Maken van een taak importeren is een proces van drie stappen. De stations met behulp van het hulpprogramma voor importeren/exporteren van Azure client eerst voorbereiden. Ten tweede, een importtaak met de klassieke portal indienen. Derde de schijven aan het verzendadres is opgegeven tijdens het maken van taken verzenden en bijwerken van de gegevens van de verzending in de details van uw project.   

> [AZURE.IMPORTANT] U kunt slechts één taak per account opslag indienen. Elk station dat u verzendt kan worden geïmporteerd in één opslag-account. Stel dat u wilt importeren van gegevens in twee accounts voor opslag. Als u afzonderlijke vaste schijven te gebruiken voor elke rekening voor opslag en afzonderlijke projecten per account opslag maken.

### <a name="prepare-your-drives"></a>De vaste schijven voorbereiden

De eerste stap bij het importeren van gegevens met behulp van de service Azure importeren/exporteren is het voorbereiden van uw stations met behulp van het hulpprogramma voor importeren/exporteren van Azure-client. De volgende stappen voor het voorbereiden van uw schijven.

1.  Bepaal welke gegevens moeten worden geïmporteerd. Dit kan de mappen en bestanden op de lokale server of een netwerkshare zelfstandig zijn.  

2.  Bepaal het aantal schijven u afhankelijk van de totale grootte van de gegevens moet. Het vereiste aantal harde schijven voor 3,5-inch SATA III-II aanschaffen.

3.  De doelaccount opslag container, virtuele mappen en BLOB's identificeren.

4.  De mappen en/of zelfstandige bestanden die worden gekopieerd naar elke vaste schijf bepalen.

5.  Gebruik het [Hulpprogramma voor importeren/exporteren van Azure](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) uw gegevens kopiëren naar een of meer vaste schijven.

    - Sessies kopiëren als u wilt uw gegevens uit bron kopiëren naar de vaste schijven worden gemaakt door het hulpprogramma Azure importeren/exporteren. Tijdens een sessie van één exemplaar kunt het hulpprogramma kopiëren van één map met bijbehorende submappen of in één bestand.

    - Mogelijk moet u meerdere kopie sessies als de brongegevens uit meerdere mappen.

    - Elke vaste schijf voorbereiden moet ten minste één kopieersessie.

6.  Kunt u het / de parameter voor het inschakelen van BitLocker-versleuteling op de vaste schijf te coderen. Ook kan u ook inschakelen van BitLocker-versleuteling handmatig op de vaste schijf en de sleutel opgeven tijdens het uitvoeren van het hulpprogramma.

7.  Het hulpprogramma voor importeren/exporteren van Azure genereert een station journal-bestand voor elk station als deze wordt voorbereid. Het station journal-bestand is opgeslagen op uw lokale computer, niet op het station zelf. Wanneer u de taak importeren maakt, wordt u het journal-bestand uploaden. Een station journal-bestand bevat de ID van het station en de sleutel van BitLocker, evenals andere informatie over het station.
**Belangrijk**: elke vaste schijf voorbereiden resulteert in een journal-bestand. Wanneer u de importtaak met de klassieke portal maakt, moet u alle bestanden van de schijven die deel van die taak importeren uitmaken journaal uploaden. Stations zonder journal-bestanden worden niet verwerkt.

8.  De gegevens op de vaste schijf of het journal-bestand niet wijzigen nadat voorbereid als de schijf is voltooid.

Hieronder ziet u de opdrachten en voorbeelden voor het voorbereiden van de vaste schijf die het hulpprogramma voor importeren/exporteren van Azure client.

Azure Import/Export client PrepImport de opdracht voor de eerste sessie kopiëren naar een map kopiëren:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Voorbeeld:**

In het volgende voorbeeld wij kopieert alle bestanden en mappen uit de H:\Video op de vaste schijf gemonteerd op X: sub. De gegevens worden geïmporteerd naar de bestemming opslag rekening door de sleutel opslag account en in de opslag container video genoemd. Als de container voor de opslag niet aanwezig is, zal worden gemaakt. Met deze opdracht wordt ook opmaken en de doel-harde schijf coderen.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure Import/Export client PrepImport de opdracht voor sessies latere kopiëren naar een map kopiëren:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Voor het kopiëren voor latere sessies op dezelfde vaste schijf, dezelfde bestandsnaam van het logboek opgeven en bieden een nieuwe sessie-ID; Er is niet nodig opnieuw, zodat de account sleutel- en het doeldomein opslagschijf noch voor het opmaken of het station versleutelt. In dit voorbeeld kopiëren we de map H:\Photo en de sub-mappen naar het doelstation dezelfde in de opslag container foto genoemd.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Het hulpprogramma Azure Import/Export client PrepImport-opdracht voor de eerste sessie kopiëren om een bestand te kopiëren:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Het hulpprogramma Azure Import/Export client PrepImport-opdracht voor het kopiëren voor latere sessies om een bestand te kopiëren:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Onthouden**: standaard, de gegevens worden geïmporteerd als BLOB's blokkeren. Voor het importeren van gegevens als een BLOB pagina's kunt u de parameter /BlobType. Bijvoorbeeld, als u wordt gekoppeld als schijven op een Azure VM VHD-bestanden importeert, moet u importeren ze als BLOB's pagina. Als u niet zeker welke blob Typ weet te gebruiken, kunt u /blobType:auto en wij helpen bepalen van het juiste type. In dit geval alle vhd en vhdx-bestanden worden geïmporteerd als BLOB's pagina en de rest wordt geïmporteerd als BLOB's blokkeren.

Zie meer informatie over het hulpprogramma voor het importeren/exporteren van Azure client in [Harde schijven voorbereiden voor importeren](https://msdn.microsoft.com/library/dn529089.aspx).

Raadpleeg ook de [Voorbeeldwerkstroom voor harde schijven voorbereiden voor een taak importeren](https://msdn.microsoft.com/library/dn529097.aspx) voor uitvoerige stapsgewijze instructies.  

### <a name="create-the-import-job"></a>Maak de taak importeren

1.  Als u het station hebt voorbereid, Ga naar uw account voor opslag in de [klassieke portal](https://manage.windowsazure.com) en het Dashboard weergeven. Klik onder **Snel bekijken**, **maken een taak importeren**. Bekijk de stappen en schakelt u het selectievakje in om aan te geven dat u het station hebt voorbereid en u hebt het station journal-bestand beschikbaar.

2.  Bieden de contactgegevens van de persoon die verantwoordelijk is voor deze taak importeren en het adres van een geldige afzender in stap 1. Als u opslaan van uitgebreide logboekgegevens voor de taak importeren wilt, Controleer de optie voor het **opslaan van het uitgebreide logboek in mijn 'waimportexport' blob-container**.

3.  Upload het station journal-bestanden die u hebt gekregen tijdens de bereiding station stap in stap 2. Moet u voor het uploaden van een bestand voor elke schijf die u hebt voorbereid.

    ![Maken van de functie import - stap 3](./media/storage-import-export-service/import-job-03.png)

4.  Voer een beschrijvende naam voor de taak importeren in stap 3. De naam die u invoert mag bevatten alleen kleine letters, cijfers, koppeltekens en onderstrepingstekens, moet beginnen met een letter en mag geen spaties bevatten. U gebruikt de naam die u bijhouden van uw taken wilt, terwijl ze worden uitgevoerd en nadat ze zijn voltooid.

    Selecteer uw regio data center vervolgens uit de lijst. Het gegevensgebied center geeft aan de Datacenter en het adres waarnaar het pakket moet worden verzonden. Zie de veelgestelde vragen hieronder voor meer informatie.

5.  In stap 4 het telecombedrijf terug in de lijst selecteren en voer uw vervoerdersrekeningnummer. Microsoft gebruikt deze account toe aan de schijven naar u verzenden nadat het importeren voltooid is.

    Als u het volgnummer, de vervoerder selecteren in de lijst en voert u het volgnummer.

    Als u een trackingnummer hebben nog niet, kies **ik bied mijn verzendgegevens voor deze taak importeren nadat ik mijn pakket hebt verzonden**, vervolgens het importproces te voltooien.

6. Als u wilt uw volgnummer invoeren nadat u het pakket, Ga terug naar de pagina **Importeren/exporteren** voor uw account voor opslag in de portal klassieke hebt verzonden selecteert u de afdruktaak in de lijst en kies **Info verzending**. Navigeren door de wizard en geef het volgnummer in stap 2.

    Als u het traceringsnummer niet binnen 2 weken na het maken van de taak wordt bijgewerkt, verloopt de taak.

    Als de taak in de staat maken, verzending of overdragen, kunt u uw vervoerdersrekeningnummer in stap 2 van de wizard ook bijwerken. Zodra de taak in de verpakking staat is, kunt u het rekeningnummer van de vervoerder voor die taak niet bijwerken.

7. Op het dashboard portal kunt u de voortgang van het project bijhouden. Zie elke taak staat in de vorige sectie: door middel van [de status van uw project bekijken](#viewing-your-job-status).

## <a name="how-to-create-an-export-job"></a>Het maken van een taak exporteren?

Maak een taak exporteren zodat de Import/Export-service dat u een of meer lege stations tot het datacenter verzenden gaat, zodat gegevens kunnen worden geëxporteerd uit uw account opslag naar de stations en de stations en vervolgens naar u verzonden.

### <a name="prepare-your-drives"></a>De vaste schijven voorbereiden

Volgende voorafgaande controles worden aanbevolen voor het voorbereiden van uw stations voor een taak exporteren:

1. Controleer het nummer van de schijven met behulp van het hulpprogramma Azure Import/Export PreviewExport opdracht vereist. Zie [Station gebruik voorvertoning voor een taak exporteren](https://msdn.microsoft.com/library/azure/dn722414.aspx)voor meer informatie. U kunt voorvertoning station gebruik voor de BLOB's die u hebt geselecteerd, is gebaseerd op de grootte van de stations die u wilt gebruiken.

2. Controleer of u kunt lezen/schrijven naar de vaste schijf die worden geleverd voor het project exporteren.

### <a name="create-the-export-job"></a>De Export-taak maken

1.  Maak een taak exporteren, navigeer naar de account van uw opslag in de [klassieke portal](https://manage.windowsazure.com)en het Dashboard weergeven. Onder **Snelle bekijken**, klikt u op **maken van een taak exporteren** en volg de stappen in de wizard.

2.  Bieden de contactgegevens van de persoon die verantwoordelijk is voor deze taak exporteren in stap 2. Als u opslaan van uitgebreide logboekgegevens voor de taak voor het exporteren wilt, controleert u de optie voor het **opslaan van het uitgebreide logboek in mijn 'waimportexport' blob-container**.

3.  Geef in stap 3, welke blob-gegevens u wilt exporteren vanuit uw account opslag naar de lege schijf of schijven. U kunt kiezen voor het exporteren van alle blob-gegevens in de opslag of kunt u opgeven welke BLOB's of ingesteld van BLOB's te exporteren.

    Als u een blob te exporteren, de kiezer **Gelijk aan** , en geeft het relatieve pad naar de blob, beginnend met de containernaam van de. Met *$root* geeft u de basiscontainer.

    Als u alle BLOB's beginnen met een voorvoegsel, de kiezer **Begint met** gebruiken en geeft u het voorvoegsel, beginnen met een slash '/'. Het voorvoegsel is mogelijk het voorvoegsel van de containernaam van de, de volledige containernaam of de volledige containernaam gevolgd door het voorvoegsel van de naam van de blob.

    De volgende tabel ziet u voorbeelden van geldige blob paden:

    Selector|BLOB-pad|Beschrijving
    ---|---|---
    Begint met|/|Exporteert alle BLOB's in de account van de opslag
    Begint met|/$root /|Exporteert alle BLOB's in de basiscontainer
    Begint met|/Book|Exporteert alle BLOB's in een willekeurige container die met het voorvoegsel **boek begint**
    Begint met|/Music/|Exporteert alle BLOB's in de container, **muziek**
    Begint met|muziek/liefde|Exporteert alle BLOB's in de container **muziek** die met het voorvoegsel van **liefde beginnen**
    Gelijk aan|$root/logo.bmp|Blob- **logo.bmp** in de basiscontainer voor uitvoer
    Gelijk aan|videos/Story.mp4|Uitvoer blob- **story.mp4** in de container, **video 's**

    U dient de blob paden in geldige notaties om fouten te voorkomen tijdens de verwerking, zoals in dit screenshot.

    ![Export-functie - stap 3 maken](./media/storage-import-export-service/export-job-03.png)


4.  Voer een beschrijvende naam voor de taak voor het exporteren in stap 4. De naam die u invoert mag alleen kleine letters, cijfers, koppeltekens en onderstrepingstekens, moet beginnen met een letter en mag geen spaties bevatten.

    Het gegevensgebied center geeft het datacenter waarnaar het pakket moet worden verzonden. Zie de veelgestelde vragen hieronder voor meer informatie.

5.  In stap 5 het telecombedrijf terug in de lijst selecteren en voer uw vervoerdersrekeningnummer. Microsoft gebruikt deze account aan uw schijven naar u verzenden zodra de taak voor het exporteren voltooid is.

    Als u het volgnummer, de vervoerder selecteren in de lijst en voert u het volgnummer.

    Als u een trackingnummer hebben nog niet, kies **ik bied mijn verzendgegevens voor dit project exporteren nadat ik mijn pakket hebt verzonden**, vervolgens het exportproces voltooid.

6. Als u wilt uw volgnummer invoeren nadat u het pakket, Ga terug naar de pagina **Importeren/exporteren** voor uw account voor opslag in de portal klassieke hebt verzonden selecteert u de afdruktaak in de lijst en kies **Info verzending**. Navigeren door de wizard en geef het volgnummer in stap 2.

    Als u het traceringsnummer niet binnen 2 weken na het maken van de taak wordt bijgewerkt, verloopt de taak.

    Als de taak in de staat maken, verzending of overdragen, kunt u uw vervoerdersrekeningnummer in stap 2 van de wizard ook bijwerken. Zodra de taak in de verpakking staat is, kunt u het rekeningnummer van de vervoerder voor die taak niet bijwerken.

    > [AZURE.NOTE] De blob te exporteren wordt gebruikt bij het kopiëren naar de vaste schijf, wordt Azure Import/Export service een momentopname van de blob als de momentopname kopiëren.

7.  Op het dashboard in de klassieke portal kunt u de voortgang van het project bijhouden. Zie wat betekent de taakstatus van elke in de vorige sectie "uw taakstatus weergeven' in.

8.  Nadat u de schijven met de geëxporteerde gegevens ontvangt, kunt u bekijken en kopiëren van de BitLocker-sleutels die zijn gegenereerd door de service voor het station. Ga naar uw account voor opslag in de klassieke portal en klik op het tabblad importeren/exporteren. Selecteer de taak voor het exporteren in de lijst en klik op de knop weergave toetsen. De BitLocker-sleutels worden als volgt weergegeven:

    ![BitLocker-sleutels voor project exporteren weergeven](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Lees de sectie Veelgestelde vragen hieronder als u de meest voorkomende vragen klanten optreden bij het gebruik van deze service dekt.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen ##


**Hoe lang duurt het voordat mijn gegevens kopiëren nadat de stations heeft bereikt van het datacenter?**

De tijd voor het kopiëren is afhankelijk van verschillende factoren, zoals het taaktype, aard en omvang van de gegevens worden gekopieerd, grootte van de schijven worden geleverd, en de bestaande werklast. Dit kan variëren van een paar dagen tot enkele weken, afhankelijk van deze factoren. Daarom is het moeilijk om een algemene raming. De service probeert uw werk te optimaliseren door het kopiëren van meerdere stations tegelijk mogelijk. Als u een taak tijd kritiek importeren/exporteren, bereiken ons voor een schatting.

**Wanneer gebruikt u Azure Import/Export Service?**
Een moet rekening houden met Azure importeren exporteren als uploaden of downloaden via netwerk duurt ongeveer als schatting meer dan 7 dagen. U kunt berekenen hoe lang het duurt met behulp van een rekenmachine online of door het [downloaden](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) van zich in onze Azure Import Export REST API monster in Azure monsters opslagplaats op [Data Transfer snelheid Rekenmachine](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html). Dit is een exacte berekening, maar slechts een ruwe indicatie.

**Kan ik de service Azure importeren/exporteren gebruiken met een Resource Manager opslag-account?**

Nee, niet kunt u gegevens kopiëren naar of van een Resource Manager opslag account rechtstreeks via de service Azure importeren/exporteren. De service ondersteunt alleen klassieke opslag rekeningen. Ondersteuning voor opslag account Resource Manager worden binnenkort verwacht. Als alternatief kunt u gegevens importeren in een klassieke opslag account en migreren naar de Resource Manager opslag account via [AzCopy](storage-use-azcopy.md) of CopyBlob.

**Kan ik kopiëren Azure bestanden via de service Azure importeren/exporteren?**

Nee, ondersteunt de Azure Import/Export-service alleen BLOB's blokkeren en BLOB's pagina. Alle andere soorten opslag waaronder Azure-bestanden, tabellen en wachtrijen worden niet ondersteund.

**Is de service Azure importeren/exporteren voor CSP abonnementen beschikbaar?**

Nee, de Azure Import/Export-service ondersteunt geen CSP abonnementen. De ondersteuning wordt in de toekomst worden toegevoegd.

**Kan ik het overslaan station voorbereiding voor een taak importeren of kan ik een station zonder kopiëren voorbereiden?**

Elk station dat u verzenden wilt voor het importeren van gegevens moet worden bereid met behulp van het hulpprogramma voor importeren/exporteren van Azure-client. U moet de client gebruiken om gegevens te kopiëren naar het station.

**Moet ik een preparaat schijf uitvoeren bij het maken van een taak exporteren?**

Geen, maar sommige voorafgaande controles worden aanbevolen. Controleer het nummer van de schijven met behulp van het hulpprogramma Azure Import/Export PreviewExport opdracht vereist. Zie [Station gebruik voorvertoning voor een taak exporteren](https://msdn.microsoft.com/library/azure/dn722414.aspx)voor meer informatie. U kunt voorvertoning station gebruik voor de BLOB's die u hebt geselecteerd, is gebaseerd op de grootte van de stations die u wilt gebruiken. Controleer ook het lezen van en schrijven naar de vaste schijf die worden geleverd voor het project exporteren.

**Wat gebeurt er als ik per ongeluk een harde schijf die niet voldoet aan de eisen van de ondersteunde?**

Azure Datacenter retourneert het station dat niet in overeenstemming met de ondersteunde eisen aan u. Als er slechts enkele van de stations in het pakket voldoet aan de ondersteuningsvereisten, deze stations worden verwerkt en de stations die niet voldoen aan de eisen aan u worden geretourneerd.

**Kan ik mijn taak annuleren?**

U kunt een taak annuleren als de status maakt of verzending.

**Hoe lang kan ik de status van de voltooide taken weergeven in het klassieke portal?**

U kunt de status van de voltooide taken weergeven voor maximaal 90 dagen. Voltooide taken worden verwijderd na 90 dagen.

**Wat moet ik doen als ik wil importeren of exporteren van meer dan 10 schijven?**

Een import of exporttaak kan verwijzen naar slechts 10 stations in een enkel project voor de service voor importeren/exporteren. Als u meer dan 10 schijven verzenden wilt, kunt u meerdere taken. Stations die gekoppeld aan dezelfde taak zijn moeten samen in één pakket worden verzonden.

**Kan ik een USB-SATA-adapter die niet in de lijst met aanbevolen gebruiken?**

Als er een converter die hierboven niet wordt vermeld, kunt u proberen het uitvoeren Azure importeren/exporteren met behulp van het conversieprogramma voor het voorbereiden van het station en zien als het werkt voordat u een ondersteunde conversieprogramma aanschaft.

**De service is formatteren van de schijven voordat ze te retourneren?**

Nr. Alle stations zijn versleuteld met BitLocker.

**Kan ik schijven voor importeren/exporteren van Microsoft kopen?**

Nr. U moet verzenden stations voor beide importeren en exporteren van taken.

**Nadat de taak importeren is voltooid, hoe zal mijn gegevens eruit in de opslag-account? Wordt mijn mapstructuur behouden?**

Als u een vaste schijf voorbereidt voor een taak importeren, het doel wordt opgegeven door de /dstdir: parameter. Dit is de doelcontainer in de opslag account waarmee gegevens van de vaste schijf worden gekopieerd. In deze bestemmingscontainer virtuele mappen worden gemaakt voor de mappen van de vaste schijf en BLOB's voor bestanden worden gemaakt.

**Als het station bestanden die al in Mijn account voor de opslag heeft, de service overschrijft bestaande BLOB's in Mijn account opslag?**

Bij de opstelling van het station, kunt u opgeven of de gekopieerde bestanden moeten worden overschreven of negeren met behulp van de parameter /Disposition genoemd: < naam | Nee overschrijven | overschrijven >. Standaard de service wordt de naam van de nieuwe bestanden in plaats van bestaande BLOB's overschrijven.

**Het hulpprogramma voor importeren/exporteren van Azure client compatibel is met 32-bits besturingssystemen?**
Nr. Het clienthulpprogramma is alleen compatibel met 64-bits Windows-besturingssystemen. Raadpleeg de sectie Operating Systems in de [noodzakelijke voorwaarden](#pre-requisites) voor een volledige lijst van ondersteunde OS versies.

**Moet ik iets anders dan de vaste schijf in mijn pakket opnemen?**

Verzend alleen de vaste schijven. Neem geen items zoals levering voedingskabels of USB-kabels.

**Heb ik mijn schijven met behulp van FedEx of DHL verzenden?**

U kunt schijven op het gegevenscentrum met alle bekende vervoerder zoals FedEx, DHL, UPS of US Postal Service verzenden. Voor het verzenden van de stations aan u in het datacenter, moet u evenwel een FedEx-account in de VS en de EU, of een DHL rekeningnummer in de regio's Azië en Australië.

**Zijn er beperkingen met mijn station internationaal verzenden?**

Houd er rekening mee dat het fysieke medium dat u levert wellicht meerdere internationale grenzen heen. U bent verantwoordelijk om ervoor te zorgen dat het fysieke medium en de gegevens worden geïmporteerd en/of geëxporteerd volgens de van toepassing zijnde wetten. Neem contact op met uw adviseurs om te verifiëren dat uw media en legaal kan worden verzonden naar het midden van de geïdentificeerde gegevens vóór verzending van het fysieke medium. Dit helpt ervoor te zorgen dat Microsoft heeft bereikt in een tijdige wijze.

**Wanneer u een taak maakt, is het verzendadres in een locatie die afwijkt van de locatie van mijn account. Wat moet ik doen?**

Sommige opslaglocaties voor de account zijn toegewezen aan verzendkosten voor alternatieve locaties. Eerder kunnen beschikbaar verzendlocaties ook tijdelijk toewijzen aan alternatieve locaties. Controleer altijd het verzendadres opgegeven tijdens het maken van een taak voordat de schijven levert.

**Waarom wordt mijn taakstatus in de klassieke portal Zeg "verzending:" wanneer de vervoerder website Mijn pakket toont wordt bezorgd?**

De status in de klassieke portal-wijzigingen van de scheepvaart te brengen wanneer het station verwerking wordt gestart. Als de schijf de faciliteit heeft bereikt, maar is niet begonnen met het verwerken, wordt de status van uw project weergegeven als verzending.

**Tijdens het verzenden van het station, wordt u gevraagd de vervoerder voor de naam van contactpersoon gegevens en telefoonnummer. Wat moet ik bieden?**

Het nummer wordt tijdens het maken van taken aan u geleverd. Als u de naam van een contactpersoon, neem contact met ons op waimportexport@microsoft.com en wij zullen u voorzien van die informatie.

**Kan ik de service Azure Import/Export PST postbussen en SharePoint-gegevens kopiëren naar O365 gebruiken?**

Zie [importeren PST-bestanden of gegevens van SharePoint en Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Kan ik de Azure Import/Export service off line Mijn back-ups kopiëren naar de back-up Azure Service gebruiken?**

Zie [off line back-up-workflow in Azure back-up](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Zie ook:

- [Het hulpprogramma voor importeren/exporteren van Azure-client instellen](https://msdn.microsoft.com/library/dn529112.aspx)

- [Gegevens overbrengen met het opdrachtregelprogramma AzCopy](storage-use-azcopy.md)

- [Azure Import Export REST API monster](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
