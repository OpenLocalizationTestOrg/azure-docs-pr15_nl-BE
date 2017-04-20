<properties 
    pageTitle="Zelfstudies Avanced Media Encoder Premium Workflow" 
    description="Dit document bevat scenario's die laten zien hoe geavanceerde taken met Media Encoder Premium Workflow uit te voeren en ook hoe complexe workflows maken met de werkstroomontwerper." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Zelfstudies voor gevorderden Media Encoder Premium Workflow

##<a name="overview"></a>Overzicht 

Dit document bevat scenario's die tonen hoe werkstromen met **Workflow Designer**aanpassen. U vindt de bestanden voor de werkstroom [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>INHOUDSOPGAVE

De volgende onderwerpen komen aan bod:

- [MXF-codering in een enkele bitsnelheid MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Een nieuwe werkstroom starten](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [Met behulp van de Media bestand Input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Inspectie van streaming media](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [Het toevoegen van een video encoder voor. MP4-bestand genereren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [De audiogegevensstroom codering](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Audio en Video-streams multiplexing naar een MP4-container](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [Het MP4-bestand schrijven](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [Maken van een actief Media-Services van het uitvoerbestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [De voltooide workflow lokaal testen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [MXF-codering naar multibitrate MP4s - dynamische verpakking ingeschakeld](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Een of meer aanvullende MP4 resultaten toe te voegen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [De namen van de uitvoer configureren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Een aparte Audio Track toevoegen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Toevoegen van de. ISM SMIL-bestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [MXF-codering naar MP4 - uitgebreide blauwdruk multibitrate](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Workflowoverzicht te verbeteren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Naamgeving bestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Publicatie-componenteigenschappen naar de hoofdmap van de workflow](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [Namen zijn afhankelijk van de waarden van de eigenschap gepubliceerde uitvoerbestand heeft gegenereerd](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [Miniaturen toevoegen aan MP4 uitvoer multibitrate](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Workflowoverzicht miniaturen toevoegen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [JPG-codering toevoegen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Omgaan met de conversie van kleurruimte](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [Schrijven van de miniaturen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Fouten opsporen in een werkstroom](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Workflow voltooid](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Multibitrate MP4-uitvoer worden ingekort op basis van tijd](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Overzicht van een workflow toe te voegen bijsnijden naar](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Met behulp van de Stream Trimmer](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Workflow voltooid](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [Introductie van de script-Component](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [Scripts in een werkstroom: Hallo wereld](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Gebaseerd op het opmaken van uitvoer multibitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Overzicht toe te voegen bijsnijden naar blauwdruk](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [Met behulp van de Clip XML-lijst](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [Aanpassen van de lijst van de clip vanuit een script-Component](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [Een ClippingEnabled gemak eigenschap toe te voegen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>MXF-codering in een enkele bitsnelheid MP4
 
In dit scenario maken we een enkele bitsnelheid. MP4-bestand met AAC-HE gecodeerde audio van een. MXF invoerbestand. 

###<a id="MXF_to_MP4_start_new"></a>Een nieuwe werkstroom starten 

Workflow Designer te openen en selecteer "Bestand"-"nieuwe werkruimte"-"transcoderen blauwdruk" 

De nieuwe werkstroom wordt 3 elementen weergeven: 

- Primaire bronbestand
- Clip XML-lijst
- Uitvoer bestand-bedrijfsmiddelen  

![Nieuwe codering Workflow](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Nieuwe codering Workflow*

###<a id="MXF_to_MP4_with_file_input"></a>Met behulp van de Media bestand Input

Om onze invoermedium bestand accepteert, begint een met een Media bestand Input-onderdeel toe te voegen. Als een onderdeel toevoegen aan de workflow, zoekt in het zoekvak van de bibliotheek en sleep de gewenste vermelding naar het deelvenster ontwerp. Doe dit voor de Media bestand Input en het primaire bestand onderdeel verbinding met de ingevoerde pincode bestandsnaam uit de Media bestand Input.

![Aangesloten Media File Input](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Aangesloten Media File Input*

Voordat we veel anders doen kunt, moet we eerst om aan te geven de werkstroomontwerper welke voorbeeldbestand we gebruiken willen om onze workflow met ontwerpen. Hiertoe klikt u op de achtergrond van het deelvenster ontwerp en zoekt u de eigenschap van het primaire bestand in het deelvenster rechts van de eigenschap. Klik op het mappictogram en selecteer het gewenste bestand in de werkstroom met testen. Zodra dit is gebeurd, wordt het onderdeel Media bestandsinvoer controleren van het bestand en vullen de output pinnen zodat het bestand dat deze gecontroleerd.

![Gevulde mediabestanden Input](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Gevulde mediabestanden Input*

Terwijl dit geeft aan welke Input willen we werken, niet duidelijk, maar waar de gecodeerde uitvoer moet gaan. Vergelijkbaar met hoe de primaire bron-bestand is geconfigureerd, configureert nu de Output map variabele eigenschap, net onder.

![Geconfigureerde eigenschappen voor invoer en uitvoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Geconfigureerde eigenschappen voor invoer en uitvoer*

###<a id="MXF_to_MP4_streams"></a>Inspectie van streaming media

Vaak is het gewenst om te weten hoe de stroom eruitziet die loopt via de workflow. Als u wilt controleren in een stream op elk punt in de werkstroom, klikt u op een uitvoer of invoer PIN-code op een van de onderdelen. Probeer in dat geval op de pincode niet-gecomprimeerde Video output van de Input van onze Media-bestand te klikken. Een dialoogvenster wordt geopend waarmee de uitgaande video te controleren.

![Inspectie van de pincode niet-gecomprimeerde Video-uitvoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Inspectie van de pincode niet-gecomprimeerde Video-uitvoer*

In ons geval het vertelt ons zo dat we je omgaan met een 1.920 x 1.080 invoer op 24 frames per seconde in 4:2:2 bemonstering voor een video van bijna 2 minuten.

###<a id="MXF_to_MP4_file_generation"></a>Het toevoegen van een video encoder voor. MP4-bestand genereren

Merk op dat nu, een niet-gecomprimeerde Video en meerdere niet-gecomprimeerde Audio output pinnen zijn beschikbaar voor gebruik op onze Media bestand Input. Om de inkomende video codeert, moeten we een codering onderdeel - in dit geval voor het genereren van. MP4-bestanden.

Toevoegen om te coderen in de video-stream met H.264, het onderdeel AVC Video Encoder op het oppervlak van de ontwerpfunctie. Dit onderdeel wordt een video-stream decomprimeren als invoer en biedt een AVC gecomprimeerde video-stream op de uitvoerpin.

![Niet-verbonden AVC-Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Niet-verbonden AVC-Encoder*

De eigenschappen wordt bepaald hoe de codering precies gebeurt. We hebben een overzicht van enkele van de belangrijkste instellingen:

- Uitvoer breedte en hoogte van de Output: deze bepalen de resolutie van de gecodeerde video. In ons geval gaan we met 640 x 360
- Framesnelheid: als de waarde passthrough het dan NET neemt de framesnelheid van de bron is het mogelijk om deze waarschuwing echter negeren. Houd er rekening mee dat deze conversie framesnelheid wordt geen beweging-gecompenseerd.
- Profiel en niveau: deze bepalen de AVC-profiel en het niveau. Als u gemakkelijk meer informatie over de verschillende niveaus en profielen, klikt u op het vraagteken op de component AVC Video Encoder en de help-pagina meer details over elk van de niveaus worden weergegeven. Voor ons voorbeeld, nu met Main profiel op niveau 3.2 (de standaardinstelling).
- Rate Control-modus en de bitsnelheid (kbps): in ons scenario kiezen wij voor een constante bitsnelheid (CBR) uitvoer voor 1200 kbps
- Video-indeling: dit is de VUI (Video bruikbaarheid informatie) die wordt geschreven naar de H.264-stream (side informatie die worden kan gebruikt door een decoder aan het verbeteren van de weergave, maar niet noodzakelijk correct decoderen):
- NTSC (standaard voor de Verenigde Staten of Japan, met 30 fps)
- PAL (standaard voor Europa, met behulp van 25 fps)
- GOP Formaatmodus: zullen we vaste GOP grootte voor onze doeleinden met een Interval van 2 seconden van de sleutel met gesloten GOPs configureren. Op deze manier compatibiliteit met de dynamische verpakking Azure Media-Services biedt.

Onze AVC-encoder diervoeders, verbinding maken met de pincode niet-gecomprimeerde Video-uitvoer van de Media bestand Input component aan de niet-gecomprimeerde Video input pin van het coderingsprogramma AVC.

![AVC het coderingsprogramma verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Het coderingsprogramma verbonden AVC Main*

###<a id="MXF_to_MP4_audio"></a>De audiogegevensstroom codering

Op dit moment hebben we video gecodeerd, maar de oorspronkelijke niet-gecomprimeerde audio-stream nog moet worden gecomprimeerd. Dit gaan we met AAC-codering door het onderdeel AAC Encoder (Dolby). Toe te voegen aan de werkstroom.

![Niet-verbonden AVC-Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Niet-verbonden AAC encoder*

Nu is er een incompatibiliteit: Er is slechts één niet-gecomprimeerde audio invoer pincode vanuit het coderingsprogramma AAC terwijl meer dan waarschijnlijk Media bestand Input twee verschillende niet-gecomprimeerde audio stream beschikbaar heeft: één voor het linkerkanaal audio en één voor het recht. (Als u bent omgaan met surround sound, dat is 6 kanalen.) Het is dus niet mogelijk rechtstreeks verbinding maken de audio van de Media bestand Input bron in het coderingsprogramma van AAC-audio. Het onderdeel AAC verwacht een zogenaamde 'interleaved' audio stream: één stream met de linker- en de juiste kanalen interleaved met elkaar. Zodra we van onze bronbestand media weten zijn welke audiotracks op welke positie in de bron, kunnen we genereren dergelijke afwisselende audio stream met de luidsprekers correct toegewezen posities voor links en rechts.

Een wilt eerst een afwisselende stroom gegenereerd op basis van de vereiste bron audio kanalen. Het onderdeel Audio Stream Interleaver verwerken dit voor ons. Toe te voegen aan de werkstroom en sluit de audio-uitgangen van de Media bestand Input erin.

![Gekoppelde Audio-Stream Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Gekoppelde Audio-Stream Interleaver*

Nu hebben we een afwisselende audio stream, we nog steeds niet geeft de posities naar links of rechts van de luidspreker wilt toewijzen. Om dit opgeven, kunnen we wel statusrapporten positie luidspreker benutten.

![Een luidspreker positie wel statusrapporten toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Een luidspreker positie wel statusrapporten toevoegen*

Luidspreker positie wel statusrapporten voor gebruik met een stereo invoerstroom configureren via een coderingsprogramma vooraf ingesteld Filter 'Aangepast' en "2.0 (L, R)" vooraf ingestelde kanaal genoemd. (Dit wordt toegewezen de positie van de linkerluidspreker aan kanaal 1 en de positie van de rechter luidspreker op kanaal 2.)

Verbinding maken met de uitvoer van de luidsprekers positie wel statusrapporten naar de invoer van de AAC-Encoder. Vervolgens geeft u de AAC Encoder om te werken met een "2.0 (L, R)" kanaal vooraf, zodat deze weet omgaan met stereogeluid als invoer.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Audio en Video-streams multiplexing naar een MP4-container

Gezien onze AVC gecodeerde video-stream en onze AAC gecodeerde audio-stream, we kunnen vastleggen zowel in een. MP4-container. Het proces van het mengen van verschillende streams in een enkele heet "multiplexing" (of "muxing"). Wij zijn in dit geval de audio en video-streams in één samenhangend interleaving. MP4-pakket. Het onderdeel waarmee dit voor een. MP4-container, heet de Multiplexer ISO MPEG-4. Toevoegen aan het oppervlak van de ontwerper en de AVC Video Encoder zowel de AAC Encoder verbinding met de invoer.

![Verbonden MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Verbonden MPEG4 Multiplexer*

###<a id="MXF_to_MP4_writing_mp4"></a>Het MP4-bestand schrijven

Als u een uitvoerbestand schrijft, wordt het onderdeel van de uitvoer in een bestand gebruikt. We kunnen verbinding maken dit met de uitvoer van de ISO MPEG-4 Multiplexer zodat de uitvoer wordt geschreven naar de schijf. U doet dit door verbinding maken met de pincode van de uitvoer Container (MPEG-4) de ingevoerde pincode schrijven van de Output bestand.

![Verbonden uitvoer in een bestand](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Verbonden uitvoer in een bestand*

De bestandsnaam die wordt gebruikt, wordt bepaald door de eigenschap van het bestand. Hoewel deze eigenschap hardcoded aan een gegeven waarde zijn kan, wilt kan een in plaats daarvan via een expressie instellen.

De werkstroom automatisch bepalen van de output bestand de eigenschap name van een expressie, klikt u op de buton naast de bestandsnaam (naast het mappictogram). Selecteer uit de vervolgkeuzelijst 'Expressie'. U krijgt de expressie-editor. Schakel eerst de inhoud van de editor.

![Lege expressie Editor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Lege expressie Editor*

De editor voor expressies kunt invoeren van een letterlijke waarde, gecombineerd met een of meer variabelen. Variabelen beginnen met een dollarteken. Als u de sleutel $ raakt, wordt de editor vervolgkeuzelijst met beschikbare variabelen met een keuze weergegeven. In ons geval gebruikt u een combinatie van de output directory en de basis invoerbestand naam van variabele:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Uit expressie-Editor gevuld](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Uit expressie-Editor gevuld*

>[AZURE.NOTE]Om te kunnen zien een uitvoerbestand zien van uw project coderen in Azure, moet u een waarde in de expressie editor opgeven. 

Als u de expressie te raken ok bevestigt, wordt het eigenschappenvenster bekijken wat op dit moment waarde van de eigenschap bestand lost.

![Expressie bestand herstelt uitvoer dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Expressie bestand herstelt uitvoer dir*

###<a id="MXF_to_MP4_asset_from_output"></a>Maken van een actief Media-Services van het uitvoerbestand

Terwijl we een uitvoerbestand MP4 hebt geschreven, moeten we nog steeds om aan te geven dat dit bestand deel uitmaakt van de mediaservices als gevolg van het uitvoeren van deze workflow, genereert uitvoer activa. Te dien einde wordt de Output bestand/actief knooppunt op het workflowcanvas gebruikt. Alle binnenkomende bestanden naar dit knooppunt maakt deel uit van het resulterende Azure Media Services actief.

Het materiaal uitvoer in een bestand koppelen aan het onderdeel uitvoer bestand-bedrijfsmiddelen van de werkstroom voltooid.

![Workflow voltooid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Workflow voltooid*

###<a id="MXF_to_MP4_test"></a>De voltooide workflow lokaal testen

Druk op de knop afspelen in de werkbalk boven de werkstroom lokaal testen. Als de werkstroom uitgevoerd, controleren de uitvoer gegenereerd in de geconfigureerde uitvoermap. Hier ziet u de voltooide MP4 uitvoerbestand vanaf de invoerbron MXF bestand is gecodeerd.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>Ingeschakeld voor dynamische verpakking MXF-codering naar MP4 - multibitrate

In dit scenario gaan we maken een set van meerdere bitsnelheid MP4-bestanden met gecodeerde AAC audio via één. MXF invoerbestand.

Wanneer de uitvoer van een multi-bitrate activa gewenst is voor gebruik in combinatie met de functies voor dynamische verpakking in Azure Media Services meerdere GOP uitgelijnd MP4-bestanden van elk een andere bitsnelheid en de resolutie moet worden gegenereerd. Hiertoe biedt de procedure [MXF codering in een enkele bitsnelheid MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) ons een goed uitgangspunt.

![Werkstroom starten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Werkstroom starten*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Een of meer aanvullende MP4 resultaten toe te voegen

Alle MP4-bestanden in onze resulterende Azure Media Services actief ondersteunt een verschillende bitsnelheid en resolutie. Stel een of meer MP4 output bestanden toevoegen aan de werkstroom.

Om te zorgen dat we onze video encoders gemaakt met dezelfde instellingen hebben, is het meest geschikte dupliceren van de bestaande AVC Video Encoder en een andere combinatie van resolutie en bitsnelheid (we toevoegen een van 960 x 540 op 25 frames per seconde met 2,5 Mbps) configureren. Als u wilt de bestaande encoder dupliceren, kopiëren plakken op het oppervlak van de ontwerpfunctie.

Verbinding maken met de pincode niet-gecomprimeerde Video-uitvoer van de Media bestand Input in onze nieuwe AVC-component.

![Tweede AVC coderingsprogramma verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Tweede AVC coderingsprogramma verbonden*

De configuratie voor onze nieuwe AVC-encoder voor uitvoer van 960 x 540 met 2,5 Mbps nu aanpassen. (Gebruik de eigenschappen Output "breedte", "Hoogte uitvoer" en "Bitsnelheid (kbps)" hiervoor.)

Krijgen we het resulterende actief samen met dynamische verpakking Azure Media Services wilt, moet het eindpunt streaming kunnen genereren uit deze MP4-bestanden HLS Fragmented/MP4/streepje fragmenten die exact met elkaar op een manier dat clients die tussen verschillende bitsnelheid overschakelt een enkele goede continue video en audio-ervaring krijgen zijn uitgelijnd. Als u dit kunt doen, moeten we ervoor zorgen dat, in de eigenschappen van beide encoders AVC de GOP ("groep foto's ') voor beide MP4-bestanden is ingesteld op 2 seconden, wat kan worden gedaan door:

- de Formaatmodus GOP GOP vaste grootte instellen en
- de sleutel Frame-Interval op twee seconden.
- ook worden de gesloten GOP zodat alle GOP GOP IDR besturingselement permanent over hun eigen zonder afhankelijkheden

Wijzigen zodat onze workflow gemakkelijk te begrijpen, de eerste AVC-codering naar "AVC Video Encoder 640 x 360 1200kbps" en de tweede AVC-encoder "AVC Video Encoder 960 x 540 2500 k '.

Een tweede ISO MPEG-4 Multiplexer en de uitvoer van een tweede bestand toevoegen. De multiplexer verbinden met de nieuwe AVC-codering en zorg ervoor dat de uitvoer in de uitvoer in een bestand wordt geleid. Sluit ook de AAC encoder audio output naar de nieuwe multiplexer van invoer. De uitvoer in een bestand kan op zijn beurt dan worden aangesloten op de Output bestand/actief knooppunt toe te voegen aan het Media-Services actief die worden gemaakt.

![Tweede mixer en de uitvoer in een bestand verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Tweede mixer en de uitvoer in een bestand verbonden*

De multiplexer Chunk modus GOP count of duur configureren voor compatibiliteit met dynamische verpakking Azure Media Services, en de GOPs per segment ingesteld op 1. (Dit is de standaardinstelling.)

![Mixer Chunk modi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Mixer Chunk modi*

Opmerking: u kunt dit proces herhalen voor alle andere bitsnelheid en resolutie combinaties die u wilt toevoegen aan de uitvoer van de activa.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>De namen van de uitvoer configureren

We hebben meer dan één enkel bestand toegevoegd aan de activa van de uitvoer. Dit is nodig om te controleren of dat de bestandsnamen voor elk van de uitvoerbestanden zijn van elkaar verschillen en een naamgevingsconventie misschien zelfs toepassen zodat duidelijk uit de naam van het bestand bent u omgaan blijkt met.

Uitvoer bestandsnamen kan worden bestuurd via de expressies in de ontwerpfunctie. Open het eigenschappenvenster voor een van de onderdelen van de uitvoer in een bestand en open de editor van de expressie voor de eigenschap van het bestand. Onze eerste uitvoerbestand is geconfigureerd met behulp van de volgende expressie (Zie de handleiding voor die van de [MXF aan een enkele bitsnelheid output MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Dit betekent dat onze bestandsnaam wordt bepaald door twee variabelen: de uitvoermap om in te schrijven en de basisnaam van gegevensbron bestand. De systeemprestatiemeteritems als een eigenschap in de hoofdmap van de workflow en deze wordt bepaald door het inkomende bestand. Houd er rekening mee dat de uitvoermap gebruikt wordt om lokaal te testen; Deze eigenschap wordt overschreven door de workflow-engine als de werkstroom wordt uitgevoerd door de Mediaprocessor cloud-gebaseerde in Azure Media Services.
Om onze uitvoerbestanden een consistente uitvoer benoemen, het eerste bestand benoemen expressie te wijzigen:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

en de tweede tot en met:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Uitvoeren van een tussenliggende test uitvoeren om ervoor te zorgen dat beide output MP4 bestanden correct worden gegenereerd.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Een aparte Audio Track toevoegen

Later zullen we zien als we een bestand .ism om te gaan met onze MP4-bestanden voor uitvoer genereert, wordt moet een bestand met alleen audio MP4 als het audiospoor voor onze adaptieve streaming. Dit bestand is gemaakt, een extra mixer toevoegen aan de werkstroom (Multiplexer ISO-MPEG-4) en AAC-encoder uitvoerpin verbinden met de ingevoerde pincode voor spoor 1.

![Audio mixer toegevoegd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Audio mixer toegevoegd*

Maak een derde component output van de uitgaande stroom van de mixer en configureren van de naamgevingsregels voor expressie als uitvoer in een bestand:
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Audio mixer uitvoer in een bestand maken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Audio mixer uitvoer in een bestand maken*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Toevoegen van de. ISM SMIL-bestand

Voor de dynamische verpakking werken in combinatie met zowel MP4-bestanden (en de alleen audio MP4) in onze Media Services actief is, moeten we ook een manifestbestand (ook wel genoemd een "SMIL" bestand: Synchronized Multimedia Integration Language). Dit bestand geeft Azure Media Services welke MP4-bestanden zijn beschikbaar voor dynamische verpakking en welke van die rekening te houden voor audio streaming. Een typische manifestbestand voor een reeks van MP4 met één audio stream ziet er zo uit:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

Het bestand .ism bevat binnen een switchinstructie, een verwijzing naar elk van de afzonderlijke video MP4-bestanden en ook een (of meer)-bestandsverwijzingen naar een MP4 met alleen de audio.

Genereren van het manifestbestand voor onze reeks van MP4 kan worden gedaan door middel van een onderdeel dat de 'AMS Manifest Writer' genoemd. Als u wilt gebruiken, sleept u deze naar het oppervlak en verbinding maken met de "Volledig schrijven" output pinnen van de drie onderdelen van de uitvoer in een bestand op de invoer van AMS Manifest van de schrijver. Controleer of de uitvoer van de schrijver AMS Manifest van de verbinding naar het Output bestand/actief.

Net als bij onze andere bestand uitvoer onderdelen, de naam van het .ism-uitvoer met een expressie te configureren:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Onze workflow voltooid ziet eruit als de onderstaande:

![Voltooide MXF bij multibitrate MP4 workflow](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Voltooide MXF bij multibitrate MP4 workflow*

##<a id="MXF_to__multibitrate_MP4"></a>MXF-codering naar MP4 - uitgebreide blauwdruk multibitrate

In het [vorige scenario van de werkstroom](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) hebt we gezien hoe een enkel MXF invoer actief kan worden omgezet in een actief uitvoer met multi-bitrate MP4-bestanden, een bestand met alleen audio MP4 en een manifest-bestand voor gebruik in combinatie met Azure Media Services dynamische verpakking.

In dit scenario wordt weergegeven hoe sommige van de aspecten kan worden verbeterd en eenvoudiger gemaakt.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Workflowoverzicht te verbeteren

![Multibitrate MP4-workflow te verbeteren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4-workflow te verbeteren*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Naamgeving bestand

In de vorige workflow we een enkelvoudige expressie opgegeven als basis voor het genereren van bestandsnamen voor uitvoer. Hebben we enkele duplicatie Hoewel: alle de onderdelen afzonderlijk uitvoer bestand deze expressie opgegeven.

Onze onderdeel van de output bestand voor de eerste video bestand is bijvoorbeeld geconfigureerd met deze expressie:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Terwijl voor het tweede output video hebben we de expressie:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Zou het niet beter, minder gevoelig en handiger zijn als wij kan deze duplicaten verwijderen en zo meer configureerbaar in plaats daarvan? Gelukkig kunnen wij: de ontwerper van de expressie-mogelijkheden in combinatie met de mogelijkheid om aangepaste eigenschappen maken in de hoofdmap van de werkstroom biedt ons een extra beveiligingslaag gemak.

Eens dat we je configuratie bestandsnaam-station van de bitsnelheid van de afzonderlijke MP4-bestanden. Deze bitsnelheid wij configureren vanuit één centraal punt (in de hoofdmap van de grafiek) beogen zult, waar ze zullen toegankelijk configureren en station bestandsnaam wordt gegenereerd. Hiertoe begin met het publiceren van de eigenschap bitsnelheid van beide encoders AVC naar de hoofdmap van de werkstroom, zodat deze toegankelijk is vanuit zowel de hoofdmap en met ingang van de AVC-encoders. (Zelfs als weergegeven in twee verschillende steunkleuren, er is slechts één onderliggende waarde.)

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Publicatie-componenteigenschappen naar de hoofdmap van de workflow

Open de eerste AVC-codering, gaat u naar de eigenschap bitsnelheid (kbps) en kies in de vervolgkeuzelijst publiceren.

![De eigenschap bitsnelheid publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*De eigenschap bitsnelheid publiceren*

Het dialoogvenster publiceren publiceren naar de hoofdmap van onze grafiek workflow configureren met de gepubliceerde naam van "video1bitrate" en een leesbare naam van "Bitrate Video 1". Configureren van een aangepaste naam "Streaming bitsnelheid" genoemd, en klik op publiceren.

![De eigenschap bitsnelheid publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Publishing dialoogvenster voor bitsnelheid, eigenschap*

Herhaal dezelfde voor de eigenschap van de bitsnelheid van de tweede AVC-codering en geef deze de naam 'video2bitrate' met de weergavenaam "Video 2 Bitrate" in dezelfde aangepaste groep 'Bitsnelheid Streaming'.

Als we nu de eigenschappen van de basis-werkstroom controleren, zien we onze aangepaste groep met de gepubliceerde twee eigenschappen worden weergegeven. Beide zijn als gevolg van de waarde van hun respectieve AVC-Codering bitsnelheid.

![De bitsnelheid van de gepubliceerde steunbalken in de hoofdmap van de workflow](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Als we toegang tot deze eigenschappen van code of van een expressie willen, wel we als volgt:

- in inline-code van een onderdeel direct onder de hoofdmap: node.getPropertyAsString('.. / video1bitrate ", null)
- in een expressie: ${ROOT_video1bitrate}
 
We publiceren onze audiotrack bitsnelheid alsmede op het voltooien de groep 'Bitsnelheid Streaming'. Zoeken naar de instelling van de bitsnelheid en selecteer publiceren in de vervolgkeuzelijst naast het in de eigenschappen van de AAC-Encoder. Publiceren naar de hoofdmap van de grafiek met de naam 'audio1bitrate' en de weergavenaam 'Audio 1 bitsnelheid' binnen onze aangepaste groep 'Bitsnelheid Streaming'.

![Publishing dialoogvenster voor audio bitrate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Publishing dialoogvenster voor audio bitrate*

![Resulterende video en audio-eigenschappen in de hoofdmap](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Resulterende video en audio-eigenschappen in de hoofdmap*

Wijzigen van een van deze drie waarden ook opnieuw worden geconfigureerd en wijzigt de waarden in de bijbehorende onderdelen zijn gekoppeld met (en waar vanuit gepubliceerd).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>Namen zijn afhankelijk van de waarden van de eigenschap gepubliceerde uitvoerbestand heeft gegenereerd

In plaats van hardcoding onze gegenereerde namen we kunt wijzigen onze bestandsnaam-expressie op elk van de onderdelen van de uitvoer in een bestand met zijn afhankelijk van de bitsnelheid eigenschappen die we zojuist gepubliceerd in de hoofdmap van de grafiek. Vanaf onze eerste uitvoer in een bestand wordt de eigenschap van het bestand niet vinden en bewerkt u de expressie als volgt:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Verschillende parameters die in deze expressie kunnen worden benaderd en te raken van de dollar-teken op het toetsenbord terwijl u in het expressievenster ingevoerd. Een van de beschikbare parameters is onze video1bitrate-eigenschap die we eerder gepubliceerd.

![Toegang tot de parameters van een expressie](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Toegang tot de parameters van een expressie*

Doe hetzelfde voor de uitvoer in een bestand voor onze tweede video:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

en voor het bestand met alleen audio-uitvoer:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Als we nu de bitsnelheid voor de video- of audio-bestanden wijzigen, de respectieve encoder opnieuw wordt geconfigureerd en de bitsnelheid-bestand met de naam overeenkomst wordt van kracht alles verloopt geheel automatisch.

##<a id="thumbnails_to__multibitrate_MP4"></a>Miniaturen toevoegen aan MP4 uitvoer multibitrate

Uitgaande van een werkstroom die [een multibitrate MP4-uitvoer van een MXF invoer genereert](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), bekijken we nu in miniaturen toe te voegen aan de uitvoer.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Workflowoverzicht miniaturen toevoegen

![Werkstroom starten vanaf Multibitrate MP4](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Werkstroom starten vanaf Multibitrate MP4*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>JPG-codering toevoegen

De kern van onze miniaturen generatie het onderdeel Encoder JPG JPG-bestanden kunnen worden.

![JPG-Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG-Encoder*

We kunnen niet echter rechtstreeks verbinding maken onze niet-gecomprimeerde Video-stream van de Media bestand Input in het coderingsprogramma JPG. In plaats daarvan verwacht het individuele frames worden overhandigd. Dit doen we via de component Video Frame Gate.

![Een frame-poort verbinding met het coderingsprogramma JPG](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Een frame-poort verbinding met het coderingsprogramma JPG*

De gate frame om de zoveel seconden of frames kunt een videoframe wilt doorgeven. Het interval en de offset waarmee dit gebeurt is geconfigureerd in de eigenschappen.

![Video-Gate Frame-eigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Video-Gate Frame-eigenschappen*

We maken een miniatuur van elke minuut door de modus op de tijd (seconden) en het Interval op 60.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Omgaan met de conversie van kleurruimte

Terwijl het lijkt het logisch dat zowel niet-gecomprimeerde Video pennen van de uitgang van het frame en de invoer van Media-bestand kunnen nu worden verbonden, zou we een waarschuwing krijgen als wij zou doen.

![Ruimte voor kleur invoerfout](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Ruimte voor kleur invoerfout*

Dit komt omdat de manier waarop in welke kleur informatie wordt weergegeven in onze oorspronkelijke raw niet-gecomprimeerde video-stream, die afkomstig zijn van onze MXF, verschilt van wat de JPG-Encoder verwacht. Meer in het bijzonder, een zogenaamde "kleurruimte" van "RGB" of "Grijswaarden" naar verwachting flow. Dit betekent dat de Video Frame Gate van inkomende video-stream moet een conversie eerst toegepast met betrekking tot de kleurruimte.

Sleep de shape naar de werkstroom de kleur ruimte Converter - Intel en verbind deze met onze gate frame.

![Verbinding maken met een kleur ruimte Convertor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Verbinding maken met een kleur ruimte Convertor*

Kies de post BGR 24 uit de lijst van de voorinstelling in het eigenschappenvenster.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Schrijven van de miniaturen

Anders dan onze MP4-video, het JPG-Encoder onderdeel zal uitvoerbestand meer dan één. Om te kunnen aanpakken, een onderdeel van de schrijver scène zoeken JPG-bestand kan worden gebruikt: wordt nemen de binnenkomende miniaturen van JPG en schrijf deze, elke bestandsnaam wordt voorafgegaan door een ander nummer. (Het getal gewoonlijk het aantal seconden/eenheden in de stroom die de miniatuur is ontleend aan aangeeft).


![Introductie van de schrijver scène zoeken JPG-bestand](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Introductie van de schrijver scène zoeken JPG-bestand*

De eigenschap uitvoerpad map configureren met de expressie: ${ROOT_outputWriteDirectory} 

en de eigenschap Filename Prefix met: 

    ${ROOT_sourceFileBaseName}_thumb_

Het voorvoegsel wordt bepaald hoe de miniatuurbestanden worden wordt genoemd. Zij wordt voorafgegaan door een getal dat de positie van het blokje in de stream aangeeft.


![Eigenschappen van scène zoeken JPG bestand Writer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Eigenschappen van scène zoeken JPG bestand Writer*

De schrijver scène zoeken JPG-bestand verbinding met het knooppunt uitvoer bestand-bedrijfsmiddelen.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Fouten opsporen in een werkstroom

Verbinding maken met de invoer van het conversieprogramma kleur ruimte de ruwe niet-gecomprimeerde video-uitgang. Een lokale test uit voor de werkstroom nu uitvoeren. Er is een goede kans dat de werkstroom wordt plotseling niet meer uitvoeren en aangeven met een rode omtrek op het onderdeel dat is een fout opgetreden:

![Kleur ruimte Converter fout](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Kleur ruimte Converter fout*

Klik op de weinig rood "E"-pictogram in de bovenste rechter hoek van de component kleur ruimte Converter om te zien wat de reden van de codering poging is mislukt.

![Dialoogvenster kleuren ruimte Converter](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Dialoogvenster kleuren ruimte Converter*

Het blijkt, zoals u ziet, dat is de binnenkomende kleurruimte voor kleur ruimte converter standaard rec601 voor onze aangevraagde YUV-conversie in RGB. Blijkbaar onze stream niet geven aan de rec601. (Rec 601 is een standaard voor het coderen van geïnterlinieerde video analoge signalen in digitale video vorm. Deze geeft een actieve gebied die betrekking hebben op 720 luminantie monsters en 360 chrominantie monsters per regel. De kleur codering systeem staat bekend als YCbCr 4:2:2.)

U kunt dit oplossen, geven we op de metagegevens van de stroom die wij je omgaan met rec601 inhoud. Hiervoor gebruikt u een component Video Data Type Updater, die wij tussen onze onbewerkte brongegevens en de component conversie ruimte plaatst. Dit type gegevens updater kan voor het handmatig bijwerken van bepaalde gegevens van de video-eigenschappen. Configureren voor het geven van een kleur ruimte standaard van "Rec 601". Hierdoor wordt de Video Data Type Updater label stream de kleurruimte "Rec 601" Als er geen kleurruimte nog gedefinieerd is. (Niet overschrijft alle bestaande metagegevens, tenzij u het selectievakje overschrijven is ingeschakeld.)

![Kleur ruimte standaard op het Type gegevens Updater bijwerken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Kleur ruimte standaard op het Type gegevens Updater bijwerken*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Workflow voltooid

Nu onze onze workflow is voltooid, voert u een andere test uitvoeren om te zien geven.

![Voltooide workflow voor multi-mp4-uitvoer met miniaturen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Voltooide workflow voor multi-mp4-uitvoer met miniaturen*

##<a id="time_based_trim"></a>Multibitrate MP4-uitvoer worden ingekort op basis van tijd

Uitgaande van een werkstroom die [een multibitrate MP4-uitvoer van een MXF invoer genereert](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), bekijken we nu in de bronvideo op basis van de tijdstempels bijsnijden.

###<a id="time_based_trim_start"></a>Overzicht van een workflow toe te voegen bijsnijden naar

![Werkstroom wilt bijsnijden naar toevoegen starten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Werkstroom wilt bijsnijden naar toevoegen starten*

###<a id="time_based_trim_use_stream_trimmer"></a>Met behulp van de Stream Trimmer

Het onderdeel Stream Trimmer kunt Snijd het begin en einde van de invoerstroom op timing informatie (seconden, minuten,...). De trimmer biedt geen ondersteuning voor frames gebaseerde bijsnijden.

![Stream Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Stream Trimmer*

In plaats van rechtstreeks de AVC-encoders en luidspreker positie wel statusrapporten koppelt aan de Media bestand Input, plaatst we tussen die de stroom trimmer. (Één voor het videosignaal en één voor de afwisselende audio signaal).

![Stream Trimmer tussen plaatsen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Stream Trimmer tussen plaatsen*

Laten we de trimmer zodanig configureren dat we alleen de video en audio tussen 15 seconden en 60 seconden in de video verwerkt.

Ga naar de eigenschappen van de Video-Stream Trimmer en (gates 15) begintijd en eindtijd (60s)-eigenschappen configureren. Zorg ervoor dat zowel onze audio- en video-trimmer altijd naar dezelfde begin- en eind-waarden zijn geconfigureerd, wordt gepubliceerd die naar de hoofdmap van de werkstroom.

![Eigenschap start uit Stream Trimmer publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Eigenschap start uit Stream Trimmer publiceren*

![Eigenschappenvenster voor begintijd publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Eigenschappenvenster voor begintijd publiceren*

![Eigenschappenvenster voor eindtijd publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Eigenschappenvenster voor eindtijd publiceren*


Als we nu de basis van onze workflow controleren, beide eigenschappen worden netjes weergegeven en kunnen worden geconfigureerd vanaf daar.

![Gepubliceerde eigenschappen die beschikbaar zijn in de hoofdmap](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Gepubliceerde eigenschappen die beschikbaar zijn in de hoofdmap*

Nu opent u de eigenschappen van het snijden van de audio trimmer en begin-en eindtijd configureren met een expressie die naar de gepubliceerde eigenschappen in de hoofdmap van de werkstroom verwijst.

Begintijd voor het bijsnijden van geluid:

    ${ROOT_TrimmingStartTime}

en voor de eindtijd:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Workflow voltooid

![Workflow voltooid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Workflow voltooid*


##<a id="scripting"></a>Introductie van de script-Component

Script Components willekeurige scripts kunnen worden uitgevoerd tijdens de fasen van de uitvoering van onze workflow. Er zijn vier verschillende scripts die kunnen worden uitgevoerd, elk met specifieke kenmerken en hun eigen plaats in de workflow-levenscyclus:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

De documentatie van het onderdeel script gaat in meer detail voor elk van de bovenstaande. In [de volgende sectie](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)wordt het scriptonderdeel **realizeScript** gebruikt een xml cliplist onderweg te maken wanneer de werkstroom wordt gestart. Dit script wordt aangeroepen tijdens de installatie component slechts eenmaal in de levenscyclus gebeurt.


###<a id="scripting_hello_world"></a>Scripts in een werkstroom: Hallo wereld

Sleep een onderdeel script naar het oppervlak designer en wijzigt u de naam (bijvoorbeeld "SetClipListXML").

![Toevoegen van een script-Component](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Toevoegen van een script-Component*

Wanneer u de eigenschappen van het onderdeel script inspecteert, de vier verschillende scripttypen worden weergegeven, elk naar een ander script te configureren.

![Eigenschappen van script](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Eigenschappen van script*

Schakel de processInputScript en open de editor voor de realizeScript. We zijn nu ingesteld en klaar om te gaan met het uitvoeren van scripts.

Scripts zijn geschreven in Groovy, een dynamisch gecompileerde scripttaal voor het Java-platform dat compatibel is met Java behoudt. Eigenlijk, de meeste Java-code is geldig Groovy.

We gaan een eenvoudige hello world groovy script schrijven in het kader van onze realizeScript. Typ het volgende in de editor:

    node.log("hello world");

Een lokale test run nu uitvoeren. Na het uitvoeren, bekijken (via het tabblad systeem van het onderdeel script) de eigenschap Logboeken.

![De uitvoer van Hallo wereld](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*De uitvoer van Hallo wereld*

Het knooppuntobject noemen we de methode logboek, verwijst naar onze huidige 'node' of het onderdeel dat wij binnen bent scripting. Elk onderdeel heeft zo de mogelijkheid om logboekregistratie uitvoergegevens, beschikbaar via het tabblad systeem. In dit geval uitvoer is de letterlijke tekenreeks 'hello world'. Belangrijk om te begrijpen is dat dit te bewijzen als een waardevol hulpmiddel voor het opsporen van fouten, zodat u inzicht in wat het script daadwerkelijk doet.

Van binnen onze scriptomgeving hebben we ook toegang tot de eigenschappen op andere onderdelen. Probeer dit eens:


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Het logboekvenster tonen ons het volgende:

![De uitvoer van voor de toegang tot Knooppuntpaden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*De uitvoer van voor de toegang tot Knooppuntpaden*


##<a id="frame_based_trim"></a>Gebaseerd op het opmaken van uitvoer multibitrate MP4

Uitgaande van een werkstroom die [een multibitrate MP4-uitvoer van een MXF invoer genereert](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), bekijken we nu in de bronvideo op basis van aantallen frame bijsnijden.

###<a id="frame_based_trim_start"></a>Overzicht toe te voegen bijsnijden naar blauwdruk

![Workflow toe te voegen bijsnijden naar](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Workflow toe te voegen bijsnijden naar*

###<a id="frame_based_trim_clip_list"></a>Met behulp van de Clip XML-lijst

In alle vorige workflow zelfstudies, hebben we het onderdeel Media bestand Input gebruikt als onze video-invoerbron. Voor dit specifieke scenario, we gebruikt het brononderdeel Clip lijst in plaats daarvan. Houd er rekening mee dat dit niet de beste manier van werken moet; de Clip lijst bron alleen gebruiken wanneer er een echte reden om dit te doen (zoals de onderstaande gevallen waar eens gebruik van de clip snijden mogelijkheden).

Sleep de Clip lijst bron-component naar het ontwerpvenster van onze bestandsinvoer Media naar de bron van de lijst Clip wilt overschakelen, en de pincode Clip lijst XML-verbinding met de Clip lijst-XML-knooppunt van de werkstroomontwerper. Dit moet de bron van de Clip lijst met pennen voor uitvoer, vullen volgens onze video input. Verbind nu de niet-gecomprimeerde Video- en niet-gecomprimeerde Audio pinnen uit de lijst bron Clip aan de respectieve AVC Encoders en Audio Stream Interleaver. De invoer van Media-bestand nu verwijderen.

![De invoer van Media-bestand vervangen door de Clip lijst bron](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*De invoer van Media-bestand vervangen door de Clip lijst bron*

Het brononderdeel Clip lijst wordt als invoer een "Clip XML-lijst". Wanneer u het bronbestand te testen met lokaal selecteert, is deze lijst clip xml automatisch voor u ingevuld.

![Eigenschap voor XML-lijst van Clip automatisch gevuld](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Eigenschap voor XML-lijst van Clip automatisch gevuld*

Op zoek naar een beetje dichter bij het XML-bestand, is dit hoe het er uitziet als:

![Dialoogvenster met de clip lijst bewerken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Dialoogvenster met de clip lijst bewerken*

Dit echter doorgevoerd niet in de mogelijkheden van de clip-lijst-xml. Een optie die we hebben is een element "Trim" onder zowel de video en audio bron als volgt toevoegen:

![Een element met spaties toe te voegen aan de lijst met clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Een element met spaties toe te voegen aan de lijst met clip*

Als u de XML-lijst clip als hierboven wijzigen en een lokale test uit te voeren, ziet u de video correct is tussen 10 en 20 seconden in de video wordt bijgesneden.

In tegenstelling tot wat er gebeurt wanneer u via een lokale uitvoering, deze zeer dezelfde cliplist xml geen hetzelfde effect wordt toegepast in een werkstroom die wordt uitgevoerd in Azure Media Services. Wanneer Azure Premium coderingsprogramma wordt gestart, wordt de xml cliplist telkens opnieuw, op basis van het invoerbestand kreeg de taak van de codering gegenereerd. Dit betekent dat zou helaas we op de xml doen wijzigingen worden overschreven.

Om de cliplist xml wordt gewist, als een codering taak is begonnen, kunnen we opnieuw genereren deze onderweg net na het begin van onze workflow. Deze aangepaste acties kunnen worden genomen door middel van wat een "script-Component" wordt genoemd. Zie voor meer informatie, de [Introductie van het onderdeel script](media-services-media-encoder-premium-workflow-tutorials.md#scripting).


Sleep een onderdeel script naar het oppervlak designer en wijzig de naam in 'SetClipListXML'.

![Toevoegen van een script-Component](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Toevoegen van een script-Component*

Wanneer u de eigenschappen van het onderdeel script inspecteert, de vier verschillende scripttypen worden weergegeven, elk naar een ander script te configureren.

![Eigenschappen van script](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Eigenschappen van script*


###<a id="frame_based_trim_modify_clip_list"></a>Aanpassen van de lijst van de clip vanuit een script-Component

Voordat we de cliplist XML-bestand dat wordt gegenereerd tijdens de workflow opnieuw schrijven kunt, moet u toegang hebben tot de inhoud en het cliplist XML-eigenschap. We kunnen dit doen als volgt:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Lijst met binnenkomende clip aan te melden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Lijst met binnenkomende clip aan te melden*

Eerst moeten we een manier om te bepalen vanaf welk moment tot welk punt we willen de video knippen. Als u wilt dit handige de gebruiker minder technische van de werkstroom, twee eigenschappen te publiceren naar de hoofdmap van de grafiek. U doet dit door het oppervlak van de ontwerpfunctie met de rechtermuisknop op en selecteer 'Eigenschap toevoegen':

- De eerste eigenschap: "ClippingTimeStart" van het type: "TIJDCODE"
- Tweede eigenschap: "ClippingTimeEnd" van het type: "TIJDCODE"

![Eigenschappenvenster voor uitknippen begintijd toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Eigenschappenvenster voor uitknippen begintijd toevoegen*

![Gepubliceerd tijd steunbalken knippen in de hoofdmap van de workflow](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Gepubliceerd tijd steunbalken knippen in de hoofdmap van de workflow*

Beide eigenschappen op een geschikte waarde configureren:

![Configureer het begin knippen en beëindigen van eigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Configureer het begin knippen en beëindigen van eigenschappen*

Nu, uit binnen onze script, we toegang tot beide eigenschappen als volgt:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Logboekvenster weergeven van begin- en einddatum van knippen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Logboekvenster weergeven van begin- en einddatum van knippen*

Laten we de tijdcode tekenreeksen parseren tot een gemakkelijker te gebruiken met behulp van een eenvoudige reguliere expressie:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Het logboekvenster met uitvoer geparseerde tijdcode](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Het logboekvenster met uitvoer geparseerde tijdcode*

Met deze informatie bij de hand kunnen we nu de xml cliplist zodat de begin- en eindtijden voor de gewenste frame nauwkeurig knippen van de film wijzigen.

![Scriptcode te knippen elementen toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Scriptcode te knippen elementen toevoegen*

Dit is gedaan door middel van bewerkingen voor het manipuleren van normale tekenreeks. Het resulterende XML-lijst gewijzigde illustratie is geschreven aan de eigenschap clipListXML in de hoofdmap van de werkstroom via de methode "setProperty". Het logboekvenster na het uitvoeren van een andere test zou ons het volgende worden weergegeven:

![Logboekregistratie van de resulterende lijst met clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Logboekregistratie van de resulterende lijst met clip*

Doe een test uitgevoerd om te zien hoe de video en audio streams hebt is afgekapt. Als u meer dan één test uitgevoerd met verschillende waarden voor de punten bijsnijden doet, zult u merken dat die zal geen rekening worden gehouden maar! De reden hiervoor is dat de ontwerper, in tegenstelling tot de Azure runtime niet de xml cliplist elke uitvoeren overschrijven. Dit betekent dat alleen de eerste keer dat u hebt ingesteld de in- en uitpunten, zal de xml te transformeren, alle andere tijden, onze guard-component (als (clipListXML.indexOf ("<trim>") -1 ==)) wordt voorkomen dat de werkstroom een andere knippen element toe te voegen wanneer er al een aanwezige.

Om onze workflow handige lokaal testen we best sommige house-keeping code toevoegen die inspecteert als een element knippen aanwezig was. In dat geval kunnen wij deze verwijderen voordat u doorgaat door aanpassing van het XML-bestand met de nieuwe waarden. In plaats van gewone string manipulaties, is het waarschijnlijk veiliger om dit te doen via het objectmodel van echte xml parseren.

Voordat we deze code echter toevoegen kunt, moeten we eerst een aantal importinstructies toevoegen aan het begin van het script:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Daarna kunnen we de vereiste reinigen code toevoegen:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Deze code gaat net boven het punt waarop we knippen elementen aan de cliplist xml toevoegen.

We kunnen op dit moment worden uitgevoerd en onze workflow als net als we willen terwijl de wijzigingen worden toegepast ooit tijd wijzigen.    

###<a id="frame_based_trim_clippingenabled_prop"></a>Een ClippingEnabled gemak eigenschap toe te voegen

Zoals u niet altijd bijsnijden om te gebeuren wilt misschien, laten we klaar bent met het uit onze workflow door het toevoegen van een handige Boole-vlag die aangeeft of we bijsnijden wilt / knippen inschakelen.

Publiceren, net als een nieuwe eigenschap aan de basis van onze workflow genaamd "ClippingEnabled" van type "BOOLEAN".

![Gepubliceerd, een eigenschap voor het inschakelen van uitknippaden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Gepubliceerd, een eigenschap voor het inschakelen van uitknippaden*

Met de onderstaande eenvoudige guard-component, we kunnen controleren of bijsnijden vereist is en beslissen of onze lijst clip als zodanig worden gewijzigd moet of niet.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Volledige code

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Zie ook 

[Introductie van Premium Services in Azure Media-codering](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Het gebruik van Premium Services in Azure Media-codering](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Codering van inhoud op aanvraag met Azure Media Service](media-services-encode-asset.md#media_encoder_premium_workflow)

[Media Encoder Premium Workflow-indelingen en -Codecs](media-services-premium-workflow-encoder-formats.md)

[Workflow-voorbeeldbestanden](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Explorer tool](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
