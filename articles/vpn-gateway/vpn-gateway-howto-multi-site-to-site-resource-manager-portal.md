<properties
   pageTitle="Het gateway website met meerdere VPN-verbindingen toevoegen aan een virtueel netwerk voor de Resource Manager implementatiemodel met de Azure portal | Microsoft Azure"
   description="S2S-verbindingen met meerdere locaties toevoegen aan een VPN-gateway met een bestaande verbinding"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Een verbinding van Site naar Site toevoegen aan een VNet met een bestaande verbinding van VPN-gateway

> [AZURE.SELECTOR]
- [Resource Manager - Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klassiek - PowerShell](vpn-gateway-multi-site.md)

Dit artikel begeleidt u bij de Azure portal verbindingen van Site naar Site (S2S) toevoegen aan een VPN-gateway met een bestaande verbinding gebruiken. Dit type verbinding wordt vaak een configuratie 'meerdere vestigingen' genoemd. 

In dit artikel kunt u een verbinding S2S toevoegen aan een VNet die al een verbinding S2S, VNet-VNet-verbinding of punt-naar-Site verbinding heeft. Er zijn enkele beperkingen bij het toevoegen van verbindingen. Controleer de sectie [voordat u begint](#before) in dit artikel om te controleren voordat u de configuratie. 

Dit artikel is van toepassing op VNets gemaakt met behulp van het implementatiemodel Resource Manager met een RouteBased VPN-gateway. Deze stappen zijn niet van toepassing op ExpressRoute/Site naar Site naast elkaar bestaande verbinding configuraties. Zie [ExpressRoute/S2S naast elkaar bestaande verbindingen](../expressroute/expressroute-howto-coexist-resource-manager.md) voor informatie over het naast elkaar bestaande verbindingen.

### <a name="deployment-models-and-methods"></a>Van implementatiemodellen en methoden

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

We werken in deze tabel als nieuwe artikelen en extra's beschikbaar komen voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Voordat u begint

Controleer of de volgende items:

- U maakt een naast elkaar bestaande ExpressRoute/S2S-verbinding niet.
- U hebt een virtueel netwerk die is gemaakt met het implementatiemodel Resource Manager met een bestaande verbinding.
- De gateway virtueel netwerk voor uw VNet is RouteBased. Als u een PolicyBased VPN-gateway hebt, moet u de gateway virtueel netwerk verwijderen en een nieuwe VPN-gateway als RoutBased maken.
- Geen van de adresbereiken van de overlappen voor elk van de VNets die deze VNet verbinding met gemaakt wordt.
- Hebt u VPN-apparaat en iemand kan configureren. Zie [VPN-apparaten](vpn-gateway-about-vpn-devices.md). Als u niet bekend zijn met het configureren van uw VPN-apparaat of niet vertrouwd bent met het IP-adres bereiken zich in uw locatie op het netwerk configuratie, moet u samenwerken met iemand die de details voor u kan bieden.
- U hebt een extern gerichte openbare IP-adres voor het VPN-apparaat. Dit IP-adres kan niet zich bevinden achter een NAT bevindt.


## <a name="part1"></a>Deel 1 - een verbinding configureren

1. Vanuit een browser, Ga naar de [Azure portal](http://portal.azure.com) en meld u zonodig met uw Azure-account.
2. Klik op **alle resources** en zoek uw **gateway virtueel netwerk** uit de lijst met resources en klik erop.
3. Klik op het blad **netwerkgateway virtuele** **verbindingen**.

    ![Verbindingen blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Klik op **+ toevoegen**op het blad **verbindingen** .

    ![Verbindingsknop toevoegen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. Vul de volgende velden op de bladeserver **verbinding toevoegen** :
    - **Naam:** De naam die u wilt verlenen tot de site die u de verbinding met maakt.
    - **Type verbinding:** Selecteer de **Site naar site (IPsec)**.

    ![Blade verbinding toevoegen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Deel 2 - de gateway voor een lokaal netwerk toevoegen

1. Klik op **LAN gateway** ***LAN gateway kiezen***. Hiermee opent u het blad **Kies LAN gateway** .

    ![Kies LAN gateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Klik op **Nieuw** om het openen van het **lokale netwerk-gateway maken** blade.

    ![LAN gateway blade maken](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Vul de volgende velden op de bladeserver **maken LAN gateway** :
    - **Naam:** De naam die u wilt toekennen aan de resource LAN gateway.
    - **IP-adres:** Het openbare IP-adres van de VPN-apparaat op de site waarmee u verbinding wilt maken.
    - **Adresruimte:** De adresruimte die u wilt worden gerouteerd naar de nieuwe site van lokale netwerk.
4. Klik op **OK** op het **lokale netwerk-gateway maken** blade de wijzigingen op te slaan.

## <a name="part3"></a>Deel 3 - de gedeelde sleutel toevoegen en de verbinding maken

1. Voeg de gedeelde sleutel die u wilt gebruiken om de verbinding te maken op de bladeserver **verbinding toevoegen** . U kunt de gedeelde sleutel ophalen uit uw VPN-apparaat of een hier en configureer uw VPN-apparaat voor het gebruik van dezelfde gedeelde sleutel. Het belangrijkste is dat de toetsen precies hetzelfde zijn.

    ![Gedeelde sleutel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Klik op **OK** om de verbinding te maken op de onderkant van het blad.

## <a name="part4"></a>Deel 4 - de VPN-verbinding controleren

U kunt controleren of uw VPN-verbinding in de portal of via PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Volgende stappen

- Zodra de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie de virtuele machines [pad leren](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) voor meer informatie.
