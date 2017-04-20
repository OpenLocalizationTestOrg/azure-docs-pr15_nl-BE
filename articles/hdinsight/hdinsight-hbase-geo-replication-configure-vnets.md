<properties 
   pageTitle="Configureer de VPN-verbinding tussen twee virtuele netwerken | Microsoft Azure" 
   description="Meer informatie over het configureren van VPN-verbindingen en domeinnamen omzetten tussen twee Azure virtuele netwerken en HBase geo-replicatie configureren." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Een VPN-verbinding tussen twee Azure virtuele netwerken configureren  

> [AZURE.SELECTOR]
- [VPN-verbindingen configureren](hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS configureren](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replicatie configureren](hdinsight-hbase-geo-replication.md) 

Azure virtuele website netwerkverbinding wordt gebruikt als een VPN-gateway een beveiligde tunnel met Ipsec/IKE. De VNets kan zich in verschillende abonnementen en de verschillende regio's. U kunt zelfs VNet combineren VNet communicatie met configuraties met meerdere sites. Er zijn verschillende redenen voor het VNet en de VNet controleren:

- Cross-regio geo-redundantie en geo-aanwezigheid 
- Regionale meerlagige toepassingen met sterke isolatie grens 
- Cross-communicatie tussen organisatie in Azure-abonnement

Zie [een VNet aan VNet-verbinding configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)voor meer informatie. 

Om te zien op de video:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Deze zelfstudie is een onderdeel van de [reeks] [ hdinsight-hbase-replication] over het maken van HBase geo-replicatie. 

- Configureren van een VPN-verbinding tussen twee virtuele netwerken (deze zelfstudie)
- [DNS configureren voor de virtuele netwerken][hdinsight-hbase-geo-replication-dns]
- [HBase geo-replicatie configureren][hdinsight-hbase-geo-replication]

In het volgende diagram ziet u de twee virtuele netwerken die in deze zelfstudie wordt gemaakt:

![HDInsight HBase replicatie virtuele netwerkdiagram][img-vnet-diagram]
 

##<a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**.

    Voordat u PowerShell scripts, moet dat u verbonden bent met uw Azure-abonnement met de volgende cmdlet:

        Add-AzureAccount

    Als er meerdere Azure abonnementen, gebruikt u de volgende cmdlet instellen van het huidige abonnement:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Servicenamen van Azure en virtuele machine moet uniek zijn. De naam die wordt gebruikt in deze zelfstudie wordt Contoso-[naam van Azure Service/VM]-[EU / VS]. Contoso VNet EU is bijvoorbeeld de Azure virtueel netwerk in het datacenter van Noord-Europa; Contoso-DNS-US is de DNS-server VM in het datacenter Oost-VS. U moet afkomstig zijn van uw eigen namen.
 

##<a name="create-two-azure-vnets"></a>Maak twee Azure-VNets



**Voor het maken van een virtueel netwerk genaamd Contoso VNet EU in Noord-Europa**

1.  Aanmelden bij de [Azure klassieke Portal][azure-portal].
2.  Klik op **Nieuw**, **NETWERKSERVICES**, **VIRTUEEL netwerk**, **aangepaste maken**.
3.  Voer in:

    - **Naam**: Contoso-VNet-EU
    - **Locatie**: Noord-Europa

        In deze zelfstudie wordt Noord-Europa en Oost-VS datacenters. U kunt uw eigen datacenters.
