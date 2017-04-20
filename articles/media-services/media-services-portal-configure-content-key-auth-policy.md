<properties 
    pageTitle="Inhoud sleutel autorisatiebeleid met behulp van de portal Azure configureren | Microsoft Azure" 
    description="Informatie over het configureren van autorisatiebeleid voor een sleutel." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Inhoud sleutel autorisatiebeleid configureren
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Overzicht

Microsoft Azure Media Services kunt u MPEG-streepje, soepele Streaming en HTTP-Live Streaming (HLS) past streams met Advanced Encryption Standard (AES) (met 128-bits coderingssleutels) of [Microsoft PlayReady-DRM](https://www.microsoft.com/playready/overview/)beveiligd leveren. AMS kunt u streepje streams die zijn gecodeerd met de DRM Widevine leveren. Zowel PlayReady als Widevine worden per specificatie van de gemeenschappelijke codering (ISO/IEC 23001-7 CENC) gecodeerd.

Media Services biedt ook een **Sleutel/licentie bezorgservice** van die clients ophalen kunnen, AES-sleutels of PlayReady/Widevine licenties voor de gecodeerde inhoud af te spelen.

In dit onderwerp ziet u hoe met de Azure portal de inhoud sleutel autorisatiebeleid configureren. De sleutel kan later worden gebruikt voor het coderen van uw inhoud dynamisch. Opmerking op dit moment kunt versleutelen volgende streaming-indelingen: HLS MPEG-streepje en soepele Streaming. U kunt geen schijven streaming format coderen of progressief gedownload.

Wanneer een speler een gegevensstroom die is ingesteld op dynamisch worden gecodeerd, gebruikt Media Services de geconfigureerde sleutel voor het coderen van inhoud met AES of DRM-versleuteling dynamisch. Om te decoderen van de gegevensstroom, de speler vraagt om de sleutel van de belangrijkste leveringsservice. De service geëvalueerd om te bepalen of de gebruiker is gemachtigd om de sleutel, het autorisatiebeleid dat u hebt opgegeven voor de sleutel.


Als u van plan bent meerdere sleutels van de inhoud of een **Sleutel/licentie bezorgservice** URL dan de levering van belangrijke Media Services-service wilt opgeven, gebruikt Media Services .NET SDK of REST API's.

[Inhoud sleutel autorisatiebeleid met .NET SDK van Media Services configureren](media-services-dotnet-configure-content-key-auth-policy.md)

[Inhoud sleutel autorisatiebeleid met REST API van Media Services configureren](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Sommige overwegingen zijn van toepassing:

- Als u dynamische verpakking en dynamische codering gebruikt, moet u ervoor zorgen om ten minste één gereserveerde eenheid streaming. Zie [voor het schalen van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
- Uw activa moet bevatten een reeks geavanceerde bitsnelheid MP4s of adaptieve bitsnelheid goede Streaming-bestanden. Zie voor meer informatie, [een actief coderen](media-services-encode-asset.md).
- De sleutel bezorgingsservice slaat ContentKeyAuthorizationPolicy en de bijbehorende objecten (beleidsopties en beperkingen) gedurende 15 minuten.  Als u een ContentKeyAuthorizationPolicy maken en geef een beperking van de "Token" gebruikt, test, en pas het beleid aan de beperking 'Openen', duurt het ongeveer 15 minuten voordat het beleid voor de 'Open' versie van het beleid.


##<a name="how-to-configure-the-key-authorization-policy"></a>Procedure: de belangrijkste autorisatiebeleid configureren

De belangrijkste autorisatiebeleid configureren, selecteert u de pagina **Beveiligde inhoud** .

Media Services biedt ondersteuning voor meerdere manieren voor het verifiëren van gebruikers die sleutel aanvragen. De inhoud sleutel verificatiebeleid kunt laten **openen**, **token**of beperkingen voor **IP-** vergunning (**IP** kan worden geconfigureerd met de REST of .NET SDK).

###<a name="open-restriction"></a>Open beperking

De beperking **openen** betekent dat het systeem de sleutel bieden voor iedereen die een belangrijke verzoek indient. Deze beperking kan handig zijn voor testdoeleinden.

![OpenPolicy][open_policy]

###<a name="token-restriction"></a>Token-beperking

Als u het beleid voor de beperkte token, klik op de **TOKEN** .

Het **token** beperkt beleid moet vergezeld gaan van een token dat is uitgegeven door een **Beveiligde Token Service** (STS). Media Services ondersteunt tokens in de **Eenvoudige Web-Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) en **JSON Web Token** (JWT)-indeling. Voor meer informatie, Zie [JWT token-verificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services biedt geen **Token Services beveiligen**. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS op tokens probleem. De STS moeten een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de token beperking worden geconfigureerd. De belangrijkste bezorgingsservice Media Services wordt de coderingssleutel terug naar de client als het token geldig is en die zijn geconfigureerd voor de sleutel komen overeen met de vorderingen in het token. Voor meer informatie, Zie [Gebruik Azure ACS op tokens probleem](http://mingfeiy.com/acs-with-key-services).

Bij het configureren van de **TOKEN** beleid worden beperkt, stelt u de waarden voor de **sleutel voor verificatie**, **uitgever** en **doelgroep**. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met, uitgever is de veilige token service het token verleent. De doelgroep (soms scope genoemd) beschrijft de intentie van het token of de resource de token gemachtigd voor toegang tot. De levering van belangrijke Media Services-service wordt gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

###<a name="playready"></a>PlayReady

Bij het beveiligen van uw inhoud met **PlayReady**, is een van de dingen die u moet opgeven in uw beleid een XML-tekenreeks die de PlayReady licentie sjabloon definieert. Het volgende beleid is standaard:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

U kunt op de knop **beleid xml importeren** en bieden een ander XML die voldoet aan het XML-Schema gedefinieerd [hier](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

