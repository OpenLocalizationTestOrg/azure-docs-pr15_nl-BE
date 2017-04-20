<properties
    pageTitle="Overzicht van de diagnostische gegevens van Azure | Microsoft Azure"
    description="Azure diagnostische gegevens te gebruiken voor foutopsporing, het meten van de prestaties, controle, analyse van het netwerkverkeer in de cloud-services, virtuele machines en service fabric"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Wat is Microsoft Azure diagnostische gegevens


Azure diagnostische gegevens bestaat de mogelijkheid in Azure waarmee het verzamelen van diagnostische gegevens op een gedistribueerde toepassing. U kunt de uitbreiding van de diagnostische gegevens uit een aantal verschillende bronnen. Momenteel ondersteund zijn Azure Cloud Service Web en rollen van de werknemer, Azure virtuele Machines met Microsoft Windows en Fabric-Service. Andere Azure diensten hebben hun eigen afzonderlijke diagnostische gegevens.

## <a name="data-you-can-collect"></a>U kunt verzamelen gegevens

Azure diagnostische gegevens kunnen de volgende soorten gegevens verzamelen:

Gegevensbron|Beschrijving
---|---
Prestatiemeteritems | Besturingssysteem en aangepaste prestatiemeters
Toepassingslogboeken     | Geschreven door uw toepassing berichten traceren
Windows-gebeurtenislogboeken   | Informatie verzonden naar het Windows-systeem gebeurtenis vastleggen
Gebeurtenisbron .NET    | Code schrijven met behulp van de klasse .NET [gebeurtenisbron](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen
IIS-logboeken             | Informatie over IIS-websites
Het manifest op basis van ETW   | Gebeurtenis Tracing voor Windows-gebeurtenissen gegenereerd door een proces.
Crashdumps          | Informatie over de status van het proces in geval van een crash van een toepassing
Aangepaste foutenlogboeken    | Logboeken gemaakt door de toepassing of service
Infrastructuur voor Azure diagnostische logboeken|Informatie over diagnostische gegevens zelf

De extensie Azure diagnostische gegevens kan deze gegevens overbrengen naar een opslag Azure-account of verzenden naar services, zoals de [Inzichten van toepassing](./application-insights/app-insights-cloudservices.md). U kunt de gegevens voor foutopsporing en probleemoplossing, meten van de prestaties, Resourcegebruik, analyse van het netwerkverkeer en capaciteitsplanning monitoring en controle.


## <a name="versioning"></a>Versiebeheer
Zie [Diagnostische gegevens van Azure Versioning geschiedenis](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Volgende stappen
Kies welke service u wilt verzamelen van diagnostische gegevens op en gebruikt u de volgende artikelen aan de slag. Gebruik de koppelingen algemeen Azure diagnostics voor verwijzing voor specifieke taken.

## <a name="web-apps"></a>Web Apps
Houd er rekening mee dat Web Apps Azure diagnostische gegevens niet gebruiken. Vergelijkbare informatie vinden op het [Web Apps](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Met diagnostische gegevens Azure cloud-Services
- Zie [Visual Studio gebruiken om een Cloud Services-toepassing](./vs-azure-tools-debug-cloud-services-virtual-machines.md) aan de slag als Visual Studio. Anders zien
- [Met diagnostische gegevens Azure Cloud-services controleren](./cloud-services/cloud-services-how-to-monitor.md)
- [Azure diagnostische gegevens in een Cloud Services-toepassing instellen](./cloud-services/cloud-services-dotnet-diagnostics.md)

Zie voor meer geavanceerde onderwerpen

- [Azure diagnostische gegevens gebruiken met inzichten van toepassing voor Cloud Services](./application-insights/app-insights-cloudservices.md)
- [De stroom van een Cloud Services-toepassing met Azure diagnostische gegevens traceren](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [PowerShell gebruiken voor het instellen van diagnostische gegevens op Cloud Services](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuele Machines met Azure diagnostische gegevens
- Zie [Visual Studio gebruiken om te kunnen traceren Azure virtuele Machines](./vs-azure-tools-debug-cloud-services-virtual-machines.md) aan de slag als Visual Studio. Anders zien
- [Azure diagnostische gegevens op een Azure Virtual Machine instellen](./virtual-machines-dotnet-diagnostics.md)

Zie voor meer geavanceerde onderwerpen

- [PowerShell gebruiken om diagnostische Azure virtuele Machines in te stellen](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Een virtuele Windows-machine met bewaking en diagnostiek met Azure Resource Manager-sjabloon maken](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Met diagnostische gegevens Azure Service-Fabric
Aan de slag op de [Monitor een Service Fabric-toepassing](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Als u eenmaal voor dit artikel zijn veel artikelen van andere Service Fabric diagnostische gegevens beschikbaar in de boomstructuur aan de linkerkant.

## <a name="general-azure-diagnostics-articles"></a>Algemene Azure Diagnostics artikelen
- [Azure diagnostische gegevens van het Schema, configuratie](https://msdn.microsoft.com/library/azure/mt634524.aspx) - informatie over het wijzigen van het schemabestand route diagnostische gegevens te verzamelen. Opmerking u kunt ook Visual Studio gebruiken om te wijzigen van het schemabestand.
- [Hoe Azure diagnostische gegevens worden opgeslagen in Azure opslag](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - weet de namen van de tabellen en BLOB's waar de diagnostische gegevens worden geschreven.
- Leer hoe [u de prestatiemeteritems in Azure diagnostische gegevens](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Informatie over [diagnostische informatie Azure Route naar inzichten van toepassing](./azure-diagnostics-configure-applicationinsights.md)
- Als u problemen met diagnostische gegevens starten of het vinden van uw gegevens in tabellen Azure opslag ondervindt, Zie [Probleemoplossing Azure diagnostische gegevens](./azure-diagnostics-troubleshooting.md)
