<properties
   pageTitle="Codering voor Windows en Linux IaaS VMs Azure schijf | Microsoft Azure"
   description="Het papier voor een overzicht van Microsoft Azure schijf codering voor Windows en Linux IaaS VMs."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="krkhan"/>


#<a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Codering voor Windows en Linux IaaS VMs Azure schijf

Microsoft Azure hecht sterk aan ervoor te zorgen dat uw privacy, gegevens-soevereiniteit en gegevens kunt u de controle uw Azure gegevens via een reeks gehost technologieën om te coderen geavanceerde, controleren en beheren van coderingssleutels, access control & controle van gegevens. Dit biedt de flexibiliteit om te kiezen van de oplossing die het beste voldoet aan hun behoeften Azure klanten. In dit artikel, we maakt u kennis met een nieuwe, technologieoplossing 'Azure schijf codering voor Windows en Linux IaaS VM van' als u wilt beveiligen en beschermen van uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving van verbintenissen. Het witboek biedt gedetailleerde instructies over het gebruik van de schijf Azure coderingsfuncties met inbegrip van de ondersteunde scenario's en de gebruiker ondervindt.

**Opmerking**: bepaalde aanbevelingen kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of Resourcegebruik berekenen wat resulteert in een extra licentie of abonnement kosten.

## <a name="overview"></a>Overzicht

