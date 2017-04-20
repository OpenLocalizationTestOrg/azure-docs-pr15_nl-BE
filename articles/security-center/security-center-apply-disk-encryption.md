<properties
   pageTitle="Schijf coderen in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document wordt beschreven hoe u de aanbeveling Azure Beveiligingscentrum **toepassen schijf codering**te implementeren."
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

# <a name="apply-disk-encryption-in-azure-security-center"></a>Schijf coderen in Azure Security Center

Azure Beveiligingscentrum wordt aangeraden schijf coderen als u met Windows of Linux VM schijven die niet zijn gecodeerd met Azure schijfversleuteling. Schijf-codering kunt u de diskettes van Windows en Linux IaaS VM coderen.  Codering wordt aanbevolen voor volumes van het besturingssysteem en de gegevens op uw VM.


Schijfversleuteling borduurt voort op de industrie standaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) -functie van Windows en Linux OS en gegevenscodering te beschermen en uw gegevens te beschermen en uw organisatie-beveiliging en naleving van de verbintenissen nakomen om de functie voor [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) . Schijfversleuteling is geïntegreerd met [Azure sleutel kluis](https://azure.microsoft.com/documentation/services/key-vault/) om te bepalen en de schijf coderingssleutels en geheimen in je abonnement sleutel kluis, terwijl ervoor te zorgen dat alle gegevens in de VM-schijven worden gecodeerd in rust in [Azure opslag](https://azure.microsoft.com/documentation/services/storage/)beheren.

> [AZURE.NOTE] Azure schijf-codering wordt ondersteund op de volgende Windows server-besturingssystemen - Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2. Schijf-codering wordt ondersteund op de volgende Linux serverbesturingssystemen - Ubuntu, CentOS, SUSE en SUSE Linux Enterprise Server (SLES).

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in het blad **aanbevelingen** **toepassen schijfversleuteling**.
2. In het blad **schijf-codering toepassen** , zal u een overzicht van VMs schijfversleuteling wordt aanbevolen.
3. Volg de instructies voor het toepassen van versleuteling op deze VMs.

![][1]

Voor het coderen van Azure Virtual Machines die zijn geïdentificeerd door Beveiligingscentrum codering nodig, het is raadzaam de volgende stappen uit:

- Installeer en configureer Azure PowerShell. Hierdoor kunt u de PowerShell-opdrachten vereist voor het instellen van de speciale vereisten voor het coderen van Azure Virtual Machines.
- Ophalen en de Azure schijf versleuteling vereisten Azure PowerShell script uitvoert.
- Codeer uw virtuele machines.

[Versleutelen een Azure Virtual Machine](security-center-disk-encryption.md) u deze stappen doorlopen.  In dit onderwerp wordt ervan uitgegaan dat u Windows 10 gebruikt als de computer van waaruit u schijf codering configureren.

Er zijn vele benaderingen die kunnen worden gebruikt voor het instellen van de vereisten en codering voor Azure virtuele Machines configureren. Bent u al goed versed in Azure PowerShell of Azure CLI, vervolgens u mogelijk liever alternatieve benaderingen. Andere benaderingen Zie als u wilt weten over deze [schijf Azure-codering](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Zie ook

Dit document werd door het implementeren van de aanbeveling van de Security Center "Toepassen schijf encryption". Voor meer informatie over codering van de schijf, raadpleegt u het volgende:

- [Codering, en sleutelbeheer met Azure sleutel kluis](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 min 39 sec)--informatie over het gebruik van codering Schijfbeheer voor VMs IaaS en Azure sleutel kluis te beschermen en uw gegevens te beschermen.
- [Coderen van een Azure Virtual Machine](security-center-disk-encryption.md) (document)--informatie over het coderen van Azure Virtual Machines.
- [Azure schijfversleuteling](../security/azure-security-disk-encryption.md) (document)--informatie over het inschakelen van codering voor Windows en Linux VMs schijf.

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het beveiligingsbeleid configureren.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) --blog zoeken naar berichten over Azure beveiliging en naleving.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
