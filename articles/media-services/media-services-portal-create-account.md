<properties
    pageTitle=" Een Azure Media Services-account maken met Azure portal | Microsoft Azure"
    description="Deze zelfstudie doorloopt u de stappen voor het maken van een account Azure Media Services met Azure portal."
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
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Een Azure Media Services account maken met de Azure portal

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/). 

De Azure portal biedt een manier om snel een Azure Media Services (AMS)-account te maken. U kunt uw account toegang tot Media-Services waarmee u kunt opslaan, codeer coderen, beheren en streamen van media-inhoud in Azure. Tijdens het maken van een Media-Services-account, u ook een account gekoppeld opslag maken (of gebruik een bestaande) in hetzelfde geografische gebied, als de Media Services-account.

In dit artikel worden enkele algemene concepten uitgelegd en wordt aangegeven hoe u een Media-Services-account maken met Azure portal.

## <a name="concepts"></a>Concepten

Toegang tot Media Services vereist twee gekoppelde accounts:

- Een Media-Services-account. Uw account krijgt u toegang tot een aantal cloud-gebaseerde Media Services die beschikbaar in Azure zijn. Werkelijke media-inhoud worden niet opgeslagen met een Media-Services-account. In plaats daarvan worden metagegevens over de media-inhoud en taken van media verwerkt in uw account opgeslagen. Op het moment dat u een account maakt, kunt u een gebied met beschikbare Media Services selecteren. De regio die u selecteert, is een datacenter waarop de records van de metagegevens voor uw account worden opgeslagen.

    Beschikbare Media Services (AMS)-regio's omvatten het volgende: Noord-Europa, West-Europa, West VS, Oost-Verenigde Staten, Zuidoost-Azië, Oost-Azië, Japan West Japan Oost. Media Services gebruikt geen affiniteit groepen.
    
    AMS is nu ook beschikbaar in de volgende datacenters: Zuid-Brazilië, India, West, Zuid-India en India centraal. U kunt nu de Azure portal Media Service accounts maken en verschillende taken die hier worden beschreven. Live-codering is niet ingeschakeld in deze datacenters. Niet alle typen codering gereserveerde eenheden zijn bovendien beschikbaar in deze datacenters.
    
    - Brazilië South: Alleen standaard en Basic Encoding gereserveerde eenheden beschikbaar zijn.
    - India West India Zuid: 

- Een account Azure opslag. Opslag rekeningen moeten zich bevinden in hetzelfde geografische gebied, als de Media Services-account. Als u een Media-Services-account maakt, kunt u een bestaande account voor opslag in dezelfde regio kiezen of kunt u een nieuwe account voor opslag in hetzelfde gebied. Als u een Media-Services-account verwijdert, worden de BLOB's in uw account gerelateerde opslag niet verwijderd.

## <a name="create-an-ams-account"></a>Maak een account AMS

De stappen in deze sectie wordt aangegeven hoe een AMS-account te maken.

1. Aanmelden op de [portal Azure](https://portal.azure.com/).
2. Klik op **+ nieuwe** > **Web + Mobile** > **mediaservices**.

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Voer de vereiste waarden in **MEDIA SERVICES-ACCOUNT maken** .

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Voer in het vak **Accountnaam**de naam van de nieuwe AMS-account. De naam van een Media Services alle kleine letters, cijfers of letters zonder spaties en 3 tot en met 24 tekens lang is.
    2. Selecteer in abonnement, tussen de verschillende Azure abonnementen waartoe u toegang hebt.
    
    2. Selecteer de nieuwe of bestaande resource in **Resourcegroep**.  Een resourcegroep is een verzameling bronnen die delen van de levenscyclus, machtigingen en beleid. Meer informatie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Selecteer in de **locatie**, de geografische regio die wordt gebruikt voor het opslaan van de records media en metagegevens voor uw Media Services-account. Deze regio wordt gebruikt om te verwerken en uw media streamen. De beschikbare Media Services gebieden worden weergegeven in het vak van de vervolgkeuzelijst. 
    
    3. Selecteer een opslag voor blobopslag van de media-inhoud van uw Media Services-account in **Rekening voor opslag**. Kunt u een bestaande account voor opslag in hetzelfde geografische gebied, als uw Media Services-account, of kunt u een account voor opslag. Een nieuwe account voor de opslag wordt gemaakt in dezelfde regio. De regels voor namen van opslag zijn hetzelfde als voor de accounts van Media Services.

        Meer informatie over opslag [hier](storage-introduction.md).

    4. Selecteer **vastmaken aan het dashboard** voor een overzicht van de voortgang van de implementatie rekening.
    
7. Klik op **maken** onder aan het formulier.

    Zodra de account is gemaakt, verandert de status **actief**. 

    ![Instellingen voor Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voor het beheren van uw account AMS (bijvoorbeeld video's te uploaden, activa te coderen, bewaken van de voortgang van het project) het venster **Instellingen** .

## <a name="manage-keys"></a>Sleutels beheren

U moet de naam van de account en de primaire sleutel gegevens via programmering toegang tot de Media Services-account.

1. Selecteer uw account in het portal voor Azure. 

    Het venster **Instellingen** verschijnt aan de rechterkant. 

2. Selecteer in het venster **Instellingen** **toetsen**. 

    De **sleutels beheren** van windows bevat de naam en de primaire en secundaire sleutels worden weergegeven. 
3. Druk op de knop kopiëren om de waarden te kopiëren.
    
    ![Media Services-toetsen](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Volgende stappen

Nu kunt u bestanden uploaden naar uw rekening AMS. Zie voor meer informatie [bestanden uploaden](media-services-portal-upload-files.md).

## <a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


