<properties 
   pageTitle="Informatie over VPN-apparaten voor verbindingen van Site naar Site VPN-Gateway voor virtuele netwerken Azure | Microsoft Azure"
   description="In dit artikel wordt ingegaan op VPN-apparaten en de IPSec-parameters voor S2S VPN-Gateway-verbindingen en bevat koppelingen naar de configuratie-instructies en voorbeelden."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Informatie over VPN-apparaten voor verbindingen van Site naar Site VPN-Gateway

Een VPN-apparaat is vereist voor het configureren van een Site naar Site (S2S) VPN-verbinding. Verbindingen van site naar Site kunnen worden gebruikt voor het maken van een hybride oplossing of gewenst een beveiligde verbinding tussen uw netwerk in de lokalen en het virtuele netwerk. In dit artikel wordt beschreven compatibele VPN-apparaten en configuratieparameters. 

>[AZURE.NOTE] Bij het configureren van een verbinding van Site naar Site, is een openbare IPv4 IP-adres vereist voor het VPN-apparaat.                                                                                                                                                                               

Als uw apparaat niet wordt weergegeven in de tabel [gevalideerd VPN-apparaten](#devicetable) , Zie de sectie [niet-geverifieerde VPN-apparaten](#additionaldevices) van dit artikel. Het is mogelijk dat het apparaat werkt nog steeds met Azure. Neem contact op met de apparaatfabrikant van uw voor ondersteuning van VPN-apparaat.

**Items als tabellen bekijken:**

- Er is een wijziging van de terminologie voor statische en dynamische routering. U zult waarschijnlijk uitgevoerd in beide termen. Er is geen wijziging in de functionaliteit, alleen de namen wilt wijzigen.
    - Statische routering = PolicyBased
    - Dynamische routering = RouteBased
- Specificaties voor hoge prestaties VPN-gateway en RouteBased VPN-gateway zijn hetzelfde, tenzij anders vermeld. Bijvoorbeeld, zijn gevalideerde VPN-apparaten die compatibel met RouteBased VPN-gateways zijn ook compatibel met de Azure hoge prestaties VPN-gateway. 


## <a name="devicetable"></a>Gevalideerde VPN-apparaten 

We hebben een aantal standaard-VPN-apparaten in partnerschap met leveranciers van apparaat gevalideerd. Alle apparaten in het apparaat families die in de volgende lijst moeten met Azure VPN-gateways werken. Zie [Over VPN-Gateway](vpn-gateway-about-vpngateways.md) om te controleren of het type van de gateway die u moet maken voor de oplossing die u wilt configureren. 

Raadpleeg de koppelingen die met het juiste apparaat familie overeenkomen zodat het VPN-apparaat te configureren. Neem contact op met de apparaatfabrikant van uw voor ondersteuning van VPN-apparaat.



| **Leverancier**                      | **Apparaat-familie**                                        | **Minimale versie van het besturingssysteem**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis                  | AR serie VPN-Routers                                    | 2.9.2                                              | Binnenkort beschikbaar                                                                                                                                                                                                                                          | Niet compatibel                                                                                                                                                                                               |
| Barracuda Networks, Inc.        | Barracuda NextGen Firewall F-serie             | PolicyBased: 5.4.3, RouteBased: 6.2.0  | [Configuratie-instructies](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Configuratie-instructies](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda Networks, Inc.        |  Barracuda NextGen Firewall X-serie                 | Barracuda-Firewall 6.5 | [Barracuda-Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Niet compatibel                                                                                                                                                                                               |
| Brocade                         | 5400 Vyatta vRouter                                      | Virtual Router 6.6R3 NH                            | [Configuratie-instructies](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Niet compatibel                                                                                                                                                                                               |
| Check Point                     | Security Gateway                                         | R75.40, R75.40VS                                     | [Configuratie-instructies](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Configuratie-instructies](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Voorbeelden van Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Niet compatibel                                                                                                                                                                                               |
| Cisco                           | AUTOMATISCH SYSTEEMHERSTEL                                                      | IOS 15.1 (PolicyBased), IOS 15,2 (RouteBased)                | [Voorbeelden van Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Voorbeelden van Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15,0 inch (PolicyBased), IOS 15.1 (RouteBased *)               | [Voorbeelden van Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Cisco monsters *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX, SDX, VPX      |10.1 en hoger                                           | [Integratie-instructies](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Niet compatibel                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ-serie reeks NSA, SuperMassive-reeks E klasse NSA-serie | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Instructies - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instructies - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Instructies - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instructies - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | BIG-IP-reeks                                 |           12.0                                            | [Configuratie-instructies](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Configuratie-instructies](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | 5.2.7 FortiOS                                      | [Configuratie-instructies](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Configuratie-instructies](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internet-initiatief Japan (IIJ) | SEIL-serie                                              | SEIL / X 4,60 4,60 SEIL/B1, 3,20 SEIL/x86            | [Configuratie-instructies](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Niet compatibel                                                                                                                                                                                               |
| Jeneverbes                         | SRX                                                      | JunOS 10.2 (PolicyBased), JunOS 11,4 (RouteBased)            | [Jeneverbes-voorbeelden](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Jeneverbes-voorbeelden](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Jeneverbes                         | J-serie                                                 | JunOS 10.4r9 (PolicyBased), JunOS 11,4 (RouteBased)          | [Jeneverbes-voorbeelden](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Jeneverbes-voorbeelden](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Jeneverbes                         | ISG                                                      | 6.3 (PolicyBased en RouteBased) ScreenOS                  | [Jeneverbes-voorbeelden](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Jeneverbes-voorbeelden](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Jeneverbes                         | SSG                                                      | ScreenOS 6.2 (PolicyBased en RouteBased)                  | [Jeneverbes-voorbeelden](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Jeneverbes-voorbeelden](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Routing and Remote Access-Service                        | Windows Server 2012                                | Niet compatibel                                                                                                                                                                                                                                       | [Voorbeelden van Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Open systemen AG | Mission Control beveiligingsgateway | N.V.T. | [Installatiehandleiding](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Installatiehandleiding](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Binnenkort beschikbaar)                                                                                                                                                                                                                                        | Niet compatibel                                                                                                                                                                                               |
| Palo Alto Networks              | Alle apparaten PAN-besturingssysteem     | 6.1.5 PAN-OS of hoger (PolicyBased), PAN-OS 7.0.5 of hoger (RouteBased)       | [Configuratie-instructies]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Configuratie-instructies](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| Watchguard                      | Alle                                                      | Fireware XTM v11.x                                 | [Configuratie-instructies](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Niet compatibel                                                                                                                                                                                               |

(*) Routers ISR 7200-reeks bieden alleen ondersteuning voor VPN's PolicyBased.

## <a name="additionaldevices"></a>Niet-geverifieerde VPN-apparaten

Als u het apparaat vermeld in de tabel gevalideerd VPN-apparaten niet ziet, werken deze nog steeds met een verbinding van Site naar Site. Controleer of de VPN-apparaat voldoet aan de minimale vereisten die worden beschreven in de sectie Gateway van het artikel [Over VPN-Gateways](vpn-gateway-about-vpngateways.md#gateway-requirements) . Inrichtingen die voldoen aan de minimale vereisten moeten ook goed werken in VPN-gateways. Neem contact op met de fabrikant van het apparaat voor verdere ondersteuning en configuratie-instructies.


## <a name="editing-device-configuration-samples"></a>Monsters van apparaat configuratie bewerken

Na het downloaden van het opgegeven voorbeeld van VPN-apparaat configureren, moet u enkele van de waarden die overeenkomen met de instellingen voor uw omgeving te vervangen. 

**Een voorbeeld van een bewerken:**

1. Het monster met Kladblok openen. 
1. Zoeken en vervangen alle <*tekst*> tekenreeksen met de waarden die betrekking op uw omgeving hebben. Zorg ervoor dat u < en >. Wanneer een naam wordt opgegeven, moet de naam die u selecteert uniek zijn. Als een opdracht niet werkt, raadpleegt u de documentatie van de fabrikant van uw apparaat.

| **Voorbeeldtekst**                | **Wijzigen**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | De naam van uw gekozen voor dit object. Voorbeeld: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | De naam van uw gekozen voor dit object. Voorbeeld: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | De naam van uw gekozen voor dit object. Voorbeeld: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | De naam van uw gekozen voor dit object. Voorbeeld: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | De naam van uw gekozen voor dit object. Voorbeeld: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Bereik opgeven. Voorbeeld: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Subnetmasker opgeven. Voorbeeld: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Geef bereik op gebouwen. Voorbeeld: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Over lokalen subnetmask opgeven. Voorbeeld: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Deze informatie over het virtuele netwerk en bevindt zich in de Portal beheren als een **Gateway-IP-adres**. |
| &lt;SP_PresharedKey&gt;                | Deze informatie is specifiek voor het virtuele netwerk en bevindt zich in de Portal beheren als sleutel beheren.          |



## <a name="ipsec-parameters"></a>IPSec-Parameters

>[AZURE.NOTE] Hoewel de waarden in de volgende tabel worden ondersteund door de VPN-Gateway voor Azure, is er momenteel geen manier om te geven of Selecteer een specifieke combinatie van de Azure VPN-Gateway. Moet u alle beperkingen van het apparaat op ruimten VPN. Bovendien moet u MSS klem op 1350.

### <a name="ike-phase-1-setup"></a>IKE fase 1 setup

| **Eigenschap**                                       | **PolicyBased** | **RouteBased en standaard of hoge prestaties VPN-gateway** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE-versie                                        | IKEv1                          | IKEv2                                                            |
| Diffie-Hellman-groep                               | Groep 2 (1024-bits)             | Groep 2 (1024-bits)                                               |
| Verificatiemethode                              | Vooraf-gedeelde sleutel                 | Vooraf-gedeelde sleutel                                                   |
| Coderingsalgoritmen                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Hash-algoritme                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Fase 1 Security Association (SA) levensduur (tijd) | 28.800 seconden                 | 10,800 seconden                                                   |


### <a name="ike-phase-2-setup"></a>IKE-fase 2-setup

| **Eigenschap**                                                             | **PolicyBased**                 | **RouteBased en standaard of hoge prestaties VPN-gateway**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE-versie                                                              | IKEv1                                          | IKEv2                                                              |
| Hash-algoritme                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Fase 2 Security Association (SA) levensduur (tijd)                        | 3600 seconden                                  | 3600 seconden                                                                  |
| Fase 2 Security Association (SA) levensduur (doorvoer)                  | 102,400,000 KB                                 | -                                                                  |
| IPSec-SA-codering en verificatie biedt (in volgorde van voorkeur) | 1. 2 ESP-AES256. ESP-AES128 3. ESP-3DES 4. N.V.T. | Zie *Gateway RouteBased IPsec SA (Security Association) biedt* (onder) |
| Perfect Forward Secrecy (PFS)                                            | Nee                                             | Geen (*)|
| Dode Peer-detectie                                                      | Niet ondersteund                                  | Ondersteund                                                          |

(*) PFS DH-groep 1, 2, 5, 14, 24, Azure Gateway als IKE responder accepteren.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>RouteBased Gateway IPsec SA (Security Association) biedt

De volgende tabel worden de IPsec SA-codering en verificatie biedt. Aanbiedingen staan de volgorde van voorkeur, dat het voorstel gepresenteerd of geaccepteerd.

| **IPsec SA-codering en verificatie aanbiedingen** | **Azure Gateway als initiatiefnemer**                               | **Azure Gateway als responder**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | SHA1 met ESP AES_128 met null HMAC AH                      |
| 5                                                 | SHA1 met ESP AES_256 met null HMAC AH                      | AH SHA1 met 3_DES ESP met null HMAC                        |
| 6                                                 | SHA1 met ESP AES_128 met null HMAC AH                      | AH MD5 met 3_DES ESP met null HMAC, geen levensduur voorgesteld |
| 7                                                 | AH SHA1 met 3_DES ESP met null HMAC                        | AH SHA1 met ESP 3_DES SHA1, geen levensduur                    |
| 8                                                 | AH MD5 met 3_DES ESP met null HMAC, geen levensduur voorgesteld | AH MD5 met ESP 3_DES MD5, geen levensduur                     |
| 9                                                 | AH SHA1 met ESP 3_DES SHA1, geen levensduur                    | ESP-DES-MD5                                                  |
| 10                                                | AH MD5 met ESP 3_DES MD5, geen levensduur                     | ESP DES SHA1, geen levensduur                                   |
| 11                                                | ESP-DES-MD5                                                  | AH SHA1 met null ESP-DES-HMAC, geen levensduur voorgesteld        |
| 12                                                | ESP DES SHA1, geen levensduur                                   | AH MD5 met null ESP-DES-HMAC, geen levensduur voorgesteld        |
| 13                                                | AH SHA1 met null ESP-DES-HMAC, geen levensduur voorgesteld        | AH SHA1 met ESP DES SHA1, geen levensduur                      |
| 14                                                | AH MD5 met null ESP-DES-HMAC, geen levensduur voorgesteld        | AH MD5 met ESP DES MD5, geen levensduur                       |
| 15                                                | AH SHA1 met ESP DES SHA1, geen levensduur                      | ESP-SHA, geen levensduur                                        |
| 16                                                | AH MD5 met ESP DES MD5, geen levensduur                       | ESP-MD5, geen levensduur                                        |
| 17                                                | -                                                            | AH SHA, geen levensduur                                         |
| 18                                                | -                                                            | AH MD5, geen levensduur                                        |


- U kunt IPSec-ESP met NULL-codering met RouteBased en hoge prestaties VPN-gateways opgeven. NULL-codering op basis biedt geen beveiliging van gegevens tijdens de overdracht en mag alleen worden gebruikt wanneer de maximale doorvoer en minimale wachttijd is vereist.  Clients kunnen kiezen dit in communicatiescenario VNet naar VNet, of wanneer de codering wordt elders in de oplossing wordt toegepast.

- Voor cross-premises verbindingen via het Internet, gebruiken de standaard Azure VPN-gateway-instellingen met codering en hash-algoritmen die worden vermeld in de tabellen hierboven om beveiliging van uw kritieke communicatie te garanderen.