4.  Voer in:

    - **DNS-SERVER**: (deze leeg laten) 
    
        U moet uw eigen DNS-server voor naamomzetting in virtuele netwerken. Zie voor meer informatie over het gebruik naam van Azure resolutie en wanneer gebruikt u uw eigen DNS-server [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Zie [DNS tussen twee Azure virtuele netwerken configureren]voor instructies voor het configureren van naamomzetting tussen VNets,[hdinsight-hbase-dns].
  
    - **Een punt-naar-site VPN configureren**: (uitgeschakeld)

        Punt-tot-site geldt niet voor dit scenario.

    - **Configureren van een VPN website**: (uitgeschakeld)
    
        In het datacenter van de Oost-Amerikaanse configureert u de VPN-verbinding van site naar site met de Azure virtueel netwerk.
5.  Voer in:

    -   **Adres ruimte STARTING IP**: 10.1.0.0
    -   **Adres ruimte CIDR**: / 16
    -   **IP subnet 1 starten**: 10.1.0.0
    -   **Subnet 1 CIDR**: / 24

    De adresruimte kan niet overlappen met de Amerikaanse virtueel netwerk.  

**Voor het maken van een virtueel netwerk genaamd Contoso VNet EU in West-Europa**

- Herhaal de laatste procedure met de volgende waarden:

    - **Naam**: Contoso-VNet-US
    - **Locatie**: Oost-VS
     
    - **DNS-SERVER**: (deze leeg laten)
    - **Een punt-naar-site VPN configureren**: (uitgeschakeld)
    - **Configureren van een VPN website**: (uitgeschakeld)
     
    - **Adres ruimte STARTING IP**: 10.2.0.0
    - **Adres ruimte CIDR**: / 16
    - **IP subnet 1 starten**: 10.2.0.0
    - **Subnet 1 CIDR**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Configureren van een VPN-verbinding tussen de twee VNets

###<a name="create-local-networks"></a>Lokale netwerken maken

Wanneer u een VNet in de configuratie van VNet maakt, moet u elke VNet om elkaar te identificeren als een lokaal netwerksite configureren. In deze sectie, zult u elke VNet configureren als een lokaal netwerk. Lokale netwerken deelt de dezelfde IP-adresruimten met de bijbehorende VNet.

![Azure VPN-configuratie van site naar site - azure lokale netwerken configureren][img-vnet-lnet-diagram]


**Voor het maken van een lokaal netwerk genaamd Contoso-LNet-EU overeenkomen met de adresruimte voor Contoso-VNet-EU-netwerk**

1. Klik op **Nieuw**, **NETWERKSERVICES**, **VIRTUAL NETWORK**, **Lokaal netwerk toevoegen**vanuit de klassieke Portal Azure.
3. Voer in:

    - **Naam**: Contoso-LNet-EU
    - **VPN-IP-adres**: 192.168.0.1 (dit adres zal later worden gewijzigd)

        Normaal gesproken zou u het externe IP-adres gebruiken voor een VPN-apparaat. Voor VNet op VNet-configuraties gebruikt u het IP-adres van de VPN-gateway. Gezien het feit dat u hebt gemaakt het VPN-gateways voor de twee VNets nog, een IP-adres van arbitary en en kom terug te herstellen.
4.  Voer in:

    - **Adres ruimte STARTING IP:** 10.1.0.0
    - **Adres ruimte CIDR:** /16
    
    Dit moet komen exact overeen met het bereik dat u eerder hebt opgegeven voor Contoso VNet EU.

**Voor het maken van een lokaal netwerk genaamd Contoso-LNet-US overeenkomen met de adresruimte voor Contoso VNet VS-netwerk**

- Herhaal de laatste procedure met de volgende parameters:

    - **Naam**: Contoso-LNet-US
    - **VPN-IP-adres**: 192.168.0.1 (dit adres zal later worden gewijzigd)
     
    - **Adres ruimte STARTING IP**: 10.2.0.0
    - **Adres ruimte CIDR**: / 16


###<a name="create-vpn-gateways"></a>VPN-gateways maken

Er zijn twee delen in deze configuratie. Eerst een VNet site naar site verbinding met een lokaal netwerk en maakt u een dynamische routering VPN. VNet tot VNet vereist Azure VPN-gateways met dynamische routering VPN-verbindingen. Azure statische routering VPN-verbindingen worden niet ondersteund.

**Configureer de verbinding van de site naar site Contoso VNet EU Contoso-LNet-US**

1.  Klik vanuit de Portal Azure klassieke **netwerken** in het linkerdeelvenster
2.  Klik op de **Contoso-VNet-EU**.
3.  Klik op het tabblad **CONFIGUE** .
4.  Controleer de **verbinding met het lokale netwerk**.
5.  Selecteer in het **Lokale netwerk**, **Contoso-LNet-US**.
6.  Klik op **gateway-subnet toevoegen** in de sectie virtueel netwerk adres spaties.
7.  Klik op **Opslaan**.
8.  Klik op **OK** om te bevestigen.


**Voor het maken van een VPN-gateway voor Contoso-VNet-EU**

1.  Klik op het tabblad **DASHBOARD** van de klassieke Portal Azure.
4.  Klik op **GATEWAY maken** aan de onderkant van de pagina en klik vervolgens op **Dynamische routering**.
5.  Klik op **Ja** om te bevestigen. U ziet de afbeelding van de gateway op de pagina gewijzigd in geel en zegt Gateway maken. Het duurt meestal ongeveer 15 minuten voor de gateway te maken.

    Wanneer de status van de gateway in verbinding maken verandert, zal het IP-adres voor de Gateway zichtbaar zijn in het Dashboard. Noteer het IP-adres dat overeenkomt met voor elke VNet, om ze van elkaar niet te bekommeren. Dit zijn de IP-adressen die worden gebruikt tijdens het bewerken van de IP-adressen van de tijdelijke aanduiding voor het VPN-apparaat in lokale netwerken.

6.  Maak een kopie van het **GATEWAY-IP-adres**. U kunt het IP-adres van de VPN-gateway configureren voor Contoso VNet EU in de volgende sectie.

**Voor het maken van een VPN-gateway voor Contoso-VNet-EU**

- Herhaal de laatste twee procedure de Contoso-VNet-US site naar site verbinding Contoso-LNet-EU en het maken van een VPN-gateway configureren voor Contoso-Vnet-US. Wanneer u klaar bent, hebt u het IP-adres van de VPN-gateway voor Contoso-VNet-US.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>IP-adressen voor lokale netwerken voor het VPN-apparaat instellen
In de laatste sectie vindt maken u een VPN-gateway voor elk van de VNets. U kunt de IP-adressen van de VPN-gateways hebt gekregen. Nu kunt u teruggaan naar het lokale netwerk VPN-apparaat IP-adressen configureren.

**Het IP-adres van de VPN-apparaat configureren voor Contoso-LNet-EU** 

1.  Klik op **netwerken** in het linkerdeelvenster van het klassieke Portal Azure.
2.  Klik op **Lokale netwerken** vanaf de bovenkant.
3.  **Contoso LNet EU**op en klik vervolgens op **bewerken** op de onderkant.
4.  **VPN-IP-adres**bijwerken.  Dit is het adres u via het tabblad DASHBOARD van Contoso VNET EU krijgt.
5.  Klik op de knop rechts.
6.  Klik op de knop controleren.

**Het IP-adres van de VPN-apparaat configureren voor Contoso-LNet-US** 

- Herhaal de laatste procedure het IP-adres van de VPN-apparaat configureren voor Contoso-LNet-US.

###<a name="set-vnet-gateway-keys"></a>VNet gateway sleutels instellen

De gateways van Vnet een gedeelde sleutel gebruiken voor het verifiëren van verbindingen tussen de virtuele netwerken. De sleutel kan niet worden geconfigureerd via de klassieke Azure-Portal. U moet de PowerShell of .NET SDK gebruiken.

**De toetsen instellen**

1. Open in uw workstation, **Windows PowerShell ISE** of de Windows PowerShell-console.
2. Update van de parameters in dit script volgen en uit te voeren:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>De VPN-verbinding controleren 

Zonder enige VMs geïmplementeerd op de VNets, kunt u het virtuele visuele netwerkdiagram de dashboardpagina VNet op de klassieke Portal Azure en controleer de verbindingsstatus:

![HDInsight HBase replicatie virtueel netwerk VPN-verbindingsstatus][img-vpn-status]
  



##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe een VPN-verbinding tussen twee Azure virtuele netwerken configureren. Betrekking hebben op de twee artikelen in de reeks:

- [DNS configureren tussen twee Azure virtuele netwerken][hdinsight-hbase-geo-replication-dns]
- [HBase geo-replicatie configureren][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
