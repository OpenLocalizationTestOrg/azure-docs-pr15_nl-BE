<properties
   pageTitle="Bescherming van uw virtuele machines in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document adressen in Azure Security Center aanbevelingen waarmee u uw virtuele machines beveiligen en blijven met inachtneming van de regels voor het beveiligingsbeleid."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Bescherming van uw virtuele machines in Azure Security Center

Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw resources Azure. Beveiligingscentrum geeft potentiële beveiligingsproblemen, wordt de aanbevelingen die u bij het helpt configureren van de benodigde besturingselementen gemaakt.  Aanbevelingen hebben betrekking op Azure brontypen: virtuele machines (VMs), netwerken, SQL en toepassingen.

In dit artikel komen de aanbevelingen die betrekking hebben op VMs.  VM aanbevelingen center rond het verzamelen van gegevens toe te passen systeemupdates, antimalware, inrichten versleutelen uw VM-schijven en nog veel meer.  Met de onderstaande tabel als referentie kunt u de beschikbare VM aanbevelingen en elkaar doen als u het toepassen.

## <a name="available-vm-recommendations"></a>Beschikbare VM aanbevelingen

|Aanbeveling|Beschrijving|
|-----|-----|
|[Gegevens verzamelen voor abonnementen inschakelen](security-center-enable-data-collection.md)|Raadt aan om te schakelen op het verzamelen van gegevens in het beveiligingsbeleid voor elk van uw abonnementen en alle virtuele machines (VMs) in uw abonnementen.|
|[Een OS beveiligingslekken te verhelpen](security-center-remediate-os-vulnerabilities.md)|Aanbevolen dat u de OS-configuraties met de aanbevolen configuratieregels, uitlijnen bijvoorbeeld toestaan niet dat wachtwoorden worden opgeslagen.|
|[Systeemupdates toepassen](security-center-apply-system-updates.md)|Raadt ontbrekende systeem beveiligingsupdates en essentiële updates voor VMs.|
|[Opnieuw opstarten na systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates)|Raadt u aan een VM systeemupdates toe te passen om opnieuw te starten.|
|[Endpoint Protection installeren](security-center-install-endpoint-protection.md)|Adviseert het inrichten van VMs (alleen Windows VMs) antimalware programma's.|
|[Endpoint Protection gezondheid waarschuwingen oplossen](security-center-resolve-endpoint-protection-health-alerts.md)|Wordt aanbevolen dat u de endpoint protection fouten oplost.|
|[VM-Agent inschakelen](security-center-enable-vm-agent.md)|Hiermee kunt u zien welke VMs vereisen de VM-Agent. De VM-Agent moet worden geïnstalleerd op VMs wilt inrichten patch scannen basislijn scannen en antimalware-programma's. De VM-Agent wordt standaard geïnstalleerd voor VMs die uit de markt Azure worden geïmplementeerd. Het artikel [VM Agent en extensies – deel 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) bevat informatie over het installeren van de Agent VM.|
| [Schijf coderen](security-center-apply-disk-encryption.md) |Raadt de VM-schijven met Azure schijfversleuteling (Windows en Linux VMs) te coderen. Codering wordt aanbevolen voor volumes van het besturingssysteem en de gegevens op uw VM.|
| [Versie van het besturingssysteem bijwerken](security-center-update-os-version.md) | Raadt aan om de versie van het besturingssysteem (OS) te werken voor uw Cloud-Service met de meest recente versie beschikbaar voor de OS-familie.  Zie voor meer informatie over Cloud Services, [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md). |
| [Beveiligingslek met betrekking tot beoordeling is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) | Raadt u aan de oplossing van een beveiligingslek met betrekking tot beoordeling te installeren op uw VM. |
| [Problemen verhelpen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Kunt u systeem- en beveiligingsproblemen ontdekt door het beveiligingslek met betrekking tot beoordeling oplossing op uw VM is geïnstalleerd. |

## <a name="see-also"></a>Zie ook

Voor meer informatie over aanbevelingen voor andere brontypen Azure, raadpleegt u het volgende:

- [Bescherming van uw toepassingen in Azure Security Center](security-center-application-recommendations.md)
- [Beveiligen van uw netwerk in Azure Security Center](security-center-network-recommendations.md)
- [Bescherming van uw service Azure SQL in Azure Security Center](security-center-sql-service-recommendations.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
