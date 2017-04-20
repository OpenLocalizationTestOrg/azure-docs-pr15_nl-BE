<properties
 pageTitle="Wat is Taakplanner Azure? | Microsoft Azure"
 description="Azure Scheduler kunt u declaratief beschreven acties worden uitgevoerd in de cloud. Het wordt gepland en die acties automatisch worden uitgevoerd."
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
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Wat is Taakplanner Azure?

Azure Scheduler kunt u declaratief beschreven acties worden uitgevoerd in de cloud. Het wordt gepland en die acties automatisch worden uitgevoerd.  Planner maakt gebruik van [de portal Azure](scheduler-get-started-portal.md), code, [REST API](https://msdn.microsoft.com/library/mt629143.aspx)of Azure PowerShell.

Planner maakt, onderhoudt en roept de geplande hoeveelheid werk.  Scheduler niet alle werklasten hosten of elke willekeurige code uitvoeren. Alleen code _roept_ elders ondergebracht IT — in Azure, in ruimten of met een andere provider. Deze methode opent via HTTP, HTTPS, een wachtrij voor de opslag, een service bus wachtrij of een onderwerp service bus

Planner [taken](scheduler-concepts-terms.md)plant, wordt bijgehouden van de taak tot het uitvoeren van resultaten een kunt bekijken en jokertekenelement en betrouwbaar plant werklasten worden uitgevoerd. Azure WebJobs (onderdeel van de functie Web Apps in Azure App Service) en andere Azure mogelijkheden plannen planner op de achtergrond gebruikt. De [Planner REST API](https://msdn.microsoft.com/library/mt629143.aspx) helpt bij de communicatie voor deze acties. Als zodanig ondersteunt planner [complexe schema's en geavanceerde herhaling](scheduler-advanced-complexity.md) gemakkelijk.

Er zijn verschillende scenario's die zich voor het gebruik van Taakplanner lenen. Bijvoorbeeld:

+ _Terugkerende toepassing acties:_ Regelmatig verzamelen van gegevens van Twitter in een feed.
+ _Dagelijks onderhoud:_ Dagelijkse weghalen van Logboeken, back-ups en andere onderhoudstaken uitvoeren. Bijvoorbeeld wilt een beheerder back-up van de database om 1:00 uur elke dag van de volgende negen maanden.

Taakplanner kunt u maken, bijwerken, verwijderen, weergeven en taken en [Project-collecties](scheduler-concepts-terms.md) via programmacode, met behulp van scripts en beheren in de portal.

## <a name="see-also"></a>Zie ook

 [Azure Scheduler concepten en terminologie entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met de planner in Azure portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure planner](scheduler-plans-billing.md)

 [Het bouwen van complexe schema's en geavanceerde herhaling met Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API: naslag](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlets verwijzing](scheduler-powershell-reference.md)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Uitgaande verificatie van Azure Scheduler](scheduler-outbound-authentication.md)
