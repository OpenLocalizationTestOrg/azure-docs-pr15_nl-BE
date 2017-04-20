<properties 
    pageTitle="Azure Media Services gefragmenteerde MP4 live nemen specificatie | Microsoft Azure" 
    description="In deze specificatie wordt het protocol en de indeling voor live streaming ingestie gefragmenteerd MP4 gebaseerd voor Microsoft Azure Media Services beschreven. Microsoft Azure Media Services biedt live streaming-service waarmee klanten om te streamen live gebeurtenissen en inhoud in real-time uitzenden met Microsoft Azure het cloud platform. Dit document is ook aandacht voor beste praktijken bij het bouwen van krachtige en uiterst redundante live nemen mechanismen." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services gefragmenteerde MP4 live nemen specificatie

In deze specificatie wordt het protocol en de indeling voor live streaming ingestie gefragmenteerd MP4 gebaseerd voor Microsoft Azure Media Services beschreven. Microsoft Azure Media Services biedt live streaming-service waarmee klanten om te streamen live gebeurtenissen en inhoud in real-time uitzenden met Microsoft Azure het cloud platform. Dit document is ook aandacht voor beste praktijken bij het bouwen van krachtige en uiterst redundante live nemen mechanismen.


##<a name="1-conformance-notation"></a>1. conformiteit-notatie

De trefwoorden 'moet', zijn "Moet niet", 'REQUIRED' 'Zenden', 'Wordt niet', 'SHOULD', "Moet niet", "Aangeraden", "Kunnen" en "Optioneel" in dit document zoals beschreven in RFC 2119 worden geïnterpreteerd.

##<a name="2-service-diagram"></a>2. Service-Diagram 

Het onderstaande diagram ziet u de architectuur van de live streaming service van hoog niveau in Microsoft Azure Media Services:

1.  Live Encoder duwt live-feeds in de kanalen die zijn gemaakt en ingericht via Microsoft Azure Media Services SDK.
2.  Kanalen, programma's en Streaming eindpunt in Microsoft Azure Media Services greep alle live streaming functies ingest, opmaak, met inbegrip van de wolk DVR, beveiliging, schaalbaarheid en redundantie.
3.  Klanten kunnen eventueel ook een CDN laag tussen het eindpunt van de Streaming en de eindpunten van de client te implementeren.
4.  Client eindpunten stroom van de Streaming-eindpunt met adaptieve Streaming HTTP-protocollen (bijvoorbeeld goede Streaming, STREEPJES, harde schijven of HLS).

