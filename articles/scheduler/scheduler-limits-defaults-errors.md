<properties
 pageTitle="Programmatoegang en -limieten Scheduler"
 description="Programmatoegang en -limieten Scheduler"
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

# <a name="scheduler-limits-and-defaults"></a>Programmatoegang en -limieten Scheduler

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Scheduler quota, maxima, standaardwaarden en Throttles

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>De Header x-ms-aanvraag-id

Elke aanvraag ten opzichte van de Scheduler-service retourneert een response-header met de naam**x-ms-aanvraag-id**. Deze header bevat een ondoorzichtige waarde die een unieke aanduiding voor de aanvraag.

Als een aanvraag voor een consequent niet goed werkt en u hebt gecontroleerd dat de aanvraag correct geformuleerd is, mag u deze waarde de fout melden aan Microsoft. In het rapport, inclusief de waarde van x-ms-aanvraag-id, de geschatte tijd die is aangevraagd, de identificatie van het abonnement, job-collectie, en/of functie en het soort bewerking dat het verzoek heeft geprobeerd.

## <a name="see-also"></a>Zie ook


 [Wat is Taakplanner?](scheduler-intro.md)

 [Azure Scheduler concepten en terminologie entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met de planner in Azure portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure planner](scheduler-plans-billing.md)

 [Azure Scheduler REST API: naslag](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlets verwijzing](scheduler-powershell-reference.md)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Uitgaande verificatie van Azure Scheduler](scheduler-outbound-authentication.md)
