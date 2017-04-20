<properties
    pageTitle="Veelgestelde vragen over Windows VMs | Microsoft Azure"
    description="Bevat antwoorden op enkele veelgestelde vragen over Windows virtuele machines voor de Resource Manager model gemaakt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Veelgestelde vragen over Windows virtuele Machines 


Dit artikel heeft betrekking op enkele veelgestelde vragen over Windows virtuele machines zijn gemaakt in het implementatiemodel Resource Manager met Azure. Voor de Linux-versie van dit onderwerp, Zie de [Veelgestelde vragen over Linux virtuele Machines](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een Azure VM

Alle abonnees kunnen serversoftware uitvoeren op een Azure virtuele machine. Zie voor informatie over het ondersteuningsbeleid voor het uitvoeren van Microsoft-serversoftware in Azure, [Microsoft-serversoftware die ondersteuning bieden voor Azure virtuele Machines](https://support.microsoft.com/kb/2721672)

Bepaalde versies van Windows 7 en Windows 8.1 zijn beschikbaar voor abonnees van MSDN Azure voordeel en abonnees MSDN Dev en Pay-As-You-Go testen, voor ontwikkel- en taken. Zie voor informatie, instructies en beperkingen, zoals de [Windows-Client-images voor MSDN-abonnees](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?

Elke gegevensschijf kan worden tot 1 TB. Het aantal gegevensschijven die kunt u afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie [voor virtuele Machines](virtual-machines-windows-sizes.md).

Een account Azure opslag biedt opslag voor de schijf en gegevensschijven. Elke schijf is opgeslagen als een blob pagina VHD-bestand. Zie voor details prijzen, [Opslag prijzen Details](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Hoe kan ik mijn virtuele machine openen?

Externe verbinding met extern bureaublad verbinding (RDP) voor een Windows VM maken. Zie voor instructies [hoe u verbinding maken met en aanmelden bij een Azure virtuele machine met Windows](virtual-machines-windows-connect-logon.md). Maximaal twee gelijktijdige verbindingen worden ondersteund, tenzij de server is geconfigureerd als host voor extern bureaublad-Services-sessie.  


Als u problemen met extern bureaublad ondervindt, raadpleegt u [problemen met extern bureaublad-verbindingen met een Windows Azure virtuele Machine](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Als u vertrouwd met Hyper-V bent, kijkt u misschien voor een hulpmiddel dat vergelijkbaar is met VMConnect. Azure biedt niet een vergelijkbaar hulpprogramma omdat consoletoegang tot een virtuele machine wordt niet ondersteund.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Kan ik de tijdelijke schijf (het station D: standaard) gebruiken om gegevens op te slaan?

De tijdelijke schijf niet worden gebruikt voor het opslaan van gegevens. Het is alleen tijdelijke opslag zodat zouden verloren gegevens kunnen niet worden hersteld. Gegevensverlies kan optreden wanneer de virtuele machine wordt verplaatst naar een andere host. Formaat van een virtuele machine, zijn bijwerken van de host of een hardwarefout opgetreden op de host redenen die een virtuele machine kunnen verplaatsen.

Als u een toepassing die moet worden gebruikt, de letter van het station D: hebt, kunt u stationsletters toewijzen zodat de tijdelijke schijf wordt gebruikt door iets anders dan D:. Zie voor instructies de [wijziging van de stationsletter van de tijdelijke schijf met Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Hoe kan ik de stationsletter van de tijdelijke schijf wijzigen?

U kunt de stationsletter wijzigen door het wisselbestand verplaatsen en stationsletters opnieuw toewijzen, maar u moet ervoor zorgen dat u de stappen in een bepaalde volgorde doen. Zie voor instructies de [wijziging van de stationsletter van de tijdelijke schijf met Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kan ik een bestaande VM toevoegen aan een set beschikbaarheid?

Nr. Als u uw VM wilt moet deel uitmaken van een set beschikbaarheid, moet u de VM binnen de set maken. Er is momenteel geen manier een VM toevoegen aan een beschikbaarheid nadat deze is gemaakt.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kan ik een virtuele machine uploaden naar Azure

Ja. Zie voor instructies [voor het uploaden van een afbeelding met een VM Windows Azure](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Kan ik de OS schijf formaat?

Ja. Zie voor instructies, [het uitbreiden van de OS-station van een virtuele Machine in een bronnengroep Azure](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan ik kopiëren of klonen van een bestaande Azure VM?

Ja. Zie voor instructies [hoe u een kopie maken van een virtuele Windows-computer in het implementatiemodel Resource Manager](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Niet waarom Canada midden- en Oost-Canada regio's via Azure Resource Manager?

De twee nieuwe regio's van Canada midden- en Oost-Canada worden niet automatisch geregistreerd voor virtuele machine maken voor bestaande Azure abonnementen. Deze registratie gebeurt automatisch als een virtuele machine wordt geïmplementeerd via de Azure portal naar een andere regio Azure Resource Manager gebruiken. Nadat u een virtuele machine wordt gedistribueerd naar andere Azure regio, moeten de nieuwe regio's beschikbaar zijn voor latere virtuele machines.

## <a name="does-azure-support-linux-vms"></a>Azure biedt ondersteuning voor Linux VMs?

Ja. U maakt snel een Linux VM uit te proberen, Zie [maken een Linux VM in Azure met behulp van de Portal](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kan ik een NIC toevoegen aan mijn VM nadat deze gemaakt?

Nr. Toevoegen van een Netwerkkaart kan alleen worden uitgevoerd bij het maken.

## <a name="are-there-any-computer-name-requirements"></a>Zijn er vereisten voor de computer naam?

Ja. De computernaam mag maximaal 15 tekens lang zijn. Zie [richtlijnen voor naamgeving van infrastructuur](virtual-machines-windows-infrastructure-naming-guidelines.md) voor meer informatie over de naamgeving van uw resources.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Wat zijn de gebruikersnaam bij het maken van een VM?

Gebruikersnamen kunnen maximaal 20 tekens lang zijn en kan niet eindigen met een punt ("."). 

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

Wachtwoorden moeten 123 van 8 tekens lang en 3 van de volgende 4 complexiteitsvereisten voldoen aan:

- Lagere tekens
- Bovenste tekens
- Een cijfer
- Hebt u een speciaal teken (Regex overeenkomen met [\W_])

De volgende wachtwoorden zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Wachtwoord!</td><td style="text-align:center">Wachtwoord1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
