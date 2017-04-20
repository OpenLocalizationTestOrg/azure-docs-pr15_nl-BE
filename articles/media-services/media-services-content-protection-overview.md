<properties 
    pageTitle="Beschermen van inhoud overzicht | Microsoft Azure" 
    description="In dit artikel geeft een overzicht van beveiligde inhoud met Media-Services." 
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
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Overzicht van de inhoud beschermen


Microsoft Azure Media Services kunt u veilig uw media vanaf het moment dat het verlaten van uw computer via de opslag, verwerking en levering. Media-Services kunt u ervoor zorgen dat uw live en on-demand-inhoud dynamisch gecodeerd met Advanced Encryption Standard (AES) (met 128-bits coderingssleutels) of een van de belangrijkste DRMs: Microsoft PlayReady Google Widevine en Apple FairPlay. Media Services biedt ook een service voor het aanbieden van AES-sleutels en DRM-licenties aan clients worden (PlayReady, Widevine en FairPlay). 

In de volgende afbeelding ziet u de beveiligde inhoud werkstromen AMS ondersteunt. 

![Bescherm met PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]U moet ten minste één eenheid van streaming gereserveerde eerst op de streaming eindpunt waaruit u gecodeerde inhoud stroomsgewijs wilt ophalen om te kunnen gebruikmaken van dynamische codering.

