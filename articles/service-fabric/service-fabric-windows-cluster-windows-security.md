<properties
   pageTitle="Een cluster met Windows-beveiliging gebruikt Windows beveiligen | Microsoft Azure"
   description="Informatie over het knooppunt tot knooppunt en client-naar-knooppunt Beveiliging configureren op een zelfstandige cluster op Windows met behulp van Windows-beveiliging."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Een zelfstandige cluster op Windows met behulp van Windows-beveiliging veilig

Om te voorkomen dat onbevoegde toegang tot een Service Fabric '-cluster die kunt u beveiligen, vooral wanneer deze productie werklasten uitgevoerd heeft. In dit artikel wordt beschreven hoe u van knooppunt naar knooppunt en client-naar-knooppunt beveiliging met behulp van Windows-beveiliging in het bestand *ClusterConfig.JSON* en komt overeen met de stap van de beveiliging configureren van [een cluster zelfstandige maken waarop Windows](service-fabric-cluster-creation-for-windows-server.md). Zie [scenario's voor netwerkbeveiliging Cluster](service-fabric-cluster-security.md)voor meer informatie over hoe Fabric-Service gebruikt de beveiliging van Windows.

>[AZURE.NOTE]
U moet zorgvuldig uw selectie voor beveiliging voor beveiliging van knooppunt tot knooppunt, omdat er geen upgrade van cluster uit één keuze naar de andere. Wijzigen van de selectie van beveiliging vereist een volledige cluster opnieuw maken.

## <a name="configure-windows-security"></a>Windows-beveiliging configureren
Het voorbeeld *ClusterConfig.Windows.JSON* configuratiebestand gedownload met de [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) zelfstandig pakket met cluster bevat een sjabloon voor het configureren van Windows-beveiliging.  Windows-beveiliging wordt geconfigureerd in de sectie **Eigenschappen** :

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Configuratie-instelling**|**Beschrijving**|
|-----------------------|--------------------------|
|ClusterCredentialType|Windows-beveiliging is ingeschakeld door de parameter **ClusterCredentialType** voor *Windows*.|
|ServerCredentialType|Windows-beveiliging voor clients is ingeschakeld door de parameter **ServerCredentialType** voor *Windows*. Dit betekent dat de clients van het cluster en de cluster zelf, worden uitgevoerd in een Active Directory-domein.|
|WindowsIdentities|De identiteit van de client en cluster bevat.|
|ClusterIdentity|Knooppunt naar beveiliging configureert. Een door komma's gescheiden lijst met groep beheerde serviceaccounts of namen van de machine.|
|ClientIdentities|Client-naar-knooppunt beveiliging configureert. Een matrix van gebruikersaccounts van de client.|
|Identiteit|De client-id, een domeingebruiker.|
|IsAdmin|True geeft aan dat de domeingebruiker administrator clienttoegang, false voor gebruikerstoegang voor de client heeft.|

[Knooppunt naar knooppunt beveiliging](service-fabric-cluster-security.md#node-to-node-security) wordt geconfigureerd via de instelling **ClusterIdentity**. Om te bouwen van vertrouwensrelaties tussen knooppunten, moeten ze worden aangebracht op de hoogte van elkaar. Dit kan op twee verschillende manieren worden uitgevoerd: de groep beheerd serviceaccount met alle knooppunten in het cluster opgeven of de identiteit van het knooppunt domein van alle knooppunten in het cluster opgeven. Het wordt aangeraden met de benadering van de [Groep beheerde Service-Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) , met name voor grotere clusters (meer dan 10 knooppunten) of voor clusters die waarschijnlijk groter of kleiner.
Deze benadering kunt knooppunten worden toegevoegd aan of verwijderd uit de gMSA, zonder dat wijzigingen in het manifest van het cluster. Deze benadering is niet vereist voor het maken van een domeingroep die clusterbeheerders toegangsrechten toevoegen en verwijderen van leden hebben verleend. Zie [Aan de slag met een groep beheerde serviceaccounts](http://technet.microsoft.com/library/jj128431.aspx)voor meer informatie.

[Knooppunt beveiliging-client](service-fabric-cluster-security.md#client-to-node-security) is geconfigureerd met behulp van **ClientIdentities**. Teneinde het vertrouwen tussen een client en het cluster moet u het cluster als u wilt weten welke client-identiteiten die kunnen worden vertrouwd. Dit kan op twee verschillende manieren worden gedaan: Geef de groep Domeingebruikers die u kunnen verbinding maken, of het domein knooppunt gebruikers die verbinding kunnen maken. Fabric-service ondersteunt twee verschillende access control voor clients die zijn verbonden met een cluster Service Fabric: beheerder en gebruiker. Access control biedt de mogelijkheid voor clusterbeheer aan de toegang beperken tot bepaalde soorten clusterbewerkingen voor verschillende groepen gebruikers, het veiliger maken van het cluster.  Beheerders hebben volledige toegang tot de beheerfuncties (met inbegrip van mogelijkheden voor lezen/schrijven). Gebruikers hebben standaard alleen leestoegang tot de beheerfuncties (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.

In het volgende gedeelte van de voorbeeld- **beveiliging** configureert Windows-beveiliging en geeft aan dat de machines in de *ServiceFabric/clusterA.contoso.com* deel van het cluster uitmaken en dat *CONTOSO\usera* admin clienttoegang heeft:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Volgende stappen

Na het configureren van Windows-beveiliging in het bestand *ClusterConfig.JSON* , het proces voor het cluster maken in [een cluster zelfstandige maken waarop Windows](service-fabric-cluster-creation-for-windows-server.md)te hervatten.

Zie [scenario's voor netwerkbeveiliging Cluster](service-fabric-cluster-security.md)voor meer informatie over het knooppunt naar beveiliging, beveiliging van client-naar-knooppunt en op rollen gebaseerde toegangscontrole.

Zie [verbinding maken met een cluster beveiligen](service-fabric-connect-to-secure-cluster.md) voor voorbeelden van verbinding maken met behulp van PowerShell of FabricClient.
