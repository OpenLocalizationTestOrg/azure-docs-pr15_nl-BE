<properties
    pageTitle="Beheer van netwerkverkeer routeringsmethoden configureren | Microsoft Azure"
    description="In dit artikel wordt uitgelegd hoe u verschillende routeringsmethoden configureren in beheer van verkeer"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Methoden voor het beheer van netwerkverkeer doorsturen configureren

Azure verkeer Manager biedt drie methoden voor routering van die bepalen hoe verkeer wordt doorgestuurd naar de eindpunten van de beschikbare service. De routering van verkeer methode wordt toegepast op elke DNS-query om te bepalen welk endpoint moet worden geretourneerd in het DNS-antwoord ontvangen.

Er zijn drie methoden voor het doorsturen van verkeer beschikbaar in beheer van verkeer:

- **Prioriteit:** Selecteer 'Priority' wanneer u wilt gebruiken een primaire service-eindpunt en back-ups bieden voor het geval de primaire niet beschikbaar is.
- **Gewogen:** Selecteer 'gewogen' als u wilt verkeer verdelen over een aantal eindpunten, een gelijkmatig of op basis van het gewicht, dat u kunt definiëren.
- **Prestaties:** Selecteer 'Performance' wanneer u eindpunten in verschillende geografische locaties hebt en u wilt dat gebruikers gebruiken de "dichtstbijzijnde" eindpunt van de laagste netwerkvertraging.

## <a name="configure-priority-routing-method"></a>Prioriteit routeringsmethode configureren

Ongeacht de modus website bieden Azure Websites al failover-functionaliteit voor websites binnen een datacenter (regio). Beheer van verkeer is failover voor websites in verschillende datacentra.

Een gemeenschappelijk patroon voor failover-service is het verkeer worden verzonden naar een primaire service en bieden een aantal identieke back-services voor failover. De volgende stappen wordt uitgelegd hoe deze prioriteit failover configureren met Azure cloud services en websites:

1. In de klassieke Azure portal, in het linkerdeelvenster klikt u op het pictogram **Verkeer Manager** om het deelvenster Beheer van verkeer.
2. Zoek het profiel verkeer Manager met de instellingen die u wilt wijzigen in het deelvenster Beheer van verkeer in de klassieke Azure portal. U opent de pagina profiel instellingen, klikt u op de pijl rechts van de naam van het profiel.
3. Klik op de profielpagina **eindpunten** boven aan de pagina. Controleer of de cloud-services en de websites die u wilt opnemen in uw configuratie aanwezig zijn.
4. Klik op **configureren** voor het openen van de configuratiepagina van boven.
5. Voor **verkeer routeringsinstellingen methode**, controleert u of de routeringsmethode verkeer **Failover**. Als dat niet het geval is, klikt u op **Failover** in de vervolgkeuzelijst.
6. Pas de volgorde voor failover voor de eindpunten voor **Failover-lijst met prioriteiten**. Wanneer u de **Failover** -verkeer routeren methode selecteert, wordt de volgorde van de geselecteerde eindpunten aangelegenheden. Het primaire eindpunt is op de voorgrond. Gebruik de omhoog en omlaag de pijlen om de volgorde te wijzigen indien nodig. Zie voor meer informatie over het instellen van de rangorde voor failover met Windows PowerShell [Set AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Controleer of de **Controle-instellingen** op de juiste wijze zijn geconfigureerd. Controleren, zorgt u ervoor dat eindpunten die off line zijn verkeer worden verzonden. U moet een pad en bestandsnaam opgeven voor het controleren van de eindpunten. Een schuine streep naar voren "/" is een geldige vermelding voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard).
8. Nadat u uw wijzigingen in de configuratie hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
9. De wijzigingen in de configuratie te testen.
10. Zodra uw profiel verkeer Manager werkt, DNS-record op de gemachtigde DNS-server de domeinnaam van uw bedrijf verwijzen naar de domeinnaam van het beheer van verkeer bewerken.

## <a name="configure-weighted-routing-method"></a>Gewogen routeringsmethode configureren

Een algemene verkeer routing methode patroon is bieden een aantal identieke eindpunten, waaronder cloud services en websites, en verkeer worden verzonden naar elk in een round robin. De volgende stappen geven een overzicht van het configureren van dit type verkeer routeringsmethode.

