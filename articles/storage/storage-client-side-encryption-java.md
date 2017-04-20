<properties
    pageTitle="Client-Side-codering met Java voor Microsoft Azure opslag | Microsoft Azure"
    description="De Azure opslag Client Library for Java ondersteunt codering van client-side en integratie met Azure sleutel kluis voor een maximale beveiliging voor uw toepassingen Azure opslag."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-java-for-microsoft-azure-storage"></a>Client-Side-codering met Java voor Microsoft Azure opslag   

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Overzicht  

De [Azure opslag Client Library for Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) ondersteunt het coderen van gegevens binnen toepassingen voordat u gaat uploaden naar Azure opslag en decoderen van gegevens tijdens het downloaden van de client. De bibliotheek ondersteunt ook integratie met [Azure sleutel kluis](https://azure.microsoft.com/services/key-vault/) voor opslag account managers.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Codering en decodering via de envelop-techniek    
De processen van de codering en decodering Volg de techniek van de envelop.  

### <a name="encryption-via-the-envelope-technique"></a>Codering via de envelop-techniek  
Codering via de envelop-techniek werkt op de volgende manier:  

1.  De clientbibliotheek Azure opslag genereert een inhoud coderingssleutel (CEK), die een symmetrische sleutel met een eenmalige gebruik is.  

2.  Gegevens van de gebruiker wordt gecodeerd met behulp van deze CEK.   

3.  De CEK wordt vervolgens verpakt (gecodeerd) met behulp van de coderingssleutel key (KEK). De KEK kan wordt aangeduid met een sleutel-id en een symmetrische sleutel zijn of een combinatie van een asymmetrische sleutel en kan lokaal beheerd of opgeslagen in Azure sleutel kluizen.  
De client-bibliotheek van opslag zelf heeft nooit toegang tot KEK. De bibliotheek roept de belangrijke onmiddellijke algoritme die wordt geleverd door de sleutel kluis. Gebruikers kunnen aangepaste providers gebruiken voor belangrijke onmiddellijke verpakking/uitpakken indien gewenst kiezen.  

4.  De gecodeerde gegevens is vervolgens naar de Azure Storage-service geüpload. De tekstomloop toets samen met enkele extra codering metagegevens is opgeslagen als metagegevens (op een blob) of (geïnterpoleerd) met de gecodeerde gegevens (Wachtrijberichten en entiteiten van de tabel).

### <a name="decryption-via-the-envelope-technique"></a>Decodering via de envelop-techniek  
Decodering via de envelop-techniek werkt op de volgende manier:  

1.  De clientbibliotheek wordt ervan uitgegaan dat de gebruiker de sleutel coderingssleutel (KEK) lokaal of in Azure sleutel kluizen beheert. De gebruiker hoeft niet op de hoogte van de specifieke sleutel die is gebruikt voor de codering. In plaats daarvan worden een belangrijke resolver die verschillende sleutel-id's worden omgezet in sleutels ingesteld en gebruikt.  

2.  De clientbibliotheek gedownload de versleutelde gegevens met codering materiaal dat is opgeslagen op de service.  

3.  Omwikkelde (gedecodeerd) met behulp van de codering met sleutels sleutel (KEK) is de ingepakte inhoud coderingssleutel (CEK). Hier opnieuw, heeft de clientbibliotheek geen toegang tot KEK. Gewoon Hiermee wordt de aangepaste of uitpakken algoritme sleutel kluis van de aanbieder.  

4.  De coderingssleutel inhoud (CEK) wordt vervolgens gebruikt om de gecodeerde gebruikersgegevens te decoderen.

## <a name="encryption-mechanism"></a>Coderingsmethode  
De client-bibliotheek van opslag maakt gebruik van [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) te coderen gegevens van de gebruiker. Met name [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) modus met AES. Elke service enigszins anders werkt dus we elk van hen hier bespreken.

### <a name="blobs"></a>BLOB 's  
De clientbibliotheek ondersteunt op dit moment codering van volledige BLOB's. Met name codering wanneer gebruikers met het **uploaden van** * methoden of de * *openOutputStream** methode. Voor downloads, zowel volledige als bereik downloads worden ondersteund.  

Tijdens het coderen, wordt de clientbibliotheek genereren van een willekeurige initialisatie Vector (IV) van 16 bytes, met een willekeurige inhoud coderingssleutel (CEK) 32 bytes en envelop codering van de blob-gegevens met behulp van deze gegevens uitvoeren. De tekstomloop CEK en sommige metagegevens extra codering worden vervolgens opgeslagen als blob-metagegevens en de gecodeerde blob van de service.

>[AZURE.WARNING] Als u bewerken of uploaden van uw eigen metagegevens voor de blob, moet u ervoor te zorgen dat deze metagegevens behouden blijft. Als u de nieuwe metagegevens zonder deze metagegevens uploadt, de tekstomloop CEK, IV en andere metagegevens verloren en de blob inhoud nooit meer opvraagbare zal zijn.

Ophalen van de inhoud van de gehele blob met behulp van de */openInputStream *downloaden**downloaden van een gecodeerde blob omvat * gemak methoden. De tekstomloop CEK is grenzeloos en gebruikt in combinatie met de IV (opgeslagen als blob metagegevens in dit geval) de gedecodeerde om gegevens te retourneren aan de gebruikers.

Downloaden van een willekeurig bereik (**downloadRange*** methoden) bestaat uit het bereik die door gebruikers om een kleine hoeveelheid extra gegevens die kunnen worden gebruikt voor het decoderen van het opgegeven bereik met succes aan te passen in de gecodeerde blob.  

Alle typen blob (BLOB's blokkeren pagina BLOB's en append BLOB's) kunnen worden gecodeerd/gedecodeerd met behulp van dit schema.

### <a name="queues"></a>Wachtrijen  
Aangezien berichten van een willekeurige indeling worden kunnen, wordt de clientbibliotheek een aangepaste notatie met de initialisatie Vector (IV) en de gecodeerde inhoud coderingssleutel (CEK) in de tekst van het bericht.  

Tijdens de codering, de clientbibliotheek genereert een willekeurig IV van 16 bytes in combinatie met een willekeurige CEK 32 bytes en envelop codering van de tekst van de wachtrij met behulp van deze gegevens wordt uitgevoerd. De tekstomloop CEK en sommige extra codering metagegevens worden toegevoegd aan de wachtrij gecodeerde bericht. Deze gewijzigde bericht (Zie hieronder) is opgeslagen op de service.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

De tekstomloop sleutel is tijdens het ontsleutelen, afkomstig uit het bericht wachtrij en grenzeloos. De IV is ook afkomstig uit het bericht wachtrij en de omwikkelde sleutel gebruikt voor het decoderen van de gegevens van de message queue. Let erop dat de metagegevens codering klein (onder de 500 bytes), dus terwijl het telt mee voor de limiet van 64KB voor een bericht wachtrij, de impact mag beheersbaar.

### <a name="tables"></a>Tabellen  
De clientbibliotheek ondersteunt codering van de entiteitseigenschappen voor insert- en vervangopdrachten.

>[AZURE.NOTE] Samenvoegen wordt momenteel niet ondersteund. Aangezien een subset van eigenschappen kan zijn versleuteld eerder met een andere sleutel, leidt gewoon samenvoegen van de nieuwe eigenschappen en het bijwerken van de metagegevens tot verlies van gegevens. Samenvoegen van een vereist extra service gesprekken te lezen van de reeds bestaande entiteit van de service of met een nieuwe sleutel per eigenschap, die beide zijn niet geschikt voor betere prestaties.

Tabel gegevenscodering werkt als volgt:  

1.  Gebruikers geven de eigenschappen moeten worden gecodeerd.  

2.  De clientbibliotheek genereert een willekeurig initialisatie Vector (IV) van 16 bytes in combinatie met een willekeurige inhoud coderingssleutel (CEK) 32 bytes voor elke entiteit en envelop codering uitvoert op de afzonderlijke eigenschappen moeten worden gecodeerd door een nieuwe IV per eigenschap af te leiden. De gecodeerde eigenschap wordt opgeslagen als binaire gegevens.  

3.  De tekstomloop CEK en sommige metagegevens extra codering worden vervolgens opgeslagen als twee extra gereserveerde eigenschappen. De eerste gereserveerde eigenschap (_ClientEncryptionMetadata1) is een tekenreekseigenschap met de informatie over IV, versie en tekstomloop sleutel. De tweede gereserveerde eigenschap (_ClientEncryptionMetadata2) is een binaire eigenschap met de informatie over de eigenschappen die zijn gecodeerd. De informatie in deze tweede eigenschap (_ClientEncryptionMetadata2) is zelf ook gecodeerd.  

4.  Door deze extra gereserveerde eigenschappen vereist voor codering wellicht gebruikers nu slechts 250 aangepaste eigenschappen in plaats van 252. De totale grootte van de entiteit moet minder dan 1MB.  

    Houd er rekening mee dat alleen de tekenreekseigenschappen kunnen worden gecodeerd. Als andere typen eigenschappen moeten worden gecodeerd, moeten ze worden geconverteerd naar tekenreeksen. De gecodeerde tekenreeksen voor de service worden opgeslagen als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen na decodering.

    Gebruikers moeten de eigenschappen worden versleuteld opgeven voor tabellen, naast het coderingsbeleid. Dit kan worden gedaan door een kenmerk [coderen] (voor POCO entiteiten die zijn afgeleid van TableEntity) of een resolver codering in de opties van de aanvraag. Een resolver codering is een gemachtigde die ervoor zorgt dat een partitiesleutel, de rijsleutel en naam van de eigenschap retourneert een boolean die aangeeft of die eigenschap moet worden gecodeerd. Tijdens het coderen, de client-bibliotheek van deze informatie wordt gebruikt om te bepalen of een eigenschap moet worden gecodeerd tijdens het schrijven naar de draad. De gemachtigde biedt ook de mogelijkheid van logica rond hoe eigenschappen worden gecodeerd. (Bijvoorbeeld als X, vervolgens coderen eigenschap A; anders coderen eigenschappen A en B.) Opmerking Het is niet nodig om deze informatie bij het lezen of het opvragen van entiteiten.

### <a name="batch-operations"></a>Batchbewerkingen  
In batchbewerkingen wordt de dezelfde KEK gebruikt in alle rijen in deze batchbewerking omdat de clientbibliotheek kunt u alleen een options-object (en dus een beleid/KEK) per batchbewerking. Echter, de clientbibliotheek intern genereert een nieuw willekeurig IV en willekeurige CEK per rij in de batch. Gebruikers kunnen ook kiezen voor het coderen van andere eigenschappen voor elke bewerking in de batch als u dit probleem in de resolver codering.

### <a name="queries"></a>Query 's  
Om de querybewerkingen uitvoert, moet u een sleutel-omzetter die kan omzetten van de sleutels in de resultaatset. Als een entiteit die is opgenomen in het resultaat van de query kan niet worden omgezet naar een provider, wordt de client-bibliotheek van een fout genereren. Voor elke query die u, server side projecties uitvoert, wordt de client-bibliotheek van de eigenschappen van de metagegevens speciale codering (_ClientEncryptionMetadata1 en _ClientEncryptionMetadata2) standaard toegevoegd aan de geselecteerde kolommen.

## <a name="azure-key-vault"></a>Azure sleutel kluis  
Azure sleutel kluis kunt u cryptografische sleutels beveiliging en geheimen die door cloud-toepassingen en services. Met behulp van Azure sleutel kluis, kunnen gebruikers coderen sleutels en geheimen (zoals verificatiesleutels opslag account sleutels, sleutels voor gegevenscodering. PFX-bestanden en wachtwoorden) met behulp van toetsen die zijn beveiligd door hardware security modules (HSM's). Zie voor meer informatie [Wat is Azure sleutel kluis?](../key-vault/key-vault-whatis.md).

De client-bibliotheek van opslag gebruikt de sleutel kluis core-bibliotheek om een gemeenschappelijk kader voor het beheer van sleutels in Azure. Gebruikers krijgen ook het extra voordeel van het gebruik van de bibliotheek sleutel kluis extensies. De bibliotheek extensions biedt functionaliteit om eenvoudig en naadloos symmetrisch/RSA lokale en belangrijkste aanbieders van cloud en aggregatie- en caching.

### <a name="interface-and-dependencies"></a>Interface en afhankelijkheden  
Er zijn drie sleutel kluis pakketten:  

- Azure-keyvault-core bevat de IKey en IKeyResolver. Het is een kleine verpakking met geen afhankelijkheden. De client-bibliotheek van opslag voor Java wordt gedefinieerd als een afhankelijkheid.
- Azure-keyvault bevat de sleutel kluis REST-client.  
- Azure-keyvault-extensies bevat extensie bevat implementaties van cryptografische algoritmen en een RSAKey en een SymmetricKey. Het hangt af van de naamruimten Core en KeyVault en biedt functionaliteit om een statistische resolver (als gebruikers met meerdere belangrijke providers) en een belangrijke caching resolver te definiëren. Hoewel de clientbibliotheek opslag niet rechtstreeks afhankelijk van dit pakket, is als gebruikers Azure sleutel kluis gebruiken wilt om de sleutels te gebruiken om de sleutel kluis extensies te verbruiken de lokale cryptografieproviders cloud, moeten ze dit pakket.  

  Sleutel kluis is ontworpen voor hoge waarde hoofdsleutels en bandbreedteregeling limieten per sleutel kluis zijn ontworpen met dit in gedachten. Bij het uitvoeren van client-side-codering met een sleutel kluis is het gewenste model lokaal gebruik van symmetrische hoofdsleutels opgeslagen als geheimen in sleutel kluis en in de cache opgeslagen. Gebruikers moeten het volgende doen:  

1.  Off line een geheim maken en uploaden naar de sleutel kluis.  

2.  Het geheim van de basis-id gebruikt als een parameter kunt oplossen door de huidige versie van het geheim voor codering en deze informatie lokaal in de cache. CachingKeyResolver gebruiken voor het in cache plaatsen; gebruikers niet worden geacht voor de uitvoering van hun eigen logica caching.  

3.  De caching resolver als input gebruikt tijdens het maken van het coderingsbeleid.
In de codevoorbeelden codering vindt u meer informatie over het gebruik van de sleutel kluis. <fix URL>  

## <a name="best-practices"></a>Aanbevolen procedures  
Ondersteuning voor codering is alleen beschikbaar in de client-bibliotheek van opslag voor Java.

>[AZURE.IMPORTANT] Let op deze belangrijke punten bij het gebruik van client-side-codering:
>  
>- Bij het lezen van of schrijven naar een gecodeerde blob, hele blob uploaden opdrachten en bereik/geheel blob downloaden opdrachten gebruiken. Schrijf naar een gecodeerde blob met protocolbewerkingen zoals het blok geplaatst, lijst geblokkeerd geplaatst, pagina's schrijven, duidelijke's of blok toevoegen; anders mag het gecodeerde blob beschadigd en onleesbaar maken.
>
>- Voor tabellen bestaat een dergelijke beperking. Zorg ervoor dat gecodeerde eigenschappen niet bijwerken zonder de codering metagegevens worden bijgewerkt.  
>
>- Als u metagegevens in de gecodeerde blob instelt, overschrijft u mogelijk de metagegevens met betrekking tot codering vereist voor decodering, omdat het instellen van metagegevens niet additief. Dit geldt ook voor momentopnamen; Vermijd metagegevens opgeven tijdens het maken van een momentopname van een gecodeerde blob. Metagegevens moet worden ingesteld, moet u roept de methode **downloadAttributes** eerst om de metagegevens van de huidige codering als gelijktijdige schrijfbewerkingen te voorkomen terwijl de metagegevens wordt ingesteld.  
>
>- De vlag **requireEncryption** in de aanvraag standaardopties voor gebruikers die met gecodeerde gegevens alleen werken inschakelen. Zie hieronder voor meer info.  

## <a name="client-api--interface"></a>Client-API / Interface  
Tijdens het maken van een object EncryptionPolicy gebruikers alleen een sleutel (IKey implementeren), kunnen bieden alleen een resolver (implementatie van IKeyResolver), of beide. IKey is het elementaire type sleutel die wordt geïdentificeerd met behulp van een sleutel-id en die de logica voor de verpakking/uitpakken. IKeyResolver wordt gebruikt voor het oplossen van een sleutel tijdens het decoderingsproces. Hiermee definieert u een ResolveKey methode geeft als resultaat een IKey sleutel-id opgegeven. Dit biedt gebruikers de mogelijkheid te kiezen tussen meerdere sleutels die worden beheerd op meerdere locaties.

- De sleutel wordt altijd gebruikt voor codering, en de afwezigheid van een sleutel in een fout resulteert.  
- Voor decodering:  
    - De belangrijkste resolver wordt aangeroepen als de sleutel wordt opgegeven. Als de resolver is opgegeven, maar beschikt niet over een toewijzing voor de sleutel-id, wordt een fout gegenereerd.  
    - Als een resolver niet is opgegeven, maar een sleutel is opgegeven, wordt de sleutel wordt gebruikt als de id overeenkomt met de vereiste sleutel-id. Als de id niet overeenkomt, wordt een fout gegenereerd.  

      De [codering voorbeelden](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>tonen in een meer gedetailleerde end-to-end scenario BLOB's, wachtrijen en tabellen, samen met sleutel kluis integratie.

### <a name="requireencryption-mode"></a>RequireEncryption-modus  
Gebruikers kunnen desgewenst inschakelen voor een modus waarbij alle uploads en downloads worden gecodeerd. In deze modus kunnen gegevens zonder een coderingsbeleid uploaden of downloaden van gegevens die niet zijn gecodeerd op de service, niet op de client. De vlag **requireEncryption** van het object request opties bepaalt dit gedrag. Als uw toepassing, alle objecten die zijn opgeslagen in Azure opslag worden gecodeerd, kunt u de eigenschap **requireEncryption** instellen op de standaardopties voor de aanvraag voor het client-object.   

Gebruik **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** bijvoorbeeld aangeven dat codering vereist voor alle blob bewerkingen die worden uitgevoerd door middel van dat clientobject.

### <a name="blob-service-encryption"></a>BLOB service codering  
Een **BlobEncryptionPolicy** -object maken en stel deze in de aanvraag (per API of op een client met behulp van **DefaultRequestOptions**). Al het andere wordt verwerkt door de client-typebibliotheek intern.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions();
    options.setEncryptionPolicy(policy);

    // Upload the encrypted contents to the blob.
    blob.upload(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    blob.download(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Queue service codering  
Een **QueueEncryptionPolicy** -object maken en stel deze in de aanvraag (per API of op een client met behulp van **DefaultRequestOptions**). Al het andere wordt verwerkt door de client-typebibliotheek intern.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions();
    options.setEncryptionPolicy(policy);

    queue.addMessage(message, 0, 0, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);

### <a name="table-service-encryption"></a>Tabel service-codering  
Naast een coderingsbeleid maken en op aanvraag opties instelt, moet u een **EncryptionResolver** opgeven in de **TableRequestOptions**of stelt u het kenmerk [coderen] op de getter en setter van de entiteit.

### <a name="using-the-resolver"></a>Met behulp van de resolver  

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    options.setEncryptionPolicy(policy);
    options.setEncryptionResolver(new EncryptionResolver() {
        public boolean encryptionResolver(String pk, String rk, String key) {
            if (key == "foo")
            {
                return true;
            }
            return false;
        }
    });

    // Insert Entity
    currentTable.execute(TableOperation.insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    retrieveOptions.setEncryptionPolicy(policy);

    TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
    TableResult result = currentTable.execute(operation, retrieveOptions, null);

### <a name="using-attributes"></a>Met behulp van kenmerken  
Zoals hierboven vermeld, als de entiteit TableEntity, implementeert en vervolgens de eigenschappen getter en setter gedecoreerd met het kenmerk [coderen] in plaats van de **EncryptionResolver**.

    private string encryptedProperty1;

    @Encrypt
    public String getEncryptedProperty1 () {
        return this.encryptedProperty1;
    }

    @Encrypt
    public void setEncryptedProperty1(final String encryptedProperty1) {
        this.encryptedProperty1 = encryptedProperty1;
    }

## <a name="encryption-and-performance"></a>Codering en prestaties  
Houd er rekening mee dat de resultaten van opslag in meer belast versleutelt. De inhoud en IV moet worden gegenereerd, de inhoud zelf moet worden gecodeerd en aanvullende metagegevens worden geformatteerd en geüpload. Deze belasting is afhankelijk van de hoeveelheid gegevens worden gecodeerd. Het is raadzaam dat klanten altijd hun prestaties tijdens de ontwikkeling van de toepassingen testen.

## <a name="next-steps"></a>Volgende stappen  

- De [Azure opslag Client-bibliotheek voor Java, Maven-pakket](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) downloaden  
- De [Client-bibliotheek van Azure opslag voor Java-broncode van GitHub](https://github.com/Azure/azure-storage-java) downloaden   
- Azure sleutel kluis Maven bibliotheek voor Java, Maven pakketten downloaden:
    - Pakket [Core](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core)
    - [Client](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault) -pakket
- Ga naar de [Azure sleutel kluis documentatie](../key-vault/key-vault-whatis.md)  
