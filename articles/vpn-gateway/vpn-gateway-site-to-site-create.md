<properties
   pageTitle="Een virtueel netwerk maken met een site naar site VPN-Gateway verbinding met de Azure klassieke portal | Microsoft Azure"
   description="Een VNet maken met een S2S VPN-Gateway verbinding voor meerdere gebouwen en hybride-configuraties met behulp van het implementatiemodel klassiek."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Een VNet maken met Site-naar-Site verbinding maken via de klassieke Azure portal

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Classic - klassieke Portal](vpn-gateway-site-to-site-create.md)

Dit artikel helpt u bij het maken van een virtueel netwerk en een site naar site VPN-gateway verbinding met uw netwerk op ruimten is met behulp van het implementatiemodel klassiek en de klassieke portal. Verbindingen van site naar Site kunnen worden gebruikt voor cross-lokalen en hybride configuraties.

![Diagram van site naar Site] (./media/vpn-gateway-site-to-site-create/site2site.png "site naar site")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Van implementatiemodellen en methoden voor verbindingen van Site naar Site

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de van de momenteel beschikbare implementatiemodellen en methoden voor website configuraties. Wanneer een artikel met de configuratiestappen beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties 

Als u VNets met elkaar verbinden wilt, Zie [configureren voor het implementatiemodel klassiek VNet-VNet - verbinding](virtual-networks-configure-vnet-to-vnet-connection.md). Zie [een S2S verbinding toevoegen met een VNet met een bestaande VPN-gateway verbinding](vpn-gateway-multi-site.md)een verbinding van Site naar Site toevoegen aan een VNet die al verbonden is.
 
## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u de configuratie.

- Een VPN-apparaat en iemand kan configureren. Zie [VPN-apparaten](vpn-gateway-about-vpn-devices.md). Als u niet bekend zijn met het configureren van uw VPN-apparaat of niet vertrouwd bent met het IP-adres bereiken zich in uw locatie op het netwerk configuratie, moet u samenwerken met iemand die de details voor u kan bieden.

- Een extern gerichte openbare IP-adres voor het VPN-apparaat. Dit IP-adres kan niet zich bevinden achter een NAT bevindt.

- Een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).


## <a name="CreateVNet"></a>Het virtuele netwerk maken

