<properties
   pageTitle="Installeer Endpoint Protection in Azure Security Center | Microsoft Azure"
   description="Dit document wordt beschreven hoe u de aanbeveling Azure Beveiligingscentrum **Endpoint Protection installeren**implementeren."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Endpoint Protection in Azure Security Center installeren

Azure Beveiligingscentrum wordt aanbevolen een antimalware-programma naar uw Azure (VMs) voor virtuele machines te creëren als antimalware is niet ingeschakeld. Deze aanbeveling geldt alleen voor Windows VMs.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in het blad **aanbevelingen** **Endpoint Protection installeren**.
![Selecteer installeren Endpoint Protection][1]

2. De blade **Endpoint Protection installeren** wordt geopend met een lijst van VMs zonder antimalware ingeschakeld. Selecteer in de lijst de gewenste antimalware op installeren en klik op **installeren op VMs**VMs.
![Selecteer VMs antimalware installeren op][2]

3. De blade **Endpoint Protection selecteren** kunt u de antimalware-oplossing die u wilt gebruiken selecteren wordt geopend. In dit voorbeeld gaan we **Microsoft Antimalware**selecteren.
![Selecteer Endpoint Protection][3]

4. Als u meer informatie over de antimalware-oplossing wordt weergegeven. Selecteer **maken**.
![Antimalware-oplossing maken][4]

5. Voer de vereiste configuratie-instellingen op het blad **Extensie toevoegen** en klik vervolgens op **OK**. Zie voor meer informatie over de configuratie-instellingen, [standaard en aangepaste Antimalware-configuratie](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../azure-security-antimalware.md) is nu actief op de geselecteerde VMs.

## <a name="see-also"></a>Zie ook

In dit artikel hebt u geleerd hoe implementeert u de aanbeveling Security Center "Installeren Endpoint Protection". Voor meer informatie over het inschakelen van een antimalware-programma in Azure, raadpleegt u het volgende:

- [Microsoft Antimalware voor Cloud Services en virtuele Machines](../azure-security-antimalware.md) --informatie over het implementeren van Microsoft antimalware.

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het beveiligingsbeleid configureren.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) --blog zoeken naar berichten over Azure beveiliging en naleving.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
