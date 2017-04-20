<properties
   pageTitle="Stateful betrouwbare Services diagnostics | Microsoft Azure"
   description="Diagnostische functies voor Stateful betrouwbare diensten"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostische functies voor Stateful betrouwbare diensten
De klasse StatefulServiceBase voor betrouwbare diensten Stateful zendt [gebeurtenisbron](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen die kunnen worden gebruikt voor foutopsporing van de service, bieden inzicht in hoe de runtime is, en helpen bij het oplossen van problemen.

## <a name="eventsource-events"></a>Gebeurtenisbron gebeurtenissen
De naam van de gebeurtenisbron voor de klasse Stateful betrouwbare Services StatefulServiceBase is 'ServiceFabric-Microsoft-Services'. Gebeurtenissen van deze gebeurtenisbron weergegeven in het venster [Diagnostiek gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) wanneer de service [Foutopsporing in Visual Studio wordt](service-fabric-debugging-your-application.md).

Voorbeelden van hulpmiddelen en technologieën die helpen bij het inzamelen en/of gebeurtenisbron gebeurtenissen zijn [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md)en de [Bibliotheek van Microsoft TraceEvent](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Gebeurtenissen

|Naam van evenement|Gebeurtenis-ID|Niveau|Beschrijving van gebeurtenis|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Informatieve|Wanneer de service RunAsync taak wordt gestart.|
|StatefulRunAsyncCancellation|2|Informatieve|Wanneer de service RunAsync taak wordt geannuleerd|
|StatefulRunAsyncCompletion|3|Informatieve|Wanneer de service RunAsync taak is voltooid.|
|StatefulRunAsyncSlowCancellation|4|Waarschuwing|Wanneer het RunAsync servicetaak duurt te lang voor het voltooien van annulering|
|StatefulRunAsyncFailure|5|Fout|Wanneer het RunAsync servicetaak genereert een uitzondering|

## <a name="interpret-events"></a>Gebeurtenissen worden geïnterpreteerd.

StatefulRunAsyncInvocation, StatefulRunAsyncCompletion en StatefulRunAsyncCancellation-gebeurtenissen zijn handig voor de schrijvers te begrijpen van de levenscyclus van een service--als de timing voor wanneer een service wordt gestart, geannuleerd of voltooid. Dit kan handig zijn wanneer foutopsporing serviceproblemen of informatie over de levenscyclus van de service.

Schrijvers van de service moeten aandacht besteden aan de gebeurtenissen StatefulRunAsyncSlowCancellation en StatefulRunAsyncFailure omdat zij problemen met de service.

StatefulRunAsyncFailure wordt gegenereerd wanneer de servicetaak RunAsync() een uitzondering genereert. Meestal geeft een uitzondering veroorzaakt een fout of fout in de service. De uitzondering wordt bovendien de service niet, zodat deze wordt verplaatst naar een ander knooppunt. Dit is geen erg efficiënte bewerking en binnenkomende aanvragen kan worden vertraagd wanneer de service wordt verplaatst. Schrijvers van de service moeten de oorzaak van de uitzondering en deze, indien mogelijk, te verminderen.

StatefulRunAsyncSlowCancellation wordt gegenereerd wanneer een verzoek om annulering van de taak RunAsync langer dan vier seconden duurt. Wanneer een service te lang duurt voor het voltooien van de annulering, dit van invloed op de mogelijkheid om snel opnieuw worden gestart op een ander knooppunt. Dit kan invloed hebben op de algehele beschikbaarheid van de service.
