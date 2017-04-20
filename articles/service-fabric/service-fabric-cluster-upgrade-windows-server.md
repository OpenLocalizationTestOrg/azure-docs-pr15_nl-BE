<properties
   pageTitle="Een zelfstandige Service Fabric '-cluster op Windows Server upgrade | Microsoft Azure"
   description="Service Fabric code en/of configuratie die wordt uitgevoerd een zelfstandige Service Fabric '-cluster, inclusief het instellen van clustermodus update upgrade"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Upgrade de zelfstandige Service Fabric '-cluster op Windows Server

> [AZURE.SELECTOR]
- [Azure Cluster](service-fabric-cluster-upgrade.md)
- [Autonome Cluster](service-fabric-cluster-upgrade-windows-server.md)

Voor alle moderne systemen is ontwerpen voor kunnen de sleutel tot het succes van uw product op lange termijn te bereiken. Een cluster Service Fabric is een bron die u bezit. In dit artikel wordt beschreven hoe u kunt ervoor zorgen dat het cluster altijd wordt uitgevoerd ondersteunde versies van de service-structuur en de configuraties.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>De fabric-versie die wordt uitgevoerd op een Cluster beheren

U kunt instellen dat uw cluster service fabric updates downloaden wanneer Microsoft een nieuwe versie is vrijgegeven of kies een gewenste cluster op ondersteunde fabric-versie te selecteren. 

Dit doet u door de configuratie van de cluster 'fabricClusterAutoupgradeEnabled' in te stellen op true of false.


>[AZURE.NOTE] Zorg ervoor dat het cluster altijd met een ondersteunde versie van de Service Fabric. Als en wanneer we de release van een nieuwe versie van de service fabric aankondigen, wordt de vorige versie gemarkeerd voor einde van ondersteuning na een minimum van 60 dagen na die datum. De nieuwe releases worden aangekondigd [op de blog service fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/ ). De nieuwe release is beschikbaar voor vervolgens kiezen. 


U kunt uw cluster upgraden naar de nieuwe versie, alleen als u een knooppuntconfiguratie productie-stijl waarbij elk knooppunt Fabric-Service op een afzonderlijke fysieke of virtuele machine is toegewezen. Als u een cluster ontwikkelen wanneer er meer dan één service fabric-knooppunten op een enkele fysieke of virtuele machine, moet u het cluster verwijderen en opnieuw te maken met de nieuwe versie.


Er zijn twee verschillende workflows voor uw cluster upgraden naar de nieuwste of een ondersteunde service fabric-versie. Voor clusters die verbinding automatisch de nieuwste versie downloaden en de tweede voor clusters die helemaal geen verbinding met de meest recente versie van de Service-Fabric.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Upgraden van clusters met connectiviteit voor het downloaden van de nieuwste code en configuratie 

Voer deze stappen uit om het cluster een upgrade naar een ondersteunde versie als de clusterknooppunten internetverbinding met [http://download.microsoft.com hebt](http://download.microsoft.com) 

Voor clusters met verbindingen met [http://download.microsoft.com](http://download.microsoft.com), controleren we regelmatig voor de beschikbaarheid van nieuwe versies van de service fabric.


Wanneer een nieuwe service fabric-versie beschikbaar is, wordt het pakket gedownload lokaal aan het cluster en ingericht voor de upgrade. Ook als de klant informeren over deze nieuwe versie, plaatst het systeem een expliciete cluster gezondheid waarschuwing met de volgende strekking:

"De huidige cluster versie [versienummer] [datum] ondersteuning wordt beëindigd.", als het cluster actief is de meest recente versie, de waarschuwing is verdwenen.


#### <a name="cluster-upgrade-workflow"></a>Cluster Upgrade workflow.
 
Nadat u het cluster gezondheid waarschuwing ziet, moet u het volgende doen:

1. Verbinding maken met het cluster vanaf een willekeurige computer met beheerderstoegang tot de computers die worden weergegeven als de knooppunten in het cluster. De machine die op dit script wordt uitgevoerd geen deel uitmaken van het cluster

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. De lijst van service fabric versies die u naar upgraden kunt ophalen

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    ontvangt u een output ongeveer als volgt uit:

    ![fabric versies ophalen][getfabversions]

3. Ere van upgraden van een cluster op een van de versies die beschikbaar is via de [PowerShell Start ServiceFabricClusterUpgrade cmd](https://msdn.microsoft.com/library/mt125872.aspx)

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
U kunt de voortgang van de upgrade op Service fabric explorer of door het uitvoeren van de volgende power shell-opdracht controleren

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nadat de problemen die in het terugdraaien resulteerde is verholpen, moet u de upgrade opnieuw starten door de stappen hierboven te volgen.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Upgraden van clusters met <U>geen verbinding</u> voor het downloaden van de nieuwste code en configuratie

Voer deze stappen uit om het cluster een upgrade naar een ondersteunde versie als cluster knooppunten **hebben geen** verbinding met het internet op [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Als u werkt met een cluster dat is niet verbonden met internet, moet u controleren de blog service fabric team op de hoogte worden gebracht van een nieuwe release. Het systeem **niet het** plaats cluster gezondheid waarschuwing waarschuwingen ontvangen over deze.  

1. Wijzig de configuratie van het cluster om de volgende eigenschap ingesteld op false.

        "fabricClusterAutoupgradeEnabled": false,

en ere van een upgrade van de configuratie. [cmd Start ServiceFabricClusterUpgrade PS](https://msdn.microsoft.com/library/mt125872.aspx) Zie voor details van gebruik. De cluster manifest versie is de versie die u in de clusterConfig.JSON hebt. Zorg ervoor dat deze bij te werken voordat u verwijderen uit de configuratie-upgrade.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Cluster Upgrade workflow.
 


1. Download de nieuwste versie van het pakket van document [maken service fabric '-cluster voor windows server](service-fabric-cluster-creation-for-windows-server.md) 


1. Verbinding maken met het cluster vanaf een willekeurige computer met beheerderstoegang tot de computers die worden weergegeven als de knooppunten in het cluster. De machine die op dit script wordt uitgevoerd geen deel uitmaken van het cluster 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Kopieer het gedownloade pakket in het cluster afbeelding archief.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Registreer de gekopieerde pakket 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Ere van upgraden van een cluster met een van de versies die beschikbaar is. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
U kunt de voortgang van de upgrade op Service fabric explorer of door het uitvoeren van de volgende power shell-opdracht controleren

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nadat de problemen die in het terugdraaien resulteerde is verholpen, moet u de upgrade opnieuw starten door de stappen hierboven te volgen.



## <a name="next-steps"></a>Volgende stappen
- Informatie over het aanpassen van sommige van de [instellingen van fabric fabric cluster-service](service-fabric-cluster-fabric-settings.md)
- Meer informatie over het [schalen van het cluster in-en uitchecken](service-fabric-cluster-scale-up-down.md)
- Meer informatie over [toepassingsupgrades](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
