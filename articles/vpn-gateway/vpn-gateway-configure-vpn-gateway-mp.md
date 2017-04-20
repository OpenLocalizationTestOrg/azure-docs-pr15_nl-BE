<properties 
   pageTitle="Een VPN-Gateway configureert in de klassieke Azure Portal | Microsoft Azure"
   description="Dit artikel begeleidt u door middel van het virtuele netwerk VPN-gateway configureren en een gateway VPN-routering type wijzigen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>Configureren van een VPN-gateway voor het implementatiemodel klassiek


Als u verbinding maken een beveiligde cross-ruimten tussen Azure en uw locatie op de gebouwen wilt, moet u een VPN-gateway configureren. In het klassieke implementatiemodel gateway zijn twee soorten VPN-routering: statisch of dynamisch. Het type dat u kiest, is afhankelijk van uw netwerk ontwerp-plan en het op ruimten VPN-apparaat dat u wilt gebruiken. 

Sommige connectiviteitsopties, zoals een punt-naar-site verbinding, vereist een dynamische routering gateway. Als u wilt dat uw gateway ter ondersteuning van zowel verbindingen met punt-naar-site (P2S) en een verbinding van site naar site (S2S) configureren, hebt u een dynamische routering-gateway configureert, hoewel de site naar site kan worden geconfigureerd met ofwel gateway VPN-routering type. 

Daarnaast moet ervoor zorgen dat het apparaat dat u wilt gebruiken voor uw verbinding ondersteunt de routering VPN-type dat u wilt maken. Zie [VPN-apparaten](vpn-gateway-about-vpn-devices.md).


**Over dit artikel** 

