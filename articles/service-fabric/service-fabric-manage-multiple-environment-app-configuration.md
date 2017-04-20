<properties
   pageTitle="Beheren van omgevingen met meerdere Service stof | Microsoft Azure"
   description="Service Fabric-toepassingen kunnen worden uitgevoerd op clusters die in grootte van één machine naar duizenden machines variëren. In sommige gevallen zult u de toepassing voor deze uiteenlopende omgevingen verschillend configureren. Dit artikel heeft betrekking op het definiëren van parameters voor verschillende toepassingen per omgeving."
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
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Toepassingsparameters voor omgevingen met meerdere beheren

Met behulp van een willekeurige plaats van één tot vele duizenden machines kunt u clusters Azure Service Fabric. Binaire bestanden van toepassing kunnen worden uitgevoerd zonder wijziging in dit brede spectrum van omgevingen, wilt u meestal de toepassing verschillend, afhankelijk van het aantal machines te introduceert configureren.

Een eenvoudig voorbeeld, kunt u `InstanceCount` voor een stateless service. Wanneer u toepassingen in Azure uitvoert, wilt u in het algemeen deze parameter instelt op de speciale waarde '-1'. Dit zorgt ervoor dat de service wordt uitgevoerd op elk knooppunt in het cluster. Deze configuratie is echter niet geschikt voor een cluster met één computer omdat er geen meerdere processen op hetzelfde eindpunt op een afzonderlijke computer luistert. In plaats daarvan wordt doorgaans ingesteld `InstanceCount` op "1".

## <a name="specifying-environment-specific-parameters"></a>Omgeving-specifieke parameters opgeven

De oplossing voor dit probleem is een set services die standaard met parameters en toepassingsbestanden parameter die vult u de parameterwaarden voor een bepaalde omgeving. Standaardservices en de parameters voor toepassingen zijn in de manifesten toepassing en service geconfigureerd. De schemadefinitie voor de bestanden ServiceManifest.xml en ApplicationManifest.xml wordt geïnstalleerd met de Service Fabric SDK en de hulpprogramma's naar *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Standaardservices

Service Fabric toepassingen zijn opgebouwd uit een verzameling van exemplaren van de service. Het is mogelijk dat u een lege toepassing maken en vervolgens dynamisch maken alle exemplaren van de service, de meeste toepassingen, hebben een aantal basisservices die altijd moet worden gemaakt wanneer de toepassing wordt gestart. Deze worden aangeduid als 'standaardservices'. Deze zijn opgegeven in het manifest van de toepassing met tijdelijke aanduidingen voor de per-omgeving configureren tussen vierkante haakjes geplaatst:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Elk van de benoemde parameters moet worden gedefinieerd in het element van de Parameters van het manifest van de toepassing:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Het kenmerk DefaultValue bevat de waarde in de afwezigheid van een meer specifieke parameter wordt gebruikt voor een bepaalde omgeving.

>[AZURE.NOTE] Niet alle parameters voor service-exemplaar zijn geschikt voor de configuratie van per-omgeving. In het bovenstaande voorbeeld zijn de LowKey en HighKey waarden voor partitionering van de service expliciet gedefinieerd voor alle exemplaren van de service omdat het bereik van de partitie een functie van het domein van de gegevens, niet op de omgeving is.


### <a name="per-environment-service-configuration-settings"></a>Configuratie-instellingen per omgeving

Het [toepassingsmodel Fabric Service](service-fabric-application-model.md) kunnen services omvatten configuratiepakketten met aangepaste sleutel / waarde-paren die tijdens runtime kunnen gelezen worden. De waarden van deze instellingen ook door de omgeving kunnen worden onderscheiden door te geven een `ConfigOverride` in het manifest van de toepassing.

Stel dat u de volgende instelling in het bestand Config\Settings.xml voor de `Stateful1` service:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Als deze waarde voor een paar specifieke toepassingsomgeving negeren, maken een `ConfigOverride` wanneer u importeert het servicemanifest in het manifest van de toepassing.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Deze parameter kan vervolgens worden geconfigureerd door de omgeving zoals hierboven is weergegeven. U kunt dit doen door in de sectie parameters van het manifest van de toepassing het declareren en milieu-specifieke waarden opgeven in de parameter toepassingsbestanden.

>[AZURE.NOTE] In het geval van configuratie-instellingen, er zijn drie plaatsen waar de waarde van een sleutel kan worden ingesteld: het configuratiepakket service manifest van de toepassing en de parameter-bestand van de toepassing. Fabric-service altijd wordt gekozen in het toepassingsbestand parameter eerste (indien opgegeven), vervolgens het manifest van de toepassing en ten slotte het configuratiepakket.


### <a name="application-parameter-files"></a>Parameter toepassingsbestanden

De Fabric Service application-project kan een of meer parameter toepassingsbestanden bevatten. Elk van hen definieert de specifieke waarden voor de parameters die zijn gedefinieerd in het manifest van de toepassing:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Een nieuwe toepassing bevat twee toepassing parameterbestanden, Local.xml en Cloud.xml:

![Toepassingsbestanden-parameter in de Solution Explorer][app-parameters-solution-explorer]

Om een nieuwe parameter, gewoon kopiëren en plakken van een bestaande en een nieuwe naam geven.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Omgeving-specifieke parameters te identificeren tijdens de implementatie

Bij de implementatie moet u de juiste parameterbestand toe te passen bij uw toepassing kiezen. U kunt dit doen via het dialoogvenster publiceren in Visual Studio of via PowerShell.

### <a name="deploy-from-visual-studio"></a>Implementeren van Visual Studio

U kunt kiezen uit de lijst met beschikbare parameterbestanden wanneer u uw toepassing in Visual Studio.

![Kies een parameterbestand in het dialoogvenster publiceren][publishdialog]

### <a name="deploy-from-powershell"></a>Implementeren van PowerShell

De `Deploy-FabricApplication.ps1` PowerShell script opgenomen in de toepassingssjabloon voor het project een publicatieprofiel als een parameter accepteert en de PublishProfile een verwijzing bevat naar het bestand met de toepassing.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over sommige van de belangrijkste begrippen die in dit onderwerp worden besproken, de [technische Service Fabric-overzicht](service-fabric-technical-overview.md). Voor informatie over andere mogelijkheden voor het beheer van app die beschikbaar in Visual Studio zijn, Zie [beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
