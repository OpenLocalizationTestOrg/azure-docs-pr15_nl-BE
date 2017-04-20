<properties
    pageTitle="Een schijf toevoegen voor Linux VM | Microsoft Azure"
    description="Meer informatie over een permanente schijf toevoegen aan uw Linux VM"
    keywords="Linux virtuele machine, resource schijf toevoegen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Een schijf toevoegen aan een Linux VM

In dit artikel ziet u hoe een permanente schijf toegevoegd aan uw VM zodat u kunt uw gegevens - zelfs als uw VM wordt door de service als gevolg van het formaat of onderhoud. Als u een schijf toevoegt, moet u [De CLI Azure](../xplat-cli-install.md) geconfigureerd in de modus Resource Manager (`azure config mode arm`).  

## <a name="quick-commands"></a>Snelle opdrachten

In de volgende voorbeelden van de opdracht vervangen door de waarden tussen &lt; en &gt; met de waarden uit uw eigen omgeving.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Een schijf toevoegen

Het is snel een nieuwe schijf toevoegen. Type `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` te maken en een nieuwe GB schijf voor de VM te koppelen. Als u een account voor opslag niet expliciet identificeren, wordt maakt u schijven geplaatst in dezelfde account opslag waar de OS schijf zich bevindt.  Deze moet er ongeveer als volgt uit:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Uitvoer

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Verbinding maken met de Linux VM de nieuwe schijf koppelen

> [AZURE.NOTE] Dit onderwerp is verbonden met een VM met behulp van gebruikersnamen en wachtwoorden. Als u paren van openbare en persoonlijke sleutels wilt communiceren met uw VM, Zie [gebruiken SSH met Linux op Azure](virtual-machines-linux-mac-create-ssh-keys.md). Kunt u de **SSH** -verbinding van VMs gemaakt met de `azure vm quick-create` opdracht met behulp van de `azure vm reset-access` opdracht **SSH** toegang volledig herstellen, toevoegen of verwijderen gebruikers, of veilige toegang tot het openbare sleutel bestanden toevoegen.

U moet SSH in uw Azure VM aan partitie, opmaken en uw nieuwe schijf koppelen zodat uw VM Linux kan worden gebruikt. Als u niet bekend bent met het maken van een verbinding met **ssh**, heeft de opdracht de vorm `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, en ziet eruit als het volgende:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Uitvoer

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Nu dat u met uw VM verbonden bent, bent u klaar voor het koppelen van een schijf.  Zoek eerst de schijf met behulp van `dmesg | grep SCSI` (de methode die u gebruikt voor het ontdekken van de nieuwe schijf kan variëren). In dit geval wordt er ongeveer als volgt:

```bash
dmesg | grep SCSI
```

Uitvoer

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

en voor dit onderwerp, de `sdc` schijf is die we willen. De schijf met partities `sudo fdisk /dev/sdc` --aangenomen dat in uw geval de schijf is `sdc`, een primaire schijf maken op partitie 1 en de andere standaardinstellingen accepteren.

```bash
sudo fdisk /dev/sdc
```

Uitvoer

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

De partitie maken door te typen `p` bij de opdrachtprompt:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

En een bestandssysteem voor de partitie met de opdracht **mkfs** , geven het type bestandssysteem en de naam van het apparaat schrijven. In dit onderwerp, we maken gebruik van `ext4` en `/dev/sdc1` van boven:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Uitvoer

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Nu we een directory maken te koppelen met het bestandssysteem via `mkdir`:

```bash
sudo mkdir /datadrive
```

En koppelt u de map met behulp van `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

De gegevensschijf is nu klaar voor gebruik als `/datadrive`.

```bash
ls
```

Uitvoer

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Om ervoor te zorgen dat het station automatisch opnieuw wordt gekoppeld na het opnieuw opstarten moet deze worden toegevoegd aan het bestand/etc/fstab. Bovendien is het raadzaam de UUID (Universally Unique IDentifier) wordt gebruikt in/etc/fstab om te verwijzen naar het station in plaats van alleen de naam van het apparaat (zoals, `/dev/sdc1`). Als het besturingssysteem een schijffout opgetreden tijdens het opstarten detecteert, voorkomt met behulp van de UUID de onjuiste schijf wordt gekoppeld aan een bepaalde locatie. Resterende gegevensschijven zou worden toegewezen die hetzelfde apparaat-id. Gebruik het hulpprogramma **blkid** om de UUID van het nieuwe station:

```bash
sudo -i blkid
```

De uitvoer ziet er ongeveer als volgt:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Onjuist bewerken van het bestand **/etc/fstab** kan leiden tot een systeem opgestart. Als u niet zeker, Raadpleeg de documentatie van de distributie voor informatie over het bewerken van dit bestand correct. Het verdient ook dat er een back-up van het bestand /etc/fstab wordt gemaakt voordat u het bewerkt.

Open vervolgens het bestand **/etc/fstab** in een teksteditor:

```bash
sudo vi /etc/fstab
```

In dit voorbeeld gebruiken we de UUID-waarde voor het nieuwe **/dev/sdc1** -apparaat dat is gemaakt in de vorige stappen en het koppelpunt **/datadrive**. Voeg de volgende regel aan het einde van het bestand **/etc/fstab** :

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Een schijf met gegevens later verwijderen zonder het bewerken van fstab kan ervoor zorgen dat de VM opstarten is mislukt. De meeste distributies bieden ofwel de `nofail` en/of `nobootwait` fstab opties. Met deze opties kunt een systeem opstart, zelfs als de schijf niet te koppelen bij het opstarten. De documentatie van uw distributie voor meer informatie over deze parameters.

>[AZURE.NOTE] De optie **nofail** zorgt ervoor dat de VM begint, zelfs als het bestandssysteem beschadigd is of de schijf tijdens het opstarten niet bestaat. Zonder deze optie, kunt u tegenkomen gedrag zoals beschreven in [Niet kan SSH voor Linux VM als gevolg van fouten in FSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>Ondersteuning voor Linux in Azure TRIM/UNMAP
Sommige Linux kernels ondersteund TRIM/UNMAP om ongebruikte blokken op de schijf ongedaan te maken. Dit is vooral handig in de standaard opslag in kennis te stellen van Azure dat verwijderde pagina's zijn niet meer geldig en kan worden genegeerd. Dit bespaart kosten als u grote bestanden maakt en vervolgens verwijderen.

Er zijn TRIM inschakelen op twee manieren ondersteuning in uw Linux VM. Zoals gebruikelijk, neem contact op met uw verdeling voor de aanbevolen werkwijze:

- Gebruik de `discard` optie in mount `/etc/fstab`, bijvoorbeeld:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- U kunt ook uitvoeren de `fstrim` opdracht handmatig vanaf de opdrachtregel of toe te voegen aan je crontab regelmatig wordt uitgevoerd:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Het oplossen van problemen
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Volgende stappen

- Vergeet niet dat de nieuwe schijf is niet beschikbaar voor de VM als opnieuw wordt opgestart als u deze gegevens naar het bestand [fstab schrijven](http://en.wikipedia.org/wiki/Fstab) .
- Bekijk dat uw Linux VM juist is geconfigureerd, zodat de aanbevelingen van het [optimaliseren van de prestaties van uw Linux-machine](virtual-machines-linux-optimization.md) .
- Uw opslagcapaciteit uitbreiden door het toevoegen van extra schijven en [RAID configureren](virtual-machines-linux-configure-raid.md) voor extra prestaties.
