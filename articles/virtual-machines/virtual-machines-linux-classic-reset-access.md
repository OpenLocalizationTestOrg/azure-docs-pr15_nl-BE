<properties
        pageTitle="Opnieuw instellen wachtwoord Linux VM en SSH-sleutel van de CLI | Microsoft Azure"
        description="Hoe een Linux VM wachtwoord of SSH-sleutel opnieuw instellen en controleren van de consistentie van de schijf de SSH-configuratie herstellen met de extensie VMAccess van Azure opdrachtregelinterface (CLI)"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Het opnieuw instellen van een wachtwoord Linux VM of SSH-sleutel, de SSH-configuratie herstellen en de consistentie van de schijf met de extensie VMAccess controleren


Als u geen verbinding maken met een virtuele Linux machine op Azure vanwege een vergeten wachtwoord, een onjuiste code Secure Shell (SSH), of een probleem met de SSH-configuratie gebruiken de extensie VMAccessForLinux met de CLI Azure de SSH-sleutel of wachtwoord opnieuw instellen, los van de SSH-configuratie en controleren van de consistentie van de schijf. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Met de CLI Azure kunt u de opdracht **set extensie azure vm** vanaf de opdrachtregelinterface (Bash, Terminal, MS-DOS-prompt) opdrachten. **Help voor azure vm extensie instellen** voor gebruik met gedetailleerde extensie worden uitgevoerd.

Met de CLI Azure kunt u de volgende taken uitvoeren:

+ [Het wachtwoord opnieuw instellen](#pwresetcli)
+ [De SSH-sleutel opnieuw instellen](#sshkeyresetcli)
+ [Opnieuw instellen van het wachtwoord en de SSH-sleutel](#resetbothcli)
+ [Maak een nieuwe gebruikersaccount voor sudo](#createnewsudocli)
+ [De SSH-configuratie herstellen](#sshconfigresetcli)
+ [Een gebruiker verwijderen](#deletecli)
+ [De status van de extensie VMAccess](#statuscli)
+ [Consistentie controleren van schijven](#checkdisk)
+ [Herstellen van schijven op uw Linux VM](#repairdisk)


## <a name="prerequisites"></a>Vereisten

Moet u het volgende doen:

- U moet [de Azure CLI installeren](../xplat-cli-install.md) en [aansluiten op uw abonnement](../xplat-cli-connect.md) Azure resources die zijn gekoppeld aan uw account te gebruiken.
- Stel de juiste modus voor het implementatiemodel klassiek door de volgende opdracht bij de opdrachtprompt te typen:
        
        azure config mode asm
        
- Hebben een nieuw wachtwoord of SSH-sleutels, als u om een opnieuw wilt. U hoeft niet dit als u de SSH-configuratie opnieuw wilt instellen.


## <a name="pwresetcli"></a>Het wachtwoord opnieuw instellen

1. Maak een bestand met de naam PrivateConf.json met deze regels. De vierkante haken vervangen en de & #60; tijdelijke aanduiding & #62; waarden met uw eigen gegevens.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Deze opdracht, vervangen door de naam van de virtuele machine voor & #60; vm naam & #62; uitvoeren.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>De SSH-sleutel opnieuw instellen

1. Maak een bestand met de naam PrivateConf.json met deze inhoud. De vierkante haken vervangen en de & #60; tijdelijke aanduiding & #62; waarden met uw eigen gegevens.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Deze opdracht, vervangen door de naam van de virtuele machine voor & #60; vm naam & #62; uitvoeren.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Opnieuw instellen van het wachtwoord en de SSH-sleutel

1. Maak een bestand met de naam PrivateConf.json met deze inhoud. De vierkante haken vervangen en de & #60; tijdelijke aanduiding & #62; waarden met uw eigen gegevens.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Deze opdracht, vervangen door de naam van de virtuele machine voor & #60; vm naam & #62; uitvoeren.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Maak een nieuwe gebruikersaccount voor sudo

Als u uw gebruikersnaam bent vergeten, kunt u VMAccess naar een nieuw bestand maken met de sudo autoriteit. In dit geval wordt worden de bestaande gebruikersnaam en wachtwoord niet gewijzigd.

Om een nieuwe sudo-gebruiker het wachtwoord toegang, gebruik van het script in [het wachtwoord opnieuw instellen](#pwresetcli) en geef de nieuwe gebruikersnaam.

Een nieuwe sudo om gebruiker te maken met SSH-sleutel toegang, het script in [de SSH-sleutel opnieuw](#sshkeyresetcli) te gebruiken en geef de naam van de nieuwe gebruiker.

Kun je [het wachtwoord en de SSH-sleutel opnieuw](#resetbothcli) een nieuwe gebruiker te maken met een wachtwoord- en SSH-sleutel toegang.

## <a name="sshconfigresetcli"></a>De SSH-configuratie herstellen

Als de SSH-configuratie in een ongewenste staat, kan ook verliest u toegang tot de VM. De VMAccess-extensie kunt u de configuratie worden de standaardinstellingen hersteld. Hiervoor moet u alleen de "reset_ssh" sleutel instelt op 'True'. De uitbreiding zal de SSH-server opnieuw starten en opent u de SSH-poort op uw VM de SSH-configuratie terugzetten op standaardwaarden. De gebruikersaccount (naam, wachtwoord of SSH-sleutels) worden niet gewijzigd.

> [AZURE.NOTE] /Etc/ssh/sshd_config bevindt het configuratiebestand SSH die opnieuw wordt ingesteld.

1. Maak een bestand met de naam PrivateConf.json met deze inhoud.

        {
        "reset_ssh":"True"
        }

2. Deze opdracht, vervangen door de naam van de virtuele machine voor & #60; vm naam & #62; uitvoeren. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Een gebruiker verwijderen

Als u een gebruikersaccount zonder logboekregistratie in voor VM rechtstreeks te verwijderen wilt, kunt u dit script.

1. Maak een bestand met de naam PrivateConf.json met deze inhoud vervangen door de naam van de gebruiker te verwijderen voor & #60; usernametoremove & #62;. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Deze opdracht, vervangen door de naam van de virtuele machine voor & #60; vm naam & #62; uitvoeren. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>De status van de extensie VMAccess

Als u wilt weergeven van de status van de extensie VMAccess, moet u deze opdracht uitvoert.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< a name = 'checkdisk' <</a>consistentie controleren van schijven

Fsck uitvoeren op alle schijven in uw Linux virtuele machine, moet u het volgende doen:

1. Maak een bestand met de naam PublicConf.json met deze inhoud. Controleer de dat schijf wordt een Boole-waarde of controleren van schijven die zijn aangesloten op uw virtuele machine of niet. 

        {   
        "check_disk": "true"
        }

2. Uitvoeren van deze opdracht moet worden uitgevoerd, vervangen door de naam van de virtuele machine voor & #60; vm naam & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Herstellen van schijven 

Met de extensie VMAccess om te herstellen van schijven die niet wilt koppelen of fouten in de configuratie koppelen, opnieuw instellen de mount-configuratie op uw Linux virtuele machine. Vervangen door de naam van de schijf voor & #60; yourdisk & #62;.

1. Maak een bestand met de naam PublicConf.json met deze inhoud. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Uitvoeren van deze opdracht moet worden uitgevoerd, vervangen door de naam van de virtuele machine voor & #60; vm naam & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Volgende stappen

* Als u wilt de SSH-sleutel of wachtwoord opnieuw instellen met Azure PowerShell-cmdlets of Azure Resource Manager-sjablonen, de SSH-configuratie herstellen en schijf consistentie controleren, raadpleegt u de [documentatie van de VMAccess-extensie op GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* U kunt ook de [Azure portal](https://portal.azure.com) het wachtwoord opnieuw instellen of SSH-sleutel van een Linux VM geïmplementeerd in de klassieke implementatiemodel. U kunt de portal kan momenteel niet gebruiken deze voor een Linux VM in het implementatiemodel bronnenbeheerder geïmplementeerd.

* Zie [functies en uitbreidingen van de virtuele machine](virtual-machines-linux-extensions-features.md) voor meer informatie over het gebruik van VM-extensies voor Azure virtuele machines.
