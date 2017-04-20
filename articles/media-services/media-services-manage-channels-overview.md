<properties 
    pageTitle="Overzicht van Live indien gestoomd met Azure Media Services | Microsoft Azure" 
    description="Dit onderwerp geeft een overzicht van live indien gestoomd met Azure Media Services." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Overzicht van Live indien gestoomd met Azure Media Services

##<a name="overview"></a>Overzicht

De volgende onderdelen zijn vaak betrokken live streaming evenementen met Azure Media Services leveren:

- Een camera die wordt gebruikt voor het uitzenden van een gebeurtenis.
- Een live video encoder die worden omgezet in signalen van de camera streams die worden verzonden naar een live streaming service.

    U kunt desgewenst meerdere levende tijd gesynchroniseerd encoders. Voor bepaalde essentiële live gebeurtenissen vraag zeer hoge beschikbaarheid en de kwaliteit van de ervaring, het verdient aanbeveling om actieve redundante encoders met synchroniseren van de tijd voor een naadloze failover zonder verlies van gegevens.
- Een live streaming-service waarmee u het volgende doen:
    
    - consumptie van live inhoud met behulp van verschillende live streaming protocollen (bijvoorbeeld RTMP of goede Streaming)
    - (optioneel) de stream te coderen in adaptieve bitsnelheid
    - Voorbeeld van de live gegevensstroom
    - opnemen en de aangezogen inhoud opslaan om later (Video-on-Demand) worden gestreamd
    - de inhoud door middel van algemene streaming protocollen (bijvoorbeeld MPEG-streepje, vloeiend, HLS, harde schijven) leveren rechtstreeks aan uw klanten, of om een inhoud Delivery Network (CDN) voor verdere distributie.


**Azure Microsoft mediaservices** (AMS) biedt de mogelijkheid om te nemen, coderen, weergeven, opslaan en leveren uw live streaming inhoud.

Uw inhoud leveren aan klanten is uw doel een video van hoge kwaliteit leveren aan verschillende apparaten onder verschillende omstandigheden. Om dit live encoders te gebruiken om de stream op een multi-bitrate (adaptief) video bitsnelheid.  Gebruiken om te zorgen voor streaming op verschillende apparaten, Media Services [dynamische verpakking](media-services-dynamic-packaging-overview.md) uw stream naar verschillende protocollen dynamisch opnieuw inpakken. Media Services biedt ondersteuning voor de levering van de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS) past, soepele Streaming MPEG-streepje en harde schijven (voor Adobe WIP/Access licentiehouders).

In Azure Media Services verwerken **kanalen**, **programma's**en **StreamingEndpoints** alle live streaming functies waaronder ingest, opmaak, DVR, beveiliging, schaalbaarheid en redundantie.

Een **kanaal** staat voor een pijpleiding voor het verwerken van live streaming inhoud. Een kanaal kan ontvangen een live streams op de volgende manieren invoeren:

- Een bedrijf op live encoder verzendt multi-bitrate **RTMP** of **Goede Streaming** (gefragmenteerde MP4) op het kanaal dat is geconfigureerd voor de levering van de **Pass Through-query** . De levering van de **Pass Through-query** is wanneer de aangezogen streams **kanaal**s passeren zonder verdere verwerking. U kunt de volgende live encoders die uitvoer met goede Streaming van multi-bitrate: vorm van het element, Envivio, Cisco.  De volgende live encoders uitvoer RTMP: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast en Tricaster transcoders.  Een live-encoder kan ook een enkele bitsnelheid verzenden naar een kanaal dat niet is ingeschakeld voor live-codering, maar die niet wordt aanbevolen. Bij een aanvraag voor levert Media Services de stroom aan klanten.

    >[AZURE.NOTE] Een Pass Through-methode is de voordeligste manier om te live streaming wanneer u meerdere gebeurtenissen gedurende een lange periode van tijd doet, en u al hebt geïnvesteerd in encoders voor gebouwen. Zie details [prijzen](/pricing/details/media-services/) .
    
    
