<properties
    pageTitle="Client-Side-codering met Python voor de opslag van Microsoft Azure | Microsoft Azure"
    description="De clientbibliotheek van Azure opslag voor Python ondersteunt codering van client-side voor een maximale beveiliging voor uw toepassingen Azure opslag."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Client-Side-codering met Python voor de opslag van Microsoft Azure

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Overzicht

De [Azure opslag Client Library voor Python](https://pypi.python.org/pypi/azure-storage) ondersteunt het coderen van gegevens binnen toepassingen voordat u gaat uploaden naar Azure opslag en decoderen van gegevens tijdens het downloaden van de client.

>[AZURE.NOTE] De bibliotheek Azure opslag Python is in voorbeeld.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Codering en decodering via de envelop-techniek
De processen van de codering en decodering Volg de techniek van de envelop.

### <a name="encryption-via-the-envelope-technique"></a>Codering via de envelop-techniek
Codering via de envelop-techniek werkt op de volgende manier:

1.  De clientbibliotheek Azure opslag genereert een inhoud coderingssleutel (CEK), die een symmetrische sleutel met een eenmalige gebruik is.

2.  Gegevens van de gebruiker wordt gecodeerd met behulp van deze CEK.

3.  De CEK wordt vervolgens verpakt (gecodeerd) met behulp van de coderingssleutel key (KEK). De KEK kan wordt aangeduid met een sleutel-id en een combinatie van een asymmetrische sleutel of een symmetrische sleutel die lokaal wordt beheerd.
De client-bibliotheek van opslag zelf heeft nooit toegang tot KEK. De bibliotheek roept de belangrijke onmiddellijke algoritme die wordt geleverd door de KEK. Gebruikers kunnen aangepaste providers gebruiken voor belangrijke onmiddellijke verpakking/uitpakken indien gewenst kiezen.

4.  De gecodeerde gegevens is vervolgens naar de Azure Storage-service geüpload. De tekstomloop toets samen met enkele extra codering metagegevens is opgeslagen als metagegevens (op een blob) of (geïnterpoleerd) met de gecodeerde gegevens (Wachtrijberichten en entiteiten van de tabel).

### <a name="decryption-via-the-envelope-technique"></a>Decodering via de envelop-techniek
Decodering via de envelop-techniek werkt op de volgende manier:

1.  De clientbibliotheek wordt ervan uitgegaan dat de gebruiker de belangrijkste sleutel (KEK) lokaal wordt beheerd. De gebruiker hoeft niet op de hoogte van de specifieke sleutel die is gebruikt voor de codering. In plaats daarvan worden een belangrijke resolver waarmee u verschillende sleutel-id's te toetsen, ingesteld en gebruikt.

2.  De clientbibliotheek gedownload de versleutelde gegevens met codering materiaal dat is opgeslagen op de service.

3.  Omwikkelde (gedecodeerd) met behulp van de codering met sleutels sleutel (KEK) is de ingepakte inhoud coderingssleutel (CEK). Hier opnieuw, heeft de clientbibliotheek geen toegang tot KEK. Dit roept gewoon uitpakken van de aangepaste provider-algoritme.

4.  De coderingssleutel inhoud (CEK) wordt vervolgens gebruikt om de gecodeerde gebruikersgegevens te decoderen.

## <a name="encryption-mechanism"></a>Coderingsmethode  
De client-bibliotheek van opslag maakt gebruik van [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) te coderen gegevens van de gebruiker. Met name [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) modus met AES. Elke service enigszins anders werkt dus we elk van hen hier bespreken.

### <a name="blobs"></a>BLOB 's
De clientbibliotheek ondersteunt op dit moment codering van volledige BLOB's. Name codering wanneer gebruikers met de **maken*** methoden. Downloads, zowel volledige als bereik downloads worden ondersteund, en parallelization van beide upload en download is beschikbaar.

Tijdens het coderen, wordt de clientbibliotheek genereren van een willekeurige initialisatie Vector (IV) van 16 bytes, met een willekeurige inhoud coderingssleutel (CEK) 32 bytes en envelop codering van de blob-gegevens met behulp van deze gegevens uitvoeren. De tekstomloop CEK en sommige metagegevens extra codering worden vervolgens opgeslagen als blob-metagegevens en de gecodeerde blob van de service.

>[AZURE.WARNING] Als u bewerken of uploaden van uw eigen metagegevens voor de blob, moet u ervoor te zorgen dat deze metagegevens behouden blijft. Als u de nieuwe metagegevens zonder deze metagegevens uploadt, de tekstomloop CEK, IV en andere metagegevens verloren en de blob inhoud nooit meer opvraagbare zal zijn.

Downloaden van een gecodeerde blob bestaat uit het ophalen van de inhoud van de gehele blob met de **get*** gemak methoden. De tekstomloop CEK is grenzeloos en gebruikt in combinatie met de IV (opgeslagen als blob metagegevens in dit geval) de gedecodeerde om gegevens te retourneren aan de gebruikers.

Downloaden van een willekeurig bereik (**get*** methoden met bereik parameters doorgegeven) bestaat uit het bereik die door gebruikers om een kleine hoeveelheid extra gegevens die kunnen worden gebruikt voor het decoderen van het opgegeven bereik met succes aan te passen in de gecodeerde blob.

Blok BLOB's en pagina-BLOB's kunnen alleen worden gecodeerd/gedecodeerd met behulp van dit schema. Er is momenteel geen ondersteuning voor het coderen van BLOB's toevoegen.

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

3.  De tekstomloop CEK en sommige metagegevens extra codering worden vervolgens opgeslagen als twee extra gereserveerde eigenschappen. De eerste eigenschap gereserveerd (\_ClientEncryptionMetadata1) is een tekenreekseigenschap met de informatie over IV, versie en tekstomloop sleutel. De tweede eigenschap gereserveerd (\_ClientEncryptionMetadata2) is een binaire eigenschap met de informatie over de eigenschappen die zijn gecodeerd. De informatie in deze tweede eigenschap (\_ClientEncryptionMetadata2) is zelf ook gecodeerd.

4.  Door deze extra gereserveerde eigenschappen vereist voor codering wellicht gebruikers nu slechts 250 aangepaste eigenschappen in plaats van 252. De totale grootte van de entiteit moet minder dan 1MB.

    Houd er rekening mee dat alleen de tekenreekseigenschappen kunnen worden gecodeerd. Als andere typen eigenschappen moeten worden gecodeerd, moeten ze worden geconverteerd naar tekenreeksen. De gecodeerde tekenreeksen voor de service worden opgeslagen als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen (raw tekenreeksen, niet EntityProperties met het type EdmType.STRING) na decodering.

    Gebruikers moeten de eigenschappen worden versleuteld opgeven voor tabellen, naast het coderingsbeleid. Dit kan gebeuren door op te slaan deze eigenschappen in met het type is ingesteld op EdmType.STRING TableEntity-objecten en coderen is ingesteld op true of instellen van de encryption_resolver_function op het tableservice-object. Een resolver codering is een functie die ervoor zorgt dat een partitiesleutel, de rijsleutel en naam van de eigenschap retourneert een boolean die aangeeft of die eigenschap moet worden gecodeerd. Tijdens het coderen, de client-bibliotheek van deze informatie wordt gebruikt om te bepalen of een eigenschap moet worden gecodeerd tijdens het schrijven naar de draad. De gemachtigde biedt ook de mogelijkheid van logica rond hoe eigenschappen worden gecodeerd. (Bijvoorbeeld als X, vervolgens coderen eigenschap A; anders coderen eigenschappen A en B.) Opmerking Het is niet nodig om deze informatie bij het lezen of het opvragen van entiteiten.

### <a name="batch-operations"></a>Batchbewerkingen
Een coderingsbeleid geldt voor alle rijen in de batch. De clientbibliotheek intern genereert een nieuw willekeurig IV en willekeurige CEK per rij in de batch. Gebruikers kunnen ook kiezen voor het coderen van andere eigenschappen voor elke bewerking in de batch als u dit probleem in de resolver codering.
Als een batch wordt gemaakt als een context-manager met de methode tableservice batch(), kan het coderingsbeleid van de tableservice automatisch aan de batch worden toegepast. Als een batch expliciet gemaakt wordt door het aanroepen van de constructor, moet het coderingsbeleid worden doorgegeven als een parameter en links niet gewijzigd voor de levensduur van de batch.
Houd er rekening mee dat de entiteiten worden gecodeerd als ze worden ingevoegd in de batch met behulp van beleid van de batch-codering (entiteiten zijn niet gecodeerd op het moment van de batch met behulp van de tableservice coderingsbeleid doorvoeren).

### <a name="queries"></a>Query 's
Om de querybewerkingen uitvoert, moet u een sleutel-omzetter die kan omzetten van de sleutels in de resultaatset. Als een entiteit die is opgenomen in het resultaat van de query kan niet worden omgezet naar een provider, wordt de client-bibliotheek van een fout genereren. Voor elke query die u, server side projecties uitvoert, de client-bibliotheek van de eigenschappen van speciale codering metagegevens toevoegen (\_ClientEncryptionMetadata1 en \_ClientEncryptionMetadata2) voor de geselecteerde kolommen.

>[AZURE.IMPORTANT] Let op deze belangrijke punten bij het gebruik van client-side-codering:
>
>- Bij het lezen van of schrijven naar een gecodeerde blob, hele blob uploaden opdrachten en bereik/geheel blob downloaden opdrachten gebruiken. Schrijf naar een gecodeerde blob met protocolbewerkingen zoals het blok geplaatst, lijst geblokkeerd geplaatst, pagina's schrijven of normale pagina's. anders mag het gecodeerde blob beschadigd en onleesbaar maken.
>
>- Voor tabellen bestaat een dergelijke beperking. Zorg ervoor dat gecodeerde eigenschappen niet bijwerken zonder de codering metagegevens worden bijgewerkt.
>
>- Als u metagegevens in de gecodeerde blob instelt, overschrijft u mogelijk de metagegevens met betrekking tot codering vereist voor decodering, omdat het instellen van metagegevens niet additief. Dit geldt ook voor momentopnamen; Vermijd metagegevens opgeven tijdens het maken van een momentopname van een gecodeerde blob. Metagegevens moet worden ingesteld, moet u roept de methode **get_blob_metadata** eerst om de metagegevens van de huidige codering als gelijktijdige schrijfbewerkingen te voorkomen terwijl de metagegevens wordt ingesteld.
>
>- De vlag **require_encryption** op het object voor gebruikers die met gecodeerde gegevens alleen werken inschakelen. Zie hieronder voor meer info.

De client-bibliotheek van opslag verwacht dat de opgegeven KEK en resolver sleutel de volgende interface implementeren. Ondersteuning voor Python KEK beheer [Azure sleutel kluis](https://azure.microsoft.com/services/key-vault/) is in behandeling en zullen worden geïntegreerd in deze bibliotheek wanneer voltooid.


## <a name="client-api--interface"></a>Client-API / Interface
Nadat een opslag-object (dat wil zeggen blockblobservice) is gemaakt, de gebruiker waarden kan toewijzen aan de velden die een coderingsbeleid vormen: key_encryption_key, key_resolver_function, en require_encryption. Gebruikers kunnen alleen een KEK bieden alleen een resolver of beide. key_encryption_key is de elementaire type sleutel die wordt geïdentificeerd met behulp van een sleutel-id en die de logica voor de verpakking/uitpakken. key_resolver_function wordt gebruikt voor het oplossen van een sleutel tijdens het decoderingsproces. Het resultaat een geldige KEK sleutel-id opgegeven. Dit biedt gebruikers de mogelijkheid te kiezen tussen meerdere sleutels die worden beheerd op meerdere locaties.

De KEK moet de volgende methoden voor het coderen van gegevens met succes te implementeren:
- wrap_key(cek): loopt het met behulp van een algoritme van de keuze van de gebruiker opgegeven CEK (bytes). Geeft als resultaat de tekstomloop sleutel.
- get_key_wrap_algorithm(): geeft als resultaat het algoritme dat wordt gebruikt om sleutels.
- get_kid(): geeft als resultaat de tekenreeks sleutel-id voor deze KEK.
De volgende methoden zijn om gegevens te decoderen, moet de KEK implementeren:
- unwrap_key (cek, algoritme): de omwikkelde vorm van het opgegeven CEK met de algoritme opgegeven tekenreeks als resultaat gegeven.
- get_kid(): geeft als resultaat een string-sleutel-id voor deze KEK.

De belangrijkste resolver moet ten minste een methode die een sleutel-id, gegeven geeft als resultaat de bijbehorende KEK ter uitvoering van de bovenstaande-interface implementeren. Alleen deze methode is om te worden toegewezen aan de eigenschap key_resolver_function van het object.

- De sleutel wordt altijd gebruikt voor codering, en de afwezigheid van een sleutel in een fout resulteert.
- Voor decodering:
    - De belangrijkste resolver wordt aangeroepen als de sleutel wordt opgegeven. Als de resolver is opgegeven, maar beschikt niet over een toewijzing voor de sleutel-id, wordt een fout gegenereerd.
    - Als een resolver niet is opgegeven, maar een sleutel is opgegeven, wordt de sleutel wordt gebruikt als de id overeenkomt met de vereiste sleutel-id. Als de id niet overeenkomt, wordt een fout gegenereerd.

      De codering voorbeelden in azure.storage.samples <fix URL>tonen in een meer gedetailleerde end-to-end scenario BLOB's, tabellen en wachtrijen.
        Voorbeeldimplementaties van de KEK en belangrijke resolver worden geleverd in de voorbeeldbestanden als KeyWrapper en KeyResolver respectievelijk.

### <a name="requireencryption-mode"></a>RequireEncryption-modus
Gebruikers kunnen desgewenst inschakelen voor een modus waarbij alle uploads en downloads worden gecodeerd. In deze modus kunnen gegevens zonder een coderingsbeleid uploaden of downloaden van gegevens die niet zijn gecodeerd op de service, niet op de client. De vlag **require_encryption** van het object bepaalt dit gedrag.

### <a name="blob-service-encryption"></a>BLOB service codering
Beleid velden de codering in te stellen op het blockblobservice-object. Al het andere wordt verwerkt door de client-typebibliotheek intern.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Queue service codering
Beleid velden de codering in te stellen op het queueservice-object. Al het andere wordt verwerkt door de client-typebibliotheek intern.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Tabel service-codering
Naast een coderingsbeleid maken en op aanvraag opties instelt, moet u een **encryption_resolver_function** op de **tableservice**opgeven of het coderen kenmerk ingesteld voor de EntityProperty.

### <a name="using-the-resolver"></a>Met behulp van de resolver

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Met behulp van kenmerken
Zoals eerder vermeld, kan een eigenschap worden gemarkeerd voor codering door te slaan in een EntityProperty-object en het veld coderen.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Codering en prestaties
Houd er rekening mee dat de resultaten van opslag in meer belast versleutelt. De inhoud en IV moet worden gegenereerd, de inhoud zelf moet worden gecodeerd en extra metagegevens moet worden geformatteerd en geüpload. Deze belasting is afhankelijk van de hoeveelheid gegevens worden gecodeerd. Het is raadzaam dat klanten altijd hun prestaties tijdens de ontwikkeling van de toepassingen testen.

## <a name="next-steps"></a>Volgende stappen

- De [Azure opslag Client-bibliotheek voor Java-PyPi-pakket](https://pypi.python.org/pypi/azure-storage) downloaden
- De [Client-bibliotheek van Azure opslag voor Python broncode van GitHub](https://github.com/Azure/azure-storage-python) downloaden
