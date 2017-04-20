<properties 
    pageTitle="Azure Services Media concepten | Microsoft Azure" 
    description="Dit onderwerp geeft een overzicht van de concepten van Azure Services" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

#<a name="azure-media-services-concepts"></a>Azure Media Services-begrippen 

Dit onderwerp geeft een overzicht van de belangrijkste concepten van Media Services.

##<a id="assets"></a>Activa en opslag

###<a name="assets"></a>Activa

Een [activum](https://msdn.microsoft.com/library/azure/hh974277.aspx) bevat digitale bestanden (met inbegrip van video, audio, afbeeldingen, miniaturen, verzamelingen, teksttracks en bestanden met ondertiteling) en de metagegevens over deze bestanden. Nadat u de digitale bestanden worden geüpload naar een actief, kan het worden gebruikt in de Media Services, codering en streaming van werkstromen.

Een activum naar een blob-container in de opslag van Azure-account is toegewezen en de bestanden in de activa worden opgeslagen als BLOB's in die container.

Bij het bepalen welke media-inhoud te uploaden en op te slaan in een actief, gelden de volgende overwegingen:

- Een actief moet alleen een unieke sessie van media-inhoud bevatten. Bijvoorbeeld een enkele bewerking van een tv-aflevering, film of reclame.
- Een actief mogen geen meerdere vertoningen of wijzigingen van een audiovisueel bestand. Een voorbeeld van een onjuist gebruik van een actief zou proberen meer dan één tv-aflevering, advertentie of meerdere camerahoeken uit een enkele productie binnen een actief op te slaan. Meerdere vertoningen of wijzigingen van een audiovisueel bestand opslaan in een actief kan resulteren in moeilijkheden ze codering, streamen en beveiligen van de levering van de activa die later in de werkstroom.  

###<a name="asset-file"></a>Activa-bestand 
Een [AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) van vertegenwoordigt een video of audio bestand dat is opgeslagen in een blob-container. Een elementbestand is altijd gekoppeld aan een actief en een actief kan één of meerdere bestanden bevatten. De taak van de Encoder van Media Services mislukt als u een bestand actief object niet is gekoppeld aan een digitaal bestand in een blob-container.

Het exemplaar van de **AssetFile** en de werkelijke mediabestand zijn twee afzonderlijke objecten. De AssetFile instantie bevat metagegevens over het mediabestand terwijl het mediabestand dat de werkelijke media-inhoud bevat.

U moet niet proberen de inhoud van de blob-containers die zijn gegenereerd door de Media Services zonder Media Service API's wijzigen.

###<a name="asset-encryption-options"></a>Coderingsopties activa

Afhankelijk van het type inhoud dat u wilt uploaden, opslaan en leveren van biedt Media Services verschillende opties voor wachtwoordcodering waaruit u kunt kiezen.

**Geen** Geen codering wordt gebruikt. Dit is de standaardwaarde. Er rekening mee dat wanneer u deze optie uw inhoud niet in doorvoer of opslag.

Als u van plan bent om een MP4 met progressieve download, gebruik deze optie voor het uploaden van uw inhoud.

**StorageEncrypted** : Gebruik deze optie voor het coderen van duidelijke inhoud lokaal met AES 256-bits codering en upload deze naar Azure opslag waar het is opgeslagen in rust zijn gecodeerd. Activa met opslag codering beveiligd worden ongecodeerd automatisch geplaatst in een gecodeerde-bestandssysteem voor de codering en eventueel opnieuw gecodeerd voordat u het uploadt terug als een nieuwe uitvoer actief. Het primaire gebruik geval voor opslag codering is als u wilt uw hoge kwaliteit invoermedium bestanden beveiligen met sterke codering in rust op schijf. 

Voor het leveren van een gecodeerde activabeheer voor opslag, moet u beleid voor levering van het actief zodat Media Services weet hoe u wilt uw inhoud te leveren. Voordat uw activa kan worden gestreamd, de server voor mediagegevensstromen verwijdert u de codering voor de opslag en het stroomsgewijs inhoud met het beleid voor levering opgegeven (bijvoorbeeld AES, PlayReady of geen codering). 

**CommonEncryptionProtected** - Gebruik deze optie als u wilt coderen (of uploaden al gecodeerde) inhoud met gemeenschappelijke codering of PlayReady DRM (bijvoorbeeld goede Streaming beveiligd met DRM PlayReady).

**EnvelopeEncryptionProtected** : Gebruik deze optie als u wilt beveiligen (of uploaden al beschermd) HTTP Live Streaming (HLS) gecodeerd met Advanced Encryption Standard (AES). Houd er rekening mee dat als u al gecodeerd met de AES HLS uploaden wilt, het moet zijn gecodeerd door Manager transformeren.

###<a name="access-policy"></a>-Beleid 

Een [AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) definieert machtigingen (zoals lezen, schrijven en lijst) en de duur van de toegang tot een actief. Gewoonlijk zou u een AccessPolicy-object doorgeven aan een locator, die vervolgens worden gebruikt voor toegang tot de bestanden in een actief.


###<a name="blob-container"></a>BLOB-container

Een blob-container kunt u een groepering van een aantal BLOB's. BLOB containers worden gebruikt in de Media Services als grens voor toegangsbeheer en gedeelde toegang handtekening (SAS) locators van de activa. Een account Azure-opslag kan een onbeperkt aantal blob-containers bevatten. Een container kan een onbeperkt aantal BLOB's worden opgeslagen.

>[AZURE.NOTE]U moet niet proberen de inhoud van de blob-containers die zijn gegenereerd door de Media Services zonder Media Service API's wijzigen.

###<a id="locators"></a>Locators

[Locator](https://msdn.microsoft.com/library/azure/hh974308.aspx)s een ingangspunt voor toegang tot de bestanden in een activum opgeven. Een-beleid wordt gebruikt om de machtigingen en de duur dat een client toegang tot een bepaald activum heeft definiëren. Locator kunnen een veel-op-één-relatie met een-beleid hebben dat verschillende locators bieden kunnen verschillende begintijden en verbindingstypen op verschillende clients tijdens alle met dezelfde machtigingen en instellingen; echter, vanwege een beleid voor gedeelde toegangsbeperking ingesteld door Azure opslagservices, er geen meer dan vijf unieke locators die is gekoppeld aan een bepaald activum in één keer. 

Media Services biedt ondersteuning voor twee soorten locators: OnDemandOrigin locators, gebruikt voor het streamen van media (bijvoorbeeld MPEG-streepje, HLS of goede Streaming) of progressief downloaden van media- en SAS-URL-locators gebruikt voor het uploaden of downloaden van media-bestanden to\from Azure opslag. 

Houd er rekening mee dat de lijst machtiging (AccessPermissions.List) niet worden gebruikt bij het maken van een OrDemandOrigin-locator. 

###<a name="storage-account"></a>Opslag account

Alle toegang tot opslag Azure vindt plaats via een account voor opslag. Een Media-account kunt koppelen aan een of meer rekeningen voor opslag. Een account kan een onbeperkt aantal containers, bevatten, zolang hun totale grootte onder 500TB per account opslag.  Media Services biedt niveau tooling SDK als u wilt meerdere opslag beheren en verdelen van de verdeling van de activa tijdens laden naar deze rekeningen op basis van maatstaven of willekeurige distributie. Zie Werken met [Azure opslag](https://msdn.microsoft.com/library/azure/dn767951.aspx)voor meer informatie. 

##<a name="jobs-and-tasks"></a>Werkzaamheden en taken

Een [taak](https://msdn.microsoft.com/library/azure/hh974289.aspx) wordt meestal gebruikt om het proces (bijvoorbeeld index of coderen) één audio-video-presentatie. Als u meerdere video's verwerkt, een taak maken voor elke video te coderen.

Een taak bevat metagegevens over de verwerking moet worden uitgevoerd. Elk project bevat een of meer [taken](https://msdn.microsoft.com/library/azure/hh974286.aspx)s die een atomaire verwerkingstaak haar activa invoer, uitvoer van activa, een Mediaprocessor en de bijbehorende instellingen opgeven. Taken binnen een project kunnen worden geschakeld, wanneer de activa van de uitvoer van de ene taak naar de volgende taak is gegeven als de input. Op deze manier bevatten één taak alle de verwerking noodzakelijk is voor een media-presentatie.

##<a id="encoding"></a>Codering

Azure Media Services biedt meerdere opties voor de codering van de media in de cloud.

Bij het starten van Media Services, is het belangrijk dat u begrijpt het verschil tussen de codecs en bestandsindelingen.
Codecs zijn de software waarmee de compressie/decompressie algoritmen worden geïmplementeerd dat bestandsindelingen zijn containers waarin de gecomprimeerde video.

Dynamische verpakking kunt u voor het leveren van de inhoud van uw adaptieve bitsnelheid MP4 of goede Streaming gecodeerd in indelingen die worden ondersteund door Media Services (MPEG-streepje, HLS, soepele Streaming, harde schijven) streaming biedt Media Services zonder dat u hoeft pakket opnieuw opslaan in deze indelingen streamen.

Als u wilt profiteren van [dynamische verpakking](media-services-dynamic-packaging-overview.md), moet u het volgende doen:

- Het bestand met mezzanine (bron) in een reeks geavanceerde bitsnelheid MP4-bestanden of adaptieve bitsnelheid goede Streaming-bestanden (de codering stappen worden verderop in deze zelfstudie aangetoond) te coderen.
- Ten minste één On-Demand streaming eenheid voor het streaming eindpunt waaruit u leveren uw inhoud wilt ophalen. Voor meer informatie Zie [On-Demand Streaming gereserveerde schaaleenheden](media-services-portal-manage-streaming-endpoints.md).

De volgende ondersteunt Media Services op vraag encoders die in dit artikel worden beschreven:

- [Media Encoder-standaard](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Zie voor meer informatie over ondersteunde encoders [Encoders](media-services-encode-asset.md).


##<a name="live-streaming"></a>Live Streaming

Een kanaal staat in Azure Media Services, een pijpleiding voor het verwerken van live streaming inhoud. Een kanaal wordt live gegevensstromen voor invoer op twee manieren:

- Een live encoder op ruimten verzendt multi-bitrate RTMP of goede Streaming (gefragmenteerd MP4) aan het kanaal. U kunt de volgende live encoders die uitvoer met goede Streaming van multi-bitrate: vorm van het element, Envivio, Cisco. De volgende live encoders uitvoer RTMP: Adobe Flash Live Telestream Wirecast en Tricaster transcoders. De geconsumeerde streams doorgeven via kanalen zonder verdere verwerking. Bij een aanvraag voor levert Media Services de stroom aan klanten.

- Een enkele bitsnelheid (in een van de volgende indelingen: RTP (MPEG-TS)), RTMP of goede Streaming (gefragmenteerd MP4)) wordt verzonden naar het kanaal dat is ingeschakeld voor live-codering met Media-Services. Het kanaal voert live-codering van de binnenkomende één bitsnelheid op een multi-(adaptief) video bitsnelheid. Bij een aanvraag voor levert Media Services de stroom aan klanten.

###<a name="channel"></a>Kanaal

[Kanaal](https://msdn.microsoft.com/library/azure/dn783458.aspx)s zijn in Media Services, die verantwoordelijk is voor het verwerken van live streaming inhoud. Een kanaal biedt een eindpunt van de invoer (consumptie van URL) dat u vervolgens naar een live transcoder bieden. Het kanaal live gegevensstromen voor invoer ontvangt van de levende transcoder en beschikbaar gemaakt voor streaming via een of meer StreamingEndpoints. Kanalen bieden ook een eindpunt preview (voorbeeld URL) die u kunt bekijken en uw stream valideren voordat verdere verwerking en levering.

U krijgt de URL ingest en de voorbeeld-URL bij het maken van het kanaal. Als u deze URL's, moet het kanaal niet worden gestart. Wanneer u klaar bent om gegevens van een levende transcoder duwen in het kanaal, moet het kanaal worden gestart. Zodra de live transcoder ingesting van gegevens is gestart, kunt u uw stream kunt bekijken.

Elke account Media Services kan bevatten meerdere kanalen, meerdere programma's en meerdere StreamingEndpoints. Afhankelijk van de bandbreedte- en behoeften, kunnen StreamingEndpoint services worden toegewezen aan een of meer kanalen. Een StreamingEndpoint kunt trekken van een kanaal.


###<a name="program"></a>Programma

Een [programma](https://msdn.microsoft.com/library/azure/dn783463.aspx) kunt u de publicatie en de opslag van segmenten in een live gegevensstroom. Kanalen beheren van programma's. De kanaal- en relatie is vergelijkbaar met de traditionele media, waarbij een kanaal is een constante stroom van inhoud en een programma is binnen het bereik van sommige getimede gebeurtenis op dat kanaal.
Kunt u het aantal uren dat u bewaren van de opgenomen inhoud van het programma wilt met de eigenschap **ArchiveWindowLength** . Deze waarde kan worden ingesteld van een minimum van 5 minuten tot een maximum van 25 uur.

ArchiveWindowLength bepaalt ook dat de maximale hoeveelheid tijd clients kunt zoeken in de tijd vanaf de huidige positie van de levende. Programma's kunnen worden uitgevoerd via de opgegeven hoeveelheid tijd, maar de inhoud achter de lengte van het venster wordt voortdurend genegeerd. Deze waarde van deze eigenschap bepaalt ook hoe lang de manifesten client te vergroten.

Elk programma dat is gekoppeld aan een actief. U moet een locator voor de gekoppelde activa maken voor het publiceren van het programma. Met deze locator kunt u een streaming URL die u aan uw klanten bieden kunt maken.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u meerdere archieven van dezelfde binnenkomende gegevensstroom kunt maken. Hiermee kunt u publiceren en archiveren van verschillende delen van een gebeurtenis als nodig is. Bijvoorbeeld, is uw behoeften voor het archiveren van 6 uur van een programma, maar alleen de laatste 10 minuten wordt uitgezonden. Om dit te bereiken, moet u twee gelijktijdig actieve programma's te maken. Een programma voor het archiveren van 6 uur van de gebeurtenis is ingesteld, maar wordt niet gepubliceerd. Het andere programma is ingesteld voor het archiveren van 10 minuten en dit programma wordt gepubliceerd.


Zie voor meer informatie:

- [Werken met de kanalen die voor het uitvoeren van Live-codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)
- [Werken met de kanalen die Multi-bitrate Live gegevensstroom van On-premises Encoders ontvangen](media-services-live-streaming-with-onprem-encoders.md)
- [Quota's en beperkingen](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Inhoud beveiligen

###<a name="dynamic-encryption"></a>Dynamische codering

Azure Media Services kunt u veilig uw media vanaf het moment dat het verlaten van uw computer via de opslag, verwerking en levering. Media-Services kunt u ervoor zorgen dat uw inhoud dynamisch gecodeerd met Advanced Encryption Standard (AES) (met 128-bits coderingssleutels) en gemeenschappelijke codering (CENC) met PlayReady en/of Widevine DRM. Media Services biedt ook een service voor AES-sleutels en PlayReady licenties aan clients worden geleverd.

Op dit moment kunt u de volgende indelingen voor streaming coderen: HLS MPEG-streepje en soepele Streaming. U kunt geen schijven streaming format coderen of progressief gedownload.

Als u Media-Services voor het coderen van een actief, moet u een coderingssleutel (CommonEncryption of EnvelopeEncryption) te koppelen aan het activum en ook autorisatiebeleid voor de sleutel te configureren.

Als u wilt voor het stroomsgewijs verzenden van een gecodeerde activabeheer voor opslag, moet u beleid voor levering van het vaste activum configureren om op te geven hoe u wilt ervoor zorgen dat uw activa.

Wanneer een gegevensstroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het coderen van inhoud met een envelop-codering (met AES) of een gemeenschappelijke codering (met PlayReady of Widevine) dynamisch. Om te decoderen van de gegevensstroom, de speler vraagt om de sleutel van de belangrijkste leveringsservice. De service geëvalueerd om te bepalen of de gebruiker is gemachtigd om de sleutel, het autorisatiebeleid dat u hebt opgegeven voor de sleutel.


###<a name="token-restriction"></a>Token-beperking

De inhoud sleutel autorisatiebeleid kan zijn voor een of meer vergunning beperkingen: open, beperking of restrictie voor IP-token. Het token beperkte beleid moet vergezeld gaan van een token dat wordt afgegeven door een beveiligde Token Service (STS). Media Services ondersteunt tokens in de eenvoudige Web Tokens (SWT) en JSON Web Token (JWT)-indeling. Media Services biedt geen Token Services beveiligen. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS op tokens probleem. De STS moeten een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de token beperking worden geconfigureerd. De belangrijkste bezorgingsservice Media Services retourneert het aangevraagde sleutel (of licentie) naar de client als de token geldig is en de vorderingen in het token overeenkomen met die welke worden geconfigureerd voor de sleutel (of licentie).

Bij het configureren van het token beleid worden beperkt, moet u de verificatie van de primaire sleutel, uitgever en parameters van de doelgroep. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met, uitgever is de veilige token service het token verleent. De doelgroep (soms scope genoemd) beschrijft de intentie van het token of de resource de token gemachtigd voor toegang tot. De levering van belangrijke Media Services-service wordt gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

Raadpleeg de volgende artikelen voor meer informatie:

[Overzicht van de inhoud te beschermen](media-services-content-protection-overview.md)
[beveiligen met AES-128](media-services-protect-with-aes128.md)
[beveiligen met DRM](media-services-protect-with-drm.md)

##<a name="delivering"></a>Bezorgen

###<a id="dynamic_packaging"></a>Dynamische verpakking

Werken met Media Services wordt aanbevolen om de mezzanine-bestanden coderen in een adaptieve bitsnelheid MP4 instellen als de set vervolgens converteren naar de gewenste indeling met behulp van de [Dynamische verpakking](media-services-dynamic-packaging-overview.md).


###<a name="streaming-endpoint"></a>Streaming eindpunt

Een StreamingEndpoint vertegenwoordigt een streaming service die inhoud leveren rechtstreeks naar een clienttoepassing player, of met een inhoud Delivery Network (CDN) voor verdere distributie (Azure Media Services biedt nu de integratie van de Azure CDN.) De uitgaande stream van een StreamingEndpoint-service is een live gegevensstroom of een video op aanvraag activa in uw Media Services-account. Bovendien kunt u de capaciteit van de StreamingEndpoint-service voor het verwerken van de groeiende vraag naar bandbreedte door het aanpassen van de schaaleenheden (ook bekend als streaming) bepalen. Het verdient aanbeveling een of meer schaaleenheden toewijzen voor toepassingen in de productie-omgeving. Schaaleenheden bieden u zowel egress toegewezen capaciteit die kan worden gekocht in stappen van 200 Mbps en extra functionaliteit die momenteel gebruik dynamische verpakking omvat.

Het verdient aanbeveling om dynamische verpakking en/of dynamische codering te gebruiken. Als u deze functies gebruikt, hebt u ten minste één streaming eenheid voor het eindpunt van waaruit u wilt streamen. Zie [schalen streaming-eenheden](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Standaard kunt u maximaal 2 streaming eindpunten in de Media-Services-account hebben. Zie [quota's en beperkingen](media-services-quotas-and-limitations.md)voor het aanvragen van een hogere limiet.

U worden alleen in rekening gebracht wanneer uw StreamingEndpoint in rijklare toestand is.

###<a name="asset-delivery-policy"></a>Beleid voor levering van activa

Een van de stappen in de workflow van Media Services leveren van inhoud is [levering beleid voor activa ](https://msdn.microsoft.com/library/azure/dn799055.aspx)die u wilt worden gestreamd configureren. De levering actief beleid Media Services vertelt hoe u het activum wilt worden geleverd: in welke streaming protocol moet uw activa worden dynamisch verpakt (voor bijvoorbeeld MPEG-streepje, HLS, soepele Streaming, of alle), al dan niet u wilt coderen dynamisch uw activa en hoe (envelop of gemeenschappelijke codering).

Hebt u een gecodeerde activabeheer voor opslag, voordat u uw activa kan worden gestreamd, kan de streaming server verwijdert de codering voor opslag en biedt uw inhoud met behulp van het beleid voor levering opgegeven. Bijvoorbeeld, als u wilt ervoor zorgen dat de activa die zijn gecodeerd met een coderingssleutel Advanced Encryption Standard (AES), het beleidstype ingesteld op DynamicEnvelopeEncryption. Opslag-codering verwijderen en het actief in de stream, stelt u het type op NoDynamicEncryption.

###<a name="progressive-download"></a>Progressief downloaden

Progressief downloaden kunt u media afspelen voordat het hele bestand is gedownload. U kunt alleen geleidelijk een MP4-bestand downloaden.

Houd er rekening mee dat u gecodeerde activa decoderen moet als u wilt dat ze beschikbaar zijn voor progressief downloaden.

Om gebruikers met progressieve download-URL's, moet u eerst een locator OnDemandOrigin maken. De locator maakt, kunt u het basispad naar het activum. Vervolgens moet u de naam van de MP4-bestand toe te voegen. Bijvoorbeeld:

http://amstest1.streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###<a name="streaming-urls"></a>Streaming URL 's

Streaming van uw inhoud aan clients. Om gebruikers met streaming URL's, moet u eerst een locator OnDemandOrigin maken. De locator maakt, kunt u het basispad naar het activum met de inhoud die u stroomsgewijs wilt. Om te kunnen streamen van deze inhoud moet u echter verder dit pad wijzigen. Als u wilt een volledige URL aan het manifestbestand streaming samenstellen, samenvoegen padwaarde van de locator en het manifest (filename.ism) de naam van bestand. /Manifest en (indien nodig) de juiste indeling vervolgens toevoegen aan het pad locator.

U kunt ook uw inhoud stroomsgewijs via een SSL-verbinding. Hiervoor moet dat uw streaming URL's beginnen met HTTPS.

Houd er rekening mee dat u alleen via SSL streamen kunt als het streaming eindpunt van waaruit u de inhoud bezorgen na 10 September 2014 is gemaakt. Als uw streaming URL's zijn gebaseerd op de streaming eindpunten die na 10 September zijn gemaakt, bevat de URL 'streaming.mediaservices.windows.net' (nieuwe indeling). Streaming URL's met 'origin.mediaservices.windows.net' (oude indeling) bieden geen ondersteuning voor SSL. Als uw URL in de oude indeling is en u wilt streamen via SSL, maakt u een nieuwe streaming eindpunt. Gemaakt op basis van het nieuwe endpoint streaming URL's gebruiken voor het stroomsgewijs verzenden van inhoud via SSL.

De volgende lijst beschrijft verschillende streaming-indelingen, en geeft voorbeelden:

- Goede Streaming

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest


- MPEG-STREEPJE

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-time-csf)



- Apple HTTP Live Streaming (HLS) V4

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)



- Apple HTTP Live Streaming (HLS) V3

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

- Harde schijven (voor Adobe WIP/Access licentiehouders)

{streaming eindpunt naam media services rekening name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=f4m-F4F)


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
