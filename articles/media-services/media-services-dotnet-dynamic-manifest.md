<properties 
    pageTitle="Filters maken met Azure mediaservices SDK voor .NET" 
    description="In dit onderwerp wordt beschreven hoe om filters te maken zodat de client om bepaalde delen van de stream van een stream gebruiken kan. Dynamische manifesten te bereiken deze selectieve streaming maken Media Services" 
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
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Filters maken met Azure mediaservices SDK voor .NET

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [REST](media-services-rest-dynamic-manifest.md)

Media Services kunt vanaf versie 2.11, u filters voor de activa definiëren. Deze filters worden serverregels waardoor uw klanten kiezen voor dingen zoals: afspelen slechts een gedeelte van een video (in plaats van de hele video afspelen), of alleen een subset van vertoningen van audio en video-apparaat van uw klant kan verwerken (in plaats van alle vertoningen die gekoppeld aan het activum zijn) opgeven. Met deze filterfunctie van uw activa wordt bereikt door **Dynamische Manifest**s die zijn gemaakt op verzoek van de klant voor het stroomsgewijs verzenden van een video op basis van een opgegeven filter (s).

Zie voor meer gedetailleerde informatie over de filters en dynamische Manifest, [dynamische manifesten overzicht](media-services-dynamic-manifest-overview.md).

In dit onderwerp ziet u hoe met behulp van Media Services .NET SDK maken, bijwerken en verwijderen van filters. 


Opmerking dat als u een filter hebt bijgewerkt, kunnen tot maximaal 2 minuten voor het streamen van eindpunt om de regels te vernieuwen. Als de inhoud is betekend met dit filter (en in het cachegeheugen van proxy's en CDN caches) bijwerken van dit filter kan leiden tot storingen van de speler. Het is aanbevolen om de cache leeg na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter. 

##<a name="types-used-to-create-filters"></a>Die worden gebruikt voor het maken van filters

De volgende typen worden gebruikt bij het maken van filters: 

- **IStreamingFilter**.  Dit type is gebaseerd op de volgende REST API- [Filter](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Dit type is gebaseerd op de volgende REST API- [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Dit type is gebaseerd op de volgende REST API- [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** en **IFilterTrackPropertyCondition**. Deze typen zijn gebaseerd op de volgende REST API's [FilterTrackSelect en FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Globale filters maken, bijwerken of lezen/verwijderen

De volgende code toont hoe u met .NET maken, bijwerken, lezen en verwijderen van filters actief.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Filters actief maken, bijwerken of lezen/verwijderen

De volgende code toont hoe u met .NET maken, bijwerken, lezen en verwijderen van filters actief.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Streaming URL's die met filters maken

Zie [Inhoud leveren aan klanten-overzicht](media-services-deliver-content-overview.md)voor meer informatie over het publiceren en leveren van uw activa.


De volgende voorbeelden laten zien hoe filters toevoegen aan uw streaming URL's.

**MPEG-STREEPJE** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Goede Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HARDE SCHIJVEN**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zie ook 

[Overzicht van dynamische manifesten](media-services-dynamic-manifest-overview.md)
 

