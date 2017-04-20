<properties
   pageTitle="Schijven op een Linux VM coderen | Microsoft Azure"
   description="Het coderen van schijven op een Linux VM met behulp van de CLI Azure en het implementatiemodel Resource Manager"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Schijven op een Linux VM met behulp van de CLI Azure coderen
Voor uitgebreide virtuele machine (VM) beveiliging en naleving, worden virtuele schijven in Azure gecodeerd in rust. Schijven worden gecodeerd met behulp van cryptografische sleutels die zijn beveiligd in een kluis Azure-sleutel. U bepaalt deze cryptografische sleutels en het gebruik ervan kunt controleren. In dit artikel wordt gedetailleerd beschreven hoe u voor het coderen van virtuele schijven op een Linux VM met behulp van de CLI Azure en het implementatiemodel Resource Manager.


## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende gegevens in de sectie snel de base opdrachten voor het coderen van virtuele schijven op de VM. Meer gedetailleerde informatie en context voor elke stap vindt u de rest van het document, [begint hier](#overview-of-disk-encryption).

U moet de [Meest recente Azure CLI](../xplat-cli-install.md) geïnstalleerd en geregistreerd bij het gebruik van de modus Resource Manager als volgt:

```
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen zijn `myResourceGroup`, `myKeyVault`, en `myVM`.

Eerst de Azure sleutel kluis provider binnen uw abonnement Azure inschakelen en een resourcegroep te maken. Het volgende voorbeeld wordt de naam van een resource-groep `myResourceGroup` in de `WestUS` locatie:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Maak een Azure sleutel kluis. Het volgende voorbeeld wordt een sleutel kluis met de naam `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Het maken van een cryptografische sleutel in uw sleutel kluis en het inschakelen voor schijfversleuteling. Het volgende voorbeeld wordt een sleutel met de naam `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Een eindpunt Azure Active Directory gebruikt voor het verwerken van de verificatie en uitwisselen van cryptografische sleutels van de kluis met sleutel maken. De `--home-page` en `--identifier-uris` hoeft niet te worden werkelijke routeerbaar adres. Voor het hoogste niveau van beveiliging, moeten de client geheimen worden gebruikt in plaats van wachtwoorden. Client geheimen kan momenteel niet de CLI Azure genereren. Geheimen van de client kunnen alleen worden gegenereerd in de portal Azure. Het volgende voorbeeld wordt een Azure Active Directory-eindpunt met de naam `myAADApp` en maakt gebruik van een wachtwoord van `myPassword`. Geef uw eigen wachtwoord als volgt:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Opmerking de `applicationId` in de uitvoer van deze opdracht wordt weergegeven. Deze toepassings-ID wordt gebruikt in de volgende stappen uit:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Een gegevensschijf toevoegen aan een bestaande VM. Het volgende voorbeeld wordt een schijf met gegevens voor een VM met de naam `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Controleer de gegevens voor de sleutel kluis en de sleutel die u hebt gemaakt. U moet de sleutel kluis-ID, de URI en de sleutel URL in de laatste stap. In het volgende voorbeeld controleert de details voor een sleutel kluis met de naam `myKeyVault` en de sleutel met de naam `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Coderen van uw schijven als volgt uw eigen parameternamen in te voeren:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

De CLI Azure biedt uitgebreide fouten niet tijdens het coderingsproces. Raadpleeg voor meer informatie over probleemoplossing, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Als de bovenstaande opdracht veel variabelen heeft en u krijgt niet veel aangegeven waarom het proces mislukt, is een voorbeeld van de volledige opdracht als volgt:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Ten slotte de revisiestatus de codering nogmaals om te bevestigen dat uw virtuele schijven nu zijn gecodeerd. Het volgende voorbeeld wordt de status van een VM met de naam `myVM` in de `myResourceGroup` groep:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling
Virtuele schijven op Linux VMs worden gecodeerd met behulp van [dm-crypt](https://wikipedia.org/wiki/Dm-crypt)rusten. Er zijn geen kosten voor het coderen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure sleutel kluis met bescherming van de software of u kunt importeren of genereren de sleutels in de Hardware Security Modules (HSM's) gecertificeerde aan FIPS 140-2 niveau 2 normen. U behoudt de controle over deze cryptografische sleutels en het gebruik ervan kunt controleren. Deze cryptografische sleutels worden gebruikt voor het coderen en decoderen van virtuele schijven die zijn aangesloten op uw VM. Een eindpunt Azure Active Directory biedt een veilig mechanisme voor de afgifte van deze cryptografische sleutels zoals VMs worden ingeschakeld en uitgeschakeld.

Het proces voor het coderen van een VM is als volgt:

1. Maak een cryptografische sleutel in een sleutel Azure kluis.
2. Configureer de cryptografische sleutel worden gebruikt voor het coderen van schijven.
3. Als u wilt lezen de cryptografische sleutel van de kluis Azure sleutel, een eindpunt met Azure Active Directory met de juiste machtigingen te maken.
4. Hiermee geeft u de opdracht voor het coderen van uw virtuele schijven, die het eindpunt Azure Active Directory en de juiste cryptografische sleutel moet worden gebruikt.
5. Het eindpunt Azure Active Directory vraagt de vereiste cryptografische sleutel van Azure sleutel kluis.
6. De virtuele schijven worden gecodeerd met behulp van de opgegeven cryptografische sleutel.


## <a name="supporting-services-and-encryption-process"></a>Ondersteunende diensten en codering
Schijfversleuteling is gebaseerd op de volgende extra onderdelen:

- **Azure sleutel kluis** - gebruikt om cryptografische sleutels veilig te stellen en geheimen die worden gebruikt voor het proces van schijf codering/decodering. 
  - Als dit bestaat, kunt u een bestaande sleutel kluis voor Azure. U hoeft niet te reserveren een kluis met sleutel voor het coderen van schijven.
  - Afzonderlijke administratieve grenzen en de belangrijkste zichtbaarheid, kunt u een speciale sleutel kluis.
- **Azure Active Directory** - zorgt voor veilige uitwisseling van vereiste cryptografische sleutels en -verificatie voor de aangevraagde acties. 
  - Meestal kunt u een bestaand exemplaar van Azure Active Directory voor de huisvesting van uw toepassing. 
  - De toepassing is meer van een eindpunt voor de sleutel kluis en virtuele Machine services aan te vragen en de juiste cryptografische sleutels ophalen afgegeven. U ontwikkelt een toepassing die is geïntegreerd met Active Directory Azure niet.


## <a name="requirements-and-limitations"></a>Vereisten en beperkingen
Ondersteunde scenario's en vereisten voor de codering van de schijf:

- De volgende Linux server SKU - CentOS, Ubuntu, SUSE en SUSE Linux Enterprise Server (SLES) en Red Hat Enterprise Linux.
- Alle resources (zoals sleutel kluis opslag account en VM) moeten zich in hetzelfde gebied van Azure en abonnement.
- Standaard A, D, DS, GS en G-serie VMs.

Schijfversleuteling wordt momenteel niet ondersteund in de volgende scenario's:

- Basic laag VMs.
- VMs gemaakt met behulp van het model Klassiek implementatie.
- OS schijf codering op Linux VMs uitschakelen.
- De cryptografiesleutels op een reeds gecodeerde Linux VM bijwerken.


## <a name="create-the-azure-key-vault-and-keys"></a>Maak de Azure sleutel kluis en sleutels
Om de rest van deze handleiding hebt voltooid, moet u de [Meest recente Azure CLI](../xplat-cli-install.md) geïnstalleerd en geregistreerd bij het gebruik van de modus Resource Manager als volgt:

```
azure config mode arm
```

In de voorbeelden is de opdracht vervangen door alle voorbeeldparameters van de namen, de locatie en uw eigen waarden. De volgende voorbeelden gebruiken een Verdrag van `myResourceGroup`, `myKeyVault`, `myAADApp`, enz.

De eerste stap is voor het maken van een kluis Azure sleutel voor het opslaan van uw cryptografiesleutels. Azure sleutel kluis kunt opslaan, sleutels, geheimen of wachtwoorden kunt u ze veilig implementeren in uw toepassingen en services. Voor de codering van de virtuele schijf met sleutel kluis kunt u een cryptografiesleutel die wordt gebruikt voor het coderen of decoderen van uw virtuele schijven opgeslagen. 

De provider Azure sleutel kluis in uw abonnement Azure inschakelen en vervolgens een bronnengroep maken. In het volgende voorbeeld maakt u een groep met de naam `myResourceGroup` in de `WestUS` locatie:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

De Azure sleutel kluis met de cryptografische sleutels en de bijbehorende compute bronnen zoals opslag en de VM zelf moet zich bevinden in hetzelfde gebied. Het volgende voorbeeld wordt een Azure sleutel kluis met de naam `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Cryptografische sleutels met de software of Hardware Security Model (HSM) bescherming kunnen worden opgeslagen. Met een HSM is een premium sleutel kluis vereist. Er is een extra kosten voor het maken van een premie sleutel kluis in plaats van standaard sleutel kluis die sleutels software beschermd worden opgeslagen. U maakt een premie sleutel kluis, in de voorgaande stap toevoegen `--sku Premium` aan de opdracht. Het volgende voorbeeld worden de sleutels software beschermd omdat we een standaard sleutel kluis gemaakt. 