![image1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. bit-stream – ISO 14496-12 gefragmenteerd MP4-indeling

De telegramindeling is voor live streaming nemen die wordt beschreven in dit document gebaseerd op [ISO-14496-12]. Verwijzen naar [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) voor een gedetailleerde uitleg van de indeling MP4 gefragmenteerd en extensies voor beide bestanden video-on-demand en live streaming via ingestie.

###<a name="live-ingest-format-definitions"></a>Definities van de indeling Live nemen 

Hieronder vindt u een lijst met speciale definities van toepassing op live in Microsoft Azure Media Services nemen-indeling:

1. De 'ftyp ' legt vast, LiveServerManifestBox en box 'moov' moeten elke aanvraag (POST HTTP) worden verzonden.  Het moet worden verzonden aan het begin van de stream en altijd en overal het coderingsprogramma moet opnieuw verbinding maken als u wilt doorgaan met consumptie van stroom.  Zie afdeling 6 in [1] voor meer informatie.
2. Punt 3.3.2 in [1] definieert een optioneel vak StreamManifestBox voor live nemen. Als gevolg van het bewerkingsplan logica van taakverdeling van Microsoft Azure, gebruik van dit vak is afgeschaft en moet niet aanwezig bij het ingesting in Microsoft Azure Media Service. Als deze aanwezig is, Azure Media Services zonder deze genegeerd.
3. De TrackFragmentExtendedHeaderBox gedefinieerd in 3.2.3.2 in [1] moet aanwezig zijn om elk fragment.
4. Versie 2 van de TrackFragmentExtendedHeaderBox moet worden gebruikt om mediasegmenten met identieke URL's genereren in meerdere datacenters. Het veld van de index fragment is vereist voor cross-datacenter failover van streaming op basis van index wordt opgemaakt zoals Apple HTTP Live Streaming (HLS) en MPEG-streepje op basis van een index.  Zodat cross-datacenter failover moet de index van het fragment worden gesynchroniseerd op meerdere encoders en verhoging voor elk fragment opeenvolgende media met 1 zelfs op encoder opnieuw wordt opgestart of fouten.
5. Sectie 3.3.6 in [1] bepaalt het zogenaamde MovieFragmentRandomAccessBox (mfra) om aan te geven (einde van Stream) EOS het kanaal aan het einde van een live opname kan worden verzonden. Als gevolg van de ingest logica van Azure Media Services, gebruik van EOS (einde van Stream) is afgeschaft en het 'mfra' voor live opname niet moet worden verzonden. Als verzonden, Azure Media Services zonder waarschuwing genegeerd. Het [Kanaal opnieuw instellen](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) met de status van het punt ingest opnieuw instellen wordt aanbevolen en wordt ook aanbevolen te gebruiken [Programma stoppen](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) tot het einde van een presentatie en stroom.
6. De duur van de MP4-fragment moet constant, om het verkleinen van de manifesten client en de methodiek van de client downloaden door middel van herhaalde labels te verbeteren.  De duur kan variëren ter compensatie van het niet-integere framesnelheden.
7. De duur van MP4 fragment moet liggen tussen ongeveer 2 tot 6 seconden.
8. MP4 fragment tijdstempels en indexen (tijd van TrackFragmentExtendedHeaderBox fragment_ absolute_ en fragment_index) moeten in oplopende volgorde binnenkomen.  Azure Media Services is een robuuste op dubbele fragmenten, maar heeft zeer beperkte mogelijkheden voor het rangschikken fragmenten op basis van de tijdlijn van de media.

##<a name="4-protocol-format--http"></a>4. protocol – HTTP-indeling

Consumptie van ISO gefragmenteerd MP4 gebaseerd live for Microsoft Azure Media Services wordt een standaard HTTP POST met een lange uitvoeringstijd voor het verzenden van gecodeerde mediagegevens verpakt in gefragmenteerde MP4-indeling naar de service aanvragen. Elk HTTP POST verzendt een volledig gefragmenteerd MP4 bitstroom ('Stream') vanaf beginnen met vakken koptekst (box 'ftyp legt vast', 'Live Server Manifest Box' en 'moov') en door te gaan met een reeks fragmenten ('moof' en 'mdat' vakken). Zie punt 9.2 in [1] voor de URL-syntaxis voor HTTP POST-aanvraag. Een voorbeeld van de POST-URL is: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Vereisten

Hier vindt u de gedetailleerde vereisten:

1. Het coderingsprogramma moet de uitzending start via een HTTP POST-aanvraag met een lege "lichaam" (inhoud lengte nul) met dezelfde URL inslikken. Dit kan helpen snel detecteren als het eindpunt van de live-opname geldig is en er wordt een verificatie- of andere voorwaarden. Per HTTP-protocol de server niet mogelijk back HTTP-antwoord verzenden totdat de volledige aanvraag met inbegrip van de POST-hoofdtekst is ontvangen. Gezien de langdurige aard van live-gebeurtenis zonder deze stap het coderingsprogramma niet mogelijk een fout detecteert totdat het klaar is met het verzenden van alle gegevens.
2. Het coderingsprogramma moet omgaan met eventuele fouten of verificatie uitdagingen als gevolg van (1). Als (1) is uitgevoerd met een 200 antwoord gaan.
3. Het coderingsprogramma moet een HTTP POST-aanvraag nieuwe beginnen met gefragmenteerde MP4-stream.  De payload moet beginnen met de koptekst vakken gevolgd door fragmenten.  Opmerking het vak 'ftyp legt vast', 'Live Server Manifest Box' en 'moov' (in deze volgorde) moet worden verzonden bij elke aanvraag, zelfs als het coderingsprogramma opnieuw verbinding maken moet omdat de vorige aanvraag vóór het einde van de stroom is beëindigd. 
4. Encoder moet gedeelde Transfer codering gebruiken voor het uploaden van omdat het onmogelijk is te voorspellen de hele lengte van de inhoud van de live-gebeurtenis.
5. Als de gebeurtenis op, na het verzenden van het laatste fragment, moet het coderingsprogramma de berichtenstroom gedeelde Transfer codering (de meeste HTTP-client stapels verwerkt automatisch) netjes eindigen. Het coderingsprogramma moet wachten tot de service de code definitief antwoord retourneren en vervolgens de verbinding verbreken. 
6. De Events() zelfstandig naamwoord coderingsprogramma moet niet gebruiken zoals beschreven in 9.2 in [1] voor live opname in Microsoft Azure Media Services.
7. Als de HTTP POST-aanvraag wordt beëindigd of vóór het einde van de stream met een TCP-fout, time-out, moet het coderingsprogramma uitgeven met een nieuwe verbinding een nieuwe POST-aanvraag en voldoen aan de vereisten boven aan de aanvullende eis dat het coderingsprogramma moet opnieuw de vorige twee MP4 fragmenten voor elke track in de stroom en hervatten zonder de invoering van wijzigingen in de tijdlijn van de media.  De laatste twee fragmenten MP4 voor elk nummer opnieuw zorgt ervoor dat er geen gegevens verloren gaan.  Met andere woorden, als een stream een track audio en video bevat, en de huidige POST-aanvraag mislukt, moet het coderingsprogramma herstellen en opnieuw de laatste twee fragmenten voor het geluidsspoor eerder met succes zijn verzonden, en de laatste twee fragmenten voor de videotrack die eerder met succes zijn verzonden, om ervoor te zorgen dat er geen gegevens verloren gaan.  Het coderingsprogramma moet ervoor zorgen dat een "forward" buffer van media fragmenten die het opnieuw wanneer u opnieuw verbinding te maken.

##<a name="5-timescale"></a>5. de tijdschaal 

[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) beschrijving van het gebruik van "Tijdschaal" van SmoothStreamingMedia (Zie punt 2.2.2.1), StreamElement (sectie 2.2.2.3), StreamFragmentElement(2.2.2.6) en LiveSMIL (punt 2.2.7.3.1). Als de waarde van de tijdschaal niet aanwezig is, wordt de standaardwaarde gebruikt 10,000,000 (10 MHz). Hoewel goede Streaming Format specificatie niet blokkeren gebruik van andere waarden van de tijdschaal, de meeste van de encoder-implementaties wordt deze standaardwaarde (10 MHz) voor het genereren van nemen goede Streaming gegevens. Door [Azure Media dynamische](media-services-dynamic-packaging-overview.md) verpakkingsfunctie is het aanbevolen gebruik 90 kHz tijdschaal voor video-streams en 44,1 of 48.1 kHz voor audiostreams. Als de tijdschaal van verschillende waarden worden gebruikt voor de verschil-lende stromingen, moet tijdschaal stream niveau worden verzonden. Zie [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. de definitie van "Stream"  

"Stream" is de basiseenheid van de bewerking in de live-opname voor live presentatie opstellen, de verwerking van streaming failover en redundantie-scenario's. "Stream" wordt gedefinieerd als een unieke gefragmenteerd MP4 bitstroom waarin één track of meerdere nummers. Een volledige live presentatie kan een of meer inhoudstromen afhankelijk van de configuratie van de levende encoder(s) bevatten. De onderstaande voorbeelden illustreren de verschillende opties van het gebruik van stream(s) voor het opstellen van een volledige live presentatie.

**Voorbeeld:** 

Klant wil een live streaming-presentatie met de volgende audio-en video-bitsnelheid te maken:

Video – 3000 k, 1500kbps, 750 k

Audio – 128kbps

###<a name="option-1-all-tracks-in-one-stream"></a>Optie 1: Alle tracks in één stroom

Bij deze optie een enkele encoder genereert alle audio-en video-tracks en ze bundelen in een gefragmenteerd MP4 bit-stream die vervolgens wordt verzonden via een HTTP POST-verbinding. In dit voorbeeld is er slechts één stream voor deze live presentatie:

![image2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Optie 2: Elke track in een aparte stream

Bij deze optie de plaats alleen een encoder(s) bijhouden in elk Fragment MP4 bitstroom en alle gegevensstromen boeken via meerdere afzonderlijke HTTP-verbindingen. Dit kan worden gedaan met encoders voor één of meerdere encoders. Uit oogpunt van live opname, deze live presentatie bestaat uit vier stromen.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Optie 3: Audio track met de laagste bitsnelheid videotrack bundelen in één stream

Bij deze optie wordt de klant wil de audio track met de laagste bitsnelheid videotrack in één Fragment MP4 bitstroom bundelen en laat de andere twee videotracks elk een eigen stream wordt. 


![image4][image4]


###<a name="summary"></a>Samenvatting

Wat hierboven is geen uitputtende lijst van de mogelijke opname-opties voor dit voorbeeld. Als een sterker nog, wordt elke groepering van nummers in streams ondersteund door het live innemen. Klanten en leveranciers encoder kunnen kiezen hun eigen implementaties op basis van technische complexiteit, encoder-capaciteit en redundantie en failover-overwegingen. Wel moet worden opgemerkt dat in de meeste gevallen slechts één audiospoor voor de hele presentatie live is dus het is belangrijk om ervoor te zorgen de gezondheid van de ingest-stroom met de audio track. Dit motief vaak resulteert in de audiotrack in de eigen stream (zoals in optie 2) te plaatsen of deze bundeling met de laagste bitsnelheid videotrack (zoals in optie 3). Ook voor betere redundantie en fouttolerantie nemen verzenden op de dezelfde audiotrack in twee verschillende streams (optie 2 met redundante audiotracks) of de audio track ten minste twee van de laagste bitsnelheid video sporen (optie 3 met gebundeld in ten minste twee video-streams audio) wordt aanbevolen voor live bundeling in Microsoft Azure Media Services.

##<a name="7-service-failover"></a>7. Service Failover 

Gezien de aard van live streaming is goede failover-ondersteuning van cruciaal belang voor het waarborgen van de beschikbaarheid van de service. Microsoft Azure Media Services is ontworpen voor het verwerken van verschillende soorten fouten, met inbegrip van netwerkfouten, server-fouten, problemen met opslag, enz. Wanneer gebruikt in combinatie met de juiste failover-logica van de kant van de live-encoder, ontstaan klant een uiterst betrouwbare live streaming service vanuit de cloud.

In dit gedeelte bespreken we service failover-scenario's. In dit geval de fout gebeurt er ergens binnen de service en doet zich voor als een netwerkfout. Hier volgen enkele aanbevelingen voor de implementatie van het coderingsprogramma voor het afhandelen van failover van de service:


1. Gebruik een tweede 10 time-out voor de TCP-verbinding tot stand te brengen.  Als een poging om de verbinding tot stand brengen langer dan 10 seconden duurt, wordt de bewerking afbreken en probeer het opnieuw. 
2. Gebruik een korte time-out voor het HTTP-verzoek verzenden bericht delen.  Als het doel MP4 fragment duur N seconden, gebruikt u een time-out voor verzenden tussen N en 2N seconden; Gebruik bijvoorbeeld een time-out van 6 tot en met 12 seconden als de duur van de MP4-fragment 6 seconden is.  Als er een time-out optreedt, de verbinding opnieuw ingesteld, een nieuwe verbinding geopend en stream hervatten nemen op de nieuwe verbinding. 
3. Een rolling buffer met de laatste twee fragmenten, voor elk nummer, dat correct en volledig om de service te verzonden is onderhouden.  Als de HTTP POST-aanvraag voor een gegevensstroom wordt beëindigd of time-out voorafgaand aan het einde van de stream, een nieuwe verbinding geopend en beginnen met een ander HTTP POST-aanvraag, opnieuw de kop van de stroom opnieuw de laatste twee fragmenten voor elk spoor en hervatten van de stroom zonder een discontinuity in de tijdlijn van de media.  Hierdoor wordt de kans op verlies van gegevens.
4. Het is raadzaam het coderingsprogramma wordt niet beperkt het aantal nieuwe pogingen om een verbinding tot stand brengen of hervatten streaming nadat een TCP-fout is opgetreden.
5. Na een TCP-fout:
    1. De huidige verbinding moet worden gesloten en een nieuwe verbinding moet worden gemaakt voor een nieuwe HTTP POST-aanvraag.
    2. De nieuwe HTTP-POST URL moet hetzelfde zijn als de oorspronkelijke URL van de POST.
    3. De nieuwe HTTP-POST moet stroom koptekst bevatten (box 'ftyp legt vast', 'Live Server Manifest Box' en 'moov') identiek aan de kop van de stroom in het oorspronkelijke bericht.
    4. De laatste twee fragmenten verzonden voor elk spoor moeten opnieuw worden verzonden, en zonder een discontinuity in de tijdlijn media streaming hervat.  De tijdstempels MP4 fragment oplopen, zelfs via een HTTP POST-verzoeken.
6. De HTTP POST-aanvraag het coderingsprogramma moet worden beëindigd als gegevens niet tegen een tarief evenredig met de duur van de MP4-fragment wordt verzonden.  Een HTTP POST-verzoek dat geen gegevens verzendt kunt voorkomen dat Azure Media Services snel loskoppelen van de encoder in geval van een update service.  Om deze reden de HTTP POST voor sparse tracks (ad signaal) moeten worden kort geleefd, wordt beëindigd zodra de verspreide fragment wordt verzonden.

##<a name="8-encoder-failover"></a>8. Failover van coderingsprogramma

Failover van coderingsprogramma is het tweede type van failover-scenario dat moet worden opgelost voor end-to-end live streaming levering. In dit scenario wordt de fout aan de kant van de encoder gebeurd. 

![image5][image5]


Hieronder zijn de verwachtingen van het eindpunt van de live-opname als failover van coderingsprogramma gebeurt:

1. Een nieuw exemplaar van het coderingsprogramma moet worden gemaakt om door te gaan met streaming, zoals in het bovenstaande diagram (Stream voor 3000 k video met onderbroken lijn).
2. De nieuwe encoder moet dezelfde URL gebruiken voor HTTP POST-aanvragen als de sessie is mislukt.
3. De nieuwe encoder POST-aanvraag moet het dezelfde gefragmenteerde MP4 vakken van de berichtkop als het exemplaar opnemen.
4. De nieuwe encoder moet correct worden gesynchroniseerd met alle andere actieve encoders voor een live presentatie voor het genereren van gesynchroniseerde audio-video monsters met uitgelijnde fragment grenzen.
5. De nieuwe stream moeten semantisch gelijkwaardig zijn met de vorige stroom en uitwisselbaar op niveau van de kop en het fragment.
6. De nieuwe encoder moet proberen om gegevensverlies te minimaliseren.  De fragment_absolute_time en de fragment_index van media fragmenten moeten verhogen vanaf het punt waar het coderingsprogramma laatste keer was gestopt.  De fragment_absolute_time en fragment_index op een continue wijze moeten verhogen, maar het is toegestaan in te voeren een discontinuity indien nodig.  Azure Media Services negeert fragmenten die al heeft ontvangen en verwerkt, dus het is beter om aan de zijkant van opnieuw verzenden van fragmenten dan te introduceren wijzigingen in de tijdlijn media err. 

##<a name="9-encoder-redundancy"></a>9. redundantie encoder 

Voor bepaalde essentiële live gebeurtenissen vraag nog hogere beschikbaarheid en kwaliteit van de ervaring, het verdient aanbeveling om actieve redundante encoders voor een naadloze failover zonder verlies van gegevens.

![image6][image6]

Zoals u in het diagram hierboven, zijn er twee groep encoders duwen twee kopieën van elke gegevensstroom tegelijk in de live-service. Deze instelling wordt ondersteund, omdat Microsoft Azure Media Services de mogelijkheid is voor het filteren van dubbele fragmenten op basis van gegevensstroom fragment-ID en het tijdstempel. De resulterende live gegevensstroom en archiveren is één van de kopie van alle stromen die niet de best mogelijke samenvoeging van de twee bronnen. Bijvoorbeeld in een hypothetische extreme geval, mits er een coderingsprogramma (hoeft niet hetzelfde te zijn) wordt uitgevoerd op elk gewenst moment in de tijd van elke gegevensstroom, de resulterende live gegevensstroom van de service worden doorlopend zonder verlies van gegevens. 

De vereiste voor dit scenario is bijna hetzelfde als de eisen in geval van Failover van coderingsprogramma met de uitzondering die de tweede set encoders op hetzelfde moment als de primaire encoders worden uitgevoerd.

##<a name="10-service-redundancy"></a>10. Service redundantie  

Voor zeer redundante wereldwijde distributie, is soms verplicht cross regio back-up voor het verwerken van regionale rampen hebben. Op de topologie "Encoder redundantie" uit te breiden, kunnen hebben een redundante service implementeren in een andere regio die is verbonden met de 2e set encoders klanten kiezen. Klanten kunnen ook werken met een CDN provider voor de implementatie van een GTM (globale verkeer Manager) voor de service twee implementaties voor het routeren van verkeer naadloos. De eisen van de encoders zijn hetzelfde als "Encoder redundantie" geval met de enige uitzondering die de tweede set encoders moet worden verwezen naar een andere live eindpunt nemen. Het onderstaande diagram ziet u deze instellingen:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. speciale typen opname-indelingen 

In deze sectie wordt een speciaal soort live opname-indelingen die zijn ontworpen om aan bepaalde specifieke scenario's beschreven.

###<a name="sparse-track"></a>Verspreide bijhouden

Wanneer u een presentatie live streaming met rijke ervaring, is het vaak nodig voor het verzenden van gebeurtenissen tijd gesynchroniseerd of signalen in-band met de belangrijkste mediagegevens. Een voorbeeld hiervan is een dynamische live advertenties invoegen. Dit type gebeurtenis signalering verschilt van gewone audio-en video streaming wegens zijn aard verspreid. Met andere woorden, de signalering van gegevens meestal niet het geval voortdurend en het interval kan het moeilijk zijn te voorspellen. Het concept van verspreide Track is speciaal ontworpen voor consumptie en signalering van gegevens in-band uitzenden.

Hieronder vindt u een aanbevolen implementatie voor ingesting verspreide bijhouden:

1. Maak een afzonderlijke gefragmenteerd MP4 bitstroom net met verspreide nummer (s) zonder audio-en video-tracks.
2. In het' Live Server Manifest"als omschreven in punt 6 in [1], geeft u de naam van het bovenliggende spoor via"parentTrackName"parameter. Zie sectie 4.2.1.2.1.2 in [1] voor meer informatie.
3. In het' Live Server Manifest"manifestOutput moet zijn ingesteld op 'true'.
4. Gezien de tijdelijke aard van de signalering gebeurtenis, wordt u aangeraden:
    1. Aan het begin van de gebeurtenis live verzendt encoder de vakken Koptekst eerste naar de service waarmee de service voor het registreren van de verspreide bijhouden in het manifest van de client.
    2. De HTTP POST-aanvraag het coderingsprogramma moet worden beëindigd wanneer de gegevens niet verzonden.  Een langdurige HTTP POST die u geen gegevens verzendt kunt voorkomen dat Azure Media Services snel verbinding verbreken met het coderingsprogramma bij service update of de server opnieuw opstarten, als de mediaserver tijdelijk worden geblokkeerd in een ontvangstbewerking op de socket. 
    3. Gedurende de tijd als signalering van gegevens niet beschikbaar is, het coderingsprogramma SHOULD sluit de HTTP POST aanvragen.  Tijdens de POST-aanvraag actief is, het coderingsprogramma moet worden gebruikt voor het verzenden van gegevens 
    4. Bij het verzenden van verspreide fragmenten encoder expliciete header met Content-Length ingesteld als deze beschikbaar is.
    5. Bij het verzenden van verspreide fragment met een nieuwe verbinding, moet encoder beginnen met het verzenden van de header vakken gevolgd door de nieuwe fragmenten. Dit is om te handelen wanneer failover heeft plaatsgevonden tussen en de nieuwe tijdelijke verbinding tot stand wordt gebracht met een nieuwe server die heeft gezien de verspreide track voordat.
    6. Het fragment sparse bijhouden worden beschikbaar gesteld aan de client als de bijbehorende bovenliggende bijhoudt fragment dat is gelijk of groter tijdstempel waarde aan de client beschikbaar wordt gesteld. Bijvoorbeeld als het fragment verspreid een tijdstempel van t heeft = 1000, naar verwachting, nadat de client krijgt te zien (aangenomen dat de naam van het bovenliggende track video) video fragment tijdstempel 1000, ook buiten het het verspreide t-fragment kunt downloaden = 1000. Houd er rekening mee dat de werkelijke signaal kan uitstekend worden gebruikt voor een andere positie in de tijdlijn van de presentatie voor het aangegeven doel. In het bovenstaande voorbeeld is het mogelijk dat het verspreide fragment van t = 1000 is een XML-nettolading wordt voor het invoegen van een advertentie in een positie die een paar seconden later.
    7. De payload van het fragment sparse bijhouden kan worden in verschillende andere indelingen (zoals XML of tekst of binaire, enz.) afhankelijk van de verschillende scenario's. 


###<a name="redundant-audio-track"></a>Redundante audiotrack

In een standaardsituatie adaptieve Streaming HTTP (bv. soepele Streaming of streepje) is er vaak slechts een audiotrack in de hele presentatie. In tegenstelling tot de videotracks die meerdere kwaliteitsniveaus voor de client op fouten te hebben, is het audiospoor potentieel risico als het innemen van de stroom met de audiotrack verbroken wordt. 

U lost dit probleem, Microsoft Azure Media Services biedt ondersteuning voor redundante audiotracks live via de mond. Het idee is dat dezelfde audio tracks meerdere keren in verschillende stromen verzenden kunt. Terwijl de service alleen de audio track eenmaal in het manifest van de client registreren wordt, is het redundante audiotracks als back-ups gebruiken voor het ophalen van audio fragmenten als het primaire nummer zijn er problemen. Om de consumptie van redundante audiotracks het coderingsprogramma moet:

1. De dezelfde audio track in meerdere MP4 Fragment bit streams maken. De redundante audiotracks moeten semantisch gelijk met precies de dezelfde fragment tijdstempels en uitwisselbaar op niveau van de kop en het fragment.
2. Ervoor te zorgen dat de "audio" in de Live Server Manifest (hoofdstuk 6 in [1]) zijn hetzelfde voor alle redundante audiotracks.

Hieronder ziet u een aanbevolen implementatie voor redundante audiotracks:

1. Elke unieke audiotrack in een gegevensstroom verzenden door zelf.  Ook een redundante stroom voor elk van deze stromen audio track, waar de 2e stream wijkt af van de 1e alleen door de id in de URL van de HTTP POST verzenden: {protocol} :// {serveradres} / {publiceren van punt path}/Streams({identifier}).
2. Afzonderlijke streams gebruiken voor het verzenden van de twee laagste video bitsnelheid. Elk van deze stromen moet ook een kopie van elke unieke audiospoor bevatten.  Wanneer er meerdere talen worden ondersteund, moeten deze streams audiotracks voor elke taal bevatten.
3. Afzonderlijke serverinstanties (encoder) gebruiken voor het coderen en verzenden van de redundante streams vermeld in (1) en (2). 



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 