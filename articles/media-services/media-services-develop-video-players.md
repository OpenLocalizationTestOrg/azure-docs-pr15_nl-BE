<properties 
    pageTitle="Video player-toepassingen ontwikkelen" 
    description="Het onderwerp bevat koppelingen Player Frameworks en plugins die u gebruiken kunt voor het ontwikkelen van uw eigen clienttoepassingen die streaming Media Services-media kunnen worden gebruikt." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Video player-toepassingen ontwikkelen

##<a name="overview"></a>Overzicht

Azure Media Services biedt de hulpprogramma's die u nodig hebt voor het maken van krachtige, dynamische speler clienttoepassingen voor de meeste platforms, waaronder: iOS apparaten, Android-apparaten, Windows, Windows Phone, Xbox en Set-top-vakken. Dit onderwerp bevat tevens koppelingen naar SDK's en -Player kaders die u gebruiken kunt voor het ontwikkelen van uw eigen clienttoepassingen die streaming media van Azure Media Services kunnen worden gebruikt.

##<a name="azure-media-player"></a>Azure MediaPlayer

[Azure Media Player](http://aka.ms/ampinfo) is een web-speler ingebouwd om af te spelen media-inhoud van Microsoft Azure Media Services op een grote verscheidenheid van browsers en apparaten. Azure Media Player maakt gebruik van industriestandaarden zoals HTML5, Media Source Extensions (MSE) en gecodeerd Media extensies (EME) bieden een verrijkte adaptieve streaming. Wanneer deze normen niet beschikbaar in een browser of op een apparaat zijn, Azure Media Player Flash en Silverlight gebruikt als terugval technologie. Ongeacht de gebruikte afspelen technologie hebben ontwikkelaars een eenduidige interface JavaScript toegang tot API's. Hiermee door Azure Media Services aangeboden inhoud wordt afgespeeld via een ruim assortiment van apparaten en browsers zonder enige extra inspanning.

Microsoft Azure Media Services kunnen inhoud aangeboden met streepje, soepele Streaming en HLS streaming-indelingen afspelen van inhoud. Azure Media Player rekening wordt gehouden met deze verschillende indelingen en de beste koppeling op basis van de mogelijkheden van het platform/browser worden automatisch afgespeeld. Microsoft Azure Media Services ook dynamische codering van activa met PlayReady codering toestaat of AES 128-bits codering van de envelop. Azure Media Player kunt u voor het decoderen van PlayReady en AES 128-bits gecodeerde inhoud op de juiste wijze geconfigureerd. 

Voor meer informatie:

- [Azure MediaPlayer](http://aka.ms/ampinfo)
- [Azure Media Player documentatie](http://aka.ms/ampdocs) 
- [Azure MediaPlayer ophalen Blog gestart](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Aanmelden voor het Blijf up-to-date met de nieuwste van Azure Media Player](http://aka.ms/ampsignup)
- [Nieuwe functie-aanvragen, ideeën en feedback toevoegen](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Andere hulpmiddelen voor het maken van toepassingen van de speler

U kunt ook de volgende SDK's:

- [Vloeiend Streaming Client SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Goede Streaming App in de Windows Store](media-services-build-smooth-streaming-apps.md)
- [-Platform: Microsoft Media Player Framework](http://playerframework.codeplex.com/) 
- [HTML5 Player Framework-documentatie](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Microsoft vloeiend Streaming Plugin voor OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Licentieverlening Microsoft® goede Streaming Client Kit overdragen](http://aka.ms/sspk) 
- [XBOX Video Application Development](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Reclame

Azure Media Services biedt ondersteuning voor ad invoegen via de Windows Media-Platform: Frameworks-speler. Player kaderregelingen met ad-ondersteuning zijn beschikbaar voor Windows 8, Silverlight, Windows Phone 8 en iOS-apparaten. Elke speler framework bevat voorbeeldcode waarmee het implementeren van een toepassing van de speler. Er zijn drie verschillende soorten advertenties die u in uw media invoegen kunt:

Lineair – volledig frame advertenties die de belangrijkste video pauzeren

Niet-lineaire – overlay-advertenties die worden weergegeven als de belangrijkste video wordt afgespeeld, meestal een logo of andere statische afbeelding geplaatst in Windows media player

Companion – advertenties die buiten Windows media player worden weergegeven

Advertenties kunnen op elk punt in de tijdlijn van de belangrijkste video worden geplaatst. U moet Windows media player te zien bij het afspelen van de advertentie en welke advertenties af te spelen. Dit wordt gedaan met behulp van een set van standaard XML-bestanden: Video Ad-Service-sjabloon (VAST), digitale Video meerdere Ad afspeellijst (VMAP), Media abstracte volgordebepaling sjabloon (b) en digitale Video Player Ad Interface Definition (VPAID). GROTE bestanden opgeven welke advertenties weer te geven. VMAP bestanden opgeven bij het afspelen van verschillende advertenties en ENORME XML bevatten. MAST-bestanden zijn een andere manier om een reeks advertenties die ook VAST XML kan bevatten. VPAID bestanden definiëren voor een interface tussen de videospeler en het ad of Active Directory-server. Zie [Advertenties invoegen](https://msdn.microsoft.com/library/dn387398.aspx)voor meer informatie.

Zie voor meer informatie over de ondersteuning van gesloten captioning en advertenties in Live streaming video's [ondersteund Closed Captioning en Ad plaatsingskosten normen](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook

[Een MPEG-streepje adaptieve Streaming Video insluiten in een toepassing HTML5 met DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[Opslagplaats van GitHub dash.js](https://github.com/Dash-Industry-Forum/dash.js)
 
