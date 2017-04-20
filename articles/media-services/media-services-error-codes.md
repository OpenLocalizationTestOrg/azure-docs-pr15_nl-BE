<properties
    pageTitle="Azure foutcodes van Media Services | Microsoft Azure"
    description="Het onderwerp bevat een overzicht van de foutcodes Azure Media Services."
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
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Azure Media Services-foutcodes

Wanneer u Microsoft Azure Media Services, wordt van de service afhankelijk van de problemen met de verificatietokens verloopt op acties die niet worden ondersteund in de Media Services HTTP-foutcodes. Hier volgt een lijst met **http-foutcodes** kunnen worden geretourneerd door de Media Services en de mogelijke oorzaken voor hen.  
  
## <a name="400-bad-request"></a>400-Ongeldig verzoek

De aanvraag bevat ongeldige gegevens en is geweigerd vanwege een van de volgende redenen:

- Een niet-ondersteunde versie van de API is opgegeven. Zie [Instellingen voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor de meest recente versie.
- De API-versie van Media Services is niet opgegeven. Zie [verbinding maken met Media-Services met de overige Media Services API](media-services-rest-connect-programmatically.md)voor meer informatie over het opgeven van de API-versie. 
   
    >[AZURE.NOTE] Als u de .NET of Java-SDK's verbinding maken met Media Services, wordt de API versie opgegeven voor u telkens wanneer u probeert en een actie tegen de Media Services.
- Een ongedefinieerde eigenschap is opgegeven. Naam van de eigenschap wordt in het foutbericht. Alleen die eigenschappen die deel uitmaken van een bepaalde entiteit kunnen worden opgegeven. Zie [Azure Media Services REST API-naslaggids](http://msdn.microsoft.com/library/azure/hh973617.aspx) voor een lijst met entiteiten en hun eigenschappen.
- Een ongeldige waarde er is opgegeven. Naam van de eigenschap wordt in het foutbericht. Zie de vorige koppeling voor typen geldige eigenschappen en waarden.
- De waarde van een eigenschap ontbreekt en is vereist.
- Gedeelte van de opgegeven URL bevat een ongeldige waarde.
- Er is geprobeerd een WriteOnce-eigenschap bij te werken.
- Er is een poging gedaan voor het maken van een taak met een invoer actief met een primaire AssetFile die niet is opgegeven of niet kan worden bepaald.
- Er is geprobeerd een SAS-Locator bijwerken. SAS-locators kunnen alleen worden gemaakt of verwijderd. Streaming locators kan worden bijgewerkt. Voor meer informatie Zie [Locators](http://msdn.microsoft.com/library/azure/hh974308.aspx).
- Een niet-ondersteunde bewerking of een query is ingediend. 

## <a name="401-unauthorized"></a>401 geen toestemming

De aanvraag kan niet worden geverifieerd (voordat deze kan worden gemachtigd) om een van de volgende redenen:

- Ontbrekende authentication header.
- Ongeldige authentication header-waarde.
    - Het token is verlopen. Zie [verbinding maken met Media-Services met de overige Media Services API](media-services-rest-connect_programmatically.md) om te leren hoe u een nieuwe verificatietoken te genereren als de REST API's rechtstreeks gebruikt. Als .NET of Java-SDK's, maakt u een CloudMediaContext of MediaContract-object om het token te genereren. Zie voor meer informatie over hoe u dit doet, [verbinding maken met Media-Services met de Media Services SDK voor .NET](media-services-dotnet-connect-programmatically.md).
    - Het token bevat een ongeldige handtekening.</li></ul></li></ul>

## <a name="403-forbidden"></a>403

De aanvraag is niet toegestaan om een van de volgende redenen:

- De Media-Services-account kan niet worden gevonden of is verwijderd.
- De Media Services-account is uitgeschakeld en het aanvraagtype niet HTTP GET. Bewerkingen van het resultaat van 403 ook.
- De verificatietoken bevat geen gegevens van de referenties van de gebruiker: accountnaam en/of SubscriptionId. U kunt deze informatie vinden in de gebruikersinterface van Media Services-uitbreiding voor uw account Media Services in Azure Management Portal.
- De resource kan niet worden geopend.
    - Er is geprobeerd een MediaProcessor die niet beschikbaar is voor uw Media Services-account gebruiken.
    - Er is geprobeerd een taaksjabloon gedefinieerd door Media Services bijwerken.
    - Er is geprobeerd enkele andere Media Services van account Locator overschrijven.
    - Er is geprobeerd te overschrijven van sommige andere Media Services-account van ContentKey.

- De resource kan niet worden gemaakt omdat het quotum van een service die is bereikt voor de Media-Services-account. Zie voor meer informatie over de servicequota [quota's en beperkingen](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 niet gevonden

De aanvraag is niet toegestaan op een bron om een van de volgende redenen:

- Er is een poging gedaan voor het bijwerken van een entiteit die niet bestaat.
- Er is geprobeerd te verwijderen van een entiteit die niet bestaat.
- Er is geprobeerd te maken van een entiteit die is gekoppeld aan een entiteit die niet bestaat.
- Er is een poging gedaan om een entiteit die niet bestaat.
- Er is geprobeerd een opslag-account die niet is gekoppeld aan het Media-Services-account opgeven.  

## <a name="409-conflict"></a>409 conflict

De aanvraag is niet toegestaan om een van de volgende redenen:

- Meer dan één AssetFile heeft de opgegeven naam in de activa.
- Er is geprobeerd een tweede primaire AssetFile binnen het actief maken.
- Er is geprobeerd een ContentKey maken met de opgegeven Id al in gebruik.
- Er is geprobeerd een Locator maken met de opgegeven Id al in gebruik.
- Meer dan één IngestManifestFile heeft de opgegeven naam in de IngestManifest.
- Er is geprobeerd een tweede opslag codering ContentKey koppelen aan het activum opslag versleuteld.
- Er is geprobeerd de dezelfde ContentKey aan het activum koppelt.
- Er is geprobeerd een locator naar een vast activum waarvan opslag container ontbreekt of is niet langer gekoppeld aan het actief maken.
- Er is geprobeerd een locator op activa die al in gebruik 5 locators is maken. (Azure opslag zorgt ervoor dat de limiet van vijf gedeelde-beleid op een container voor opslag.)
- Opslag-account van een activum koppelen aan een IngestManifestAsset is niet hetzelfde als de opslag die wordt gebruikt door de bovenliggende IngestManifest.  

## <a name="500-internal-server-error"></a>Interne serverfout 500

Tijdens de verwerking van de aanvraag Media Services een fout aantreft die wordt voorkomen de verwerking van voortgezette dat. Dit kan worden veroorzaakt door een van de volgende redenen:

- Maken van een actief of een taak mislukt, omdat de quotumgegevens voor Media Services-servicesaccount is tijdelijk niet beschikbaar.
- Maken van een actief of IngestManifest blob storage container mislukt, omdat de gegevens van opslag van de account is tijdelijk niet beschikbaar.
- Een andere onverwachte fout. 

## <a name="503-service-unavailable"></a>503 Service niet beschikbaar

De server kan momenteel geen aanvragen ontvangen. Deze fout kan worden veroorzaakt door overmatige verzoeken voor de service. Media Services mechanisme bandbreedtebeperking beperkt het gebruik van bronnen voor toepassingen die buitensporig verzoek naar de service.

>[AZURE.NOTE]Controleer de foutmelding en code fouttekenreeks als u meer gedetailleerde informatie over de reden die u kreeg de 503-fout. Deze fout betekent niet altijd beperken.

Statusbeschrijvingen mogelijk zijn:

- "Server is bezet. Dit type aanvraag eerder uitgevoerde duurde meer dan {0} seconden."
- "Server is bezet. Meer dan {0}-aanvragen per seconde kan worden beperkt."
- "Server is bezet. Meer dan {0} aanvragen binnen enkele seconden {1} kan worden beperkt."

Voor het verwerken van deze fout, beste exponentiële opnieuw terug uit logica te gebruiken. Dit betekent dat geleidelijk langer wacht tussen nieuwe pogingen voor opeenvolgende foutreacties gebruiken.  Zie [Tijdelijke fout afhandeling toepassing blokkeren](https://msdn.microsoft.com/library/hh680905.aspx)voor meer informatie. 

>[AZURE.NOTE]Als u van [Azure Media Services SDK voor .net gebruikmaakt](https://github.com/Azure/azure-sdk-for-media-services/tree/master), is de logica opnieuw voor de 503-fout geïmplementeerd door de SDK.  
  
## <a name="see-also"></a>Zie ook  

[Media Services-beheer-foutcodes](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
