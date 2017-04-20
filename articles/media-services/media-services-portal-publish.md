<properties
    pageTitle="  Publiceren van informatie met de Azure portal | Microsoft Azure"
    description="Deze zelfstudie doorloopt u de stappen voor het publiceren van uw inhoud met Azure portal."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>

# <a name="publish-content-with-the-azure-portal"></a>Publiceren van informatie met de Azure portal

> [AZURE.SELECTOR]
- [Portal](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [REST](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Overzicht

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/). 

Om de gebruiker met een URL die kan worden gebruikt om te streamen of te downloaden van uw inhoud, moet u eerst 'publiceren' uw activa door het maken van een locator. Locators bieden toegang tot bestanden in de activa. Media Services ondersteunt twee typen locators: 

- Streaming (OnDemandOrigin)-locators, gebruikt voor adaptieve streaming (bijvoorbeeld naar streepje MPEG stream, HLS of goede Streaming). Als u wilt een streaming locator maken uw bezit, moeten een bestand .ism bevatten. 
- Progressief (SAS)-Locator, die wordt gebruikt voor de levering van video via progressief downloaden.


Een streaming URL heeft de volgende indeling en kunt u deze activa goede Streaming afgespeeld.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Voor het opbouwen van een HLS streaming URL toevoegen (format = m3u8 aapl) naar de URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Voor het opbouwen van een MPEG-streepje streaming URL toevoegen (format = mpd-tijd-csf) naar de URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Een SAS-URL heeft de volgende indeling.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Zie [inhoud leveren-overzicht](media-services-deliver-content-overview.md)voor meer informatie.

>[AZURE.NOTE] Als u de portal locators gemaakt vóór maart 2015, zijn locators met een vervaldatum van twee jaar gemaakt.  

Als u wilt bijwerken met een vervaldatum op een locator [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) of [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API's te gebruiken. Houd er rekening mee dat als u de vervaldatum van een SAS-locator, de URL is gewijzigd.

### <a name="to-use-the-portal-to-publish-an-asset"></a>De portal te gebruiken voor het publiceren van een actief

Voor het gebruik van de portal voor het publiceren van een actief, het volgende doen:

1. Selecteer in de [Azure portal](https://portal.azure.com/)uw Azure Media Services-account.
1. Selecteer **Instellingen** > **activa**.
1. Selecteer het element dat u wilt publiceren.
1. Klik op de knop **publiceren** .
1. Selecteer het type locator.
2. Druk op **toevoegen**.

    ![Publiceren](./media/media-services-portal-vod-get-started/media-services-publish1.png)

De URL wordt toegevoegd aan de lijst met **URL's gepubliceerd**.

## <a name="play-content-from-the-portal"></a>Afspelen vanaf de portal-inhoud

De Azure portal biedt een content-speler die u gebruiken kunt voor het testen van uw video.

Klik op de gewenste video en klik op de knop **afspelen** .

![Publiceren](./media/media-services-portal-vod-get-started/media-services-play.png)

Sommige overwegingen zijn van toepassing:

- Controleer of dat de video is gepubliceerd.
- Deze **Mediaspeler** speelt uit het standaardbeleid voor streaming eindpunt. Als u afspelen vanaf een niet-standaard streaming eindpunt wilt, klikt u op de URL kopiëren en het gebruik van een andere speler. Bijvoorbeeld: [Azure Services mediaspeler](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- De streaming eindpunt waaruit u streaming moet worden uitgevoerd.  
- Als u wilt streamen vanaf een streaming-eindpunt, ten minste één streaming eenheid toe te voegen. Raadpleeg [Dit](media-services-portal-scale-streaming-endpoints.md) onderwerp voor meer informatie.   

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


