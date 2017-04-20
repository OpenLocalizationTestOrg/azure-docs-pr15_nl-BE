<properties
   pageTitle="Upgrade van de toepassing: geavanceerde onderwerpen | Microsoft Azure"
   description="Dit artikel heeft betrekking op bepaalde geavanceerde onderwerpen over het upgraden van een Service Fabric-toepassing."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Service Fabric toepassing upgrade: geavanceerde onderwerpen

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Toevoegen of verwijderen van services tijdens een upgrade van de toepassing

Als een nieuwe service is toegevoegd aan een toepassing die is al geïmplementeerd en gepubliceerd als een upgrade, wordt de nieuwe service toegevoegd aan de gedistribueerde toepassing.  Een dergelijke upgrade heeft geen invloed op een van de services die al deel van de toepassing uitmaakten. Echter een exemplaar van de service die is toegevoegd voor de nieuwe service actief moet worden gestart (met behulp van de `New-ServiceFabricService` cmdlet).

Services kunnen ook vanuit een toepassing als onderdeel van een upgrade worden verwijderd. Echter, alle huidige exemplaren van de service naar-worden verwijderd moeten worden gestopt voordat u de upgrade uitvoert (met behulp van de `Remove-ServiceFabricService` cmdlet). 

## <a name="manual-upgrade-mode"></a>Modus voor handmatig bijwerken

> [AZURE.NOTE]  De niet-beheerde handmatige modus overwogen worden alleen voor de upgrade van een mislukte of onderbroken. De gecontroleerde modus is de aanbevolen upgrade modus voor Service Fabric-toepassingen.

Azure Service Fabric biedt meerdere upgrade modi ter ondersteuning van de ontwikkeling en productie van clusters. Implementatie-opties gekozen zijn voor verschillende omgevingen anders.

De gecontroleerde rolling upgrade van toepassing is de meest voorkomende upgrade gebruiken in productie. Wanneer de upgrade beleid is opgegeven, Fabric-Service zorgt ervoor dat de toepassing in orde voordat u de upgrade verloopt.

 De beheerder van de toepassing kunt gebruiken de handmatige rolling upgrade toepassingsmodus totale controle over de voortgang van de upgrade via de verschillende domeinen van de upgrade. Deze modus is handig wanneer u een aangepaste of complexe evaluatie statusbeleid vereist is of een niet-conventionele upgrade gebeurt (bijvoorbeeld de toepassing is al in het verlies van gegevens).

Ten slotte is de geautomatiseerde rolling upgrade van toepassing handig voor ontwikkeling of testomgevingen te bieden een snelle iteratiecyclus tijdens de serviceontwikkeling.

## <a name="change-to-manual-upgrade-mode"></a>Naar de modus voor handmatig bijwerken
**Handleiding**--de upgrade van toepassing op de huidige UD stoppen en de upgrade-modus op niet-beheerde handmatig wijzigen. De beheerder moet handmatig call **MoveNextApplicationUpgradeDomainAsync** terugdraaien activeren door het initiëren van een nieuwe upgrade als u wilt doorgaan met de upgrade. Zodra de upgrade in de handmatige modus invoert, blijft in de handmatige modus totdat een nieuwe upgrade wordt gestart. De opdracht **GetApplicationUpgradeProgressAsync** retourneert FABRIC\_toepassing\_UPGRADE\_staat\_rollend\_naar voren\_in behandeling.

## <a name="upgrade-with-a-diff-package"></a>Upgraden met een diff-pakket

Een Service Fabric-toepassing kan worden geüpgraded door inrichten met een volledige, op zichzelf staande toepassingspakket. Een toepassing kan met behulp van een diff-pakket alleen de bestanden van de bijgewerkte toepassing manifest van de bijgewerkte toepassing en de manifest-bestanden bevat worden bijgewerkt.

Een volledige toepassing-pakket bevat alle bestanden die nodig zijn om te starten en uitvoeren van een Service Fabric-toepassing. Een diff-pakket bevat alleen de bestanden die sinds de laatste bepaling en de huidige upgrade gewijzigd, plus het manifest voor de volledige toepassing en de manifest-bestanden. Elke verwijzing in het manifest van de toepassing of servicemanifest is niet in de lay-out maken gevonden is gezocht in het archief van de installatiekopie.

Volledige toepassingspakketten zijn vereist voor de eerste installatie van een toepassing aan het cluster. Bijwerkingen kunnen worden ofwel een volledige toepassing of een diff-pakket.

Gelegenheden wanneer met behulp van een diff-pakket een goede keuze zou zijn:

* Een diff-pakket verdient de voorkeur als u beschikt over een groot pakket dat verwijst naar verschillende service manifest-bestanden en/of verschillende pakketten code, config pakketten of data pakketten.

* Een diff-pakket heeft de voorkeur wanneer er een systeem implementatie die de lay-out maken rechtstreeks vanuit uw bouwproces toepassing genereert. In dit geval, hoewel de code niet is gewijzigd, krijgt nieuw gebouwde assembly's een andere controlesom. Met behulp van een pakket voor de volledige toepassing moet u de versie op alle pakketten met code te werken. Met behulp van een diff-pakket, bieden u alleen de bestanden die worden gewijzigd en de manifest-bestanden waarin de versie is gewijzigd.

Wanneer een toepassing wordt bijgewerkt met behulp van Visual Studio, is het diff-pakket automatisch gepubliceerd. U maakt een diff-pakket handmatig, manifest voor de toepassing en de manifesten service moeten worden bijgewerkt, maar alleen de gewijzigde pakketten moeten worden opgenomen in het toepassingspakket van de uiteindelijke. 

Bijvoorbeeld, laten we beginnen met de volgende toepassing (versienummers voorzien wat gebruiksgemak):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Nu gaan we ervan uit u wilde updatepakket alleen de code van service1 met behulp van een diff-pakket met PowerShell. De bijgewerkte toepassing heeft nu de volgende mappenstructuur:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

In dit geval bijwerken u manifest van de toepassing 2.0.0 en het servicemanifest voor service1 om update van het pakket weer te geven. De map voor uw toepassingspakket zou de volgende structuur hebben:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Volgende stappen

[Upgraden van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) doorloopt u de upgrade van een toepassing met behulp van Visual Studio.

[Upgraden van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) doorloopt u de upgrade van een toepassing met PowerShell.

Bepalen hoe uw toepassing met behulp van [Parameters Upgrade](service-fabric-application-upgrade-parameters.md)upgrades.

De toepassingsupgrades van uw compatibel maken door te leren hoe u [Gegevens serialisatie](service-fabric-application-upgrade-data-serialization.md).

Veelvoorkomende problemen oplossen in de toepassingsupgrades door te verwijzen naar de stappen in [Upgrades voor het oplossen van toepassing](service-fabric-application-upgrade-troubleshooting.md).
 
