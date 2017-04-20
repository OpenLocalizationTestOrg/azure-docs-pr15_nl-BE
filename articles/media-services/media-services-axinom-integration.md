<properties 
    pageTitle="Met behulp van Axinom Widevine licenties Azure Media services leveren | Microsoft Azure" 
    description="In dit artikel wordt beschreven hoe u kunt Azure Media Services (AMS) voor het leveren van een gegevensstroom die dynamisch wordt gecodeerd door AMS met PlayReady en Widevine DRMs. De licentie PlayReady afkomstig van PlayReady van Media Services-licentieserver en Widevine licentie wordt geleverd door de licentieserver Axinom." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Met behulp van Axinom Widevine licenties Azure Media services leveren  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Overzicht

Azure Media Services (AMS) Google Widevine dynamische beveiliging is toegevoegd (Zie [blog van Mingfei](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) voor details). Bovendien Azure Media Player (AMP) ook Widevine ondersteuning is toegevoegd (Zie [AMP document](http://amp.azure.net/libs/amp/latest/docs/) voor meer informatie). Dit is een belangrijke vervullen in streaming streepje inhoud beschermd door CENC met meerdere-native-DRM (PlayReady en Widevine) in moderne browsers voorzien van MSE en EME.

Media Services kunt vanaf Media Services SDK voor .NET versie 3.5.2, u configureren Widevine licentie sjabloon Widevine licenties ophalen. U kunt ook de volgende AMS partners waarmee u licenties Widevine leveren: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

In dit artikel wordt beschreven hoe integreren en Widevine licentieserver beheerd door Axinom testen. Dit omvat met name:  

- Dynamische gemeenschappelijke codering configureren met multi-DRM (PlayReady en Widevine) met bijbehorende licentie aanschaf-URL's;
- Een token JWT genereren om te voldoen aan de licentievereisten server;
- Ontwikkelen van Azure Media Player app die verwerving van licenties bij JWT token verificatie;

Het volledige systeem en de stroom van inhoud die en de belangrijkste ID sleutel zaaizaad, JTW token en haar vorderingen kan best door in het volgende diagram worden beschreven.

![STREEPJE en CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Inhoudsbeveiliging

Zie voor het configureren van dynamische beveiliging en beleid belangrijke levering van Mingfei blog: [Widevine verpakking met Azure Media Services configureren](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

U kunt dynamische CENC bescherming met multi-DRM configureren voor streaming met het volgende streepje:

1. PlayReady bescherming voor MS Edge en IE11 die een token vergunning beperkingen kan hebben. Het token beperkte beleid moet vergezeld gaan van een token dat wordt afgegeven door een beveiligde Token Service (STS), zoals Azure Active Directory.
1. Widevine bescherming voor chroom, kan het token verificatie vereist met een token is uitgegeven door een andere STS. 

Zie [JWT Token genereren](media-services-axinom-integration.md#jwt-token-generation) voor waarom Azure Active Directory kan niet worden gebruikt als een STS voor Axinom van Widevine licentieserver.

###<a name="considerations"></a>Overwegingen met betrekking tot

1. Moet u de opgegeven Axinom sleutel zaad (8888000000000000000000000000000000000000) en de gegenereerde of geselecteerde ID voor het genereren van de sleutel voor het configureren van belangrijke bezorgservice sleutel. Axinom licentieserver verleent alle licenties die inhoud sleutels op basis van de dezelfde sleutel zaad, dat geldig voor het testen en productie is.
1. De URL Widevine licentie verkrijgen voor het testen: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP- en HTTS zijn toegestaan.

##<a name="azure-media-player-preparation"></a>Voorbereiding van Azure Media Player

AMP v1.4.0 ondersteunt het afspelen van inhoud AMS die dynamisch wordt geleverd met zowel PlayReady als Widevine DRM.
Als de licentieserver Widevine token verificatie niet vereist, is er niets meer hoeft te doen om een streepje inhoud beschermd door Widevine testen. Het AMP team biedt bijvoorbeeld een eenvoudig [voorbeeld](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), waar kunt u deze zien in de rand en IE11 met PlayReady en chroom met Widevine werkt.
De server Widevine licentie van Axinom JWT token verificatie vereist is. Het token JWT moet worden ingediend met licentie aanvragen via een HTTP-header "X-AxDRM-bericht". Voor dit doel moet u de volgende javascript in de webpagina hosten AMP voordat u de bron toevoegen:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

De rest van het AMP code is standaard AMP-API in AMP document [hier](http://amp.azure.net/libs/amp/latest/docs/).

Opmerking het bovenstaande javascript voor instelling aangepaste Verificatieheader is nog steeds een benadering van de korte termijn vóór de officiële lange termijn aanpak in het AMP wordt vrijgegeven.

##<a name="jwt-token-generation"></a>JWT Token genereren

Axinom Widevine licentieserver voor het testen van JWT token verificatie vereist is. Bovendien is een van de vorderingen in het token JWT in plaats van primitieve van een complexe object.

Helaas, Azure AD kan alleen worden verleend JWT tokens met primitieve typen. Ook kunt .NET Framework-API (System.IdentityModel.Tokens.SecurityTokenHandler en JwtPayload) u alleen naar complexe objecttype invoer als vorderingen. De claims zijn echter nog steeds geserialiseerd als tekenreeks. Daarom niet we gebruiken een van de twee voor het genereren van het token JWT voor Widevine licentie aanvragen.


Van John Sheehan [JWT Nuget pakket](https://www.nuget.org/packages/JWT) voldoet aan de behoeften, dus gaan we dit pakket Nuget gebruiken.

Hieronder vindt u de code voor JWT-token genereren met de benodigde vorderingen zoals vereist door de licentieserver Axinom Widevine voor het testen van:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine licentieserver

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Overwegingen met betrekking tot

1.  Hoewel de bezorgservice van AMS PlayReady licentie is vereist ' aan toonder = "voorafgaand aan een verificatietoken, Axinom Widevine licentieserver gebruikt deze niet.
2.  De sleutel Axinom communicatie wordt gebruikt als handtekeningsleutel. Houd er rekening mee dat de sleutel een hexadecimale tekenreeks is, maar het moet worden behandeld als een reeks bytes niet een tekenreeks als codering. Dit wordt bereikt door de methode ConvertHexStringToByteArray.

##<a name="retrieving-key-id"></a>Sleutel-ID ophalen

U misschien opgevallen dat in de code voor het genereren van een JWT token, sleutel-ID is vereist. Aangezien JWT token moet het klaar zijn voordat de player laden AMP, sleutel moet-ID worden opgehaald om JWT-token te genereren.

Uiteraard zijn er meerdere manieren opvragen voor sleutel-id. Zo mogelijk een opgeslagen ID sleutel samen met de metagegevens van inhoud in een database. Of u kunt ophalen sleutel-ID van het bestand MPD-streepje (beschrijving van Media-presentatie). De volgende code is voor de laatstgenoemde.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Samenvatting

Met de nieuwste toevoeging van ondersteuning voor Widevine in Azure Media Services Content Protection en Azure Media Player kunnen we implementeren van het streepje + meerdere-native-DRM (PlayReady + Widevine) streaming met beide PlayReady license service in AMS en Widevine-licentieserver Axinom voor de volgende moderne browsers:

- Chroom
- Microsoft Edge op Windows 10
- IE 11 op Windows 8.1 en Windows 10
- Zowel (Desktop) Firefox en Safari op Mac (geen iOS) worden ook ondersteund via Silverlight en dezelfde URL met Azure Media Player

De volgende parameters zijn vereist in de licentieserver Mini oplossing inzetten Axinom Widevine. Met uitzondering van de sleutel-ID, de overige parameters worden geleverd door Axinom op basis van hun Widevine server setup.


Parameter|Hoe het wordt gebruikt.
---|---
Communicatie ID sleutel|Moet worden opgenomen als de waarde van de claim "com_key_id" in JWT token (Zie [hieronder](media-services-axinom-integration.md#jwt-token-generation) ).
Sleutel voor communicatie|Als de handtekeningsleutel van JWT token moet worden gebruikt (Zie [hieronder](media-services-axinom-integration.md#jwt-token-generation) ).
Belangrijkste zaad|Moet worden gebruikt voor het genereren van de sleutel met de inhoud van een bepaalde ID sleutel (Zie [hieronder](media-services-axinom-integration.md#content-protection) ).
De URL Widevine licentie|Moet worden gebruikt bij het configureren van beleid voor levering van activa voor streepje streaming [(Zie hieronder)](media-services-axinom-integration.md#content-protection) .
Inhoud sleutel-ID|Moet worden opgenomen als onderdeel van de waarde van de claim recht bericht van token JWT (Zie [hieronder](media-services-axinom-integration.md#jwt-token-generation) ). 


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Bevestigingen 

Wij willen graag de volgende personen die hebben bijgedragen tot de verwezenlijking van dit document erkent: Kristjan Jõgi van Axinom, Mingfei Yan en Amit Rajput.