Azure schijfversleuteling is een nieuwe mogelijkheid waarmee u de virtuele machine Windows en Linux IaaS schijven te versleutelen. Azure schijf codering maakt gebruik van de industrie standaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) -functie van Windows en de functie [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) van Linux voor codering van het volume voor het besturingssysteem en de gegevensschijven. De oplossing is geïntegreerd met [Azure sleutel](https://azure.microsoft.com/documentation/services/key-vault/) kluis om te bepalen en de schijf coderingssleutels en geheimen in je abonnement sleutel kluis, terwijl ervoor te zorgen dat alle gegevens in de virtuele machine-schijven worden gecodeerd in rust in Azure opslag beheren.

Codering voor Windows en Linux IaaS VMs Azure schijf is nu **Beschikbaar** in alle Azure openbare gebieden voor standaard VMs en VMs met premium-opslagruimte.

### <a name="encryption-scenarios"></a>Scenario's voor codering

De codering van Azure schijf oplossing ondersteunt de volgende scenario's van klant:

- Nieuwe IaaS VMs gemaakt op basis van vooraf gecodeerde VHD en coderingssleutels-codering inschakelen
- Nieuwe IaaS VMs gemaakt op basis van de afbeeldingen Azure-codering inschakelen
- Bestaande IaaS VMs uitgevoerd in Azure-codering inschakelen
- Codering in Windows IaaS VMs uitschakelen
- Codering op gegevensstations voor Linux IaaS VMs uitschakelen

De oplossing ondersteunt de volgende voor VMs IaaS wanneer ingeschakeld in Microsoft Azure:

- Integratie met Azure sleutel kluis
- Standaard-tier VMs - [A, D, DS, G, enz GS serie IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)
- Voor Windows en Linux IaaS VMs-codering inschakelen
- Codering op stations met OS en gegevens voor Windows IaaS VMs uitschakelen
- Codering op gegevensstations voor Linux IaaS VMs uitschakelen
- IaaS VMs Windows Client besturingssysteem-codering inschakelen
- Op volumes met koppelpaden zodat-codering inschakelen
- Schakel codering in voor Linux VMs geconfigureerd met softwarematige RAID-systeem 
- Gebruik codering voor Windows VMs geconfigureerd met opslagruimtes
- Alle Azure openbare gebieden worden ondersteund.

De oplossing ondersteunt niet de volgende scenario's, voorzieningen en technologie in de release:

- Basic laag IaaS VMs
- Codering op OS schijf voor Linux IaaS VMs uitschakelen
- IaaS VMs gemaakt met klassieke methode van VM maken
- Integratie met uw on-premises Key Management Service
- Windows Server 2016 technische Preview wordt niet ondersteund in deze release
- Azure bestanden (Azure bestandsshare) netwerkbestandssysteem (NFS), dynamische volumes, Windows VMs geconfigureerd met softwarematige RAID-systemen


### <a name="encryption-features"></a>Coderingsfuncties

Als u inschakelen en implementeren van Azure schijfversleuteling voor Azure IaaS VMs, zijn de volgende functies ingeschakeld, afhankelijk van de configuratie:

- Het coderen van het volume met het besturingssysteem beveiligen opstartvolume rusten in de opslag van de klant
- Codering van gegevens volume/s ter bescherming van de hoeveelheden gegevens in rust in de opslag van de klant
- Codering op stations met OS en gegevens voor Windows IaaS VMs uitschakelen
- Codering op gegevensstations voor Linux IaaS VMs uitschakelen
- Bescherming van de coderingssleutels en geheimen in Azure sleutel kluis abonnement klant
- Rapportage over de status van de gecodeerde IaaS VM codering
- Verwijderen van de schijf versleuteling configuratie-instellingen van de virtuele machine van IaaS

De Azure schijfversleuteling voor VMS IaaS voor Windows en Linux-oplossing bevat de schijf versleuteling-extensie voor Windows, de extensie van schijf versleuteling voor Linux, schijf versleuteling PowerShell cmdlets, schijf versleuteling CLI-cmdlets en schijf versleuteling Azure Resource Manager-sjablonen. De oplossing Azure schijf versleuteling wordt op IaaS VMs Windows-of Linux-besturingssysteem ondersteund. Zie de voorwaarden hieronder voor meer informatie over de ondersteunde besturingssystemen.

**Opmerking**: Er is geen extra kosten voor VM-schijven met Azure schijfversleuteling te coderen.

### <a name="value-proposition"></a>Toegevoegde waarde

De oplossing Azure codering Schijfbeheer kunnen de volgende zakelijke behoeften in de cloud:

-   De IaaS VM worden in rust industrie standaard coderingstechnologie organisatorische beveiliging en naleving eisen beveiligd.
-   Boot onder klant IaaS VM van sleutels en het beleid gecontroleerd en ze hun gebruik in een sleutel kluis kunnen controleren.


### <a name="encryption-workflow"></a>Workflow voor codering

Hoog niveau vereiste stappen voor het inschakelen van codering voor Windows en Linux VM van schijf zijn:

1. Klant kiest een scenario voor de codering van de bovenstaande scenario's voor codering
2. Klant kiest in de codering van de schijf via de Azure schijf versleuteling Resource Manager sjabloon of PS-cmdlets of CLI-opdracht inschakelen en geeft de configuratie van codering

    - De klant gecodeerd VHD scenario voor de klant uploads de gecodeerde VHD naar hun opslag account- en belangrijke materiaal tot hun sleutel kluis en de codering configureren om een nieuwe IaaS VM-codering inschakelen
    - Voor de nieuwe VM gemaakt uit de galerie met Azure en bestaande VM al uitgevoerd in Azure, de klant bieden de configuratie coderen codering op de VM IaaS inschakelen

3. Klant verleent toegang tot Azure platform voor het lezen van het sleutelmateriaal van encryption (BitLocker-versleuteling sleutels voor Windows-systemen en wachtwoordzin voor Linux) uit de sleutel kluis aan de IaaS VM-codering inschakelen
4. De klant bieden Azure AD toepassings-id het sleutelmateriaal codering schrijven naar de sleutel kluis codering op de VM IaaS in #2 hierboven genoemde scenario's inschakelen
5.  Azure het servicemodel VM bijgewerkt met de codering en configuratie sleutel kluis en bepalingen VM gecodeerd voor de klant

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Decodering Workflow

Hoog niveau vereiste stappen voor het uitschakelen van codering voor de IaaS VM schijf zijn:

1. Klant wil (decodering) codering uitschakelen op een actieve IaaS VM in Azure via de Azure schijf versleuteling Resource Manager sjabloon of PS-cmdlets en de configuratie van de decodering.
2. De codering uitschakelen stap wordt het coderen van het volume met het besturingssysteem of gegevens of op de actieve Windows IaaS VM uitgeschakeld. Echter uitschakelen OS schijf versleuteling voor Linux wordt niet ondersteund als bedoeld in de bovenstaande documentatie. De stap uitschakelen is alleen toegestaan voor gegevensstations op Linux VMs. 
4. Azure werkt het VM service-model en de VM IaaS gedecodeerde is gemarkeerd. De inhoud van de VM zijn niet gecodeerd in rust niet meer.
5. De werking van de codering uitschakelen, verwijdert u de sleutel kluis klant en het sleutelmateriaal codering-BitLocker-versleuteling sleutels voor Windows of wachtwoordzin voor Linux.

## <a name="prerequisites"></a>Vereisten

De volgende zijn voorwaarden Azure schijfversleuteling op Azure IaaS VMs inschakelen voor de ondersteunde scenario's wordt beschreven in de overzichtssectie

- Gebruiker moet een geldige Azure active abonnement maken van bronnen in Azure in de regio's ondersteund hebben
- Azure schijf-codering wordt ondersteund op de volgende Windows server SKU's - Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2. Windows Server 2016 technische Preview wordt niet ondersteund in deze release.
- Azure schijf-codering wordt ondersteund op de volgende Windows-client SKU's - Client voor Windows 8 en Windows 10.

**Opmerking**: voor Windows Server 2008 R2, .net framework 4.5 moet zijn geïnstalleerd voordat u codering in Azure inschakelt. U kunt deze installeren vanaf Windows update door het installeren van de optionele update "Microsoft .NET Framework 4.5.2 voor Windows Server 2008 R2 x 64-systemen ([KB2901983](https://support.microsoft.com/kb/2901983))"

- Azure schijf-codering wordt ondersteund op de volgende Linux server SKU - Ubuntu, CentOS, SUSE en SUSE Linux Enterprise Server (SLES) en Red Hat Enterprise Linux.

**Opmerking**: op de volgende Linux distributies - 7.2 RHEL, CentOS 7.2, 16.04 Ubuntu Linux OS schijfversleuteling wordt ondersteund

- Alle resources (Ex: sleutel kluis, opslag-account VM, enz.) moet deel uitmaken van dezelfde Azure regio en abonnement.

**Opmerking**: Azure schijf codering is vereist dat de sleutel kluis en het VMs zich in hetzelfde gebied, Azure bevinden. Deze configureren voor afzonderlijke regio veroorzaakt fout in Azure schijf versleuteling functie inschakelt.

- Als u wilt instellen en configureren van Azure sleutel kluis voor codering van Azure schijfgebruik, Zie sectie **instelling en Azure sleutel kluis voor Azure schijfgebruik voor codering configureren** in de sectie *vereisten* van dit artikel.
- Als u wilt instellen en configureren van AD Azure toepassing in Azure Active directory voor de codering van Azure schijfgebruik, Zie sectie **de Azure toepassing AD in Azure Active Directory instellen** in de sectie *vereisten* van dit artikel.
- Als u wilt instellen en sleutel kluis-beleid configureren voor de toepassing van de Azure AD, Zie sectie **instelling sleutel kluis-beleid voor de Azure AD-toepassing** in de sectie *vereisten* van dit artikel.
- Zie de sectie **een vooraf gecodeerde Windows VHD voorbereiden** in de bijlage van dit artikel om voor te bereiden op een vooraf gecodeerde Windows VHD.
- Zie de sectie **een vooraf gecodeerde Linux VHD voorbereiden** in de bijlage van dit artikel om voor te bereiden op een vooraf gecodeerde Linux VHD.
- Toegang tot de sleutels of geheimen in Azure sleutel kluis klant moet Azure platform deze op de virtuele machine te starten en de virtuele machine OS volume ontsleutelen beschikbaar te maken. Als machtigingen wilt verlenen aan Azure platform voor toegang tot de klant sleutel kluis, moet de eigenschap **enabledForDiskEncryption** op de sleutel kluis voor deze eis worden ingesteld. Raadpleeg de sectie **instellen en configureren van Azure sleutel kluis voor codering van Azure schijfgebruik** in de bijlage van dit artikel voor meer informatie.
- De geheime sleutel kluis en codering (KEK) sleutel URL's moeten onder versiebeheer worden bijgehouden. Azure zorgt ervoor dat deze beperking versiebeheer. Zie hieronder voorbeelden van geldige geheim en KEK-URL:
    - Voorbeeld van een geldige geheime URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
    - Voorbeeld van een geldige KRK KEK:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Azure schijf codering ondersteunt geen poortnummers als onderdeel van de geheime sleutel kluis en KEK-URL's worden opgegeven. Zie hieronder voorbeelden voor ondersteunde sleutel kluis URL:
    - Niet-geaccepteerde toets kluis URL   *https://contosovault.vault.azure.net:443/geheimen/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
    - Aanvaarde sleutel kluis-URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Azure schijf codering inschakelen als u wilt moet functie, de IaaS VMs voldoen aan netwerk eindpunt configuratie: 
    - De IaaS VM moet verbinding kunnen maken met het eindpunt Azure Active Directory \[Login.windows.net\] voor een token met Azure sleutel kluis
    - De IaaS VM moet verbinding kunnen maken met Azure sleutel kluis eindpunt de coderingen sleutels schrijven naar sleutel kluis klant
    - De IaaS VM moet verbinding kunnen maken met Azure opslag eindpunt die fungeert als host voor de opslagplaats Azure extensie en Azure opslag account die fungeert als host voor de VHD-bestanden

**Opmerking:** Als het beveiligingsbeleid Azure VMs toegang heeft tot Internet beperkt, kunt u de bovenstaande URI waarmee u verbinding moet en configureren van een specifieke regel voor uitgaande verbindingen met de IP-adressen dat oplossen.

- Gebruik de nieuwste versie van Azure PowerShell SDK versie Azure schijf codering configureren. Download de nieuwste versie van [Azure PowerShell vrijgeven](https://github.com/Azure/azure-powershell/releases)

**Opmerking:** Azure schijf codering wordt niet ondersteund op [Azure PowerShell SDK versie 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Als er een fout opgetreden met het gebruik van Azure PowerShell 1.1.0, Zie het artikel [Azure schijf codering fout met betrekking tot Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

- Een van de opdrachten Azure CLI uitvoeren en koppelen aan uw abonnement Azure, moet u eerst de Azure CLI versie installeren:
    - Azure CLI installeren en koppelen aan uw abonnement Azure, Zie [het installeren en configureren van Azure CLI](../xplat-cli-install.md)
    - Met behulp van de CLI Azure voor Mac, Linux en Windows Azure Resource Manager, Zie [hier](azure-cli-arm-commands.md)
- BitLocker-sleutelbeveiliging externe schijf Azure codering oplossing gebruiken voor Windows IaaS VMs. Als uw VMs domein gekoppeld zijn, geen push-Groepsbeleid dat TPM protectors afdwingen. Raadpleeg [dit artikel](https://technet.microsoft.com/library/ee706521) voor meer informatie over de Groepsbeleid voor 'BitLocker zonder compatibele TPM toestaan'.
- De Azure schijf versleuteling vereiste PowerShell script Azure AD-toepassing maken, maken nieuwe sleutel kluis of setup bestaande sleutel kluis en-codering inschakelen bevindt zich [hier](https://github.com/Azure/azure-powershell/blob/dev/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).

#### <a name="setup-the-azure-ad-application-in-azure-active-directory"></a>Installatie van de toepassing AD Azure in Azure Active Directory

Codering moet worden ingeschakeld op een actieve VM in Azure, Azure schijfversleuteling wordt gegenereerd als de coderingssleutels schrijft naar de sleutel kluis. Beheer van coderingssleutels in een sleutel kluis nodig Azure AD verificatie.

Voor dit doel moet een Azure AD-toepassing worden gemaakt. Stapsgewijze instructies voor het registreren van een toepassing vindt u hier, in de sectie 'Een identiteit voor de toepassing ophalen' sectie in deze [blog posten](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).  Deze post bevat ook een aantal praktische voorbeelden over het inrichten en het configureren van uw sleutel kluis. Geheim van de client op basis van verificatie of Azure AD op certificaten gebaseerde verificatie kan worden gebruikt ter verificatie.

##### <a name="client-secret-based-authentication-for-azure-ad"></a>Het geheim van de client op basis van verificatie voor Azure AD

In de volgende gedeelten hebben de nodige maatregelen om een geheime basis clientverificatie voor Azure AD configureren.

##### <a name="create-a-new-azure-ad-app-using-azure-powershell"></a>Maak een nieuwe Azure AD app met Azure PowerShell

De onderstaande PowerShell-cmdlet gebruikt voor het maken van een nieuwe Azure AD app:

    $aadClientSecret = “yourSecret”
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

**Opmerking:** $azureAdApplication.ApplicationId is de Azure AD ClientID en $aadClientSecret is het geheim dat u kunt later schakelen ADE-client. U moet het geheim van de client Azure AD op passende wijze beschermen.


##### <a name="provisioning-the-azure-ad-client-id-and-secret-from-the-azure-classic-deployment-model-portal"></a>Het Azure AD client-ID en het geheim van het klassieke Azure implementatiemodel Portal inrichten

Azure AD Client-ID en een geheim kan ook worden ingericht met klassieke Azure implementatiemodel Portal op https://manage.windowsazure.com, de volgende stappen voor het uitvoeren van deze taak:

1. Klik op het tabblad Active Directory, zoals in onderstaande afbeelding wordt getoond:

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Klik toepassing toevoegen en typ de naam van de toepassing zoals hieronder wordt weergegeven:

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. Klik op de pijl en eigenschappen van de toepassing te configureren als volgt:

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Klik op het vinkje in de linkerbenedenhoek te voltooien. De pagina configuratie van de toepassing wordt weergegeven. U ziet dat de Azure AD Client-ID bevindt zich in de onderkant van de pagina, zoals in onderstaande afbeelding.

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. het geheim van de client Azure AD opslaan door te klikken op de knop opslaan. Klik op de opslaan knop en Let op het geheim van het tekstvak toetsen, dit is het geheim van de client Azure AD. U moet het geheim van de client Azure AD op passende wijze beschermen.

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig7.png)


**Opmerking:** deze bovenstaande overdracht wordt niet ondersteund in de Portal.

##### <a name="use-an-existing-app"></a>Gebruik een bestaande app

De volgende opdrachten uit te voeren moet u de module Azure AD PowerShell kan worden verkregen uit [hier](https://technet.microsoft.com/library/jj151815.aspx).

**Opmerking:** de onderstaande opdrachten moeten worden uitgevoerd in een nieuw venster met PowerShell. Gebruik geen Azure PowerShell of het venster Azure Resource Manager voor het uitvoeren van deze opdrachten. De reden voor deze aanbeveling is dat deze cmdlets in de MSOnline-module of Azure AD PowerShell zijn.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Op basis van certificaten voor Azure advertentie

> [AZURE.NOTE] AAD op basis van certificaatverificatie wordt momenteel niet ondersteund op Linux VMs.

De volgende secties zijn de nodige maatregelen om een op basis van certificaatverificatie configureren voor AD Azure.

##### <a name="create-a-new-azure-ad-app"></a>Maak een nieuwe Azure AD app

De PowerShell-cmdlets hieronder om een nieuwe uitvoeren Azure AD app:

**Opmerking:** Vervangen `yourpassword` onder de tekenreeks met beveiligd-wachtwoordverificatie en bescherming van het wachtwoord.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Nadat u deze stap hebt, een pfx-bestand uploaden naar sleutel kluis en het-beleid die nodig zijn voor de implementatie van dat certificaat voor een VM inschakelen.

##### <a name="use-an-existing-azure-ad-app"></a>Gebruik een bestaande Azure AD app
Als u verificatie op basis van certificaten voor een bestaande app configureert, gebruiken de PowerShell cmdlets hieronder. Zorg ervoor dat ze worden uitgevoerd in een nieuw venster met PowerShell.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Nadat u deze stap hebt, een pfx-bestand uploaden naar sleutel kluis en het-beleid die nodig zijn voor de implementatie van dat certificaat voor een VM inschakelen.

##### <a name="upload-a-pfx-file-to-key-vault"></a>Een PFX-bestand uploaden naar sleutel kluis
Deze [blog posten](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx) voor een gedetailleerde uitleg over hoe dit proces werkt, kunt u lezen. De onderstaande PowerShell-cmdlets zijn echter alles wat die u nodig hebt voor deze taak. Zorg ervoor dat ze van Azure PowerShell console uitvoeren:

**Opmerking:** Vervangen `yourpassword` onder de tekenreeks met beveiligd-wachtwoordverificatie en bescherming van het wachtwoord.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-key-vault-to-an-existing-vm"></a>Een certificaat in een sleutel kluis implementeren op een bestaande VM
Nadat de PFX uploaden is voltooid, gebruik de volgende stappen voor de implementatie van een certificaat in een kluis met sleutel voor een bestaande VM:

    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName


#### <a name="setting-key-vault-access-policy-for-the-azure-ad-application"></a>Sleutel kluis-beleid instellen voor de Azure AD-toepassing

Uw toepassing Azure AD moet rechten voor toegang tot de sleutels of geheimen in de kluis. Gebruik de cmdlet [Set-AzureKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/dn903607.aspx) machtigen om de toepassing, de Client-Id (die is gegenereerd toen de toepassing is geregistreerd) gebruikt als de waarde van de parameter – ServicePrincipalName. U kunt [dit blogbericht](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) voor enkele voorbeelden van die lezen. U hebt ook een voorbeeld van hoe deze taak via PowerShell kunt uitvoeren:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

**Opmerking**: Azure schijf codering, moet u de volgende-beleid naar de clienttoepassing AAD - configureren 'WrapKey' en 'Set machtigingen

## <a name="terminology"></a>Terminologie

De tabel terminologie gebruiken als referentie te begrijpen sommige van de algemene termen die worden gebruikt door deze technologie:

| Terminologie           | Definitie                                                                                                                                                                                                                                   |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure AD                   | Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Azure AD-account is een vereist item voor verificatie, opslaan en ophalen van de geheimen van de sleutel kluis.                                                                                                        |
| Azure sleutel kluis [AKV] | Azure sleutel kluis is een cryptografische sleutel service op basis van Hardware Security Modules ter bescherming van de cryptografische sleutels en gevoelige geheimen veilig FIPS gevalideerd., verwijzen naar de [Sleutel kluis](https://azure.microsoft.com/services/key-vault/) -documentatie voor meer informatie.          |
| ARM                   | Azure Resource Manager                                                                                                                                                                                                                       |
| BitLocker             | [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is dat een industrie herkend Windows volume encryption technologie gebruikt voor het inschakelen van schijfversleuteling op Windows IaaS VMs                                                                                                                  |
| BEK                   | BitLocker-sleutels worden gebruikt voor het coderen van het volume met het besturingssysteem opstarten en hoeveelheden gegevens. De BitLocker-sleutels zijn geheimen vrijwaringsmaatregelen in Azure sleutel kluis van de klant.                                                                              |
| CLI                   | [Azure Interface met opdrachtregel](../xplat-cli-install.md)                                                                                                                                                                                                                 |
| DM-Crypt              | [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is het Linux-gebaseerde transparante codering schijfsubsysteem gebruikt voor het inschakelen van schijfversleuteling op Linux IaaS VMs                                                                                                                           |
| KEK                   | Coderingssleutel Key is de asymmetrische sleutel (RSA 2048) gebruikt om te beschermen of het geheim laten teruglopen als gewenst. U kunt een beveiligd met een HSM of software beschermd sleutel opgeven. Voor meer details verwijzen naar [Azure sleutel kluis](https://azure.microsoft.com/services/key-vault/) -documentatie voor meer informatie |
| PS-cmdlets            | [Azure PowerShell-cmdlets](powershell-install-configure.md)                                                                                                                                                                                                                                           |

### <a name="setting-and-configuring-azure-key-vault-for-azure-disk-encryption-usage"></a>Instellen en configureren van Azure sleutel kluis voor Azure schijfgebruik codering

Azure schijfversleuteling beschermt de coderingssleutels schijf en geheimen in uw Azure sleutel kluis. Volg de stappen op elke één van de onderstaande hoofdstukken Setup sleutel kluis voor codering van Azure schijfgebruik.

#### <a name="create-a-new-key-vault"></a>Maak een nieuwe sleutel kluis
Gebruik wilt maken van een nieuwe sleutel kluis, een van de volgende opties:

- '101-maken-KeyVault' Resource Manager sjabloon zich [hier](https://github.com/Azure/azure-quickstart-templates/blob/master/101-create-key-vault/azuredeploy.json)
- Gebruik de Azure PowerShell [cmdlets sleutel kluis](https://msdn.microsoft.com/library/dn868052.aspx).
- De portal Azure resource manager gebruiken.

**Opmerking:** Als u al een sleutel kluis installatie voor uw abonnement hebt, gaat u naar de volgende sectie.

![Azure sleutel kluis](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="provisioning-a-key-encryption-key-optional"></a>Inrichten van een coderingssleutel Key (optioneel)

Als u wilt dat een sleutel Encryption Key (KEK) voor een extra beveiligingslaag doorloopt de BitLocker-sleutels voor gegevenscodering te gebruiken, moet u een KEK toevoegen aan uw sleutel kluis voor gebruik in het inrichtingsproces.  Gebruik de cmdlet [Add-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868048.aspx) voor het maken van een nieuwe versleutelingssleutel van de sleutel in sleutel kluis. U kunt ook KEK importeren vanuit uw Sleutelbeheer op ruimten HSM. Zie de [documentatie van de sleutel kluis](https://azure.microsoft.com/documentation/services/key-vault/)voor meer details.

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

De KEK kan van Azure Resource Manager portal ook met Azure sleutel kluis UX worden toegevoegd

![Azure sleutel kluis](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions-to-allow-the-azure-platform-access-to-the-keys-and-secrets"></a>Sleutel kluis machtigingen instellen voor de Azure platform toegang tot de sleutels en geheimen

Toegang tot de sleutels of geheimen in uw Azure sleutel kluis moet de Azure platform deze beschikbaar te maken voor de VM opstarten en het decoderen van de volumes. Als machtigingen wilt verlenen aan de Azure platform zodat deze toegang heeft tot de sleutel kluis, moet de eigenschap *enabledForDiskEncryption* worden ingesteld op de sleutel kluis. U kunt de eigenschap enabledForDiskEncryption instellen op uw sleutel kluis met behulp van de sleutel kluis cmdlet PS:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

U kunt ook de eigenschap *enabledForDiskEncryption* instellen door naar https://resources.azure.com te gaan. Zoals eerder, moet u de eigenschap *enabledForDiskEncryption* instellen op uw sleutel kluis. Anders mislukt de installatie.

U kunt-beleid instellen voor uw toepassing AAD van de sleutel kluis UX:

![Azure sleutel kluis](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure sleutel kluis](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Zorg ervoor dat de sleutel kluis is ingeschakeld voor de schijfversleuteling in 'Advanced-beleid':

![Azure sleutel kluis](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Schijf versleuteling implementatiescenario's en ervaringen van gebruikers

Er zijn veel scenario's dat u schijfversleuteling inschakelen kunt en de stappen kunnen variëren naar gelang het geval. In de volgende gedeelten komen deze scenario's met meer details.

### <a name="enable-encryption-on-new-iaas-vms-created-from-the-azure-gallery"></a>Gebruik codering voor nieuwe IaaS VM van gemaakt in de galerie Azure

Schijfversleuteling kan worden ingeschakeld op nieuwe IaaS Windows VM in Azure galerie in Azure met behulp van de sjabloon Resource Manager gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image). Klik op de knop 'Distribueren naar Azure' op de sjabloon Azure quickstart, invoer codering configuratie in de blade parameters en klik op OK. Selecteer abonnement, resourcegroep, bron locatie, juridische voorwaarden en de overeenkomst en klik op de knop maken om een nieuwe IaaS VM-codering inschakelen.

**Opmerking:** Met deze sjabloon maakt een nieuwe gecodeerde Windows VM met behulp van de galerijafbeelding van Windows Server 2012.

Schijfversleuteling kan worden ingeschakeld op een nieuwe IaaS RedHat Linux 7.2 VM met een 200 GB RAID 0-array met behulp van [deze](https://aka.ms/fde-rhel) sjabloon resource manager. Nadat de sjabloon is geïnstalleerd, controleert u of de VM codering status met behulp van de `Get-AzureRmVmDiskEncryptionStatus` cmdlet zoals beschreven in de sectie '[coderen OS station op een actieve Linux VM](#encrypting-os-drive-on-a-running-linux-vm)'. Wanneer de computer wordt opgevraagd `VMRestartPending`, opnieuw de VM.

Voor nieuwe VM in Azure galerie scenario met Azure AD Client-ID in de tabel hieronder ziet u de details van Resource Manager sjabloon parameters:

| Parameter                        | Beschrijving|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| adminUserName                 | De gebruikersnaam Admin voor de virtuele machine                                                                                                                           |
| adminPassword                 | Admin-gebruikerswachtwoord voor de virtuele machine                                                                                                                       |
| newStorageAccountName         | Naam van de account van de opslag voor het opslaan van OS en gegevens VHD 's                                                                                                             |
| vmSize                        | De grootte van de VM. Op dit moment worden alleen standaard A, D en G-reeksen ondersteund                                                                                          |
| virtualNetworkName            | De naam van de de VM NIC tot behoren moeten VNet.                                                                                                            |
| subnetName                    | Naam van het subnet van de vNet waarop de VM NIC tot behoren moet                                                                                               |
| AADClientID                   | Client-ID van de Azure AD app met machtigingen geheimen schrijven naar sleutel kluis                                                                                       |
| AADClientSecret               | Geheim van de client van de Azure AD app met machtigingen geheimen schrijven naar sleutel kluis                                                                                   |
| keyVaultURL                   | URL van de sleutel kluis aan welke BitLocker sleutel worden geüpload naar. U krijgt met de cmdlet: (Get-AzureRmKeyVault - VaultName-ResourceGroupName). VaultURI |
| keyEncryptionKeyURL           | URL van de Key-codering die wordt gebruikt voor het coderen van de gegenereerde sleutel van BitLocker. Dit is optioneel.                                                               |
| keyVaultResourceGroup         | De resourcegroep van de sleutel kluis                                                               |
| vmName                        | Naam van de VM op welke codering bewerking wordt uitgevoerd


**Opmerking:** KeyEncryptionKeyURL is een optionele parameter. U kunt uw eigen KEK de coderingssleutel gegevens (wachtwoordzin geheim) in een kluis met sleutel voor verdere beveiliging overbrengen.

### <a name="enable-encryption-on-new-iaas-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Gebruik codering voor nieuwe IaaS VM van gemaakt op basis van de klant gecodeerd VHD en de versleutelingssleutels

In dit scenario kunt u met behulp van de sjabloon Resource Manager, PowerShell-cmdlets of CLI-opdrachten te coderen. In de volgende secties wordt uitgelegd in meer details de bronnenbeheerder sjabloon en CLI-opdrachten.

Volg de instructies van een van deze secties voor het voorbereiden van vooraf gecodeerde afbeeldingen die kunnen worden gebruikt in Azure. Als de afbeelding is gemaakt, kunnen de stappen in de volgende sectie worden gebruikt voor het maken van een gecodeerde Azure VM.

- [Een vooraf gecodeerde Windows VHD voorbereiden](#preparing-a-pre-encrypted-windows-vhd)
- [Een vooraf gecodeerde Linux VHD voorbereiden](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-resource-manager-template"></a>Met behulp van bronbeheer sjabloon

Schijfversleuteling kan worden ingeschakeld op klant gecodeerd met behulp van de sjabloon Resource Manager VHD gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm). Klik op de knop 'Distribueren naar Azure' op de sjabloon Azure quickstart, invoer codering configuratie in de blade parameters en klik op OK. Selecteer abonnement, resourcegroep, bron locatie, juridische voorwaarden en de overeenkomst en klik op de knop maken om het nieuwe IaaS VM-codering inschakelen.

De bronnenbeheerder sjabloon parameters details klant gecodeerd VHD scenario worden beschreven in de onderstaande tabel:

| Parameter                        | Beschrijving|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| newStorageAccountName | De naam van de account van de opslagruimte voor het opslaan van gecodeerde OS vhd. Deze account opslag moet zijn al gemaakt in dezelfde bronnengroep en dezelfde locatie als de VM                                                     |
| osVhdUri              | URI van OS vhd van opslag account                                                                                                                                                                                      |
| waardereeksen                | Productsoort OS (Windows/Linux)                                                                                                                                                                                         |
| virtualNetworkName    | De naam van de de VM NIC tot behoren moeten VNet. Dit moet zijn al gemaakt in dezelfde bronnengroep en dezelfde locatie als de VM                                                                     |
| subnetName            | Naam van het subnet van de vNet waarop de VM NIC tot behoren moet                                                                                                                                                     |
| vmSize                | De grootte van de VM. Op dit moment worden alleen standaard A, D en G-reeksen ondersteund                                                                                                                                                |
| keyVaultResourceID    | De sleutel identificeren ResourceID kluis bron in de ARM. U kunt daar met de PowerShell-cmdlet: (Get-AzureRmKeyVault - VaultName &lt;yourKeyVaultName&gt; - ResourceGroupName &lt;yourResourceGroupName&gt;). ResourceId |
| keyVaultSecretUrl     | URL van de schijf coderingssleutel ingericht in sleutel kluis                                                                                                                                                                |
| keyVaultKekUrl        | URL van de coderingssleutel Key is de coderingssleutel gegenereerde schijf coderen                                                                                                                                       |
| vmName               | Naam van de VM IaaS   |


#### <a name="using-powershell-cmdlets"></a>Met behulp van PowerShell-cmdlets

Schijfversleuteling kan worden ingeschakeld op klant gecodeerd met behulp van de PS-cmdlets VHD gepubliceerde [hier](https://msdn.microsoft.com/library/azure/mt603746.aspx).  

#### <a name="using-cli-commands"></a>CLI-opdrachten

Volg onderstaande stappen voor de schijfversleuteling voor dit scenario CLI-opdrachten gebruiken:

1. -Beleid op de sleutel kluis instellen:
    - 'EnabledForDiskEncryption' vlag instellen:`azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
    - Machtigingen instellen voor Azure AD app geheimen schrijven naar KeyVault:`azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]`
2. Schakelt u codering in een VM bestaande/uitgevoerd:  *azure vm inschakelen-schijf-versleuteling--resource groep <resourceGroupName> --naam <vmName> --aad-client-id <aadClientId> --aad-geheim-client <aadClientSecret> --schijf-versleuteling-sleutel-kluis-url <keyVaultURL> --schijf-versleuteling-sleutel-kluis-id <keyVaultResourceId> *
3. Get status codering: *"azure vm weergeven-schijf-versleuteling-status--resource groep <resourceGroupName> --naam <vmName> --json"*
4. Gecodeerd VHD naar een nieuwe VM van klant-codering inschakelen, gebruikt u de onderstaande parameters met opdracht 'azure vm maken':
    - schijf-versleuteling-sleutel-kluis-id < schijf-versleuteling-sleutel-kluis-id >
    - schijf-versleuteling-sleutel-url < schijf-versleuteling-sleutel-url >
    - sleutel-codering-sleutel-kluis-id < sleutel-codering-sleutel-kluis-id >
    - sleutel-codering-sleutel-url < sleutel-codering-sleutel-url >


### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Op bestaande of IaaS Windows VM in Azure-codering inschakelen

In dit scenario kunt u met behulp van de sjabloon Resource Manager, PowerShell-cmdlets of CLI-opdrachten te coderen. In de volgende secties wordt uitgelegd in meer details inschakelen met behulp van bronbeheer sjabloon en CLI-opdrachten.

#### <a name="using-resource-manager-template"></a>Met behulp van bronbeheer sjabloon

Schijfversleuteling kan worden ingeschakeld op bestaande uitvoeren/IaaS Windows VM in Azure met behulp van de sjabloon Resource Manager gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm). Klik op de knop 'Distribueren naar Azure' op de sjabloon Azure quickstart, invoer codering configuratie in de blade parameters en klik op OK. Selecteer abonnement, resourcegroep, bron locatie, juridische voorwaarden en de overeenkomst en klik op de knop maken om het bestaande actief/IaaS VM-codering inschakelen.

De Resource Manager sjabloon parameters details voor bestaande uitvoeren/VM scenario met Azure AD Client-ID zijn beschikbaar in de volgende tabel:

| Parameter                 | Beschrijving|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AADClientID            | Client-ID van de Azure AD app met machtigingen geheimen schrijven naar sleutel kluis                                                                                                                                              |
| AADClientSecret         | Geheim van de client van de Azure AD app met machtigingen geheimen schrijven naar sleutel kluis                                                                                                                                          |
| keyVaultName | De naam van de sleutel kluis aan welke BitLocker sleutel worden geüpload naar. U krijgt met de cmdlet: (Get-AzureRmKeyVault - ResourceGroupName <yourResourceGroupName>). Vaultname   |
|  keyEncryptionKeyURL   | URL van de Key-codering die wordt gebruikt voor het coderen van de gegenereerde sleutel van BitLocker. Dit is optioneel als u `nokek` in de vervolgkeuzelijst UseExistingKek. Als u `kek` in de vervolgkeuzelijst UseExistingKek de keyEncryptionKeyURL-waarde moet worden ingevoerd                                                                                                                        |
| volumeType             | Het type van het volume op welke codering bewerking wordt uitgevoerd. Geldige waarden zijn "OS", "Gegevens", "Alle"                                                                                                                     |
| sequenceVersion         | Versie volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijf versleuteling-bewerking wordt uitgevoerd op dezelfde VM                                                                             |
| vmName                 | Naam van de VM op welke codering bewerking wordt uitgevoerd


**Opmerking:** KeyEncryptionKeyURL is een optionele parameter. U kunt uw eigen KEK de coderingssleutel gegevens (geheime BitLocker-versleuteling) in een kluis met sleutel voor verdere beveiliging overbrengen.

#### <a name="using-powershell-cmdlets"></a>Met behulp van PowerShell-cmdlets

Ga naar de **verkennen Azure schijfversleuteling met Azure PowerShell** blog post [deel 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) voor meer informatie over het inschakelen van codering met Azure schijfversleuteling met PS-cmdlets.

#### <a name="using-cli-commands"></a>CLI-opdrachten

De volgende stappen codering inschakelen op bestaande/het uitvoeren IaaS Windows VM in Azure CLI-opdrachten gebruiken:

1. -Beleid op de sleutel kluis instellen:
    - "EnabledForDiskEncryption' vlag instellen:" azure keyvault-beleid instellen--kluis naam <keyVaultName> --ingeschakeld-voor-schijf-versleuteling true "
    - Machtigingen instellen voor Azure AD app geheimen schrijven naar KeyVault: "azure keyvault-beleid instellen--kluis naam <keyVaultName> --spn <aadClientID> --perms te toetsen [\"alle\"]--perms naar geheimen [\"alle\"]"
2. Schakelt u codering in een VM bestaande/uitgevoerd:  *azure vm inschakelen-schijf-versleuteling--resource groep <resourceGroupName> --naam <vmName> --aad-client-id <aadClientId> --aad-geheim-client <aadClientSecret> --schijf-versleuteling-sleutel-kluis-url <keyVaultURL> --schijf-versleuteling-sleutel-kluis-id <keyVaultResourceId> *
3. Get status codering: *"azure vm weergeven-schijf-versleuteling-status--resource groep <resourceGroupName> --naam <vmName> --json"*
4. Gecodeerd VHD naar een nieuwe VM van klant-codering inschakelen, gebruikt u de onderstaande parameters met opdracht 'azure vm maken':
    - schijf-versleuteling-sleutel-kluis-id < schijf-versleuteling-sleutel-kluis-id >
    - schijf-versleuteling-sleutel-url < schijf-versleuteling-sleutel-url >
    - sleutel-codering-sleutel-kluis-id < sleutel-codering-sleutel-kluis-id >
    - sleutel-codering-sleutel-url < sleutel-codering-sleutel-url >


### <a name="enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure"></a>Op bestaande of IaaS Linux VM in Azure-codering inschakelen

Schijfversleuteling kan worden ingeschakeld op bestaande uitvoeren/IaaS Linux VM in Azure met behulp van de sjabloon Resource Manager gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm). Klik op de knop 'Distribueren naar Azure' op de sjabloon Azure quickstart, invoer codering configuratie in de blade parameters en klik op OK. Selecteer abonnement, resourcegroep, bron locatie, juridische voorwaarden en de overeenkomst en klik op de knop maken om het bestaande actief/IaaS VM-codering inschakelen.

De bronnenbeheerder sjabloon parameters details voor bestaande uitvoeren/VM scenario met Azure AD Client-ID worden in de tabel hieronder beschreven:

| Parameter                 | Beschrijving|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AADClientID            | Client-ID van de Azure AD app met machtigingen geheimen schrijven naar sleutel kluis                                                                                                                                              |
| AADClientSecret         | Geheim van de client van de Azure AD app met machtigingen geheimen schrijven naar sleutel kluis                                                                                                                                          |
| keyVaultName | De naam van de sleutel kluis aan welke BitLocker sleutel worden geüpload naar. U krijgt met de cmdlet: (Get-AzureRmKeyVault - ResourceGroupName <yourResourceGroupName>). Vaultname   |
|  keyEncryptionKeyURL   | URL van de Key-codering die wordt gebruikt voor het coderen van de gegenereerde sleutel van BitLocker. Dit is optioneel als u 'nokek' in de vervolgkeuzelijst UseExistingKek. Als u 'kek' in de vervolgkeuzelijst UseExistingKek selecteert, de keyEncryptionKeyURL-waarde moet worden ingevoerd                                                                                                                        |
| volumeType             | Het type van het volume op welke codering bewerking wordt uitgevoerd. Geldige ondersteunde waarden zijn "OS" / "Alle" (voor 7.2 RHEL, CentOS 7.2 & Ubuntu 16.04) en 'Data' voor alle andere distros.                                                                                                                      |
| sequenceVersion         | Versie volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijf versleuteling-bewerking wordt uitgevoerd op dezelfde VM                                                                             |
| vmName                 | Naam van de VM op welke codering bewerking wordt uitgevoerd
| Wachtwoordzin              | Een sterke wachtwoordzin typt als de sleutel data codering                                                                                                                                                                       |                                                                                                                                                                                                                                                      

**Opmerking:** KeyEncryptionKeyURL is een optionele parameter. U kunt uw eigen KEK de coderingssleutel gegevens (wachtwoordzin geheim) in een kluis met sleutel voor verdere beveiliging overbrengen.

#### <a name="cli-commands"></a>CLI-opdrachten

Schijfversleuteling kan worden ingeschakeld op klant gecodeerd met behulp van de CLI-opdracht hebt geïnstalleerd vanaf [hier](../xplat-cli-install.md)VHD. De volgende stappen codering inschakelen op bestaande/het uitvoeren IaaS Linux VM in Azure CLI-opdrachten gebruiken:

1. -Beleid op de sleutel kluis instellen:
    - "EnabledForDiskEncryption' vlag instellen:" azure keyvault-beleid instellen--kluis naam <keyVaultName> --ingeschakeld-voor-schijf-versleuteling true "
    - Machtigingen instellen voor Azure AD app geheimen schrijven naar KeyVault: "azure keyvault-beleid instellen--kluis naam <keyVaultName> --spn <aadClientID> --perms te toetsen [\"alle\"]--perms naar geheimen [\"alle\"]"
2. Schakelt u codering in een VM bestaande/uitgevoerd:  *azure vm inschakelen-schijf-versleuteling--resource groep <resourceGroupName> --naam <vmName> --aad-client-id <aadClientId> --aad-geheim-client <aadClientSecret> --schijf-versleuteling-sleutel-kluis-url <keyVaultURL> --schijf-versleuteling-sleutel-kluis-id <keyVaultResourceId> *
3. Get status codering: ' azure vm weergeven-schijf-versleuteling-status--resource groep <resourceGroupName> --naam <vmName> --json "
4. Gecodeerd VHD naar een nieuwe VM van klant-codering inschakelen, gebruikt u de onderstaande parameters met opdracht 'azure vm maken'.
    - *schijf-versleuteling-sleutel-kluis-id < schijf-versleuteling-sleutel-kluis-id >*
    - *schijf-versleuteling-sleutel-url < schijf-versleuteling-sleutel-url >*
    - *sleutel-codering-sleutel-kluis-id < sleutel-codering-sleutel-kluis-id >*
    - *sleutel-codering-sleutel-url < sleutel-codering-sleutel-url >*

### <a name="get-encryption-status-of-an-encrypted-iaas-vm"></a>Status van een gecodeerde IaaS VM codering ophalen

Status van codering met portal bronnenbeheerder Azure, [PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt622700.aspx) of CLI-opdrachten kunt u ophalen. In de volgende secties wordt uitgelegd hoe de Azure portal en CLI-opdrachten gebruiken om de status van de versleuteling.

#### <a name="get-encryption-status-of-an-encrypted-windows-vm-using-azure-resource-manager-portal"></a>Status van een gecodeerde Windows VM met behulp van bronbeheer Azure portal codering ophalen

De status van de codering van de VM IaaS krijgt u vanuit de portal Azure Resource Manager. Aanmelden bij een Azure portal op https://portal.azure.com/, klikt u op de koppeling in het menu links voor een overzicht van de overzichtsweergave van de virtuele machines in uw abonnement virtuele machines. De weergave van virtuele machines kunt u filteren door de naam van het abonnement te selecteren in de vervolgkeuzelijst van het abonnement. Klik op kolommen die zich bovenaan in het menu van de virtuele machines. Schijfversleuteling-kolom van de bladeserver Kies kolom selecteren en klik op bijwerken. Hier ziet u de schijf versleuteling kolom de coderingsstatus 'Ingeschakeld' of 'Uitgeschakeld' weergegeven voor elke VM zoals in de onderstaande afbeelding.

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-encryption-status-of-an-encrypted-windowslinux-iaas-vm-using-disk-encryption-ps-cmdlet"></a>Status van een gecodeerde (Windows/Linux) IaaS VM met schijfversleuteling PS Cmdlet codering ophalen
De status van de codering van de VM IaaS krijgt u vanaf de schijf versleuteling PS cmdlet 'Get-AzureRmVMDiskEncryptionStatus'. Als u de versleutelingsinstellingen voor uw VM, typt u in uw Azure PowerShell-sessie:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

De uitvoer van de Get-AzureRmVMDiskEncryptionStatus kan worden gekeurd voor codering belangrijke URL's.
    
    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMNam
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey
    
    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

De waarde van de OSVolumeEncrypted en DataVolumesEncrypted-instellingen zijn ingesteld op "Gecodeerd" waarin zowel de volumes worden gecodeerd met Azure schijfversleuteling. Ga naar de **verkennen Azure schijfversleuteling met Azure PowerShell** blog post [deel 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) voor meer informatie over het inschakelen van codering met Azure schijfversleuteling met PS-cmdlets.

**Opmerking**: op Linux VMs, de `Get-AzureRmVMDiskEncryptionStatus` cmdlet duurt 3-4 minuten om de status van de codering te rapporteren.

#### <a name="get-encryption-status-of-the-iaas-vm-from-disk-encryption-cli-command"></a>Status van de VM IaaS codering van schijfversleuteling CLI opdracht ophalen

Van schijfversleuteling CLI opdracht *azure vm weergeven-schijf-versleuteling-status*kunt u de status van de codering van de VM IaaS krijgen. Als u de versleutelingsinstellingen voor uw VM, typt u in uw Azure CLI-sessie:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Codering uitschakelen voor het uitvoeren van Windows IaaS VM

U codering in een actieve Windows- of Linux IaaS VM via de Azure schijf versleuteling Resource Manager sjabloon of PS-cmdlets kunt uitschakelen en de configuratie van de decodering.


##### <a name="windows-vm"></a>Windows VM

De codering uitschakelen stap wordt het coderen van het volume met het besturingssysteem of gegevens of op de actieve Windows IaaS VM uitgeschakeld. U kan uitschakelen van het volume met het besturingssysteem en laat het gegevensvolume gecodeerd. Wanneer de codering uitschakelen stap wordt uitgevoerd, model klassieke Azure-implementatie werkt het VM service-model en de IaaS Windows VM gedecodeerde is gemarkeerd. De inhoud van de VM zijn niet gecodeerd in rust niet meer. De codering uitschakelen, verwijdert de klant sleutel kluis en het sleutelmateriaal codering, BitLocker-versleuteling sleutels voor Windows en wachtwoordzin voor Linux. 

##### <a name="linux-vm"></a>Linux VM

De codering uitschakelen stap wordt het coderen van het gegevensvolume van de op de lopende Linux IaaS VM uitgeschakeld

**Opmerking**: codering op OS schijf uitschakelen is niet toegestaan op Linux VMs.

##### <a name="disable-encryption-on-existingrunning-iaas-vm-in-azure-using-resource-manager-template"></a>Codering op bestaande uitvoeren/IaaS VM in Azure met behulp van bronbeheer sjabloon uitschakelen

Schijfversleuteling kan worden uitgeschakeld voor het uitvoeren van Windows IaaS VM met behulp van de sjabloon Resource Manager gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm). Klik op de knop 'Distribueren naar Azure' op de sjabloon Azure quickstart, invoer decodering configuratie in de blade parameters en klik op OK. Selecteer abonnement, resourcegroep, bron locatie, juridische voorwaarden en de overeenkomst en klik op de knop maken om een nieuwe IaaS VM-codering inschakelen.

Voor Linux VM, kan [deze](https://aka.ms/decrypt-linuxvm) sjabloon worden gebruikt voor codering uitschakelen.

Sjabloon parameters details Resource Manager voor het uitschakelen van codering uitgevoerd IaaS VM:

| vmName         | Naam van de VM op welke codering bewerking wordt uitgevoerd                                                                                                                                                                       |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| volumeType     | Het type van het volume op welke decodering bewerking wordt uitgevoerd. Geldige waarden zijn "OS", "Gegevens", "All". **Opmerking:** U kunt codering voor het uitvoeren van Windows IaaS VM OS/opstartvolume zonder codering op 'Gegevens' volume uitschakelen niet uitschakelen. **Opmerking**: codering op OS schijf uitschakelen is niet toegestaan op Linux VMs. |
| sequenceVersion | Versie volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijf decoderingsbewerking wordt uitgevoerd op dezelfde VM                                                                                          |

##### <a name="disable-encryption-on-existingrunning-iaas-vm-in-azure-using-ps-cmdlet"></a>Schakel codering op bestaande uitvoeren/IaaS VM in Azure met PS-cmdlet uit

Om uit te schakelen met de cmdlet PS, [Disable AzureRmVMDiskEncryption](https://msdn.microsoft.com/library/azure/mt715776.aspx) cmdlet codering op een infrastructuur uitgeschakeld als een virtuele machine van service (IaaS). Deze cmdlet ondersteunt Windows en Linux VMs. Deze cmdlet wordt een uitbreiding op de virtuele machine uit te schakelen codering geïnstalleerd. Als de parameter Name niet wordt opgegeven, wordt een uitbreiding met de standaardnaam "AzureDiskEncryption voor Windows VMs" gemaakt. 

Op Linux VMs gebruikt de extensie 'AzureDiskEncryptionForLinux'.

**Opmerking**: deze cmdlet de virtuele machine opnieuw is opgestart. 

## <a name="appendix"></a>Aanhangsel

### <a name="connect-to-your-subscription"></a>Verbinding maken met uw abonnement

Zorg ervoor dat de sectie *voorwaarden* in dit document voordat u verdergaat. Als u zeker bent dat aan alle vereisten zijn voldaan, als volgt verbinding maken met uw abonnement:

1. een Azure PowerShell-sessie start en aanmelden bij uw account Azure met de volgende opdracht:

    Login-AzureRmAccount

2. Als u meerdere abonnementen hebt en opgeven van een specifieke naam wilt wilt gebruiken, typt u het volgende overzicht van de abonnementen voor uw account:

    Get-AzureRmSubscription

3. wilt opgeven van het abonnement dat u wilt gebruiken, typt u:

    Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>

4. om te controleren of het abonnement dat is geconfigureerd voor het juist is, typt u:

    Get-AzureRmSubscription

5. om te bevestigen dat de Azure schijfversleuteling-cmdlets worden geïnstalleerd, typt u:

    Get-command *diskencryption*

6. ziet de onderstaande output Azure schijf versleuteling PowerShell installatie bevestigen:

    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     

### <a name="preparing-a-pre-encrypted-windows-vhd"></a>Een vooraf gecodeerde Windows VHD voorbereiden
De volgende secties zijn nodig voor het bereiden van een vooraf gecodeerde Windows VHD voor implementatie van een gecodeerde VHD in Azure IaaS. De stappen worden voor het voorbereiden en een verse windows VM (vhd) op Hyper-V of Azure opstarten gebruikt.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Update groepsbeleid waarmee niet-TPM voor bescherming van OS
U moet BitLocker-groepsbeleidsinstelling genoemd BitLocker-stationsversleuteling, te vinden onder beleid voor lokale Computer \Computer Configuration\Administrative Templates\Windows onderdelen configureren. Deze instelling te wijzigen: *Besturingssysteem stations - extra verificatie bij opstarten: BitLocker zonder compatibele TPM toestaan vereisen* zoals in de onderstaande afbeelding:

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>BitLocker-functie onderdelen installeren
Voor Windows Server 2012 en gebruik de volgende opdracht:

    dism /online /Enable-Feature /all /FeatureName:Bitlocker /quiet /norestart

Voor Windows Server 2008 R2-gebruik de volgende opdracht:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-os-volume-for-bitlocker-using-bdehdcfg"></a>Volume met het besturingssysteem voorbereiden voor het gebruik van BitLocker`bdehdcfg`

Uitvoeren van de opdracht hieronder voor het comprimeren van de OS-partitie en de computer voorbereiden voor BitLocker.

    bdehdcfg -target c: shrink -quiet

#### <a name="using-bitlocker-to-protect-the-os-volume"></a>Met behulp van BitLocker voor het volume met het besturingssysteem te beveiligen
Gebruik de [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) opdracht inschakelen van codering op het opstartvolume met behulp van een externe-sleutelbeveiliging en plaatst u de externe sleutel (.bek-bestand) op de externe schijf of het volume. Codering wordt ingeschakeld op het volume van het systeem of opstartstation nadat de computer opnieuw wordt opgestart.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

**Opmerking:** De VM moet worden voorbereid met een vhd van afzonderlijke gegevens of een resource voor het verkrijgen van de externe sleutel met behulp van BitLocker.

#### <a name="encrypting-os-drive-on-a-running-linux-vm"></a>Coderen van OS-station op een actieve Linux VM

Codering van het station op een actieve Linux VM OS wordt ondersteund op de volgende distros:

- 7.2 RHEL
- 7.2 centOS
- Ubuntu-16.04

Vereisten voor de codering van OS schijf:

- VM moet worden gemaakt van Azure galerijafbeelding in Azure Resource Manager-portal.
- Azure VM met ten minste 4 GB RAM (grootte 7 GB wordt aanbevolen).
- (Voor RHEL en CentOS) SELinux moet worden [uitgeschakeld](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) op de VM. De VM opnieuw moet worden gestart ten minste eenmaal na het uitschakelen van SELinux.


##### <a name="steps"></a>Werk

1. Maak een VM met behulp van een van de hierboven vermelde distros.

Voor CentOS-7.2 OS schijf codering via een speciale afbeelding. Geef voor het gebruik van deze afbeelding "7.2n" Als de Sku bij het maken van de VM:

    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"

2. de VM aan uw behoeften configureren. Als u wilt versleutelen alle gegevens stations moeten worden opgegeven en worden gemonteerd van /etc/fstab-stations (OS + gegevens).

> [AZURE.NOTE] Moet u UUID =... gegevensstations in/etc/fstab in plaats van blok naam van het apparaat, bijvoorbeeld /dev/sdb1 opgeven. Tijdens het coderen wordt de volgorde van de stations wijzigen op de VM. Als uw VM afhankelijk van een bepaalde volgorde van blokapparaten is wordt het niet koppelen ze na codering.

3. Logout SSH-sessies.

4. voor het coderen van het besturingssysteem, geef volumeType als "alle" of "OS" Als [codering inschakelen](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

> [AZURE.NOTE] Alle gebruikers ruimte processen die niet worden uitgevoerd als `systemd` services worden gedood met een `SIGKILL`. De VM moet opnieuw worden opgestart. Neem in ogenschouw op uitval van de VM OS inschakelen schijf versleuteling op een actieve VM.

5. periodiek controleren de voortgang van codering met instructies in de [volgende sectie](#monitoring-os-encryption-progress).

6. als Get-AzureRmVmDiskEncryptionStatus 'VMRestartPending' bevat, start opnieuw op uw VM door een registratie bij het of via PowerShell-Portal/CLI.

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, please reboot the VM

Het wordt aanbevolen de [Diagnostische gegevens van het opstarten](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) van de VM *voordat* opnieuw opslaan.

#### <a name="monitoring-os-encryption-progress"></a>OS codering voortgang bewaken

Er zijn drie manieren om OS codering voortgang te controleren.

1. de cmdlet Get-AzureRmVmDiskEncryptionStatus gebruiken en controleren van het veld ProgressMessage: 
 
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started

Zodra de VM bereikt "OS disk encryption gestart" duurt ongeveer 40-50 minuten op een Premium-opslag back-VM.

[Probleem #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` en `DataVolumesEncrypted` als `Unknown` in sommige distros. Met de WALinuxAgent versie 2.1.5 en boven dit probleem automatisch opgelost. In het geval er `Unknown` in de uitvoer, controleert u codering schijfstatus met Azure bron Viewer.

Ga naar [Azure bron Viewer](https://resources.azure.com/)uit en vouw vervolgens deze hiërarchie in het deelvenster selectie links:

~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

Ga in de InstanceView, de status van de codering van uw schijven.

![VM-exemplaar weergeven](./media/azure-security-disk-encryption/vm-instanceview.png)

2. [Start diagnostische gegevens](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/)bekijken. Berichten van ade-extensie moeten worden voorafgegaan door `[AzureDiskEncryption]`.

3. aanmelden bij de VM via SSH en het ophalen van het logboek van de uitbreiding van

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

Het is niet aanbevolen aan te melden bij de VM terwijl OS versleuteling uitgevoerd wordt. De logboeken moeten daarom alleen als de andere twee methoden zijn niet worden gekopieerd.

#### <a name="preparing-a-pre-encrypted-linux-vhd"></a>Een vooraf gecodeerde Linux VHD voorbereiden

##### <a name="ubuntu-16"></a>Ubuntu 16

###### <a name="configure-encryption-during-distro-install"></a>Codering tijdens de installatie van de distro configureren

1. Selecteer "Gecodeerde volumes configureren" wanneer het partitioneren van schijven.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. maken een afzonderlijke opstartstation die niet worden gecodeerd. Codeer uw basisstation.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. een wachtwoordzin opgeven. Dit is de wachtwoordzin die u naar een KeyVault uploadt.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. voltooien partitioneren.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. bij het opstarten van de VM, wordt u gevraagd een wachtwoordzin. Gebruik de wachtwoordzin die u hebt opgegeven in stap 3.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. bereid VM voor het uploaden van [deze](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/)instructies Azure. De laatste stap (VM deprovisioning) kunnen niet worden uitgevoerd nog.

###### <a name="configure-encryption-to-work-with-azure"></a>Codering voor het werken met Azure configureren

1. Maak een onder /usr/local/sbin/azure_crypt_key.sh, met de inhoud van het onderstaande script. Let op de KeyFileName, omdat het de naam wachtwoordzin Azure heeft gebracht.

    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do
        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi


2. het crypt-configuratie in de */etc/crypttab*wijzigen. Het ziet er zo uit:

    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh

3. Wanneer u de *azure_crypt_key.sh* in Windows bewerkt en gekopieerd naar Linux, vergeet niet uit te voeren *dos2unix /usr/local/sbin/azure_crypt_key.sh*.

4. uitvoerbare machtigingen toevoegen aan het script:

    chmod +x /usr/local/sbin/azure_crypt_key.sh

4. bewerken */etc/initramfs-tools/modules* door regels toe te voegen:

    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1

5. uitvoeren `update-initramfs -u -k all` voor het bijwerken van de initramfs om de `keyscript` van kracht.
6. nu kunt u de VM deprovision.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

7. doorgaan naar de volgende stap en [upload uw VHD](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure.

##### <a name="opensuse-132"></a>13.2 openSUSE

###### <a name="configure-encryption-during-distro-install"></a>Codering tijdens de installatie van de distro configureren

1. Selecteer 'Volume groep coderen' tijdens het partitioneren van schijven. Een wachtwoordzin opgeven. Dit is de wachtwoordzin die u naar een KeyVault uploadt.

![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. start met behulp van uw wachtzin VM.

![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. bereid VM voor het uploaden van [deze](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131)instructies Azure. De laatste stap (VM deprovisioning) kunnen niet worden uitgevoerd nog.

###### <a name="configure-encryption-to-work-with-azure"></a>Codering voor het werken met Azure configureren

1. de /etc/dracut.conf bewerken en voeg de volgende regel:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2. een reactie op deze regels aan het einde van het bestand ' / usr/lib/dracut/modules.d/90crypt/module-setup.sh ':

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3. toevoegen de volgende regel aan het begin van het bestand "/ usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"

    DRACUT_SYSTEMD=0

en alle instanties van

    if [ -z "$DRACUT_SYSTEMD" ]; then

Aan

    if [ 1 ]; then

4. /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh bewerken en deze toe te voegen na de "# Open LUKS apparaat"

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5. Voer de ' / usr/sbin/dracut - f - v "de initrd bijwerken.

6. nu kunt u de VM en [upload uw VHD](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure deprovision.

##### <a name="centos-7"></a>CentOS 7

###### <a name="configure-encryption-during-distro-install"></a>Codering tijdens de installatie van de distro configureren

1. Selecteer "Mijn gegevens coderen" schijven partitioneren.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Zorg voor root partitie "Coderen" is geselecteerd.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. een wachtwoordzin opgeven. Dit is de wachtwoordzin die u naar een KeyVault uploadt.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. wanneer de VM opstarten, wordt u gevraagd een wachtwoordzin. Gebruik de wachtwoordzin die u hebt opgegeven in stap 3.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. VM voorbereiden voor het uploaden van [deze](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70)instructies Azure. De laatste stap (VM deprovisioning) kunnen niet worden uitgevoerd nog.

6. nu kunt u de VM en [upload uw VHD](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure deprovision.

###### <a name="configure-encryption-to-work-with-azure"></a>Codering voor het werken met Azure configureren

1. de /etc/dracut.conf bewerken en voeg de volgende regel:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2. een reactie op deze regels aan het einde van het bestand ' / usr/lib/dracut/modules.d/90crypt/module-setup.sh ':

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3. toevoegen de volgende regel aan het begin van het bestand "/ usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"

    DRACUT_SYSTEMD=0

en alle instanties van

    if [ -z "$DRACUT_SYSTEMD" ]; then

Aan

    if [ 1 ]; then

4. /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh bewerken en deze toe te voegen na de "# Open LUKS apparaat"

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5. Voer de ' / usr/sbin/dracut - f - v "de initrd bijwerken.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Gecodeerde VHD uploaden naar een account Azure opslag
Zodra BitLocker-versleuteling pr DM-Crypt codering is ingeschakeld, moet de lokale gecodeerde VHD worden geüpload naar uw account voor opslag.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-disk-encryption-secret-for-the-pre-encrypted-vm-to-key-vault"></a>Schijf versleuteling geheim uploaden voor het vooraf gecodeerde VM met sleutel kluis
Het geheim van schijf versleuteling verkregen moet eerder worden geüpload als een geheim in sleutel kluis. De sleutel kluis machtigingen nodig hebben voor de AAD-client is ingeschakeld en de codering van de schijf.


    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $KeyVault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Schijf versleuteling geheim niet gecodeerd met een KEK
[Set AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) inrichten van het geheim in een kluis met sleutel gebruiken. De bek-bestand is gecodeerd als een base64-tekenreeks en vervolgens geüpload naar de sleutel kluis met de cmdlet Set-AzureKeyVaultSecret in het geval van een virtuele Windows-computer. De wachtwoordzin is gecodeerd als een base64-tekenreeks en vervolgens geüpload naar de sleutel kluis voor Linux. Zorg bovendien dat de volgende codes zijn ingesteld tijdens het maken van het geheim in sleutel kluis.

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

De `$secretUrl` in de volgende stap wordt gebruikt voor het [koppelen van de schijf OS zonder KEK](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Schijf versleuteling geheim, gecodeerd met een KEK

Het geheim kan eventueel worden gecodeerd met een coderingssleutel Key voordat u uploadt naar de sleutel kluis. De wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) gebruiken voor het coderen van eerst het geheim met behulp van de coderingssleutel Key. De uitvoer van deze bewerking wrap is een base64 gecodeerde URL-tekenreeks die vervolgens wordt geüpload als een geheim met de cmdlet [Set-AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) .

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

De `$KeyEncryptionKey` en `$secretUrl` in de volgende stap wordt gebruikt voor het [koppelen van de OS-schijf met behulp van KEK](#using-a-kek).

### <a name="specify-secret-url-when-attaching-os-disk"></a>Geheime URL opgeven wanneer OS schijf koppelen

#### <a name="without-using-a-kek"></a>Zonder een KEK

Tijdens het toevoegen van de schijf OS `$secretUrl` moet worden doorgegeven. De URL is gegenereerd in de sectie ["schijf versleuteling geheim niet gecodeerd, met een KEK"](#disk-encryption-secret-not-encrypted-with-a-kek).

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Met behulp van een KEK

Tijdens het toevoegen van de schijf OS `$KeyEncryptionKey` en `$secretUrl` moet worden doorgegeven. De URL is gegenereerd in de sectie ["schijf versleuteling geheim gecodeerd met een KEK"](#disk-encryption-secret-encrypted-with-a-kek).

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a>Download dit productoverzicht
U kunt deze handleiding downloaden van de [TechNet-galerie](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).


## <a name="for-more-information"></a>Voor meer informatie
[Codering met Azure PowerShell Azure schijf verkennen](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)

[Ontdek de Azure schijfversleuteling met Azure PowerShell - deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
