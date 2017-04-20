<properties
   pageTitle="Interactie met de Service Fabric clusters met CLI | Microsoft Azure"
   description="Azure CLI gebruiken voor de interactie met een cluster Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Met behulp van de CLI Azure voor interactie met een Fabric '-Cluster Service

U kunt communiceren met de Service Fabric '-cluster van Linux-machines met behulp van de CLI Azure op Linux.

De eerste stap is neemt u de nieuwste versie van de CLI van de git rep en instellen in het pad met de volgende opdrachten:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Voor elke opdracht, ondersteunt, kunt u de naam van de opdracht voor het verkrijgen van de help voor deze opdracht typt. Automatisch aanvullen wordt ondersteund voor de opdrachten. Met de volgende opdracht geeft bijvoorbeeld u help voor alle opdrachten van toepassing. 

```sh
 azure servicefabric application 
```

U kunt de help voor een specifieke opdracht verder filteren zoals in het volgende voorbeeld wordt getoond:

```sh
 azure servicefabric application  create
```

Als u automatisch aanvullen in de CLI, voert u de volgende opdrachten:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

De volgende opdrachten verbinding maken met het cluster en de knooppunten in het cluster weergegeven:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Als u benoemde parameters gebruiken en vinden wat ze zijn, na een opdracht typt u--help. Bijvoorbeeld:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Wanneer u verbinding maakt met een cluster van meerdere machines uit een computer **die geen deel uitmaakt van het cluster**, gebruikt u de volgende opdracht:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Vervangen door de tag PublicIPorFQDN de echte IP of FQDN zo nodig. Wanneer u verbinding maakt met een cluster van meerdere machines uit een computer **die deel uitmaakt van het cluster**, gebruikt u de volgende opdracht:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Kunt u PowerShell of CLI interactie met uw Linux Service Fabric Cluster gemaakt via de portal Azure. 

**Let op:** Deze clusters niet is beveiligd, dus u kan worden openstelling van de-in-één door het openbare IP-adres toe te voegen in het manifest van het cluster.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Met behulp van de CLI Azure verbinding maken met een Cluster Service Fabric

De volgende Azure CLI-opdrachten beschreven hoe u verbinding maken met een cluster beveiligen. Details van het certificaat moeten overeenkomen met een certificaat op de clusterknooppunten.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Als uw certificaat Certificate Authorities (CA's), moet u het toevoegen van de parameter--ca-certificaat-pad zoals in het volgende voorbeeld: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Als er meerdere certificeringsinstanties, gebruik een komma als scheidingsteken.
 
Als de algemene naam in het certificaat niet overeenkomt met het verbindingseindpunt, kunt u de parameter `--strict-ssl` de verificatie overslaan zoals in de volgende opdracht: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Als u Overslaan verificatie van de CA wilt, zou u de--parameter afwijzen onbevoegde zoals in de volgende opdracht: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Nadat u verbinding hebt gemaakt, kunt u moet kunnen andere CLI-opdrachten voor de interactie met het cluster worden uitgevoerd. 

## <a name="deploying-your-service-fabric-application"></a>Uw Service Fabric-toepassing implementeren

De volgende opdrachten voor kopiëren, registreren en start de service fabric-toepassing uitvoeren:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Upgraden van uw toepassing

Het proces is vergelijkbaar met het [proces dat in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Maken, kopiëren, registreren en uw toepassing maken vanaf de hoofdmap van het project. Als uw toepassingsexemplaar de naam fabric: / MySFApp en het type MySFApp, de opdrachten worden als volgt:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Breng de wijziging aan uw toepassing en de gewijzigde-service opnieuw.  Manifestbestand van de gewijzigde service (ServiceManifest.xml) bijwerken met de bijgewerkte versies voor de Service (en Code of Config of gegevens indien nodig). Manifest van de toepassing (ApplicationManifest.xml) ook wijzigen met het nummer van de bijgewerkte versie van de toepassing en de gewijzigde service.  Nu Kopieer en Registreer uw bijgewerkte toepassing met de volgende opdrachten:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

U kunt de upgrade van de toepassing nu starten met de volgende opdracht:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

U kunt nu de upgrade van de toepassing met SFX controleren. In een paar minuten zou de toepassing zijn bijgewerkt.  Ook kunt u proberen een bijgewerkt app met een fout en controleren de functionaliteit auto rollback service stof.

## <a name="troubleshooting"></a>Het oplossen van problemen

### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopiëren van het toepassingspakket niet is gelukt

Als `openssh` is geïnstalleerd. Standaard hebt Desktop van Ubuntu niet geïnstalleerd. Installeren met behulp van de volgende opdracht:

```
 sudo apt-get install openssh-server openssh-client**
```

Als het probleem zich blijft voordoen, kunt u uitschakelen PAM voor ssh door het **sshd_config** bestand met de volgende opdrachten:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Als het probleem zich weer voordoet, verhoog het aantal ssh sessies door het uitvoeren van de volgende opdrachten:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Met behulp van sleutels voor ssh verificatie (in plaats van wachtwoorden) niet wordt nog ondersteund (Aangezien het platform maakt gebruik van ssh pakketten kopiëren), dus in plaats daarvan gebruiken wachtwoordverificatie.


## <a name="next-steps"></a>Volgende stappen

Instellen van de ontwikkelomgeving en -toepassingen naar een Linux cluster Service Fabric distribueren.
