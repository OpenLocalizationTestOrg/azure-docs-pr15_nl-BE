<properties 
    pageTitle="Release-opmerkingen voor Media Services | Microsoft Azure" 
    description="Media Services-Release-opmerkingen" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Azure Media Services release-opmerkingen

Deze release-opmerkingen samenvatten wijzigingen ten opzichte van eerdere versies en bekende problemen.

>[AZURE.NOTE] We willen horen van onze klanten en zich richten op het oplossen van problemen die invloed hebben op u. Boeken in het [MSDN-Forum van Azure Media Services]om een probleem melden of vragen.


##<a id="issues"></a>Bekende problemen

### <a id="general_issues"></a>Algemene problemen met Media-Services

Probleem|Beschrijving
---|---
Enkele veelvoorkomende HTTP-headers zijn niet beschikbaar in de REST API.|Als u Media Services toepassingen met de REST API ontwikkelen, vindt u dat sommige veelgebruikte HTTP-headervelden (CLIENT-aanvraag-ID, met inbegrip van de aanvraag-ID en RETURN-CLIENT-aanvraag-ID) worden niet ondersteund. De koppen wordt in een toekomstige update toegevoegd.
Percentage codering is niet toegestaan.|De waarde van de eigenschap IAssetFile.Name van Media Services wordt gebruikt bij het maken van URL's voor de stroomsgewijze inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Om deze reden is procent codering niet toegestaan. De waarde van de eigenschap **Name** geen van de volgende [tekens voor percentage-codering-gereserveerd](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Bovendien kunnen alleen er een '.' voor de bestandsextensie.
De methode ListBlobs die deel uitmaakt van de SDK van Azure opslag versie 3.x werkt niet.|Media Services wordt gegenereerd op basis van de versie [2012-02-12](http://msdn.microsoft.com/library/azure/dn592123.aspx) SAS-URL's. Als u wilt met behulp van Azure opslag SDK lijst BLOB's in een blob-container, gebruikt de methode [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) , die deel uitmaakt van Azure opslag SDK versie 2.x. De methode ListBlobs, die deel uitmaakt van de SDK van Azure opslag versie 3.x mislukt.
Media Services mechanisme bandbreedtebeperking beperkt het gebruik van bronnen voor toepassingen die buitensporig verzoek naar de service. De service kan de Service niet beschikbaar is (503) HTTP-statuscode retourneren.|Zie de beschrijving van de 503 HTTP-statuscode in het onderwerp van de [Foutcodes van Azure Media Services](http://msdn.microsoft.com/library/azure/dn168949.aspx) voor meer informatie.
Bij het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer worden geretourneerd omdat openbare REST v2 queryresultaten 1000 resultaten beperkt. | U moet gebruiken, **overslaan** en **nemen** (.NET) / **top** (REST) als beschreven in [dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [REST API voorbeeld](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
Sommige clients kunnen komen voor een herhaalde label probleem in het manifest goede Streaming.|Zie voor meer informatie [in deze](media-services-deliver-content-overview.md#known-issues) sectie.
Azure Media Services .NET SDK objecten kunnen niet worden geserialiseerd en werken daardoor niet met Azure Caching.|Als u de SDK-AssetCollection-object toe te voegen aan de Azure Caching serialiseren, wordt een uitzondering gegenereerd.
Codering taken mislukken met een tekenreeks voor het bericht "fase: DownloadFile. Code: System.NullReferenceException ".|De doorsnee werkstroom codering is input video bestanden uploaden naar een input-element en een of meer taken van codering in te dienen voor de invoer activa, zonder verdere invoer van activa. Echter, als u de invoer activa (bijvoorbeeld door het toevoegen/verwijderen/hernoemen van bestanden binnen het activum) wijzigt, vervolgens latere taken mislukken met een DownloadFile-fout. De oplossing is het verwijderen van de activa van de invoer en input bestanden naar een nieuw activum opnieuw te uploaden. 

##<a id="rest_version_history"></a>REST API versiegeschiedenis

Zie voor meer informatie over de geschiedenis van Media Services REST API versie [Azure Media Services REST API Reference].

##<a id="july_changes16"></a>Juli 2016 Release

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Updates van het manifest bestand (*. ISM) gegenereerd door codering taken

Wanneer een taak codering wordt ingediend bij de Media Encoder standaard of Azure Media Encoder, de codering taak genereert een [manifest streaming-bestand](media-services-deliver-content-overview.md) (* .ism) bestand in de uitvoer van activa. Met de meest recente versie van de service, is de syntaxis van dit manifest streaming-bestand bijgewerkt.

>[AZURE.NOTE]De syntaxis van het bestand met streaming manifest (.ism) is gereserveerd voor intern gebruik en kan worden gewijzigd in toekomstige versies. Neem niet wijzigen of de inhoud van dit bestand bewerken.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Nieuw manifest voor een client (*. Bestand ISMC) wordt gegenereerd in de uitvoer actief wanneer een taak codering een of meer MP4-bestanden wordt

Beginnen met de meest recente versie van de service na de voltooiing van een taak voor tekstcodering die een genereert meer MP4-bestanden, de uitvoer bevat activum ook een streaming bestand voor client-manifest (*.ismc). Het bestand .ismc verbetert de prestaties van dynamische streaming. 

>[AZURE.NOTE]De syntaxis van het bestand client manifest (.ismc) is gereserveerd voor intern gebruik en kan worden gewijzigd in toekomstige versies. Neem niet wijzigen of de inhoud van dit bestand bewerken.

Zie [deze](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blog voor meer informatie.

### <a name="known-issues"></a>Bekende problemen

Sommige clients kunnen afkomstig zijn van een repeat-tag probleem in het manifest goede Streaming aross. Zie voor meer informatie [in deze](media-services-deliver-content-overview.md#known-issues) sectie.

##<a id="apr_changes16"></a>April 2016 Release

### <a name="azure-media-analytics"></a>Azure Media Analytics

Media-Analytics Azure Azure Media Servces geïntroduceerd voor krachtige video intelligence. Zie [Overzicht van Azure Media Services Analytics](media-services-analytics-overview.md)voor gedetailleerde informatie.

### <a name="apple-fairplay-preview"></a>Apple FairPlay (voorbeeld)

Azure Media Services kunt u nu uw HTTP Live Streaming (HLS) past Apple FairPlay-inhoud dynamisch te coderen. Ook kunt u AMS licentie bezorgservice leveren FairPlay licenties aan clients. Voor meer gedetailleerde informatie, Zie [Gebruik Azure Media Services voor het stroomsgewijs verzenden van de HLS inhoud beveiligd met Apple FairPlay ](media-services-protect-hls-with-fairplay.md).
  
##<a id="feb_changes16"></a>Februari 2016 Release

De nieuwste versie van Azure Media Services SDK voor .NET (3.5.3) bevat een Widevine gerelateerde bug fix. Het probleem is: AssetDeliveryPolicy kan niet opnieuw worden gebruikt voor meerdere activa die zijn gecodeerd met Widevine. Als onderdeel van deze bug fix de volgende eigenschap is toegevoegd aan de SDK: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Januari 2016 Release

Om verwarring met de namen van de Encoder codering gereserveerde eenheden gewijzigd.

De Basic, Standard en Premium codering gereserveerde eenheden worden gewijzigd aan S1, S2 en S3 eenheden, respectievelijk gereserveerd.  Klanten met Basic Encoding RUs vandaag S1 ziet als het label in Azure Portal (en in de stuklijst) tijdens het Standard en Premium etiketten S2 en S3 respectievelijk zien. 

##<a id="dec_changes_15"></a>December 2015 Release

De SDK Azure-team een nieuwe versie van de [SDK voor PHP Azure](http://github.com/Azure/azure-sdk-for-php) -pakket updates en nieuwe functies voor Microsoft Azure Media Services bevat gepubliceerd. In het bijzonder de Azure Media Services SDK voor PHP biedt nu ondersteuning voor de nieuwste functies van [beveiligde inhoud](media-services-content-protection-overview.md) : dynamische versleuteling met AES en DRM (PlayReady en Widevine) met en zonder beperking Token. Ondersteunt ook schalen [Codering eenheden](media-services-dotnet-encoding-units.md).

Zie voor meer informatie:

- De blog [Microsoft Azure Media Services SDK voor PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) .
- De volgende [codevoorbeelden](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) kunt u snel de slag:
    - **vodworkflow_aes.php**: dit is een PHP-bestand dat met behulp van dynamische Encryption AES 128 en Key leveringsservice. Het is gebaseerd op de .NET voorbeelden in [Dit](media-services-protect-with-aes128.md) artikel wordt uitgelegd.
    - **vodworkflow_aes.php**: dit is een PHP-bestand waarin het gebruik van dynamische PlayReady-codering en bezorgservice licentie. Het is gebaseerd op de .NET voorbeelden in [Dit](media-services-protect-with-drm.md) artikel wordt uitgelegd.
    - **scale_encoding_units.php**: dit is een PHP-bestand, waarin wordt uitgelegd hoe u codering gereserveerde eenheid schalen.


##<a id="nov_changes_15"></a>Versie november 2015

Azure Media Services biedt nu Google Widevine licentie bezorgservice in de cloud. Raadpleeg [deze aankondiging blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/)voor meer details. Zie ook [deze zelfstudie](media-services-protect-with-drm.md) en [opslagplaats van GitHub](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Let erop dat Widevine licentie levering diensten van Azure Media Services wordt uitgevoerd in de voorvertoning. Zie [deze blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/)voor meer informatie.

##<a id="oct_changes_15"></a>Versie oktober 2015

Azure Media Services (AMS) is in de volgende datacenters nu live: Zuid-Brazilië, India, West, Zuid-India en India centraal. Nu de Azure klassieke Portal gebruiken om [Media Service accounts maken](media-services-portal-create-account.md) en uitvoeren van verschillende taken beschreven [hier](https://azure.microsoft.com/documentation/services/media-services/). Live-codering is niet ingeschakeld in deze datacenters. Niet alle typen codering gereserveerde eenheden zijn bovendien beschikbaar in deze datacenters.

- Brazilië South: Alleen standaard en Basic Encoding gereserveerde eenheden beschikbaar
- West India, Zuid-India en India centraal: alleen elementaire codering gereserveerde eenheden beschikbaar zijn.


##<a id="september_changes_15"></a>Versie september 2015 

- AMS biedt nu de mogelijkheid om zowel Video-On-Demand (VOD) en Live gegevensstromen met Widevine modulaire DRM-technologie te beschermen. U kunt de volgende delivery services partners kunt u licenties Widevine leveren: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Zie [deze blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)voor meer informatie.

    Kunt u [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (te beginnen met de versie 3.5.1) of REST API voor het configureren van uw AssetDeliveryConfiguration voor het gebruik van Widevine.  

- AMS toegevoegde ondersteuning voor Apple ProRes video's. U kunt de QuickTime video's bronbestanden die Apple ProRes of andere codecs gebruiken nu uploaden. Zie [deze blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/)voor meer informatie.

- Nu kunt u Media Encoder standaard wilt subtropische knippen en live archief uitpakken. Zie [deze blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/)voor meer informatie.

- De volgende updates filteren zijn aangebracht: 

    - U kunt nu Apple HTTP Live Streaming (HLS) past opmaak met alleen audio filter. Deze update kunt u alleen audio track verwijderen door op te geven (alleen audio = false) in de URL.
    - Bij het definiëren van filters voor de activa, je hebt nu kunnen meerdere combineren (maximaal 3) filters in een enkele URL.

    Zie [deze](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog voor meer informatie.

- AMS ondersteunt nu ik Frames in HLS v4. Ondersteuning voor I-Frame optimaliseert vooruit- of terugspoelen bewerkingen. Standaard zijn alle HLS v4 uitgangen afspeellijst I-Frame (EXT-X-I-FRAME-STREAM-INF).
 
    Zie [deze](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog voor meer informatie.

##<a id="august_changes_15"></a>Versie augustus 2015

- Azure Media Services SDK voor Java V0.8.0 release en nieuwe monsters zijn nu beschikbaar. Zie voor meer informatie:

    - [Blogbericht](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Opslagplaats voor Java-voorbeelden](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Azure Media Player update met ondersteuning voor meerdere audio-stream. Zie voor meer informatie:
    - [Blogbericht](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>Juli 2015 Release

- De algemene beschikbaarheid van Media Encoder standaard aangekondigd. Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    Media Encoder standaard gebruikt de voorinstellingen die in [deze](http://go.microsoft.com/fwlink/?LinkId=618336) sectie worden beschreven. Houd er rekening mee dat wanneer u met een voorinstelling voor 4k wordt gecodeerd, u het eenheidstype **Premium** gereserveerd ontvangt. Zie [schaal codering](media-services-scale-media-processing-overview.md)voor meer informatie.
- Live Real-time bijschriften met Azure Media Services en de speler. Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>Updates voor .NET SDK-mediaservices

Azure Media Services .NET SDK is nu versie 3.4.0.0. De volgende functies is in deze release toegevoegd:  

- Geïmplementeerde ondersteuning voor live archief. Houd er rekening mee dat u een actief die een live archief bevat niet downloaden.
- Geïmplementeerde ondersteuning voor dynamische filters.
- Geïmplementeerde functionaliteit waarmee gebruikers opslag container houden tijdens het verwijderen van de activa.
- Correcties die betrekking hebben op het beleid in de kanalen opnieuw.
- **Media Encoder Premium Workflow**ingeschakeld.

##<a id="june_changes_15"></a>Versie juni 2015

###<a name="media-services-net-sdk-updates"></a>Updates voor .NET SDK-mediaservices

Azure Media Services .NET SDK is nu versie 3.3.0.0. De volgende functies is in deze release toegevoegd:  

- ondersteuning voor OpenId verbinding Discovery-spec
- ondersteuning voor het verwerken van toetsen rollover identity provider aan. 

Als u een id-provider waarmee verbinding OpenID discovery-document (zoals de volgende providers: Azure Active Directory, Google, televergaderingen), kunt u Media Services Azure handtekeningsleutels verkrijgen voor validatie van token van OpenID JWT discovery spec opdracht geven. 

Zie [Met behulp van Json Web sleutels van discovery spec OpenID verbinding werken met JWT token verificatie in Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/)voor meer informatie.


##<a id="may_changes_15"></a>Release in mei 2015

Aankondiging van de volgende nieuwe functies:

- [Een voorbeeld van Live-codering met Media-Services](media-services-manage-live-encoder-enabled-channels.md)
- [Dynamische manifest](media-services-dynamic-manifest-overview.md)
- [Een voorbeeld van Azure Media Hyperlapse media-processor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>Versie april 2015

 ###<a name="general-media-services-updates"></a>Updates voor algemene Media Services

- [Azure mediaspeler aangekondigd](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Beginnen met Media Services REST 2.10, kanalen die zijn geconfigureerd voor het nemen van een protocol RTMP worden gemaakt met de primaire en secundaire URL's nemen. Zie voor meer informatie, [kanaal consumptie van configuraties](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Azure Media indexering updates
- Ondersteuning voor Spaans
- Nieuwe configuratie-XML-indeling

Zie [deze blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/)voor meer informatie.
###<a name="media-services-net-sdk-updates"></a>Updates voor .NET SDK-mediaservices

Azure Media Services .NET SDK is nu versie 3.2.0.0.

Dit zijn enkele van de klant naar updates:

- **Wijziging breken**: **TokenRestrictionTemplate.Issuer** en **TokenRestrictionTemplate.Audience** van een tekenreekstype worden gewijzigd.
- Updates met betrekking tot het maken van aangepaste beleid opnieuw.
- Correcties die verband houden met het uploaden/downloaden van bestanden.
- De klasse **MediaServicesCredentials** accepteert nu de primaire en secundaire toegang controle-eindpunt te verifiëren aan de hand.



##<a id="march_changes_15"></a>Release van maart 2015

### <a name="general-media-services-updates"></a>Updates voor algemene Media Services

- Media Services biedt nu Azure CDN-integratie. De eigenschap **CdnEnabled** is ter ondersteuning van de integratie, toegevoegd aan de **StreamingEndpoint**.  **CdnEnabled** kan worden gebruikt met REST API's vanaf versie 2.9 (Zie [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)voor meer informatie).  **CdnEnabled** kan worden gebruikt met .NET SDK vanaf versie 3.1.0.2 (Zie [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)voor meer informatie).
- Aankondiging van **Media Encoder Premium Workflow**. Zie [Inleiding tot Premium codering in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)voor meer informatie.
 


##<a id="february_changes_15"></a>Versie februari 2015

### <a name="general-media-services-updates"></a>Updates voor algemene Media Services

Media Services REST API is nu versie 2.9. Vanaf deze versie, kunt u de integratie van de Azure CDN met streaming eindpunten. Zie [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx)voor meer informatie.

##<a id="january_changes_15"></a>Versie januari 2015

### <a name="general-media-services-updates"></a>Updates voor algemene Media Services

Aankondiging van de algemene beschikbaarheid (GA) van de beveiligde inhoud met dynamische codering. Zie voor meer informatie [Azure Media Services verbetert de streaming-beveiliging met de algemene beschikbaarheid van DRM-technologie](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

###<a name="media-services-net-sdk-updates"></a>Updates voor .NET SDK-mediaservices

Azure Media Services .NET SDK is nu versie 3.1.0.1.

Deze versie van de standaard Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate constructor als verouderd gemarkeerd. De constructor new duurt TokenType als argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>Versie december 2014

###<a name="general-media-services-updates"></a>Updates voor algemene Media Services

- Sommige updates en nieuwe functies zijn toegevoegd aan de processor Azure indexeerfunctie Media. Zie [Azure Media indexeerfunctie versie 1.1.6.7 Release Notes](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/)voor meer informatie.
- Toegevoegd een nieuwe REST API waarmee u kunt werken met codering eenheden gereserveerd: [EncodingReservedUnitType met de REST](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- CORS toegevoegde ondersteuning voor belangrijke bezorgservice.
- Prestatieverbeteringen van het zoeken van vergunning beleidsopties werden gedaan.
- In het datacenter van China, de [Sleutel levering URL](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) is nu per klant (net als in andere datacenters).
- Toegevoegde HLS auto doel duur. Bij het uitvoeren van live streaming, is dynamisch HLS altijd verpakt. Standaard berekend Media Services automatisch HLS segment verpakking verhouding (FragmentsPerSegment) op basis van het interval hoofdframe (KeyFrameInterval), ook wel aangeduid als groep van afbeeldingen – GOP die wordt ontvangen van het coderingsprogramma Live. Zie [werken met Azure Media Services Live Streaming]voor meer informatie.
 
###<a name="media-services-net-sdk-updates"></a>Updates voor .NET SDK-mediaservices

- [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) is nu versie 3.1.0.0.
- De afhankelijkheid van de SDK voor .net een upgrade naar .NET-Framework 4.5.
- Een nieuwe API waarmee u kunt codering gereserveerde eenheden bijwerken toegevoegd. Zie [gereserveerde eenheidstype bijwerken en codering RUs verhogen met behulp van .NET](http://msdn.microsoft.com/library/azure/jj129582.aspx)voor meer informatie.
- Toegevoegde JWT (JSON Web Token) ondersteuning voor token-verificatie. Zie [JWT token verificatie in Azure Media Services en dynamische codering](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)voor meer informatie.
- Toegevoegde relatieve verschuivingen voor BeginDate en ExpirationDate in de sjabloon PlayReady licentie.


##<a id="november_changes_14"></a>Versie november 2014

- Media-Services kunt u nu een live inhoud voor soepele Streaming (FMP4) nemen via een SSL-verbinding. Om op te nemen via SSL, zorg ervoor dat de URL ingest naar HTTPS.  Voor meer informatie over live streaming, kunt u [werken met Azure Media Services Live Streaming].
- Houd er rekening mee dat op dit moment u niet kunt een live gegevensstroom RTMP via een SSL-verbinding nemen.
- U kunt ook uw inhoud stroomsgewijs via een SSL-verbinding. Hiervoor moet dat uw streaming URL's beginnen met HTTPS.
- Houd er rekening mee dat u alleen via SSL streamen kunt als het streaming eindpunt van waaruit u de inhoud bezorgen na 10 September 2014 is gemaakt. Als uw streaming URL's zijn gebaseerd op de streaming eindpunten die na 10 September zijn gemaakt, bevat de URL 'streaming.mediaservices.windows.net' (nieuwe indeling). Streaming URL's met 'origin.mediaservices.windows.net' (oude indeling) bieden geen ondersteuning voor SSL. Als de URL in de oude indeling is en u streamen via SSL wilt, [Maak een nieuwe streaming eindpunt](media-services-portal-manage-streaming-endpoints.md). Gemaakt op basis van het nieuwe endpoint streaming URL's gebruiken voor het stroomsgewijs verzenden van inhoud via SSL.

##<a id="october_changes_14"></a>Versie oktober 2014

### <a id="new_encoder_release"></a>Media Services Encoder Release

Aankondiging van de nieuwe versie van Media Services Azure Media Encoder. Met de nieuwste Azure Media Encoder alleen in rekening u gebracht worden uitvoer GB, maar anders de nieuwe encoder is compatibel met het eerder gebruikte coderingsprogramma functie. Voor meer informatie over [Prijzen Details van Media Services]).

### <a id="oct_sdk"></a>Mediaservices SDK voor .NET 

Media Services SDK voor .NET extensies is nu versie 2.0.0.3.

Media Services SDK voor .NET is nu versie 3.0.0.8.

De volgende wijzigingen zijn aangebracht:

- Refactoring in opnieuw beleid klassen.
- Tekenreeks van de gebruikersagent aan HTTP-aanvraagheaders toevoegen.
- Nuget terugzetten build stap toevoegen.
- Vaststelling van tests scenario voor het gebruik van x509 cert uit de opslagplaats.
- Valideren van de instellingen bij het bijwerken van kanaal en streaming einde.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Voorbeelden van Media Services host nieuwe GitHub opslagplaats

Voorbeelden bevinden zich in de [opslagplaats van Azure Media Services monsters GitHub](https://github.com/Azure/Azure-Media-Services-Samples).


##<a id="september_changes_14"></a>Versie september 2014

Metagegevens voor Media Services REST is nu versie 2.7. Zie [Azure Media Services REST API Reference]voor meer informatie over de meest recente updates voor de REST.

Er is nu Media Services SDK voor .NET versie 3.0.0.7
 
### <a id="sept_14_breaking_changes"></a>Overtredingen van de wijzigingen

* **Oorsprong** is gewijzigd in [StreamingEndpoint].
* Een wijziging in het standaardgedrag bij het gebruik van de **Klassieke Azure-Portal** voor het coderen en vervolgens publiceren MP4-bestanden.

Eerder, wanneer de Azure klassieke Portal gebruiken voor het publiceren van een enkel bestand MP4 video activum een SAS-URL moet worden gemaakt (SAS URL's kunt u downloaden van de video van een blob-opslag). Op dit moment als u de klassieke Azure-Portal voor het coderen en vervolgens publiceren van een enkel bestand MP4 video actief, de gegenereerde URL verwijst naar een Azure Media Services stroomsgewijze eindpunt.  Deze wijziging heeft geen invloed op de MP4-video's rechtstreeks uploaden naar Media Services en gepubliceerd zonder codering Azure Media Services.

Op dit moment hebt u de volgende twee opties het probleem op te lossen.

* Streaming units inschakelen en het .mp4 actief als een goede streaming presentatie streamen met dynamische verpakking.

* Maak een url SAS de .mp4 downloaden (of geleidelijk afspelen). Zie [Inhoud leveren]voor meer informatie over het maken van een SAS-locator.


### <a id="sept_14_GA_changes"></a>Nieuwe functies/scenario's die deel van de release van NH uitmaken

* **Media-Processor voor indexering**. Zie voor meer informatie [Met Azure Media indexeerfunctie mediabestanden indexeren].

* De entiteit [StreamingEndpoint] kunt u nu aangepaste domeinnamen (host) toevoegen.

    Voor een aangepaste domeinnaam moet worden gebruikt als de naam van het Media Services streaming, moet u aangepaste hostnamen toevoegen aan uw streaming eindpunt. Gebruik de Media Services REST API's of SDK voor .NET aangepaste hostnamen toevoegen.
    
    De volgende overwegingen zijn van toepassing:
    
    * U moet de eigenaar van de naam van het aangepaste domein hebben.
    
    * De eigenaar van de domeinnaam moet worden gevalideerd door Azure Media Services. Maak een CName die is toegewezen voor het valideren van het domein <MediaServicesAccountId>.<parent domain> verifydns. < mediaservices DNS-zone >. 
    
    * U moet een andere CName die de aangepaste host-naam (bijvoorbeeld sports.contoso.com) aan uw Media Services StreamingEndpont hostnaam (bijvoorbeeld amstest.streaming.mediaservices.windows.net).


    Zie de eigenschap **CustomHostNames** in het onderwerp [StreamingEndpoint] voor meer informatie.

### <a id="sept_14_preview_changes"></a>Nieuwe functies/scenario's die deel van de openbare preview-versie uitmaken

* Live Streaming voorbeeld. Zie [werken met Azure Media Services Live Streaming]voor meer informatie.

* Belangrijkste bezorgservice. Zie voor meer informatie [met behulp van AES 128 dynamische codering en Key leveringsservice].

* Dynamische AES-codering. Zie voor meer informatie [met behulp van AES 128 dynamische codering en Key leveringsservice].

* PlayReady licentie bezorgservice. Zie voor meer informatie [met behulp van dynamische PlayReady-codering en bezorgservice licentie].

* Dynamische PlayReady-codering. Zie voor meer informatie [met behulp van dynamische PlayReady-codering en bezorgservice licentie].

* Sjabloon voor PlayReady Media-Services. Zie [Overzicht van Media Services PlayReady licentie sjabloon]voor meer informatie.

* Activa streaming opslag worden gecodeerd. Voor meer informatie Zie [Streaming opslag gecodeerde inhoud].

##<a id="august_changes_14"></a>Augustus 2014-Release

Wanneer u een activum codeert, wordt na voltooiing van de taak codering een actief uitvoer geproduceerd. Tot deze versie is geproduceerd Azure Media Services Encoder metagegevens over de activa van de uitvoer. Vanaf deze versie van het coderingsprogramma produceert ook metagegevens over de invoer van activa. Zie voor meer informatie de onderwerpen van de [Metagegevens van de invoer] en [Uitvoer, metagegevens] .


##<a id="july_changes_14"></a>Juli 2014-Release

De volgende correcties zijn aangebracht voor de Azure Media Services Packager en coderen:

* Alleen audio wordt afgespeeld wanneer een activum live archief voor HTTP Live Streaming – transmuxing dit probleem is opgelost en nu zowel audio als video afgespeeld.

* Wanneer u een activum naar HTTP Live Streaming en AES 128-bits envelop codering verpakking, verpakte stromen worden niet afgespeeld op Android-apparaten: deze fout is verholpen en verpakte stream op Android-apparaten die ondersteuning bieden voor HTTP Live Streaming afgespeeld.

##<a id="may_changes_14"></a>Mei 2014-Release

### <a id="may_14_changes"></a>Updates voor algemene Media Services

Nu kunt u [Dynamische verpakking] aan stroom HTTP Live Streaming (HLS) past v3. HLS v3 streamen, de volgende indeling toevoegen aan het pad van oorsprong locator: * .ism/manifest(format=m3u8-aapl-v3). Zie voor meer informatie [van Nick Drouin Blog].

Dynamische verpakking nu ook ondersteuning leveren HLS (v3 en v4) gecodeerd met PlayReady op basis van goede Streaming statisch gecodeerd met PlayReady. Zie voor meer informatie over het coderen van goede Streaming met PlayReady, [goede Stream PlayReady beveiligen].

### <a name="may_14_donnet_changes"></a>Updates voor .NET SDK-mediaservices

De volgende verbeteringen zijn opgenomen in de release van .NET SDK 3.0.0.5 van Media Services:

* Betere snelheid en robuustheid voor uploaden/downloaden van media-elementen.

* Verbeteringen in de verwerking van opnieuw logica en tijdelijk uitzonderingen: 

    * Tijdelijke fout detectie en probeer het opnieuw logica zijn verbeterd voor uitzonderingen die worden veroorzaakt door het uitvoeren van query's, het opslaan van wijzigingen, bestanden uploaden of downloaden. 
    
    * Bij het ophalen van website-uitzonderingen (bijvoorbeeld tijdens een verzoek van de ACS-token), zult u merken dat fatale fouten niet goed sneller nu werkt zijn.

Zie voor meer informatie de [Logica opnieuw in de Media Services SDK voor .NET].

##<a id="april_changes_14"></a>April 2014-Encoder Release

### <a name="april_14_enocer_changes"></a>Media Services Encoder Updates

* Ondersteuning toegevoegd voor AVI-bestanden geschreven met behulp van de niet-lineaire editor gras Valley EDIUS waar de video licht is ingesting gecomprimeerd met gras Valley HQ/HQX-codec. Zie voor meer informatie, [Gras Valley kondigt EDIUS 7 Streaming via de Cloud].

* Ondersteuning toegevoegd voor het opgeven van de naamgevingsconventie voor de bestanden die met de Media Encoder. Voor meer informatie, Zie [Beheren van Media Service Encoder namen voor uitvoerbestanden].

* Ondersteuning toegevoegd voor audio en/of video-overlays. Zie [Bedekkingen maken]voor meer informatie.

* Ondersteuning toegevoegd voor wilt samenvoegen meerdere video-segmenten. Voor meer informatie, Zie [Video segmenten Stitching].

* Vast een bug die verband houden met transcodering van MP4s waarbij het geluid is gecodeerd met MPEG-1 Audio layer 3 (aka MP3).


##<a id="jan_feb_changes_14"></a>Januari/februari 2014 Releases

### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services SDK voor .NET 3.0.0.1, 3.0.0.2 en 3.0.0.3

De wijzigingen in 3.0.0.1 en 3.0.0.2 zijn:

* Vaste problemen met het gebruik van het LINQ-query's met overzichten sorteren op.

* Gesplitste testoplossingen in [GitHub] in op basis van Unit tests en tests op basis van een Scenario.

Zie voor meer informatie over de wijzigingen: [Azure Media Services SDK voor .NET 3.0.0.1 en 3.0.0.2 uitgebracht].

De volgende wijzigingen zijn aangebracht in de 3.0.0.3:

* Bijgewerkte Azure opslag afhankelijkheden versie 3.0.3.0 te gebruiken. 

* Vaste compatibiliteitsprobleem voor 3.0. *.* releases. 


##<a id="december_changes_13"></a>Versie december 2013

### <a name="dec_13_donnet_changes"></a>Azure Media Services SDK voor .NET 3.0.0.0-prestatiemeters

>[AZURE.NOTE] 3.0.x.x versies zijn niet compatibel met versies van de 2.4.x.x.

De meest recente versie van Media Services SDK is nu 3.0.0.0-prestatiemeters. U kunt de meest recente downloaden van Nuget of de bits van [GitHub]ophalen.

U kunt de tokens [Azure Active Directory Access Control Service (ACS)] begint met de Media Services SDK versie 3.0.0.0-prestatiemeters, hergebruiken. Zie voor meer informatie de sectie "Hergebruik van Access Control Service Tokens" in het onderwerp [verbinding maken met Media-Services met de Media Services SDK voor .NET] .

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services-uitbreidingen voor .NET SDK 2.0.0.0

De serverextensies Azure Media Services .NET SDK is een set van uitbreidingsmethoden en Help-functies die uw code eenvoudiger en gemakkelijker te ontwikkelen met Azure Media Services. U krijgt de meest recente bits van [Azure Media Services .NET SDK-uitbreidingen].

##<a id="november_changes_13"></a>Versie november 2013

### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK wijzigingen

Vanaf deze versie is verwerkt de Media Services SDK voor .NET tijdelijke fout met betrekking tot fouten die optreden kunnen bij het aanroepen van de Media Services REST API-laag.

##<a id="august_changes_13"></a>Release augustus 2013

### <a name="aug_13_powershell_changes"></a>PowerShell-Cmdlets voor Media Services is opgenomen in de Sdk-hulpprogramma Azure

De volgende Media Services PowerShell-cmdlets zijn nu opgenomen in [azure-sdk-hulpprogramma's].

* Get-AzureMediaServices 

    Bijvoorbeeld `Get-AzureMediaServicesAccount`.

* Nieuwe AzureMediaServicesAccount 

    Bijvoorbeeld `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Nieuwe AzureMediaServicesKey 

    Bijvoorbeeld `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Verwijderen AzureMediaServicesAccount 

    Bijvoorbeeld `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Versie juni 2013

### <a name="june_13_general_changes"></a>Azure Media-Services wijzigen

De wijzigingen vermeld in deze sectie zijn opgenomen in de versies van juni 2013 Media Services-updates.

* Mogelijkheid om meerdere accounts voor opslag op een Media-account koppelen. 

    StorageAccount
    
    Asset.StorageAccountName en Asset.StorageAccount

* Mogelijkheid om de Job.Priority. 

* Melding gerelateerde entiteiten en eigenschappen: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Taak

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK wijzigingen

De volgende wijzigingen zijn opgenomen in juni 2013 Media Services SDK vrijgegeven. De meest recente Media Services SDK is beschikbaar op GitHub.

* Vanaf de versie van 2.3.0.0, de Media Services SDK ondersteunt meerdere opslag koppelen aan een Media-Services-account accounts. Deze functie wordt ondersteund door de volgende API's:
    
    Het type IStorageAccount.
    
    De eigenschap Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
    
    De eigenschap StorageAccount.
    
    De eigenschap StorageAccountName.
    
    Voor meer informatie Zie [Media Services activa beheren van meerdere accounts voor opslag].

* Melding gerelateerde API's. Beginnend met versie 2.2.0.0 hebt u de mogelijkheid om te luisteren naar de wachtrij Azure opslag meldingen. Zie voor meer informatie, [Media Services project meldingen afhandelen].
    
    De eigenschap Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.

* De afhankelijkheid van de opslag van Azure Client SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Afhankelijkheid van OData 5.5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>Versie december 2012

### <a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK wijzigingen

* IntelliSense: Ontbrekende Intellisense-documentatie voor vele typen toegevoegd.

* Microsoft.Practices.TransientFaultHandling.Core: Vaste een probleem waarbij de SDK nog steeds een afhankelijkheid van een oude versie van deze assembly was. Versie 5.1.1209.1 van deze assembly wordt nu verwezen naar de SDK.

Oplossingen voor problemen die zijn gevonden in de SDK van November 2012:

* IAsset.Locators.Count: Deze waarde wordt nu correct gemeld op nieuwe IAsset interfaces nadat alle locators zijn verwijderd.

* IAssetFile.ContentFileSize: Deze waarde is nu goed ingesteld na het uploaden van IAssetFile.Upload(filepath).

* IAssetFile.ContentFileSize: Deze eigenschap kan nu worden ingesteld bij het maken van een bestand actief. Het was eerder alleen-lezen.

* IAssetFile.Upload(filepath): Vaste een probleem waar deze Uploadmethode synchrone de volgende fout is genereren tijdens het uploaden van meerdere bestanden naar het activum. De fout is 'Server kan niet worden geverifieerd verzoek. Zorg ervoor dat de waarde van verificatieheader goed met inbegrip van de handtekening wordt gevormd."

* IAssetFile.UploadAsync: Vaste een probleem waarbij niet meer dan 5 bestanden tegelijkertijd kunnen worden geüpload.

* IAssetFile.UploadProgressChanged: Deze gebeurtenis wordt nu geleverd door de SDK.

* IAssetFile.DownloadAsync (string, BlobTransferClient, ILocator, CancellationToken): deze methode overbelasting, is nu beschikbaar.

* IAssetFile.DownloadAsync: Vaste een probleem waarbij niet meer dan 5 bestanden tegelijk kunnen worden gedownload.

* IAssetFile.Delete(): Vaste een probleem waarbij aanroep delete kan genereert een uitzondering als er geen bestand is geüpload voor het IAssetFile.

* Taken: Vaste een probleem waarbij een "MP4 aan goede Streams taak" met een"PlayReady bescherming" met behulp van een sjabloon koppelen niet maakt alle taken op alle.

* EncryptionUtils.GetCertificateFromStore(): Deze methode genereert niet langer een uitzondering null-verwijzing als gevolg van een mislukte het certificaat op basis van configuratieproblemen certificaat zoeken.

##<a id="november_changes_12"></a>Versie november 2012

De wijzigingen vermeld in deze sectie zijn updates die zijn opgenomen in de November 2012 (versie 2.0.0.0) SDK. Deze wijzigingen mogelijk code geschreven voor het voorbeeld van juni 2012 SDK release worden gewijzigd of opnieuw geschreven.

* Activa
    
    IAsset.Create(assetName) is de functie alleen actief maken. Bestanden uploadt IAsset.Create niet langer als onderdeel van het aanroepen van de methode. Gebruik IAssetFile voor het uploaden.
    
    De methode IAsset.Publish en de waarde van de opsomming AssetState.Publish zijn verwijderd uit de Services SDK. Elke code die is gebaseerd op deze waarde moet worden opnieuw geschreven.

* FileInfo

    Deze klasse is verwijderd en vervangen door IAssetFile.

    IAssetFiles

    IAssetFile vervangt FileInfo en vertoont ander gedrag. Om dit te gebruiken, het IAssetFiles-object, gevolgd door een bestandsupload met behulp van de Media Services SDK of de SDK Azure opslag te instantiëren. De volgende IAssetFile.Upload overbelastingen kunnen worden gebruikt:

    * IAssetFile.Upload(filePath): Een synchrone methode die de thread blokkeert en wordt aanbevolen als één bestand uploaden.
    
    * IAssetFile.UploadAsync (filePath, blobTransferClient, locator, cancellationToken): een asynchrone bewerking. Dit is het mechanisme voorkeur uploaden. 

    Bekende bug: met behulp van de cancellationToken inderdaad annuleert u het uploaden; de status van de annulering van de taken kan evenwel een van een aantal lidstaten. U moet goed opvangen en afhandelen van uitzonderingen.

* Locators
    
    De oorsprong-specifieke versies zijn verwijderd. De SAS-specifieke context. Locators.CreateSasLocator (actief, accessPolicy) wordt vervangen of verwijderd door GA. gemarkeerd Zie de sectie Locators onder nieuwe functionaliteit voor bijgewerkte gedrag.


##<a id="june_changes_12"></a>Preview-versie van juni 2012

De volgende functionaliteit is nieuw in de release van November van de SDK.

* Entiteiten verwijderen

    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey-objecten worden nu op het niveau, dat wil zeggen IObject.Delete(), zodat een verwijderen in de collectie die cloudMediaContext.ObjCollection.Delete(objInstance) is niet verwijderd.

* Locators

    Locators moeten nu worden gemaakt met de methode CreateLocator en de waarden van de opsomming LocatorType.SAS of LocatorType.OnDemandOrigin gebruiken als argument voor het specifieke type locator dat u wilt maken.

    Nieuwe eigenschappen zijn toegevoegd aan Locators gemakkelijker bruikbaar URI's voor uw inhoud verkrijgen. Dit nieuwe ontwerp van Locators is bedoeld als bieden meer flexibiliteit voor toekomstige uitbreiding van derden, waardoor het gemakkelijk te gebruiken voor media-clienttoepassingen.

* Ondersteuning voor asynchrone methode

    Ondersteuning voor asynchrone is toegevoegd aan alle methoden.


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN Forum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API: naslag]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Media Services prijsdetails]: http://azure.microsoft.com/pricing/details/media-services/
[Metagegevens invoeren]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Uitvoer, metagegevens]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Inhoud leveren]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Mediabestanden met Azure Media indexeerfunctie indexeren]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Werken met Azure mediaservices Live Streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Met behulp van dynamische Encryption AES 128 en belangrijke bezorgservice]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Met behulp van dynamische PlayReady codering en licentie bezorgservice]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady licentie sjabloon overzicht]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Opslag streaming inhoud gecodeerd]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dynamische verpakking]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Blog van Nick Drouin]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Bescherming van de goede Stream PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Opnieuw de logica in de mediaservices SDK voor .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Gras Valley kondigt EDIUS 7 Streaming via de Cloud]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Controle over Media Service Encoder uitvoer bestandsnamen]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Overlays maken]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Video segmenten stitching]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure releases van Media Services SDK voor .NET 3.0.0.1 en 3.0.0.2]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory Access Control-Service (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Verbinding maken met Media-Services met de mediaservices SDK voor .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services-uitbreidingen voor .NET SDK]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure-sdk-hulpprogramma 's]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Media beheren Services activa over meerdere Accounts voor opslag]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Verwerking van Media Services-taak meldingen]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
