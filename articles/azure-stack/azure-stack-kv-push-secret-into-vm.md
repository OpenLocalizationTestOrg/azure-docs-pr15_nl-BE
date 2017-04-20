<properties
    pageTitle="Implementeer een VM met een certificaat met Azure Stack sleutel kluis | Microsoft Azure"
    description="Meer informatie over hoe een VM implementeren en invoeren van een certificaat van Azure Stack sleutel kluis"
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
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>VMs maken en certificaten die zijn opgehaald uit de kluis met sleutel opnemen

In Azure-Stack VMs via Azure Resource Manager worden geïmplementeerd en certificaten kunnen nu worden opgeslagen in Azure Stack sleutel kluis. Vervolgens duwt Azure Stack (resource-provider worden specifieke Microsoft.Compute) ze in uw VMs wanneer het VMs worden geïmplementeerd. Certificaten kunnen worden gebruikt in veel scenario's, met inbegrip van SSL, codering en verificatie op basis van certificaten.

Met deze methode kunt u het certificaat veilig houden. Het is nu niet in de VM-afbeelding, of in de configuratiebestanden van de toepassing of andere onveilige locatie. Juiste-beleid voor de sleutel kluis instelt, kunt u ook bepalen die toegang tot uw certificaat haalt. Een ander voordeel is dat u uw certificaten op één plaats in Azure Stack sleutel kluis kunt beheren.

Hier volgt een kort overzicht van het proces:

-   U moet een certificaat in de PFX-indeling.

-   Maak een sleutel kluis (met een sjabloon of het volgende voorbeeldscript).

-   Zorg ervoor dat u de schakeloptie EnabledForDeployment hebt ingeschakeld.

-   Het certificaat uploaden als een geheim.

## <a name="deploying-vms"></a>VMs implementeren

Dit voorbeeldscript wordt een sleutel kluis gemaakt en vervolgens een certificaat dat is opgeslagen in het .pfx-bestand in een lokale map, als een geheim om de sleutel wordt opgeslagen.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Het eerste deel van het script leest het .pfx-bestand en als het een JSON-object met het bestand inhoud base64-gecodeerde winkels. De JSON-object wordt ook base64-gecodeerde.

Vervolgens maakt u een nieuwe bronnengroep en maakt vervolgens een sleutel kluis. Let op de laatste parameter met de opdracht Nieuw AzureKeyVault '-EnabledForDeployment', waarmee toegang wordt verleend aan Azure (specifiek voor de Microsoft.Compute resource-provider) lezen de geheimen van de sleutel kluis voor implementaties.

De laatste opdracht slaat gewoon het base64-gecodeerde JSON-object in de sleutel kluis als een geheim.

Hier is een voorbeeld van de uitvoer uit het voorgaande script:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

We kunnen nu een VM-sjabloon te implementeren. Opmerking de URI van het geheim van de uitvoer (zoals u elders in de voorafgaande uitvoer in het groen).

U moet een sjabloon die hier te vinden. De parameters van speciaal belang (naast de gebruikelijke parameters voor VM) zijn de naam van de kluis, de resourcegroep van de kluis en de geheime URI. U kunt natuurlijk ook het downloaden van GitHub en zo nodig wijzigen.

Wanneer deze VM is geïmplementeerd, injects Azure het certificaat in de VM.
Certificaten in het .pfx-bestand worden toegevoegd in Windows, met de persoonlijke sleutel kan niet worden geëxporteerd. Het certificaat wordt toegevoegd aan de locatie van het certificaat LocalMachine, met het certificaatarchief waarin voor de gebruiker. Op Linux, het certificaatbestand is geplaatst in de map /var/lib/waagent met de naam &lt;UppercaseThumbprint&gt;.crt voor het X509 bestand, en &lt;UppercaseThumbprint&gt;.prv voor de persoonlijke sleutel.
Beide bestanden zijn .pem opgemaakt.

De toepassing zoekt naar het certificaat met behulp van de vingerafdruk meestal en hoeft niet gewijzigd.

## <a name="retiring-certificates"></a>Certificaten intrekken


In de vorige sectie, we u laten zien hoe u een nieuw certificaat aan uw bestaande VMs duwen. Maar het oude certificaat is nog steeds in de VM en kan niet worden verwijderd. Voor extra beveiliging kunt u het kenmerk voor een oude geheim op 'Uitgeschakeld', zodat zelfs als een oude sjabloon probeert te maken van een VM met deze oude versie van het certificaat, wordt. Hier ziet u hoe u een bepaalde geheime versie moet worden uitgeschakeld kunt instellen:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Conclusie


Met deze nieuwe methode wordt kan het certificaat gescheiden worden gehouden van de VM-afbeelding of de nettolading van toepassing. We hebben dus blootstelling van één punt verwijderd.

Het certificaat kan ook worden vernieuwd en geüpload naar de sleutel kluis zonder opnieuw op te bouwen de VM-afbeelding of het implementatiepakket toepassing. De toepassing moet nog maar met de nieuwe URI voor dit nieuwe certificaatversie worden geleverd.

Door het scheiden van het certificaat van de VM of de nettolading van toepassing, hebben we nu het aantal medewerkers met directe toegang tot het certificaat verminderd.

Een bijkomend voordeel hebt u nu één handige locatie in een sleutel kluis voor het beheren van alle certificaten, inclusief de alle versies die zijn geïmplementeerd in de tijd.

## <a name="next-steps"></a>Volgende stappen

[Implementeren van een VM met een wachtwoord sleutel kluis](azure-stack-kv-deploy-vm-with-secret.md)

[Een toepassing toegang tot de sleutel kluis](azure-stack-kv-sample-app.md)