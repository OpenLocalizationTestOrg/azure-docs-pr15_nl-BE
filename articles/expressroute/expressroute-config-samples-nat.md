<properties
   pageTitle="ExpressRoute klant router configuratie voorbeelden | Microsoft Azure"
   description="Deze pagina bevat een router configuratie monsters voor de routers van Cisco en jeneverbes."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Monsters van de router configuratie instellen en beheren van NAT

Deze pagina bevat voorbeelden van NAT-configuratie voor routers van Cisco ASA en jeneverbes SRX series. Deze zijn bedoeld als voorbeelden alleen ter indicatie en moeten niet worden gebruikt. U kunt werken met uw leverancier om geschikte configuraties voor uw netwerk. 

>[AZURE.IMPORTANT] Voorbeelden op deze pagina zijn bedoeld als louter ter indicatie. U moet werken met de verkoop / technische team van de leverancier en uw netwerk team verzinnen van geschikte configuraties aan uw wensen voldoet. Microsoft ondersteunt geen problemen met betrekking tot configuraties die op deze pagina worden vermeld. Neem contact op met de leverancier van uw apparaat voor ondersteuning.

Router configuratie met de onderstaande voorbeelden zijn van toepassing op openbare Azure en Microsoft peerings. U moet niet NAT voor Azure private peering configureren. [ExpressRoute peerings](expressroute-circuit-peerings.md) en [ExpressRoute NAT vereisten](expressroute-nat.md) bekijken voor meer informatie.

**Opmerking:** Voor een verbinding met het internet en ExpressRoute moet u afzonderlijke NAT IP-adresgroepen. Met behulp van dezelfde IP NAT-toepassingen via het internet en ExpressRoute leidt asymmetrische Routering en verlies van connectiviteit.

## <a name="cisco-asa-firewalls"></a>Cisco ASA firewall

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>PAT configuratie voor het verkeer van het netwerk van de klant naar Microsoft

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>PAT configuratie voor het verkeer van Microsoft naar het netwerk van de klant

#### <a name="interfaces-and-direction"></a>Interfaces en richting:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Configuratie:
NAT-adresgroep:

    object network outbound-PAT
        host <NAT-IP>

Doelserver:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Object-groep voor IP-adressen van klanten

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT-opdrachten:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Jeneverbes SRX series routers 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. redundante Ethernet-interfaces voor het cluster maken

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. Maak twee beveiligingszones

 - Vertrouwde Zone voor het interne netwerk en Untrust Zone voor externe netwerk tegenoverliggende rand Routers
 - Geschikte interfaces aan de zones toewijzen
 - Services op de interfaces toestaan


    beveiliging {zones-beveiligingszone Vertrouwde {{-inkomend-hostverkeer {-systeemservices {ping;                  } {bgp; protocollen                  interfaces}} {reth0.100;              }} beveiligingszone Untrust {-inkomend-hostverkeer {-systeemservices {ping;                  } {bgp; protocollen                  interfaces}} {reth1.100;              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. beleid tussen de zones maken
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. NAT-beleid configureren
 - Twee groepen van NAT maken. Een wordt gebruikt voor verkeer voor NAT uitgaande aan Microsoft en andere Microsoft aan de klant.
 - Regels NAT maken de respectieve verkeer

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. BGP om selectief prefixen in elke richting configureren

Zie voorbeelden in [Routing configuratie](expressroute-config-samples-routing.md) -voorbeelden.

### <a name="6-create-policies"></a>6. beleid maken

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie.
