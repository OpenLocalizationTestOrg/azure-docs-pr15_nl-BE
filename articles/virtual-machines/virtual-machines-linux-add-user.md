<properties
        pageTitle="Een gebruiker toevoegen aan een Linux VM in Azure | Microsoft Azure"
        description="Een gebruiker toevoegen aan een Linux VM in Azure."
        services="virtual-machines-linux"
        documentationCenter=""
        authors="vlivech"
        manager="timlt"
        editor=""
        tags="azure-resource-manager"
/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="08/18/2016"
        ms.author="v-livech"
/>

# <a name="add-a-user-to-an-azure-vm"></a>Een gebruiker toevoegen aan een Azure VM

Een van de eerste taken op elke nieuwe Linux VM is een nieuwe gebruiker te maken.  In dit artikel, we helpt bij het maken van een gebruikersaccount sudo instellen van het wachtwoord, SSH openbare sleutels toe te voegen, en ten slotte `visudo` sudo zonder wachtwoord toestaan.

Vereisten zijn: [een account Azure](https://azure.microsoft.com/pricing/free-trial/), [SSH openbare en persoonlijke sleutels](virtual-machines-linux-mac-create-ssh-keys.md), een resourcegroep Azure en de CLI Azure geïnstalleerd en overgeschakeld naar de modus met behulp van bronbeheer Azure `azure config mode arm`.

## <a name="quick-commands"></a>Snelle opdrachten

```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Gedetailleerde scenario

### <a name="introduction"></a>Inleiding

Een van de eerste en de meest voorkomende taak met een nieuwe server is een gebruikersaccount wilt toevoegen.  Root-aanmeldingen moeten worden uitgeschakeld en de root-account zelf mag niet worden gebruikt met de Linux-server alleen sudo.  Waardoor een gebruiker root doorverwijsbeheer bevoegdheden sudo met het de juiste manier voor het beheer en het gebruik van Linux.

Met de opdracht `useradd` we voor Linux VM gebruikersaccounts toevoegt.  Met `useradd` worden gewijzigd `/etc/passwd`, `/etc/shadow`, `/etc/group`, en `/etc/gshadow`.  We een opdrachtregelprogramma markering toevoegt de `useradd` opdracht ook de nieuwe gebruiker toevoegen aan de groep goede sudo op Linux.  Zelfs als u `useradd` maakt een item in `/etc/passwd` maar niet geeft de nieuwe gebruikersaccount een wachtwoord.  We maken een eerste wachtwoord voor de nieuwe gebruiker met behulp van eenvoudige `passwd` opdracht.  De laatste stap is het wijzigen van de regels met sudo zodat de gebruiker opdrachten met sudo bevoegdheden uitvoeren zonder dat u een wachtwoord opgeven voor elke opdracht.  Aanmelden met behulp van de persoonlijke sleutel wordt uitgegaan van een gebruikersaccount is beschermd tegen slecht actoren en gaan sudo toegang zonder wachtwoord.  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a>Een gebruiker één sudo toe te voegen aan een Azure VM

Log in op de Azure VM met behulp van SSH-sleutels.  Als u geen setup SSH openbare sleutel toegang hebt, volledige dit artikel eerste [openbare sleutel-verificatie gebruiken met Azure](http://link.to/article).  

De `useradd` opdracht voltooit u de volgende taken:

- een nieuwe gebruikersaccount maken
- Maak een nieuwe groep met dezelfde naam
- een lege vermelding toevoegen`/etc/passwd`
- een lege vermelding toevoegen`/etc/gpasswd`

De `-G` opdrachtregelprogramma vlag wordt de nieuwe gebruikersaccount toegevoegd aan de juiste Linux-groep waarin de nieuwe gebruikersaccount root doorverwijsbeheer bevoegdheden.

#### <a name="add-the-user"></a>De gebruiker toevoegen

```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Een wachtwoord instellen

De `useradd` opdracht maakt de gebruiker en wordt een vermelding toegevoegd aan de beide `/etc/passwd` en `/etc/gpasswd` , maar het wachtwoord niet daadwerkelijk instellen.  Het wachtwoord wordt toegevoegd aan de post met behulp van de `passwd` opdracht.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

We hebben nu een gebruiker met de bevoegdheden van de sudo op de server.

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a>De openbare SSH-sleutel toevoegen aan de nieuwe gebruikersaccount

Gebruik van uw computer, de `ssh-copy-id` opdracht met het nieuwe wachtwoord.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a>Met behulp van visudo sudo gebruik zonder wachtwoord toestaan

Met `visudo` voor het bewerken van de `/etc/sudoers` bestand enkele lagen van bescherming tegen het onjuist wijzigen van dit belangrijk bestand toegevoegd.  Bij het uitvoeren van `visudo`, de `/etc/sudoers` bestand is vergrendeld, zodat andere gebruikers geen wijzigingen kunt aanbrengen terwijl deze actief wordt bewerkt.  De `/etc/sudoers` bestand ook gecontroleerd op fouten door `visudo` wanneer u probeert op te slaan of te sluiten, zodat u een verbroken sudoers bestand niet opslaan.

Al hebben we gebruikers in de juiste groep voor sudo toegang.  Nu gaan we deze groepen te gebruiken sudo zonder wachtwoord inschakelen.

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a>Controleer of de gebruiker, ssh-sleutels en sudo

```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
