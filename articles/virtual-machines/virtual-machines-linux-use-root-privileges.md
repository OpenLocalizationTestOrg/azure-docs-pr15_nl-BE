<properties 
    pageTitle="Root-rechten gebruiken op Linux virtuele machines | Microsoft Azure" 
    description="Leren werken met root rechten op een virtuele Linux machine in Azure." 
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


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Met behulp van root-rechten op Linux virtuele machines in Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Standaard, de `root` gebruiker op Linux virtuele machines in Azure is uitgeschakeld. Gebruikers kunnen opdrachten uitvoeren met verhoogde bevoegdheden met behulp van de `sudo` opdracht. De ervaring kan echter variëren afhankelijk van hoe het systeem is ingericht.

1. **SSH-sleutel en wachtwoord of alleen** - de virtuele machine is ingericht met hetzij een certificaat (`.CER` bestand) of SSH-sleutel en een wachtwoord, of alleen een gebruikersnaam en wachtwoord. In dit geval `sudo` voor het wachtwoord van de gebruiker wordt gevraagd voordat de opdracht wordt uitgevoerd.

2. **Alleen SSH-sleutel** - de virtuele machine is voorzien van een certificaat (`.cer`, `.pem`, of `.pub` bestand) of SSH-sleutel, maar geen wachtwoord.  In dit geval `sudo` **niet** vragen om het wachtwoord van de gebruiker voordat de opdracht wordt uitgevoerd.


## <a name="ssh-key-and-password-or-password-only"></a>SSH-sleutel en wachtwoord of wachtwoord alleen

Log in op de Linux virtuele machine met behulp van SSH-sleutel of wachtwoord verificatie en voer vervolgens met behulp van opdrachten `sudo`, bijvoorbeeld:

    # sudo <command>
    [sudo] password for azureuser:

In dit geval wordt de gebruiker gevraagd om een wachtwoord. Na het invoeren van het wachtwoord `sudo` wordt de opdracht uitgevoerd `root` bevoegdheden.

U kunt ook een passwordless sudo inschakelen door het bewerken van het `/etc/sudoers.d/waagent` het bestand, bijvoorbeeld:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Deze wijziging kan voor passwordless sudo door de gebruiker "azureuser".

## <a name="ssh-key-only"></a>SSH sleutel alleen

Log in op de Linux virtuele machine met behulp van SSH key-verificatie en voer vervolgens met behulp van opdrachten `sudo`, bijvoorbeeld:

    # sudo <command>

In dit geval de gebruiker wordt **niet** gevraagd een wachtwoord op. Na het drukken op `<enter>`, `sudo` , wordt de opdracht met uitgevoerd `root` bevoegdheden.

 