Voor beide modellen bescherming moet de Azure platform toegang om te vragen de cryptografische sleutels wanneer de VM opgestart voor het decoderen van de virtuele schijven. Een coderingssleutel binnen uw sleutel kluis maken en vervolgens instellen voor gebruik met de codering van de virtuele schijf. Het volgende voorbeeld wordt een sleutel met de naam `myKey` en wordt vervolgens voor schijf-codering:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>De Azure Active Directory-toepassing maken
Wanneer de virtuele schijven worden gecodeerd of gedecodeerd, gebruikt u een eindpunt voor het verwerken van de verificatie en cryptografische sleutels uit sleutel kluis uitwisselen. Dit eindpunt, Azure Active Directory-toepassing, kunt de Azure platform voor het aanvragen van de juiste cryptografiesleutels voor de VM. Een standaardexemplaar Azure Active Directory vindt u in uw abonnement, al veel organisaties zijn speciaal bestemd voor Azure Active Directory-adreslijsten.

Als u niet een volledige Azure Active Directory-toepassing, maakt de `--home-page` en `--identifier-uris` parameters in het volgende voorbeeld hoeft niet te worden werkelijke routeerbaar adres. In het volgende voorbeeld bevat ook een geheim wachtwoord gebaseerde in plaats van genereren van sleutels van binnen de portal Azure. Als deze periode genereren van sleutels is niet mogelijk uit de CLI Azure. 

