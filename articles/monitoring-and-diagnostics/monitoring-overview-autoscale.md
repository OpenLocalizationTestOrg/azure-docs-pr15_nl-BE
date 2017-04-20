<properties
    pageTitle="Overzicht van automatisch schalen in virtuele Machines van Microsoft Azure, Cloud Services en Web Apps | Microsoft Azure"
    description="Overzicht van automatisch schalen in Microsoft Azure. Van toepassing op virtuele Machines, Cloud Services en Web Apps."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Overzicht van automatisch schalen in virtuele Machines van Microsoft Azure, Cloud Services en Web Apps

In dit artikel wordt beschreven welke Microsoft Azure automatisch schalen is, de voordelen, aan de slag met het.  

Azure Monitor automatisch schalen geldt alleen voor [Virtuele Machine schaal Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/)en [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure is op twee manieren automatisch schalen. Een oudere versie van automatisch schalen is van toepassing op virtuele Machines (sets van beschikbaarheid). Deze functie biedt beperkte ondersteuning en beste migreren naar VM schaal ingesteld voor ondersteuning van automatisch schalen sneller en betrouwbaarder. Een koppeling voor het gebruik van de oudere technologie is opgenomen in dit artikel.  


## <a name="what-is-autoscale"></a>Wat is automatisch schalen?

Automatisch schalen kunt u het juiste bedrag van de middelen voor het afhandelen van de belasting van uw toepassing wordt uitgevoerd. U kunt resources stijgingen van de belasting te verwerken en ook geld besparen door het verwijderen van resources die zit toevoegen die niet actief. U opgeven een minimum en maximum aantal keren uitgevoerd en toevoegen of verwijderen van VMs automatisch op basis van een reeks regels. Met een minimale zorgt ervoor dat uw toepassing wordt altijd uitgevoerd zelfs geen belasting. Met een beperkt mogelijk van de totale kosten per uur. U wordt automatisch schalen tussenin met regels die u maakt.

 ![Automatisch schalen toegelicht. Toevoegen en verwijderen van VMs](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Als regelvoorwaarden is voldaan, worden een of meer acties van automatisch schalen wordt geactiveerd. U kunt toevoegen en verwijderen van VMs of andere acties uitvoeren. De volgende conceptueel diagram toont dit proces.  

 ![Stroomdiagram van conceptuele automatisch schalen](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Automatisch schalen proces uitgelegd
De volgende uitleg van toepassing op de stukken van het vorige diagram.   

### <a name="resource-metrics"></a>Resource metrics
Resources verzenden metrics, die later worden verwerkt door de regels. Statistieken worden geleverd via verschillende methoden.
VM-schaal wordt gebruikt telemetriegegevens van Azure Diagnostische agentia dat telemetrie voor Web apps en Cloud services direct afkomstig van de infrastructuur Azure. Sommige algemeen gebruikte statistieken omvatten het CPU-gebruik, geheugengebruik, thread telt, wachtrijlengte en gebruik van de schijf. Zie voor een overzicht van welke telemetriegegevens kunt u [Algemene statistieken automatisch schalen](insights-autoscale-common-metrics.md).

### <a name="time"></a>Tijd
Regels op basis van een schema zijn gebaseerd op UTC. U moet uw tijdzone correct ingesteld bij het instellen van regels.  

### <a name="rules"></a>Regels
Het diagram toont slechts één regel voor automatisch schalen kunt, maar u veel van hen. Kunt u complexe, overlappende regels als nodig is voor uw situatie.  Typen opnemen  

 - **Op basis van een metric** - deze actie bijvoorbeeld doen wanneer het CPU-gebruik meer dan 50 is %.
 - **Op basis van tijd** - bijvoorbeeld een webhook om 8 uur op zaterdag in een bepaalde tijdzone trigger.

Metric gebaseerde regels toepassing laden meten en toevoegen of verwijderen op basis van die belasting VMs. Regels op basis van een planning kunt u schaalt wanneer u Zie tijdnotatie in de belasting en wilt schalen voordat een mogelijke belasting stijging of daling doet zich voor.  


### <a name="actions-and-automation"></a>Acties en automatisering

Regels kunnen een of meer typen acties activeren.

- **Schaal** - schaal VMs in- of uitzoomen
- **E-mailadres** - e-mail verzenden abonnement beheerders, co-beheerders en/of extra e-mailadres dat u opgeeft
- **Automatiseren via webhooks** - aanroep van webhooks, die in meerdere complexe acties binnen of buiten de Azure resulteren kan. U kunt in Azure, een runbook Azure automatisering, Azure functie of Azure logica App starten. Voorbeeld van de 3de partij-URL buiten Azure services, zoals vertraging en Twilio bevatten.


## <a name="autoscale-settings"></a>Instellingen voor automatisch schalen
Automatisch schalen gebruiken de volgende terminologie en structuur.

- Een **instelling automatisch schalen** wordt gelezen door de motor automatisch schalen om te bepalen of de schaal omhoog of omlaag. Het bevat een of meer profielen, informatie over de resource doel en instellingen voor meldingen.
    - Een **profiel automatisch schalen** is een combinatie van een instelling, een set van regels voor de triggers en acties van de schaal voor het profiel en een terugkeerpatroon capaciteit. U kunt meerdere profielen, zodat u kunt zorgen voor verschillende overlappende eisen hebben.
        - De **instelling van de capaciteit** geeft aan de minimale, maximale en standaardwaarden voor aantal exemplaren. [figuur 1 gebruiken om de juiste plaats]
        - Een **regel** van een trigger bevat, een metrische trigger of een trigger tijd, en een schaal, die aangeeft of automatisch schalen wordt geschaald omhoog of omlaag wanneer de regel wordt voldaan.
        - Een **Terugkeerpatroon** aangeeft wanneer automatisch schalen moet dit profiel tot stand is gebracht. U kunt bijvoorbeeld verschillende automatisch schalen profielen voor verschillende tijdstippen van de dag of dagen van de week, hebben.
- Een **instelling van de melding** bepaalt welke meldingen moeten worden uitgevoerd als een gebeurtenis automatisch schalen op basis van die voldoet aan de criteria van een van de profielen van de instelling automatisch schalen. Automatisch schalen kan een of meer e-mailadressen in kennis of aanroepen naar een of meer webhooks.

![Instelling Azure automatisch schalen, profiel en regelstructuur](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

De volledige lijst met velden kan worden geconfigureerd en beschrijvingen is beschikbaar in het [Automatisch schalen REST API](https://msdn.microsoft.com/library/dn931928.aspx).

Zie voor voorbeelden van programmacode

* [Configuratie van geavanceerde automatisch schalen met bronbeheer sjablonen voor VM schaal Sets](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Automatisch schalen REST API](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Horizontale en verticale schaling

Automatisch schalen vergroot bronnen in enige schalen horizontaal, die een stijging ("out") of verkleinen ('in') in het aantal exemplaren van de VM.  Horizontale schaal, die in een situatie van de wolk meer flexibiliteit omdat u mogelijk duizenden VMs verwerken belasting uitvoeren. Verticaal schalen is anders. Het houdt hetzelfde aantal VMs, maar zorgt ervoor dat de VM meer ('up') of minder ('down') krachtige. Energie wordt gemeten in het geheugen, CPU-snelheid, ruimte, enz.  Verticale schaling heeft meer beperkingen. Het is afhankelijk van de beschikbaarheid van grotere hardware, die kan verschillen per regio en raakt snel en bovengrens. Verticaal schalen is meestal ook vereist een VM stoppen en starten. Voor meer informatie Zie [Azure virtuele machine met Azure automatisering verticaal schalen](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


## <a name="methods-of-access"></a>Methoden van access
U kunt automatisch schalen via instellen

- [Azure portal](insights-how-to-scale.md)
- [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Cross-platform Command Line Interface (CLI)](insights-cli-samples.md#autoscale )
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Ondersteunde services voor automatisch schalen


| Service                              | Schema & Docs                                       |
|--------------------------------------|-----------------------------------------------------|
| Web Apps                             | [Web Apps schaal](insights-how-to-scale.md)              |
| Cloud Services                       | [Automatisch schalen een Cloud-Service](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuele Machines: klassiek           | [Klassieke virtuele Machine beschikbaarheid Sets schalen](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuele Machines: Windows schaal Sets| [VM-schaal schaal wordt ingesteld in Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Virtuele Machines: Linux schaal instellen  | [VM-schaal schaal wordt ingesteld in Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuele Machines: Voorbeeld van de Windows   | [Configuratie van geavanceerde automatisch schalen met bronbeheer sjablonen voor VM schaal Sets](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het automatisch schalen, gebruik de Autoscale Walkthroughs eerder is vermeld, of Raadpleeg de volgende bronnen:

- [Azure Monitor automatisch schalen algemene statistieken](insights-autoscale-common-metrics.md)
- [Aanbevolen procedures voor Azure Monitor automatisch schalen](insights-autoscale-best-practices.md)
- [Automatisch schalen acties gebruiken voor het verzenden van e-mail en webhook meldingen](insights-autoscale-to-webhook-email.md)
- [Automatisch schalen REST API](https://msdn.microsoft.com/library/dn931953.aspx)
- [Probleemoplossing virtuele Machine schaal stelt automatisch schalen](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
