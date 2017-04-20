<properties
    pageTitle="Service Bus relay-overzicht | Microsoft Azure"
    description="Overzicht van de relay Service Bus."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Overzicht van de relay Service Bus

Een belangrijk onderdeel van de Bus-Service is een centrale (maar zeer verdeeld) *relay* service waarmee u kunt bouwen hybride toepassingen die worden uitgevoerd in een datacenter Azure en uw eigen bedrijfsomgeving op gebouwen.  De relay Service Bus ondersteunt een groot aantal verschillende transportprotocollen en normen. Dit geldt ook voor SOAP, WS-*, en zelfs de REST. De relay-service vereenvoudigt uw hybride toepassingen doordat u veilig blootstellen Windows Communication Foundation (WCF) services die zich bevinden binnen een onderneming corporate netwerk naar de openbare cloud zonder open firewallverbinding met een, of opdringerige wijzigingen in een bedrijfsnetwerkinfrastructuur vereist. 

![Relay-concepten](./media/service-bus-relay-overview/sb-relay-01.png)

De relayservice ondersteunt de traditionele unilaterale messaging, aanvraag/reactie-berichten en peer-to-peer berichten. Het ondersteunt ook de verdeling van de gebeurtenis in het internet-bereik om scenario's publiceren/abonneren en socket bi-directionele communicatie voor point-to-point verhoogde efficiëntie. 

In het patroon van het indirecte messaging, een service op locatie verbinding maakt met de relay-service via een uitgaande poort en maakt een bi-directionele socket voor communicatie is gebonden aan een bepaalde rendezvous-adres. De client kan vervolgens communiceren met de service op locatie door berichten te sturen naar de relayservice gericht op het adres rendezvous. De relay-service wordt vervolgens "" berichten doorsturen naar de on-premises-service via de bi-directionele socket reeds. De client niet een directe verbinding met de service op locatie nodig heeft, hoeft niet te weten waar de service is geïnstalleerd en de service op locatie hoeft niet alle poorten voor inkomend verkeer geopend in de firewall.

U de verbinding tot stand brengen tussen uw lokale service en de relayservice via een reeks WCF 'relay'-bindingen. Achter de schermen wordt de relay-bindingen toewijzen aan nieuwe transport bindende elementen ontworpen WCF kanaal componenten die worden geïntegreerd met de Bus in de cloud-Service maken. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de relay Service Bus.

- [Azure Service Bus structuuroverzicht](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Het gebruik van de Service Bus Relay-service](service-bus-dotnet-how-to-use-relay.md)

 