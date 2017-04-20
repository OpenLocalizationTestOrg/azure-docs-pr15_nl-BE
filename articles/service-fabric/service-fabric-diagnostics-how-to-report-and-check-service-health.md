<properties
   pageTitle="Rapporteren en controleren met Azure Service Fabric | Microsoft Azure"
   description="Informatie over het statusrapporten verzenden vanuit uw servicecode en de gezondheid van uw service te controleren met behulp van de controleprogramma's van gezondheid waarmee Azure Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Melden en controleren of de gezondheid van service
Wanneer uw services problemen ondervindt, is de mogelijkheid te reageren op incidenten en storingen verhelpen afhankelijk van de mogelijkheid om de problemen snel detecteren. Als u problemen en fouten aan de gezondheid Azure Service Fabric manager vanuit uw servicecode, kunt u standaard controlehulpprogramma's waarmee Fabric-Service om te controleren van de status van de gezondheid.

Er zijn twee manieren dat u de gezondheid van de service kunt melden:

- [Partitie](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) of [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) gebruiken.  
U kunt de `Partition` en `CodePackageActivationContext` objecten voor aangifte van de gezondheid van de elementen die deel van de huidige context uitmaken. Code die wordt uitgevoerd als onderdeel van een replica kan bijvoorbeeld gezondheid verslag slechts op de replica die de partitie waartoe het behoort en de toepassing die een onderdeel van is.

- Gebruik `FabricClient`.   
U kunt `FabricClient` voor de gezondheid van het rapport van de code als het cluster niet is [beveiligd](service-fabric-cluster-security.md) of als de service wordt uitgevoerd met beheerdersbevoegdheden. Dit is niet waar in de meeste concrete gevallen. Met `FabricClient`, u kunt elke entiteit die deel uitmaakt van het cluster gezondheid rapporteren. In het ideale geval echter servicecode moet alleen worden verzonden rapporten die betrekking op de eigen gezondheid hebben.

Dit artikel helpt u bij een voorbeeld van de gezondheid van de code van de rapporten. In het voorbeeld ziet u ook hoe de hulpprogramma's van Fabric-Service kunnen worden gebruikt om te controleren van de status. Dit artikel is bedoeld als een korte inleiding van de controlefuncties van weefsel van de Service. U kunt de reeks van diepgaande artikelen over gezondheid die met de koppeling aan het einde van dit artikel beginnen lezen voor meer gedetailleerde informatie.

## <a name="prerequisites"></a>Vereisten
U hebt de volgende software zijn geïnstalleerd:

   * Visual Studio 2015
   * Fabric Service SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Een lokale beveiligde dev-cluster maken
- PowerShell openen met beheerdersbevoegdheden en voer de volgende opdrachten.

