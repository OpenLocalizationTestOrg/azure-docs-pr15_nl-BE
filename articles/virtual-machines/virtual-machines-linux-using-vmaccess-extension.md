<properties
    pageTitle="Opnieuw instellen op Azure Linux VMs met de extensie VMAccess | Microsoft Azure"
    description="Opnieuw instellen op Azure Linux VMs met de extensie VMAccess."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Beheren van gebruikers, SSH en controleer of het herstellen van schijven op Azure Linux VMs met de extensie VMAccess

In dit artikel wordt beschreven hoe u de extensie Azure VMAcesss gebruiken om te controleren of repareren van een schijf, opnieuw toegang voor gebruikers, gebruikersaccounts beheren of de SSHD configuratie terugzetten op Linux. Het artikel moet:

- een Azure-account ([gratis proberen](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure CLI](../xplat-cli-install.md) vastgelegd met `azure login`.

- de modus Azure CLI _moet in_ Azure Resource Manager `azure config mode arm`.

## <a name="quick-commands"></a>Snelle opdrachten

Er zijn twee manieren om VMAccess op uw Linux VMs:

- Met behulp van de CLI Azure en de vereiste parameters.
- Ruwe JSON bestanden gebruiken die door VMAccess worden verwerkt en vervolgens op te volgen.

Voor de sectie snelle opdracht gaan we gebruiken de CLI Azure `azure vm reset-access` methode. In de volgende voorbeelden van de opdracht vervangen door de waarden met 'voorbeeld' met de waarden uit uw eigen omgeving.

## <a name="create-a-resource-group-and-linux-vm"></a>Maken van een resourcegroep en Linux VM

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Maken van een Debian VM

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Root-wachtwoord opnieuw instellen

Het root-wachtwoord opnieuw instellen:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH-sleutel opnieuw instellen

De SSH-sleutel van een niet-root gebruiker resetten:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Een gebruiker maken

Een gebruiker te maken:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Een gebruiker verwijderen

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>SSHD opnieuw instellen

De SSHD configuratie resetten:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Gedetailleerde scenario

### <a name="vmaccess-defined"></a>VMAccess gedefinieerd:

De schijf van uw Linux VM weergegeven fouten. U op het root-wachtwoord opnieuw instellen voor uw VM Linux of uw persoonlijke SSH-sleutel per ongeluk zijn verwijderd. Als dat is gebeurd in de dagen van het datacenter, moet u er station en open vervolgens de KVM opvragen via de serverconsole. De extensie VMAccess Azure beschouwen als die KVM-switch waarmee u toegang tot de console Linux opnieuw instellen of schijf niveau onderhoud uit te voeren.

Voor de gedetailleerde scenario gaan we de lange vorm van VMAccess waarbij raw JSON-bestanden gebruiken.  Deze VMAccess JSON-bestanden kunnen ook worden aangeroepen vanuit Azure sjablonen.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Gebruik VMAccess om te controleren of herstellen van de schijf van een Linux-VM

Met VMAccess kunt u een fsck doen uitvoeren op de schijf onder uw Linux VM.  U kunt ook een schijf controleren en een schijf herstellen met behulp van een VMAccess doen.

De schijf gebruikt om te controleren en vervolgens repareren dit script VMAccess:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Het script VMAccess uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Gebruikerstoegang instellen op Linux met behulp van VMAccess

Als op uw Linux VM u toegang tot hoofdmap verloren hebt, kunt u een VMAccess script om het root-wachtwoord opnieuw instellen te starten.

Om het root-wachtwoord opnieuw instellen, gebruikt u dit script VMAccess:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Het script VMAccess uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Als u de SSH-sleutel van een niet-root gebruiker herstellen, gebruik dit script VMAccess:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Het script VMAccess uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Gebruik VMAccess voor het beheren van gebruikersaccounts op Linux

VMAccess is een Python-script dat kan worden gebruikt om gebruikers te beheren op uw Linux VM zonder aanmelden en sudo of het hoofdaccount.

U kunt dit script VMAccess een gebruiker maken:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Het script VMAccess uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Gebruik dit script VMAccess om te verwijderen van een gebruiker:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Het script VMAccess uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>De SSHD configuratie terugzetten met behulp van VMAccess

Als u wijzigingen in de configuratie van Linux VMs SSHD aanbrengt en de wijzigingen te controleren voordat u de SSH-verbinding sluit, u mag geen SSH'ing weer in.  VMAccess kan worden gebruikt voor de SSHD configuratie terugzetten naar een bekende juiste configuratie zonder te melden via SSH.

Als u opnieuw wilt gebruiken de SSHD configuratie dit script VMAccess:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Het script VMAccess uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Volgende stappen

Bijwerken van Linux is Azure VMAccess Extensions één methode wijzigingen aanbrengen in een lopende Linux VM.  Ook kunt u hulpmiddelen zoals cloud init en Azure-sjablonen voor het wijzigen van uw VM Linux op te starten.

[Over functies en uitbreidingen van de virtuele machine](virtual-machines-linux-extensions-features.md)

[Sjablonen met Linux VM extensies Azure Resource Manager ontwerpen](virtual-machines-linux-extensions-authoring-templates.md)

[Met behulp van cloud-init een Linux VM aanpassen tijdens het maken van](virtual-machines-linux-using-cloud-init.md)
