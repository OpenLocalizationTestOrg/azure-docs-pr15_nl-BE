<properties
    pageTitle="Gegevens kopiëren of verplaatsen naar opslag met AzCopy | Microsoft Azure"
    description="Gebruik het hulpprogramma AzCopy te verplaatsen of kopiëren van gegevens naar of van de blob, tabel en de bestandsinhoud van het. Gegevens kopiëren naar Azure opslag van lokale bestanden of Kopieer gegevens binnen of tussen rekeningen voor opslag. Eenvoudig uw gegevens migreren naar Azure opslag."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel

## <a name="overview"></a>Overzicht

AzCopy is een opdrachtregelprogramma van Windows ontworpen voor het kopiëren van gegevens van en naar Microsoft Azure Blob-bestand en tabel opslag met behulp van eenvoudige opdrachten met optimale prestaties. U kunt gegevens kopiëren van het ene object naar het andere binnen uw account opslag of tussen rekeningen voor opslag.

> [AZURE.NOTE] Deze handleiding wordt ervan uitgegaan dat u al bekend met [Azure opslag bent](https://azure.microsoft.com/services/storage/). Als dat niet het geval is, lees de documentatie van de [Inleiding tot Azure opslag](storage-introduction.md) zal nuttig zijn. Belangrijker is, moet u [een account opslag](storage-create-storage-account.md#create-a-storage-account) maken om te kunnen starten met behulp van AzCopy.

## <a name="download-and-install-azcopy"></a>Downloaden en installeren van AzCopy

### <a name="windows"></a>Windows

Download de [nieuwste versie van AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac/Linux

AzCopy is niet beschikbaar voor Mac/Linux OSs. CLI Azure is echter een geschikt alternatief voor het kopiëren van gegevens naar en van Azure opslag. Gelezen [met behulp van de CLI Azure met Azure opslag](storage-azure-cli.md) voor meer informatie.

## <a name="writing-your-first-azcopy-command"></a>Schrijven van uw eerste AzCopy opdracht

De basissyntaxis voor opdrachten van AzCopy is:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Open een opdrachtvenster en navigeer naar de map AzCopy installeren op uw computer - waar de `AzCopy.exe` uitvoerbare bestand zich bevindt. Indien gewenst, kunt u de locatie van de AzCopy-installatie toevoegen aan het systeempad. AzCopy is standaard geïnstalleerd op `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` of `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

De volgende voorbeelden worden verschillende scenario's voor het kopiëren van gegevens naar en van Microsoft Azure BLOB's, bestanden en tabellen. Raadpleeg de sectie [Parameters voor AzCopy](#azcopy-parameters) voor een gedetailleerde uitleg van de parameters die worden gebruikt in elk monster.

## <a name="blob-download"></a>BLOB: downloaden

### <a name="download-single-blob"></a>Enkele blob downloaden

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Opmerking Als de map `C:\myfolder` niet bestaat, AzCopy wordt het maken en downloaden `abc.txt ` naar de nieuwe map.

### <a name="download-single-blob-from-secondary-region"></a>Enkele blob van secundaire regio downloaden

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Houd er rekening mee dat de geo-redundante opslag lezen toegang ingeschakeld moet zijn.

### <a name="download-all-blobs"></a>Downloaden van alle BLOB 's

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Stel dat de volgende BLOB's bevinden zich in de opgegeven container:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Na het downloaden, de map `C:\myfolder` bevat de volgende bestanden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Als u geen optie opgeeft `/S`, geen BLOB's worden gedownload.

### <a name="download-blobs-with-specified-prefix"></a>BLOB's met het opgegeven voorvoegsel downloaden

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Stel dat de volgende BLOB's bevinden zich in de opgegeven container. Alle BLOB's beginnen met het voorvoegsel `a` worden gedownload:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Na het downloaden, de map `C:\myfolder` bevat de volgende bestanden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Het voorvoegsel is van toepassing op de virtuele map die het eerste deel van de naam van de blob. In het bovenstaande voorbeeld, de virtuele map komt niet overeen met het opgegeven voorvoegsel, zodat het niet is gedownload. Bovendien, als de optie `\S` niet is opgegeven, AzCopy alle BLOB's niet gedownload.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>De tijd van de laatste wijziging van geëxporteerde bestanden moet hetzelfde zijn als de bron BLOB's instellen

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

U kunt ook BLOB's uitsluiten van de downloadbewerking op basis van hun tijd van laatste wijziging. Als u wilt uitsluiten van BLOB's waarvan het laatste tijd gewijzigd is bijvoorbeeld hetzelfde of nieuwer is dan het doelbestand toevoegen de `/XN` optie:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Of als u wilt uitsluiten van BLOB's waarvan het laatste tijd gewijzigd is dezelfde of ouder dan het doelbestand toevoegen de `/XO` optie:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>BLOB: uploaden

### <a name="upload-single-file"></a>Bestand uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Als de container voor de opgegeven bestemming niet bestaat, wordt de AzCopy maken en upload het bestand naar het.

### <a name="upload-single-file-to-virtual-directory"></a>Bestand uploaden naar de virtuele map

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Als de opgegeven virtuele map niet bestaat, AzCopy uploaden van het bestand in de naam van de virtuele map (*bijvoorbeeld*, `vd/abc.txt` in het bovenstaande voorbeeld).

### <a name="upload-all-files"></a>Alle bestanden uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Optie `/S` de inhoud van de opgegeven map uploads naar Blob storage recursief, wat betekent dat alle submappen en de bestanden ook worden geüpload. Bijvoorbeeld, wordt ervan uitgegaan dat de volgende bestanden bevinden zich in de map `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Na het uploaden van bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Als u geen optie opgeeft `/S`, AzCopy wordt niet recursief uploaden. Na het uploaden van bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Uploaden van bestanden die overeenkomen met opgegeven patroon

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Stel dat de volgende bestanden bevinden zich in de map `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Na het uploaden van bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Als u geen optie opgeeft `/S`, AzCopy wordt alleen uploaden BLOB's die zich niet in een virtuele map bevinden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Het MIME-inhoudstype van een blob bestemming opgeven

Standaard AzCopy stelt het type inhoud van een blob bestemming naar `application/octet-stream`. Vanaf versie 3.1.0, kunt u expliciet opgeven het type inhoud via de optie `/SetContentType:[content-type]`. Deze syntaxis wordt het type inhoud voor alle BLOB's in een uploadbewerking.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Als u `/SetContentType` zonder een waarde, AzCopy zal stelt elke blob of inhoudstype volgens het de bestandsextensie van het bestand.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>BLOB: kopiëren

### <a name="copy-single-blob-within-storage-account"></a>Kopieer één blob in opslag account

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Bij het kopiëren van een blob binnen een opslag-account wordt een [kopie van server-side](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) -bewerking wordt uitgevoerd.

### <a name="copy-single-blob-across-storage-accounts"></a>Enkele blob Storage accounts kopiëren

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Bij het kopiëren van een blob Storage accounts wordt een [kopie van de server-side](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) -bewerking wordt uitgevoerd.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Één blob van secundaire regio naar regio primaire kopiëren

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Houd er rekening mee dat de geo-redundante opslag lezen toegang ingeschakeld moet zijn.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopieer één blob en de momentopnamen in opslag rekeningen

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Na het kopiëren van bestanden omvatten de doelcontainer de blob en de momentopnamen. Ervan uitgaande dat de blob in het bovenstaande voorbeeld heeft twee momentopnamen, bevat de container de volgende blob en momentopnamen:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchroon kopiëren BLOB's in opslag rekeningen

AzCopy standaard gekopieerd asynchroon gegevens tussen twee eindpunten van de opslag. Daarom worden de kopieerbewerking wordt uitgevoerd in de achtergrond met ongebruikte bandbreedte capaciteit geen SLA in termen van hoe snel heeft een blob gekopieerd en AzCopy controleert de status van de kopie regelmatig totdat het kopiëren is voltooid of mislukt.

De `/SyncCopy` optie zorgt ervoor dat de kopieerbewerking wordt consistente snelheid. AzCopy voert de synchrone kopie door de BLOB's van de opgegeven bron te kopiëren naar het lokale geheugen downloaden en vervolgens uploaden naar de bestemming Blob-opslag.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`egress extra kosten in vergelijking met asynchrone kopie kan genereren, de aanbevolen werkwijze is het gebruik van deze optie in een Azure VM in hetzelfde gebied, als de bronaccount opslag om te voorkomen dat de kosten van de egress.

## <a name="file-download"></a>Bestand: Download

### <a name="download-single-file"></a>Bestand downloaden

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Als de opgegeven bron een bestandsshare Azure, dan moet u de exacte naam opgeven (*bijvoorbeeld* `abc.txt`) een bestand downloaden of de optie opgeven `/S` voor het downloaden van alle bestanden in de share recursief. U probeert een bestandspatroon en de optie te geven `/S` samen resulteren in een fout.

### <a name="download-all-files"></a>Alle bestanden downloaden

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Houd er rekening mee dat alle lege mappen worden niet gedownload.

## <a name="file-upload"></a>Bestand: uploaden

### <a name="upload-single-file"></a>Bestand uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Alle bestanden uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Houd er rekening mee dat alle lege mappen niet worden geüpload.

### <a name="upload-files-matching-specified-pattern"></a>Uploaden van bestanden die overeenkomen met opgegeven patroon

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Bestand: kopiëren

### <a name="copy-across-file-shares"></a>Alle gedeelde bestanden kopiëren

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Van de bestandsshare naar een blob-kopiëren

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Houd er rekening mee dat asynchrone kopiëren van opslag van bestanden naar de pagina Blob wordt niet ondersteund.

### <a name="copy-from-blob-to-file-share"></a>Van blob kopiëren naar de bestandsshare

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Synchroon kopiëren van bestanden

Kunt u de `/SyncCopy` de optie om gegevens te kopiëren bestandsopslag opslag, opslag van bestanden naar een Blob-opslag en uit de Blob-opslag voor opslag van bestanden synchroon, AzCopy doet dit door het downloaden van de brongegevens naar het lokale geheugen en upload deze opnieuw naar bestemming.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Bij het kopiëren van de opslag van bestanden naar een Blob-opslag, het standaardtype blob is een blob blok, optie kunt u opgeven `/BlobType:page` type blob bestemming wijzigen.

Houd er rekening mee dat `/SyncCopy` extra egress kosten vergelijken met asynchrone kopie kan genereren, de aanbevolen werkwijze is het gebruik van deze optie in Azure VM in hetzelfde gebied, als de bronaccount opslag om te voorkomen dat de egress-kosten.

## <a name="table-export"></a>Tabel: exporteren

### <a name="export-table"></a>Tabel exporteren

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy schrijft een manifest-bestand naar de opgegeven doelmap. Het manifestbestand wordt gebruikt in het importproces te vinden van de nodige gegevensbestanden en gegevensvalidatie. Het manifestbestand maakt standaard gebruik van de volgende naamgevingsconventie:

    <account name>_<table name>_<timestamp>.manifest

Gebruiker kan ook opgeven de optie `/Manifest:<manifest file name>` de naam van manifestbestand instellen.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Export splitsen in meerdere bestanden

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy maakt gebruik van een *volume-index* in de namen van de gesplitste gegevens te onderscheiden van meerdere bestanden. De volume-index bestaat uit twee delen, een *partitie sleutel bereik index* en een *bestandsindex splitsen*. Beide indexen zijn gebaseerd op nul.

De partitie-index key bereik is 0 als de gebruiker geeft geen optie `/PKRS`.

Stel AzCopy twee gegevensbestanden gegenereerd nadat de gebruiker Hiermee kunt u de optie `/SplitSize`. De namen van de resulterende gegevens kunnen zijn:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Merk op dat de minimaal mogelijke waarde voor de optie `/SplitSize` 32 MB. Als de opgegeven bestemming is Blob-opslag, AzCopy wordt het bestand gesplitst als de grootte van de blob grootte beperking (200GB) bereikt, ongeacht of de optie `/SplitSize` door de gebruiker is opgegeven.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Tabel exporteren naar de bestandsindeling JSON- of CSV-gegevens

Tabellen exporteert AzCopy standaard naar JSON-gegevensbestanden. U kunt opgeven de optie `/PayloadFormat:JSON|CSV` de tabellen exporteren als JSON of CSV.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Bij het opgeven van de nettolading CSV-indeling, AzCopy genereren ook een schemabestand met de extensie `.schema.csv` voor elk gegevensbestand.

### <a name="export-table-entities-concurrently"></a>Gelijktijdig tabel entiteiten exporteren

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

Gelijktijdige bewerkingen entiteiten wanneer de gebruiker de optie exporteren wordt gestart door AzCopy `/PKRS`. Elke bewerking wordt één partitie sleutel bereik geëxporteerd.

Houd er rekening mee dat het aantal gelijktijdige bewerkingen wordt eveneens bepaald door de optie `/NC`. AzCopy het aantal core-processors gebruikt als de standaardwaarde van `/NC` bij het kopiëren van tabel entiteiten, zelfs als `/NC` is niet opgegeven. Wanneer de gebruiker de optie geeft `/PKRS`, AzCopy maakt gebruik van de kleinste van de twee waarden - partitie sleutel bereiken versus impliciet of expliciet opgegeven gelijktijdige bewerkingen - tot bepalen het aantal gelijktijdige bewerkingen te starten. Typ voor meer informatie `AzCopy /?:NC` op de opdrachtregel.

### <a name="export-table-to-blob"></a>Tabel exporteren naar een blob-

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy genereert een JSON-gegevensbestand in de blob-container met de volgende naamgevingsconventie:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Het gegenereerde bestand JSON volgt de indeling van de nettolading voor minimale metagegevens. Zie voor meer informatie over deze indeling payload [Payload-indeling voor de tabel Service-activiteiten](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Houd er rekening mee dat wanneer u tabellen exporteren naar BLOB's, AzCopy wordt de tabel entiteiten naar lokale tijdelijke gegevensbestanden downloaden en deze diensten vervolgens naar de blob uploaden. Deze tijdelijke bestanden in de map met het standaardpad voor het dagboek worden gebracht '<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>', kunt u optie/Z: [journaal-map] wijzigen het journaal locatie van het bestand en dus de locatie van tijdelijke bestanden wijzigen. Grootte van de tijdelijke bestanden wordt bepaald door de instanties tabel grootte en de grootte die u hebt opgegeven met de optie /SplitSize, hoewel het tijdelijke bestand in de lokale schijf worden direct verwijderd nadat deze is geüpload naar de blob, Controleer of er voldoende ruimte voor lokale schijf voor het opslaan van deze tijdelijke gegevensbestanden voordat ze worden verwijderd.

## <a name="table-import"></a>Tabel: importeren

### <a name="import-table"></a>Tabel importeren

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

De optie `/EntityOperation` geeft aan hoe u entiteiten in de tabel invoegen. Mogelijke waarden zijn:

- `InsertOrSkip`: Een bestaande entiteit slaat over of een nieuwe entiteit wordt ingevoegd als deze niet in de tabel bestaat nog.
- `InsertOrMerge`: Een bestaande entiteit worden samengevoegd of een nieuwe entiteit wordt ingevoegd als deze niet in de tabel bestaat nog.
- `InsertOrReplace`: Vervangt een bestaande entiteit of een nieuwe entiteit wordt ingevoegd als deze niet in de tabel bestaat nog.

Opmerking dat kunt u de optie opgeven `/PKRS` in het scenario importeren. In tegenstelling tot het scenario waarin moet u optie export `/PKRS` gelijktijdige bewerkingen starten, AzCopy wordt standaard gestart gelijktijdige bewerkingen wanneer u een tabel importeert. Het aantal bewerkingen gelijktijdig gestart is gelijk aan het aantal core processors; echter, kunt u een ander aantal gelijktijdige met optie `/NC`. Typ voor meer informatie `AzCopy /?:NC` op de opdrachtregel.

Houd er rekening mee dat AzCopy biedt alleen ondersteuning voor JSON, niet CSV importeren. AzCopy zonder ondersteuning voor invoer van de gebruiker gemaakt JSON tabel en manifest-bestanden. Beide bestanden moeten afkomstig zijn van een AzCopy-tabel exporteren. Om fouten te voorkomen, moet de geëxporteerde JSON wijzigen of niet-manifestbestand.

### <a name="import-entities-to-table-using-blobs"></a>Entiteiten die aan een tabel met BLOB's importeren

Een Blob-container bevat het volgende: een JSON-bestand dat een tabel Azure en het bijbehorende manifest bestand vertegenwoordigt.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

U kunt de volgende opdracht om diensten importeren in een tabel met behulp van het manifest bestand in die container blob uitvoeren:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Andere functies AzCopy

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Alleen gegevens die niet in de doelmap bestaat kopiëren

De `/XO` en `/XN` parameters kunt u uitsluiten dat oudere of nieuwere bron bronnen worden gekopieerd, respectievelijk. Als u alleen bron bronnen die niet in de bestemming te kopiëren, kunt u beide parameters in de opdracht AzCopy:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Opmerking: Dit wordt niet ondersteund als de bron of het doel een tabel is.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Met een antwoordbestand geeft u parameters voor de opdrachtregel

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

U kunt eventuele opdrachtregelparameters AzCopy opnemen in een antwoordbestand. AzCopy verwerkt u de parameters in het bestand alsof ze hadden opgegeven op de opdrachtregel uitvoeren van een directe vervanging met de inhoud van het bestand.

Stel dat u een antwoordbestand met de naam `copyoperation.txt`, die de volgende regels bevat. Elke parameter AzCopy kan op één regel worden opgegeven.

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

of op afzonderlijke regels:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy mislukt als u de parameter over twee regels splitsen, zoals hier wordt weergegeven voor de `/sourcekey` parameter:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Meerdere antwoord-bestanden opgeven van parameters voor de opdrachtregel gebruiken

Stel dat u een antwoordbestand met de naam `source.txt` die aangeeft dat een Broncontainer:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

En een antwoordbestand met de naam `dest.txt` die Hiermee geeft u een doelmap in het bestandssysteem:

    /Dest:C:\myfolder

En een antwoordbestand met de naam `options.txt` die geeft opties voor AzCopy:

    /S /Y

AzCopy aanroepen met de bestanden van deze reactie, die zich bevinden in een map `C:\responsefiles`, gebruik van deze opdracht:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy verwerkt deze opdracht, net zoals wanneer u alle afzonderlijke parameters in de opdracht opgenomen:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Geef een gedeelde access-handtekening (SAS)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

U kunt ook een SAS op de URI-container opgeven:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Map voor logboek

Elke keer dat u een opdracht aan AzCopy geven, wordt gecontroleerd of een journal-bestand bestaat in de standaardmap, of of deze in een map die u hebt opgegeven via deze optie bestaat. De journal-bestand bestaat niet in beide plaatsen, AzCopy de bewerking als nieuw beschouwd als genereert een nieuwe journal-bestand.

Als het journal-bestand bestaat controleert AzCopy of de opdrachtregel die u ingevoerd overeenkomt met de opdrachtregel in een journal-bestand. Als de twee regels overeenkomen, wordt de bewerking niet voltooid in AzCopy hervat. Als deze gegevens niet overeenkomen, wordt u gevraagd of het journaalbestand wilt overschrijven start een nieuwe bewerking of de huidige bewerking te annuleren.

Als u gebruiken de standaardlocatie voor de journal-bestand wilt:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Als u de optie weglaat `/Z`, of de optie opgeven `/Z` zonder het pad van de map zoals hierboven, AzCopy maakt het journal-bestand in de standaardlocatie is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Als het journal-bestand al bestaat, hervat de bewerking op basis van het journal-bestand met AzCopy.

Als u een aangepaste locatie voor het bestand dagboek:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

In dit voorbeeld wordt het journal-bestand gemaakt als deze nog niet bestaat. Als deze bestaat, hervat de bewerking op basis van het journal-bestand met AzCopy.

Als u doorgaan met de bewerking van een AzCopy wilt:

    AzCopy /Z:C:\journalfolder\

In dit voorbeeld wordt de laatste bewerking kan voltooien hervat.

### <a name="generate-a-log-file"></a>Een logboekbestand genereren

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Als u de optie opgeven `/V` zonder dat u een pad naar het uitgebreide logboek AzCopy maakt het logboekbestand op de standaardlocatie is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Anders kunt u een logboekbestand maken in een aangepaste locatie:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Als u een relatief pad optie na `/V`, zoals `/V:test/azcopy1.log`, wordt het uitgebreide logboekbestand in de huidige map in een submap met de naam gemaakt `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Geef het aantal gelijktijdige bewerkingen te starten

Optie `/NC` geeft het aantal gelijktijdige kopieerbewerkingen. AzCopy wordt standaard een aantal gelijktijdige bewerkingen te verhogen, de gegevensdoorvoer overdracht gestart. Voor bewerkingen van de tabel is het aantal gelijktijdige bewerkingen gelijk aan het aantal processors dat u hebt. Voor bewerkingen Blob en de bestandsnaam, het aantal gelijktijdige bewerkingen is gelijk aan 8 maal het aantal processors dat u hebt. Als u AzCopy via een netwerk met lage bandbreedte uitvoert, kunt u een lager getal voor /NC om te voorkomen dat de storing veroorzaakt door de concurrentie van de resource opgeven.

### <a name="run-azcopy-against-azure-storage-emulator"></a>AzCopy uitvoeren voor Azure opslag Emulator

U kunt AzCopy uitvoeren op de [Azure opslag Emulator](storage-use-emulator.md) voor BLOB's:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

en tabellen

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>AzCopy-Parameters

Parameters voor AzCopy worden hieronder beschreven. U kunt ook een van de volgende opdrachten vanaf de opdrachtregel voor hulp bij het gebruik van AzCopy typen:

- Voor gedetailleerde help voor AzCopy:`AzCopy /?`
- Voor gedetailleerde help met de parameter AzCopy:`AzCopy /?:SourceKey`
- Voor de opdrachtregel-voorbeelden:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Bron: "bron"

Hiermee geeft u de brongegevens waaruit u wilt kopiëren. De bron kan een bestandssysteemmap, een blob-container, een virtuele map blob, een bestandsshare voor de opslag, opslag een map of een Azure-tabel zijn.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="destdestination"></a>/ Dest: 'bestemming'

Hiermee geeft u de bestemming te kopiëren. De bestemming kan een bestandssysteemmap, een blob-container, een virtuele map blob, een bestandsshare voor de opslag, opslag een map of een Azure-tabel zijn.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="patternfile-pattern"></a>/ Patroon: "bestand-patroon"

Hiermee geeft u een bestandspatroon dat welke bestanden aangeeft worden gekopieerd. Het gedrag van de parameter /Pattern wordt bepaald door de locatie van de brongegevens en de aanwezigheid van de recursieve modusoptie. Recursieve modus wordt opgegeven via de optie/s.

De opgegeven bron is een map in het bestandssysteem, dan standaard jokertekens zijn als het opgegeven bestandspatroon wordt vergeleken met de bestanden in die map. Als de optie die /s is opgegeven, klikt u vervolgens AzCopy komt ook overeen met het opgegeven patroon ten opzichte van alle bestanden in alle submappen onder de map.

Als de opgegeven bron een blob-container of virtuele map is, worden geen jokertekens toegepast. Als de optie die /s is opgegeven, klikt u vervolgens AzCopy interpreteert het patroon van het opgegeven bestand als een blob-voorvoegsel. Als de optie die /s niet is opgegeven, klikt u vervolgens AzCopy komt overeen met het bestandspatroon tegen blob exacte namen.

Als de opgegeven bron een bestandsshare Azure, dan moet u de exacte bestandsnaam opgeeft, (bv. abc.txt) als een enkel bestand wilt kopiëren, of de optie opgeven /S alle bestanden in de share recursief kopiëren. Probeert op te geven van een bestandspatroon en de optie resulteert /S samen in een fout.

AzCopy maakt gebruik van hoofdlettergevoelige overeenkomen wanneer de /Source een container blob of blob virtuele map is en maakt gebruik van niet-hoofdlettergevoelige vergelijken in alle andere gevallen.

Het standaard bestandspatroon gebruikt als er geen bestandspatroon wordt opgegeven, is *.* voor een locatie in het bestandssysteem of een lege voorvoegsel voor een opslaglocatie Azure. Meerdere patronen van het bestand op te geven wordt niet ondersteund.

**Van toepassing op:** BLOB's, bestanden

### <a name="destkeystorage-key"></a>/ DestKey: "storage key"

Geeft de sleutel opslag account voor de resource voor de bestemming.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="destsassas-token"></a>/ DestSAS: "sas-token"

Hiermee geeft u gedeelde toegang handtekening (SAS) met machtigingen voor lezen en schrijven voor de bestemming (indien van toepassing). De SA's met dubbele aanhalingstekens rond aangezien hierin tekens vanaf de opdrachtregel.

Als de bron bestemming een blob-container, een bestandsshare of een tabel is, kunt u deze optie gevolgd door het token SAS opgeven of als onderdeel van de doelcontainer blob, de bestandsshare of de URI van de tabel, zonder deze optie kunt u de SA's opgeven.

Als de bron- en doelcomputer beide BLOB's zijn, moet de bestemming blob staan binnen hetzelfde account als de bron blob storage.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="sourcekeystorage-key"></a>/ SourceKey: "storage key"

Geeft de sleutel opslag account voor de resource voor de bron.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="sourcesassas-token"></a>/ SourceSAS: "sas-token"

Hiermee geeft u een gedeelde Access-handtekening met lees- en machtigingen voor de bron (indien van toepassing). De SA's met dubbele aanhalingstekens rond aangezien hierin tekens vanaf de opdrachtregel.

Als de bron bron is een blob-container, noch een SAS als een sleutel is opgegeven, wordt de container blob via anonieme toegang worden gelezen.

Als de bron een bestandsshare of een tabel is, moet een sleutel of een SA's worden opgegeven.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="s"></a>/S

Hiermee geeft u een recursieve modus voor het kopiëren van. In de recursieve modus kopieert AzCopy alle BLOB's of bestanden die overeenkomen met het opgegeven bestandspatroon, met inbegrip van submappen.

**Van toepassing op:** BLOB's, bestanden

### <a name="blobtypeblock--page--append"></a>/ BlobType: 'blokkeren' | "pagina" | "append"

Hiermee geeft u aan of de bestemming blob een blob blok, een blob pagina of een blob toevoegen is. Deze optie is alleen toepasbaar wanneer u een blob uploadt. Anders wordt een fout gegenereerd. Als de bestemming een blob is en deze optie niet wordt gespecificeerd, standaard, maakt AzCopy een blob blokkeren.

**Van toepassing op:** BLOB 's

### <a name="checkmd5"></a>/ CheckMD5

Een MD5-hash voor gedownloade gegevens berekent en controleert of de MD5-hash wordt opgeslagen in de blob of Content-MD5-eigenschap van het bestand overeenkomt met de berekende hash. De MD5-controle is standaard uitgeschakeld zodat u deze optie om de MD5-controle uitvoeren bij het downloaden van gegevens moet opgeven.

Houd er rekening mee dat Azure opslag garandeert niet dat de MD5-hash opgeslagen voor de blob of het bestand bijgewerkt is. Het is de verantwoordelijkheid van de client de MD5 bijgewerkt telkens wanneer de blob of het bestand is gewijzigd.

AzCopy stelt de eigenschap Content-MD5 voor een bestand of Azure blob altijd na het uploaden naar de service.  

**Van toepassing op:** BLOB's, bestanden

### <a name="snapshot"></a>/ Snapshot

Geeft aan of het overdragen van momentopnamen. Deze optie is alleen geldig wanneer de bron een blob.

De overgebrachte blob momentopnamen worden gewijzigd in deze notatie: .extensie blob-naam (snapshot-tijd)

Momentopnamen worden standaard niet gekopieerd.

**Van toepassing op:** BLOB 's

### <a name="vverbose-log-file"></a>/ V: [verbose-logboekbestand]

Uitgangen uitgebreide statusberichten in een logboekbestand.

Standaard het uitgebreide logboekbestand met de naam AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`. Als u een bestaande locatie voor deze optie opgeeft, wordt het uitgebreide logboekbestand toegevoegd naar dat bestand.  

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="zjournal-file-folder"></a>/ Z: [journaal-map]

Hiermee geeft u een map journal-bestand voor een bewerking te hervatten.

AzCopy ondersteunt altijd als een bewerking is onderbroken hervatten.

Als deze optie niet is opgegeven, of zonder het pad naar een map is opgegeven, maakt AzCopy het journal-bestand in de standaardlocatie % LocalAppData%\Microsoft\Azure\AzCopy is.

Elke keer dat u een opdracht aan AzCopy geven, wordt gecontroleerd of een journal-bestand bestaat in de standaardmap, of of deze in een map die u hebt opgegeven via deze optie bestaat. De journal-bestand bestaat niet in beide plaatsen, AzCopy de bewerking als nieuw beschouwd als genereert een nieuwe journal-bestand.

Als het journal-bestand bestaat controleert AzCopy of de opdrachtregel die u ingevoerd overeenkomt met de opdrachtregel in een journal-bestand. Als de twee regels overeenkomen, wordt de bewerking niet voltooid in AzCopy hervat. Als deze gegevens niet overeenkomen, wordt u gevraagd of het journaalbestand wilt overschrijven start een nieuwe bewerking of de huidige bewerking te annuleren.

De journal-bestand wordt verwijderd nadat de bewerking is voltooid.

Houd er rekening mee dat het hervatten van een bewerking van een journal-bestand gemaakt met een eerdere versie van AzCopy wordt niet ondersteund.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="parameter-file"></a>/@:"parameter-file"

Hiermee geeft u een bestand met parameters. AzCopy worden de parameters in het bestand verwerkt alsof ze hadden opgegeven op de opdrachtregel.

In een antwoordbestand, kunt u meerdere parameters opgeven op één regel of elke parameter opgeven op een aparte regel. Houd er rekening mee dat een afzonderlijke parameter kan niet over meerdere regels verdeeld.

Reactie van bronbestanden zijn commentaar regels die met het teken beginnen #.

U kunt meerdere bestanden van antwoord opgeven. Let er wel AzCopy ondersteunt geen geneste antwoord bestanden.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="y"></a>/Y

Onderdrukt alle AzCopy bevestiging vragen.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="l"></a>/L

Hiermee geeft u de bewerking van een aanbieding. Er zijn geen gegevens gekopieerd.

AzCopy zal interpreteren de met behulp van deze optie als een simulatie voor het uitvoeren van de opdrachtregel zonder deze optie /L en tellen hoeveel objecten worden gekopieerd, kunt u de optie/v op hetzelfde moment om te controleren welke objecten worden gekopieerd in het uitgebreide logboekbestand opgeven.

Het gedrag van deze optie wordt ook bepaald door de locatie van de brongegevens en de aanwezigheid van de recursieve modus optie /S bestand patroon optie en /Pattern.

AzCopy moet lijst lezen en de machtiging van deze locatie wanneer u deze optie.

**Van toepassing op:** BLOB's, bestanden

### <a name="mt"></a>/MT

Hiermee stelt u het gedownloade bestand tijd laatste wijziging moet dezelfde zijn als de bron blob of van het bestand.

**Van toepassing op:** BLOB's, bestanden

### <a name="xn"></a>/XN

Aangeven dat een nieuwe bron bron. De resource wordt niet gekopieerd als de laatst gewijzigd van de bron hetzelfde is of nieuwer zijn dan de bestemming.

**Van toepassing op:** BLOB's, bestanden

### <a name="xo"></a>/XO

Hiermee sluit u een oudere bron bron. De resource wordt niet gekopieerd als de laatst gewijzigd van de bron gelijk is of ouder is dan de bestemming.

**Van toepassing op:** BLOB's, bestanden

### <a name="a"></a>/A

Alleen bestanden die het kenmerk Archief hebben geüpload.

**Van toepassing op:** BLOB's, bestanden

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Alleen bestanden die een van de opgegeven kenmerken ingesteld hebt geüpload.

Beschikbare kenmerken omvatten:

- R = alleen-lezen bestanden
- A = bestanden gereed voor archivering
- S = systeembestanden
- H = Verborgen bestanden
- C = gecomprimeerde bestanden
- N = normale bestanden
- E = gecodeerde bestanden
- T = tijdelijke bestanden
- O = off line bestanden
- I = niet-geïndexeerde bestanden

**Van toepassing op:** BLOB's, bestanden

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Bestanden met een van de opgegeven kenmerken ingesteld, worden uitgesloten.

Beschikbare kenmerken omvatten:

- R = alleen-lezen bestanden
- A = bestanden gereed voor archivering
- S = systeembestanden
- H = Verborgen bestanden
- C = gecomprimeerde bestanden
- N = normale bestanden
- E = gecodeerde bestanden
- T = tijdelijke bestanden
- O = off line bestanden
- I = niet-geïndexeerde bestanden

**Van toepassing op:** BLOB's, bestanden

### <a name="delimiterdelimiter"></a>/ Scheidingsteken: "Scheidingstekens"

Geeft het scheidingsteken dat wordt gebruikt voor het scheiden van de virtuele mappen in een blob-naam.

Standaard wordt gebruikt voor AzCopy / als scheidingsteken. AzCopy ondersteunt echter met behulp van een gemeenschappelijke teken (zoals @, # of %) als scheidingsteken. Als u één van deze speciale tekens op de opdrachtregel, plaatst u de naam van het bestand met dubbele aanhalingstekens.

Deze optie is alleen van toepassing voor het downloaden van BLOB's.

**Van toepassing op:** BLOB 's

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "nummer-van-gelijktijdige-bewerkingen"

Hiermee geeft u het aantal gelijktijdige bewerkingen.

AzCopy standaard begint een aantal gelijktijdige bewerkingen te verhogen, de gegevensdoorvoer overdracht. Opmerking: groot aantal gelijktijdige bewerkingen in een omgeving met lage bandbreedte, kan de netwerkverbinding overstelpen en te voorkomen dat de bewerkingen volledig uitvoeren. Gelijktijdige bewerkingen op basis van de werkelijke beschikbare netwerkbandbreedte beperken.

De bovengrens van gelijktijdige bewerkingen is 512.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="sourcetypeblob--table"></a>/ SourceType: 'Blob' | 'Tabel'

Hiermee geeft u aan dat de `source` bron is een blob die beschikbaar zijn in de omgeving van plaatselijke ontwikkeling, uitgevoerd in de emulator opslag.

**Van toepassing op:** BLOB's, tabellen

### <a name="desttypeblob--table"></a>/ DestType: 'Blob' | 'Tabel'

Hiermee geeft u aan dat de `destination` bron is een blob die beschikbaar zijn in de omgeving van plaatselijke ontwikkeling, uitgevoerd in de emulator opslag.

**Van toepassing op:** BLOB's, tabellen

### <a name="pkrskey1key2key3"></a>/ PKRS: "key1 #key2 key3 #..."

Splitst de belangrijkste bereik partitie zodat u tabelgegevens parallel verhoogt de snelheid van de exportbewerking exporteren.

Als deze optie niet is opgegeven, gebruikt AzCopy één thread tabel entiteiten exporteren. Als de gebruiker geeft een /PKRS bijvoorbeeld: "aa #bb" en vervolgens AzCopy begint drie gelijktijdige bewerkingen.

Elke bewerking exporteert een van de drie belangrijkste bereiken van partitie, zoals hieronder wordt weergegeven:

  [eerste partitie-toetsen, aa)

  [aa, bb)

  [bb laatste partitie toets]

**Van toepassing op:** Tabellen

### <a name="splitsizefile-size"></a>/ SplitSize: "bestandsgrootte"

Hiermee geeft u het geëxporteerde bestand splitsen grootte in MB, de minimale toegestane waarde is 32.

Als deze optie niet is opgegeven, wordt AzCopy gegevens in een tabel exporteren naar één bestand.

Als de tabelgegevens worden geëxporteerd naar een blob en de geëxporteerde bestandsgrootte de 200 GB-limiet voor de blobgrootte bereikt, vervolgens splitsen AzCopy het geëxporteerde bestand zelfs als deze optie niet is opgegeven.

**Van toepassing op:** Tabellen

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: 'InsertOrSkip' | "InsertOrMerge" | "InsertOrReplace"

Hiermee geeft u het gedrag van tabel gegevens importeren.

- InsertOrSkip - slaat over een bestaande entiteit of een nieuwe entiteit wordt ingevoegd als deze nog niet bestaat in de tabel.

- InsertOrMerge - samenvoegingen van een bestaande entiteit of een nieuwe entiteit wordt ingevoegd als deze nog niet bestaat in de tabel.

- InsertOrReplace - vervangt een bestaande entiteit of een nieuwe entiteit wordt ingevoegd als deze nog niet bestaat in de tabel.

**Van toepassing op:** Tabellen

### <a name="manifestmanifest-file"></a>/ Manifest: "manifest-bestand"

Het manifestbestand bevat voor de tabel exporteren en importeren van de bewerking.

Deze optie is optioneel tijdens de exportbewerking, AzCopy genereert een manifestbestand met vooraf gedefinieerde naam als deze optie niet is opgegeven.

Deze optie is vereist voor het zoeken van de bestanden tijdens het importeren.

**Van toepassing op:** Tabellen

### <a name="synccopy"></a>/ SyncCopy

Geeft aan of BLOB's of bestanden tussen twee eindpunten van Azure opslag synchroon kopiëren.

AzCopy standaard wordt gebruikt voor asynchrone serverversie. Geef deze optie voor het uitvoeren van een synchrone kopie BLOB's of bestanden gedownload naar het lokale geheugen en vervolgens worden geüpload naar de opslag van Azure.

U kunt deze optie gebruiken bij het kopiëren van bestanden in een Blob-opslag in de opslag van bestanden of Blob-opslag in de opslag van bestanden of vice versa.

**Van toepassing op:** BLOB's, bestanden

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "content-type"

Hiermee geeft u het MIME-inhoudtype voor bestemming BLOB's of bestanden.

AzCopy wordt het type inhoud voor een bestand of blob ingesteld op application/octet-stream standaard. Door expliciet een waarde voor deze optie kunt u het type inhoud voor alle BLOB's of bestanden instellen.

Als u deze optie zonder een waarde opgeeft, vervolgens instellen AzCopy elke blob of inhoudstype volgens het de bestandsextensie van het bestand.

**Van toepassing op:** BLOB's, bestanden

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: 'JSON' | "CSV"

Hiermee geeft u de indeling van het bestand met de geëxporteerde gegevens tabel.

Als deze optie niet is opgegeven, standaard AzCopy geëxporteerd tabelbestand in JSON-indeling.

**Van toepassing op:** Tabellen

## <a name="known-issues-and-best-practices"></a>Bekende problemen en Best Practices

### <a name="limit-concurrent-writes-while-copying-data"></a>Gelijktijdige schrijfbewerkingen tijdens het kopiëren van gegevens beperken

Bij het kopiëren van bestanden met AzCopy of BLOB's, houd er rekening mee dat een andere toepassing de gegevens heeft terwijl u kopieert. Indien mogelijk, ervoor te zorgen dat de gegevens die u kopieert niet wordt gewijzigd tijdens het kopiëren. Bijvoorbeeld bij het kopiëren van een VHD die is gekoppeld aan een Azure virtuele machine, zorg dat er geen andere toepassingen die momenteel op de VHD schrijft. Een goede manier om dit te doen is door overdracht van de resource moet worden gekopieerd. U kunt ook eerst een momentopname van de VHD maken en vervolgens de momentopname kopiëren.

Als u niet kunt voorkomen andere toepassingen schrijven van BLOB's of bestanden dat terwijl ze worden gekopieerd, vervolgens Houd er rekening mee dat op het moment dat de taak is voltooid, de gekopieerde bronnen niet langer mogelijk volledige pariteit met de bronnen van de bron.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Één exemplaar van de AzCopy op één computer uitvoeren.
AzCopy is ontworpen om te maximaliseren van het gebruik van de bron van de machine te versnellen, de overdracht van gegevens, is het raadzaam slechts één exemplaar van de AzCopy op één computer worden uitgevoerd en u de optie `/NC` als u meer gelijktijdige bewerkingen. Typ voor meer informatie `AzCopy /?:NC` op de opdrachtregel.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Inschakelen van FIPS-compliant MD5 algorithmes voor AzCopy wanneer u ' gebruik FIPS compliant algorithmes voor codering, hashing en ondertekening '.
AzCopy standaard .NET MD5-implementatie gebruikt voor het berekenen van de MD5 bij het kopiëren van objecten, maar er zijn enkele beveiligingsvereisten die AzCopy inschakelen van FIPS compatibele MD5 instelling nodig.

U kunt een bestand app.config maken `AzCopy.exe.config` met de eigenschap `AzureStorageUseV1MD5` en het bestemmen met AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Voor de eigenschap 'AzureStorageUseV1MD5' • waar - gebruikt de standaardwaarde, AzCopy .NET MD5-implementatie.
• False – AzCopy FIPS-compliant MD5-algoritme gebruikt.

FIPS-algoritmen is standaard uitgeschakeld op uw Windows-computer, dat u kunt Typ secpol.msc in het venster uitvoeren en controleren van deze schakeloptie bij beveiligingsinstelling -> lokaal beleid Beveiliging -> Opties -> Systeemcryptographie: gebruik FIPS-algoritmen voor codering, hashing en ondertekening.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende bronnen voor meer informatie over Azure opslag- en AzCopy.

### <a name="azure-storage-documentation"></a>Azure opslag documentatie:

- [Inleiding tot Azure opslag](storage-introduction.md)
- [Het gebruik van .NET Blob-opslag](storage-dotnet-how-to-use-blobs.md)
- [Het gebruik van opslag van .NET](storage-dotnet-how-to-use-files.md)
- [Het gebruik van opslag van .NET tabel](storage-dotnet-how-to-use-tables.md)
- [Het maken, beheren of een opslag-account verwijderen](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure opslag blog-updates:
- [Introductie van voorbeeld van Azure opslag gegevensverplaatsing bibliotheek](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Inleiding tot synchroon kopiëren en aangepaste inhoudstype](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Aankondiging van algemene beschikbaarheid van AzCopy 3.0 plus preview-versie van AzCopy 4.0 en ondersteuning](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Geoptimaliseerd voor het kopiëren van grootschalige scenario 's](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Ondersteuning voor geo-redundante opslag lezen toegang](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Overdracht van gegevens met modus opnieuw meer gestart en SAS-token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Met behulp van cross-account kopiëren Blob](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Uploaden/downloaden van bestanden voor Azure BLOB 's](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
