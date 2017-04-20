<properties 
    pageTitle="Het maken van een Mediaprocessor | Microsoft Azure" 
    description="Informatie over het maken van een onderdeel van de media-processor als u wilt coderen, indeling converteren, coderen of decoderen van media-inhoud voor Azure Media Services. Codevoorbeelden zijn geschreven in C# en de Media Services SDK voor .NET." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Procedure: een exemplaar van de Processor Media ophalen

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [REST](media-services-rest-get-media-processor.md)


##<a name="overview"></a>Overzicht

In Media Services die een Mediaprocessor een onderdeel dat zorgt voor verwerking van specifieke taken is, zoals codering, conversie, codering of decodering van media-inhoud van de indeling. Meestal maakt u een Mediaprocessor bij het maken van een taak wilt coderen, coderen of de inhoud van media-indeling converteren.

De volgende tabel bevat de naam en beschrijving van elke beschikbare media-processor.

Mediumnaam Processor|Beschrijving|Meer informatie
---|---|---
Media Encoder-standaard|Standaard mogelijkheden biedt voor codering op aanvraag. |[Overzicht en vergelijking van Azure op vraag Media Encoders](media-services-encode-asset.md)
Media Encoder Premium Workflow|Kunt u Media Encoder Premium Workflow codering taken uitvoeren.|[Overzicht en vergelijking van Azure op vraag Media Encoders](media-services-encode-asset.md)
De indexeerfunctie Azure Media| Hiermee kunt u media-bestanden en inhoud doorzoekbaar maken als gesloten closed captioning tracks en trefwoorden te genereren.|[De indexeerfunctie Azure Media](media-services-index-content.md)
Azure Media Hyperlapse (voorbeeld)|Hiermee kunt u de "schokken" in uw video met video stabilisatie vloeiend. U kunt ook uw inhoud in een clip verbruikbare versnellen.|[Hyperlapse Azure Media](media-services-hyperlapse-content.md)
Azure Media Encoder|Afgeschreven
Opslag-decodering| Afgeschreven|
Azure Media Packager|Afgeschreven|
Azure Media coderen|Afgeschreven|

##<a name="get-media-processor"></a>Ophalen Mediaprocessor

De volgende methode ziet u hoe u een exemplaar van de processor media. In het voorbeeld wordt ervan uitgegaan dat het gebruik van een variabele op moduleniveau **_context** om te verwijzen naar de servercontext zoals beschreven in de sectie met de naam [procedure: verbinding maken met Media Services via een programma](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Volgende stappen

Als u weet hoe u een exemplaar van de processor media, gaat u naar het onderwerp voor [het coderen van een actief](media-services-dotnet-encode-with-media-encoder-standard.md) die hoe u met Media Encoder standaard weergeven voor het coderen van een actief.


