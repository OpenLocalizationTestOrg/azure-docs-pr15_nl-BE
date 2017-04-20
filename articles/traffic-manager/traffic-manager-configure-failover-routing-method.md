<properties
   pageTitle="Configureer Manager verkeer failover verkeer routeringsmethode | Microsoft Azure"
   description="Dit artikel helpt u routeringsmethode voor failover-verkeer configureren in beheer van verkeer"
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

# <a name="configure-failover-routing-method"></a>Failover-methode configureren

Vaak wil een organisatie betrouwbaarheid voor haar diensten. Dit gebeurt door middel van back-up services in het geval de primaire service afgesloten wordt. Een gemeenschappelijk patroon voor failover-service is een reeks van identieke services en verkeer worden verzonden naar een primaire service, terwijl een geconfigureerde lijst met een of meer back-services. U kunt dit type back-up configureren met Azure cloud services en websites door de onderstaande procedures te volgen.

Houd er rekening mee dat Azure Websites al verkeer van failover-functionaliteit voor websites binnen een datacenter (ook bekend als een regio) methode Routering biedt ongeacht de modus van de website. Verkeer Manager kunt u failover-verkeer routeringsmethode voor websites in verschillende datacenters.

## <a name="to-configure-failover-traffic-routing-method"></a>Routeringsmethode voor failover-verkeer configureren:

1. In de klassieke Azure portal, in het linkerdeelvenster klikt u op het pictogram **Verkeer Manager** om het deelvenster Beheer van verkeer. Als u uw profiel verkeer Manager nog niet hebt gemaakt, ziet u [Verkeer Manager profielen beheren](traffic-manager-manage-profiles.md) voor stapsgewijze instructies voor het maken van een basisprofiel verkeer Manager.
2. In het deelvenster Beheer van verkeer in de klassieke Azure portal naar het profiel dat verkeer Manager met de instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
3. Op uw profielpagina **eindpunten** boven aan de pagina op en controleer of dat de beide cloud services en websites (eindpunten) die u wilt opnemen in uw configuratie aanwezig zijn. Toevoegen of verwijderen van eindpunten, Zie [Beheren van eindpunten in beheer van verkeer](traffic-manager-endpoints.md).
4. Klik op de profielpagina **configureren** boven de configuratiepagina openen.
5. Voor **verkeer routeringsinstellingen methode**, controleert u of de routeringsmethode verkeer **Failover**. Als dat niet het geval is, klikt u op **Failover** in de vervolgkeuzelijst.
6. Pas de volgorde voor failover voor de eindpunten voor **Failover-lijst met prioriteiten**. Wanneer u de **Failover** -verkeer routeren methode selecteert, wordt de volgorde van de geselecteerde eindpunten aangelegenheden. Het primaire eindpunt is op de voorgrond. Gebruik de omhoog en omlaag de pijlen om de volgorde te wijzigen indien nodig. Zie voor meer informatie over het instellen van de rangorde voor failover met Windows PowerShell [Set AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Controleer of de **Controle-instellingen** op de juiste wijze zijn geconfigureerd. Controleren, zorgt u ervoor dat eindpunten die off line zijn verkeer worden verzonden. Eindpunten controleren, moet u een pad en bestandsnaam opgeven. Merk op dat een forward slash '/' is een ongeldige gegevensinvoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard). Zie voor meer informatie over het controleren van [Netwerkverkeer Manager Monitoring](traffic-manager-monitoring.md).
8. Nadat u uw wijzigingen in de configuratie hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
9. De wijzigingen in de configuratie te testen. Zie [Instellingen voor beheer van netwerkverkeer testen](traffic-manager-testing-settings.md) voor meer informatie.
10. Als uw serviceprofiel verkeer setup en werken is, bewerken de DNS-record op de gemachtigde DNS-server de domeinnaam van uw bedrijf verwijzen naar de domeinnaam van het beheer van verkeer. Voor meer informatie over hoe u dit doet, Zie [punt een bedrijf internetdomein een domein Manager verkeer](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Volgende stappen

[Wijst een bedrijf internetdomein een domein Manager verkeer](traffic-manager-point-internet-domain.md)

[Methoden voor het beheer van netwerkverkeer doorsturen](traffic-manager-routing-methods.md)

[Methode round robin configureren](traffic-manager-configure-round-robin-routing-method.md)

[Prestaties routeringsmethode configureren](traffic-manager-configure-performance-routing-method.md)

[Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)

[Manager - Disable, enable verkeer of een profiel verwijderen](disable-enable-or-delete-a-profile.md)

[Manager - verkeer uitschakelen of inschakelen van een eindpunt](disable-or-enable-an-endpoint.md)

