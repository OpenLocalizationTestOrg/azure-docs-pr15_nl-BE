<properties
    pageTitle="VPN-verbindingen in Azure API Management instellen"
    description="Informatie over het instellen van een VPN-verbinding in Azure API beheer- en webservices toegang via het."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>VPN-verbindingen in Azure API Management instellen

API van VPN-ondersteuning kunt u verbinding maken Management API vindt u een virtueel netwerk van Azure (classic). Zo kunnen klanten API beheer veilig verbinding maken met hun back-end-webservices die worden op de bedrijfsruimten of anders niet toegankelijk zijn voor het openbare internet.

>[AZURE.NOTE] Azure API Management werkt met klassieke VNETs. Zie voor meer informatie over het maken van een klassieke VNET [(klassiek) met behulp van de Portal Azure virtueel netwerk maken](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Zie voor informatie over het klassieke VNETs verbinding te maken met de ARM VNETS, [klassieke VNets verbinding maken met nieuwe VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Inschakelen VPN-verbindingen

>VPN-verbindingen is alleen beschikbaar in de niveaus van **Premium** en **Developer** . Als u wilt overschakelen, open uw service Management API in de [Klassieke Portal Azure][] en open vervolgens het tabblad **schaal** . Selecteer de premie laag onder de sectie **Algemeen** en klik op opslaan.

Uw service Management API in de [Klassieke Portal Azure][] openen en overschakelen naar het tabblad **configureren** zodat de VPN-verbinding. 

Onder de sectie VPN-Activeer **VPN-verbinding** **op**.

![Tabblad van de beheer-API exemplaar configureren][api-management-setup-vpn-configure]

U ziet nu een lijst van alle regio's waar uw service Management API is ingericht.

Selecteer een VPN en het subnetmasker voor elke regio. De lijst met VPN-verbindingen wordt gevuld op basis van de virtuele netwerken beschikbaar is in uw abonnement Azure die zijn ingesteld in de regio die u wilt configureren.

![Selecteer VPN][api-management-setup-vpn-select]

Klik op **Opslaan** onder aan het scherm. Niet mogelijk voor andere bewerkingen uitvoeren op de API-Management-service van de klassieke Portal Azure terwijl deze wordt bijgewerkt. De gateway service beschikbaar blijven en runtime oproepen geen invloed op.

Houd er rekening mee dat de VIP-adres van de gateway wordt gewijzigd wanneer VPN is ingeschakeld of uitgeschakeld.

## <a name="connect-vpn"> </a>Verbinding maken met een webservice achter VPN

Nadat u uw service Management API is aangesloten op de VPN-verbinding, is toegang tot de webservices binnen het virtuele netwerk niet anders dan de toegang tot openbare diensten. Typ in het lokale adres of de hostnaam van uw webservice (als een DNS-server is geconfigureerd voor het virtuele netwerk Azure) in het veld **URL van Web** bij het maken van een nieuwe API of een bestaande bewerken.

![API van VPN toevoegen][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Vereiste poorten voor API beheer van VPN-ondersteuning

Wanneer een instantie API Management-service wordt gehost in een VNET, worden de poorten in de volgende tabel worden gebruikt. Als deze poorten zijn geblokkeerd, is het mogelijk dat de service niet goed werkt. Met een of meer van deze poorten geblokkeerd is het meest voorkomende probleem met onjuiste configuratie Management API gebruiken met een VNET.

| Poort(en)                      | Richting        | Transportprotocol | Doel                                                          | Bron / bestemming              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443                      | Inkomend          | TCP                | Clientcommunicatie beheer-API                           | INTERNET / VIRTUAL_NETWORK        |
| 80,443                       | Uitgaande         | TCP                | API beheer afhankelijkheid van Azure opslag en Azure Service Bus | VIRTUAL_NETWORK / INTERNET        |
| 1433                         | Uitgaande         | TCP                | Beheer-API afhankelijkheden van SQL                               | VIRTUAL_NETWORK / INTERNET        |
| 9350, 9351, 9352, 9353, 9354 | Uitgaande         | TCP                | Afhankelijkheden op Bus Service Management API                       | VIRTUAL_NETWORK / INTERNET        |
| 5671                         | Uitgaande         | AMQP               | Afhankelijkheid van de beheer-API voor logboek gebeurtenis Hub beleid            | VIRTUAL_NETWORK / INTERNET        |
| 6381, 6382, 6383             | Inkomende/uitgaande | UDP                | Beheer-API afhankelijk is van de Cache bestand Vgx.                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Uitgaande         | TCP                | API beheer afhankelijkheid van Azure bestandsshare voor GIT            | VIRTUAL_NETWORK / INTERNET        |

## <a name="custom-dns"> </a>Aangepaste DNS-server setup

Beheer-API is afhankelijk van een aantal Azure services. Wanneer een instantie API Management-service wordt gehost in een VNET waarin een aangepaste DNS-server wordt gebruikt, moet deze kunnen herleiden van hostnamen die gebruikmaken van deze diensten Azure. Volg [deze](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) instructies op de aangepaste DNS-instellingen.  

## <a name="related-content"> </a>Verwante inhoud


* [Een virtueel netwerk maken met een site naar site VPN-verbinding met de klassieke Azure-Portal][]
* [Het gebruik van de API-inspecteur traceren roept in Azure API beheer][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure klassieke Portal]: https://manage.windowsazure.com/

[Een virtueel netwerk maken met een site naar site VPN-verbinding met de klassieke Azure-Portal]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Het gebruik van de API-inspecteur traceren roept in Azure API beheer]: api-management-howto-api-inspector.md
