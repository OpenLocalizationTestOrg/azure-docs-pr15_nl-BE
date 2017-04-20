<properties
    pageTitle="Een app publiceren op een externe cluster met Visual Studio | Microsoft Azure"
    description="Informatie over het publiceren van een toepassing naar een externe service fabric '-cluster met behulp van Visual Studio."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Een toepassing uitgeven aan een externe cluster met behulp van Visual Studio

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

De Azure Service Fabric-extensie voor Visual Studio biedt een eenvoudig, herhaalbaar en aanpasbare manier naar een toepassing uitgeven aan een Service Fabric '-cluster.

## <a name="the-artifacts-required-for-publishing"></a>De artefacten die nodig zijn voor publicatie

### <a name="deploy-fabricapplicationps1"></a>Implementatie van FabricApplication.ps1

Dit is een PowerShell-script dat gebruikmaakt van een profielpad publiceren als een parameter voor publishing Service Fabric-toepassingen. Aangezien dit script onderdeel van uw toepassing is, bent u Welkom om deze te wijzigen indien nodig voor uw toepassing.

### <a name="publish-profiles"></a>Publicatieprofielen

Een map in de Fabric Service application-project genaamd **PublishProfiles** bevat XML-bestanden waarin essentiële informatie voor het uitgeven van een toepassing, zoals:

- Verbinding voor service Fabric Clusterparameters
- Pad naar een parameterbestand
- Upgrade-instellingen

Standaard de toepassing bevat twee Publicatieprofielen: Local.xml en Cloud.xml. U kunt meer profielen toevoegen door kopiëren en plakken van een van de standaard-bestanden.

### <a name="application-parameter-files"></a>Parameter toepassingsbestanden

