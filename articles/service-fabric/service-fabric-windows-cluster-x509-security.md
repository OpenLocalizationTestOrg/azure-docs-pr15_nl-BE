<properties
   pageTitle="Verbinding maken met een beveiligde particuliere cluster | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe een beveiligde communicatie binnen de zelfstandige of particuliere cluster ook tussen clients en het cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Een zelfstandige cluster op Windows met behulp van x.509-certificaten beveiligen

In dit artikel wordt beschreven hoe voor het beveiligen van de communicatie tussen de knooppunten van het cluster zelfstandige Windows ook hoe voor het verifiëren van clients die verbinding maken met dit cluster met X.509-certificaten. Dit zorgt ervoor dat alleen geautoriseerde gebruikers kunnen toegang krijgen het cluster, de gebruikte toepassingen tot en beheertaken uitvoeren.  Certificaatbeveiliging moet zijn ingeschakeld op het cluster als het cluster wordt gemaakt.  

Zie [scenario's voor netwerkbeveiliging Cluster](service-fabric-cluster-security.md)voor meer informatie over de beveiliging zoals beveiliging node naar node cluster client tot het knooppunt beveiliging en rollen gebaseerde toegangscontrole.

## <a name="which-certificates-will-you-need"></a>Welke certificaten hebt u nodig?

Om te beginnen met [downloaden van het zelfstandige pakket met cluster](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) naar een van de knooppunten in het cluster. Het gedownloade pakket vindt u een **ClusterConfig.X509.MultiMachine.json** -bestand. Open het bestand en lees de sectie **beveiliging** onder de sectie **-Eigenschappen** :

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

Deze sectie beschrijft de certificaten die u nodig hebt voor het beveiligen van uw zelfstandige Windows-cluster. Stel de waarden van **ClusterCredentialType** en **ServerCredentialType** naar *X509*als u beveiliging op basis van certificaten.

>[AZURE.NOTE] Een [vingerafdruk](https://en.wikipedia.org/wiki/Public_key_fingerprint) is de primaire identiteit van een certificaat. Lees [hoe u kunt ophalen van de blauwdruk van een certificaat](https://msdn.microsoft.com/library/ms734695.aspx) wilt weten de vingerafdruk van de certificaten die u maakt.

De volgende tabel worden de certificaten die u op de clusterinstallatie moet:

|**CertificateInformation instellen**|**Beschrijving**|
|-----------------------|--------------------------|
|ClusterCertificate|Dit certificaat is vereist voor de communicatie tussen de knooppunten in een cluster beveiligen. U kunt twee verschillende certificaten, een primair en een secundair voor de upgrade. De blauwdruk van het certificaat van de primaire instellen in de sectie **vingerafdruk** en die van de secundaire in de variabelen **ThumbprintSecondary** .|
|ServerCertificate|Dit certificaat wordt overgelegd aan de client wanneer deze verbinding probeert te maken voor dit cluster. Voor het gemak kunt u hetzelfde certificaat gebruiken voor *ClusterCertificate* en *ServerCertificate*. U kunt twee verschillende servercertificaten, een primair en een secundair voor de upgrade. De blauwdruk van het certificaat van de primaire instellen in de sectie **vingerafdruk** en die van de secundaire in de variabelen **ThumbprintSecondary** . |
|ClientCertificateThumbprints|Dit is een set van certificaten die u wilt installeren op de clients geverifieerd. U kunt een aantal verschillende clientcertificaten die zijn geïnstalleerd op de computers die u toegang wilt geven tot het cluster hebben. De vingerafdruk van elk certificaat in de variabele **CertificateThumbprint** ingesteld. Als u de **IsAdmin** ingesteld op *true*, is dit certificaat zijn geïnstalleerd op de client beheerder activiteiten op het cluster doen kan. Als de **IsAdmin** *ingesteld op false is*, kan de client met dit certificaat de toegestane acties voor gebruikerstoegangsrechten, meestal het kenmerk alleen-lezen alleen uitvoeren. Lees voor meer informatie over rollen [Role-based access control RBAC)](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|De algemene naam van het eerste clientcertificaat instellen voor de **CertificateCommonName**. De **CertificateIssuerThumbprint** is de blauwdruk voor de uitgever van dit certificaat. [Werken met certificaten](https://msdn.microsoft.com/library/ms731899.aspx) meer weten over de uitgevende instelling en de algemene namen worden gelezen.|
|HttpApplicationGatewayCertificate|Dit is een optionele certificaat dat kan worden opgegeven als u wilt uw Gateway Http-toepassing beveiligen. Stel reverseProxyEndpointPort in nodeTypes als u met dit certificaat.|

Hier is de clusterconfiguratie voorbeeld waarin de Cluster Server en Client-certificaten zijn verleend.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Krijgen de X.509 certificaten
Beveiligde communicatie binnen het cluster, moet u eerst verkrijgen x.509-certificaten voor de clusterknooppunten. Bovendien alleen verbinding met dit cluster gemachtigde computers gebruikers kunnen moet u downloaden en installeren van certificaten voor de clientcomputers.

Voor clusters die productie werklasten worden uitgevoerd, moet u een [Certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority) ondertekend x.509-certificaat voor het beveiligen van het cluster. Ga voor meer informatie over het verkrijgen van deze certificaten naar [procedure: een certificaat verkrijgen](http://msdn.microsoft.com/library/aa702761.aspx).

Voor clusters die u voor testdoeleinden gebruikt, kunt u een zelfondertekend certificaat gebruiken.

## <a name="optional-create-a-self-signed-certificate"></a>Optioneel: Een zelfondertekend certificaat maken
Een manier voor het maken van een zelf-ondertekend certificaat dat goed worden beveiligd is met behulp van de *CertSetup.ps1* -script in de map Service Fabric SDK in de map *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*. Dit bestand bewerken en deze gebruiken voor het maken van een certificaat met een passende naam.

Exporteer nu het certificaat naar een PFX-bestand met een wachtwoord beveiligd. U moet eerst de blauwdruk van het certificaat ophalen. Voer de toepassing certmgr.exe. Ga naar de map **Local Computer\Personal** en zoek het certificaat dat u zojuist hebt gemaakt. Dubbelklik op het certificaat te openen, selecteer het tabblad *Details* en blader naar het veld *vingerafdruk* . Kopieer de waarde in de PowerShell-opdracht, waarbij de spaties worden verwijderd.  Wijzig de waarde *$pswd* naar een veilig wachtwoord geschiktheid voor het beveiligen en het PowerShell uitvoeren:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

U kunt de details van een certificaat dat is geïnstalleerd op de computer de volgende PowerShell-opdracht uitvoeren:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Ook als u een Azure-abonnement hebt, voert u de sectie [toevoegen van certificaten aan de sleutel kluis](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Certificaten installeren
Wanneer u certificaten hebt, kunt u deze installeren op clusterknooppunten. De knooppunten moet de meest recente Windows PowerShell hebt geïnstalleerd op deze 3.x. U moet deze stappen op elk knooppunt voor Cluster- en Server en een secundaire certificaten.

1. De PFX-bestanden worden gekopieerd naar het knooppunt.
2. Open een venster PowerShell als beheerder en voer de volgende opdrachten. De *$pswd* vervangen door het wachtwoord dat u gebruikt om dit certificaat te maken. De *$PfxFilePath* vervangen door het volledige pad van het .pfx gekopieerd naar dit knooppunt.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Vervolgens moet u het toegangsbeheer op dit certificaat instellen zodat het proces Fabric-Service, die wordt uitgevoerd onder de account Netwerkservice, kan worden gebruikt door het volgende script uit te voeren. De blauwdruk van het certificaat en de 'NETWORK SERVICE' voor de serviceaccount opgeven. U kunt controleren of de ACL's op het certificaat juist gebruik van het hulpprogramma certmgr.exe en persoonlijke sleutels beheren bekijkt op het certificaat zijn.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Verbinding maken met ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation CurrentUser - StoreName mijn
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Verbinding maken met ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