1. Log in op de [Azure klassieke portal](https://manage.windowsazure.com/).

2. Klik op **Nieuw**in de linkerbenedenhoek van het scherm. Klik op **Netwerkservices**en klik op **Virtual Network**in het navigatiedeelvenster. Klik op **Aangepaste maken** om te beginnen met de configuratiewizard.

3. Wilt maken van uw VNet, voert u de configuratie-instellingen op de volgende pagina's:

## <a name="Details"></a>De detailpagina virtueel netwerk

Voer de volgende gegevens:

- **Naam**: de naam van het virtuele netwerk. Bijvoorbeeld *EastUSVNet*. Gebruikt u deze naam virtueel netwerk wanneer u de exemplaren VMs en PaaS implementeert, zodat u niet wilt maken van de naam is te ingewikkeld.
- **Locatie**: de locatie is afhankelijk van de fysieke locatie (regio), waar u uw bronnen (VMs) zich bevinden. Als u wilt dat de VMs die u op deze virtuele netwerk op fysiek bevindt zich in de *Oost-Amerikaanse*implementeert, selecteert u die locatie in. De regio die is gekoppeld aan het virtuele netwerk nadat u deze hebt gemaakt kunt u niet wijzigen.

## <a name="DNS"></a>DNS-servers en VPN-verbindingen pagina

Voer de volgende informatie en klik vervolgens op de pijl volgende gemarkeerd in de rechterbenedenhoek.

- **DNS-Servers**: Voer de naam van de DNS-server en IP-adres of Selecteer een eerder geregistreerde DNS-server in het snelmenu. Deze instelling maakt een DNS-server. U kunt de DNS-servers opgeven die u wilt gebruiken voor het herleiden van namen voor deze virtuele netwerk.
- **Configureren van Site naar Site VPN**: Schakel het selectievakje in voor het **configureren van een VPN website**.
- **Lokaal netwerk**: een lokaal netwerk geeft de locatie van uw fysieke op vestigingen. Kunt u een lokaal netwerk dat u eerder hebt gemaakt, of kunt u een nieuw lokaal netwerk. Als u een lokale netwerk die u eerder hebt gemaakt, gaat u naar de configuratiepagina van **Lokale netwerken** en controleer of het VPN-apparaat IP-adres (openbare tegenoverliggende IPv4-adres) voor het VPN-apparaat correct is.

## <a name="Connectivity"></a>Verbinding van site naar site pagina

Als u een nieuw lokaal netwerk maakt, ziet u de pagina **Connectiviteit van Site naar Site** . Als u een lokale netwerk die u eerder hebt gemaakt, wordt deze pagina niet wordt weergegeven in de wizard en u kunt verplaatsen in de volgende sectie.

Voer de volgende informatie en klik vervolgens op de pijl volgende gemarkeerd.

-   **Naam**: de naam die u wilt bellen (op locatie) van uw lokale netwerk site.
-   **VPN-IP-adres**: de openbare tegenoverliggende IPv4-adres van uw op ruimten VPN-apparaat waarmee u verbinding maakt met Azure. Het VPN-apparaat kan niet zich bevinden achter een NAT bevindt.
-   **Address Space**: zijn het IP-adres en CIDR (aantal adressen) starten. U het adres range(s) die u wilt worden verzonden via de gateway virtueel netwerk naar de locatie van uw lokale op gebouwen. Als een IP-adres binnen de bereiken die u hier opgeeft valt, wordt deze via het van virtuele netwerkgateway gerouteerd.
-   **Toevoegen-adresruimte**: als u meerdere adresbereiken die u wilt worden verzonden via het van virtuele netwerkgateway, elke extra adresbereik opgeven. U kunt toevoegen of verwijderen van bereiken later op de pagina van het **Lokale netwerk** .

## <a name="Address"></a>Virtueel netwerk adres spaties pagina

Geef het bereik van adressen die u wilt gebruiken voor het virtuele netwerk. Dit zijn de dynamische IP-adressen (SPANNINGSDIPS) die wordt toegewezen aan de VMs en andere rol exemplaren die u met dit virtuele netwerk implementeert.

Het is vooral belangrijk voor een bereik selecteren dat niet overlapt met een van de bereiken die worden gebruikt voor het netwerk op gebouwen. U moet samenwerken met de netwerkbeheerder. De netwerkbeheerder kan moet kunnen fungeren van een bereik van IP-adressen van uw space op ruimten netwerk adres kunt gebruiken voor het virtuele netwerk.

Voer de volgende informatie en klik op het vinkje in de rechterbenedenhoek om uw netwerk te configureren.

- **Address Space**: zijn het IP-adres en een aantal adressen te starten. Controleer of de adresruimte die u niet een van de adresruimte die u in uw netwerk op ruimten hebt overlappen.
- **Subnet toevoegen**: opnemen vanaf IP-adres en aantal adressen. Extra subnetten zijn niet vereist, maar u kunt een apart subnet maken voor VMs met statische SPANNINGSDIPS. Of u kunt uw VMs hebben in een subnet dat losstaat van de andere exemplaren van de rol.
- **Gateway-subnet toevoegen**: Klik hierop om de gateway subnet toevoegen. Het gateway-subnet wordt alleen gebruikt voor het virtuele netwerkgateway en is vereist voor deze configuratie.

Klik op het vinkje op de onderkant van de pagina en het virtuele netwerk begint te maken. Wanneer deze is voltooid, ziet u **dat gemaakt** weergegeven onder **Status** op de pagina **netwerken** in de klassieke Azure-Portal. Nadat de VNet is gemaakt, kunt u vervolgens uw gateway virtueel netwerk.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>Configureer uw gateway virtueel netwerk

Het virtuele netwerkgateway wilt maken van een beveiligde verbinding van site naar site configureren. Zie [een virtueel netwerkgateway in de klassieke Azure portal configureren](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Volgende stappen

Zodra de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Raadpleeg de documentatie van de [virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) voor meer informatie.