Een map in de Fabric Service application-project genoemd **ApplicationParameters** bevat XML-bestanden voor gebruiker ingestelde toepassing manifest parameterwaarden. Manifest-bestanden van toepassing kunnen zijn ingesteld, zodat kunt u verschillende waarden voor de implementatie-instellingen. Meer informatie over de toepassing van parameters voorzien, Zie [beheren van omgevingen met meerdere Service stof](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Voor actor-services, moet u het project eerst voordat u het bestand in een editor of via het dialoogvenster publiceren samenstellen. Dit komt omdat het deel van de manifest-bestanden worden gegenereerd tijdens het opbouwen.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Een toepassing uitgeven met behulp van het dialoogvenster Service Fabric-toepassing publiceren

De volgende stappen laten zien hoe een toepassing uitgeven met behulp van het dialoogvenster **Publiceren Service Fabric-toepassing** die door Visual Studio Service Fabric Tools.

1. Kies in het snelmenu van het project Fabric servicetoepassing **publiceren...** het dialoogvenster **Publiceren Service Fabric toepassing** bekijken.

    ![De ** dialoogvenster publiceren Service Fabric toepassing **][0]

    Het bestand is geselecteerd in de vervolgkeuzelijst **doelprofiel** is waar alle instellingen, met uitzondering van het **Manifest van de versies**, worden opgeslagen. Een bestaand profiel gebruiken of een nieuwe maken door **< profielen beheren... >** kiezen in de vervolgkeuzelijst van het **doelprofiel** . Als u een publicatieprofiel kiest, wordt de inhoud ervan in de corresponderende velden in het dialoogvenster weergegeven. Om uw wijzigingen op elk gewenst moment opslaan, kiest u de koppeling **Profiel opslaan** .    

2. Geef in de sectie **eindpunt voor de verbinding** publishing eindpunt van een lokale of externe Service weefsel van het cluster. Als u wilt toevoegen of wijzigen van het verbindingseindpunt, klikt u op de vervolgkeuzelijst van het **Eindpunt voor de verbinding** . De lijst bevat de beschikbare Service Fabric '-cluster verbinding eindpunten die u kunt publiceren op basis van uw abonnement(en) Azure. Houd er rekening mee dat als u zijn niet al aangemeld bij Visual Studio, wordt u gevraagd om dit te doen.

    Gebruik het dialoogvenster selectie cluster kiezen uit de verzameling beschikbare abonnementen en clusters.

    ![De ** Selecteer Service Fabric Cluster ** in het dialoogvenster][1]

    >[AZURE.NOTE] Zie de onderstaande sectie **publiceren op een willekeurige cluster eindpunt** als u wilt publiceren op een willekeurige eindpunt (zoals een partij cluster).

    Wanneer u een eindpunt, controleert Visual Studio de verbinding met de geselecteerde Service Fabric '-cluster. Als het cluster niet is beveiligd, kunt Visual Studio verbinden met het onmiddellijk. Echter, als het cluster beveiligd is, moet u een certificaat installeren op de lokale computer voordat u verdergaat. Zie [beveiligde verbindingen configureren](service-fabric-visualstudio-configure-secure-connections.md) voor meer informatie. Wanneer u bent klaar, klikt u op **OK** . Het geselecteerde cluster wordt weergegeven in het dialoogvenster **Publiceren Service Fabric-toepassing** .

3. Ga naar de parameterbestand van een toepassing in de vervolgkeuzelijst **Bestand Parameter** . Een parameter toepassingsbestand bevat de gebruiker opgegeven waarden voor parameters in het manifestbestand van toepassing. Als u wilt toevoegen of wijzigen van een parameter, klik op de knop **bewerken** . Typ of wijzig de waarde van de parameter in het raster met **Parameters** . Wanneer u klaar bent, klik op de knop **Opslaan** .

    ![De ** dialoogvenster bewerken Parameters **][2]

4. Gebruik het selectievakje **Upgrade van de toepassing** op te geven of deze actie publiceren is een upgrade. Upgrade publiceert acties die afwijken van normale publiceert acties. Zie [Service Fabric toepassing een upgrade uitvoeren](service-fabric-application-upgrade.md) voor een overzicht van de verschillen. Upgrade om instellingen te configureren, kiest u de koppeling **Bijwerken-instellingen configureren** . De upgrade parameter editor weergegeven. Zie [de upgrade van een Service Fabric-toepassing configureren](service-fabric-visualstudio-configure-upgrade.md) voor meer informatie over parameters voor het bijwerken.

5. Kies het **Manifest van de versies...** de knop om het dialoogvenster **Bewerken versies** weer te geven. U moet bijwerken, toepassing en versies van de service voor een upgrade worden uitgevoerd. Zie [zelfstudie upgrade Service Fabric-toepassing](service-fabric-application-upgrade-tutorial.md) voor meer informatie over hoe manifest versies impact een upgradeproces toepassing en service.

    ![De ** dialoogvenster bewerken versies **][3]

    Als de toepassing en de versies van service semantische versiebeheer, zoals 1.0.0 of numerieke waarden in de indeling van 1.0.0.0, selecteert u de optie **automatisch bijwerken van toepassing en versies van service** . Wanneer u deze optie, de service en versienummers van toepassing worden automatisch bijgewerkt wanneer u een code, gegevens of config versie pakket wordt bijgewerkt. Als u liever de versies handmatig bewerken, schakel het selectievakje om deze functie uitschakelen.

    >[AZURE.NOTE] Voor alle pakket items worden weergegeven voor een actor-project, het project voor het genereren van de posten in het Manifest van de Service bestanden eerst te bouwen.

6. Wanneer u klaar bent opgeven van alle benodigde instellingen, klik op de knop **publiceren** om uw toepassing uitgeeft aan de geselecteerde Service Fabric '-cluster. De instellingen die u hebt opgegeven, worden toegepast op het proces publiceren.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Publiceren op een willekeurige cluster eindpunt (met inbegrip van partij-clusters)

De Visual Studio ervaring publiceren is geoptimaliseerd voor publicatie in externe clusters die zijn gekoppeld aan een van uw abonnementen op Azure. Het is echter mogelijk om te publiceren op een willekeurige eindpunten (bijvoorbeeld Service Fabric partij clusters) door het publicatieprofiel XML rechtstreeks te bewerken. Zoals hierboven beschreven, publiceert twee profielen worden geleverd door de standaard -**Local.xml** en **Cloud.xml**, maar u bent Welkom bij extra profielen maken voor verschillende omgevingen. U wilt bijvoorbeeld een profiel maken voor publicatie in partij clusters, bijvoorbeeld met de naam **Party.xml**.

Als u verbinding maakt met een onbeveiligd cluster, die is vereist is het eindpunt van de verbinding cluster zoals `partycluster1.eastus.cloudapp.azure.com:19000`. In dat geval wordt het verbindingseindpunt in het publicatieprofiel zou als volgt uitzien:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Als u verbinding met een beveiligde cluster maakt, moet u ook de gegevens van het certificaat uit het lokale archief moet worden gebruikt voor verificatie. Zie [beveiligde verbindingen naar een Service Fabric '-cluster configureren](service-fabric-visualstudio-configure-secure-connections.md)voor meer informatie.

  Als uw profiel publiceren is ingesteld, u kunt verwijzen naar in het dialoogvenster publiceren als volgt.

  ![Nieuw profiel publiceren publiceren in het dialoogvenster][4]

  Houd er rekening mee dat in dit geval de nieuw publicatieprofiel naar een van de standaard parameter toepassingsbestanden verwijst. Dit is relevant als u wilt dat dezelfde configuratie van de toepassing te publiceren op een aantal verschillende omgevingen. Daarentegen, in gevallen waar u hebt verschillende configuraties voor elke omgeving die u publiceren wilt naar zou het zinvol voor het maken van een bestand voor parameter van overeenkomstige toepassing.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatiseren van het publicatieproces in een omgeving met continue integratie, Zie [Service Fabric continue integratie instellen](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
