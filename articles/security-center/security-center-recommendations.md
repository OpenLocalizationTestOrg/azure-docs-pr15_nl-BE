<properties
   pageTitle="Azure Security Center aanbevelingen voor beveiliging beheren | Microsoft Azure"
   description="Dit document helpt u bij hoe Azure Security Center aanbevelingen helpen u uw Azure bronnen beschermen en blijven met inachtneming van de regels voor het beveiligingsbeleid."
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

# <a name="managing-security-recommendations-in-azure-security-center"></a>Azure Security Center aanbevelingen voor beveiliging beheren

Dit document helpt u bij de aanbevelingen in Azure Beveiligingscentrum gebruiken om u te helpen beschermen uw Azure resources.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="what-are-security-recommendations"></a>Wat zijn de aanbevelingen voor de beveiliging?
Security Center analyseert regelmatig de beveiligingsstatus van uw resources Azure. Wanneer Beveiligingscentrum potentiële beveiligingsproblemen geeft, maakt deze aanbevelingen. De aanbevelingen helpen u bij het proces van het configureren van de benodigde besturingselementen.

## <a name="implementing-security-recommendations"></a>Uitvoering van de aanbevelingen voor beveiliging

### <a name="set-recommendations"></a>Set aanbevelingen

In de [instelling voor beveiligingsbeleid in Azure Beveiligingscentrum](security-center-policies.md)leren u:

- Beveiligingsbeleid configureren.
- Het verzamelen van gegevens inschakelen.
- Kies welke aanbevelingen om te zien als onderdeel van het beveiligingsbeleid.

Huidige beleid aanbevelingen center rond systeemupdates, regels volgens de basislijn, antimalware-programma's, [netwerk-beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) op subnetten en netwerkinterfaces, controle van de SQL-database SQL database transparante gegevenscodering en web application firewalls.  [Het instellen van beveiligingsbeleid](security-center-policies.md) vindt u een beschrijving van elke optie aanbeveling.

### <a name="monitor-recommendations"></a>Aanbevelingen voor monitor
Na het instellen van beveiligingsbeleid analyseert Beveiligingscentrum de beveiligingsstatus van uw middelen ter identificatie van beveiligingslekken. De **aanbevelingen** naast elkaar op het blad **Security Center** kunt u weten hoeveel aangeduid met Security Center aanbevelingen.

![Aanbevelingen tegel][1]

Kunt u de details van elke aanbeveling:

1. Klik op de **aanbevelingen naast elkaar** op het blad **Security Center** . Hiermee opent u het blad **aanbevelingen** .

De aanbevelingen worden weergegeven in een tabel waarbij elke regel staat voor één bepaalde aanbeveling. De kolommen van deze tabel zijn:

- **Beschrijving**: de aanbeveling en wat moet worden gedaan om het op te lossen.
- **Bron**: Hier worden de bronnen waarvoor deze aanbeveling geldt.
- **Status**: beschrijft de huidige status van de aanbeveling:
    - **Open**: de aanbeveling nog niet is verholpen.
    - **In uitvoering**: de aanbeveling dat momenteel wordt toegepast op de bronnen en door u is geen actie vereist.
    - **Opgelost**: de aanbeveling is voltooid (in dit geval wordt de regel wordt grijs weergegeven).
- **ERNST**: Beschrijving van de ernst van deze specifieke aanbeveling:
    - **Hoog**: een beveiligingsprobleem bestaat met een zinvolle resource (bijvoorbeeld een toepassing, een VM of een beveiligingsgroep netwerk) en aandacht vereist.
    - **Medium**: Er bestaat een beveiligingslek en niet-kritieke of aanvullende stappen zijn vereist om deze weg te nemen of om een proces te voltooien.
    - **Laag**: Er bestaat een beveiligingslek met betrekking tot die moeten worden behandeld, maar geen onmiddellijke aandacht vereist. (Standaard lage aanbevelingen worden niet weergegeven, maar u kunt filteren op lage aanbevelingen als u wilt zien.)

De onderstaande tabel als referentie gebruiken waarmee u inzicht in de beschikbare aanbevelingen en wat elke doen als u het toepast.