- Een live encoder op het bedrijf een enkele bitsnelheid verzendt naar het kanaal dat is ingeschakeld voor live-codering met Media-Services in een van de volgende indelingen: RTMP of goede Streaming (gefragmenteerde MP4). RTP (MPEG-TS) wordt ondersteund, mits u een speciale verbinding naar het datacenter Azure. De volgende live encoders met RTMP-uitvoer om te werken met kanalen van dit type zijn bekend: Telestream Wirecast, FMLE. Het kanaal voert live-codering van de binnenkomende één bitsnelheid op een multi-(adaptief) video bitsnelheid. Bij een aanvraag voor levert Media Services de stroom aan klanten.


Vanaf de versie 2.10 van Media-Services als u een kanaal maakt, kunt u opgeven op welke manier u wilt gebruiken voor uw kanaal de invoerstroom ontvangen en of u wilt voor het kanaal voor het uitvoeren van live-codering van de stroom. U hebt twee opties:

- **Geen** (pass-through) – deze waarde opgeven als u gebruiken een op ruimten live encoder die multi-bitsnelheid (Pass Through-stream wilt) wordt uitgevoerd. In dit geval de inkomende stroom doorgegeven aan de uitvoer zonder de codering. Dit is het gedrag van een kanaal voor 2,10 release.  

- **Standaard** : Kies deze waarde als u van plan bent Media Services gebruiken voor het coderen van de live gegevensstroom één bitsnelheid op multi-bitsnelheid. Deze methode is voordeliger voor verticaal schalen snel voor incidentele gebeurtenissen. Let erop dat er een factuuradres impact voor live-codering en u eraan denken moet dat zonder een live uitzending codering in de status "Actief" de facturering tarieven gelden zal.  Het is raadzaam uw actieve kanalen onmiddellijk te stoppen na de live streaming kan om te voorkomen dat de kosten van extra per uur. 

##<a name="comparison-of-channel-types"></a>Vergelijking van de typen kanalen

Volgende tabel vindt u een handleiding voor het vergelijken van de twee typen kanalen, worden ondersteund in Media Services

Functie|Channel Pass Through-query|Standaard-kanaal
---|---|---
Enkele bitsnelheid input gecodeerd naar meerdere bitsnelheid in de cloud|Nee|Ja
Maximale resolutie, aantal lagen|1080p, 8 lagen, 60 + fps|720p, 6 lagen, 30 fps
Invoer-protocollen|RTMP, soepele Streaming|RTMP, soepele Streaming en RTP
Prijs|Zie de [prijzen pagina](/pricing/details/media-services/) en klik op de tab "Live Video"|Zie de [prijzen pagina](/pricing/details/media-services/) 
Maximale uitvoeringstijd|24 x 7|8 uur
Ondersteuning voor het invoegen van leien|Nee|Ja
Ondersteuning voor ad-signalering|Nee|Ja
Pass Through-query CEA 608/708 bijschriften|Ja|Ja
Mogelijkheid om te herstellen van korte hokjes in diervoeders bijdrage|Ja|Nee (kanaal beginnen slating na 6 + seconden zonder invoer)
Ondersteuning voor niet-uniform invoer GOPs|Ja|Nee – invoer moet worden opgelost 2 sec GOPs.
Ondersteuning voor de variabele frame rate input|Ja|Nee – moet dat invoer framesnelheid worden vastgesteld.<br/>Kleine variaties zijn toegestaan, bijvoorbeeld tijdens hoge motion scènes. Maar de encoder kan niet neerzetten op 10 frames per seconde.
Auto-signalen van kanalen als input kanaal gaat verloren|Nee|Na 12 uur, als er geen programma dat wordt uitgevoerd 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Werken met de kanalen die multi-bitrate live gegevensstroom van encoders voor bedrijfsruimten (Pass Through-query ontvangen)

Het volgende diagram ziet de grote delen van het platform AMS die betrokken zijn in de workflow van de **Pass Through-query** .

