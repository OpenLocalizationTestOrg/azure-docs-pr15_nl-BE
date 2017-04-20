<properties
   pageTitle="Agent VM in Azure Beveiligingscentrum inschakelen | Microsoft Azure"
   description="Dit document wordt beschreven hoe u voor de uitvoering van de aanbeveling Beveiligingscentrum Azure **VM-Agent inschakelen**."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>Agent VM in Azure Beveiligingscentrum inschakelen

De VM-Agent moet worden geïnstalleerd op de virtuele machines (VMs) om het [verzamelen van gegevens inschakelen](security-center-enable-data-collection.md).  Azure Security Center kunt u zien welke VMs vereisen de VM-Agent en wordt aanbevolen dat u de VM-Agent inschakelen op de VMs.

De VM-Agent wordt standaard geïnstalleerd voor VMs die uit de markt Azure worden geïmplementeerd. Het artikel [VM Agent en extensies – deel 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) bevat informatie over het installeren van de Agent VM.


> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie. Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in de **aanbevelingen blade** **VM-Agent inschakelen**.
![VM-Agent inschakelen][1]

2. Hiermee opent u het blad **VM Agent ontbreekt of niet meer reageert**. Deze blade geeft een overzicht van de VMs waarvoor de VM-Agent. Volg de aanwijzingen op het blad de VM-agent installeren.
![VM-Agent ontbreekt][2]

## <a name="see-also"></a>Zie ook

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md)--informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Azure Security Center aanbevelingen voor beveiliging te beheren](security-center-recommendations.md), informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Security health monitoring in Azure Security Center](security-center-monitoring.md)--informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md)--informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md)--zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/)--laatste Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