>[AZURE.NOTE] Azure Websites bieden al round robin-taakverdeling functionaliteit voor websites binnen een datacenter (regio). Verkeer Manager kunt u verkeer van round-robin methode voor websites in verschillende datacenters.

1. In de klassieke Azure portal, in het linkerdeelvenster klikt u op het pictogram **Verkeer Manager** om het deelvenster Beheer van verkeer.
2. Zoek het profiel verkeer Manager met de instellingen die u wilt wijzigen in het deelvenster Beheer van verkeer. U opent de pagina profiel instellingen, klikt u op de pijl rechts van de naam van het profiel.
3. Op de pagina voor uw profiel **eindpunten** boven aan de pagina op en controleer of de eindpunten van de service die u wilt opnemen in uw configuratie aanwezig zijn.
4. Klik op de profielpagina **configureren** boven de configuratiepagina openen.
5. Voor **verkeer routeringsmethode instellingen**, Controleer of de routeringsmethode verkeer **Round Robin**. Als dat niet het geval is, klikt u in de vervolgkeuzelijst op **Round-Robin** .
6. Controleer of de **Controle-instellingen** op de juiste wijze zijn geconfigureerd. Controleren, zorgt u ervoor dat eindpunten die off line zijn verkeer worden verzonden. U moet een pad en bestandsnaam opgeven voor het controleren van de eindpunten. Een schuine streep naar voren "/" is een geldige vermelding voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard).
7. Nadat u uw wijzigingen in de configuratie hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
8. De wijzigingen in de configuratie te testen.
9. Zodra uw profiel verkeer Manager werkt, DNS-record op de gemachtigde DNS-server de domeinnaam van uw bedrijf verwijzen naar de domeinnaam van het beheer van verkeer bewerken.

## <a name="configure-performance-traffic-routing-method"></a>Prestaties verkeer routeringsmethode configureren

Prestaties verkeer methode kunt u rechtstreeks verkeer naar het eindpunt met de laagste latentie van het netwerk van de client. Het datacenter met de laagste latentie is meestal het dichtst in de geografische afstand. Deze methode verkeer kan rekening voor realtime wijzigingen in de netwerkconfiguratie of laden.

1. In de klassieke Azure portal, in het linkerdeelvenster klikt u op het pictogram **Verkeer Manager** om het deelvenster Beheer van verkeer.
2. Zoek het profiel verkeer Manager met de instellingen die u wilt wijzigen in het deelvenster Beheer van verkeer. U opent de pagina profiel instellingen, klikt u op de pijl rechts van de naam van het profiel.
3. Op de pagina voor uw profiel **eindpunten** boven aan de pagina op en controleer of de eindpunten van de service die u wilt opnemen in uw configuratie aanwezig zijn.
4. Klik op **configureren** voor het openen van de configuratiepagina van boven op de pagina voor uw profiel.
5. Voor **verkeer routeringsinstellingen methode**, controleert u of de routeringsmethode verkeer * *prestaties*. Als dat niet het geval is, klikt u op * *prestaties** in de vervolgkeuzelijst.
6. Controleer of de **Controle-instellingen** op de juiste wijze zijn geconfigureerd. Controleren, zorgt u ervoor dat eindpunten die off line zijn verkeer worden verzonden. U moet een pad en bestandsnaam opgeven voor het controleren van de eindpunten. Een schuine streep naar voren "/" is een geldige vermelding voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard).
7. Nadat u uw wijzigingen in de configuratie hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
8. De wijzigingen in de configuratie te testen.
9. Zodra uw profiel verkeer Manager werkt, DNS-record op de gemachtigde DNS-server de domeinnaam van uw bedrijf verwijzen naar de domeinnaam van het beheer van verkeer bewerken.

## <a name="next-steps"></a>Volgende stappen

* [Verkeer profielen beheren](traffic-manager-manage-profiles.md)
* [Methoden voor het beheer van netwerkverkeer doorsturen](traffic-manager-routing-methods.md)
* [Beheer van netwerkverkeer-instellingen testen](traffic-manager-testing-settings.md)
* [Wijst een bedrijf internetdomein een domein Manager verkeer](traffic-manager-point-internet-domain.md)
* [Beheer van netwerkverkeer eindpunten beheren](traffic-manager-manage-endpoints.md)
* [Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)
