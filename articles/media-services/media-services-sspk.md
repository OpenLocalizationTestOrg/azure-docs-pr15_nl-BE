<properties 
    pageTitle="Licentieverlening Microsoft® goede Streaming Client Kit overdragen" 
    description="Meer informatie over hoe u in Microsoft® goede Streaming Client overdragen Kit-licentiëring." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licentieverlening Microsoft® goede Streaming Client Kit overdragen

##<a name="overview"></a>Overzicht

Microsoft goede Streaming Client Porting-Kit (kortweg**SSPK** ) is een goede Streaming client implementatie die is geoptimaliseerd om ingesloten apparaatfabrikanten, kabel en mobiele operators, aanbieders van inhoud, handset fabrikanten, onafhankelijke softwareleveranciers (ISV's) en aanbieders van oplossingen voor het maken van producten en services voor streaming adaptieve streaming inhoud in goede Streaming format. SSPK is een apparaat en platform onafhankelijke uitvoering van goede Streaming client die door de licentiehouder aan elk apparaat en het platform kan worden geïmplementeerd. 

Hieronder is de architectuur van een hoog niveau en IIS goede Streaming Porting-Kit is de goede Streaming Client implementatie door Microsoft geleverd en de kern-logica voor het afspelen van goede Streaming inhoud bevat. Dit wordt vervolgens door de juiste interfaces implementeren door partners voor een bepaald apparaat of platform overgezet. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Beschrijving

SSPK is in licentie gegeven onder voorwaarden die uitstekende zakelijke waarde te bieden. SSPK licentie verleend voor de industrie met:

- Goede bron van Streaming Porting-Kit in C++ 
  - Goede Streaming Client functionaliteit implementeert
  - voegt indeling parseren, heuristiek, buffering logica, enz.
- Toepassing van de Player API 's 
  - API's voor interactie met een toepassing media player
- Platform Abstraction Layer (PAL) Interface 
  - API's voor interactie met het besturingssysteem (threads, sockets)
- Hardware Abstraction Layer (HAL)-Interface 
  - programmeerinterfaces voor interactie met de hardware A / V-decoders (decodering, opbouw)
- Digital Rights Management (DRM)-Interface 
  - API's voor het verwerken van DRM via DRM Abstraction Layer (DAL)
  - Porting-Kit van Microsoft PlayReady apart geleverd maar via deze interface geïntegreerd. Voor meer informatie over licenties voor Microsoft PlayReady Device Klik [hier](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Voorbeelden van de implementatie 
  - Voorbeeldimplementatie PAL voor Linux
  - Voorbeeldimplementatie HAL voor GStreamer

##<a name="licensing-options"></a>Opties voor licentieverlening

Microsoft goede Streaming Client Porting-Kit is beschikbaar gesteld voor licentiehouders onder twee afzonderlijke gebruiksrechtovereenkomst: een voor het ontwikkelen van goede Streaming Client Interim-producten en een andere voor soepele Streaming Client eindproducten te distribueren aan eindgebruikers.
 
- Voor chipsetfabrikanten, systeemintegrators of onafhankelijke softwareleveranciers (ISV's) die behoefte hebben aan een kit voor het ontwikkelen van producten voor Interim overdragen broncode, moet een Microsoft goede Streaming Client Porting-Kit **Productlicentie Interim** worden uitgevoerd.
- Voor fabrikanten of ISV's die distributierechten voor soepele Streaming Client eindproducten aan eindgebruikers nodig hebben, moet de Microsoft goede Streaming Client Porting-Kit **Definitieve productlicentie** worden uitgevoerd.

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft vloeiend Streaming Client Kit Interim productlicentie overdragen

Onder deze licentie biedt Microsoft een goede Streaming Client Porting-Kit en de nodige intellectuele eigendomsrechten ontwikkelen en verspreiden van goede Streaming Client Interim-producten aan andere licentiehouders van goede Streaming Client Porting-Kit apparaat die goede Streaming Client eindproducten te verspreiden.

####<a name="fee-structure"></a>Kostenstructuur

Een eenmalige licentiekosten van US $50.000 biedt toegang tot de soepele Streaming Client Porting-Kit. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft vloeiend Streaming Client Kit Final productlicentie overdragen

Onder deze licentie biedt Microsoft alle nodige intellectuele eigendomsrechten goede Streaming Client Interim-producten ontvangen van andere licentiehouders voor soepele Streaming Client Porting-Kit en bedrijf-merk goede Streaming Client definitieve om producten te distribueren aan eindgebruikers.

####<a name="fee-structure"></a>Kostenstructuur

Het goede Streaming Client eindproduct wordt aangeboden onder een model royalty als onder:

- $0,10 per uitvoering apparaat verzonden
- Royalty is beperkt tot 50.000 euro per jaar
- Geen royalty's voor de eerste 10.000 apparaat implementaties elk jaar 

##<a name="licensing-procedure-and-sspk-access"></a>Procedure voor licentieverlening en SSPK

Stuur een e-mail [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) licenties voor alle query's.

De [verdeling van de SSPK portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) is toegankelijk voor geregistreerde Interim licentiehouders.

Tussentijdse en definitieve SSPK licentiehouders technische vragen te kunnen indienen [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft vloeiend Streaming Client Product tussentijdse overeenkomst licentiehouders

- Adroit Business Solutions, Inc.
- Geavanceerde digitale uitzending SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis Technologies Ltd.
- Alticast Corporation
- Amazon Digital Services, Inc.
- AVC Multimedia Software Co., Ltd.
- Cavium, Inc.
- EchoStar inkoop Corporation
- Enseo, Inc.
- Fluendo S.A.
- HANDAN BroadInfoCom Co., Ltd.
- Infomir GMBH
- Irdeto USA Inc.
- Liberty Global Services BV
- MediaTek Inc.
- MStar Co, Ltd
- NINTENDO Co., Ltd.
- OpenTV, Inc.
- Saffraan digitale beperkt
- Sichuan Changhong elektrische Co., Ltd
- SoftAtHome
- Sony Corporation
- Tatung Technology, Inc.
- TCL-Technoly Electronics (Huizhou) Co., Ltd.
- Vestel Elektronik Sanayi ve Ticaret A.S.
- VisualOn, Inc.
- ZTE Corporation

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft vloeiend Streaming Client eindproduct overeenkomst licentiehouders

- Geavanceerde digitale uitzending SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis Technologies Ltd.
- Amazon Digital Services, Inc.
- AmTRAN Technology Co., Ltd.
- Arcadyan Technology Corporation
- ATMACA ELEKTRONİK SAN. VE TİC. A.Ş
- British Sky Broadcasting beperkt
- CastPal Technology, Inc., Shenzhen
- Compal Electronics, Inc.
- Dongguan digitale AV-technologie Corp., Ltd.
- EchoStar inkoop Corporation
- Enseo, Inc.
- Filmflex films beperkt
- Fluendo S.A.
- Gibson innovaties beperkt
- Haier informatie Applicantion S.R.L
- HANDAN BroadInfoCom Co., Ltd.
- Homecast Co., Ltd
- Hon Hai Precision industrie Co., Ltd.
- Infomir GMBH
- Kaonmedia Co., Ltd.
- KDDI Corporation
- NINTENDO Co., Ltd.
- Oranje SA
- Saffraan digitale beperkt
- Sagemcom breedband SAS
- Shenzhen Coship Electronics CO., LTD
- Shenzhen Jiuzhou elektrische Co., Ltd
- Shenzhen Skyworth Digital Technology Co., Ltd
- Sichuan Changhong elektrische Co., Ltd.
- Skardin industriële Corp.
- Sky Deutschland, Fernsehen GmbH & Co. KG
- SmarDTV S.A.
- SoftAtHome
- Sony Corporation
- Beperkte TCL overzeese Marketing (Offshore Macau commerciële)
- SAS-technologieën leveren Technicolor
- Tongfang Global Ltd.
- Toshiba levensstijl producten & Services Corporation
- Universal Media Corporation /Slovakia/ s.r.o.
- VIZIO, Inc.
- Wistron Corporation
- ZTE Corporation

##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
