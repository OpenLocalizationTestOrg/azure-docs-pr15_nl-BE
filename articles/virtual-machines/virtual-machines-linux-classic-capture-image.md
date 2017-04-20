<properties
    pageTitle="Een installatiekopie van een Linux VM | Microsoft Azure"
    description="Informatie over hoe u een installatiekopie van een Linux-gebaseerde Azure virtual machine (VM) gemaakt met de klassieke implementatiemodel."
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
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Een klassieke Linux virtuele machine als een afbeelding vastleggen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](virtual-machines-linux-capture-image.md).

In dit artikel wordt beschreven hoe u voor het vastleggen van een klassieke Azure virtuele machine als een afbeelding naar een andere virtuele machines maken waarop Linux wordt uitgevoerd. Deze afbeelding bevat de OS schijf en gegevensschijven die zijn gekoppeld aan de virtuele machine. Het bevat geen netwerkconfiguratie, dus u configureren dat moet bij het maken van de andere virtuele machines in de afbeelding.

Azure slaat de afbeelding onder **afbeeldingen**, samen met alle afbeeldingen die je hebt geüpload. Zie voor meer informatie over afbeeldingen [Over Virtual Machine-afbeeldingen in Azure] [].

## <a name="before-you-begin"></a>Voordat u begint

Deze stappen wordt ervan uitgegaan dat u hebt al een Azure virtuele machine met behulp van het model Klassiek implementatie gemaakt en geconfigureerd, het besturingssysteem, met inbegrip van alle gegevensschijven koppelen. Als u nodig hebt voor het maken van een VM, Lees [het maken van een Linux virtuele Machine] [].


## <a name="capture-the-virtual-machine"></a>De virtuele machine vastleggen

1. [Verbinding maken met de virtuele machine](virtual-machines-linux-mac-create-ssh-keys.md) met behulp van een SSH-client van uw keuze.

2. Typ de volgende opdracht in het venster SSH. De uitvoer van `waagent` kan enigszins variëren afhankelijk van de versie van dit hulpprogramma:

    `sudo waagent -deprovision+user`

    Deze opdracht wordt geprobeerd het systeem schoon en geschikt voor reprovisioning. Deze bewerking worden de volgende taken uitgevoerd:

    - SSH host sleutels verwijderen (als Provisioning.RegenerateSshHostKeyPair 'y' in het configuratiebestand)
    - Configuratie van de nameserver in /etc/resolv.conf wordt gewist
    - Hiermee verwijdert u de `root` het wachtwoord van de gebruiker uit/etc/shadow (als Provisioning.DeleteRootPassword 'y' in het configuratiebestand)
    - Hiermee verwijdert u DHCP-clientleases in cache
    - Hiermee stelt u de hostnaam localhost.localdomain
    - Hiermee verwijdert u de laatste gebruiker ingerichte account (afkomstig van /var/lib/waagent) **en de bijbehorende gegevens**.

    >[AZURE.NOTE] Deprovisioning verwijdert alle bestanden en gegevens ' generaliseren ' van de afbeelding. Deze opdracht alleen uitvoeren op een virtuele machine die u wilt vastleggen als een nieuwe sjabloon in de afbeelding. Dit garandeert niet dat de afbeelding van alle gevoelige informatie is uitgeschakeld of geschikt voor distributie aan derden is.


3. Typ **j** om door te gaan. U kunt toevoegen de `-force` parameter om te voorkomen dat dit bevestigen.

4. Typ **Exit** om af te sluiten van de SSH-client.

    >[AZURE.NOTE] De resterende stappen wordt ervan uitgegaan dat u al hebt [geïnstalleerd de CLI Azure](../xplat-cli-install.md) op uw clientcomputer. De volgende stappen is ook mogelijk in de [Azure klassieke portal] [].

5. Open vanaf de clientcomputer Azure CLI en meld u aan bij uw abonnement Azure. Lees voor meer informatie [verbinding maken met een Azure-abonnement van de CLI Azure](../xplat-cli-connect.md).

6. Zorg ervoor dat u in de modus Beheer Service:

    `azure config mode asm`

7. Sluit de virtuele machine die is al in de vorige stappen met deprovisioned:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] U kunt alle virtuele machines die worden gemaakt in uw abonnement met uitzoeken`azure vm list`

8. Wanneer de virtuele machine wordt gestopt, vastleggen van de afbeelding met de opdracht:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Typ de naam van de afbeelding die u wilt dat in plaats van een _nieuwe image-naam_. Met deze opdracht maakt een gegeneraliseerde installatiekopie van het besturingssysteem. De `-t` subopdracht verwijdert u de oorspronkelijke virtuele machine.

9.  De nieuwe afbeelding is nu beschikbaar in de lijst met afbeeldingen die kunnen worden gebruikt om een nieuwe virtuele machines configureren. U kunt deze weergeven met de opdracht:

    `azure vm image list`

    Op de [Azure klassieke portal] []wordt deze in de lijst met **afbeeldingen** .

    ![Vastleggen van het image voltooid](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Volgende stappen
De afbeelding is klaar om te worden gebruikt voor het maken van virtuele machines. U kunt de opdracht Azure CLI `azure vm create` en geef de naam van de afbeelding u hebt gemaakt. Zie [met behulp van de CLI Azure met klassieke implementatiemodel](../virtual-machines-command-line-tools.md) voor meer informatie over de opdracht. Ook de [Azure klassieke portal] [] gebruiken voor het maken van een aangepaste virtuele machine met behulp van de methode **Van de galerie** en selecteert u de afbeelding die u hebt gemaakt. Zie [een aangepaste virtuele Machine maken] [] voor meer informatie.

**Zie ook:** [Azure Linux Agent-gebruikershandleiding](virtual-machines-linux-agent-user-guide.md)

[Azure klassieke portal]: http://manage.windowsazure.com
[Informatie over afbeeldingen in Azure Virtual Machine]: virtual-machines-linux-classic-about-images.md
[Het maken van een aangepaste virtuele Machine]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Het maken van een virtuele Machine van Linux]: virtual-machines-linux-classic-create-custom.md
