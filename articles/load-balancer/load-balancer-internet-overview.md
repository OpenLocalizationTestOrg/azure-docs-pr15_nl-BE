
<properties
   pageTitle="Internet facing load balancer-overzicht | Microsoft Azure "
   description="Overzicht voor de taakverdeling en de bijbehorende functies toegankelijk via Internet. Hoe werkt een load balancer voor Azure met behulp van virtuele machines en cloud services."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internet-facing-load-balancer-overview"></a>Internet gerichte load balancer-overzicht

Azure taakverdeling wijst het openbare IP-adres en poort nummer binnenkomend verkeer naar het persoonlijke IP-adres en poort nummer van de virtuele machine en vice versa voor het verkeer van de reactie van de virtuele machine. Load balancing regels kunt u specifieke soorten verkeer tussen meerdere virtuele machines of services distribueren. U kunt bijvoorbeeld het laden van webverkeer aanvraag verspreid over meerdere webservers of web-rollen.

Voor een cloud service met exemplaren van web-rollen of functies van de werknemer, kunt u een openbare eindpunt in het service-definitiebestand (.csdef).

Het bestand _servicedefinition.csdef_ bevat de eindpuntconfiguratie en wanneer er meerdere exemplaren van de rol van een web- of werknemer rol distributie, verdeling van de belasting is instellingen. De manier waarop exemplaren toevoegen aan uw cloud-implementatie is het aantal exemplaar op het configuratiebestand (.csfg) van de service wijzigen.

De volgende afbeelding ziet een eindpunt taakverdeling voor gecodeerde Internet-verkeer dat wordt gedeeld door drie virtuele machines voor de openbare en particuliere TCP-poort van 443. Deze drie virtuele machines zijn in een evenwichtige taakverdeling.

![Voorbeeld van de openbare load balancer](./media/load-balancer-internet-overview/IC727496.png))

Figuur 1 - taakverdeling eindpunt voor gecodeerde webverkeer

Als Internet-clients aanvragen voor webpagina's naar het openbare IP-adres van de cloud-service op TCP-poort 443 verzendt, verdeelt de Azure Load Balancer de verzoeken tussen de drie virtuele machines in de taakverdeling. Zie voor meer informatie over load balancer algoritmen, de [verdeling van de pagina te laden](load-balancer-overview.md#load-balancer-features).

Standaard taakverdeling Azure netwerkverkeer wordt gelijkelijk verdeeld over meerdere exemplaren van de virtuele machine. U kunt ook de sessie affiniteit, voor meer informatie, Zie [load balancer distributie-modus](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [interne balancer laadt](load-balancer-internal-overview.md) beter te begrijpen welke taakverdeling is beter geschikt zijn voor de implementatie van de wolk.

U kunt ook [de taakverdeling van een internetverbinding maken](load-balancer-get-started-internet-arm-ps.md) en wat voor soort [distributie-modus](load-balancer-distribution-mode.md) voor een specifieke load balancer netwerkverkeer gedrag configureren.

Als uw toepassing verbindingen voor servers achter een load balancer in leven te houden moet, kunt u meer begrijpen over [niet-actieve TCP-time-outinstellingen voor een load balancer](load-balancer-tcp-idle-timeout.md). Het helpt om te leren over de werking van niet-actieve verbinding wanneer u taakverdeling Azure.
