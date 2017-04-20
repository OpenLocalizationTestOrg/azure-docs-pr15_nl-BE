<properties
    pageTitle="Inleiding tot Linux in Azure | Microsoft Azure"
    description="Informatie over het gebruik van Linux virtuele machines op Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

#<a name="introduction-to-linux-on-azure"></a>Inleiding tot Linux op Azure

Dit onderwerp bevat een overzicht van enkele aspecten van het gebruik van Linux virtuele machines in de Azure cloud. Implementatie van een virtuele Linux machine is een eenvoudig proces met behulp van een afbeelding in de galerie.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>-Verificatie: Gebruikersnamen, wachtwoorden en sleutels voor SSH

Bij het maken van een Linux virtuele machine de Azure klassieke portal gebruiken, wordt u gevraagd om een gebruikersnaam, wachtwoord of een openbare SSH-sleutel. De keuze van een gebruikersnaam voor de implementatie van een virtuele Linux machine op Azure is onderworpen aan de volgende beperking: de namen van de systeemaccounts (UID < 100) aanwezig in de virtuele machine niet zijn toegestaan, 'root' bijvoorbeeld.


 - Zie [een virtuele Machine waarop Linux maken](virtual-machines-linux-quick-create-cli.md)
 - Zie [het gebruik van SSH met Linux op Azure](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Het verkrijgen van supergebruiker bevoegdheden gebruiken`sudo`

De gebruikersaccount die is opgegeven bij de implementatie van de virtuele machine exemplaar op Azure is een beschermde account. Deze account is geconfigureerd door de Agent Azure Linux kan uitbreiding van bevoegdheden naar de root (beheerder account) met behulp van de `sudo` hulpprogramma. Nadat u bent aangemeld met deze account, is het mogelijk om opdrachten te voeren als root met behulp van de opdrachtsyntaxis van de

    # sudo <COMMAND>

Desgewenst kunt u een root-shell met **sudo s**.

- Zie [werken met root rechten op Linux virtuele machines in Azure](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Configuratie van de firewall

Azure biedt een binnenkomend pakketfilter dat poorten in de klassieke Azure portal connectiviteit. Standaard is de enige toegestane poort SSH. U kunt op uw Linux virtuele machine van toegang tot extra poorten openen door het configureren van eindpunten in de klassieke Azure portal:

 - Zie: [eindpunten op een virtuele Machine instellen](virtual-machines-windows-classic-setup-endpoints.md)

De Linux-afbeeldingen in de galerie met Azure doen niet de *iptables* firewall standaard ingeschakeld. Indien gewenst, kan de firewall worden geconfigureerd voor aanvullende filters.


## <a name="hostname-changes"></a>Wijzigingen van de host-naam

Als u in eerste instantie een instantie van een Linux-installatiekopie implementeert, wordt u verplicht om een host-naam voor de virtuele machine. Zodra de virtuele machine wordt uitgevoerd, wordt deze hostnaam gepubliceerd naar de DNS-servers van platform zodat meerdere virtuele machines met elkaar verbonden IP-adres-lookups met behulp van hostnamen kan uitvoeren.

Als de hostnaam wijzigingen wilt aanbrengen nadat u een virtuele machine is geïmplementeerd, gebruikt u de opdracht

    # sudo hostname <newname>

De Agent Azure Linux bevat functionaliteit voor het wijzigen van de naam automatisch detecteren en configureren van de virtuele machine voor het publiceren van deze wijziging naar de DNS-servers platform en deze wijziging permanent.

 - [Azure Linux Agent-gebruikershandleiding](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Cloud-Init
Cloud-init op Azure biedt extra mogelijkheden voor het bootstrappen van een virtuele machine maken gebruik van afbeeldingen in **Ubuntu** en **CoreOS** .

 - [Het invoeren van aangepaste gegevens](virtual-machines-windows-classic-inject-custom-data.md)
 - [Aangepaste gegevens en Cloud-Init op Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Azure Swap partities maakt met Cloud-Init](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Het gebruik van CoreOS op Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Vastleggen van het Image Virtual Machine

Azure biedt de mogelijkheid voor het vastleggen van de status van een bestaande virtuele machine in een afbeelding die vervolgens kan worden gebruikt voor de implementatie van extra virtuele machine exemplaren. De Agent Azure Linux kan worden gebruikt voor het terugdraaien van de aanpassing die tijdens het inrichtingsproces is uitgevoerd. U kunt de volgende stappen voor het vastleggen van een virtuele machine als een afbeelding:

1. Uitvoeren van **waagent-identiteitsgegevens** provisioning aanpassing ongedaan te maken. Of **waagent-identiteitsgegevens + user** eventueel verwijderen van de account van de gebruiker opgegeven tijdens het aanmaken en alle bijbehorende gegevens.

2. Shut down/uitschakelen van de virtuele machine.

3. Klik op *vastleggen* in de klassieke Azure portal of Powershell of CLI-hulpprogramma's gebruiken voor het vastleggen van de virtuele machine als een afbeelding.

 - Zie: [een virtuele Linux Machine te gebruiken als een sjabloon vastleggen](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Schijven koppelen

Elke virtuele machine heeft een tijdelijke, lokale *schijf resource* gekoppeld. Omdat gegevens op een schijf bron mogelijk niet duurzaam via de computer opnieuw hebt opgestart, wordt het vaak gebruikt door toepassingen en processen die worden uitgevoerd in de virtuele machine voor tijdelijk en **tijdelijke** opslag van gegevens. Het wordt ook gebruikt voor de pagina opslaan of uitwisselen van bestanden voor het besturingssysteem.

Op Linux, is de bron schijf gewoonlijk beheerd door de Agent Azure Linux en automatisch worden gekoppeld aan **/mnt/resource** (of **mnt** op Ubuntu afbeeldingen).


>[AZURE.NOTE] Let op de bron schijf is een **tijdelijke** schijf en kan worden verwijderd en opnieuw opgemaakt wanneer de VM opnieuw wordt opgestart.

De naam onder Linux de gegevensschijf door de kernel als `/dev/sdc`, en gebruikers moeten partitioneren, formatteren en koppelen die resource. Dit valt in de zelfstudie stapsgewijze: [het koppelen van een schijf met gegevens op een virtuele Machine](virtual-machines-linux-classic-attach-disk.md).

 - **Zie ook:** [Software RAID onder Linux configureren](virtual-machines-linux-configure-raid.md)  &  [LVM op een Linux VM in Azure configureren](virtual-machines-linux-configure-lvm.md)

