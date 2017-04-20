<properties
    pageTitle="Wat is verkeer Manager | Microsoft Azure"
    description="Dit artikel helpt u begrijpt wat verkeer Manager is en of het gaat om de juiste verkeer routing keuze voor uw toepassing"
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
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Overzicht van beheer van netwerkverkeer

Microsoft Azure verkeer Manager kunt u de verdeling van de gebruikersverkeer voor de service-endpoints in verschillende datacentra. Ondersteund door verkeer Manager service-endpoints Azure VMs, Web-Apps omvatten en cloud services. U kunt verkeer Manager ook gebruiken met externe, niet-Azure eindpunten.

Domain Name System (DNS) verkeer Manager gebruikt om aanvragen van clients naar het meest geschikte eindpunt op basis van een [methode voor routering van verkeer](traffic-manager-routing-methods.md) en de gezondheid van de eindpunten. Beheer van netwerkverkeer biedt tal van manieren aanpassen aan de behoeften van de andere toepassing eindpunt [monitoring](traffic-manager-monitoring.md)en automatische failover-verkeer routeren. Beheer van netwerkverkeer is veerkrachtig voor storingen, met inbegrip van het mislukken van een hele regio Azure.

## <a name="traffic-manager-benefits"></a>Voordelen van verkeer Manager

Verkeer Manager kan u helpen:

- **Verbetering van de beschikbaarheid van belangrijke toepassingen**

    Verkeer Manager levert maximale beschikbaarheid voor toepassingen door de eindpunten monitoring en automatische failover te bieden als een eindpunt uitvalt.

- **De reactietijd voor krachtige toepassingen verbeteren**

    Azure kunt u cloud-services of websites worden uitgevoerd in datacenters over de hele wereld. Verkeer Manager verbetert de reactiesnelheid van uw toepassing door te verwijzen naar het eindpunt met de laagste netwerkvertraging voor de client.

- **Onderhoud service zonder uitvaltijd**

    U kunt geplande onderhoudsbewerkingen uitvoeren op uw toepassingen zonder uitvaltijd. Beheer van netwerkverkeer zorgt ervoor dat verkeer naar alternatieve eindpunten terwijl het onderhoud uitgevoerd wordt.

- **Combineren van lokalen en Cloud-gebaseerde toepassingen**

    Verkeer Manager ondersteunt externe, niet-Azure eindpunten waardoor deze worden gebruikt in combinatie met hybride cloud en op-gebouwen installaties, met inbegrip van de 'burst-naar-wolk,""migreren-naar-wolk,"en"failover wolk"-scenario's.

- **Verdelen van verkeer voor grote, complexe implementaties**

    Met behulp van [geneste verkeer Manager profielen](traffic-manager-nested-profiles.md)worden routering van verkeer methoden gecombineerd als geavanceerde en flexibele regels ter ondersteuning van de behoeften van grotere, complexe implementaties wilt maken.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [werking van beheer van netwerkverkeer](traffic-manager-how-traffic-manager-works.md).

- Informatie over het hoge beschikbaarheid ontwikkelen met behulp van [verkeer Manager eindpunt controleren](traffic-manager-monitoring.md).

- Meer informatie over [Routering van verkeer methoden](traffic-manager-routing-methods.md) worden ondersteund door verkeer Manager.

- [Een serviceprofiel verkeer maken](traffic-manager-manage-profiles.md).

