<properties
   pageTitle="Een virtueel netwerk en -Gateway configureren voor ExpressRoute in de klassieke portal | Microsoft Azure"
   description="Dit artikel helpt u bij het instellen van een virtueel netwerk voor ExpressRoute met behulp van het implementatiemodel klassiek en de klassieke portal."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Een virtueel netwerk voor ExpressRoute in de klassieke portal maken

De stappen in dit artikel helpt u bij het configureren van een virtueel netwerk en een virtueel netwerkgateway voor gebruik met ExpressRoute met behulp van het implementatiemodel klassiek en de klassieke portal.

Als u instructies voor het implementatiemodel Resource Manager zoeken wilt, kunt u de volgende artikelen: [maken met behulp van PowerShell virtueel netwerk](../virtual-network/virtual-networks-create-vnet-arm-ps.md) en [een VPN-Gateway naar een Resource Manager VNet voor ExpressRoute toevoegen](expressroute-howto-add-gateway-resource-manager.md).

**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Een klassieke VNet en gateway maken

De volgende stappen maakt een klassieke VNet en de gateway van een virtueel netwerk. Als u al een klassieke VNet, Zie de sectie [een bestaande klassieke VNet configureren](#config) in dit artikel.

1. Log in op de [Azure klassieke portal](http://manage.windowsazure.com).

2. Klik op **Nieuw**in de linkerbenedenhoek van het scherm. Klik op **Netwerkservices**en klik op **Virtual Network**in het navigatiedeelvenster. Klik op **Aangepaste maken** om te beginnen met de configuratiewizard.

3. Geef het volgende op de pagina **Virtuele netwerkdetails** :

    - **Naam** : naam van het virtuele netwerk. Gebruikt u deze naam virtueel netwerk wanneer u de exemplaren VMs en PaaS implementeert, zodat u niet wilt maken van de naam is te ingewikkeld.
    - **Locatie** : de locatie is afhankelijk van de fysieke locatie (regio), waar u uw bronnen (VMs) zich bevinden. Als u wilt dat de VMs die u op deze virtuele netwerk op fysiek bevindt zich in de Oost-Amerikaanse implementeert, selecteert u die locatie in. De regio die is gekoppeld aan het virtuele netwerk nadat u deze hebt gemaakt kunt u niet wijzigen.

4. Voer de volgende gegevens op de pagina **DNS-Servers en VPN-verbindingen** en klik vervolgens op de pijl volgende gemarkeerd in de rechterbenedenhoek. 

    - **DNS-Servers** - Voer de naam van de DNS-server en IP-adres of Selecteer een eerder geregistreerde DNS-server in het snelmenu. Deze instelling maakt een DNS-server. U kunt de DNS-servers opgeven die u wilt gebruiken voor het herleiden van namen voor deze virtuele netwerk.
    - **Verbinding van site naar Site** - Selecteer het selectievakje voor het **configureren van een VPN website**.
    - **ExpressRoute** : Schakel het selectievakje **Gebruik ExpressRoute**. Deze optie wordt alleen weergegeven als u **een VPN website configureren**hebt geselecteerd.
    - **LAN** - u moet een lokale netwerksite voor ExpressRoute hebben. In het geval van een verbinding ExpressRoute worden, de voor de locatie van het lokale netwerk opgegeven adresprefixen genegeerd. In plaats daarvan wordt het adresprefixen aan Microsoft via het ExpressRoute circuit gebruikt voor de routering.<BR>Als er al een lokaal netwerk voor uw ExpressRoute verbinding is gemaakt, kunt u deze in de vervolgkeuzelijst. Als dat niet het geval is, selecteert u **een nieuw lokaal netwerk opgeven**.

5. De **verbinding van Site naar Site** -pagina wordt weergegeven als u een nieuw lokaal netwerk opgeven in de vorige stap hebt geselecteerd. Voer de volgende gegevens configureren op uw lokale netwerk, en klik vervolgens op de pijl volgende gemarkeerd. 

    - **Name** - de naam die u wilt bellen (op locatie) van uw lokale netwerk site.
    - **Address space** - met inbegrip van begin-IP- en CIDR (aantal adressen). Zolang het niet met het adresbereik van het virtuele netwerk overlapt, kunt u een adresbereik opgeven. Normaal gesproken zou dit de adresbereiken van de voor de netwerken op locatie opgeven, maar voor ExpressRoute, worden deze instellingen niet gebruikt. Deze instelling is echter vereist voor het lokale netwerk te maken wanneer u met de klassieke portal.
    - **Toevoegen-adresruimte** - met deze instelling is niet relevant voor ExpressRoute.


6. Voer de volgende gegevens op de pagina **Virtuele adresruimte voor netwerk** en klik op het vinkje in de rechterbenedenhoek om uw netwerk te configureren. 

    - **Address space** - zoals begin-en IP-adres. Controleer of de adresruimte die u niet een van de adresruimte die u in uw lokale netwerk hebt overlappen.
    - **Subnet toevoegen** - met inbegrip van begin-IP- en een aantal adressen. Extra subnetten zijn niet vereist.
    - **Add gateway subnet** - Klik hier om het subnet van de gateway. Het gateway-subnet wordt alleen gebruikt voor het virtuele netwerkgateway en is vereist voor deze configuratie.<BR>De gateway subnet in CIDR (aantal adressen) voor ExpressRoute moet /28 zijn of groter (/ 27/26 enz.). Hierdoor voor dat subnet voldoende IP-adressen om de configuratie te werken. In de klassieke portal als u het selectievakje voor het gebruik van ExpressRoute, geeft de portal een subnet gateway met /28.  U kunt het aantal CIDR adressen in de klassieke portal niet aanpassen. Het subnet gateway worden weergegeven als **Gateway** in de klassieke portal, hoewel de echte naam van het subnet gateway die wordt gemaakt werkelijk **GatewaySubnet is**. U kunt deze naam weergeven met PowerShell of in de portal Azure.

7. Klik op het vinkje op de onderkant van de pagina en het virtuele netwerk begint te maken. Wanneer deze is voltooid, ziet u **dat gemaakt** weergegeven onder **Status** op de pagina **netwerken** in de klassieke portal.

## <a name="gw"></a>De gateway maken

1. Klik op de pagina **netwerken** het virtuele netwerk dat u zojuist hebt gemaakt, klik **Dashboard** boven aan de pagina.

2. Aan de onderkant van de pagina van het **Dashboard** op **Gateway maken** en selecteer **De dynamische routering**. Klik op **Ja** om te bevestigen dat u wilt maken van een gateway.

3. Zodra de gateway maken, ziet u een bericht verhuur die u weet dat de gateway is gestart. Het duurt maximaal 45 minuten voor de gateway te maken.

4. Uw netwerk koppelen aan een circuit. Volg de instructies in het artikel in [het koppelen van VNets aan ExpressRoute circuits](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Een bestaande klassieke VNet voor ExpressRoute configureren

Als u al een klassieke VNet, kunt u deze verbinding maken met ExpressRoute in de klassieke portal. De instellingen zijn dezelfde als hierboven, dus Lees deze secties vertrouwd wilt raken met de vereiste instellingen. Als u verbinding maken een ExpressRoute/Site naar Site naast elkaar bestaande wilt, Zie de stappen [in dit artikel](expressroute-howto-coexist-classic.md) . Ze zijn anders dan de stappen in dit artikel.
 
1. U moet het lokale netwerk te maken voordat u de rest van de VNet-instellingen bijwerken. Klik op **Nieuw** om een nieuw lokaal netwerk, die nodig is bij het configureren van ExpressRoute via de klassieke portal, **>** **Network Services** **>** **Virtueel netwerk** **>** **lokaal netwerk toevoegen**. Volg de stappen in de wizard om het lokale netwerk te maken.

2. Gebruik de pagina **configureren** voor het bijwerken van de rest van de instellingen voor de VNet en de VNet voor het lokale netwerk koppelen.

3. Na het configureren van de instellingen, gaat u naar de sectie [maken de gateway](#gw) van dit artikel voor het maken van de gateway.


## <a name="next-steps"></a>Volgende stappen

- Als u toevoegen van virtuele machines aan het virtuele netwerk wilt, Zie [leren paden van virtuele Machines](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Als u weten over ExpressRoute wilt, Zie het [Overzicht ExpressRoute](expressroute-introduction.md).


 
