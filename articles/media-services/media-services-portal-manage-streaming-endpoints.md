<properties 
    pageTitle="Beheren van streaming eindpunten met Azure portal | Microsoft Azure" 
    description="In dit onderwerp wordt beschreven hoe streaming eindpunten met Azure portal beheren." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    writer="juliako" 
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


#<a name="manage-streaming-endpoints-with-the-azure-portal"></a>Streaming eindpunten met Azure portal beheren

## <a name="overview"></a>Overzicht

> [AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u een account Azure. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/). 

In Microsoft Azure Media Services vertegenwoordigt een **Eindpunt Streaming** streaming service die inhoud rechtstreeks naar een clienttoepassing player, of met een inhoud Delivery Network (CDN) voor verdere distributie leveren. Media Services biedt ook een naadloze integratie van de Azure CDN. De uitgaande stream van een StreamingEndpoint-service is een live gegevensstroom of een video op aanvraag activa in uw Media Services-account.

U kunt bovendien bepalen dat de capaciteit van de service Endpoint Streaming voor het verwerken van de groeiende vraag naar bandbreedte door streaming eenheden aan te passen. Het verdient aanbeveling een of meer schaaleenheden toewijzen voor toepassingen in de productie-omgeving. Streaming eenheden bieden u zowel egress toegewezen capaciteit die kan worden gekocht in stappen van 200 Mbps en aanvullende functionaliteit, waaronder: [dynamische verpakking](media-services-dynamic-packaging-overview.md), CDN-integratie en geavanceerde configuratie.

>[AZURE.NOTE]U worden alleen in rekening gebracht als uw eindpunt Streaming in rijklare toestand is.

Dit onderwerp geeft een overzicht van de belangrijkste functies die worden geleverd door Streaming eindpunten. Ook ziet het onderwerp u hoe de Azure portal gebruiken voor het beheren van streaming eindpunten. Raadpleeg [Dit](media-services-portal-scale-streaming-endpoints.md) onderwerp voor meer informatie over het schalen van het eindpunt van de streaming.

## <a name="start-managing-streaming-endpoints"></a>Beginnen met het beheren van streaming eindpunten

Als u wilt beginnen met het beheren van streaming eindpunten voor uw account, door het volgende te doen.

