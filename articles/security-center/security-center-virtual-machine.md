<properties
   pageTitle="Azure Security Center en Azure virtuele Machines | Microsoft Azure"
   description="Dit document helpt u te begrijpen hoe Azure Security Center kunt beschermen Azure virtuele Machines."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure Security Center en Azure virtuele Machines

[Azure Beveiligingscentrum](https://azure.microsoft.com/services/security-center/) helpt bij het voorkomen, opsporen en reageren op bedreigingen. Het biedt geïntegreerde beveiliging beleid en het toezicht op verschillende de Azure abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

In dit artikel ziet u hoe Security Center kunt u veilig uw Azure virtuele Machines (VM).

## <a name="why-use-security-center"></a>Waarom Beveiligingscentrum gebruiken?

Security Center kunt u gegevens van virtuele machine in Azure beschermen door middel van inzicht in de beveiligingsinstellingen van uw virtuele machine. Wanneer Beveiligingscentrum uw VMs beschermt, zijn de volgende mogelijkheden beschikbaar:

- Instellingen voor besturingssysteem (OS) met de aanbevolen configuratieregels
- Beveiliging van het systeem en de essentiële updates die ontbreken
- Eindpunt beveiliging aanbevelingen
- Validatie van schijf-versleuteling
- Beveiligingslek met betrekking tot beoordeling en herstel
- Detectie van bedreiging

Naast het beveiligen van uw Azure VMs, biedt Security Center ook beveiliging bewaking en beheer voor Cloud Services, App, Services en virtuele netwerken. 

>[AZURE.NOTE] Zie [Inleiding tot Azure Security Center](security-center-intro.md) voor meer informatie over Azure Security Center.

## <a name="prerequisites"></a>Vereisten

Om aan de slag met Azure Security Center, moet u weten en houd rekening met het volgende:

- Hebt u een abonnement op Microsoft Azure. Zie [Security Center prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie over vrije en standaard lagen van Security-Center.
- Plan de vaststelling van de Security Center, Zie [Azure Beveiligingscentrum planning en operations guide](security-center-planning-and-operations-guide.md) voor meer informatie over overwegingen bij het plannen en activiteiten.
- Zie voor meer informatie over ondersteuning voor besturingssysteem [Azure Security Center-Veelgestelde vragen (FAQ)](security-center-faq.md). 

## <a name="set-security-policy"></a>Beveiligingsbeleid instellen

Het verzamelen van gegevens moet worden ingeschakeld, zodat die Azure Security Center verzamelen informatie die nodig is kunt om aanbevelingen en waarschuwingen die zijn gegenereerd op basis van het beveiligingsbeleid dat u wilt configureren. In de onderstaande afbeelding ziet u **het verzamelen van gegevens** is **ingeschakeld**.

Een beveiligingsbeleid bepaalt welke besturingselementen die worden aanbevolen voor bronnen binnen de opgegeven abonnement of resourcegroep. Voordat u beveiligingsbeleid inschakelt, moet u verzamelen van gegevens is ingeschakeld, Security Center verzamelt gegevens van uw virtuele machines beoordeling van de toestand van hun beveiliging, aanbevelingen voor de beveiliging te bieden en een waarschuwing voor bedreigingen. In Beveiligingscentrum definieert u het beleid voor uw Azure abonnementen of resourcegroepen volgens de beveiligingsbehoeften van uw bedrijf en het type van toepassingen of de gevoeligheid van de gegevens in elk abonnement. 

![Beveiligingsbeleid](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] Voor meer informatie over elke **preventiebeleid voor** beschikbaar, Zie artikel [beveiligingsbeleid instellen](security-center-policies.md) .

## <a name="manage-security-recommendations"></a>Aanbevelingen voor beveiliging beheren

Beveiligingscentrum analyseert de beveiligingsstatus van uw resources Azure. Wanneer Beveiligingscentrum potentiële beveiligingsproblemen geeft, maakt deze aanbevelingen. De aanbevelingen helpen u bij het proces van het configureren van de benodigde besturingselementen.

Na het instellen van beveiligingsbeleid analyseert Beveiligingscentrum de beveiligingsstatus van uw middelen ter identificatie van beveiligingslekken. De aanbevelingen worden weergegeven in een tabel waarbij elke regel staat voor één bepaalde aanbeveling. In de volgende tabel bevat enkele voorbeelden van aanbevelingen voor Azure VMs en wat elke doen als u het toepast. Wanneer u een aanbeveling selecteert, krijgt u informatie waarmee u het implementeren van de aanbeveling in Beveiligingscentrum.

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
| [Beveiligingslek met betrekking tot beoordeling is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) | Raadt u aan de oplossing van een beveiligingslek met betrekking tot beoordeling te installeren op uw VM. |
| [Problemen verhelpen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Kunt u systeem- en beveiligingsproblemen ontdekt door het beveiligingslek met betrekking tot beoordeling oplossing op uw VM is geïnstalleerd. |

>[AZURE.NOTE] Voor meer informatie over aanbevelingen, Zie [beheren beveiligingsaanbevelingen](security-center-recommendations.md) artikel.

## <a name="monitor-security-health"></a>Beveiliging-health monitor

Nadat u [beveiligingsbeleid](security-center-policies.md) voor een abonnement van resources hebt ingeschakeld, wordt Beveiligingscentrum de beveiliging van uw resources voor het identificeren van potentiële problemen te analyseren.  U kunt de beveiligingsstatus van uw resources, inclusief eventuele problemen met de **gezondheid van security Resource** blade weergeven. Als u **virtuele machines** in de **beveiliging van** de gezondheid van tegel klikt, wordt de blade **virtuele machines** met aanbevelingen voor uw VMs geopend. 

![Veiligheid, gezondheid](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Beheren en reageren op de beveiligingswaarschuwingen

Security Center automatisch verzamelt, analyseert en logboekgegevens van uw resources Azure, netwerk en verbonden partneroplossingen (zoals een firewall en endpoint protection solutions), geïntegreerd om reële dreigingen te detecteren en het beperken van vals-positief zijn. Beveiligingscentrum is door gebruik te maken van een gevarieerde aggregatie van [detectiemogelijkheden](security-center-detection-capabilities.md), naar prioriteit gerangschikte beveiligingswaarschuwingen zodat u snel het opgetreden probleem onderzoeken en aanbevelingen te doen voor het verhelpen van mogelijke aanvallen kunt genereren.

![Beveiligingswaarschuwingen](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Selecteer een beveiligingswaarschuwing voor meer informatie over de gebeurtenis of gebeurtenissen waarvoor de waarschuwing en wat, als alle stappen die u moet nemen om te herstellen van een aanval. Beveiligingswaarschuwingen worden gegroepeerd op [type](security-center-alerts-type.md) en de datum.


## <a name="see-also"></a>Zie ook

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
