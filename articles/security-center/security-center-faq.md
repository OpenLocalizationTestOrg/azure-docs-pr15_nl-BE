<properties
   pageTitle="Beveiligingscentrum Azure Veelgestelde vragen (FAQ) | Microsoft Azure"
   description="Deze Veelgestelde vragen antwoorden op vragen over Azure Security Center."
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
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Beveiligingscentrum Azure Veelgestelde vragen (FAQ)

Deze Veelgestelde vragen antwoorden op vragen over Azure Security Center, een service die u helpt bij het voorkomen, opsporen en reageren op bedreigingen met een beter inzicht in en controle over de beveiliging van uw resources Microsoft Azure.

## <a name="general-questions"></a>Algemene vragen

### <a name="what-is-azure-security-center"></a>Wat is Azure Security Center?
Azure Beveiligingscentrum helpt bij het voorkomen, opsporen en reageren op bedreigingen met een beter inzicht in en controle over de beveiliging van uw resources Azure. Het biedt geïntegreerde beveiliging controleren en beleid verschillende abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

### <a name="how-do-i-get-azure-security-center"></a>Hoe krijg ik Azure Security Center
Azure Beveiligingscentrum is ingeschakeld bij uw abonnement voor Microsoft Azure en via de [portal Azure](https://azure.microsoft.com/features/azure-portal/). ([Aanmelden bij de portal](https://portal.azure.com), selecteert u **Bladeren**, en Ga naar de **Security Center**).  

## <a name="billing"></a>Facturering

### <a name="how-does-billing-work-for-azure-security-center"></a>Hoe werkt facturering voor Azure Security Center?
Beveiligingscentrum is verkrijgbaar in twee lagen: gratis en standaard.

De vrij laag kunt u instellen van beveiligingsbeleid en beveiligingswaarschuwingen, incidenten en aanbevelingen die u bij het proces begeleiden van het benodigde besturingselementen configureren. U kunt ook de beveiligingsstatus van uw resources Azure en partneroplossingen geïntegreerd met uw abonnement Azure controleren met de vrij laag.

De standaard biedt de gratis tier functies plus geavanceerde detectie: dreiging intelligence, doorgevoerd analyse crash-analyse en detectie van de afwijking. Er is een gratis proefversie van 90 dagen van de laag standaard beschikbaar. Als u wilt upgraden, prijzen laag te selecteren in het [beveiligingsbeleid](security-center-policies.md#setting-security-policies-for-subscriptions). Zie [prijzen Security Center](security-center-pricing.md) voor meer informatie.

## <a name="data-collection"></a>Het verzamelen van gegevens

Security Center verzamelt gegevens van uw virtuele machines beoordeling van de toestand van hun beveiliging, aanbevelingen voor de beveiliging te bieden en een waarschuwing voor bedreigingen. Wanneer u eerst Security Center, is het verzamelen van gegevens is ingeschakeld op alle virtuele machines op uw abonnement. Het verzamelen van gegevens wordt aanbevolen, maar u kunt opt-out door het [verzamelen van gegevens uit te schakelen](#how-do-i-disable-data-collection) in het beleid van de Security Center.

### <a name="how-do-i-disable-data-collection"></a>Hoe schakel ik het verzamelen van gegevens

U kunt **het verzamelen van gegevens** voor een abonnement in het beveiligingsbeleid op elk gewenst moment uitschakelen. ([Aanmelden bij de Azure portal](https://portal.azure.com), selecteert u **Bladeren** **Security Center**en selecteer **beleid**selecteren.)  Wanneer u een abonnement hebt geselecteerd, opent u een nieuwe blade- en biedt u de optie voor het **verzamelen van gegevens** uit te schakelen. Selecteer de optie **verwijderen agenten** in het bovenste lint agents verwijderen uit bestaande virtuele machines.

> [AZURE.NOTE] Beveiligingsbeleid kunnen worden ingesteld op het niveau van de resourcegroep en Azure-abonnement, maar moet u een abonnement op het verzamelen van gegevens uit te schakelen.

### <a name="how-do-i-enable-data-collection"></a>Hoe schakel ik het verzamelen van gegevens
Kunt u het verzamelen van gegevens voor uw abonnement(en) Azure in het beveiligingsbeleid. Als u wilt dat het verzamelen van gegevens, [aanmelden bij de Azure portal](https://portal.azure.com), selecteer **Bladeren** **Security Center**en selecteer **beleid**selecteren. **Verzamelen van gegevens** **op** en de opslag rekeningen waar u gegevens wilt verzamelen om te configureren (Zie de vraag '[waar is mijn gegevens opgeslagen?](#where-is-my-data-stored)"). Wanneer **het verzamelen van gegevens** is ingeschakeld, automatisch security configuration en gebeurtenis worden gegevens verzameld van alle ondersteunde virtuele machines in het abonnement.

> [AZURE.NOTE] Beveiligingsbeleid kunnen worden ingesteld op het niveau van de resourcegroep en Azure abonnement maar configuratie van het verzamelen van gegevens vindt plaats met het soort abonnement.

### <a name="what-happens-when-data-collection-is-enabled"></a>Wat gebeurt er bij het verzamelen van gegevens is ingeschakeld?
Het verzamelen van gegevens is via de Azure Monitoring Agent en de Azure Security Monitoring-extensie ingeschakeld. Azure Security Monitoring-extensie zoekt naar verschillende relevante Beveiligingsconfiguratie en stuurt dit naar sporen [Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Bovendien maakt het besturingssysteem logboekvermeldingen.  De Azure Monitoring Agent logboekvermeldingen leest en ETW traceert en kopieert deze naar uw account opslag voor analyse.  Dit is de opslag-account die u hebt geconfigureerd in het beveiligingsbeleid. Zie voor meer informatie over de opslag-account, vraag '[waar is mijn gegevens opgeslagen?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Is de extensie Monitoring Agent of de beveiliging controleren van invloed op de prestaties van de server (s)?
De agent en de extensie een nominale hoeveelheid systeembronnen verbruikt en moeten weinig impact hebben op de prestaties.

### <a name="where-is-my-data-stored"></a>Waar worden mijn gegevens opgeslagen?
Voor elke regio waarin u de virtuele machines met hebt, kiest u de opslag account waarin gegevens verzameld van deze virtuele machines worden opgeslagen. Hierdoor kunt gemakkelijk gegevens op te slaan in hetzelfde geografische gebied voor privacy en gegevens soevereiniteit doeleinden. U kiest de rekening van de opslag voor een abonnement in het beveiligingsbeleid. ([Aanmelden bij de Azure portal](https://portal.azure.com), selecteert u **Bladeren** **Security Center**en selecteer **beleid**selecteren.) Wanneer u op een abonnement klikt, wordt er een nieuwe blade geopend. Selecteer **Kies opslag rekeningen** om een gebied te selecteren.

> [AZURE.NOTE] Beveiligingsbeleid kunnen worden ingesteld op het niveau van de resourcegroep en Azure abonnement maar aan het abonnement op een regio voor uw opslag account selecteren plaatsvindt.

Zie voor meer informatie over Azure opslag en rekeningen voor opslag, [Opslag documentatie](https://azure.microsoft.com/documentation/services/storage/) en [over Azure opslag rekeningen](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Azure Beveiligingscentrum gebruiken

### <a name="what-is-a-security-policy"></a>Wat is beveiligingsbeleid?
Een beveiligingsbeleid bepaalt welke besturingselementen die worden aanbevolen voor bronnen binnen de opgegeven abonnement of resourcegroep. In Beveiligingscentrum Azure definieert u het beleid voor uw abonnementen Azure en resourcegroepen volgens de vereisten voor de beveiliging van uw bedrijf en het type van toepassingen of de gevoeligheid van de gegevens in elk abonnement.

Bronnen die worden gebruikt voor ontwikkel- of testomgeving kunnen bijvoorbeeld verschillende beveiligingseisen dan die gebruikt voor productietoepassingen. Toepassingen met gereglementeerde gegevens zoals PII (persoonlijke informatie) kunnen ook een hoger niveau van beveiliging vereisen. Het beveiligingsbeleid in Azure Beveiligingscentrum ingeschakeld aantrekt aanbevelingen voor beveiliging en bewaking. Zie voor meer informatie over het beveiligingsbeleid, [Security health monitoring in Azure Security Center](security-center-monitoring.md).

> [AZURE.NOTE] Bij een conflict tussen een abonnement op beleid en resource group level, de resource groep niveau beleid gaat.

### <a name="who-can-modify-a-security-policy"></a>Wie kan een beveiligingsbeleid wijzigen?
Beveiligingsbeleid wordt geconfigureerd voor elk abonnement of resourcegroep. Als u wilt wijzigen van een beveiligingsbeleid op het niveau van de resourcegroep of abonnement, moet u een eigenaar of de medewerker van het abonnement.

Als u wilt weten hoe u een beveiligingsbeleid configureren, Zie de [instelling voor beveiligingsbeleid in Azure Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Wat is een aanbeveling voor beveiliging?
Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw resources Azure. Wanneer de potentiële beveiligingsproblemen zijn geïdentificeerd, aanbevelingen gemaakt. De aanbevelingen helpen u bij het proces van het configureren van het besturingselement nodig. Voorbeelden zijn:

- Inrichten van antimalware te kunnen identificeren en verwijderen van schadelijke software
- [Netwerk-beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) en regels voor het verkeer voor netwerkbesturing aan virtuele machines configureren
- Inrichten van een web application firewall te beschermen tegen aanvallen die zijn gericht op uw webtoepassingen
- Ontbrekende systeemupdates implementeren
- OS-configuraties die niet overeenkomen met de aanbevolen basislijnen adressering

Alleen de aanbevelingen die zijn ingeschakeld in het beveiligingsbeleid worden hier weergegeven.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Hoe kan ik de huidige beveiligingsstatus van mijn Azure resources zien?
Een tegel **gezondheid van bronnen** op het blad **Security Center** wordt de algehele houding van de beveiliging van uw omgeving onderverdeeld op basis van virtuele machines, toepassingen en andere bronnen. Elke resource heeft een indicator wordt weergegeven als een potentiële beveiligingsproblemen zijn geïdentificeerd. Te klikken op de tegel van de gezondheid van bronnen resources worden weergegeven en wordt aangegeven waar aandacht is vereist of problemen kunnen zich voordoen.

### <a name="what-triggers-a-security-alert"></a>Wat een beveiligingswaarschuwing geactiveerd?
Azure Security Center automatisch worden verzameld, geanalyseerd en worden de logboekgegevens van uw resources Azure, het netwerk en oplossingen van partners zoals antimalware en firewalls. Wanneer er bedreigingen zijn gedetecteerd, wordt een beveiligingswaarschuwing wordt gemaakt. Detectie van enkele voorbeelden:

- Gemanipuleerde virtuele machines communiceren met bekende schadelijke IP-adressen
- Geavanceerde malware gedetecteerd met behulp van Windows Foutrapportage
- Brute kracht aanvallen op virtuele machines
- Beveiligingswaarschuwingen van partner integrated security oplossingen zoals Anti-Malware of Web Application Firewalls

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Wat is het verschil tussen de bedreigingen gevonden en door Microsoft Security Response Center versus Azure Beveiligingscentrum een melding op?
Het Microsoft Security Response Center (MSRC) selecteert u beveiliging controle op de Azure-netwerk en de infrastructuur uitvoert en threat intelligence en misbruik klachten van derden ontvangt. Wanneer het MSRC rekenschap van klantgegevens is geopend door een onwettige of onbevoegde partij of dat van de klant gebruik van Azure niet aan de voorwaarden voor acceptabel gebruik voldoet, meldt een security incident manager aan de klant. Kennisgeving gebeurt meestal door een e-mail te sturen naar de contactpersonen beveiliging die is opgegeven in Azure Security Center of de eigenaar van de Azure abonnement als een contactpersoon beveiliging niet is opgegeven.

Security Center is een Azure service die voortdurend bewaakt Azure omgeving van de klant en analytics voor het automatisch detecteren van een breed scala aan potentieel schadelijke activiteit van toepassing is. Deze detectie worden opgehaald als de beveiligingsberichten in het dashboard Security Center.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Hoe worden de machtigingen in Beveiligingscentrum Azure verwerkt?
Azure Security Center ondersteunt toegang op basis van rollen. Zie voor meer informatie over op rollen gebaseerde toegangscontrole (RBAC) in Azure, [Azure Active Directory op rollen gebaseerde toegangscontrole](../active-directory/role-based-access-control-configure.md).

Wanneer een gebruiker opent Security Center, alleen de aanbevelingen en waarschuwingen die zijn gerelateerd aan de gebruiker toegang tot heeft bronnen zal worden gezien. Dit betekent dat gebruikers alleen zien items die betrekking hebben op bronnen waar de gebruiker de rol van de eigenaar, de inzender of de lezer is toegewezen aan het abonnement of de groep waartoe een resource behoort.

Als u wilt:

- **Een beveiligingsbeleid bewerken**, moet u een eigenaar of de medewerker van het abonnement.
- **Een aanbeveling toepassen**, moet u een eigenaar of de medewerker van het abonnement.
- **Inzicht in de beveiligingsstatus voor al uw abonnementen hebt**, u moet een eigenaar, medewerker of Reader (IT-beheer, Security Team) van elk abonnement.
- **Inzicht in de beveiligingsstatus van uw resources hebt**, moet u een resourcegroep eigenaar, medewerker of Reader (DevOps).

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Welke bronnen Azure worden gecontroleerd door Azure Security Center?
Azure Beveiligingscentrum controleert de volgende Azure bronnen:

- Virtuele machines (VMs) (met inbegrip van [Cloud](../cloud-services/cloud-services-choose-me.md))
- Azure virtuele netwerken
- Azure SQL service
- Oplossingen van partners die zijn geïntegreerd met uw abonnement Azure bijvoorbeeld een web application firewall VMs en [App-omgeving](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtuele Machines

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Welke soorten virtuele machines worden ondersteund?
Statuscontrole voor beveiliging en aanbevelingen zijn beschikbaar voor virtuele machines (VMs) gemaakt met zowel de [klassieke en implementatiemodellen Resource Manager](../azure-classic-rm.md).

Ondersteunde Windows VMs:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

Linux VMs ondersteund:

- Ubuntu versie 12.04, 14.04, 16.04
- Debian versie 7, 8
- 6 versies centOS. \*, 7.*
- Versies van Red Hat Enterprise Linux (RHEL) 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) versie 11. \*, 12.*
- Oracle Linux versie 6. \*, 7.*

VMs in een cloud-service wordt uitgevoerd, worden ook ondersteund. Alleen cloud services web en werknemer rollen uitgevoerd in productie sleuven worden bewaakt. Zie voor meer informatie over de service cloud, [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Waarom niet de antimalware-oplossing die draait op mijn Azure VM in Azure Security Center herkent?

Azure Beveiligingscentrum heeft alleen inzicht in antimalware via Azure-extensies geïnstalleerd. Beveiligingscentrum is bijvoorbeeld niet kunnen detecteren antimalware die vooraf geïnstalleerd op een afbeelding die u hebt opgegeven of is als u antimalware geïnstalleerd op de virtuele machines met behulp van uw eigen processen (zoals configuratie management systems).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Waarom krijg ik het bericht 'Scannen gegevens ontbreken' voor mijn VM?

Het kan enige tijd duren voordat (in het algemeen voor minder dan een uur) scan gegevens om in te vullen nadat het verzamelen van gegevens in Azure Beveiligingscentrum is ingeschakeld. Scans worden niet ingevuld voor VMs in een gestopte staat.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Waarom krijg ik het bericht "VM Agent ontbreekt?"

De VM-Agent moet worden geïnstalleerd op VMs teneinde het verzamelen van gegevens. De VM-Agent wordt standaard geïnstalleerd voor VMs die uit de markt Azure worden geïmplementeerd. Zie voor meer informatie over het installeren van de Agent VM op andere VMs, de blog boeken [VM Agent en uitbreidingen](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
