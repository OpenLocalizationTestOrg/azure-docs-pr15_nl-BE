<properties
   pageTitle="Configureren van de upgrade van de toepassing van een Service Fabric | Microsoft Azure"
   description="Informatie over het configureren van de instellingen voor het upgraden van een Service Fabric-toepassing met behulp van Microsoft Visual Studio."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>De upgrade van een Service Fabric-toepassing configureren in Visual Studio

Visual Studio tools voor Azure Service Fabric bieden upgrade ondersteuning voor publicatie in de lokale of externe clusters. Er zijn twee voordelen verbonden aan een upgrade van uw toepassing naar een nieuwere versie in plaats van de toepassing tijdens het testen en debuggen vervangen:

- Toepassingsgegevens niet verloren tijdens de upgradeprocedure.
- Beschikbaarheid blijft hoog zodat er niet een onderbreking van de service tijdens de upgrade, als er dat voldoende service-exemplaren verspreid over de domeinen upgraden.

Tests kunnen worden uitgevoerd ten opzichte van een toepassing, terwijl deze wordt bijgewerkt.

## <a name="parameters-needed-to-upgrade"></a>Parameters die nodig zijn om te upgraden

U kunt kiezen uit twee soorten implementatie: regelmatige of upgrade. Een gewone implementatie wist alle vorige implementatie-informatie en gegevens op het cluster, terwijl een upgrade-installatie deze behoudt. Wanneer u een Service Fabric-toepassing in Visual Studio bijwerkt, moet u opgeven upgrade parameters voor de toepassing en de gezondheid beleid controleren. Toepassingsparameters upgrade kunnen uitvoeren van de upgrade terwijl het selectievakje statusbeleid bepalen of de upgrade voltooid is. Zie [Fabric Service application upgrade: parameters bijwerken](service-fabric-application-upgrade-parameters.md) voor meer informatie.

Er zijn drie modi voor upgrade: *gecontroleerde*, *UnmonitoredAuto*en *UnmonitoredManual*.

  - Een upgrade van de gecontroleerde automatiseert de upgrade en statuscontrole van toepassing.

  - Een upgrade UnmonitoredAuto automatiseert de upgrade maar de statuscontrole van toepassingen wordt overgeslagen.

  - Wanneer u een upgrade van UnmonitoredManual doet, moet u elk domein upgrade handmatig bijwerken.

Elke upgrade modus vereist verschillende sets van parameters. Zie een [toepassing parameters bijwerken](service-fabric-application-upgrade-parameters.md) voor meer informatie over de beschikbare upgradeopties.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Upgrade uitvoeren op een toepassing Service Fabric in Visual Studio

Als u de hulpprogramma's van Visual Studio Service Fabric upgrade uitvoeren op een toepassing Fabric-Service gebruikt, kunt u een proces publiceren als u een upgrade in plaats van een gewone implementatie door het selectievakje **Upgrade van de toepassing** controleren.

### <a name="to-configure-the-upgrade-parameters"></a>De upgrade parameters configureren

1. Klik op de knop **Instellingen** naast het selectievakje. Het dialoogvenster **Upgrade Parameters bewerken** wordt weergegeven. Het dialoogvenster **Upgrade Parameters bewerken** ondersteunt de upgrade modi gecontroleerde, UnmonitoredAuto en UnmonitoredManual.

2. Selecteer de upgrade-modus die u wilt gebruiken en vul vervolgens het raster voor de parameter.

    Elke parameter heeft standaardwaarden. De optionele parameter *DefaultServiceTypeHealthPolicy* wordt de invoer van een hash-tabel. Hier volgt een voorbeeld van de hash-tabel invoer-indeling voor *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* is een andere is een optionele parameter waarmee de invoer van een hash-tabel in de volgende notatie:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Hier volgt een voorbeeld van de praktijk:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Als u een upgrade UnmonitoredManual-modus selecteert, moet u een console PowerShell om te gaan en het proces voltooien handmatig starten. Verwijzen naar [Fabric Service application upgrade: geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md) voor meer informatie over hoe handmatige upgrade werkt.

## <a name="upgrade-an-application-by-using-powershell"></a>Upgrades uitvoeren voor toepassingen met PowerShell

PowerShell-cmdlets kunt u upgrade uitvoert voor een toepassing Fabric-Service. Zie [zelfstudie upgrade Service Fabric-toepassing](service-fabric-application-upgrade-tutorial.md) en [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) voor meer informatie.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Een selectievakje statusbeleid opgeven in het manifestbestand van toepassing

Elke service in een toepassing Fabric-Service kan zijn eigen gezondheid beleidsparameters die voorrang de standaardwaarden boven hebben. U kunt deze parameterwaarden in het manifestbestand van toepassing bieden.

In het volgende voorbeeld ziet u hoe een unieke controle van het statusbeleid voor elke service in het manifest van de toepassing toepassen.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het implementeren van een toepassing [een bestaande toepassing in Azure Service Fabric implementeren](service-fabric-deploy-existing-app.md).
