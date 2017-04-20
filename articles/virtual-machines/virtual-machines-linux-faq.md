<properties
    pageTitle="Veelgestelde vragen over Linux VMs | Microsoft Azure"
    description="Bevat antwoorden op enkele veelgestelde vragen over Linux virtuele machines zijn gemaakt met het objectmodel Resource Manager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Veelgestelde vragen over Linux virtuele Machines

In dit artikel komen enkele veelgestelde vragen over Linux virtuele machines zijn gemaakt in het implementatiemodel Resource Manager met Azure. Zie [Veelgestelde vragen over Windows virtuele Machines](virtual-machines-windows-faq.md) voor de Windows-versie van dit onderwerp

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een Azure VM

Alle abonnees kunnen serversoftware uitvoeren op een Azure virtuele machine. Voor meer informatie, Zie [Linux op Azure-Endorsed verdelingen](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?

Elke gegevensschijf kan worden tot 1 TB. Het aantal gegevensschijven die kunt u afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie [voor virtuele Machines](virtual-machines-linux-sizes.md).

Een account Azure opslag biedt opslag voor de schijf en gegevensschijven. Elke schijf is opgeslagen als een blob pagina VHD-bestand. Zie voor details prijzen, [Opslag prijzen Details](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Hoe kan ik mijn virtuele machine openen?

Externe verbinding aan te melden op de virtuele machine, met behulp van Secure Shell (SSH). Raadpleeg de instructies voor het verbinden [van Windows](virtual-machines-linux-ssh-from-windows.md) of [Linux en Mac](virtual-machines-linux-mac-create-ssh-keys.md). SSH kan maximaal 10 verbindingen tegelijk. U kunt dit aantal verhogen door het configuratiebestand bewerken.


Raadpleeg [problemen met Secure Shell (SSH)-verbindingen](virtual-machines-linux-troubleshoot-ssh-connection.md)als u problemen ondervindt.


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Kan ik de tijdelijke schijf (/ dev/sdb1) gebruiken om gegevens op te slaan?

Gebruik niet de tijdelijke schijf (/ dev/sdb1) om gegevens op te slaan. Is er alleen voor tijdelijke opslag. Verloren gegevens kunnen niet worden hersteld.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan ik kopiëren of klonen van een bestaande Azure VM?

Ja. Zie voor instructies [hoe u een kopie maken van een Linux virtuele machine in het implementatiemodel Resource Manager](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Niet waarom Canada midden- en Oost-Canada regio's via Azure Resource Manager?

De twee nieuwe regio's van Canada midden- en Oost-Canada worden niet automatisch geregistreerd voor virtuele machine maken voor bestaande Azure abonnementen. Deze registratie gebeurt automatisch als een virtuele machine wordt geïmplementeerd via de Azure portal naar een andere regio Azure Resource Manager gebruiken. Nadat u een virtuele machine wordt gedistribueerd naar andere Azure regio, moeten de nieuwe regio's beschikbaar zijn voor latere virtuele machines.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kan ik een NIC toevoegen aan mijn VM nadat deze gemaakt?

Nr. Toevoegen van een Netwerkkaart kan alleen worden uitgevoerd bij het maken.


## <a name="are-there-any-computer-name-requirements"></a>Zijn er vereisten voor de computer naam?

Ja. De computernaam mag maximaal 64 tekens lang zijn. Zie [richtlijnen voor naamgeving van infrastructuur](virtual-machines-linux-infrastructure-naming-guidelines.md) voor meer informatie over de naamgeving van uw resources.


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Wat zijn de gebruikersnaam bij het maken van een VM?

Gebruikersnamen moet 1-64 tekens lang zijn.

De volgende gebruikersnamen zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">beheerder </td><td style="text-align:center"> Admin </td><td style="text-align:center"> gebruiker </td><td style="text-align:center"> Gebruiker1</td>
    </tr>
    <tr>
        <td style="text-align:center">Test </td><td style="text-align:center"> User2 </td><td style="text-align:center"> Test1 </td><td style="text-align:center"> User3</td>
    </tr>
    <tr>
        <td style="text-align:center">Beheer1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> een</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">back-up </td><td style="text-align:center"> console </td><td style="text-align:center"> David </td><td style="text-align:center"> Gast</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> eigenaar </td><td style="text-align:center"> hoofdmap </td><td style="text-align:center"> Server</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> ondersteuning </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">Test2 </td><td style="text-align:center"> Test3 </td><td style="text-align:center"> User4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Wat zijn de vereisten voor wachtwoorden bij het maken van een VM?

Wachtwoorden moeten 6-72 tekens lang en 3 van de volgende 4 complexiteitsvereisten voldoen aan:

- Lagere tekens
- Bovenste tekens
- Een cijfer
- Hebt u een speciaal teken (Regex overeenkomen met [\W_])

De volgende wachtwoorden zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Wachtwoord!</td>
        <td style="text-align:center">Wachtwoord1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
