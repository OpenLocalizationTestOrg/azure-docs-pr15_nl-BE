<properties
    pageTitle="Azure Media Services Analytics overzicht | Microsoft Azure"
    description="Azure Media Services biedt de public preview van Azure Media Analytics, een verzameling services voor spraak- en visie op de schaal van de onderneming, compliance, beveiliging en mondiaal bereik. Azure Analytics Media services zijn gebouwd met behulp van de kernonderdelen van Azure Media Services platform en dus gereed zijn voor media verwerken op schaal in één dag verwerken. "
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Azure Media Services Analytics overzicht

##<a name="overview"></a>Overzicht

Meer organisaties en ondernemingen worden omvat onderdelen die video als het gewenste medium trainen hun werknemers, hun klanten en zakelijke functies van document. Cloud computing maakt het effectief opslaan, streamen en toegang krijgen tot deze grote mediabestanden, maar naarmate bedrijven hun video Inhoudsbibliotheek groeien, ze moeten een even effectieve middelen voor uitgepakt nieuwe inzichten video om meer herkenbare alias, persoonlijke interacties met hun doelgroepen en zich tot het volgende niveau.

Azure Media Services biedt om deze groeiende behoefte in de markt op te lossen, Media-Analytics, een verzameling van spraak en visie onderdelen (op onderneming schaal, compliance, beveiliging en mondiaal bereik) die het eenvoudiger voor bedrijven en ondernemingen voor het afleiden van inzichten die hun video's sneller. Azure Analytics Media services zijn gebouwd met behulp van de kernonderdelen van Azure Media Services platform en dus gereed zijn voor media verwerken op schaal in één dag verwerken.

Azure Media Analytics kunnen ontwikkelaars snel aan de slag met mogelijkheden voor video op beperkte schaal visie en brengen deze geavanceerde functies in toepassingen. Azure Media Analytics is gebouwd om te worden gebruikt door enterprise-omgevingen met de volledige schaal, compliance, beveiliging en mondiaal bereik vereist voor grote organisaties.

In het volgende diagram ziet u **Analytics Media** en andere belangrijke delen van het platform van Media Services. 

