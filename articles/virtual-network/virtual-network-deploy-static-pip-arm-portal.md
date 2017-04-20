<properties 
   pageTitle="Implementeren van een VM met een statische openbare IP-adres met behulp van de portal Azure in Resource Manager | Microsoft Azure"
   description="Informatie over het implementeren van VMs met een statische openbare IP-adres met behulp van de portal zure in Resource Manager"
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
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Een VM met een statische openbare IP-adres met behulp van de portal Azure implementeren

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Een VM maken met een openbare statische IP 

Volg de onderstaande stappen om een VM maken met een statisch, openbaar IP-adres in de portal Azure.

1. Vanuit een browser, Ga naar de [Azure portal](https://portal.azure.com) en meld u zonodig met uw Azure-account.
2. Klik op **Nieuw**op de bovenste linker hoek van de portal,>>**berekenen**>**Windows Server 2012 R2 Datacenter**.
3. Selecteer **Resource Manager** en klikt u op **maken**in de lijst **Selecteer een implementatiemodel** .
4. In de blade **Grondbeginselen van** de VM-informatie invoeren zoals hieronder wordt weergegeven en klik op **OK**.

    ![Azure portal - grondbeginselen](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. In het blad **Kies een grootte** op **A1-standaard** zoals hieronder wordt weergegeven en klik vervolgens op **selecteren**.

    ![Azure portal - Kies een grootte](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. In het blad **Instellingen** **openbaar IP-adres**Klik in het blad met **het openbare IP-adres maken** onder **toewijzing**, **statische** zoals hieronder wordt weergegeven. En klik vervolgens op **OK**.

    ![Azure portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. Klik op **OK**in het blad **Instellingen** .
8. Bekijk de **Samenvatting** blade, zoals hieronder wordt weergegeven en klik vervolgens op **OK**.

    ![Azure portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. U ziet de nieuwe tegel in het dashboard.

    ![Azure portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Zodra de VM is gemaakt, wordt de blade **Instellingen** zoals hieronder weergegeven

    ![Azure portal - openbare IP-adres maken](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)