1. Selecteer in de [Azure portal](https://portal.azure.com/)uw Azure Media Services-account.
2. Selecteer in het venster **Instellingen voor** **Streaming-eindpunten**.

    ![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##<a name="adddelete-a-streaming-endpoint"></a>Een streaming eindpunt toevoegen/verwijderen

Streaming eindpunt met de Azure portal toevoegen/verwijderen, als u wilt het volgende doen:

1. Klik op de **+ eindpunt** aan de bovenkant van de pagina als u een streaming-eindpunt. 
2. Als u wilt verwijderen van een streaming-eindpunt, klik op **verwijderen** . 

    De standaard streaming eindpunt kan niet worden verwijderd.
2. Klik op de knop **Start** om te beginnen het streaming-eindpunt.

    ![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

U kunt maximaal twee streaming eindpunten hebben standaard. Als u meer vragen, Zie [quota's en beperkingen](media-services-quotas-and-limitations.md).
    
##<a id="configure_streaming_endpoints"></a>Het eindpunt voor mediagegevensstromen configureren

Eindpunt streaming, kunt u de volgende eigenschappen configureren wanneer u ten minste 1 schaaleenheid: 

- Toegangsbeheer
- Cache-control
- Cross-site-beleid

Zie [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)voor gedetailleerde informatie over deze eigenschappen.

U kunt streaming eindpunt als volgt configureren:

1. Selecteer het streaming-eindpunt dat u wilt configureren.
1. Klik op **Instellingen**.
  
Er volgt een korte omschrijving van de velden.

![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Maximale beleid: configureren voor activa die worden bediend door middel van dit eindpunt streaming-maximumtijd in cache gebruikt. Als er geen waarde is ingesteld, wordt de standaardwaarde gebruikt. De standaardwaarden kunnen ook rechtstreeks in Azure opslag worden gedefinieerd. Als Azure CDN voor het eindpunt streaming is ingeschakeld, moet u de waarde van het beleid voor cache niet ingesteld op minder dan 600 seconden.  

2. Toegestane IP-adressen: geeft u IP-adressen die verbinding maken met de gepubliceerde streaming eindpunt zou worden. Als er geen IP-adressen opgegeven is IP-adres in staat om verbinding te maken. IP-adressen kunnen worden opgegeven als een enkel IP-adres (bijvoorbeeld ' 10.0.0.1"), een IP-adresbereik met een IP-adres en een subnetmasker CIDR (bijvoorbeeld ' 10.0.0.1/22') of een IP-bereik met IP-adres en een subnetmasker voor het gestippelde decimale (bijvoorbeeld ' 10.0.0.1(255.255.255.0)').

3. Configuratie voor de verificatie van Akamai handtekening kop: gebruikt om aan te geven hoe handtekening kop verificatieaanvraag van Akamai servers is geconfigureerd. Vervaldatum is in UTC.



##<a id="enable_cdn"></a>Integratie van de Azure CDN

U kunt opgeven dat de integratie van de Azure CDN voor een eindpunt Streaming (dit is standaard uitgeschakeld.)

De integratie van de Azure CDN instellen op true:

- Het eindpunt streaming moet ten minste één streaming eenheid hebben. Als u later schaaleenheden ingesteld op 0 wilt, moet u eerst de integratie CDN uitschakelen. Bij het maken van een nieuwe streaming is eindpunt een streaming-eenheid standaard automatisch ingesteld.

- Het eindpunt streaming moet een gestopte staat. Met deze eigenschap haalt de CDN ingeschakeld, kunt u het eindpunt streaming starten. 

Het kan tot 90 minuten voor de integratie Azure CDN ingeschakeld duren.  Het duurt twee uur om de wijzigingen op alle de CDN POP's actief zijn.

CDN-integratie is ingeschakeld in de Azure datacenters: ons West, ons Oost-Noord-Europa, West-Europa, Japan, West, Oost-Japan, Zuid-Oost-Azië en Oost-Azië.

Eenmaal is ingeschakeld, wordt de configuratie voor **Toegang** uitgeschakeld.

![Streaming eindpunt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] Integratie van de Azure Media Services met Azure CDN is geïmplementeerd op **Azure CDN van Verizon**.  Als u gebruik wenst te **Azure CDN van Akamai** Azure Media Services, moet u [het handmatig configureren van het eindpunt](../cdn/cdn-create-new-endpoint.md).  Zie voor meer informatie over functies van Azure CDN [CDN-overzicht](../cdn/cdn-overview.md).

###<a name="additional-considerations"></a>Aanvullende overwegingen

- Wanneer CDN voor een streaming-eindpunt is ingeschakeld, kunnen geen clients inhoud rechtstreeks vanaf de oorsprong aanvragen. Als u de mogelijkheid om uw inhoud met of zonder CDN te testen, kunt u een andere streaming endpoint dat niet CDN ingeschakeld.
- De hostnaam van uw streaming eindpunt blijft ongewijzigd na het inschakelen van CDN. U hoeft niet alle wijzigingen aanbrengen in uw werkstroom media services nadat CDN is ingeschakeld. Bijvoorbeeld, als uw streaming eindpunt hostname strasbourg.streaming.mediaservices.windows.net, na het inschakelen van CDN, wordt de exacte dezelfde hostnaam gebruikt.
- Voor nieuwe streaming eindpunten, kunt u een CDN inschakelen door het maken van een nieuw eindpunt; voor bestaande streaming eindpunten moet u eerst het eindpunt te stoppen en de CDN in te schakelen.
 

Zie voor meer informatie, [integratie van de Azure Media Services aangekondigd met Azure CDN (Content Delivery Network)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