![VoD-werkstroom](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Media-Analytics media processors produceren MP4 of JSON bestanden. Als een Mediaprocessor geproduceerd een MP4-bestand, kunt u het bestand progressief downloaden. Als een Mediaprocessor geproduceerd een JSON-bestand, kunt u het bestand kunt downloaden van de Azure blobopslag. 

## <a name="azure-media-analytics-services"></a>Azure Analytics Media services

- **Indexering** – Azure Media indexeerfunctie kunt u informatie kan worden doorzocht, goed gesloten closed captioning nummers te genereren. **Azure Media indexeerfunctie 2 Preview** Azure Media Services uitgebracht met sneller indexeren en bredere ondersteuning. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees, Portugees en Arabisch. Zie voor uitgebreide informatie en voorbeelden [Process video's met Azure Media indexeerfunctie 2](media-services-process-content-with-indexer2.md)
 
- **Hyperlapse** : Microsoft Hyperlapse is een gevolg van de computer vision onderzoek bij Microsoft Research (MSR), video stabilisatie en de tijd zo snel, verbruikbare, prachtige video's maken van de lange vorm inhoud combineren meer dan 20 jaar. Naast het maken van tijd verstrijkt, kunt u ook Hyperlapse stabiele video's maken van beelden video's opgenomen via mobiele telefoons, en camcorders gebruiken. Zie voor uitgebreide informatie en voorbeelden [Hyperlapse mediabestanden met Azure Media Hyperlapse](media-services-hyperlapse-content.md)
 
- **Detectie van beweging** – u kunt deze service gebruiken voor het detecteren van beweging in een video met een achtergrond voor briefpapier. Dit is ideaal voor klanten die op onjuiste positieve detectie van beweging gebeurtenissen gedetecteerd door toezicht camera's op de surveillance video feeds wilt controleren. Zie [Bewegings-detectie voor Azure Media Analytics](media-services-motion-detection.md)voor uitgebreide informatie en voorbeelden.
 
- **Face detection en emoties gezicht** – met deze service, kunt u de gezichten van mensen en hun emoties, met inbegrip van vriendschap, sadness, verrassing, anger, contempt, vrees, beantwoord en indifference/neutraal detecteren. Dit heeft verschillende nuttige toepassingen in sectoren beschreven, waaronder het verzamelen en analyseren van de reacties van mensen die het bijwonen van een gebeurtenis. Zie voor uitgebreide informatie en voorbeelden [gezicht en detectie van Azure Media Analytics emoties](media-services-face-and-emotion-detection.md).
 
- **Samenvatting van de video** : Video-samenvatting kunt u samenvattingen van lange video's maken door het automatisch selecteren van interessante fragmenten uit de bronvideo. Dit is handig wanneer u een snel overzicht van wat u wilt kunt verwachten in een lange video. Zie voor uitgebreide informatie en voorbeelden [Azure Media Video miniaturen gebruiken voor het maken van een Video-samenvatting](media-services-video-summarization.md)

- **Optische tekenherkenning** - Azure Media Analytics OCR (optische tekenherkenning) kunt u de tekstinhoud in de FLV-bestanden converteren naar bewerkbare, doorzoekbare digitale tekst. Hiermee kunt u voor het automatiseren van de winning van zinvolle metagegevens van het videosignaal van uw media.
 
- **Schaalbare gezicht redactie** - **Azure Media Redactor** is een Azure Media Analytics MP biedt schaalbare gezicht redactie in de cloud. Redactie gezicht kunt u uw video te vervagen vlakken van geselecteerde gebruikers wijzigen. U kunt de redactie gezicht service gebruiken in scenario's voor openbare veiligheid en de nieuws media. Een paar minuten beeldmateriaal met meerdere vlakken uren Redigeren handmatig kunnen nemen, maar met deze service vereist de redactie gezicht proces een paar eenvoudige stappen. Zie [Dit](media-services-face-redaction.md) artikel voor meer informatie.

 
## <a name="common-scenarios"></a>Gebruikelijke scenario 's

Hieronder vindt u een aantal scenario's waar Azure Media Analytics kunnen organisaties en ondernemingen in verschillende branches vindt nieuwe inzichten van de video meer persoonlijke publiek en bewaken van de werknemer te maken, evenals grote hoeveelheid video-inhoud efficiënter te beheren:

- **Call centers** – Even met de komst van sociale media, klant call centers nog steeds een groot percentage van service klanttransacties vergemakkelijken. In dit audio gegevens gecodeerd is een schat aan informatie over klanten die kunnen worden geanalyseerd om de producten en ook de trein call center werknemers hogere klanttevredenheid te bereiken. Met behulp van Azure Media indexeerfunctie kunnen klanten tekst en maken een zoekindex en dashboards te pakken intelligence rond de meest voorkomende hierover een klacht, bron van hierover een klacht en dergelijke andere relevante gegevens extraheren.

- **Gebruiker gegenereerde inhoud matiging** – van nieuws media-uitgevers aan politie-afdelingen in veel organisaties hebben openbare gerichte portals waar ze UGC media, zoals video's en afbeeldingen accepteren. Het volume van de inhoud kunt oploopt als gevolg van onverwachte gebeurtenissen. In deze scenario's, is het bijna onmogelijk uit te voeren van een effectief handmatige controle van de inhoud op geschiktheid. Klanten vertrouwen op de service content matiging te concentreren op de inhoud die van toepassing is.

- **Toezicht** - de groei van de IP-camera's, er is een explosie van video's voor toezicht. Handmatig controleren video surveillance is tijd intensief en gevoelig voor menselijke fouten. Azure Media Analytics biedt verschillende onderdelen zoals motion detection, face detection en Hyperlapse het proces bekijken, beheren en maken van derivaten gemakkelijker te maken.

## <a name="media-services-analytics-media-processors"></a>Media Services Analytics Media-Processors 

Deze sectie geeft een overzicht van alle de Media Services Analytics Media Processors (MP) en wordt getoond hoe .NET of REST kunt een MP-object.

### <a name="mp-names"></a>MP-namen


- Azure Media indexeerfunctie 2 Preview
- De indexeerfunctie Azure Media
- Hyperlapse Azure Media
- Azure Media gezicht Detector
- Azure Media Motion Detector
- Video Azure mediaminiaturen
- Azure Media OCR

### <a name="net"></a>.NET

De volgende functie wordt een van de opgegeven namen van MP en een MP-object als resultaat.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


## <a name="rest"></a>REST

Aanvraag:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Antwoord:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Demo 's

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Verwante artikelen

[Media Services Analytics aankondiging](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
