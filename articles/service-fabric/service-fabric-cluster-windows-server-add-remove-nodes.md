<properties
   pageTitle="Toevoegen of verwijderen van knooppunten aan een cluster van zelfstandige Service Fabric | Microsoft Azure"
   description="Informatie over het toevoegen of verwijderen van knooppunten aan een cluster Azure Service weefsel op een fysieke of virtuele machine met Windows Server, op de bedrijfsruimten worden kan of in een wolk."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Toevoegen of verwijderen van knooppunten aan een zelfstandige Service Fabric '-cluster waarop Windows Server

Nadat u [gemaakt van uw zelfstandige Service Fabric '-cluster op computers met Windows Server hebt](service-fabric-cluster-creation-for-windows-server.md), kunnen uw zakelijke behoeften zodanig wijzigen dat u wilt toevoegen of verwijderen van meerdere knooppunten aan het cluster. Dit artikel bevat gedetailleerde stappen om dit doel te bereiken.


## <a name="add-nodes-to-your-cluster"></a>Knooppunten toevoegen aan het cluster

1. De VM/machine die u toevoegen aan het cluster wilt door de stappen vermeld in de sectie voor het [voorbereiden van computers die voldoen aan de vereisten voor de implementatie van het cluster](service-fabric-cluster-creation-for-windows-server.md#preparemachines) voorbereiden.
2. Plannen welke domein voor fouttolerantie en upgrade domein u gaat deze VM/machine aan toevoegen.
3. Extern bureaublad (RDP) in de VM/machine die u wilt toevoegen aan het cluster.
4. Kopiëren of [downloaden van het zelfstandige pakket voor Fabric Service voor Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) met deze VM/computer en unzip het pakket.
5. Powershell uitvoeren als administrator en Ga naar de locatie van het pakket uitgepakt.
6. Powershell *AddNode.ps1* uitgevoerd met de parameters met een beschrijving van het nieuwe knooppunt toe te voegen. In het volgende voorbeeld wordt een nieuw knooppunt met de naam VM5, met NodeType0, IP-adres 182.17.34.52 in UD1 en FD1 type toegevoegd. De *ExistingClusterConnectionEndPoint* is voor een knooppunt in het cluster al een endpoint voor de verbinding. Voor dit eindpunt is, kunt u het IP-adres van *een* knooppunt in het cluster.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Knooppunten uit het cluster verwijderen

1. Afhankelijk van het Reliablity gekozen voor het cluster, kunt u de eerste n (3/5/7/9)-knooppunten van het type primaire knooppunt niet verwijderen
2. RemoveNode opdracht uitvoeren op een cluster dev wordt niet ondersteund.
2. Extern bureaublad (RDP) in de VM/machine die u wilt verwijderen uit het cluster.
2. Kopie of [het zelfstandige pakket voor Fabric Service voor Windows Server downloaden](http://go.microsoft.com/fwlink/?LinkId=730690) en unzip het pakket deze VM/machine.
3. Powershell uitvoeren als administrator en Ga naar de locatie van het pakket uitgepakt.
4. Voer *RemoveNode.ps1* in PowerShell. In het volgende voorbeeld wordt het huidige knooppunt uit het cluster verwijderd. De *ExistingClientConnectionEndpoint* is een client verbindingseindpunt voor elk knooppunt dat in het cluster blijft. Kies het IP-adres en de poort van eindpunt van *een* **ander knooppunt** in het cluster. Dit **andere knooppunt** werkt op zijn beurt de configuratie van het cluster voor het knooppunt verwijderd. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Houd er rekening mee dat zelfs na het verwijderen van een knooppunt, het kan worden weergegeven als u in query's en SFX. Dit is een bekende defect en wordt vastgesteld in een toekomstige release. 


## <a name="next-steps"></a>Volgende stappen
- [Configuratie-instellingen voor een zelfstandige Windows-cluster](service-fabric-cluster-manifest.md)
- [Een zelfstandige cluster op Windows met behulp van Windows-beveiliging veilig](service-fabric-windows-cluster-windows-security.md)
- [Een zelfstandige cluster op Windows X509 met beveiligde certificaten](service-fabric-windows-cluster-x509-security.md)
- [Een zelfstandige Service Fabric-cluster maken met Azure VMs waarop Windows wordt uitgevoerd](service-fabric-cluster-creation-with-windows-azure-vms.md)
