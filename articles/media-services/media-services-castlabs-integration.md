<properties 
    pageTitle="Met behulp van castLabs te leveren licenties Widevine Azure Media Services | Microsoft Azure" 
    description="In dit artikel wordt beschreven hoe u kunt Azure Media Services (AMS) voor het leveren van een gegevensstroom die dynamisch wordt gecodeerd door AMS met PlayReady en Widevine DRMs. De licentie PlayReady afkomstig van PlayReady van Media Services-licentieserver en Widevine licentie wordt geleverd door de licentieserver castLabs." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>Met behulp van castLabs te leveren licenties Widevine Azure Media Services

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u kunt Azure Media Services (AMS) voor het leveren van een gegevensstroom die dynamisch wordt gecodeerd door AMS met PlayReady en Widevine DRMs. De licentie PlayReady afkomstig van PlayReady van Media Services-licentieserver en Widevine licentie wordt geleverd door de licentieserver **castLabs** .

Voor het afspelen van streaming inhoud beschermd door CENC (PlayReady en/of Widevine), kunt u [Mediaspeler Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Zie [AMP document](http://amp.azure.net/libs/amp/latest/docs/) voor meer informatie.

In het volgende diagram ziet u een hoog niveau van Azure Media Services en castLabs integratiearchitectuur.

![integratie](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Typische systeem instellen

- Media-inhoud wordt opgeslagen in de AMS.
- Sleutel-id's van inhoud sleutels worden opgeslagen in zowel castLabs als AMS.
- castLabs en AMS hebben gebouwd token-verificatie. In de volgende secties besproken verificatietokens. 
- Wanneer een clientaanvragen naar de video-stream wordt de inhoud dynamisch gecodeerd met **Gemeenschappelijke codering** (CENC) en dynamisch verpakt door AMS goede Streaming en-streepje. Wij bieden ook PlayReady M2TS elementaire codering voor HLS streaming protocol.
- PlayReady licentie wordt opgehaald uit de licentieserver AMS en Widevine licentie wordt opgehaald uit de castLabs-licentieserver. 
- Media Player bepaalt automatisch welke licentie ophalen op basis van de client platforms. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Verificatie token genereren voor het verkrijgen van een licentie

Ondersteuning voor zowel castLabs als AMS token JWT (JSON Web Token)-indeling gebruikt een licentie toe te staan. 

###<a name="jwt-token-in-ams"></a>JWT token in AMS 

De volgende tabel beschrijft de token JWT in AMS. 

Uitgevende instelling|Tekenreeks van de uitgevende instelling van de gekozen veilige Token Service (STS)
---|---
Doelgroep|Doelgroep-tekenreeks van de gebruikte STS
Vorderingen|Een set van vorderingen
Niet voor|Start de geldigheid van het token
Verloopt|Einde geldigheid van het token
SigningCredentials|De sleutel die wordt gedeeld door de licentieserver PlayReady, castLabs-licentieserver en STS, is het mogelijk symmetrisch of asymmetrisch sleutel.

###<a name="jwt-token-in-castlabs"></a>JWT token in castLabs

De volgende tabel beschrijft de token JWT in castLabs. 

Naam|Beschrijving
---|---
optData|Een JSON-tekenreeks met informatie over u. 
CRT|Een JSON-tekenreeks met informatie over de activa, de licentierechten info en afspelen.
IAT|De huidige datum/tijd in epoch.
jti|Een unieke id over deze token (elke token kan slechts eenmaal worden gebruikt in het systeem castLabs).

##<a name="sample-solution-set-up"></a>Monsteroplossing instellen 

De [oplossing](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) bestaat uit twee projecten:

-   Een console app die DRM beperkingen instellen voor een activum al aangezogen, voor zowel PlayReady als Widevine kan worden gebruikt.
-   Een webtoepassing die in handen van beveiligingstokens die kunnen worden gezien als een zeer vereenvoudigde versie van een STS.


Voor de consoletoepassing:

1.  Wijzig de app.config om referenties AMS, castLabs referenties, STS-configuratie en gedeelde sleutel in te stellen.
2.  Upload een actief naar AMS.
3.  De UUID van de geüploade activa ophalen en 32 van de regel in het bestand Program.cs wijzigen:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Een item-id hebt gebruikt voor de naamgeving van het activum in het castLabs systeem (44 regel in het bestand Program.cs).

    U moet de item-id hebt ingesteld voor **castLabs**; het moet een unieke alfanumerieke tekenreeks.

5.  Het programma uitvoeren.


Gebruik van Web Application (STS):

1.  Het bestand web.config wijzigen om setup castlabs verkoper-ID, de STS-configuratie en de gedeelde sleutel.
2.  Implementeren op Azure Websites.
3.  Ga naar de website.

##<a name="playing-back-a-video"></a>Afspelen van een video

Voor het afspelen van een video gecodeerd met gemeenschappelijke codering (PlayReady en/of Widevine), kunt u de [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Wanneer de console-app wordt uitgevoerd, wordt de inhoud sleutel-ID en de URL van het Manifest geëchood.

1.  Open een nieuw tabblad en start uw STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Ga naar [Azure MediaPlayer](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  In de streaming URL te plakken.
4.  Schakel het selectievakje **Geavanceerde opties** in.
5.  Selecteer in de vervolgkeuzelijst voor **bescherming** , PlayReady en/of Widevine.
6.  Plak het token dat u van uw STS in de textbox Token gekregen. 
    
    De licentieserver castLab hoeft niet de ' aan toonder = "prefix voor het token. Zo moet u die verwijderen alvorens het token.
7.  Windows media player bijwerken.
8.  De video moet worden afgespeeld.


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
