<properties
    pageTitle="Wat te doen bij een Azure service verstoring van de invloed van Azure Cloud Services | Microsoft Azure"
    description="Informatie over wat te doen in geval van een onderbreking Azure service die van invloed op Azure Cloud Services."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Wat te doen bij een Azure verstoring van de invloed van Azure Cloud Services-service

Bij Microsoft werken we hard om ervoor te zorgen dat onze services altijd voor u beschikbaar zijn wanneer u ze nodig hebt. Krachten buiten onze wil soms van invloed op ons op manieren die ervoor zorgen dat de niet-geplande serviceonderbrekingen.

Microsoft heeft een Service niveau overeenkomst (SLA) voor zijn diensten als een verbintenis voor beschikbaarheid en connectiviteit. De SLA voor afzonderlijke Azure services kunt u vinden op [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure is al veel ingebouwde functies die ondersteuning bieden voor toepassingen met maximale beschikbaarheid. Lees voor meer informatie over deze services, [noodherstel en hoge beschikbaarheid voor Azure toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Dit artikel heeft betrekking op true herstelscenario wanneer een hele regio een storing door grote natuurrampen of onderbreking van de wijdverbreide service krijgt. Dit zeldzame exemplaren zijn, maar u moet voorbereiden op de mogelijkheid dat er sprake is van een storing van een hele regio. Als er een onderbreking van de service in een hele regio, kunnen de lokale kopieën van uw gegevens zouden tijdelijk niet beschikbaar. Als u de geo-replicatie hebt ingeschakeld, worden drie extra kopieën van uw opslag Azure BLOB's en tabellen opgeslagen in een andere regio. Bij een volledige regionale uitvalt of een noodsituatie waarin de primaire regio kan niet worden hersteld, remaps Azure alle DNS-vermeldingen op de regio geo gerepliceerd.

>[AZURE.NOTE]Let erop dat er geen controle over dit proces en alleen voor datacenter-wide serviceonderbrekingen gebeurt. Daarom moet u ook gebaseerd op een andere toepassingsspecifieke back-upstrategieën te bereiken van het hoogste niveau van beschikbaarheid. Zie het gedeelte over [Data strategieën voor herstel na noodgevallen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR)voor meer informatie. Als u kunnen van invloed zijn op uw eigen failover wilt, is het raadzaam rekening te houden met het gebruik van [geo-redundante opslag lezen-toegang (RA-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), maakt u een alleen-lezen kopie van de gegevens in een ander gebied.

Om u te helpen deze zeldzame exemplaren te verwerken, bieden wij de volgende richtlijnen voor Azure virtuele machines (VMs) in het geval van een onderbreking van de service van het gehele gebied waarin uw Azure VM-toepassing wordt geïmplementeerd.

##<a name="option-1-wait-for-recovery"></a>Optie 1: Wachten op herstel
In dat geval is geen actie van uw kant vereist. Weet dat Azure teams naar eer en geweten werken om de beschikbaarheid van de service te herstellen. De huidige servicestatus kunt u bekijken op onze [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Dit is de beste optie als een klant Azure Site herstel niet ingesteld of een secundaire distributie in een andere regio heeft.

De volgende opties zijn beschikbaar voor klanten die directe toegang tot hun geïmplementeerde cloud diensten.

>[AZURE.NOTE]Let erop dat deze opties de mogelijkheid van gegevensverlies hebben.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Optie 2: De configuratie van de service cloud naar een nieuw gebied opnieuw te implementeren

Als u de oorspronkelijke code, kunt u de toepassing, gekoppelde configuratie en bijbehorende resources aan een nieuwe wolk service in een nieuw gebied gewoon net implementeren.  

Zie voor meer informatie over het maken en implementeren van een cloud service-toepassing [maken en implementeren van een cloud-service](./cloud-services-how-to-create-deploy-portal.md).

Afhankelijk van de gegevensbronnen van uw toepassing moet u controleren de invorderingsprocedures voor de gegevensbron voor uw toepassing.
  * Zie voor gegevensbronnen Azure opslag, [replicatie Azure opslag](../storage/storage-redundancy.md#read-access-geo-redundant-storage) bij het controleren van de opties die beschikbaar zijn op basis van het replicatiemodel hebt gekozen voor uw toepassing.
  * Lees voor bronnen voor SQL-Database, [Overzicht: Cloud business continuity en database noodherstel met SQL-Database](../sql-database/sql-database-business-continuity.md) bij het controleren van de opties die beschikbaar zijn op basis van de gekozen replicatiemodel voor uw toepassing.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Optie 3: Een back-up distributie via Azure verkeer Manager gebruiken
Deze optie wordt ervan uitgegaan dat u al uw oplossing met regionale noodherstel in gedachten hebt ontworpen. U kunt deze optie als u al een secundaire cloud services distributie die wordt uitgevoerd in een andere regio en verbonden via een kanaal verkeer manager. Controleer in dit geval, de gezondheid van de secundaire distributie. Als het goed is, kunt u verkeer omleiden naar het via Azure verkeer Manager. Met deze strategie kunt u profiteren van de routeringsmethode verkeer en failover-order configuraties in Azure verkeer Manager. Zie voor meer informatie [het configureren van instellingen voor beheer van netwerkverkeer](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Azure Cloud Services te verdelen over de regio's met Azure verkeer Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het implementeren van een noodherstel en een strategie voor hoge beschikbaarheid, [noodherstel en hoge beschikbaarheid voor Azure toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Zie [Azure resiliency technische richtsnoeren](../resiliency/resiliency-technical-guidance.md)voor het ontwikkelen van een gedetailleerde technische kennis van de mogelijkheden van een wolk platform.

Als de instructies worden niet gewist, of als u wilt dat Microsoft voor de bewerkingen namens u contact op met [Customer Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
