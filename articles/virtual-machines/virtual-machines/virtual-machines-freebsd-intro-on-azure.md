<properties
   pageTitle="Inleiding tot FreeBSD op Azure | Microsoft Azure"
   description="Klik hier voor informatie over het gebruik van virtuele machines voor FreeBSD op Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>Inleiding tot FreeBSD op Azure
Dit onderwerp bevat een overzicht van het uitvoeren van een virtuele machine van FreeBSD in Azure.

## <a name="overview"></a>Overzicht
FreeBSD voor Microsoft Azure is een geavanceerd besturingssysteem gebruikt om power moderne servers, desktops, en ingesloten platforms. De afbeelding 10.3 FreeBSD wordt geleverd door Microsoft Corporation en is beschikbaar in Azure. Het is gebaseerd op de release van FreeBSD 10.3 en Azure VM Gast Agent [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) is geïnstalleerd. De agent is verantwoordelijk voor de communicatie tussen de FreeBSD VM en Azure leefklimaat voor bewerkingen, zoals het inrichten van de VM bij eerste gebruik (gebruikersnaam, wachtwoord, hostnaam, enz.) en de functionaliteit voor selectieve VM-extensies inschakelen.
Voor toekomstige versies van FreeBSD, de strategie is op de hoogte blijven en de nieuwste versies beschikbaar kort nadat ze zijn vrijgegeven door de FreeBSD release engineering team. De aanstaande release is [11 FreeBSD](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>Implementatie van een virtuele machine van FreeBSD
Implementatie van een virtuele machine van FreeBSD is een eenvoudig proces met een afbeelding van de [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/).

## <a name="vm-extensions-for-freebsd"></a>VM-uitbreidingen voor FreeBSD
Hieronder volgen de ondersteunde extensies voor VM in FreeBSD.

### <a name="vmaccess"></a>VMAccess

De [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) -extensie kunt doen:

- Het wachtwoord van de oorspronkelijke sudo gebruiker opnieuw instellen.
- Maak een nieuwe sudo gebruiker met het opgegeven wachtwoord.
- Stel de sleutel openbare host-sleutel gegeven.
- Reset de openbare host-sleutel tijdens het VM ingericht als de host-sleutel is opgegeven.
- De SSH-poort (22) openen en herstellen van de sshd_config als reset_ssh is ingesteld op true.
- De bestaande gebruiker te verwijderen.
- Schijven controleren.
- Een extra schijf herstellen.

### <a name="customscript"></a>CustomScript

De [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) -extensie kunt doen:

- Als geleverd, download u aangepaste scripts van Azure opslag of externe openbare opslag (bijvoorbeeld GitHub).
- Het punt post script uitvoeren.
- Ondersteuning voor inline-opdrachten.
- Windows-stijl nieuwe regel in shell en Python scripts automatisch worden geconverteerd.
- Stuklijst in shell en Python scripts automatisch verwijderen.
- Beschermen van vertrouwelijke gegevens in de CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>-Verificatie: gebruikersnamen, wachtwoorden en sleutels voor SSH
Als u een virtuele FreeBSD machine met behulp van de portal Azure, moet u een gebruikersnaam, wachtwoord of openbare SSH-sleutel opgeven.
Gebruikersnamen voor de implementatie van een virtuele machine van FreeBSD op Azure mag niet overeenkomen met de namen van de systeemaccounts (UID < 100) al in de virtuele machine (zoals ' root').
Op dit moment wordt alleen de RSA SSH-sleutel ondersteund. Een multiline SSH-sleutel moet beginnen met '---BEGIN SSH2 openbare sleutel---' en eindigen met '---END SSH2 openbare sleutel---'.

## <a name="obtaining-superuser-privileges"></a>Het verkrijgen van supergebruiker bevoegdheden
De gebruikersaccount die is opgegeven bij de implementatie van de virtuele machine exemplaar op Azure is een beschermde account. Het sudo-pakket is geïnstalleerd in de gepubliceerde FreeBSD afbeelding.
Nadat u bent aangemeld via deze gebruikersaccount, kunt u opdrachten uitvoeren als root met behulp van de opdrachtsyntaxis van de.

    # sudo <COMMAND>

U kunt desgewenst een root-shell verkrijgen via sudo -s.

## <a name="next-steps"></a>Volgende stappen
- Ga naar [Azure marktplaats](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) voor het maken van een FreeBSD VM.
- Als u wilt uw eigen FreeBSD naar Azure brengen, Raadpleeg [maken en uploaden van een FreeBSD VHD naar Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).
