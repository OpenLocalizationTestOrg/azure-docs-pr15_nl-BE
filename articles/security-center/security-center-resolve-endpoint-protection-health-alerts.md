<properties
   pageTitle="Endpoint protection gezondheid waarschuwingen in Azure Security Center oplossen | Microsoft Azure"
   description="Dit document wordt beschreven hoe u de aanbeveling Azure Security Center **lossen Endpoint Protection gezondheid waarschuwingen**te implementeren."
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

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Eindpunt beveiliging gezondheid waarschuwingen in Azure Security Center oplossen

Beveiligingscentrum Azure aanbevelen dat u gedetecteerde eindpunt bescherming volksgezondheid waarschuwingen oplossen.  Security Center kunt u zien welke virtuele machines (VMs) endpoint protection storingen en hoeveel.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie. Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in de **aanbevelingen blade** **oplossen Endpoint Protection gezondheid waarschuwingen**.
![Eindpunt beveiliging gezondheid waarschuwingen oplossen][1]

2. Hiermee opent u het blad **Endpoint Protection storing** VMs met fouten en het aantal fouten vindt voor elke VM. Selecteer een VM uit de lijst.
![Fout bij het eindpunt beveiliging][2]

3. Een **Lijst met fouten** blade wordt geopend voor de geselecteerde VM, met een lijst van fouten. Selecteer een storing in de lijst voor meer informatie. Hiermee opent u een bladeserver met informatie over de geselecteerde fout.
![Lijst van mislukte][3]
  ![fout, gebeurtenis][4]

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
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
