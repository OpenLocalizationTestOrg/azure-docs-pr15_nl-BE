<properties 
    pageTitle="Gebruikersnamen voor Linux selecteren | Microsoft Azure" 
    description="Informatie over gebruikersnamen voor een virtuele Linux machine in Azure selecteren." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



#<a name="selecting-user-names-for-linux-on-azure"></a>Gebruikersnamen voor Linux op Azure selecteren#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Wanneer u een virtuele Linux machine op Azure inrichten moet u de naam van een niet-root-gebruiker die u later gebruiken kunt om aan te melden bij de VM. U kunt de naam van de nieuwe gebruiker of als via de klassieke Azure-portal inrichten kunt u standaard de naam 'azureuser' accepteren.

In de meeste gevallen wordt deze gebruiker won't aanwezig zijn op de afbeelding en wordt gemaakt tijdens het inrichten. Als de gebruiker zich op de basisafbeelding VM en vervolgens de agent Azure Linux gewoon configureert u het wachtwoord en/of SSH-sleutel voor die gebruiker op basis van de informatie die u hebt opgegeven bij het maken van de VM.

**Echter**, Linux definieert een set namen van gebruikers die niet worden gebruikt. Tijdens het inrichtingsproces wordt **niet uitgevoerd** als u probeert een Linux VM met behulp van een bestaande systeemgebruiker, dat wordt gedefinieerd als een gebruiker met UID 0-99 inrichten. Een typisch voorbeeld is de `root` gebruiker met UID 0.

 - Zie ook: [bereik standaard Linux Base - gebruikers-ID](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Hier volgt een lijst met gebruikers van gemeenschappelijke ingebouwd systeem voor CentOS en dat moet u niet gebruiken tijdens het inrichten van een Linux virtuele machine op Azure Ubuntu. Deze lijst is slechts een voorbeeld, raadpleegt u de documentatie van uw distributie om ervoor te zorgen dat de gebruikersnaam die u ervoor kiezen geen conflict met een bestaande gebruiker veroorzaakt.


## <a name="centos"></a>CentOS
- abrt
- adm
- audio
- opslaglocatie
- cd-rom
- cgred
- daemon
- dbus
- bellen met
- DIP
- schijf
- diskettestation
- FTP
- FTP
- spellen
- Gopher
- haldaemon
- stoppen
- kmem
- vergrendelen
- LP
- e-mail
- Man
- Mem
- nfsnobody
- niemand
- NTP
- operator
- oprofile
- postdrop
- postfix
- qpidd
- hoofdmap
- RPC
- rpcuser
- saslauth
- afsluiten
- slocate
- sshd
- stapdev
- stapusr
- Sync
- sys
- tape
- Test
- tcpdump
- TTY
- gebruikers
- utempter
- utmp
- uucp
- vcsa
- video
- wiel


## <a name="ubuntu"></a>Ubuntu
- adm
- Admin
- audio
- back-up
- opslaglocatie
- cd-rom
- crontab
- daemon
- bellen met
- DIP
- schijf
- Fax
- diskettestation
- De zekering
- spellen
- gnats
- IRC
- kmem
- Liggend
- libuuid
- lijst
- LP
- e-mail
- Man
- MessageBus
- mlocate
- netdev
- nieuws
- niemand
- nogroup
- operator
- plugdev
- proxy
- hoofdmap
- SASL
- schaduw
- src
- SSH
- sshd
- personeel
- sudo
- Sync
- sys
- Syslog
- tape
- TTY
- gebruikers
- utmp
- uucp
- video
- stem
- whoopsie
- www-data

 