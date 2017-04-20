<properties
   pageTitle="Netwerk-beveiligingsgroepen in Azure Beveiligingscentrum inschakelen | Microsoft Azure"
   description="Dit document wordt beschreven hoe u voor de uitvoering van de aanbeveling Azure Beveiligingscentrum **Inschakelen netwerk-beveiligingsgroepen**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-network-security-groups-in-azure-security-center"></a>Netwerk-beveiligingsgroepen in Azure Beveiligingscentrum inschakelen

Azure Beveiligingscentrum wordt aanbevolen een beveiligingsgroep netwerk (NSG) in te schakelen als een is niet ingeschakeld. NSGs bevat een lijst met lijst ACL (Access Control)-regels die toestaan of weigeren netwerkverkeer naar de VM-exemplaren in een virtueel netwerk. NSGs kunnen worden gekoppeld aan de subnetten of afzonderlijke exemplaren van de VM op dat subnet. Wanneer een NSG gekoppeld aan een subnet is, gelden de regels van de ACL voor de VM exemplaren in dat subnet. Bovendien een afzonderlijke VM-verkeer kan worden beperkt door het koppelen van een NSG rechtstreeks naar die VM verder. Zie voor meer informatie [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)

Als er geen NSGs ingeschakeld, Security Center twee aanbevelingen aan u worden aangeboden: netwerk-beveiligingsgroepen inschakelen op subnetten en netwerk-beveiligingsgroepen inschakelen op de virtuele machines. U kiest welke niveau, een subnet of een VM NSGs toepassen.


> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in het blad **aanbevelingen** **Beveiligingsgroepen netwerk inschakelen** op subnetten of virtuele machines.
![Beveiligingsgroepen voor netwerk inschakelen][1]

2. Hiermee opent u de blade **Ontbrekend netwerk beveiligingsgroepen configureren** voor subnetten of voor virtuele machines, afhankelijk van de aanbeveling die u hebt geselecteerd. Selecteer een subnet of een virtuele machine voor het configureren van een NSG op.

  ![NSG voor subnet configureren][2]

  ![NSG configureren voor VM][3]
3. In de **groep kies netwerk** blade selecteert een bestaande NSG of voor het maken van een nieuwe NSG.

  ![Kies netwerk-beveiligingsgroep][4]

Als u een nieuwe NSG maakt, de stappen in [het beheren van NSGs met behulp van de portal Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) een NSG maken en instellen van regels.

## <a name="see-also"></a>Zie ook

Dit artikel werd u het implementeren van de aanbeveling van de Security Center 'Network Security groepen inschakelen' voor subnetten of virtuele machines. Voor meer informatie over het inschakelen van NSGs, Zie de volgende onderwerpen:

- [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [NSGs met de Azure portal beheren](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --laatste Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
