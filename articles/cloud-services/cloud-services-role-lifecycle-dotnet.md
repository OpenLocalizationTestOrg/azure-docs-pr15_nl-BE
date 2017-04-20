<properties 
pageTitle="Cloud Service lifecycle gebeurtenissen afhandelen | Microsoft Azure" 
description="Meer informatie over hoe de methoden van de levenscyclus van de rol van een Cloud-Service kunnen worden gebruikt in .NET" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>Aanpassen van de levenscyclus van een webpagina of werknemer rol in .NET

Wanneer u een rol werknemer maakt, kunt u de klasse [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) biedt methoden voor u om te negeren waarmee u kunt reageren op gebeurtenissen die lifecycle uitbreiden. Deze klasse is voor web-rollen optioneel, zodat u deze gebruiken moet om te reageren op gebeurtenissen lifecycle.

## <a name="extend-the-roleentrypoint-class"></a>De klasse RoleEntryPoint uitbreiden

De klasse [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) bevat methoden die worden aangeroepen door Azure wanneer deze **wordt gestart**, **wordt uitgevoerd**of **reageert** de rol van een web- of werknemer. U kunt deze methoden voor het beheer van de initialisatie van de rol, rol afgesloten reeksen of de uitvoeringsthread van de rol desgewenst negeren. 

Wanneer u **RoleEntryPoint**uitbreidt, moet u zich bewust zijn van de volgende problemen van de methoden:

-   De [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) en [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) retourneert een boolean-waarde als resultaat **false** van deze methoden mogelijk is.

     Als uw code **false retourneert**, wordt het proces van de rol abrupt beëindigd, zonder de afsluitprocedure op zijn plaats wellicht. In het algemeen moet u niet van de methode **OnStart** **false** retourneert.
     
-   Een niet-afgevangen uitzondering binnen een overbelasting van een methode **RoleEntryPoint** wordt behandeld als een niet-afgehandelde uitzondering.

     Azure de gebeurtenis [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) wordt er een uitzondering voordoet in een van de methoden van de levenscyclus, en vervolgens het proces wordt beëindigd. Nadat uw rol offline is genomen, wordt deze opnieuw gestart door Azure. De gebeurtenis [stoppen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) wordt niet geactiveerd wanneer een niet-verwerkte uitzondering optreedt, en niet de methode **OnStop** wordt aangeroepen.

Als de rol niet gestart of tussen de bij het initialiseren, bezet, en de lidstaten stoppen is recycling, kan de code een onverwerkte uitzondering binnen een van de levenscyclus van gebeurtenissen opnieuw telkens wanneer de rol gestart genereren. In dit geval de gebeurtenis [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) gebruiken om de oorzaak van de uitzondering en goed overweg. Uw rol kan ook worden geretourneerd uit de methode [Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , waardoor de rol op te starten. Zie voor meer informatie over de implementatie van lidstaten, [Gemeenschappelijke problemen die oorzaak rollen hergebruiken](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Als u de **Azure Tools for Microsoft Visual Studio** voor het ontwikkelen van uw toepassing, uitbreiden de rol van project-sjablonen de klasse **RoleEntryPoint** , automatisch in de bestanden *WebRole.cs* en *WorkerRole.cs* .

## <a name="onstart-method"></a>OnStart, methode

De methode **OnStart** wordt aangeroepen wanneer een exemplaar van uw rol door Azure on line wordt gebracht. Terwijl de code OnStart wordt uitgevoerd, het exemplaar van de rol is gemarkeerd als **bezet** en geen extern verkeer wordt doorgestuurd naar het door de verdeling van de belasting. U kunt deze methode voor het uitvoeren van de initialisatie werk, zoals de implementatie van gebeurtenis-handlers en [Azure Diagnostics](cloud-services-how-to-monitor.md)starten overschrijven.

Als **OnStart** **true**retourneert, wordt de sessie is geïnitialiseerd en Azure roept de methode **RoleEntryPoint.Run** . Als **OnStart** **false**retourneert, wordt de rol direct, beëindigt zonder het uitvoeren van een geplande afsluiting reeksen.

In het volgende voorbeeld ziet u hoe de methode **OnStart** negeren. Deze methode wordt geconfigureerd en een diagnostisch bewakingssysteem wordt gestart wanneer het exemplaar van de rol wordt gestart en een overdracht van de logboekgegevens naar een opslag ingesteld:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop, methode

De methode **OnStop** wordt aangeroepen nadat een instantie van de rol heeft genomen offline Azure en voordat het proces wordt afgesloten. U kunt negeren deze methode aanroepen van code die nodig is voor uw exemplaar van de functie correct af te sluiten.

> [AZURE.IMPORTANT] Code die wordt uitgevoerd in de **OnStop** -methode heeft een beperkte tijd te voltooien wanneer deze wordt aangeroepen om andere redenen dan een gebruiker aangestuurde afsluiting. Nadat deze tijd is verstreken, wordt het proces wordt beëindigd, dus moet u ervoor zorgen dat code in de methode **OnStop** kunnen snel worden uitgevoerd of niet volledig met maximaal wordt toegestaan. De methode **OnStop** wordt aangeroepen nadat de gebeurtenis **stoppen** .


## <a name="run-method"></a>De methode Run

U kunt de methode **Run** implementatie van een langdurige thread voor uw exemplaar van de functie negeren.

De methode **uitvoeren** is niet verplicht. de standaardimplementatie wordt een thread die altijd in slaapstand gaat. Als u de methode **Run** overschrijven, moet de code voor onbepaalde tijd blokkeren. Als de methode **Run** geeft, wordt de rol automatisch worden herhaald; met andere woorden, Azure activeert de gebeurtenis **stoppen** en de methode **OnStop** aangeroepen zodat uw reeksen afsluiten kunnen worden uitgevoerd voordat de functie Off line wordt genomen.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Uitvoering van de levenscyclus van ASP.NET-methoden voor de rol van een webpagina

Initialisatie en afgesloten reeksen voor de rol van een website beheren kunt u de ASP.NET-lifecycle methoden, naast die welke door de klasse **RoleEntryPoint** . Dit mogelijk handig voor compatibiliteit van toepassing als u een bestaand ASP.NET-toepassing naar Azure worden overdragen. De levenscyclus van ASP.NET methoden worden aangeroepen vanuit de methoden **RoleEntryPoint** . De **toepassing\_Start** methode wordt aangeroepen nadat de methode **RoleEntryPoint.OnStart** is voltooid. De **toepassing\_End** methode wordt aangeroepen voordat de methode **RoleEntryPoint.OnStop** wordt aangeroepen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [maken van een cloud service-pakket](cloud-services-model-and-package.md).