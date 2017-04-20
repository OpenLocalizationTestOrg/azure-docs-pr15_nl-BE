<properties
   pageTitle="Update-OS-versie in Beveiligingscentrum Azure | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe u voor de uitvoering van de aanbeveling Azure Security Center **versie van het besturingssysteem bijwerken**."
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

# <a name="update-os-version-in-azure-security-center"></a>In Beveiligingscentrum Azure versie van het besturingssysteem bijwerken

Voor virtuele machines (VMs) in de cloud-services aanbevelen Azure Beveiligingscentrum dat het besturingssysteem (OS) moet worden bijgewerkt als er een recentere versie beschikbaar is.  Alleen cloud services web en werknemer rollen uitgevoerd in productie sleuven worden bewaakt.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in het blad **aanbevelingen** **versie van het besturingssysteem bijwerken**.
![Versie van het besturingssysteem bijwerken][1]

2. **Update-OS-versie**van het blad wordt geopend. Volg de stappen in deze blade voor het bijwerken van de versie van het besturingssysteem.

## <a name="see-also"></a>Zie ook

In dit artikel hebt u geleerd hoe implementeert u de aanbeveling Security Center 'OS Update versie'. Voor meer informatie over cloud services en het bijwerken van de versie van het besturingssysteem voor een cloud-service, Zie:

- [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md)
- [Het bijwerken van een cloud-service](../cloud-services/cloud-services-update-azure-service.md)
- [Cloud-Services configureren](../cloud-services/cloud-services-how-to-configure-portal.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --laatste Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png
