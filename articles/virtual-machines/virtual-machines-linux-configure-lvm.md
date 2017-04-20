<properties 
    pageTitle="LVM configureren op een virtuele machine waarop Linux | Microsoft Azure" 
    description="Informatie over het configureren van de LVM op Linux in Azure." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Configureren van de LVM op een Linux VM in Azure

Dit document wordt uitgelegd hoe het logische Volume Manager (LVM) in uw Azure virtuele machine configureren. Hoewel bruikbare LVM op een schijf die is gekoppeld aan de virtuele machine te configureren, standaard de meeste cloud afbeeldingen geen LVM op de OS-schijf is geconfigureerd. Dit is om te voorkomen dat problemen met dubbele volume groepen als de OS schijf is ooit gekoppeld aan een andere VM van de distributie en de tekst, dat wil zeggen tijdens een herstelscenario. Het verdient daarom alleen voor LVM op de gegevensschijven gebruiken.


## <a name="linear-vs-striped-logical-volumes"></a>Lineaire versus striped logische volumes

LVM kan worden gebruikt om een aantal fysieke schijven te combineren in één opslagvolume. Standaard maakt LVM meestal lineaire logische volumes, wat betekent dat de fysieke opslag is samengevoegd. In dit geval wordt lees-en schrijfbewerkingen doorgaans alleen worden verzonden naar een enkele schijf. Daarentegen kunnen we ook striped logische volumes waar lees- en schrijfbewerkingen worden verdeeld over meerdere schijven die zijn opgenomen in de groep volume (dat wil zeggen vergelijkbaar met RAID 0) maken. Voor betere prestaties is het waarschijnlijk zult u de logische volumes stripe, zodat alle gekoppelde gegevensschijven maken gebruik van lees- en schrijfbewerkingen.

Dit document wordt beschreven hoe verschillende data schijven combineren in een enkel volume groep en vervolgens een logische striped volume maken. De volgende stappen zijn enigszins om te werken met de meeste distributies algemene. In de meeste gevallen zijn de hulpprogramma's en werkstromen voor het beheren van LVM op Azure niet fundamenteel anders dan andere omgevingen. Zoals gebruikelijk ook Raadpleeg de leverancier van uw Linux voor documentatie en aanbevolen procedures voor het gebruik van LVM met uw specifieke verdeling.


## <a name="attaching-data-disks"></a>Data schijven koppelen
Een wilt meestal beginnen met twee of meer lege gegevensschijven bij gebruik van LVM. Op basis van de i/o-behoeften, kunt u schijven die zijn opgeslagen in onze standaard opslagcapaciteit, met maximaal 500 i/o/ps per schijf of onze Premium-opslag met maximaal 5000 i/o/ps per schijf koppelen. Dit artikel gaat niet in detail over het inrichten en gegevensschijven koppelen aan een Linux virtuele machine. Zie de Microsoft Azure artikel [koppelen van een schijf](virtual-machines-linux-add-disk.md) voor gedetailleerde instructies over hoe u een schijf met lege gegevens koppelen aan een virtuele Linux machine op Azure.

## <a name="install-the-lvm-utilities"></a>De LVM-hulpprogramma's installeren

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS & Oracle Linux**

        # sudo yum install lvm2

- **12 SLES en openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    Op SLES11 u moet ook /etc/sysconfig/lvm bewerken en instellen `LVM_ACTIVATED_ON_DISCOVERED` ' inschakelen ':

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>LVM configureren
In deze handleiding wordt ervan uitgegaan hebt u drie data schijven we naar als verwijzen gekoppeld aan `/dev/sdc`, `/dev/sdd` en `/dev/sde`. Houd er rekening mee dat deze altijd mogelijk niet dezelfde padnamen in uw VM. U kunt uitvoeren '`sudo fdisk -l`' of een vergelijkbare opdracht om een lijst van de beschikbare schijven.

1. Voorbereiding van de fysieke volumes:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Maak een volume groep. Wij bellen het volume groep 'data-vg01' in het volgende voorbeeld:

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. De logische volumes maken. De opdracht hieronder we maakt één logisch volume genaamd 'data-lv01"te omvatten van het gehele volume groep, maar is ook mogelijk meerdere logische volumes maken in de groep volume.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Het logische volume formatteren

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] Met een SLES11 "-t ext3" in plaats van ext4. SLES11 ondersteunt alleen-lezen toegang tot ext4 filesystems.


## <a name="add-the-new-file-system-to-etcfstab"></a>Het nieuwe bestandssysteem toevoegen aan /etc/fstab

**Let op:** Onjuist bewerken van het bestand /etc/fstab kan leiden tot een systeem opgestart. Als u niet zeker, Zie de verdeling van documentatie voor informatie over het bewerken van dit bestand correct. Het verdient ook dat er een back-up van het bestand /etc/fstab wordt gemaakt voordat u het bewerkt.

1. Maak de gewenste koppelpunt voor het nieuwe bestandssysteem, bijvoorbeeld:

        # sudo mkdir /data


2. Ga naar het pad van logische volumes

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. /Etc/fstab in een teksteditor openen en een vermelding toevoegen voor het nieuwe bestandssysteem, bijvoorbeeld:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Vervolgens opslaan en sluiten /etc/fstab.


4. Test of de post/etc/fstab juist is:

        # sudo mount -a

    Controleer de syntaxis in het bestand/etc/fstab als deze opdracht in een foutbericht wordt weergegeven resulteert.

    Voer vervolgens de `mount` opdracht om ervoor te zorgen het bestandssysteem is gekoppeld:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Optioneel) Failsafe boot parameters in /etc/fstab

    Veel distributies bevatten ofwel de `nobootwait` of `nofail` parameters die kunnen worden toegevoegd aan het/etc/fstab bestand koppelen. Deze parameters kunnen fouten bij het koppelen van een bepaald bestandssysteem en laat het Linux-systeem om door te gaan om op te starten, zelfs als het niet goed koppelen de RAID-bestandssysteem. Raadpleeg de documentatie van uw distributie voor meer informatie over deze parameters.

    Voorbeeld (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