Dit artikel is geschreven voor de klassieke implementatiemodel met [klassieke portal](https://manage.windowsazure.com) (niet de Azure portal). 

**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Configuratie, overzicht

De volgende stappen kunnen u via de VPN-gateway configureert in de Azure klassieke portal. Deze stappen gelden voor gateways voor virtuele netwerken die zijn gemaakt met het klassieke implementatiemodel. Niet alle configuratie-instellingen voor gateways zijn momenteel beschikbaar in de portal Azure. Wanneer ze zijn, maken we een nieuwe reeks instructies die van toepassing zijn op de portal Azure.


1. [Een VPN-gateway maken voor uw VNet](#create-a-vpn-gateway)

1. [Verzamelen van informatie voor uw VPN-configuratie](#gather-information-for-your-vpn-device-configuration)

1. [Configureer uw VPN-apparaat](#configure-your-vpn-device)

1. [Controleer of uw lokale netwerkbereiken en VPN-gateway-IP-adres](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Voordat u begint

Voordat u uw gateway configureert, moet u eerst uw virtueel netwerk maken. Zie voor stapsgewijze instructies voor het maken van een virtueel netwerk voor verbinding met meerdere gebouwen, [een virtueel netwerk met een VPN-verbinding van site naar site configureren](vpn-gateway-site-to-site-create.md)of [een virtueel netwerk met een punt-naar-site VPN-verbinding configureren](vpn-gateway-point-to-site-create.md). Gebruik vervolgens de volgende stappen uit op de VPN-gateway configureren en verzamel de informatie die u nodig hebt om uw VPN-apparaat te configureren. 

Als u al een VPN-gateway en gewenste type VPN-routering wijzigen, Zie [de VPN-routering voor wijzigen uw gateway](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Een VPN-gateway maken

1. Controleer of de statuskolom van het virtuele netwerk is **gemaakt**in de [Azure klassieke portal](https://manage.windowsazure.com)op de pagina **netwerken** .

1. Klik in de kolom **naam** op de naam van het virtuele netwerk.

1. U ziet dat deze VNet geen gateway nog geconfigureerd op de **Dashboard** -pagina. Deze status wordt weergegeven terwijl u door de stappen voor het configureren van uw gateway.

![Gateway niet gemaakt.](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Klik vervolgens op **Gateway maken**onderaan de pagina. U kunt *Statische routering* of *Dynamische routering*selecteren. De VPN-routering type dat u selecteert, is afhankelijk van enkele factoren. Bijvoorbeeld, wat het VPN-apparaat ondersteunt en of u moet ondersteuning voor verbindingen met punt-naar-site. Controleer [Over VPN-apparaten voor verbinding met het virtuele netwerk](vpn-gateway-about-vpn-devices.md) om te controleren of de VPN-routering type die u nodig hebt. Wanneer u de gateway hebt gemaakt, kunt u niet wijzigen tussen gateway VPN-routering typen zonder te verwijderen en opnieuw maken van de gateway. Wanneer het systeem wordt u gevraagd te bevestigen dat u wilt dat de gateway is gemaakt, klikt u op **Ja**.

![Gateway VPN-routering type](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Wanneer uw gateway maakt, ziet de afbeelding van de gateway op de pagina wordt gewijzigd in geel en zegt *Gateway maken*. Het duurt maximaal 45 minuten voor de gateway te maken. Wacht totdat de gateway voltooid is voordat u kunt verdergaan met andere configuratie-instellingen.

![Gateway maken](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Als de gateway in *verbinding te maken verandert*, kunt u de informatie die u nodig hebt voor het VPN-apparaat te verzamelen.

![Gateway verbinding maken](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Verzamelen van informatie voor uw VPN-configuratie

Nadat u de gateway hebt gemaakt, gegevens verzamelen voor de configuratie van VPN-apparaat. Deze informatie is te vinden op de pagina van het **Dashboard** voor het virtuele netwerk:

1. **IP-adres gateway-** Het IP-adres vindt u op de pagina van het **Dashboard** . U kunt niet kunnen zien tot wanneer uw gateway maken is voltooid.

1. **Shared key-** Klik op **Sleutel beheren** onder aan het scherm. Klik op het pictogram naast de sleutel naar het Klembord kopiëren en plakken en de sleutel opslaan. Deze knop werkt alleen als er één S2S VPN-tunnel. Als u meerdere S2S VPN-tunnels hebt, gebruik de *Get virtuele netwerk Gateway Shared Key* API of de PowerShell-cmdlet.

![Sleutel beheren](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Configureer uw VPN-apparaat

Na het voltooien van de voorgaande stappen, moet u of de netwerkbeheerder het VPN-apparaat configureren van de verbinding te maken. Zie [Over VPN-apparaten voor verbinding met het virtuele netwerk](vpn-gateway-about-vpn-devices.md) voor meer informatie over VPN-apparaten.

Nadat het VPN-apparaat is geconfigureerd, kunt u de bijgewerkte verbindingsgegevens bekijken op de Dashboard-pagina voor uw VNet.

U kunt ook een van de volgende opdrachten om de verbinding te testen uitvoeren:

|                      | Cisco ASA             | Cisco ISR/ASR         | Jeneverbes SSG/ISG | Jeneverbes SRX/J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Hoofdmodus controleren**  | crypto isakmp weergeven sa | crypto isakmp weergeven sa | ike-cookie ophalen  | weergeven beveiliging ike-beveiligingskoppeling   |
| **SA's in de snelle modus controleren** | show crypto ipsec sa  | show crypto ipsec sa  | sa ophalen          | ipsec-beveiligingskoppeling beveiliging weergeven |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Controleer of uw lokale netwerkbereiken en VPN-gateway-IP-adres

### <a name="verify-your-vpn-gateway-ip-address"></a>Controleer of uw IP-adres van de VPN-gateway

Voor de gateway verbinding, het IP-adres voor het VPN-apparaat moet correct zijn geconfigureerd voor het lokale netwerk die u hebt opgegeven voor de configuratie van de verschillende ruimten. Dit is meestal geconfigureerd tijdens de configuratie van site naar site. Als u dit lokale netwerk eerder met een ander apparaat gebruikt of als het IP-adres is gewijzigd voor dit lokale netwerk, echter de instellingen voor het opgeven van het juiste IP-adres Gateway bewerken.

1. Om te controleren of het IP-adres van de gateway, klikt u op **netwerken** in het linkerdeelvenster van de portal en selecteer **Lokale netwerken** aan de bovenkant van de pagina. Hier ziet u de VPN-Gateway-adres voor elk lokaal netwerk die u hebt gemaakt. Als u wilt bewerken in het IP-adres, het VNet en klik op **bewerken** onder aan de pagina.

1. Het IP-adres te bewerken op de pagina **Geef de gegevens van uw lokale netwerk** en klik vervolgens op de pijl volgende onder aan de pagina.

1. Klik op het vinkje in de rechterbenedenhoek in uw instellingen op te slaan op de pagina **Geef de adresruimte** .

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Controleer of de adresbereiken weer voor de lokale netwerken

Voor de juiste verkeer via de gateway naar uw locatie op de gebouwen, moet u controleren of dat elk bereik van IP-adres is opgegeven. Elk bereik moet worden vermeld in de configuratie van de **Lokale netwerken** van Azure. Dit is afhankelijk van de configuratie van uw locatie op de gebouwen, een redelijk grote taak. Verkeer dat afhankelijk is van een IP-adres dat is opgenomen in de weergegeven bereiken worden via het virtuele netwerk VPN-gateway verzonden. De bereiken die u opgeeft, hoeft particuliere adresbereiken, maar is het verstandig om te controleren dat de configuratie op locatie het binnenkomende verkeer kan ontvangen.

Toevoegen of bewerken van het bereik voor een lokaal netwerk, gebruikt u de volgende stappen uit.

1. Als u wilt bewerken in de IP-adresbereiken voor een lokaal netwerk, klikt u op **netwerken** in het linkerdeelvenster van de portal en selecteer **Lokale netwerken** aan de bovenkant van de pagina. In de portal is de eenvoudigste manier om het bekijken van de bereiken die u hebt opgegeven op de pagina **bewerken** . Bereiken, selecteer het VNet en klik op **bewerken** onder aan de pagina.

1. Klik op de pagina **Geef de gegevens van uw lokale netwerk** niet wijzigen. Klik op de pijl volgende onder aan de pagina.

1. Controleer uw wijzigingen in het netwerk adres ruimte op de pagina **Geef de adresruimte** . Vervolgens klikt u op het vinkje om de configuratie op te slaan.

## <a name="how-to-view-gateway-traffic"></a>Het verkeer van de gateway weergeven

U kunt gateway- en gateway-verkeer vanaf de pagina virtuele netwerken **Dashboard** weergeven.

Op de pagina van het **Dashboard** kunt u het volgende weergeven:

- De hoeveelheid gegevens die stroomt via de gateway, zowel gegevens in en uit.

- De namen van de DNS-servers die zijn opgegeven voor het virtuele netwerk.

- De verbinding tussen de gateway en de VPN-apparaat.

- De gedeelde sleutel die wordt gebruikt voor het configureren van uw gateway verbinding met het VPN-apparaat.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Het type VPN-routering voor uw gateway wijzigen

Omdat sommige configuraties verbindingen alleen beschikbaar voor bepaalde soorten gateway bewerkingsplannen zijn, kan het gebeuren dat u moet de gateway VPN-routering type van een bestaande VPN-gateway wijzigen. U wilt bijvoorbeeld de verbinding punt-naar-site toevoegen aan een al bestaande verbinding van site naar site met een statische gateway. Punt-tot-site-verbindingen vereist een dynamische gateway. Dit betekent dat een P2S verbinding configureren, hebt u uw gateway VPN-routering type wijzigen van statische naar een dynamische.

Als u een gateway VPN-routering type wijzigen, u de bestaande gateway te verwijderen, en maak vervolgens een nieuwe gateway met het nieuwe bewerkingsplan type. U hoeft niet te verwijderen van de hele virtuele netwerk als de gateway routing type wilt wijzigen.

Voordat u uw gateway VPN-routering type wijzigt, vergeet te controleren of uw VPN-apparaat ondersteunt het routetype die u wilt gebruiken. Voorbeelden van nieuwe routing configuratie downloaden en vereisten voor VPN-apparaten controleren, [Over VPN-apparaten voor verbinding met het virtuele netwerk](vpn-gateway-about-vpn-devices.md)ziet.

>[AZURE.IMPORTANT] Als u een virtueel netwerk VPN-gateway verwijdert, wordt het toegewezen aan de gateway VIP uitgebracht. Wanneer u de gateway opnieuw te maken, is een nieuwe VIP toegewezen.

1. **Verwijder de bestaande VPN-gateway.**

    **De dashboardpagina voor het virtuele netwerk,** gaat u naar de onderkant van de pagina en klik op **Gateway verwijderen**. Wacht tot de melding dat de gateway is verwijderd. Nadat u de melding op het scherm dat de gateway is verwijderd hebt ontvangen, kunt u een nieuwe gateway.

1. **Maak een nieuwe VPN-gateway.**

    Gebruik de procedure aan de bovenkant van de pagina voor het maken van een nieuwe gateway: [een VPN-gateway maken](#create-a-vpn-gateway).


## <a name="next-steps"></a>Volgende stappen

U kunt virtuele machines toevoegen aan het virtuele netwerk. Zie [een aangepaste virtuele machine maken](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Zie [een punt-naar-site VPN-verbinding configureren](vpn-gateway-point-to-site-create.md)voor het configureren van een point-to-site VPN-verbinding.

 
