<properties
   pageTitle="ExpressRoute klant router configuratie voorbeelden | Microsoft Azure"
   description="Deze pagina bevat voorbeelden van router-config voor routers van Cisco en jeneverbes."
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

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Monsters van de router configuratie instellen en beheren van Routering

Deze pagina biedt interface en routing configuratie voorbeelden voor de routers van Cisco IOS-XE en jeneverbes MX-serie. Deze zijn bedoeld als voorbeelden alleen ter indicatie en moeten niet worden gebruikt. U kunt werken met uw leverancier om geschikte configuraties voor uw netwerk. 

>[AZURE.IMPORTANT] Voorbeelden op deze pagina zijn bedoeld als louter ter indicatie. U moet werken met de verkoop / technische team van de leverancier en uw netwerk team verzinnen van geschikte configuraties aan uw wensen voldoet. Microsoft ondersteunt geen problemen met betrekking tot configuraties die op deze pagina worden vermeld. Neem contact op met de leverancier van uw apparaat voor ondersteuning.

Router configuratie met de onderstaande voorbeelden zijn van toepassing op alle peerings. [ExpressRoute peerings](expressroute-circuit-peerings.md) en [ExpressRoute routering vereisten](expressroute-routing.md) bekijken voor meer informatie over routering.

## <a name="cisco-ios-xe-based-routers"></a>Routers op basis van Cisco IOS-XE

De voorbeelden in deze sectie van toepassing voor elke router de IOS-XE OS-familie wordt uitgevoerd.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. onderliggende interfaces en interfaces configureren

U moet een sub-interface per peering in elke router dat u verbinding met Microsoft maakt. Een sub-interface kan worden geïdentificeerd met een VLAN-ID of een combinatie van een gestapelde VLAN-id's en een IP-adres.

#### <a name="dot1q-interface-definition"></a>Definitie van de Dot1Q-interface

In dit voorbeeld worden de onderliggende interfacedefinitie voor een onderliggende interface met een enkele VLAN-ID. De VLAN-ID is uniek voor een peering. Het laatste octet van het IPv4-adres is een oneven getal.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>Definitie van de QinQ-interface

In dit voorbeeld worden de onderliggende interfacedefinitie voor een interface met een twee VLAN-id's voor onderdelen. De buitenste VLAN-ID (s-code), als gebruikt blijft hetzelfde op alle peerings. De binnenste (c-code) van de VLAN-ID is uniek per peering. Het laatste octet van het IPv4-adres is een oneven getal.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. instellen eBGP-sessies

U moet voor elke peering een BGP-sessie met Microsoft setup. Het onderstaande voorbeeld kunt u voor het instellen van een BGP-sessie met Microsoft. Als het IPv4-adres dat u voor de sub-interface gebruikt a.b.c.d is, worden het IP-adres van de neighbor BGP (Microsoft) a.b.c.d+1. Het laatste octet van het BGP-neighbor IPv4-adres is altijd een even getal.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. het instellen van voorvoegsels geadverteerd via het BGP-sessie

U kunt de router selecteert prefixen aan Microsoft. U kunt dit doen met behulp van het voorbeeld hieronder.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. de route is toegewezen

Kunt u route kaarten en voorvoegsel lijsten op filter voorvoegsels in uw netwerk worden doorgegeven. Het onderstaande voorbeeld kunt u de taak uit te voeren. Zorg ervoor dat u beschikt over de juiste voorvoegsel lijsten instellen.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Jeneverbes MX series routers 

De voorbeelden in deze sectie van toepassing voor routers jeneverbes MX-serie.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. onderliggende interfaces en interfaces configureren

#### <a name="dot1q-interface-definition"></a>Definitie van de Dot1Q-interface

In dit voorbeeld worden de onderliggende interfacedefinitie voor een onderliggende interface met een enkele VLAN-ID. De VLAN-ID is uniek voor een peering. Het laatste octet van het IPv4-adres is een oneven getal.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>Definitie van de QinQ-interface

In dit voorbeeld worden de onderliggende interfacedefinitie voor een interface met een twee VLAN-id's voor onderdelen. De buitenste VLAN-ID (s-code), als gebruikt blijft hetzelfde op alle peerings. De binnenste (c-code) van de VLAN-ID is uniek per peering. Het laatste octet van het IPv4-adres is een oneven getal.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. instellen eBGP-sessies

U moet voor elke peering een BGP-sessie met Microsoft setup. Het onderstaande voorbeeld kunt u voor het instellen van een BGP-sessie met Microsoft. Als het IPv4-adres dat u voor de sub-interface gebruikt a.b.c.d is, worden het IP-adres van de neighbor BGP (Microsoft) a.b.c.d+1. Het laatste octet van het BGP-neighbor IPv4-adres is altijd een even getal.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. het instellen van voorvoegsels geadverteerd via het BGP-sessie

U kunt de router selecteert prefixen aan Microsoft. U kunt dit doen met behulp van het voorbeeld hieronder.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. de route is toegewezen

Kunt u route kaarten en voorvoegsel lijsten op filter voorvoegsels in uw netwerk worden doorgegeven. Het onderstaande voorbeeld kunt u de taak uit te voeren. Zorg ervoor dat u beschikt over de juiste voorvoegsel lijsten instellen.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie.
