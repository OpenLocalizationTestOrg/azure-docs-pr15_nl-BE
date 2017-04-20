<properties 
   pageTitle="Het maken van NSGs in de ARM-modus met behulp van de portal Azure | Microsoft Azure"
   description="Meer informatie over het maken en implementeren van NSGs in de ARM met de Azure portal"
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

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>NSGs met de Azure portal beheren

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op het implementatiemodel Resource Manager. U kunt ook de [NSGs in de klassieke implementatiemodel maken](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Het monster PowerShell onderstaande opdrachten een eenvoudige omgeving al gemaakt verwachten op basis van het bovenstaande scenario. Als u wilt dat de opdrachten uitvoeren zoals deze worden weergegeven in dit document, eerst de testomgeving bouwen door het implementeren van [deze sjabloon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klik op **Deploy om Azure**standaardparameterwaarden indien nodig vervangen en volg de instructies in de portal. De volgende stappen gebruiken **RG NSG** als de naam van de resourcegroep die op de sjabloon is geïmplementeerd.

## <a name="create-the-nsg-frontend-nsg"></a>De NSG NSG-FrontEnd maken

**NSG-FrontEnd** NSG maken zoals in het bovenstaande scenario, de volgende stappen uit te voeren.

1. Vanuit een browser, Ga naar http://portal.azure.com en meld u zonodig met uw Azure-account.
2. Klik op **Bladeren >** > **netwerk beveiligingsgroepen**.

    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. Klik op **toevoegen**in het blad **netwerk beveiligingsgroepen** .
  
    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. Een NSG *NSG-FrontEnd* in de resourcegroep *RG NSG* naam maken in de blade **groep netwerk maken** en klik op **maken**.

    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Regels maken in een bestaande NSG

Als regels wilt maken in een bestaande NSG vanaf de portal Azure, volg de onderstaande stappen.

2. Klik op **Bladeren >** > **netwerk beveiligingsgroepen**.

3. Klik in de lijst met NSGs, **NSG-FrontEnd** > **regels voor binnenkomende**

    ![Azure portal - NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. In de lijst met **regels voor binnenkomende**, klikt u op **toevoegen**.

    ![Azure portal - regel toevoegen](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. Maak een regel met de naam *web regel* prioriteit van *200* zodat toegang via *TCP* op poort *80* voor elke VM uit elke bron in de blade **binnenkomende regel voor toevoegen** en klik op **OK**. U ziet dat de meeste van deze instellingen standaardwaarden al.

    ![Azure portal - instellingen](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Na een paar seconden ziet u de nieuwe regel in het NSG.

    ![Azure portal - nieuwe regel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Herhaal stap 6 voor het maken van een binnenkomende regel met de naam *rdp-regel* met een prioriteit van *250* zodat toegang via *TCP* op poort *3389* voor elke VM uit elke bron.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>De NSG naar het subnet FrontEnd koppelen

1. Klik op **Bladeren >** > **resourcegroepen** > **RG NSG**.
2. Klik in de blade **RG NSG** op **...**  >  **TestVNet**.

    ![Azure portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Klik in de blade **Instellingen** op **subnetten** > **FrontEnd** > **netwerkgroep** > **NSG-FrontEnd**.

    ![Azure portal - Subnet-instellingen](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. Klik op **Opslaan**in het blad **FrontEnd** .

    ![Azure portal - Subnet-instellingen](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>De NSG NSG-back-end maken

**NSG BackEnd** NSG maken en deze te koppelen aan het subnet **BackEnd** , volg de onderstaande stappen.

1. Herhaal de stappen in [de NSG NSG-FrontEnd maken](#Create-the-NSG-FrontEnd-NSG) voor het maken van een met de naam *NSG BackEnd* NSG
2. Herhaal de stappen in de [regels in een bestaande NSG maken](#Create-rules-in-an-existing-NSG) om **binnenkomende** regels in de onderstaande tabel.

  	|Binnenkomende regel|Uitgaande regel|
  	|---|---|
  	|![Azure portal - binnenkomende regel](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure portal - uitgaande regel](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Herhaal de stappen in [de NSG naar het subnet FrontEnd koppelen](#Associate-the-NSG-to-the-FrontEnd-subnet) **NSG Backend** NSG koppelen aan het subnet **BackEnd** .

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren van bestaande NSGs](virtual-network-manage-nsg-arm-portal.md)
- [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor NSGs.