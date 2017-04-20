<properties
    pageTitle="WinRM toegang instellen voor virtuele Machines in Azure Resource Manager | Microsoft Azure"
    description="WinRM toegang voor gebruik met een bronnenbeheerder Azure virtuele machine instellen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>WinRM toegang instellen voor virtuele Machines in Azure Resource Manager

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM in Azure Service Management vs Azure Resource Manager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

* Zie dit [artikel](../azure-resource-manager/resource-group-overview.md) voor een overzicht van de Azure Resource Manager
* Zie dit [artikel](../resource-manager-deployment-model.md) voor de verschillen tussen Azure Service Management en Azure Resource Manager,

Het belangrijkste verschil bij het instellen van configuratie tussen de twee stapels WinRM is hoe het certificaat wordt geïnstalleerd op de VM. De certificaten worden in de stack Azure Resource Manager gemodelleerd als bronnen die worden beheerd door de sleutel kluis Resource Provider. Daarom moet de gebruiker naar een kluis met sleutel uploaden voordat u deze in een VM en leveren hun eigen certificaat.

Hier zijn de stappen die u moet nemen om in te stellen een VM met WinRM connectiviteit

1. Maak een sleutel kluis
2. Een zelfondertekend certificaat maken
3. De zelf-ondertekend certificaat uploaden naar sleutel kluis
4. Ophalen van de URL van uw zelf-ondertekende certificaat in de sleutel kluis
5. Verwijst naar de URL van uw zelf-ondertekende certificaten tijdens het maken van een VM

## <a name="step-1-create-a-key-vault"></a>Stap 1: Maak een sleutel kluis

U kunt de onderstaande opdracht voor het maken van de sleutel kluis

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Stap 2: Een zelfondertekend certificaat maken
U kunt een zelfondertekend certificaat gebruiken deze PowerShell-script maken

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Stap 3: Uw zelf-ondertekend certificaat uploaden naar de sleutel kluis

Voordat het certificaat uploaden om de sleutel in stap 1 hebt gemaakt, moet deze omgezet in een indeling die de provider van de resource Microsoft.Compute goed begrijpt. De onderstaande PowerShell script, kunt u dat doen

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Stap 4: Haal de URL voor uw zelf-ondertekende certificaat in de sleutel kluis

De bron Microsoft.Compute provider moet een URL naar het geheim van de sleutel kluis tijdens het inrichten van de VM. Hiermee kunt de provider Microsoft.Compute resource het geheim downloaden en het overeenkomstige certificaat maken op de VM.

>[AZURE.NOTE]De URL van het geheim moet bevatten, alsmede de versie. Een voorbeeld-URL ziet er onder https://contosovault.vault.azure.net:443/geheimen/contososecret/01h9db0df2cd4300a20ence585a6s7ve


#### <a name="templates"></a>Sjablonen

Krijgt u de koppeling naar de URL in de sjabloon met behulp van de onderstaande code

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell

Kun je deze URL met behulp van de onderstaande PowerShell-opdracht

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Stap 5: Verwijzen naar de URL van uw zelf-ondertekende certificaten tijdens het maken van een VM

#### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Het certificaat wordt tijdens het maken van een VM via sjablonen verwezen in de geheimen en de winRM-sectie, zoals hieronder:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Voorbeeldsjabloon voor het bovenstaande vindt u hier op [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

De broncode voor deze sjabloon kunt u vinden op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Stap 6: Verbinding maken met de VM
Voordat u verbinding kunt maken voor het VM moet u controleren of wordt de computer geconfigureerd voor extern beheer van WinRM. PowerShell als beheerder starten en uitvoeren van de onder de opdracht om te controleren of u bent ingesteld.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Mogelijk moet u controleren of dat de WinRM-service wordt uitgevoerd als het bovenstaande niet werkt. U kunt doen met die`Get-Service WinRM`

Als de installatie is voltooid, kunt u de VM met behulp van de onderstaande opdracht

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate