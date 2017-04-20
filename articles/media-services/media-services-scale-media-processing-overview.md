<properties
    pageTitle="Schalen Verwerkingsoverzicht Media | Microsoft Azure"
    description="Dit onderwerp is een overzicht van de schaal Media behandelen met Azure Media Services."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Media Verwerkingsoverzicht schalen

Deze pagina geeft een overzicht van hoe en waarom voor het schalen van media-verwerking. 

## <a name="overview"></a>Overzicht

Een Media-Services-account is gekoppeld aan gereserveerde eenheid het Type, bepaalt de snelheid waarmee uw media verwerking worden verwerkt. U kunt kiezen tussen de volgende gereserveerde eenheid: **S1**, **S2**of **S3**. Bijvoorbeeld dat dezelfde codering taak sneller wordt uitgevoerd wanneer u de **S2** gereserveerd eenheid type vergelijken met het type **S1** . Zie voor meer informatie de [Gereserveerde eenheidstypen](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Naast het opgeven van het eenheidstype gereserveerd, kunt u om uw account inrichten met gereserveerde eenheden. Het aantal eenheden van ingerichte gereserveerde bepaalt het aantal media-taken die tegelijkertijd kunnen worden verwerkt in een bepaalde account. Bijvoorbeeld, als uw account vijf eenheden gereserveerd heeft, vijf media taken uitvoert, gelijktijdig zo lang als er zijn taken moeten worden verwerkt. De resterende taken in de wachtrij moeten wachten en zal krijgen trok voor het sequentieel worden verwerkt als een actieve taak is voltooid. Als een account niet alle gereserveerde eenheden worden ingericht hoeft, zullen vervolgens taken worden opgenomen automatisch opeenvolgend genummerd. In dit geval wordt hangt de wachttijd tussen een taak eindigt en de volgende begin de beschikbaarheid van resources in het systeem.

## <a name="choosing-between-different-reserved-unit-types"></a>Kiezen tussen verschillende gereserveerde eenheid

De volgende tabel kunt u de beslissing moet bij de keuze tussen verschillende snelheden voor de codering. Ook hier enkele gevallen van benchmark en bevat SAS-URL's die u gebruiken kunt voor het downloaden van video's waarop u uw eigen tests kunt uitvoeren:

Scenario 's|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Beoogde use-case| Eén Codering bitsnelheid. <br/>Bestanden op een SD of onder resoluties, tijd geen gevoelige, lage kosten.|Enkele bitsnelheid en meerdere Codering bitsnelheid.<br/>Normaal gebruik voor zowel SD als HD-codering. |Enkele bitsnelheid en meerdere Codering bitsnelheid.<br/>Volledige HD en 4K resolutie video's. Tijd gevoelig, snellere afwikkeling codering. 
Benchmark|[Invoerbestand: 5 minuten lang 640x360p op 29,97 frames per seconde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Codering op een enkele bitsnelheid MP4-bestand, met de resolutie, duurt ongeveer 11 minuten.|[Invoerbestand: 5 minuten lang 1280x720p op 29,97 frames per seconde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Codering met "H264 één bitsnelheid 720p" vooraf duurt ongeveer 5 minuten.<br/><br/>Codering met "H264 meerdere bitsnelheid 720p" voorinstelling duurt ongeveer 11,5 minuten.|[Invoerbestand: 5 minuten lang 1920x1080p op 29,97 frames per seconde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Codering met "H264 één Bitrate 1080p" vooraf neemt ongeveer 2,7 minuten.<br/><br/>Codering met "H264 meerdere Bitrate 1080p" voorinstelling duurt ongeveer 5,7 minuten.

##<a name="considerations"></a>Overwegingen met betrekking tot

>[AZURE.IMPORTANT] Bekijk de overwegingen die in deze sectie.  

- Gereserveerde eenheden werken voor alle media-verwerking, met inbegrip van taken met behulp van Azure Media indexeerfunctie indexing parallelizing.  Echter, in tegenstelling tot de codering, indexeertaken doen niet krijgen sneller verwerkt met sneller gereserveerde eenheden.

- Als de gedeelde toepassingen, zonder alle gereserveerde eenheden hebben vervolgens coderen taken dezelfde prestaties als bij RUs S1. Er is echter geen bovengrens voor de tijd die uw taken besteden kunnen in een wachtrij staat en op een bepaald moment één taak is uitgevoerd.

- De volgende datacenters bieden niet het eenheidstype **S2** gereserveerd: Zuid-Brazilië, India, West Centraal India en Zuid-India.

- De volgende datacenters bieden niet het eenheidstype **S3** gereserveerd: Brazilië, Zuid, West, India, India centraal.

- Het grootste aantal eenheden dat is opgegeven voor de periode van 24 uur wordt gebruikt bij het berekenen van de kosten.


##<a name="quotas-and-limitations"></a>Quota's en beperkingen

Zie voor meer informatie over quota en beperkingen en het openen van een ticket voor de ondersteuning van [quota's en beperkingen](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Volgende stap

De taak voor verwerking van schaal media met een van deze technologieën bereiken: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-scale-media-processing.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