In dit onderwerp vindt [concepten en terminologie](media-services-content-protection-overview.md) voor inhoudsbeveiliging met AMS begrijpen. Dit onderwerp bevat ook [koppelingen](media-services-content-protection-overview.md#common-scenarios) naar onderwerpen waarin wordt aangegeven hoe u taken beveiliging bereiken. 

##<a name="dynamic-encryption"></a>Dynamische codering

Microsoft Azure Media Services kunt u ervoor zorgen dat uw inhoud dynamisch versleuteld met AES wissen sleutel of codering van DRM: Microsoft PlayReady Google Widevine en Apple FairPlay.

Op dit moment kunt u de volgende indelingen voor streaming coderen: HLS MPEG-streepje en soepele Streaming. U kunt geen schijven streaming format coderen of progressief gedownload.

Als u Media-Services voor het coderen van een actief, moet u een coderingssleutel (CommonEncryption of EnvelopeEncryption) te koppelen aan het activum en ook autorisatiebeleid voor de sleutel te configureren.

Ook moet u voor het configureren van beleid voor levering van het actief. Als u wilt voor het stroomsgewijs verzenden van een gecodeerde activabeheer voor opslag, moet u opgeven hoe u wilt dat door activa levering beleid configureren.

Een stroom door een speler is aangevraagd, gebruikt Media Services de opgegeven sleutel voor het coderen van dynamisch de inhoud met behulp van duidelijke sleutel AES of DRM-codering. Om te decoderen van de gegevensstroom, de speler vraagt om de sleutel van de belangrijkste leveringsservice. De service geëvalueerd om te bepalen of de gebruiker is gemachtigd om de sleutel, het autorisatiebeleid dat u hebt opgegeven voor de sleutel.

>[AZURE.NOTE]Om te profiteren van dynamische codering, moet u eerst ten minste één On-demand streaming eenheid voor het streaming eindpunt waaruit u leveren de gecodeerde inhoud wilt ophalen. Zie [schaal Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

##<a name="storage-encryption"></a>Opslag-codering

Opslag-codering gebruikt voor het coderen van duidelijke inhoud lokaal met AES 256-bits codering en upload deze naar Azure opslag waar het is opgeslagen in rust zijn gecodeerd. Activa met opslag codering beveiligd worden ongecodeerd automatisch geplaatst in een gecodeerde-bestandssysteem voor de codering en eventueel opnieuw gecodeerd voordat u het uploadt terug als een nieuwe uitvoer actief. Het primaire gebruik geval voor opslag codering is als u wilt uw hoge kwaliteit invoermedium bestanden beveiligen met sterke codering in rust op schijf.

Voor het leveren van een gecodeerde activabeheer voor opslag, moet u beleid voor levering van het actief zodat Media Services weet hoe u wilt uw inhoud te leveren. Voordat uw activa kan worden gestreamd, de server voor mediagegevensstromen verwijdert u de codering voor de opslag en het stroomsgewijs inhoud met het beleid voor levering opgegeven (bijvoorbeeld AES, gemeenschappelijke codering of geen codering).

## <a name="common-encryption-cenc"></a>Gemeenschappelijke codering (CENC)

Gemeenschappelijke codering wordt gebruikt bij het coderen van uw inhoud met PlayReady of / en Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Met behulp van codering cbcs aapl

Cbcs aapl wordt gebruikt bij het coderen van uw inhoud met de FairPlay.

## <a name="envelope-encryption"></a>Envelop-codering 

Gebruik deze optie als u wilt uw inhoud beveiligen met duidelijke AES 128-sleutel. Als u een veiligere optie, kiest u een van de DRMs in dit onderwerp vermeld. 

##<a name="licenses-and-keys-delivery-service"></a>Bezorgservice voor licenties en sleutels

Media Services biedt een service voor het leveren van licenties voor DRM (PlayReady, Widevine, FairPlay) en AES Schakel sleutels aan clients worden. U kunt [de portal Azure](media-services-portal-protect-content.md), REST API of Media Services SDK voor .NET autorisatie en verificatie beleidsregels configureren voor uw licenties en sleutels.

##<a name="token-restriction"></a>Token-beperking

De inhoud sleutel autorisatiebeleid kan zijn voor een of meer vergunning beperkingen: open of beperking token. Het token beperkte beleid moet vergezeld gaan van een token dat wordt afgegeven door een beveiligde Token Service (STS). Media Services ondersteunt tokens in de eenvoudige Web Tokens (SWT) en JSON Web Token (JWT)-indeling. Media Services biedt geen Token Services beveiligen. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS op tokens probleem. De STS moeten een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de token beperking worden geconfigureerd. De belangrijkste bezorgingsservice Media Services retourneert het aangevraagde sleutel (of licentie) naar de client als de token geldig is en de vorderingen in het token overeenkomen met die welke worden geconfigureerd voor de sleutel (of licentie).

Bij het configureren van het token beleid worden beperkt, moet u de verificatie van de primaire sleutel, uitgever en parameters van de doelgroep. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met, uitgever is de veilige token service het token verleent. De doelgroep (soms scope genoemd) beschrijft de intentie van het token of de resource de token gemachtigd voor toegang tot. De levering van belangrijke Media Services-service wordt gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

##<a name="streaming-urls"></a>Streaming URL 's

Als het activum is gecodeerd met meer dan één DRM, moet u een tag codering in de streaming URL: (format = 'm3u8-aapl' codering = 'xxx').

De volgende overwegingen zijn van toepassing:

- Alleen nul of een coderingstype dat kan worden opgegeven.
- Coderingstype hoeft niet te worden opgegeven in de url als er slechts één codering is toegepast op het activum.
- Coderingstype is niet hoofdlettergevoelig.
- De volgende coderingstypen u kunt opgeven:  
    - **cenc**: gemeenschappelijke codering (Playready of Widevine)
    - **cbcs aapl**: Fairplay
    - **CBC**: envelop AES-codering.

##<a name="common-scenarios"></a>Gebruikelijke scenario 's

De volgende onderwerpen laten zien hoe beveiliging van inhoud in de opslag, levering dynamisch gecodeerde streaming media, AMS sleutel/licentie leveringsservice gebruiken

- [Beveiligen met AES](media-services-protect-with-aes128.md) 
- [Beveiligen met PlayReady en/of Widevine](media-services-protect-with-drm.md)
- [Streamen van uw inhoud beveiligd HLS met Apple FairPlay en/of PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Aanvullende scenario 's

- [Het integreren van service met uw eigen encryptor/streaming server Azure PlayReady licentie](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [Met behulp van castLabs voor het leveren van DRM-licenties Azure Media Services](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Verwante koppelingen

[PlayReady aankondigen als een service en dynamische AES-codering met Azure Media Services](http://mingfeiy.com/playready)

[Azure Media Services PlayReady licentie levering prijzen verklaard](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Foutopsporing uitvoeren voor AES gecodeerde gegevensstroom in Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT token-authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integratie van Azure Media Services OWIN MVC app met Azure Active Directory gebaseerd en leveren van inhoud sleutel op basis van de claims JWT beperken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Gebruik Azure ACS op tokens probleem](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
