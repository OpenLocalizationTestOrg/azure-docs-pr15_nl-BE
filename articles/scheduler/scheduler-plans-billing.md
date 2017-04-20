<properties
 pageTitle="Plannen en facturering in Azure planner"
 description="Plannen en facturering in Azure planner"
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
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Plannen en facturering in Azure planner

## <a name="job-collection-plans"></a>Project collectie plannen

Taak collecties zijn de factureerbare entiteit in Azure Scheduler. Taak collecties bevatten een aantal taken en worden geleverd in drie plannen – vrije, Standard en Premium – die hieronder worden beschreven.

|**Plan voor het verzamelen van taak**|**Maximum aantal taken per taak, collectie**|**Terugkeerpatroon voor max.**|**De functie Max collecties per abonnement**|**Limieten**|
|:---|:---|:---|:---|:---|
|**Gratis**|5 taken per taak, collectie|Eén keer per uur. Taken niet vaker dan eens per uur worden uitgevoerd|Een abonnement mag maximaal 1 gratis project collectie|Kan geen [HTTP-verificatie voor uitgaande-object](scheduler-outbound-authentication.md) gebruiken
|**Standaard**|50 taken per taak, collectie|Eenmaal per minuut. Taken niet vaker dan eenmaal per minuut worden uitgevoerd|Een abonnement mag maximaal 100 standaard taak collecties|Toegang tot het volledige onderdelenpakket van Taakplanner|
|**Premium P10**|50 taken per taak, collectie|Eenmaal per minuut. Taken niet vaker dan eenmaal per minuut worden uitgevoerd|Een abonnement mag maximaal 10.000 P10 Premium taak collecties. <a href="mailto:wapteams@microsoft.com">Neem contact met ons</a> op voor meer informatie.|Toegang tot het volledige onderdelenpakket van Taakplanner|
|**P20 Premium**|1000 taken per taak, collectie|Eenmaal per minuut. Taken niet vaker dan eenmaal per minuut worden uitgevoerd|Een abonnement mag maximaal 10.000 P20 Premium taak collecties. <a href="mailto:wapteams@microsoft.com">Neem contact met ons</a> op voor meer informatie.|Toegang tot het volledige onderdelenpakket van Taakplanner|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Upgrades en Downgrades van taak collectie plannen

U kunt upgrade of downgrade een taak collectie plan op elk moment over de vrije, Standard en Premium plannen. Wanneer downgraden naar een verzameling gratis taak, kan echter de downgrade voor een van de volgende redenen mislukken:

- Een verzameling gratis taak al bestaat in het abonnement
- Een taak in de collectie van de taak heeft een hogere herhaling dan toegestaan voor projecten in Project gratis collecties. De maximale herhaling toegestaan in een verzameling gratis taak wordt eenmaal per uur
- Er zijn meer dan 5 taken in de project-collectie
- Een taak in de collectie van het project is een HTTP- of HTTPS-actie die gebruikmaakt van een [HTTP-verificatie voor uitgaande-object](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Factuur- en Azure plannen

Abonnementen worden niet in rekening gebracht voor het vrije project collecties. Als er meer dan 100 standaard taak collecties (10 standaard facturatie-eenheden), dan is een betere deal hebben alle collecties van de taak in het speciale niveau.

Als u één taak standaard collectie en een taak premie-incasso, bent u gefactureerde één standaard facturering eenheid _en_ één facturatie-eenheid. De Taakplanner-service van rekeningen op basis van het aantal actieve taak collecties die zijn ingesteld op standard of premium; Dit wordt nader uitgelegd in de volgende twee secties.

## <a name="standard-billable-units"></a>Standaard factureerbare eenheden

Een standaard factureerbare eenheden kan bevatten maximaal 10 standaard taak. Aangezien een standaard taak-collectie maximaal 50 functies per taak collectie hebben kan, kan een standaardeenheid facturering een abonnement tot 500 taken – tot uitvoering bijna 22 miljoen per maand hebben.

Als u tussen 1 en 10 collecties van project standard hebt, zult u gefactureerd 1 standaard facturering eenheid. Als u tussen 11 en 20 collecties van project standard hebt, zult u gefactureerd 2 standaardeenheden facturering. Als er tussen de 21 en 30 standaard taak collecties, u zult worden gefactureerd voor 3 standaardeenheden facturering, enzovoort.

## <a name="p10-premium-billable-units"></a>P10 Premium factureerbare eenheden

Een P10 premium factureerbare eenheden kan bevatten maximaal 10.000 P10 premium taak. Aangezien een P10 premium taak collectie maximaal 50 functies per taak collectie hebben kan, kan een facturering-eenheid een abonnement te hebben van maximaal 500.000 taken – tot uitvoering bijna 22 miljard per maand.

Als er tussen de 1 en 10.000 premie taak, zult u worden gefactureerd voor 1 P10 facturering-eenheid. Als er tussen 10,001 en 20.000 premie taak collecties, u zult worden gefactureerd voor premium 2 P10 facturering eenheden, enzovoort.

Dus P10 premium taak collecties hebben dezelfde functionaliteit als de standaard taak collecties maar bieden een prijsverschil in het geval uw toepassing een groot aantal collecties van de taak vereist.

## <a name="p20-premium-billable-units"></a>P20 Premium factureerbare eenheden

Een P20 premium factureerbare eenheden kan bevatten maximaal 5.000 P20 premium taak. Aangezien een P20 premium job-collectie maximaal 1000 taken per taak collectie hebben kan, kan een facturering-eenheid een abonnement hebben op 5,000,000 taken – tot uitvoering bijna 220 miljard per maand.

P20 premium taak collecties biedt dezelfde functionaliteit als P10 premium taak collecties, maar ondersteunt ook een groter aantal taken per taak collectie en een groter aantal algemene taken dan P10 premie waarmee u meer schaalbaarheid.

## <a name="billing-and-active-status"></a>Facturering en actieve Status

Taak collecties zijn altijd actief, tenzij je hele abonnement zijn geworden sommige tijdelijk uitgeschakeld als gevolg van problemen met facturering. De enige manier om ervoor te zorgen dat een collectie van de taak niet is gefactureerd is een instelling de _vrije_ plan of de collectie taak verwijderen.

Hoewel u alle taken binnen de collectie van een taak in één bewerking kunt uitschakelen, niet de factureringsstatus van de collectie taak verandert – de collectie van de taak wordt worden _nog steeds_ gebruikt. Ook lege taak collecties als actief beschouwd en wordt in rekening gebracht.

## <a name="pricing"></a>Prijzen

Zie [Taakplanner prijzen](https://azure.microsoft.com/pricing/details/scheduler/)voor prijsdetails.

## <a name="see-also"></a>Zie ook


 [Wat is Taakplanner?](scheduler-intro.md)

 [Azure Scheduler concepten en terminologie entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met de planner in Azure portal](scheduler-get-started-portal.md)

 [Azure Scheduler REST API: naslag](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlets verwijzing](scheduler-powershell-reference.md)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Uitgaande verificatie van Azure Scheduler](scheduler-outbound-authentication.md)
