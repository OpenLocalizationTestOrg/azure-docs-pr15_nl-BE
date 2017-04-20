<properties
   pageTitle="Aan de slag met het distribueren en upgraden van apps op het lokale cluster | Microsoft Azure"
   description="Instellen met een lokale Service Fabric '-cluster, een bestaande toepassing te implementeren en vervolgens een upgrade die toepassing."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Aan de slag met het distribueren en upgraden van toepassingen op uw lokale cluster
De Azure Service Fabric SDK bevat een volledige lokale ontwikkelomgeving kunt u snel aan de slag met toepassingen op een lokale cluster implementeren en beheren. In dit artikel wordt een lokale cluster maken, een bestaande toepassing te implementeren en vervolgens een upgrade deze toepassing naar een nieuwe versie van Windows PowerShell.

> [AZURE.NOTE] In dit artikel wordt ervan uitgegaan dat u al [uw ontwikkelomgeving instellen](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Een lokale cluster maken
Een Service Fabric '-cluster vertegenwoordigt een set van hardware resources die u kunt toepassingen te implementeren. Normaal gesproken een cluster bestaat uit overal vijf tot duizenden machines. De Service Fabric SDK bevat echter een clusterconfiguratie die kan worden uitgevoerd op één computer.

Het is belangrijk te begrijpen dat het lokale cluster Service Fabric niet een emulator of simulator is. Dezelfde code platform die zich op meerdere machines clusters wordt uitgevoerd. Het enige verschil is dat het platform processen die normaal zijn verspreid over vijf machines op één computer wordt uitgevoerd.

De SDK biedt twee manieren om een cluster lokaal te stellen: een Windows PowerShell-script en het lokale Cluster Manager system tray app. In deze zelfstudie gebruiken we het PowerShell-script.

> [AZURE.NOTE] Als u een lokale cluster al door de implementatie van de toepassing in Visual Studio hebt gemaakt, kunt u deze sectie overslaan.


1. Een nieuw venster met PowerShell starten als administrator.

2. Het cluster setup-script uitvoeren vanuit de map SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Clusterinstallatie duurt enkele minuten. Nadat setup is voltooid, ziet u uitvoer die lijkt op:

    ![Cluster setup uitvoer][cluster-setup-success]

    U bent nu klaar om te proberen een toepassing aan het cluster implementeren.

## <a name="deploy-an-application"></a>Een toepassing
De Service Fabric-SDK bevat een groot aantal kaders en ontwikkelaar gereedschap voor het maken van toepassingen. Als u wilt weten hoe u toepassingen maken in Visual Studio, Zie [maken uw eerste Service Fabric-toepassing in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

In deze zelfstudie gebruiken we een bestaande voorbeeldtoepassing (WordCount genoemd) zodat we op de aspecten van het beheer van het platform richten kunnen, met inbegrip van de implementatie, bewaking en upgrade.


1. Een nieuw venster met PowerShell starten als administrator.

2. Importeer de module Service Fabric SDK PowerShell.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Maak een map voor het opslaan van de toepassing die u downloaden en installeren, zoals C:\ServiceFabric.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [De toepassing WordCount downloaden](http://aka.ms/servicefabric-wordcountapp) naar de locatie gemaakt.  Opmerking: de Microsoft Edge browser het bestand opgeslagen met de extensie *.zip* .  De bestandsextensie wijzigen in *.sfpkg*.

5. Verbinding maken met het lokale cluster:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Maak een nieuwe toepassing met de SDK van implementatie-opdracht met een naam en een pad naar het toepassingspakket.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Als alles goed gaat, ziet u de volgende uitvoer:

    ![Een toepassing met het lokale cluster implementeren][deploy-app-to-local-cluster]

7. Open de browser en Ga naar [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html)overzicht van de toepassing in de praktijk. U ziet:

    ![Gedistribueerde toepassing UI][deployed-app-ui]

    De toepassing WordCount is erg eenvoudig. Het bevat client-side JavaScript-code voor het genereren van willekeurige vijf tekens 'woorden', die vervolgens worden doorgegeven aan de toepassing via de API voor ASP.NET-webpagina. Een stateful-service houdt het aantal woorden geteld. Ze worden gepartitioneerd op basis van het eerste teken van het woord. U vindt de broncode voor de WordCount app in de [monsters aan de slag](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/).

    De toepassing die we hebben geïmplementeerd, bevat vier partities. Zodat woorden die beginnen met A tot en met G worden opgeslagen in de eerste partitie, worden woorden die beginnen met H tot en met N in de tweede partitie worden opgeslagen, enzovoort.

## <a name="view-application-details-and-status"></a>Details van toepassing en status
Nu dat we de toepassing hebt geïmplementeerd, Bekijk enkele van de details van app in PowerShell.

1. Alle gebruikte toepassingen in het cluster een query:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Ervan uitgaande dat u alleen de app WordCount hebt geïmplementeerd, ziet er ongeveer uitzien als:

    ![Alle gebruikte toepassingen in PowerShell opvragen][ps-getsfapp]

2. Ga naar het volgende niveau via query's in de set services die zijn opgenomen in de toepassing WordCount.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Lijst van diensten voor de toepassing in PowerShell][ps-getsfsvc]

    De toepassing bestaat uit twee services--front-end voor het web en de stateful-service de woorden beheert.

3. Tot slot, kijk eens naar de lijst met partities voor WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Bekijk de partities service in PowerShell][ps-getsfpartitions]

    De lijst met opdrachten die u, net als alle Service Fabric PowerShell-opdrachten gebruikt zijn beschikbaar voor elk cluster dat u kunt verbinding met lokale of externe.

    Voor een meer visuele manier interactief werken met het cluster, kunt u het hulpprogramma web gebaseerde Service Fabric Explorer door te navigeren naar [http://localhost:19080/Explorer](http://localhost:19080/Explorer) in de browser.

    ![Details van toepassing in Service Fabric Explorer][sfx-service-overview]

    > [AZURE.NOTE] Voor meer informatie over Service Fabric Explorer, Zie [cluster Service Fabric Explorer visualiseren](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Upgrades uitvoeren op toepassingen
Fabric-service biedt geen uitvaltijd upgrades door toezicht op de gezondheid van de toepassing deze rolt uit in het cluster. We een eenvoudige upgrade uitvoeren van de toepassing WordCount.

De nieuwe versie van de toepassing nu telt alleen woorden die met een klinker beginnen. Als de upgrade uitgevoerd wordt, zien we twee wijzigingen in de werking van de toepassing. Ten eerste moet de snelheid waarmee het aantal groeit vertragen, omdat er minder woorden geteld. Ten tweede, omdat de eerste partitie twee klinkers (A en E heeft) en alle andere partities slechts één elk bevatten, het aantal uiteindelijk begint was de andere.

1. [Download het pakket WordCount v2](http://aka.ms/servicefabric-wordcountappv2) op dezelfde locatie waar u het pakket v1 gedownload.

2. Terug naar het venster PowerShell en upgrade van de SDK-opdracht met de nieuwe versie te registreren in het cluster. Vervolgens begint met het upgraden van het weefsel: / WordCount toepassing.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Hier ziet u uitvoer in PowerShell die ziet er ongeveer zo uitziet als de upgrade begint.

    ![Voortgang van de upgrade in PowerShell][ps-appupgradeprogress]

3. Tijdens de upgrade voortgezet wordt, kan het handiger zijn om te controleren van de status van Service Fabric Explorer. Start een browser-venster en Ga naar [http://localhost:19080/Explorer](http://localhost:19080/Explorer). **Toepassingen** in de boomstructuur aan de linkerkant uitbreiden en kies vervolgens **WordCount**, en ten slotte **fabric: / WordCount**. In het tabblad essentials ziet u de status van de upgrade als het doorlopen van het cluster upgraden domeinen.

    ![Voortgang in Service Fabric Explorer bijwerken][sfx-upgradeprogress]

    Als de upgrade via elk domein verloopt, worden uitgevoerd om ervoor te zorgen dat de toepassing goed gedrag.

4. Als u de query opnieuw eerder voor de set van diensten in het weefsel uitvoeren: / WordCount toepassing, u ziet dat de versie van WordCountService gewijzigd, maar de versie van WordCountWebService niet het geval:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Toepassingsservices query na de upgrade][ps-getsfsvc-postupgrade]

    Dat onderstreept hoe Fabric-Service beheert toepassingsupgrades. Het raakt alleen de set met services (of code/configuratie pakketten binnen die services) die zijn gewijzigd, waardoor het proces van het bijwerken sneller en betrouwbaarder.

5. Ten slotte terug naar de browser om het observeren van het gedrag van de nieuwe versie van de toepassing. Zoals verwacht, het aantal trager verloopt, en wordt de eerste partitie met iets meer van het volume.

    ![De nieuwe versie van de toepassing in de browser weergeven][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Opruimen

Voordat de onmiddellijke verpakking van, is het belangrijk te weten dat het lokale cluster real. Toepassingen blijven op de achtergrond uitgevoerd totdat u ze verwijdert.  Afhankelijk van de aard van uw toepassingen kan een actieve app nemen veel bronnen op uw computer. U hebt verschillende opties voor het beheren van toepassingen en het cluster:

1. U verwijdert een afzonderlijke toepassing en alle bijbehorende gegevens, het volgende uitvoeren:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Of de toepassing verwijderen uit het menu **Acties** van Service Fabric Explorer of het contextmenu in de toepassing van het deelvenster aan de linkerkant.

    ![Een toepassing verwijderen is Service Fabric Explorer][sfe-delete-application]

2. Nadat de toepassing is verwijderd uit het cluster, kunt u de registratie van versie 1.0.0 en 2.0.0 van het toepassingstype dat WordCount. Verwijderen verwijdert u de toepassingspakketten, met inbegrip van de code en de configuratie van het archief van de installatiekopie van het cluster.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Of kies Service Fabric Explorer **Type inrichting verwijderen** voor de toepassing.

3. Als u wilt afsluiten van het cluster, maar houd de toepassingsgegevens en sporen, klikt u op **Stoppen met het lokale Cluster** in de system tray app.

4. Als het cluster volledig wilt verwijderen, klikt u op **Lokale Cluster verwijderen** in de system tray app. Deze optie leidt tot een andere trage implementatie van de volgende keer dat u op F5 drukt in Visual Studio. Verwijder het lokale cluster alleen als u niet wilt gebruiken voor een bepaalde tijd of als u nodig hebt om bronnen vrij te maken.

## <a name="1-node-and-5-node-cluster-mode"></a>Knooppunt 1 en 5 knooppunt clustermodus

Als u werkt met het lokale cluster om toepassingen te ontwikkelen, u vaak merkt dat snelle herhalingen van het schrijven van code, debuggen, code, wijzigen doet foutopsporing enz. Als u wilt dit proces te optimaliseren, het lokale cluster kan worden uitgevoerd in twee modi: knooppunt 1 of knooppunt 5. Beide modi cluster hebben hun voordelen.
Clustermodus voor 5 knooppunt kunt u werken met een echte cluster. U kunt testen failover-scenario's, werken met meer exemplaren en replica's van uw diensten.
1 knooppunt clustermodus is geoptimaliseerd voor snelle distributie en registratie van services, waarmee u snel code met behulp van de Service Fabric runtime te valideren.

Zowel knooppunt 1 cluster-modus en 5 knooppunt clustermodus is niet een emulator of simulator. Dezelfde code platform die zich op meerdere machines clusters wordt uitgevoerd.

> [AZURE.NOTE] Deze functie is beschikbaar in de SDK versie 5.2 of hoger.

Als u een cluster met 1 knooppunt de clustermodus, Service Fabric lokale Cluster Manager gebruiken of gebruikt u PowerShell de volgende manier:

1. Een nieuw venster met PowerShell starten als administrator.

2. Het cluster setup-script uitvoeren vanuit de map SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Clusterinstallatie duurt enkele minuten. Nadat setup is voltooid, ziet u uitvoer die lijkt op:
    
    ![Cluster setup uitvoer][cluster-setup-success-1-node]

Als u de Service Fabric lokale Cluster Manager:

![Clustermodus switch][switch-cluster-mode]

> [AZURE.WARNING] Bij het wijzigen van clustermodus, het huidige cluster wordt verwijderd uit uw systeem en een nieuw cluster wordt gemaakt. De gegevens die in het cluster, moet zijn opgeslagen worden verwijderd wanneer u de clustermodus wijzigen.

## <a name="next-steps"></a>Volgende stappen
- Nu dat u hebt geïmplementeerd en sommige vooraf ontworpen toepassingen bijgewerkt, kunt u [proberen het bouwen van uw eigen in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
- Alle acties die worden uitgevoerd op het lokale cluster in dit artikel kunnen worden uitgevoerd op een [cluster met Azure](service-fabric-cluster-creation-via-portal.md) .
- De upgrade die we in dit artikel hebt uitgevoerd, is basic. Zie de [documentatie bijwerken](service-fabric-application-upgrade.md) voor meer informatie over de kracht en flexibiliteit van Fabric Service-upgrades.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
