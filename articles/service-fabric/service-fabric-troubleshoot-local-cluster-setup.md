<properties
   pageTitle="Problemen met uw lokale Service cluster fabrieksinstelling | Microsoft Azure"
   description="In dit artikel komen een aantal suggesties voor het oplossen van het cluster voor plaatselijke ontwikkeling"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Problemen met uw plaatselijke ontwikkeling clusterinstallatie

Als u een probleem tijdens de interactie met uw lokale Azure Service weefsel ontwikkeling cluster, bekijk de volgende suggesties voor mogelijke oplossingen.

## <a name="cluster-setup-failures"></a>Cluster setup-fouten

### <a name="cannot-clean-up-service-fabric-logs"></a>Logboeken Fabric-Service niet kan opschonen

#### <a name="problem"></a>Probleem

Tijdens het uitvoeren van het script DevClusterSetup, ziet u een fout als volgt:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Oplossing

Het huidige PowerShell-venster sluit en opent een nieuw venster PowerShell als beheerder. Nu moet u kunt het script uitvoeren.

## <a name="cluster-connection-failures"></a>Cluster-verbindingsfouten

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Service Fabric PowerShell cmdlets worden niet herkend in Azure PowerShell

#### <a name="problem"></a>Probleem

Als u probeert uit te voeren van de Service Fabric PowerShell-cmdlets, zoals `Connect-ServiceFabricCluster` in een venster Azure PowerShell mislukt, zeggen de cmdlet wordt niet herkend. De reden hiervoor is dat Azure PowerShell wordt gebruikt voor de 32-bits versie van Windows PowerShell (zelfs op 64-bits besturingssysteemversies), terwijl de Service Fabric-cmdlets alleen in 64-bits omgevingen werken.

#### <a name="solution"></a>Oplossing

Service Fabric cmdlets rechtstreeks vanuit Windows PowerShell worden altijd uitgevoerd.

>[AZURE.NOTE] De nieuwste versie van Azure PowerShell maakt een speciale snelkoppeling, zodat dit niet meer gebeurt.

### <a name="type-initialization-exception"></a>Type initialisatie uitzondering

#### <a name="problem"></a>Probleem

Wanneer u verbinding met het cluster in PowerShell, ziet u de fout TypeInitializationException voor System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Oplossing

De variabele path is niet juist ingesteld tijdens de installatie. Meld u af bij Windows en opnieuw aanmelden. Hiermee wordt het pad volledig vernieuwd.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Cluster verbinding mislukt met "-Object is gesloten"

#### <a name="problem"></a>Probleem

Een aanroep naar de Connect-ServiceFabricCluster mislukt met fout als volgt:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Oplossing

Het huidige PowerShell-venster sluit en opent een nieuw venster PowerShell als beheerder. U moet nu verbinding kunnen zijn.

### <a name="fabric-connection-denied-exception"></a>Verbinding geweigerd fabric-uitzondering

#### <a name="problem"></a>Probleem

Wanneer foutopsporing van Visual Studio, krijgt u een FabricConnectionDeniedException-fout.

#### <a name="solution"></a>Oplossing

Deze fout treedt meestal op wanneer u probeer te proberen te starten van een ServiceHost handmatig verwerkt in plaats van de runtime Fabric-Service u wilt starten.

Zorg ervoor dat er geen serviceprojecten als projecten opstarten in de oplossing. Alleen projecten van Service Fabric-toepassing moeten worden ingesteld als projecten opstarten.

>[AZURE.TIP] Als u na de installatie, het lokale cluster zich abnormaal gedraagt begint, kunt u met behulp van het lokale cluster manager-lade toepassing herstellen. Dit wordt het cluster te verwijderen en een nieuwe instellen. Houd er rekening mee dat alle toepassingen hebt geïmplementeerd en de bijbehorende gegevens worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

- [Begrijpen en oplossen van problemen met het cluster met statusrapporten systeem](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [De cluster-Service Fabric Explorer visualiseren](service-fabric-visualizing-your-cluster.md)
