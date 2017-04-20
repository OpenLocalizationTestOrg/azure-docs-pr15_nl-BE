
<properties
   pageTitle="Maken van een beveiligde Service Fabric '-cluster met behulp van bronbeheer Azure | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe voor het instellen van een beveiligde Service Fabric '-cluster in Azure met Azure Resource Manager, Azure sleutel kluis en Azure Active Directory (AAD) voor clientverificatie."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Maken van een cluster Service Fabric in Azure Azure Resource Manager gebruiken

> [AZURE.SELECTOR]
- [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
- [Azure portal](service-fabric-cluster-creation-via-portal.md)

Dit is een stapsgewijze handleiding die u de stappen doorloopt voor het instellen van een beveiligde Azure Service Fabric '-cluster in Azure Azure Resource Manager gebruiken. Deze handleiding begeleidt u bij de volgende stappen uit:

 - Sleutel kluis voor het beheren van sleutels voor cluster- en beveiliging instellen.
 - Maak een beveiligde cluster in Azure Azure Resource Manager.
 - Verificatie van gebruikers met Azure Active Directory (AAD) voor Clusterbeheer.

Een veilige cluster is een cluster dat voorkomt onbevoegde toegang tot beheertaken uit te voeren, inclusief implementeren, bijwerken en verwijderen van toepassingen, services en de gegevens die ze bevatten. Een niet-beveiligde cluster is een cluster dat iemand verbinding kan op elk gewenst moment en management bewerkingen uitvoeren. Is het mogelijk te maken van een niet-beveiligde cluster, maar het is **ten zeerste aanbevolen voor het maken van een cluster beveiligen**. Een niet-beveiligde cluster **later kan niet worden beveiligd** - een nieuw cluster moet worden gemaakt.

De concepten zijn hetzelfde voor het maken van beveiligde clusters, of de clusters clusters van Linux of Windows-clusters zijn. Zie voor meer informatie en helper-scripts voor het maken van veilige Linux-clusters [maken veilig clusters op Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Log in op Azure
Deze handleiding maakt gebruik van [Azure PowerShell][azure-powershell]. Bij het starten van een nieuwe PowerShell-sessie, log in op uw account Azure en selecteer uw abonnement voor Azure opdrachten uitvoeren.

Log in op uw azure account:

```powershell
Login-AzureRmAccount
```

Selecteer uw abonnement:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Sleutel kluis instellen

Deze sectie helpt bij het maken van een kluis met sleutel voor een cluster Service Fabric in Azure en Service Fabric-toepassingen. Raadpleeg de [sleutel kluis aan de slag]voor een volledige handleiding op sleutel kluis[key-vault-get-started].

X.509-certificaten Fabric-service gebruikt voor het beveiligen van een cluster en functies voor beveiliging te bieden. Azure sleutel kluis wordt gebruikt voor het beheren van certificaten voor een Service Fabric clusters in Azure. Een cluster is geïmplementeerd in Azure, Azure resource degene die verantwoordelijk is voor het maken van clusters Fabric-Service haalt certificaten uit sleutel kluis als ze worden geïnstalleerd op het cluster VMs.

Het volgende diagram illustreert de relatie tussen de sleutel kluis, een Service Fabric '-cluster en de provider Azure bron die gebruikmaakt van certificaten die zijn opgeslagen in een kluis met sleutel bij het maken van een cluster:

![Installatie van het certificaat][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Een resourcegroep maken

De eerste stap is voor het maken van een resourcegroep voor sleutel kluis. Sleutel kluis te plaatsen in een eigen bronnengroep wordt aanbevolen. Hiermee kunt u verwijderen de resourcegroepen berekenen en opslag, met inbegrip van de de bronnengroep met de cluster-Service Fabric zonder verlies van uw sleutels en geheimen. De bronnengroep met de sleutel kluis moet zich in hetzelfde gebied, als het cluster dat wordt gebruikt.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Maak de sleutel kluis 

Een sleutel kluis in de nieuwe resourcegroep maken. De sleutel kluis **voor implementatie moet zijn ingeschakeld** zodat de Service Fabric resource provider om certificaten te halen uit het en installeren op clusterknooppunten:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Als u een bestaande sleutel kluis hebt, kunt u deze inschakelen voor Azure CLI-implementatie:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Certificaten toevoegen aan een sleutel kluis

Certificaten worden gebruikt in Fabric Service voor verificatie en codering voor de beveiliging van verschillende aspecten van een cluster en de bijbehorende toepassingen. Zie voor meer informatie over hoe u certificaten in Service Fabric gebruikt, [Fabric Service cluster security scenario's][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster- en certificaat (vereist) 

Dit certificaat is vereist voor het beveiligen van een cluster en toegang door onbevoegden te voorkomen. Biedt beveiliging op een paar manieren cluster:
 
 - **Cluster verificatie:** Communicatie voor Federatie cluster node naar node verifieert. Alleen knooppunten die hun identiteit met dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster.
 - **Server-verificatie:** Verifieert de eindpunten van de cluster management aan een management-client, zodat de client management weet dat het communiceert met het bestaande cluster. Dit certificaat biedt ook SSL voor de HTTPS-API voor beheer en Service Fabric Explorer via HTTPS.

Om deze doeleinden het certificaat moet voldoen aan de volgende eisen voldoen:

 - Het certificaat bevat een persoonlijke sleutel.
 - Het certificaat moet worden gemaakt voor sleuteluitwisseling, geëxporteerd naar een bestand Personal Information Exchange (.pfx).
 - De onderwerpnaam van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de Service Fabric '-cluster. Deze matchng is vereist om te voorzien in een SSL HTTPS management eindpunten en Service Fabric Explorer van het cluster. Krijgt u geen een SSL-certificaat van een certificeringsinstantie (CA) voor de `.cloudapp.azure.com` domein. U kunt een aangepaste domeinnaam voor uw cluster moet aanschaffen. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, hebben de onderwerpnaam van het certificaat moet overeenkomen met de aangepaste domeinnaam die wordt gebruikt voor het cluster.

### <a name="application-certificates-optional"></a>Certificaten van toepassing (optioneel)

Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster om veiligheidsredenen van toepassing. Voordat u het cluster maakt, kunt u de toepassing beveiliging scenario's waarvoor een certificaat moet worden geïnstalleerd op de knooppunten, zoals:

 - Codering en decodering van configuratiewaarden van toepassing
 - Codering van gegevens tussen knooppunten tijdens een replicatie 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Certificaten voor Azure resource provider opmaak

Persoonlijke bestanden (.pfx) toe te voegen en rechtstreeks via de sleutel kluis gebruikt. De provider Azure resource vereist echter sleutels worden opgeslagen in een speciale JSON-indeling met de PFX als een Base64-tekenreeks en het wachtwoord voor persoonlijke sleutel gecodeerd. Ten behoeve van deze vereisten, worden sleutels geplaatst in een JSON-string en vervolgens opgeslagen als *geheimen* in sleutel kluis.

Om dit proces te vergemakkelijken, een PowerShell-module is [beschikbaar op GitHub][service-fabric-rp-helpers]. Ga als volgt te werk om de module:

  1. Download de volledige inhoud van de repo in een lokale map. 
  2. De module in het venster PowerShell importeren:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
De `Invoke-AddCertToKeyVault` in deze module PowerShell automatisch wordt opgemaakt van de persoonlijke sleutel van een certificaat in een JSON-string en geüpload naar de sleutel kluis. Deze gebruiken om de cluster-certificaat en de toepassing van extra certificaten toevoegen aan de sleutel kluis. Herhaal deze stap voor elke extra certificaten die u wilt installeren in het cluster.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Het voorgaande zijn de sleutel kluis-vereisten voor het configureren van een Service Fabric cluster Resource Manager sjabloon waarmee certificaten voor de verificatie, verificatie en beveiliging van eindpunt wordt geïnstalleerd en elke toepassing extra beveiligingsfuncties die x.509-certificaten te gebruiken. Nu hebt u nu de volgende instellingen in Azure:

 - Resourcegroep sleutel kluis
   - Sleutel kluis
     - Certificaat voor serververificatie cluster
     - Certificaten van toepassing

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory instellen voor clientverificatie

AAD kunnen organisaties (huurders genoemd) voor het beheren van toegang tot toepassingen die zijn onderverdeeld in toepassingen met een aanmelding web gebaseerde gebruikersinterface en toepassingen met een native Clientervaring. In dit document, nemen we aan dat u een huurder al hebt gemaakt. Zo niet, lees eerst [het verkrijgen van een huurder Azure Active Directory][active-directory-howto-tenant].

Een cluster Service Fabric biedt verschillende ingangspunten voor de functionaliteit, met inbegrip van de web-based [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] en [Visual Studio][service-fabric-manage-application-in-visual-studio]. Hierdoor maakt u twee AAD toepassingen toegang tot het cluster, één webtoepassing en één van de oorspronkelijke toepassing.

Enkele van de stappen die betrokken zijn bij AAD configureren met een cluster Service Fabric vereenvoudigen, hebben wij een set Windows PowerShell-scripts.

>[AZURE.NOTE] U moet uitvoeren van deze stappen *voordat* u het cluster maakt dat in de gevallen waarin de scripts clusternamen en eindpunten verwacht, moet deze de geplande waarden niet zijn die u al hebt gemaakt.

1. [De scripts downloaden] [ sf-aad-ps-script-download] op uw computer.

2. Met de rechtermuisknop op het zip-bestand, kiest u **Eigenschappen**, vervolgens Vink het selectievakje **deblokkeren** en toepassen.

3. Pak het zip-bestand.

4. Uitvoeren van `SetupApplications.ps1`, de TenantId, de clusternaam en de WebApplicationReplyUrl die als parameters. Bijvoorbeeld:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    U vindt de **TenantId** door het uitvoeren van de opdracht PowerShell '' Get-AzureSubscription'' '. Hierdoor wordt de **TenantId** voor elk abonnement weergegeven.

    De **clusternaam** is gebruikt als voorvoegsel voor de AAD toepassingen die zijn gemaakt door het script. Het hoeft niet overeen met de werkelijke clusternaam precies zoals deze is alleen bedoeld voor het gemakkelijker voor u AAD artefacten toewijzen aan de cluster-Service Fabric dat ze worden gebruikt met.

    De **WebApplicationReplyUrl** is het standaardeindpunt dat AAD terug naar uw gebruikers na voltooiing van het aanmeldingsproces. U moet dit instellen op de Fabric-Explorer-Service-eindpunt voor uw cluster, die standaard is:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Moet u zich aanmeldt bij een account die beheerdersrechten heeft voor de huurder AAD. Zodra u dat doet, gaat het script door websites en toepassingen voor uw Service Fabric '-cluster maken. Als u naar de toepassingen van de huurder in [Azure klassieke portal kijkt][azure-classic-portal], ziet u twee nieuwe vermeldingen:

    - *Clusternaam*\_Cluster
    - *Clusternaam*\_Client

    Het script drukt de Json Azure Resource Manager nodig bij het maken van het cluster in het volgende gedeelte laten de PowerShell-venster geopend.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Maak een sjabloon Service Fabric cluster Resource Manager

In dit gedeelte worden de uitvoer van de voorgaande PowerShell-opdrachten in een sjabloon Service Fabric cluster Resource Manager gebruikt.

Monster Resource Manager sjablonen zijn beschikbaar in de [Sjabloongalerie van Azure snel starten op GitHub][azure-quickstart-templates]. Deze sjablonen kunnen worden gebruikt als uitgangspunt voor de sjabloon voor het cluster. 

### <a name="create-the-resource-manager-template"></a>Maak de sjabloon Resource Manager

Deze handleiding maakt gebruik van het [beveiligde cluster 5-node] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] van de voorbeeldsjabloon en sjabloonparameters. Download `azuredeploy.json` en `azuredeploy.parameters.json` op de computer en opent u beide bestanden in uw favoriete tekstverwerker.

### <a name="add-certificates"></a>Certificaten toevoegen

Certificaten worden toegevoegd aan een cluster Resource Manager sjabloon door te verwijzen naar de sleutel kluis met de toetsen van het certificaat. Het wordt aanbevolen dat deze sleutel kluis waarden worden geplaatst in een bronnenbeheerder parameters sjabloonbestand behouden de bronnenbeheerder sjabloon bestand herbruikbare en vrij van waarden die specifiek zijn voor een implementatie.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Alle certificaten toevoegen aan de osProfile VMSS

Elk certificaat dat moet worden geïnstalleerd in het cluster moet worden geconfigureerd in de sectie osProfile van het middel VMSS (Microsoft.Compute/virtualMachineScaleSets). Dit geeft de provider resource in het certificaat te installeren op de VMs. Dit omvat het certificaat cluster als een toepassing beveiligingscertificaten die u wilt gebruiken voor uw toepassingen:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Cluster-certificaat Fabric-Service configureren

Het verificatiecertificaat cluster moet ook worden geconfigureerd in de clusterbron Fabric-Service (Microsoft.ServiceFabric/clusters) en de extensie Fabric-Service voor VMSS in de bron VMSS. Dit kan de provider van de resource Fabric-Service te configureren voor gebruik voor cluster-verificatie en serververificatie voor beheer van eindpunten.

##### <a name="vmss-resource"></a>VMSS bron:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Service Fabric resource:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>AAD config invoegen

De eerder gemaakte AAD-configuratie kan worden ingevoegd direct in uw sjabloon Resource Manager, het is echter raadzaam de waarden in de parameters eerst naar een parameterbestand te houden de bronnenbeheerder sjabloon opnieuw te gebruiken en vrij van waarden specifiek aan een distributie uitpakken.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < een "configureren-arm" ></a>Sjabloonparameters Resource Manager configureren

Ten slotte de uitvoerwaarden van de sleutel kluis en AAD PowerShell-opdrachten gebruiken voor het vullen van het parameterbestand:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Nu hebt u nu de volgende:

 - Resourcegroep sleutel kluis
    - Sleutel kluis
    - Certificaat voor serververificatie cluster
    - Gegevens uitwisselen certificaat
 - Azure Active Directory huurder 
    - Toepassing voor beheer- en Service-Fabric Explorer AAD
    - AAD van toepassing voor het beheer van native client
    - Gebruikers met de rol toegewezen 
 - Sjabloon voor service Fabric cluster Resource Manager
    - Certificaten die zijn geconfigureerd met behulp van sleutel kluis
    - Azure Active Directory geconfigureerd 

In het volgende diagram ziet u waar sleutel kluis en AAD configuratie past in uw sjabloon Resource Manager.

![Toewijzing van Resource Manager afhankelijkheid][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Het cluster maken

U bent nu gereed voor het maken van het cluster met behulp van [ARM implementatie][resource-group-template-deploy].

#### <a name="test-it"></a>Het testen

De volgende PowerShell-opdracht voor het testen van uw sjabloon Resource Manager met een parameterbestand met gebruiken:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Deze implementeren

Als de bronnenbeheerder sjabloon test doorstaat, u het volgende PowerShell implementatie van uw sjabloon Resource Manager met een parameterbestand met:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Gebruikers toewijzen aan rollen

Als u de toepassingen voor het cluster hebt gemaakt, moet u uw gebruikers toewijzen aan de functies die worden ondersteund door de Service Fabric: alleen-lezen en beheerder. U kunt dit doen met behulp van de [portal voor Azure klassieke][azure-classic-portal].

1. Ga naar de huurder en kies toepassingen.
2. Kies de webtoepassing die een naam heeft zoals `myTestCluster_Cluster`.
3. Klik op het tabblad gebruikers.
4. Kies een gebruiker wilt toewijzen en klik op de knop **toewijzen** aan de onderkant van het scherm.

    ![Gebruikers toewijzen aan rollen][assign-users-to-roles-button]

5. Selecteer de rol toe te wijzen aan de gebruiker.

    ![Gebruikers toewijzen aan rollen][assign-users-to-roles-dialog]

>[AZURE.NOTE] Zie [op rollen gebaseerde toegangscontrole voor Service Fabric-clients](service-fabric-cluster-security-roles.md)voor meer informatie over rollen stof Service.

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Beveiligde clusters maken op Linux

Om het proces gemakkelijker te maken, een script helper is geleverd [hier](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Voor deze helper script gebruikt, wordt ervan uitgegaan dat u al hebt Azure CLI geïnstalleerd en in het pad is. Zorg dat het script heeft machtigingen uit te voeren door het uitvoeren van `chmod +x cert_helper.py` na het downloaden. De eerste stap is het aan te melden bij uw Azure-account met behulp van de CLI met de `azure login` opdracht. Na het aanmelden bij uw account Azure, gebruikt u de helper met het CA-certificaat, zoals in de volgende opdracht:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Deze opdracht retourneert de volgende drie reeksen als de uitvoer: 

1. Een SourceVaultID wordt de ID voor de nieuwe KeyVault ResourceGroup deze voor u gemaakt. 

2. Een CertificateUrl voor toegang tot het certificaat.

3. Een CertificateThumbprint wordt gebruikt voor verificatie.


In het volgende voorbeeld ziet u hoe u de opdracht:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
De bovenstaande opdracht uitvoeren biedt u de drie reeksen als volgt:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 De onderwerpnaam van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de Service Fabric '-cluster. Dit is nodig om te voorzien in een SSL HTTPS management eindpunten en Service Fabric Explorer van het cluster. Krijgt u geen een SSL-certificaat van een certificeringsinstantie (CA) voor de `.cloudapp.azure.com` domein. U kunt een aangepaste domeinnaam voor uw cluster moet aanschaffen. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt van de naam van de certificaathouder, moet overeenkomen met de naam van het aangepaste domein gebruikt voor het cluster.

Dit zijn de gegevens die nodig zijn voor het maken van een beveiligde fabric '-cluster (zonder AAD) zoals beschreven bij [Sjabloonparameters Resource Manager configureren](#configure-arm). U kunt verbinding maken met het cluster beveiligen via de instructies op de [verificatie van de clienttoegang tot een cluster](service-fabric-connect-to-secure-cluster.md). Linux voorbeeld clusters ondersteunen AAD-verificatie niet. U kunt client en beheer rollen zoals beschreven in de sectie [rollen aan gebruikers toewijzen](#assign-roles). Wanneer u client voor het beheer en rollen voor een voorbeeld van Linux cluster opgeeft, moet u vingerafdrukken certificaat voor verificatie (in plaats van de naam certificaathouder, aangezien er geen validatie keten of intrekking wordt uitgevoerd in deze preview-versie).


Als u een zelfondertekend certificaat gebruiken wilt voor het testen, kunt u hetzelfde script genereert een zelf-ondertekend certificaat en uploaden naar KeyVault, door middel van de vlag `ss` in plaats van het certificaatpad en de naam van het certificaat. Zie bijvoorbeeld de volgende opdracht voor het maken en uploaden van een zelfondertekend certificaat:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Deze opdracht retourneert de dezelfde drie reeksen, SourceVault, CertificateUrl en CertificateThumbprint, die wordt gebruikt voor het maken van een veilige Linux cluster, samen met de locatie waar het zelfondertekende certificaat is geplaatst. U moet verbinding maken met het cluster zelf-ondertekend certificaat.  U kunt verbinding maken met het cluster beveiligen via de instructies op de [verificatie van de clienttoegang tot een cluster](service-fabric-connect-to-secure-cluster.md). De onderwerpnaam van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de Service Fabric '-cluster. Dit is nodig om te voorzien in een SSL HTTPS management eindpunten en Service Fabric Explorer van het cluster. Krijgt u geen een SSL-certificaat van een certificeringsinstantie (CA) voor de `.cloudapp.azure.com` domein. U kunt een aangepaste domeinnaam voor uw cluster moet aanschaffen. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt van de naam van de certificaathouder, moet overeenkomen met de naam van het aangepaste domein gebruikt voor het cluster.

De parameters die door het script helper kunnen worden ingevuld in de portal, zoals beschreven in de sectie [een cluster in de Azure portal maken](service-fabric-cluster-creation-via-portal.md#create-cluster-portal).

## <a name="next-steps"></a>Volgende stappen

U hebt nu een cluster beveiligen met Azure Active Directory bieden management-verificatie. Volgende, [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en informatie over het [beheren van toepassingen geheimen](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory in te stellen voor de verificatie van clientcomputers oplossen

Als u een probleem tijdens het instellen van Azure Active Directory voor clientverificatie, bekijk de volgende suggestings voor mogelijke oplossingen.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Service-structuur wordt gevraagd voor het certificaat te selecteren

#### <a name="problem"></a>Probleem

Na aanmelding op de aanmeldingspagina AAD in Service Fabric Explorer, de browser terug naar de introductiepagina, maar vraagt een dialoogvenster voor het selecteren van een certificaat.

![SFX certificaat selecteren dialoogvenster][sfx-select-certificate-dialog]

#### <a name="reason"></a>Reden

De gebruiker is niet een rol in de toepassing van AAD cluster toegewezen. Dus mislukt AAD verificatie bij Service Fabric '-cluster. Service Fabric Explorer valt terug op verificatie.

#### <a name="solution"></a>Oplossing

Volg de instructies voor het instellen van AAD en rollen toewijzen. Ook verdient 'Gebruiker toewijzing vereist voor ACCESS APP' zijn ingeschakeld als `SetupApplications.ps1` heeft.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Verbinding maken met PowerShell mislukt met fout: de opgegeven referenties zijn ongeldig

#### <a name="problem"></a>Probleem

Wanneer gebruikt u PowerShell verbinding maken met een cluster met behulp van de beveiligingsmodus 'AzureActiveDirectory', na aanmelding op de aanmeldingspagina AAD, verbinding mislukt met fout: de opgegeven referenties zijn ongeldig weergegeven.

#### <a name="solution"></a>Oplossing

Hetzelfde als hierboven.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Service Fabric Explorer ondertekenen in fout: AADSTS50011

#### <a name="problem"></a>Probleem

Na aanmelding op AAD aanmeldingspagina in Service Fabric Explorer, retourneert pagina ondertekenen in fout - AADSTS50011: het antwoordadres &lt;url&gt; komt niet overeen met de antwoordadressen geconfigureerd voor de toepassing: &lt;guid&gt;. 

![Antwoordadres SFX niet overeen met][sfx-reply-address-not-match]

#### <a name="reason"></a>Reden

De cluster(web) toepassing die Service Fabric Explorer pogingen om te verifiëren aan de hand van AAD vertegenwoordigt, als onderdeel van de aanvraag biedt het de retour-URL voor omleiding. Maar staat niet in de lijst van AAD antwoord-URL.

#### <a name="solution"></a>Oplossing

Url van Service Fabric Explorer toevoegen aan de antwoord-URL in het tabblad configureren van cluster(web)-toepassing of een van de items in de lijst vervangen. Vervolgens opslaan.

![Antwoord van de webtoepassing][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Kan ik de dezelfde huurder AAD voor meerdere clusters gebruiken?

#### <a name="answer"></a>Antwoord

Ja. Maar denk eraan dat de URL van Service Fabric Explorer toevoegt aan uw toepassing cluster(web) is die anders Service Fabric Explorer werkt niet.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Waarom moet ik nog steeds servercertificaat terwijl AAD ingeschakeld?

#### <a name="answer"></a>Antwoord

Wederzijdse verificatie wordt uitgevoerd door FabricClient en FabricGateway. In het geval van AAD verificatie biedt AAD integratie de identiteit van de client aan de server en de servercertificaat wordt gebruikt om te controleren of de serveridentiteit. Voor meer informatie over de werking van certificaten op Service Fabric [x.509-certificaten en Fabric Service] controleren[x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png