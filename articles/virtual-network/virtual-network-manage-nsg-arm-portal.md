<properties 
   pageTitle="Beheren met behulp van de voorbeeld-portal in Resource Manager NSGs | Microsoft Azure"
   description="Informatie over het beheren van de huidige NSGs met de voorbeeld-portal in Resource Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>Beheren met behulp van de voorvertoning portal NSGs

> [AZURE.SELECTOR]
- [Portal](virtual-network-manage-nsg-arm-portal.md)
- [PowerShell](virtual-network-manage-nsg-arm-ps.md)
- [Azure CLI](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Gegevens ophalen

U kunt uw bestaande NSGs weergeven, ophalen van regels voor een bestaande NSG en weten welke bronnen een NSG is gekoppeld aan.

### <a name="view-existing-nsgs"></a>Bestaande NSGs weergeven
Als u wilt weergeven van alle bestaande NSGs in een abonnement, volg de onderstaande stappen.

1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2. Klik op **Bladeren >** > **netwerk beveiligingsgroepen**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Controleer de lijst met NSGs in het **netwerk beveiligingsgroepen** blade.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Volg de onderstaande stappen uit om de lijst met NSGs weergeven in de resourcegroep **RG NSG** . 

1. Klik op **resourcegroepen >** > **RG NSG** > **...**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. In de lijst met resources weer te geven pictogram van de NSG, zoals in de onderstaande **bronnen** -blade items zoeken.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>Lijst van alle regels voor een NSG

Als u wilt weergeven op de regels van een met de naam **NSG FrontEnd**NSG, volg de onderstaande stappen. 

1. Klik vanuit het **netwerk beveiligingsgroepen** blade of de bovenstaande **bronnen** -blade, **NSG-FrontEnd**.
2. Klik op het tabblad **Instellingen** op **binnenkomende regels**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. De **regels voor binnenkomende** blade wordt weergegeven zoals hieronder wordt weergegeven.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Klik op **uitgaande regels** Zie regels voor uitgaande verbindingen op het tabblad **Instellingen** .

>[AZURE.NOTE] Als standaardregels wilt weergeven, klikt u op het pictogram **standaardregels** aan de bovenkant van het blad waarin de regels worden weergegeven.

### <a name="view-nsgs-associations"></a>NSGs koppelingen weergeven

Als u wilt bekijken de NSG **NSG-FrontEnd is** resources koppelen aan, de volgende stappen uit te voeren.

1. Klik vanuit het **netwerk beveiligingsgroepen** blade of de bovenstaande **bronnen** -blade, **NSG-FrontEnd**.
2. Klik op het tabblad **Instellingen** op **subnetten** als u wilt bekijken welke subnetten worden gekoppeld aan de NSG.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Klik op het tabblad **Instellingen** op **Netwerkinterfaces** om te bekijken wat NIC's zijn gekoppeld aan de NSG.

## <a name="manage-rules"></a>Regels beheren

U kunt regels toevoegen aan een bestaande NSG, de bestaande regels bewerken en verwijderen van regels.

### <a name="add-a-rule"></a>Een regel toevoegen

Een regel die **binnenkomend** verkeer op poort **443** waardoor vanaf een willekeurige computer op de **NSG-FrontEnd** NSG toe, de volgende stappen uit te voeren.

1. Klik vanuit het **netwerk beveiligingsgroepen** blade of de bovenstaande **bronnen** -blade, **NSG-FrontEnd**.
2. Klik op het tabblad **Instellingen** op **binnenkomende regels**.
3. Klik op **toevoegen**in het blad **binnenkomende regels** . Vul de waarden als volgt in de blade **binnenkomende regel voor toevoegen** en klik op **OK**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Na een paar seconden ziet u de nieuwe regel in het blad voor **binnenkomende regels** .

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Een regel wijzigen

Als u wilt wijzigen in de regel voor het binnenkomende verkeer vanaf het **Internet** alleen toestaan dat hierboven is gemaakt, de volgende stappen uit te voeren.

1. Klik vanuit het **netwerk beveiligingsgroepen** blade of de bovenstaande **bronnen** -blade, **NSG-FrontEnd**.
2. Klik op de bovenstaande regel op het tabblad **Instellingen** .
3. De eigenschap **Source** wijzigen zoals hieronder wordt weergegeven in het blad **staan https** en klik op **Opslaan**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Een regel verwijderen

Als u wilt verwijderen van de bovenstaande regel, de volgende stappen uit te voeren.

1. Klik vanuit het **netwerk beveiligingsgroepen** blade of de bovenstaande **bronnen** -blade, **NSG-FrontEnd**.
2. Klik op de bovenstaande regel op het tabblad **Instellingen** .
3. In het blad **staan https** op **verwijderen**en klik vervolgens op **Ja**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Koppelingen beheren

U kunt een NSG met subnetten en NIC's koppelen. U kunt ook een NSG van alle bronnen die met bijbehorende koppeling.

### <a name="associate-an-nsg-to-a-nic"></a>Een NSG naar een NIC koppelen

Als u wilt koppelen de **NSG-FrontEnd** NSG naar **TestNICWeb1** NIC, volg de onderstaande stappen.

1. Klik vanuit het **netwerk beveiligingsgroepen** blade of de bovenstaande **bronnen** -blade, **NSG-FrontEnd**.
2. Klik op het tabblad **Instellingen** op **Netwerkinterfaces** > **koppelen** > **TestNICWeb1**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Koppeling van een NSG van een NIC

Volg de onderstaande stappen om de koppeling **NSG FrontEnd** NSG uit **TestNICWeb1** NIC.

1. Vanuit de Azure portal op **resourcegroepen >** > **RG NSG** > **...**  >  **TestNICWeb1**.
2. Klik in het blad **TestNICWeb1** **wijzigen beveiliging...**  > **None**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] U kunt ook deze blade koppelen de NIC aan alle bestaande NSG.

