<properties
   pageTitle="VNet Peering met de Azure portal maken | Microsoft Azure"
   description="Informatie over het maken van een virtueel netwerk met behulp van de portal Azure in Resource Manager."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Een virtueel netwerk peering met de Azure portal maken

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Als u wilt maken van een VNet peering op basis van het scenario hierboven met behulp van de portal Azure, volg de onderstaande stappen.

1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2. Om te bepalen VNET peering, moet u twee koppelingen, één voor elke richting, tussen twee VNets te maken. U kunt eerst VNET peering koppeling voor VNET1 voor VNET2 maken. Klik op de portal, **Bladeren** > **virtuele netwerken kiezen**

    ![VNet peering in Azure portal maken](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. Blade virtuele netwerken, kiest u VNET1, Peerings, klik op toevoegen

    ![Kies peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. Geeft de naam van een peering link LinkToVnet2, kiest het abonnement en de peer virtuele netwerk VNET2 in het blad Peering toevoegen, klikt u op OK.

    ![Koppeling naar VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Als deze VNET peering koppeling is gemaakt. U kunt de status van de koppeling als volgt zien:

    ![Verbindingsstatus](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Maak de VNET peering koppeling voor VNET2 voor VNET1. Blade virtuele netwerken, kiest u VNET2, Peerings, klik op toevoegen

    ![Peer met andere VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Geeft de naam van een peering link LinkToVnet1 in het blad Peering toevoegen, kiest u het abonnement en de peer virtueel netwerk, en klik op OK.

    ![Maken virtueel netwerk naast elkaar](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Als deze VNET peering koppeling is gemaakt. U kunt de status van de koppeling als volgt zien:

    ![Laatste status](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. De status controleren van LinkToVnet2 en nu verandert in verbonden ook.  

    ![Laatste koppeling staat 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] VNET peering wordt alleen ingesteld als beide koppelingen zijn verbonden.

Er zijn een aantal configureerbare eigenschappen voor elke koppeling:

|Optie|Beschrijving|Standaard|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Of de adresruimte van Peer-VNet om te worden opgenomen als onderdeel van de Tag Virtual_network|Ja|
|AllowForwardedTraffic|Verkeer dat afkomstig is uit een peered VNet niet is geaccepteerd of verwijderd|Nee|
|AllowGatewayTransit|Hiermee kunt u de peer VNet gebruik van de gateway VNet|Nee|
|UseRemoteGateways|Van de peer VNet gateway gebruiken. De peer VNet moet een gateway geconfigureerd en AllowGatewayTransit is geselecteerd. U kunt deze optie niet gebruiken als u een gateway geconfigureerd|Nee|

Elke koppeling in VNet peering is een set van bovenstaande eigenschappen. Vanuit de portal kunt u Klik op de koppeling VNet Peering en alle beschikbare opties te wijzigen, klik op Opslaan om het effect te wijzigen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2. In dit voorbeeld wordt gebruiken we twee abonnementen A en B, en twee gebruikers GebruikerA en gebruiker b met bevoegdheden in de abonnementen respectievelijk
3. Kies op de portal, klikt u op Bladeren, virtuele netwerken. Klik op de VNET en klik op toevoegen.

    ![Scenario 2 bladeren](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Klik op het blad toevoegen toegang, selecteer een rol Inzender netwerk kiezen, klikt u op gebruikers toevoegen, typ de aanmeldingsnaam van gebruiker b naam en klikt u op OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Dit is niet vereist, peering kan worden ingesteld zelfs als gebruikers peering aanvragen voor hun respectieve Vnets afzonderlijk verhogen als de aanvragen voldoen aan. Bevoegde gebruiker van de andere VNet toe te voegen als gebruikers in de lokale VNet gemakkelijker in de portal wilt instellen.

5. Vervolgens aanmelden bij de Azure portal met verleent die de gebruiker bevoegdheid voor SubscriptionB is. Volg bovenstaande stappen voor het toevoegen van gebruiker a als Inzender netwerk.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] U kunt zich afmelden en aanmelden beide sessies van gebruikers in de browser om dat de verificatie met succes is ingeschakeld.

6. Aanmelding op de portal als GebruikerA, navigeer naar de blade VNET3, Peering, klikt u op controleren ' Ik weet mijn ID van de resource "checkbox en type van de resource-ID voor VNET5 in onderstaande indeling.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Resource-ID](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Meld u aan als gebruiker b en volg bovenstaande stap peering koppeling maken van VNET5 naar VNet3 portal.

    ![Resource-ID 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Peering worden vastgesteld en een virtuele machine in VNet3 moeten kunnen communiceren met een virtuele machine in VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Als eerste stap, VNET peering koppelingen van HubVnet naar VNET1. Houd er rekening mee dat de optie doorgestuurd verkeer toestaan niet is ingeschakeld voor de koppeling.

    ![Eenvoudige Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Een volgende stap kunnen peering koppelingen van VNET1 naar HubVnet worden gemaakt. Houd er rekening mee dat toestaan doorgestuurd verkeer optie is geselecteerd.

    ![Eenvoudige Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Nadat peering is gemaakt, kunt u verwijzen naar dit [artikel](virtual-network-create-udr-arm-ps.md) en gebruiker gedefinieerd Route(UDR) VNet1 verkeer omgeleid via een virtueel toestel gebruik van de mogelijkheden ervan definiëren. Wanneer u de volgende-Hop-adres in de route opgeeft, kunt u deze instellen op het IP-adres van het virtuele apparaat in een peer-VNet HubVNet


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.

2. Om vast te stellen in dit scenario peering VNET, moet u slechts één koppeling van het virtuele netwerk in Azure resourcemanager naar de klassieke maken. Dat wil zeggen, van **VNET1** tot **VNET2**. Klik op de portal, **Bladeren** > **Virtuele netwerken** kiezen

3. Kies in de blade virtuele netwerken, **VNET1**. **Peerings**, klik op **toevoegen**.

4. In het blad toevoegen Peering naam op de koppeling. Hier wordt het **LinkToVNet2**genoemd. Selecteer **klassieke**onder Peer details.

5. Kies het abonnement en de peer virtueel netwerk **VNET2**. Klik vervolgens op OK.

    ![Vnet1 koppelen aan Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Zodra deze peering VNet-koppeling is gemaakt, de twee virtuele netwerken zijn peered en kunt u het volgende zien:

    ![Peering verbinding controleren](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>Peering VNet verwijderen

1.  Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2.  Ga naar de virtueel netwerk blade, Peerings, klik op de koppeling die u wilt verwijderen, klikt u op verwijderen.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Zodra u een koppeling in VNET peering verwijdert, gaat de verbindingsstatus peer naar verbroken.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. In deze toestand niet kunt u de koppeling opnieuw maken totdat de status van de koppeling peer gestarte verandert. Wij raden u aan dat u de koppelingen verwijderen voordat u opnieuw de VNET peering maken.