Uw Azure Active Directory-toepassing maken. Het volgende voorbeeld wordt een toepassing met de naam `myAADApp` en maakt gebruik van een wachtwoord van `myPassword`. Geef uw eigen wachtwoord als volgt:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Maak een notitie van de `applicationId` die wordt geretourneerd in de uitvoer van deze opdracht. Deze toepassings-ID wordt gebruikt in sommige van de resterende stappen. Maak vervolgens een service principal name (SPN), zodat de toepassing toegankelijk in uw omgeving. Om te coderen of decoderen virtuele schijven, moeten de machtigingen voor de cryptografische sleutel opgeslagen in een kluis met sleutel toe te staan de toepassing Azure Active Directory te lezen van de sleutels worden ingesteld. 

De SPN maken en stel de juiste machtigingen als volgt:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Een virtuele schijf toevoegen en codering status bekijken
Sommige virtuele schijven daadwerkelijk te coderen kunt u een schijf toevoegen aan een bestaande VM. Een schijf van 5Gb gegevens aan een bestaande VM als volgt toevoegen:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

De virtuele schijven zijn die momenteel niet gecodeerd. De huidige status van de codering van uw VM als volgt bekijken:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Virtuele schijven te versleutelen
Als u wilt nu de virtuele schijven coderen, u bij elkaar brengen de eerdere onderdelen:

1. De Azure Active Directory-toepassing en het wachtwoord opgeven.
2. Geef de sleutel kluis voor het opslaan van de metagegevens voor uw versleutelde schijven.
3. Geef de cryptografische sleutels te gebruiken voor de werkelijke codering en decodering.
4. Geef op of u wilt coderen van de OS-schijf, de gegevensschijven of alle.

Hiermee kunt de details voor de Azure sleutel kluis en de sleutel die u hebt gemaakt, moet de sleutel kluis-ID, URI, en vervolgens belangrijke URL in de laatste stap te controleren:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Codeer uw virtuele schijven met behulp van de uitvoer van de `azure keyvault show` en `azure keyvault key show` -opdrachten als volgt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Als de bovenstaande opdracht veel variabelen heeft, wordt in het volgende voorbeeld de volledige opdracht voor verwijzing:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

De CLI Azure biedt uitgebreide fouten niet tijdens het coderingsproces. Raadpleeg voor meer informatie over probleemoplossing, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` op de VM u versleutelt.

Ten slotte kunt codering revisiestatus nogmaals om te bevestigen dat uw virtuele schijven nu zijn gecodeerd:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Gegevensschijven meer toevoegen
Zodra u uw gegevensschijven hebt gecodeerd, kunt u later extra virtuele schijven toevoegen aan uw VM en ook versleuteld. Bij het uitvoeren van de `azure vm enable-disk-encryption` opdracht, verhogen de reeks versie met behulp van de `--sequence-version` parameter. Deze parameter sequence versie kunt u herhaalde bewerkingen uitvoeren op de dezelfde VM.

U kunt bijvoorbeeld een tweede virtuele schijf toevoegen aan uw VM als volgt:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

De opdracht voor het coderen van de virtuele schijven opnieuw toevoegen van deze tijd de `--sequence-version` parameter en de waarde van onze eerste uitvoeren als volgt verhogen:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van Azure sleutel kluis, met inbegrip van het verwijderen van cryptografische sleutels en kluizen, [sleutel kluis beheren CLI](../key-vault/key-vault-manage-with-cli.md).
- Zie voor meer informatie over codering van de schijf, zoals het voorbereiden van een gecodeerde aangepaste VM te uploaden naar Azure, [Azure schijf coderen](../security/azure-security-disk-encryption.md).