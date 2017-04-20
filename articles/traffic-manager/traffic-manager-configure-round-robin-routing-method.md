<properties
   pageTitle="Verkeer Manager configureren round robin verkeer routeringsmethode | Microsoft Azure"
   description="Dit artikel helpt u taakverdeling van round robin voor het beheer van verkeer eindpunten configureren."
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

# <a name="configure-round-robin-routing-method"></a>De routeringsmethode Round-Robin configureren

Een algemene verkeer routing methode patroon is bieden een aantal identieke eindpunten, waaronder cloud services en websites, en verkeer worden verzonden naar elk in een round robin. De volgende stappen uit een overzicht van het uitvoeren van dit type verkeer routeringsmethode verkeer Manager configureren. Zie voor meer informatie over de methoden voor routering van ander verkeer, [over het beheer van verkeer verkeer routeringsmethoden](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Azure Websites biedt al een round robin-taakverdeling functionaliteit voor websites binnen een datacenter (regio). Verkeer Manager kunt u verkeer van round-robin methode voor websites in verschillende datacenters.

## <a name="routing-traffic-equally-round-robin-across-a-set-of-endpoints"></a>Verkeer (round robin) gelijkelijk over een set van eindpunten routering:

1. In de klassieke Azure portal, in het linkerdeelvenster klikt u op het pictogram **Verkeer Manager** om het deelvenster Beheer van verkeer. Als u uw profiel verkeer Manager nog niet hebt gemaakt, ziet u [Verkeer Manager profielen beheren](traffic-manager-manage-profiles.md) voor stapsgewijze instructies voor het maken van een basisprofiel verkeer Manager.
2. In de Azure klassieke portal, in het deelvenster Beheer van verkeer naar het profiel dat verkeer Manager met de instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
3. Op de pagina voor uw profiel **eindpunten** boven aan de pagina op en controleer of de eindpunten van de service die u wilt opnemen in uw configuratie aanwezig zijn. Toevoegen of verwijderen van eindpunten, Zie [Beheren van eindpunten in beheer van verkeer](traffic-manager-endpoints.md).
4. Klik op de profielpagina **configureren** boven de configuratiepagina openen.
5. Voor **verkeer routeringsmethode instellingen**, Controleer of de routeringsmethode verkeer **Round Robin**. Als dat niet het geval is, klikt u in de vervolgkeuzelijst op **Round-Robin** .
6. Controleer of de **Controle-instellingen** op de juiste wijze zijn geconfigureerd. Controleren, zorgt u ervoor dat eindpunten die off line zijn verkeer worden verzonden. Eindpunten controleren, moet u een pad en bestandsnaam opgeven. Merk op dat een forward slash '/' is een ongeldige gegevensinvoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard). Voor meer informatie over het controleren van Zie [Over het beheer van netwerkverkeer controleren](traffic-manager-monitoring.md).
7. Nadat u uw wijzigingen in de configuratie hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
8. De wijzigingen in de configuratie te testen. Zie [Beheer van netwerkverkeer-instellingen testen](traffic-manager-testing-settings.md)voor meer informatie.
9. Als uw serviceprofiel verkeer setup en werken is, bewerken de DNS-record op de gemachtigde DNS-server de domeinnaam van uw bedrijf verwijzen naar de domeinnaam van het beheer van verkeer. Voor meer informatie over hoe u dit doet, Zie [punt een bedrijf internetdomein een domein Manager verkeer](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Volgende stappen


[Wijst een bedrijf internetdomein een domein Manager verkeer](traffic-manager-point-internet-domain.md)

[Methoden voor het beheer van netwerkverkeer doorsturen](traffic-manager-routing-methods.md)

[Failover-methode configureren](traffic-manager-configure-failover-routing-method.md)

[Prestaties routeringsmethode configureren](traffic-manager-configure-performance-routing-method.md)

[Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)

[Manager - Disable, enable verkeer of een profiel verwijderen](disable-enable-or-delete-a-profile.md)

[Manager - verkeer uitschakelen of inschakelen van een eindpunt](disable-or-enable-an-endpoint.md)

