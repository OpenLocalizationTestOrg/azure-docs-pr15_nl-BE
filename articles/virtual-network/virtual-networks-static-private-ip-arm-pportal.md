<properties 
   pageTitle="Een statische particuliere IP-adres instellen in ARM modus via de portal Azure | Microsoft Azure"
   description="Informatie over particuliere IP-adressen (Spanningsdips) en hoe u ze kunt beheren in de ARM-modus met behulp van de portal Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Het instellen van een statisch particuliere IP-adres in de portal voor Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op het implementatiemodel Resource Manager. U kunt ook een [statische particuliere IP-adres in het klassieke implementatiemodel beheren](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

De volgende stappen uit het monster verwachten een eenvoudige omgeving al gemaakt. Als u de stappen zoals deze worden weergegeven in dit document worden uitgevoerd, eerst de testomgeving wordt beschreven in het [maken van een vnet](virtual-networks-create-vnet-arm-pportal.md)te bouwen.

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Een VM maken voor het testen van statische particuliere IP-adressen

U instellen niet een statische particulier IP-adres tijdens het maken van een VM in de Resource Manager distributie modus via de portal Azure. U moet eerst de VM maken, tehn het particuliere IP-statisch ingesteld.

Als u wilt maken in een VM met de naam *DNS01* in de *FrontEnd* -subnet van een VNet met de naam *TestVNet*, de volgende stappen uit te voeren.

1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2. Klik op **Nieuw** > **berekenen** > **Windows Server 2012 R2 Datacenter**, mededeling van de lijst **Selecteer een implementatiemodel** al **Resource Manager**bevat en klik vervolgens op **maken**, zoals in de onderstaande afbeelding.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. Voer de naam van VM (*DNS01* in ons scenario), worden gemaakt in de blade **Grondbeginselen van** de lokale administrator-account en het wachtwoord, zoals weergegeven in de onderstaande afbeelding.

    ![Grondbeginselen van blade](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Controleren of de geselecteerde **locatie** *Central US*, en vervolgens klikt u op **Selecteer bestaande** onder de **resourcegroep**en vervolgens **resourcegroep** , klik nogmaals op en vervolgens klikt u op *TestRG*en klik op **OK**.

    ![Grondbeginselen van blade](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. Selecteer **Standaard A1**in het blad **een formaat kiezen** en klik op **selecteren**.

    ![Kies een grootte blade](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. Controleer of de volgende eigenschappen zijn ingesteld in de blade **Instellingen** worden ingesteld met de onderstaande waarden en klik vervolgens op **OK**.

    -**Opslag account**: *vnetstorage*
    - **Netwerk**: *TestVNet*
    - **Subnetmasker**: *FrontEnd*

    ![Kies een grootte blade](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. Klik op **OK**in het blad **Overzicht** . U ziet de tegel weergegeven in het dashboard.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische particuliere IP-adresgegevens ophalen voor een VM

De volgende stappen uit als u wilt weergeven in de statische persoonlijke IP-adresgegevens voor de VM gemaakt met de bovenstaande stappen, worden uitgevoerd.

1. Vanaf de portal Azure Azure, klikt u op **Alles zoeken** > **virtuele machines** > **DNS01** > **alle instellingen** > **Netwerkinterfaces** en klik vervolgens op de netwerkinterface alleen vermeld.

    ![VM tegels implementeren](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. Klik op **alle instellingen**in de **netwerkinterface** -blade > **IP-adressen** en u de waarden van de **toewijzing** en het **IP-adres** ziet.

    ![VM tegels implementeren](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statische particulier IP-adres toevoegen aan een bestaande VM
Als een statische particulier IP-adres toevoegen aan de VM gemaakt met behulp van de bovenstaande stappen, volg de onderstaande stappen:

1. Van de **IP-adressen** blade hierboven wordt weergegeven, klikt u op **statische** onder **toewijzing**.
2. Typ *192.168.1.101* voor **IP-adres**en klik vervolgens op **Opslaan**.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Als na het klikken op **Opslaan** u ziet dat de toewijzing nog steeds is ingesteld op **dynamische**, betekent dit dat het IP-adres dat u hebt opgegeven is al in gebruik. Probeer een ander IP-adres.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statische particulier IP-adres van een VM verwijderen
Voer de onderstaande stappen om het statische particuliere IP-adres verwijdert uit de bovenstaande VM.
    
1. Van de **IP-adressen** blade hierboven wordt weergegeven, klikt u op **dynamische** onder **toewijzing**en klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gereserveerde openbare IP-](virtual-networks-reserved-public-ip.md) adressen.
- Informatie over adressen [instantieniveau openbare IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Neem contact op met het [gereserveerde IP-REST API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).