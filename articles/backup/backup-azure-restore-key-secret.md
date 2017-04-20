<properties
    pageTitle="Sleutel sleutel kluis en geheim voor gecodeerde VMs met Azure back-up herstellen | Microsoft Azure"
    description="Informatie over het herstellen van de sleutel sleutel kluis en geheim in Azure back-up met PowerShell"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Sleutel sleutel kluis en geheim voor gecodeerde VMs met Azure back-up terugzetten
In dit artikel gesprekken voert over Azure VM back-up uitvoeren met het herstellen van gecodeerde Azure VMs, als uw sleutel en geheime niet bestaan in de sleutel kluis. Deze stappen kunnen ook worden gebruikt als u wilt bijhouden een afzonderlijke kopie van de sleutel (Key-codering) en geheim (BitLocker Encryption Key) voor de teruggezette VM.

## <a name="pre-requisites"></a>Minimumvereisten

1. **Back-up gecodeerd VMs** - Azure VMs gecodeerde reservekopie is met Azure back-up. Het [beheren van back-up en terugzetten van Azure VMs met PowerShell](backup-azure-vms-automation.md) -artikel voor meer informatie over hoe u een back-up maken van gecodeerde Azure VMs verwijzen.

2. **Configureer Azure sleutel kluis** – Zorg ervoor dat de sleutel van de kluis die sleutels en geheimen moeten worden teruggezet, is al aanwezig. Raadpleeg het artikel [Aan de slag met Azure sleutel kluis](../key-vault/key-vault-get-started.md) voor meer informatie over de sleutel kluis management.

## <a name="setup-recovery-services-vault"></a>Setup recovery services kluis 
Gebruik de volgende stappen aan PowerShell aanmelden en recovery services vault-context instellen

### <a name="log-in-to-azure-powershell"></a>Log in op Azure PowerShell 

Log in bij Azure rekening met de volgende cmdlet

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Set recovery services kluis context

Als u eenmaal bent aangemeld, de volgende cmdlet gebruikt om de lijst met de beschikbare abonnementen

```
PS C:\> Get-AzureRmSubscription
```

Selecteer het abonnement dat resources beschikbaar zijn

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

De context van de kluis met Recovery Services kluis waar de back-up is ingeschakeld voor gecodeerde VMs instellen

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Herstelpunt krijgen 

Selecteer de container in de kluis met gecodeerde Azure virtuele machine

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Het object voor de desbetreffende virtuele machine met behulp van deze container terughalen

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Een matrix van punten herstel voor de geselecteerde back-item in de variabele rp ophalen

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Gecodeerde virtuele machine herstellen
Gebruik de volgende stappen terugzetten van gecodeerde VM, sleutel en het geheim.

### <a name="restore-key"></a>Sleutel herstellen

De array $rp hierboven, gesorteerd in omgekeerde volgorde van de tijd met het meest recente herstelpunt op index 0. Bijvoorbeeld: $rp [0] Hiermee selecteert u het meest recente herstelpunt.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Nadat deze cmdlet is uitgevoerd, wordt een blob-bestand gegenereerd in de opgegeven map op de computer waarop wordt uitgevoerd. Deze blob-bestand vertegenwoordigt sleutel gecodeerde sleutel in gecodeerde vorm.

Herstellen sleutel terug om de sleutel met de volgende cmdlet. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Geheim herstellen

Geheime gegevens terugzetten vanaf een herstelpunt verkregen boven

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
De tekst vóór de vault.azure.net vertegenwoordigt de oorspronkelijke naam van de sleutel kluis. De tekst na geheimen / geheime naam vertegenwoordigt. 

Krijg de geheime naam en waarde uit de uitvoer van de cmdlet uitvoert, voor het geval u wilt dezelfde geheime naam gebruiken. In andere gevallen moet de $secretname hieronder voor het gebruik van de naam van de nieuwe geheime worden bijgewerkt. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Codes voor het geheim ingesteld als VM moet ook worden teruggezet. Voor het label DiskEncryptionKeyFileName moet waarde bevatten van de naam van het geheim dat u wilt gebruiken. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
Waarde voor DiskEncryptionKeyFileName is hetzelfde als geheime naam hierboven verkregen. Waarde voor DiskEncryptionKeyEncryptionKeyURL kan worden verkregen uit de sleutel kluis na het terugzetten van de sleutels terug en met behulp van de cmdlet [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx)   

De geheime terug naar de sleutel kluis instellen

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Virtuele machine herstellen
De bovenstaande PowerShell-cmdlets waarmee u sleutel en geheime weer terugzetten naar de sleutel kluis, als u een reservekopie hebt gecodeerd VM met Azure VM back-up. Raadpleeg na deze terug te zetten, het [beheren van back-up en terugzetten van Azure VMs met PowerShell](backup-azure-vms-automation.md) terugzetten van gecodeerde VMs-artikel.
