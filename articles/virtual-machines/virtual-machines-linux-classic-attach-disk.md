<properties
    pageTitle="Een schijf toevoegen aan een VM Linux | Microsoft Azure"
    description="Hoe een gegevensschijf koppelen aan een Azure virtuele machine waarop Linux wordt uitgevoerd en het initialiseren zodat deze gereed voor gebruik is."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Het koppelen van een schijf met gegevens aan een virtuele Linux Machine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie het [koppelen van een schijf met gegevens met behulp van het implementatiemodel Resource Manager](virtual-machines-linux-add-disk.md).

U kunt zowel lege schijven en schijven met gegevens aan uw Azure VMs koppelen. Beide typen schijven zijn VHD-bestanden die zich in een opslagruimte Azure-account bevinden. Als bij het toevoegen van een schijf met een Linux-machine moet nadat u de schijf toevoegen u initialiseren en formatteren zodat deze gereed voor gebruik is. In dit artikel informatie zowel lege schijven en schijven met al gegevens voor uw VMs als vervolgens initialiseren en formatteren van een nieuwe schijf toevoegen.

> [AZURE.NOTE]Het is raadzaam om een of meer afzonderlijke schijven te gebruiken voor het opslaan van gegevens van een virtuele machine. Wanneer u een Azure virtuele machine maakt, heeft een diskette en een tijdelijke schijf. **Gebruik niet de tijdelijke schijf voor het opslaan van de permanente gegevens.** Zoals de naam al aangeeft, biedt alleen tijdelijke opslag. Het biedt geen redundantie of back-up omdat deze zich niet in Azure opslag.
> De tijdelijke schijf wordt gewoonlijk beheerd door de Agent Azure Linux en automatisch worden gekoppeld aan **/mnt/resource** (of **mnt** op Ubuntu afbeeldingen). Aan de andere kant een gegevensschijf kan de naam door de kernel Linux zoals `/dev/sdc`, en u wilt partitioneren, formatteren en koppelen van deze bron. Zie de [Gebruikershandleiding voor Azure Linux-Agent] [ Agent] voor meer informatie.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Initialiseren van een gegevensschijf in Linux

1. SSH aan uw VM. Zie voor meer informatie [hoe u zich aanmeldt bij een virtuele machine waarop Linux][Logon].

2. Vervolgens moet u de apparaat-id voor de schijf initialiseren met gegevens vinden. Er zijn twee manieren doen:

    een) Grep voor SCSI-apparaten in de logboeken, zoals in de volgende opdracht:

            $sudo grep SCSI /var/log/messages

    Voor recente distributies Ubuntu, moet u wellicht gebruiken `sudo grep SCSI /var/log/syslog` omdat de registratie in `/var/log/messages` mogelijk is standaard uitgeschakeld.

    Hier vindt u de id van de laatste gegevensschijf die is toegevoegd in de berichten die worden weergegeven.

    ![De berichten van schijf ophalen](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    OR

    b) Gebruik de `lsscsi` de opdracht om erachter te komen van de apparaat-id. `lsscsi`kan worden geïnstalleerd door een `yum install lsscsi` (gebaseerd op Red Hat distributies) of `apt-get install lsscsi` (gebaseerd op Debian distributies). U kunt de schijf die u door de _lun_ of **logische-eenheidnummer zoekt**vinden. Bijvoorbeeld het _lun_ voor de schijven die u hebt gekoppeld kan eenvoudig bekijken op `azure vm disk list <virtual-machine>` als:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Vergelijk deze gegevens met de uitvoer van `lsscsi` voor hetzelfde voorbeeld van een virtuele machine:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    Het laatste nummer in de tupel in elke rij is het _lun_. Zie `man lsscsi` voor meer informatie.

3. Typ achter de prompt de volgende opdracht om uw apparaat te maken:

        $sudo fdisk /dev/sdc


