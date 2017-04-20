<properties
   pageTitle="Prestaties verkeer routeringsmethode configureren | Microsoft Azure"
   description="In dit artikel kunt u prestaties verkeer routeringsmethode in beheer van verkeer configureren"
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-performance-traffic-routing-method"></a>Prestaties verkeer routeringsmethode configureren

Om het doorsturen van verkeer voor cloud services en websites (eindpunten) bevinden zich in verschillende datacenters over de hele wereld (regio's), kunt u binnenkomend verkeer naar het eindpunt met de laagste latentie rechtstreeks van de aanvragende client. Het datacenter met de laagste latentie komt doorgaans overeen met het dichtst in de geografische afstand. Prestaties verkeer methode kunt u verdelen op basis van de laagste latentie, maar kan geen rekening worden real-time wijzigingen in de netwerkconfiguratie of laden. Zie [over het beheer van verkeer verkeer routing methoden](traffic-manager-routing-methods.md)voor meer informatie op de routeringsmethoden voor ander verkeer die Azure verkeer Manager biedt.

## <a name="route-traffic-based-on-lowest-latency-across-a-set-of-endpoints"></a>Route-verkeer op basis van de laagste latentie van een set van eindpunten:

1. In de klassieke Azure portal, in het linkerdeelvenster klikt u op het pictogram **Verkeer Manager** om het deelvenster Beheer van verkeer. Als u uw profiel verkeer Manager nog niet hebt gemaakt, ziet u [Verkeer Manager profielen beheren](traffic-manager-manage-profiles.md) voor de stappen voor het maken van een basisprofiel verkeer Manager.
2. In de Azure klassieke portal, in het deelvenster Beheer van verkeer naar het profiel dat verkeer Manager met de instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
3. Op de pagina voor uw profiel **eindpunten** boven aan de pagina op en controleer of de eindpunten van de service die u wilt opnemen in uw configuratie aanwezig zijn. Zie [Eindpunten beheren in beheer van verkeer](traffic-manager-endpoints.md)toe te voegen of eindpunten uit uw profiel verwijderen.
4. Klik op **configureren** voor het openen van de configuratiepagina van boven op de pagina voor uw profiel.
5. Voor **verkeer routeringsinstellingen methode**, controleert u of de routeringsmethode verkeer * *prestaties*. Als dat niet het geval is, klikt u op * *prestaties** in de vervolgkeuzelijst.
6. Controleer of de **Controle-instellingen** op de juiste wijze zijn geconfigureerd. Controleren, zorgt u ervoor dat eindpunten die off line zijn verkeer worden verzonden. Eindpunten controleren, moet u een pad en bestandsnaam opgeven. Merk op dat een forward slash '/' is een ongeldige gegevensinvoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard). Voor meer informatie over het controleren van Zie [Over het beheer van netwerkverkeer controleren](traffic-manager-monitoring.md).
7. Nadat u uw wijzigingen in de configuratie hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
8. De wijzigingen in de configuratie te testen. Zie [Beheer van netwerkverkeer-instellingen testen](traffic-manager-testing-settings.md)voor meer informatie.
9. Als uw serviceprofiel verkeer setup en werken is, bewerken de DNS-record op de gemachtigde DNS-server de domeinnaam van uw bedrijf verwijzen naar de domeinnaam van het beheer van verkeer. Voor meer informatie over hoe u dit doet, Zie [punt een bedrijf internetdomein een domein Manager verkeer](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Volgende stappen


[Wijst een bedrijf internetdomein een domein Manager verkeer](traffic-manager-point-internet-domain.md)

[Methoden voor het beheer van netwerkverkeer doorsturen](traffic-manager-routing-methods.md)

[Failover-methode configureren](traffic-manager-configure-failover-routing-method.md)

[Methode round robin configureren](traffic-manager-configure-round-robin-routing-method.md)

[Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)

[Manager - Disable, enable verkeer of een profiel verwijderen](disable-enable-or-delete-a-profile.md)

[Manager - verkeer uitschakelen of inschakelen van een eindpunt](disable-or-enable-an-endpoint.md)

