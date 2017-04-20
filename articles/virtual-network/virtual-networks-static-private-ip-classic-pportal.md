<properties 
   pageTitle="Het instellen van een statisch particuliere IP-adres in de klassieke modus via de Portal Azure | Microsoft Azure"
   description="Wat zijn statische particuliere IP-adressen en hoe u ze kunt beheren in de klassieke modus via de portal Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Het instellen van een statisch IP-adres privé de Azure portal (klassiek)

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel. U kunt ook [een statische particuliere IP-adres in het implementatiemodel Resource Manager beheren](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

De volgende stappen uit het monster verwachten een eenvoudige omgeving al gemaakt. Als u de stappen zoals deze worden weergegeven in dit document worden uitgevoerd, eerst de testomgeving wordt beschreven in het [maken van een vnet](virtual-networks-create-vnet-classic-pportal.md)te bouwen.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statische particulier IP-adres opgeven bij het maken van een VM
Als u wilt maken in een VM met de naam *DNS01* in de *FrontEnd* -subnet van een VNet met de naam *TestVNet* met een statische particuliere IP-adres van *192.168.1.101*, volg de onderstaande stappen:

1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2. Klik op **Nieuw** > **berekenen** > **Windows Server 2012 R2 Datacenter**, aankondiging **klassieke**al ziet u de lijst **Selecteer een implementatiemodel** en klik vervolgens op **maken**.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. Voer de naam van de VM (*DNS01* in ons scenario), worden gemaakt in de blade **VM maken** de lokale administrator-account en het wachtwoord.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Klik op **Optionele configuratie** > **netwerk** > **Virtueel netwerk**, en klik vervolgens op **TestVNet**. Als **TestVNet** niet beschikbaar is, moet u de *Amerikaanse centrale* locatie en de aan het begin van dit artikel beschreven testomgeving hebt gemaakt.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. In de blade **netwerk** controleren of het geselecteerde subnet *FrontEnd*, en vervolgens klikt u op de **IP-adressen**, **IP-adrestoewijzing** Klik op **statische**en voer *192.168.1.101* voor **IP-adres** zoals hieronder wordt weergegeven.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. Klik op **OK** in het blad **IP-adressen** en vervolgens klikt u op **OK** in het **netwerk** blade en klik op **OK** in het **optionele config** blade.
7. Klik op **maken**in de blade **VM maken** . U ziet de tegel weergegeven in het dashboard.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische particuliere IP-adresgegevens ophalen voor een VM

De volgende stappen uit als u wilt weergeven in de statische persoonlijke IP-adresgegevens voor de VM gemaakt met de bovenstaande stappen, worden uitgevoerd.

1. Vanaf de portal Azure Azure, klikt u op **Alles zoeken** > **virtuele machines (klassiek)** > **DNS01** > **alle instellingen** > **IP-adressen** en de aankondiging van de toewijzing van IP-adres en het IP-adres zoals hieronder wordt weergegeven.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statische particulier IP-adres van een VM verwijderen
Volg de onderstaande stappen om het statische particuliere IP-adres verwijdert uit de bovenstaande VM.
    
1. Van de **IP-adressen** blade hierboven wordt weergegeven, klikt u op **dynamische** rechts van de **toewijzing van IP-adressen**, en vervolgens klikt u op **Opslaan**en klik op **Ja**.

    ![VM in Azure portal maken](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statische particulier IP-adres toevoegen aan een bestaande VM
Als een statische particulier IP-adres toevoegen aan de VM gemaakt met behulp van de bovenstaande stappen, volg de onderstaande stappen:

1. Van de **IP-adressen** blade hierboven wordt weergegeven, klikt u op **statische** rechts van de **toewijzing van IP-adressen**.
2. *192.168.1.101* voor **IP-adres**, typ en vervolgens klikt u op **Opslaan**en klik vervolgens op **Ja**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gereserveerde openbare IP-](virtual-networks-reserved-public-ip.md) adressen.
- Informatie over adressen [instantieniveau openbare IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Neem contact op met het [gereserveerde IP-REST API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).