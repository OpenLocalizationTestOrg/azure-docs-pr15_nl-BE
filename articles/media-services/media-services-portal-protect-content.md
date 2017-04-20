<properties 
    pageTitle="Configureren van beleid voor beveiligde inhoud met behulp van de portal Azure | Microsoft Azure" 
    description="In dit artikel wordt beschreven hoe de Azure portal gebruiken voor het configureren van beleid voor beveiligde inhoud. Het artikel bevat ook het inschakelen van dynamische codering voor uw activa." 
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

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Met behulp van de portal Azure inhoudsbeveiliging-beleid configureren

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="overview"></a>Overzicht

Azure Media Services (AMS) kunt u voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via de opslag, verwerking en levering. Media-Services kunt u ervoor zorgen dat de inhoud van uw versleutelde dynamisch met Advanced Encryption Standard (AES) (met 128-bits coderingssleutels), gemeenschappelijke codering (CENC) met behulp van PlayReady en/of Widevine DRM Apple FairPlay. 

AMS biedt een service voor het leveren van DRM-licenties en AES Schakel sleutels aan clients worden. De Azure portal kunt u één **sleutel/licentie Autorisatiebeleid** voor alle typen coderingen maken.

In dit artikel wordt beschreven hoe inhoudsbeveiliging beleid configureren met de Azure portal. Het artikel bevat ook dynamische codering toepassen op uw activa.

> [AZURE.NOTE]  Als u de klassieke Azure portal gebruikt voor het maken van beleid voor bescherming, is het mogelijk dat het beleid niet in de [portal Azure](https://portal.azure.com/). Echter, alle het oude beleid wordt nog steeds aanwezig. U kunt ze met de .NET SDK van Azure Media Services of het hulpprogramma [Azure Media-Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) bekijken (overzicht van het beleid, klik met de rechtermuisknop op de activa -> weergave-informatie (F4) -> Klik op het tabblad inhoud sleutels -> Klik op de sleutel). 
> 
> Als u coderen van de activa met een nieuw beleid wilt, configureert u deze met de Azure portal, klikt u op Opslaan en opnieuw toepassen dynamische codering. 

## <a name="start-configuring-content-protection"></a>Beginnen met het configureren van beveiligde inhoud

Om de portal te starten met het configureren van content bescherming aan uw account AMS algemene met het volgende doen:

1. Selecteer in de [Azure portal](https://portal.azure.com/)uw Azure Media Services-account.
2. Selecteer **Instellingen** > **Content protection**.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Sleutel/licentie beleid

AMS ondersteunt meerdere manieren voor het verifiëren van gebruikers die sleutel of licentie aanvragen. Het verificatiebeleid voor inhoud sleutel moet worden geconfigureerd door u en voldaan door de client om de sleutel/licentie te worden delived naar de client. De inhoud sleutel autorisatiebeleid kan zijn voor een of meer vergunning beperkingen: beperking **openen** of **token** .

De Azure portal kunt u één **sleutel/licentie Autorisatiebeleid** voor alle typen coderingen maken.

###<a name="open"></a>Open 

Open beperking houdt in dat het systeem zal ervoor zorgen dat de sleutel iedereen die sleutel aanvragen. Deze beperking kan handig zijn voor testdoeleinden. 

### <a name="token"></a>Token

Het token beperkte beleid moet vergezeld gaan van een token dat wordt afgegeven door een beveiligde Token Service (STS). Media Services ondersteunt tokens in de eenvoudige Web Tokens (SWT) en JSON Web Token (JWT)-indeling. Media Services biedt geen Token Services beveiligen. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS op tokens probleem. De STS moeten een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de token beperking worden geconfigureerd. De belangrijkste bezorgingsservice Media Services retourneert het aangevraagde sleutel (of licentie) naar de client als de token geldig is en de vorderingen in het token overeenkomen met die welke worden geconfigureerd voor de sleutel (of licentie).

Bij het configureren van het token beleid worden beperkt, moet u de verificatie van de primaire sleutel, uitgever en parameters van de doelgroep. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met, uitgever is de veilige token service het token verleent. De doelgroep (soms scope genoemd) beschrijft de intentie van het token of de resource de token gemachtigd voor toegang tot. De levering van belangrijke Media Services-service wordt gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>Sjabloon voor PlayReady rechten

Zie voor gedetailleerde informatie over de rechten PlayReady sjabloon [Sjabloon overzicht van Media Services PlayReady licentie](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Niet-permanente

Als u de licentie als niet-permanente configureert, het alleen gehouden in het geheugen terwijl de speler met behulp van de licentie.  

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Permanente

Als u de licentie als permanente configureert, wordt deze opgeslagen in het permanente opslag op de client.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Sjabloon voor Widevine rechten

Zie voor gedetailleerde informatie over de sjabloon Widevine rechten [Widevine licentie sjabloon overzicht](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Basic

Wanneer u **standaard**selecteert, wordt de sjabloon gemaakt met de waarden van alle standaardinstellingen.

### <a name="advanced"></a>Geavanceerde

Zie voor uitgebreide uitleg over de optie vooraf Widevine configuraties van [Dit](media-services-widevine-license-template-overview.md) onderwerp.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay configuratie

Om FairPlay codering inschakelt, moet u het App-certificaat en de toepassing geheime sleutel (ASK) via de FairPlay-optie. Zie [Dit](media-services-protect-hls-with-fairplay.md) artikel voor meer informatie over de vereisten en FairPlay-configuratie.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dynamische codering van toepassing op de activa

Als u wilt profiteren van dynamische codering, moet u het volgende doen:

- Het bronbestand te coderen in een verzameling adaptieve bitsnelheid MP4-bestanden.
- Streaming eenheid van ten minste één-op-verzoek voor het streaming eindpunt waaruit u wilt ervoor zorgen dat uw inhoud ophalen. Zie [schalen streaming eenheden gereserveerd op verzoek](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Selecteert een element dat u wilt coderen

Overzicht van alle activa, selecteer **Instellingen** > **activa**.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Coderen met AES of DRM

Indrukken van **coderen** voor een actief, er worden weergegeven met twee opties: **AES** of **DRM**. 

#### <a name="aes"></a>AES

Schakel codering met sleutels wordt ingeschakeld voor alle streaming protocollen AES: goede Streaming HLS en MPEG-streepje.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Als u het DRM-tabblad selecteert, verschijnt er verschillende mogelijkheden van het beleid voor beveiligde inhoud (die u moet nu hebt geconfigureerd) + een verzameling protocollen voor streaming.

- **PlayReady en Widevine met MPEG-streep** - zal uw MPEG-streepje stream met PlayReady en Widevine DRMs dynamisch worden versleuteld.
- **PlayReady en Widevine met MPEG-streep + FairPlay met HLS** - wordt dynamisch codeert u MPEG-streepje stream met PlayReady en Widevine DRMs. De HLS gegevensstromen met FairPlay zal ook worden versleuteld.
- **PlayReady alleen met goede Streaming, HLS en MPEG-streep** - wordt dynamisch coderen goede Streaming HLS, streams met DRM PlayReady MPEG-streepje.
- **Widevine alleen bij MPEG-streep** -, u MPEG-streep met DRM Widevine dynamisch worden gecodeerd.
- **FairPlay alleen met HLS** - wordt dynamisch uw stream HLS FairPlay coderen.

Om FairPlay codering inschakelt, moet u het App-certificaat en de toepassing geheime sleutel (ASK) via de optie FairPlay-configuratie van de instellingen voor beveiligde inhoud blade.

![Inhoud beveiligen](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Als u de coderingsselectie, klikt u op **toepassen**.

##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