> [AZURE.NOTE] Wilt u inzicht in de [klassieke en Resource Manager implementatiemodellen](../azure-classic-rm.md) voor Azure resources.

|Aanbeveling|Beschrijving|
|-----|-----|
|[Gegevens verzamelen voor abonnementen inschakelen](security-center-enable-data-collection.md)|Raadt aan om te schakelen op het verzamelen van gegevens in het beveiligingsbeleid voor elk van uw abonnementen en alle virtuele machines (VMs) in uw abonnementen.|
|[Een OS beveiligingslekken te verhelpen](security-center-remediate-os-vulnerabilities.md)|Aanbevolen dat u de OS-configuraties met de aanbevolen configuratieregels, uitlijnen bijvoorbeeld toestaan niet dat wachtwoorden worden opgeslagen.|
|[Systeemupdates toepassen](security-center-apply-system-updates.md)|Raadt ontbrekende systeem beveiligingsupdates en essentiële updates voor VMs.|
|[Opnieuw opstarten na systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates)|Raadt u aan een VM systeemupdates toe te passen om opnieuw te starten.|
|[Een web application firewall toevoegen](security-center-add-web-application-firewall.md)|Raadt een web application firewall (WAF) voor web-eindpunten. U kunt meerdere webtoepassingen in Beveiligingscentrum beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties. WAF-toestellen (gemaakt met het implementatiemodel Resource Manager) moeten worden geïmplementeerd op een afzonderlijke virtuele netwerk. WAF-toestellen (gemaakt met het klassieke implementatiemodel) zijn beperkt tot het met behulp van een beveiligingsgroep voor het netwerk. Deze ondersteuning zal in de toekomst worden uitgebreid tot een volledig aangepaste implementatie van een WAF toestel (klassiek). Beveiligingscentrum wordt aanbevolen het inrichten van een WAF om u te beschermen tegen aanvallen zijn geweest van uw webtoepassingen op VMs en op App Service omgeving (ASE). Voor meer informatie over het ASE, raadpleegt u de [Documentatie van App Service omgeving](../app-service/app-service-app-service-environments-readme.md). |
|[Toepassingsbeveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection)|Als u de configuratie van een WAF wilt moet verkeer worden omgeleid naar het toestel WAF. Na deze aanbeveling wordt de nodige wijzigingen worden voltooid.|
|[Een volgende generatie Firewall toevoegen](security-center-add-next-generation-firewall.md)|Beveelt de volgende generatie Firewall (NGFW) van een Microsoft-partner toe te voegen om de beveiligingsinstellingen te verhogen.|
|[Route-verkeer via NGFW alleen](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Wordt aanbevolen dat u configureert network group (NSG) regels voor binnenkomend verkeer voor uw VM via uw NGFW te dwingen.|
|[Endpoint Protection installeren](security-center-install-endpoint-protection.md)|Adviseert het inrichten van VMs (alleen Windows VMs) antimalware programma's.|
|[Endpoint Protection gezondheid waarschuwingen oplossen](security-center-resolve-endpoint-protection-health-alerts.md)|Wordt aanbevolen dat u de endpoint protection fouten oplost.|
|[Inschakelen netwerk beveiligingsgroepen op subnetten of virtuele machines](security-center-enable-network-security-groups.md)|Raadt aan om NSGs in te schakelen op subnetten of VMs.|
|[Beperken van toegang via Internet facing eindpunt](security-center-restrict-access-through-internet-facing-endpoints.md)|Adviseert het configureren van regels voor binnenkomend verkeer voor NSGs.|
|[Controle van SQL-server inschakelen](security-center-enable-auditing-on-sql-servers.md)|Wordt aanbevolen dat u de controle voor Azure SQL servers (Azure SQL service; niet opgenomen op de virtuele machines met SQL).|
|[Database SQL-controle inschakelen](security-center-enable-auditing-on-sql-databases.md)|Wordt aanbevolen dat u de controle voor Azure SQL-databases (Azure SQL service; niet opgenomen op de virtuele machines met SQL).|
|[Schakel transparante gegevenscodering op SQL-databases](security-center-enable-transparent-data-encryption.md)|Raadt aan om de codering voor SQL-databases (alleen SQL Azure service) in te schakelen.|
|[VM-Agent inschakelen](security-center-enable-vm-agent.md)|Hiermee kunt u zien welke VMs vereisen de VM-Agent. De VM-Agent moet worden geïnstalleerd op VMs wilt inrichten patch scannen basislijn scannen en antimalware-programma's. De VM-Agent wordt standaard geïnstalleerd voor VMs die uit de markt Azure worden geïmplementeerd. Het artikel [VM Agent en extensies – deel 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) bevat informatie over het installeren van de Agent VM.|
| [Schijf coderen](security-center-apply-disk-encryption.md) |Raadt de VM-schijven met Azure schijfversleuteling (Windows en Linux VMs) te coderen. Codering wordt aanbevolen voor volumes van het besturingssysteem en de gegevens op uw VM.|
|[Contactgegevens van beveiliging bieden](security-center-provide-security-contact-details.md) | Aanbevolen dat u beveiliging een contactgegevens voor elk van uw abonnementen. Contactgegevens is een nummer van een e-mailadres en telefoonnummer. De informatie wordt gebruikt om contact met u opnemen als ons security team vindt dat uw resources in gevaar komen. |
| [Versie van het besturingssysteem bijwerken](security-center-update-os-version.md) | Raadt aan om de versie van het besturingssysteem (OS) te werken voor uw Cloud-Service met de meest recente versie beschikbaar voor de OS-familie.  Zie voor meer informatie over Cloud Services, [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md). |
| [Beveiligingslek met betrekking tot beoordeling is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) | Raadt u aan de oplossing van een beveiligingslek met betrekking tot beoordeling te installeren op uw VM. |
| [Problemen verhelpen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Kunt u systeem- en beveiligingsproblemen ontdekt door het beveiligingslek met betrekking tot beoordeling oplossing op uw VM is geïnstalleerd. |

U kunt filteren en aanbevelingen negeren.

1. Klik op **Filter** in het blad **aanbevelingen** . Hiermee opent u het **Filter** blade en selecteert u de prioriteit en status-waarden die u wilt zien.

    ![Aanbevelingen voor filter][2]

2. Als u vaststelt dat een aanbeveling niet van toepassing is, kunt u de aanbeveling negeert en filtert vervolgens uit zicht. Er zijn twee manieren om een aanbeveling te sluiten. Een manier is om Klik met de rechtermuisknop op een item en selecteer vervolgens **verwijderen**. De andere is de muisaanwijzer op een item, klikt u op de drie puntjes rechts en selecteer **verwijderen**. U kunt de ontslagen aanbevelingen weergeven door te klikken op **Filter**en vervolgens te klikken op **Dismissed**.

    ![Aanbeveling negeren][3]

### <a name="apply-recommendations"></a>Toepassing van de aanbevelingen
Bekijk bepalen alle aanbevelingen welke u wordt aangeraden eerst. Wij raden u aan het prioriteitsniveau te gebruiken, zoals de belangrijkste parameter om te evalueren welke aanbevelingen moet eerst worden toegepast.

Selecteer een aanbeveling in de tabel van de bovenstaande aanbevelingen en wandel door het als een voorbeeld van het toepassen van een aanbeveling.

## <a name="see-also"></a>Zie ook
In dit document, kunt u kennisgemaakt met Security Center aanbevelingen voor beveiliging. Voor meer informatie over de Security Center, raadpleegt u het volgende:

- [Het instellen van beveiligingsbeleid in Beveiligingscentrum Azure](security-center-policies.md) , informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Security health monitoring in Beveiligingscentrum Azure](security-center-monitoring.md) , informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Beveiligingscentrum Azure](security-center-managing-and-responding-alerts.md) , informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring oplossingen van partners met Azure Security Center](security-center-partner-solutions.md) , informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het Azure Security Center](security-center-faq.md) , zoeken, veelgestelde vragen over het gebruik van de service.
- [Beveiligingsblog voor Azure](http://blogs.msdn.com/b/azuresecurity/) — vinden over Azure beveiliging en naleving van blogberichten.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