### <a name="dissociate-an-nsg-from-a-subnet"></a>Koppeling van een NSG van een subnet

Volg de onderstaande stappen om de **NSG-FrontEnd** NSG koppeling van het subnet **FrontEnd** .

1. Vanuit de Azure portal op **resourcegroepen >** > **RG NSG** > **...**  >  **TestVNet**.
2. Klik in de blade **Instellingen** op **subnetten** > **FrontEnd** > **netwerkgroep** > **geen**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. Klik op **Opslaan**in het blad **FrontEnd** .

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Een NSG met een subnet koppelen

Als u wilt koppelen **NSG FrontEnd** NSG opnieuw naar het subnet **FronEnd** , volg de onderstaande stappen.

1. Vanuit de Azure portal op **resourcegroepen >** > **RG NSG** > **...**  >  **TestVNet**.
2. Klik in de blade **Instellingen** op **subnetten** > **FrontEnd** > **netwerkgroep** > **NSG-FrontEnd**.
3. Klik op **Opslaan**in het blad **FrontEnd** .

>[AZURE.NOTE] U kunt ook een NSG koppelen aan een subnet van thh NSG van **Instellingen** blade.

## <a name="delete-an-nsg"></a>Een NSG verwijderen

U kunt een NSG alleen verwijderen als deze niet aan een resource zijn gekoppeld. U verwijdert een NSG, volg de onderstaande stappen.

1. Vanuit de Azure portal op **resourcegroepen >** > **RG NSG** > **...**  >  **NSG-FrontEnd**.
2. Klik op **Netwerkinterfaces**in het blad **Instellingen** .
3. Als er NIC's die worden vermeld, klikt u op de NIC en voert u stap 2 in [Dissociate een NSG van een Netwerkkaart](#Dissociate-an-NSG-from-a-NIC).
4. Herhaal stap 3 voor elke NIC
5. Klik in de blade **Instellingen** **subnetten**.
6. Als u subnetten die worden vermeld, klikt u op het subnet en volg de stappen 2 en 3 in de [Dissociate een NSG van een subnet](#Dissociate-an-NSG-from-a-subnet).
7. Links naar de blade **NSG FrontEnd** schuift vervolgens klikt u op **verwijderen** > **Ja**.

[Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Volgende stappen

- [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor NSGs.
