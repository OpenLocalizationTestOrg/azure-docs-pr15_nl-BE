<properties
   pageTitle="Problemen oplossen met gebeurtenistracering | Microsoft Azure"
   description="De meest voorkomende problemen bij het implementeren van services op Microsoft Azure Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Algemene problemen bij het implementeren van services op Azure Service Fabric

Wanneer u services op uw computer developer uitvoert, is eenvoudig te gebruiken [Hulpmiddelen voor foutopsporing van Visual Studio](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). [Statusrapporten](service-fabric-view-entities-aggregated-health.md) zijn voor externe clusters, altijd een goede plaats om te starten. De eenvoudigste manieren om toegang tot deze rapporten zijn via PowerShell of [SFX](service-fabric-visualizing-your-cluster.md). In dit artikel wordt ervan uitgegaan dat u fouten in een extern cluster opsporen wilt en hebben een basiskennis van het gebruik van een van deze hulpprogramma's.

##<a name="application-crash"></a>De toepassing is vastgelopen
De "partitie kleiner is dan aantal replica of het exemplaar als doel" rapport is een goede indicatie dat uw service vastloopt. Als u wilt weten duurt waar uw service vastloopt een beetje meer onderzoek. Wanneer de service wordt uitgevoerd op de schaal, worden je beste vriend een reeks goed doordacht sporen.  Wij stellen voor dat u [Azure diagnostische gegevens probeert](service-fabric-diagnostics-how-to-setup-wad.md) voor het verzamelen van deze sporen en met behulp van een oplossing zoals [Elastische zoeken](service-fabric-diagnostic-how-to-use-elasticsearch.md) voor het weergeven en zoeken naar de sporen.

![SFX partitie gezondheid](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Tijdens het initialiseren van de service of actor
Eventuele uitzonderingen voordat het servicetype wordt geïnitialiseerd, wordt het proces vast. Voor deze typen vastloopt, wordt het logboek voor toepassingsgebeurtenissen de fout in de service weergegeven.
Dit zijn de meest voorkomende uitzonderingen te zien voordat de service wordt geïnitialiseerd.

***System.IO.FileNotFoundException***

Deze fout wordt vaak veroorzaakt door een ontbrekende assembly-afhankelijkheden. Controleer de eigenschap CopyLocal in Visual Studio of in de globale assembly-cache voor het knooppunt.

***System.Runtime.InteropServices.COMException***
 *bij System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 Hiermee wordt aangegeven dat de naam van het geregistreerde service komt niet overeen met het servicemanifest.

[Azure diagnostische gegevens](service-fabric-diagnostics-how-to-setup-wad.md) kan worden geconfigureerd om het logboek voor toepassingsgebeurtenissen voor alle knooppunten in het automatisch uploaden.

###<a name="runasync-or-onactivateasync"></a>RunAsync() of OnActivateAsync()
Als het vastlopen tijdens de initialisatie gebeurt of van het geregistreerde type of actor wordt uitgevoerd, de uitzondering wordt opgespoord door Azure Service Fabric. U kunt bekijken uit de gebeurtenisbron providers beschreven in de sectie 'Volgende stappen'.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over bestaande diagnostische gegevens verstrekt door Fabric-Service:

* [Betrouwbare diagnostische gegevens van Actors](service-fabric-reliable-actors-diagnostics.md)
* [Betrouwbare diagnostische gegevens van Services](service-fabric-reliable-services-diagnostics.md)