![Opdrachten die laten zien hoe een beveiligde dev-cluster maken](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Een toepassing implementeren en controleren van de gezondheid

1. Visual Studio openen als beheerder.

2. Een project maken met behulp van de sjabloon **Stateful-Service** .

    ![Een Service Fabric-toepassing maken met Stateful-Service](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Druk op **F5** om de toepassing uitvoert in foutopsporingsmodus. De toepassing wordt geïmplementeerd met het lokale cluster.

4. Nadat de toepassing wordt uitgevoerd, met de rechtermuisknop op het lokale Cluster Manager-pictogram in het systeemvak en selecteer **Lokale Cluster beheren** in het snelmenu Service Fabric Explorer openen.

    ![Service Fabric Explorer openen uit systeemvak](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. De gezondheid van de toepassing moet worden weergegeven in deze afbeelding. Op dit moment is de toepassing worden gezonde zonder fouten.

    ![Gezonde toepassing in Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. U kunt ook de status controleren met PowerShell. U kunt ```Get-ServiceFabricApplicationHealth``` controleren van de gezondheid van de toepassing en u kunt ```Get-ServiceFabricServiceHealth``` te controleren van de service. Het statusrapport voor dezelfde toepassing in PowerShell is in deze afbeelding.

    ![Gezonde toepassing in PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Gezondheid aangepaste gebeurtenissen aan uw servicecode toevoegen
De sjablonen Service Fabric-project in Visual Studio bevatten voorbeeldcode. De volgende stappen laten zien hoe kunt u aangepaste gezondheid gebeurtenissen melden vanuit uw servicecode. Dergelijke rapporten worden automatisch weergegeven in de standaard controlehulpprogramma's voor de gezondheid biedt Service Fabric, zoals Service Fabric Explorer Azure portal gezondheid weergeven en PowerShell.

1. De toepassing die u eerder hebt gemaakt in Visual Studio openen of een nieuwe toepassing maken met de **Service van Stateful** Visual Studio-sjabloon.

2. Open het bestand Stateful1.cs en zoek de `myDictionary.TryGetValueAsync` aanroepen in de `RunAsync` methode. U kunt zien dat deze methode resulteert in een `result` die de huidige waarde van het item bevat, omdat de sleutel logica in deze toepassing is het houden van een telling uitgevoerd. Dit is een echte toepassing en het ontbreken van het resultaat van een fout weergegeven, wilt markeren die gebeurtenis.

3. Toevoegen als u wilt een gezondheid gebeurtenis rapporteren wanneer het gebrek aan resultaat duidt op een fout, de volgende stappen uit.

    een. Voeg toe de `System.Fabric.Health` naamruimte naar het bestand Stateful1.cs.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Voeg de volgende code na de `myDictionary.TryGetValueAsync` aanroepen

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    We melden gezondheid replica omdat deze van een stateful-service wordt gerapporteerd. De `HealthInformation` parameter wordt informatie opgeslagen over de gezondheid probleem dat wordt gerapporteerd.

    Als u een service stateless had gemaakt, gebruikt u de volgende code

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Als de service wordt uitgevoerd met beheerdersbevoegdheden of als het cluster niet [beveiligd is](service-fabric-cluster-security.md), kunt u ook gebruiken `FabricClient` voor de gezondheid van het rapport zoals in de volgende stappen uit.  

    een. Maak de `FabricClient` instantie na de `var myDictionary` aangifte.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Voeg de volgende code na de `myDictionary.TryGetValueAsync` aanroepen.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Laten we deze storing simuleren en Zie geven in de controleprogramma's voor de gezondheid. Voor het simuleren van de fout, opmerking uit de eerste regel van de gezondheid reporting code die u eerder hebt toegevoegd. Nadat u een reactie op de eerste regel, lijken de code in het volgende voorbeeld.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Deze code nu geactiveerd dit statusrapport telkens `RunAsync` wordt uitgevoerd. Druk op **F5** om de toepassing te starten nadat u de wijziging hebt aangebracht.

6. Nadat de toepassing is gestart, opent u Service Fabric Explorer om te controleren van de status van de toepassing. Deze keer geeft Service Fabric Explorer aan dat de toepassing beschadigd is. Dit komt door de fout die is gerapporteerd uit de code die we eerder toegevoegd.

    ![Beschadigde toepassing in Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Als u de primaire replica in de boomstructuurweergave van Service Fabric Explorer selecteert, ziet u dat **Status** een fout aangeeft, te. Service Fabric Explorer geeft ook aan gezondheid rapport die zijn toegevoegd aan de `HealthInformation` -parameter in de code. Hier ziet u de dezelfde statusrapporten in PowerShell en de Azure portal.

    ![Gezondheid in Service Fabric Explorer replica](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Dit rapport blijft in beheer van de gezondheid totdat deze is vervangen door een ander rapport of totdat deze replica wordt verwijderd. Omdat we niet ingesteld `TimeToLive` voor dit rapport gezondheid in de `HealthInformation` object, wordt het rapport nooit verloopt.

Wij raden aan dat de gezondheid op het meest gedetailleerde niveau, in dit geval de replica is moet worden gerapporteerd. U kunt ook rapporteren gezondheid `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Voor de gezondheid van het rapport op `Application`, `DeployedApplication`, en `DeployedServicePackage`, gebruikt u `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Volgende stappen
[Grondige kennismaking op gezondheid Service Fabric](service-fabric-health-introduction.md)
