<properties
 pageTitle="Scheduler hoge beschikbaarheid en betrouwbaarheid"
 description="Scheduler hoge beschikbaarheid en betrouwbaarheid"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Scheduler hoge beschikbaarheid en betrouwbaarheid

## <a name="azure-scheduler-high-availability"></a>Azure Scheduler, hoge beschikbaarheid

Als een service Azure platform core Azure Scheduler maximaal beschikbaar is en zowel geo-redundante service implementeren en geo-regionale taak replicatie-functies.

### <a name="geo-redundant-service-deployment"></a>Geo-redundante service implementeren

Azure Scheduler is beschikbaar via de gebruikersinterface in bijna elke geografische gebied dat in Azure vandaag. De lijst van regio's die Azure Scheduler is beschikbaar in is [Hier worden vermeld](https://azure.microsoft.com/regions/#services). Als een datacenter in een gehoste gebied wordt weergegeven niet beschikbaar, zijn de failover-mogelijkheden van Azure Scheduler zodanig dat de service beschikbaar in een ander datacenter is.

### <a name="geo-regional-job-replication"></a>Geo-regionale taak replicatie

Is niet alleen de Scheduler Azure front-end beschikbaar voor het beheer van aanvragen, maar uw eigen werk is ook geo gerepliceerd. Er is een storing in één regio, Azure Scheduler wordt overgenomen als zorgt ervoor dat de taak wordt uitgevoerd vanuit een ander datacenter in de gepaarde geografische regio.

Bijvoorbeeld als u een taak hebt gemaakt in Zuid-centraal ons, Azure Scheduler automatisch gerepliceerd die taak in North Central ons. Als er een storing in Zuid-centraal ons, Azure Scheduler zorgt u ervoor dat de taak wordt uitgevoerd bij North Central ons. 

![][1]

Als gevolg hiervan Azure Scheduler zorgt ervoor dat uw gegevens altijd binnen hetzelfde ruimere geografische regio in geval van een defecte Azure. Als gevolg hiervan moet niet dupliceren van uw werk gewoon om toe te voegen van hoge beschikbaarheid: Azure Scheduler automatisch biedt functies voor hoge beschikbaarheid voor uw projecten.

## <a name="azure-scheduler-reliability"></a>Azure Scheduler betrouwbaarheid

Azure Scheduler garandeert een hoge beschikbaarheid en een andere benadering gaat naar door de gebruiker gemaakte taken. De taak kan bijvoorbeeld een HTTP-eindpunt dat is niet beschikbaar aanroepen. Azure planner wil toch met succes, de taak uitgevoerd door middel van alternatieve opties voor de behandeling van de fout. Azure Scheduler gebeurt dit op twee manieren:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Configureerbare opnieuw beleid via "retryPolicy"

Azure Scheduler kunt u een beleid opnieuw configureren. Standaard als een taak mislukt, probeert Taakplanner de taak opnieuw vier keer, met tussenpozen van 30 seconden. U kunt dit beleid opnieuw worden geplaatst (bijvoorbeeld tien keer 30 seconden intervallen) opnieuw configureren of lossere (bijvoorbeeld twee keer met dagelijkse intervallen.)

Als een voorbeeld van wanneer dit kan helpen, kunt u een taak die eens per week wordt uitgevoerd en wordt een HTTP-eindpunt mag maken. Als de HTTP-eindpunt is niet beschikbaar voor een paar uur wanneer de taak wordt uitgevoerd, kunt u niet wachten een week van meer voor de taak opnieuw uit te voeren omdat zelfs het standaardbeleid voor opnieuw proberen mislukt. In dergelijke gevallen kunt u het beleid standaard opnieuw te proberen om de drie uur (bijvoorbeeld) opnieuw configureren in plaats van elke 30 seconden.

Voor meer informatie over het configureren van een beleid voor nieuwe pogingen, verwijzen naar [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternatieve eindpunt configureerbaarheid via "errorAction"

Als het eindpunt van het doel voor de job Scheduler Azure onbereikbaar blijft, valt Azure Scheduler terug op het andere eindpunt van de afhandeling van fouten na het volgen van haar beleid opnieuw. Als een alternatief eindpunt voor foutafhandeling is geconfigureerd, wordt het door Azure Scheduler roept. Uw eigen taken zijn met een alternatieve eindpunt hoge beschikbaarheid tegen storingen.

Als u bijvoorbeeld in het diagram hieronder volgt Azure Scheduler haar beleid opnieuw om de druk van een webservice van New York. Nadat de pogingen mislukken, wordt gecontroleerd of er een alternatief is. Vervolgens gaat u vooruit en begint het aanvragen in de alternatieve met hetzelfde beleid opnieuw.

![][2]

Houd er rekening mee dat opnieuw hetzelfde beleid voor zowel de oorspronkelijke actie en de alternatieve fout geldt. Het is ook mogelijk om van de alternatieve fout actie actietype afwijken van de belangrijkste actie actietype. Bijvoorbeeld, terwijl de belangrijkste actie van een HTTP-eindpunt aanroepen kan, de actie bij fout in plaats daarvan mogelijk een wachtrij voor opslag, service bus wachtrij of service bus onderwerp actie waarmee fouten vastleggen.

Voor meer informatie over het configureren van een alternatief eindpunt, verwijzen naar [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Zie ook

 [Wat is Taakplanner?](scheduler-intro.md)

 [Azure Scheduler concepten en terminologie entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met de planner in Azure portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure planner](scheduler-plans-billing.md)

 [Het bouwen van complexe schema's en geavanceerde herhaling met Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API: naslag](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlets verwijzing](scheduler-powershell-reference.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Uitgaande verificatie van Azure Scheduler](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