4. Wanneer dat wordt gevraagd, typt u **n** om een nieuwe partitie te maken.


    ![Apparaat maken](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Wanneer dat wordt gevraagd, typt u **p** als u de partitie van de primaire partitie. Typ **1** om de eerste partitie en vervolgens invoeren voor het accepteren van de standaardwaarde voor de cilinder. Op sommige systemen, kan dit de standaardwaarden van de eerste en de laatste sectoren, in plaats van de cilinder weergeven. U kunt deze standaardinstellingen te accepteren.


    ![Partitie maken](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Typ **p** kunt u de details over de schijf die wordt opgedeeld.


    ![Gegevens van de schijf weergeven](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Typ **w** de instellingen voor de schijf te schrijven.


    ![De wijzigingen van de schijf schrijven](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Nu kunt u het bestandssysteem op de nieuwe partitie maken. Het partitienummer van de toevoegen aan de apparaat-ID (in het volgende voorbeeld `/dev/sdc1`). In het volgende voorbeeld wordt een ext4 partitie gemaakt op /dev/sdc1:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Bestandssysteem maken](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] SuSE Linux Enterprise 11 alleen ondersteuning voor alleen-lezen toegang voor ext4 bestandssystemen. Het wordt aanbevolen voor deze systemen, het nieuwe bestandssysteem formatteren als ext3 in plaats van ext4.


9. Maak een map voor het koppelen van het nieuwe bestandssysteem als volgt:

        # sudo mkdir /datadrive


10. Ten slotte kunt u het station als volgt koppelen:

        # sudo mount /dev/sdc1 /datadrive

    De gegevensschijf is nu klaar voor gebruik als **/datadrive**.
    
    ![Maak de map en de schijf koppelen](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Het nieuwe station toevoegen aan /etc/fstab:

    Om ervoor te zorgen dat het station automatisch opnieuw wordt gekoppeld na het opnieuw opstarten moet deze worden toegevoegd aan het bestand/etc/fstab. Bovendien verdient de UUID (Universally Unique IDentifier) wordt gebruikt in /etc/fstab om te verwijzen naar het station in plaats van alleen de apparaatnaam (d.w.z. /dev/sdc1). Met behulp van de UUID voorkomt de onjuiste schijf als het besturingssysteem detecteert een schijffout opgetreden tijdens het opstarten en alle overige gegevensschijven vervolgens de apparaat-id's worden toegewezen aan een bepaalde vestiging worden gemonteerd. De UUID van het nieuwe station zoekt, kunt u het hulpprogramma **blkid** :

        # sudo -i blkid

    De uitvoer ziet er ongeveer als volgt:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] Onjuist bewerken van het bestand **/etc/fstab** kan leiden tot een systeem opgestart. Als u niet zeker, Raadpleeg de documentatie van de distributie voor informatie over het bewerken van dit bestand correct. Het verdient ook dat er een back-up van het bestand /etc/fstab wordt gemaakt voordat u het bewerkt.

    Open vervolgens het bestand **/etc/fstab** in een teksteditor:

        # sudo vi /etc/fstab

    In dit voorbeeld gebruiken we de UUID-waarde voor het nieuwe **/dev/sdc1** -apparaat dat is gemaakt in de vorige stappen en het koppelpunt **/datadrive**. Voeg de volgende regel aan het einde van het bestand **/etc/fstab** :

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Of op systemen die zijn gebaseerd op SuSE Linux zult u een iets andere indeling gebruiken:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] De `nofail` optie zorgt ervoor dat de VM begint, zelfs als het bestandssysteem beschadigd is of de schijf tijdens het opstarten niet bestaat. Zonder deze optie, kunt u ondervinden gedrag zoals beschreven in [Niet kan SSH voor Linux VM als gevolg van fouten in FSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    U kunt nu testen of het bestandssysteem correct is gekoppeld door te ontkoppelen en opnieuw het opstellen van het bestandssysteem, dat wil zeggen in het voorbeeld koppelpunt `/datadrive` in de vorige stappen hebt gemaakt:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Als de `mount` opdracht geeft een fout, moet u het/etc/fstab-bestand voor de juiste syntaxis controleren. Als aanvullende gegevens, stations of partities zijn gemaakt, / etc/fstab ook afzonderlijk invoeren.

    Het station beschrijfbare maken met behulp van deze opdracht:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Vervolgens een schijf met gegevens verwijderen zonder de fstab bewerkt, kan ervoor zorgen dat de VM opstarten is mislukt. Als dit vaak het geval is, de meeste distributies bieden ofwel de `nofail` en/of `nobootwait` fstab opties die ervoor zorgen dat een systeem opstart, zelfs als de schijf niet te koppelen aan de opstarttijd. De documentatie van uw distributie voor meer informatie over deze parameters.

### <a name="trimunmap-support-for-linux-in-azure"></a>Ondersteuning voor Linux in Azure TRIM/UNMAP
Sommige Linux kernels ondersteund TRIM/UNMAP om ongebruikte blokken op de schijf ongedaan te maken. Deze bewerkingen zijn vooral nuttig bij standaardopslag in kennis te stellen van Azure dat verwijderde pagina's zijn niet meer geldig en kan worden genegeerd. Verwijderen van pagina's bespaart u kosten als u grote bestanden maakt en vervolgens verwijderen.

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
U kunt meer lezen over het gebruik van uw Linux VM in de volgende artikelen:

- [Hoe aan te melden op een virtuele machine waarop Linux][Logon]

- [Het ontkoppelen van een schijf vanaf een virtuele machine onder Linux](virtual-machines-linux-classic-detach-disk.md)

- [Met behulp van de CLI Azure met het model Klassiek implementatie](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
