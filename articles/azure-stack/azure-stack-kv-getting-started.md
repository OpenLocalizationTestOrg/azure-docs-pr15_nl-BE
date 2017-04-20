<properties
    pageTitle="Aan de slag met Azure Stack sleutel kluis | Microsoft Azure"
    description="Aan de slag met Azure Stack sleutel kluis"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Aan de slag met sleutel kluis

Deze sectie beschrijft de stappen voor het maken van een kluis, sleutels en geheimen te beheren, evenals toestaan dat gebruikers of toepassingen aan te roepen van de bewerkingen in de kluis in Azure stapel. De volgende stappen wordt ervan uitgegaan dat een huurder abonnement bestaat en KeyVault service is geregistreerd binnen dit abonnement. Alle opdrachten in het voorbeeld zijn gebaseerd op de KeyVault-cmdlets beschikbaar als onderdeel van de Azure PowerShell SDK.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>De huurder abonnement voor kluis bewerkingen inschakelen 

Voordat u bewerkingen op de kluis uitgeven kunt, moet u ervoor zorgen dat uw abonnement is ingeschakeld voor bewerkingen van de kluis. U kunt dat controleren door de volgende PowerShell-opdracht:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

De uitvoer van de bovenstaande opdracht moet verslag "Geregistreerd" voor de "Registratie" status van elke rij.

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Als dat niet het geval is, moet u de volgende opdracht om de service KeyVault binnen uw abonnement registreren aanroepen:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

En de volgende is de uitvoer van de opdracht:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Als u het foutbericht: '*het abonnement is niet geregistreerd bij Azure sleutel kluis*' bij het aanroepen van KeyVault-cmdlets bevestigen dat u de provider KeyVault resource per bovenstaande instructies hebt ingeschakeld.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Maken van een harde container (kluis) in Azure Stack opslaan en beheren van cryptografische sleutels en geheimen

Een huurder wilt maken in een kluis, te eerst een resourcegroep maken. De volgende PowerShell-opdrachten maken een resourcegroep en klik vervolgens op een kluis in de betreffende bronnengroep. Het voorbeeld bevat ook de standaard uitvoer van deze cmdlet.

### <a name="creating-a-resource-group"></a>Een resourcegroep maken:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Uitvoer:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Een kluis maken:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Uitvoer:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
De uitvoer van deze cmdlet worden de eigenschappen van de sleutel kluis die u net hebt gemaakt. De twee belangrijkste eigenschappen zijn:

-   **Naam van de kluis**: In het voorbeeld is dit **vault010**. U gebruikt deze naam voor een andere sleutel kluis cmdlets.

-   **Kluis URI**: In het voorbeeld is dit https://vault010.vault.azurestack.local. Toepassingen die gebruikmaken van de kluis via de REST API, moeten deze URI gebruiken.

Uw Azure account mag zich nu alle bewerkingen uitvoeren op deze sleutel kluis. Nog is niemand anders.


## <a name="operating-on-keys-and-secrets"></a>Werkzaam op toetsen en geheimen

Na het maken van een kluis, volg de onderstaande stappen te volgen voor het maken van sleutels en geheimen beheren:

### <a name="creating-a-key"></a>Maken van een sleutel

Gebruik wilt maken van een sleutel, de **Add-AzureKeyVaultKey** per het voorbeeld hieronder. Na de succesvolle maken, wordt de cmdlet details van de zojuist gemaakte sleutel uitvoeren.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Dit is het resultaat van de cmdlet *Add-AzureKeyVaultKey* :

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Nu kunt u verwijzen naar deze sleutel die u hebt gemaakt of geüpload naar Azure sleutel kluis, met behulp van de URI. **Sleutels-https://vault010.vault.azurestack.local:443/keyVaultKeyName001** gebruiken om altijd de huidige versie; en **https://vault010.vault.azurestack.local:443/toetsen/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff** met deze specifieke versie.

### <a name="retrieving-a-key"></a>Een sleutel ophalen

Gebruik de **Get-AzureKeyVaultKey** voor het ophalen van een sleutel en de bijbehorende details per in het volgende voorbeeld:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

Hieronder ziet u de uitvoer van de Get-AzureKeyVaultKey

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Een geheim instellen

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Uitvoer

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Een geheim ophalen

    Get-AzureKeyVaultSecret -VaultName vault010

Uitvoer

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Uw sleutel kluis en code of geheim is nu gereed om toepassingen te gebruiken.
Toepassingen wilt gebruiken, moet u machtigen.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>De toepassing voor het gebruik van de sleutel of een geheim toestaan 

Als u wilt toestaan dat de toepassing toegang tot de sleutel of een geheim in de kluis, gebruikt u de cmdlet Set -**AzureRmKeyVaultAccessPolicy** .

Bijvoorbeeld als de naam van uw kluis *ContosoKeyVault is* en de toepassing die u wilt toestaan dat een *Client-ID* van de *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*heeft en u toestaan dat de toepassing wilt voor het decoderen en ondertekenen met sleutels in uw kluis, het volgende uitvoeren:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Als u toestaan dat dezelfde toepassing wilt lezen geheimen in de kluis, het volgende uitvoeren:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Volgende stappen
[Implementeren van een VM met een wachtwoord sleutel kluis](azure-stack-kv-deploy-vm-with-secret.md)

[Implementeren van een VM met een certificaat sleutel kluis](azure-stack-kv-push-secret-into-vm.md)