![Live workflow](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Voor meer informatie, Zie [werken met kanalen die ontvangen Multi-Live bitsnelheid van On-premises Encoders](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Werken met de kanalen die zijn ingeschakeld voor live-codering met Azure Media Services

Het volgende diagram ziet de grote delen van het platform AMS die betrokken zijn in Live Streaming werkstroom waarin een kanaal voor het uitvoeren van live-codering met Media Services is ingeschakeld.

![Live workflow](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Zie [werken met kanalen, die voor het uitvoeren van Live-codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.

##<a name="description-of-a-channel-and-its-related-components"></a>Beschrijving van een kanaal en de bijbehorende onderdelen

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


##<a name="billing-implications"></a>Implicaties voor facturering

Een kanaal begint facturering als overgangen staat in 'Actief' via de API is.  

De volgende tabel ziet u hoe lidstaten kanaal toewijzen aan de facturering van de lidstaten in de API en Azure-portal. De lidstaten zijn enigszins verschillend zijn voor de API en de Portal UX Als u een kanaal heeft de status 'Actief' via de API of de status "Gereed" of "Streaming" in de portal Azure facturering actief zal zijn.

Als u wilt stoppen met het kanaal van u verdere facturering, hebt u het kanaal via de API of in de portal Azure stoppen.
U bent verantwoordelijk voor het stoppen van de kanalen als u klaar bent met het kanaal. Fout bij het stoppen van het kanaal zal leiden tot verdere facturering.

>[AZURE.NOTE]Als u werkt met standaard kanalen, wordt AMS automatisch signalen een kanaal dat is nog steeds in staat 'Uitvoeren' 12 uur na de invoer feed verloren gegaan is en er zijn geen programma's worden uitgevoerd. Echter, u wordt nog steeds gefactureerd voor de tijd die het kanaal in staat 'Uitvoeren was'.

###<a id="states"></a>Kanaal Staten en hoe ze worden toegewezen aan de facturering modus 

De huidige status van een kanaal. Mogelijke waarden zijn:

- **Gestopt**. Dit is de beginstatus van het kanaal nadat het is gemaakt (tenzij die automatisch starten is geselecteerd in de portal.) Geen facturering vindt plaats in deze staat. In deze toestand kunnen de kanaaleigenschappen worden bijgewerkt, maar streaming is niet toegestaan.
- Wordt **gestart**. Het kanaal wordt gestart. Geen facturering vindt plaats in deze staat. Er zijn geen updates of streaming is toegestaan tijdens deze status. Als een fout optreedt, wordt het kanaal de staat gestopt.
- **Uitgevoerd**. Het kanaal is kunnen verwerken live gegevensstromen. Gebruik het nu een factuur. U kunt het kanaal om te voorkomen dat verdere facturering moet stoppen. 
- **Stoppen**. Het kanaal wordt gestopt. Geen facturering vindt plaats in deze tijdelijke status. Er zijn geen updates of streaming is toegestaan tijdens deze status.
- **Verwijderen**. Het kanaal wordt verwijderd. Geen facturering vindt plaats in deze tijdelijke status. Er zijn geen updates of streaming is toegestaan tijdens deze status.

De volgende tabel ziet u hoe lidstaten kanaal toewijzen aan de facturering modus. 
 
Kanaal staat|Portal UI indicatoren|Het factuuradres is?
---|---|---
Starten|Starten|Geen (status)
Wordt uitgevoerd|Gereed (geen programma's)<br/>of<br/>Streaming (ten minste één actieve programma)|JA
Stoppen|Stoppen|Geen (status)
Gestopt|Gestopt|Nee


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Verwante onderwerpen

[Azure mediaservices gefragmenteerde MP4 Live nemen specificatie](media-services-fmp4-live-ingest-overview.md)

[Werken met de kanalen die voor het uitvoeren van Live-codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)

[Werken met de kanalen die Multi-bitrate Live gegevensstroom van On-premises Encoders ontvangen](media-services-live-streaming-with-onprem-encoders.md)

[Quota's en beperkingen](media-services-quotas-and-limitations.md).  

[Media Services-begrippen](media-services-concepts.md)
