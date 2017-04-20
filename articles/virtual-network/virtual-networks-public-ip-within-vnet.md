<properties 
   pageTitle="Het gebruik van openbare IP-adressen in een virtueel netwerk"
   description="Informatie over het configureren van een virtueel netwerk voor het gebruik van openbare IP-adressen"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Openbare IP-adresruimte in een virtueel netwerk (VNet)

Virtuele netwerken (VNets) kunnen zowel openbare als particuliere (RFC 1918 adresblokken) IP-adres spaties bevatten. Wanneer u een openbaar IP-adresbereik toevoegt, wordt deze beschouwd als onderdeel van de persoonlijke VNet IP-adresruimte die alleen kunnen worden bereikt binnen de VNet, VNets, onderling verbonden, en vanaf uw locatie op gebouwen.

De volgende afbeelding ziet u een VNet die openbare en particuliere IP-adres spaties bevat.

![Openbare IP-begrippen](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Hoe voeg ik een openbaar IP-adresbereik

U een openbaar IP-adresbereik op dezelfde manier u een particulier IP-adresbereik voegt; toevoegen met behulp van een bestand *netcfg* of door de configuratie in de [Azure portal](http://portal.azure.com)toe te voegen. U kunt een openbare IP-adresbereik toevoegen wanneer u uw VNet maken of u kunt teruggaan en daarna toe te voegen. In het volgende voorbeeld ziet u openbare en particuliere IP-adresruimten geconfigureerd in de dezelfde VNet.

![Openbare IP-adres in de Portal](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Zijn er beperkingen?

Er zijn enkele IP-adresbereiken die zijn niet toegestaan:

- 224.0.0.0/4 (Multicast)

- 255.255.255.255/32 (Broadcast)

- 127.0.0.0/8 (loopback)

- 169.254.0.0/16 (link-local)

- 168.63.129.16/32 (interne DNS)

## <a name="next-steps"></a>Volgende stappen

[Het beheren van DNS-servers die worden gebruikt door een VNet](virtual-networks-manage-dns-in-vnet.md)