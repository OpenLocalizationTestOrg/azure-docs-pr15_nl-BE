<properties
    pageTitle="SSH op uw Linux VM wachtwoorden uitschakelen door SSHD configureren | Microsoft Azure"
    description="Beveiligt uw Linux VM in Azure aanmeldingen wachtwoord uitschakelen voor SSH."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>SSH op uw Linux VM wachtwoorden uitschakelen door SSHD configureren

In dit artikel wordt behandeld hoe de beveiliging van de aanmelding van uw Linux VM vergrendelen.  Zodra de SSH-poort 22 wordt geopend op de wereld bots begin probeert aan te melden door het raden van wachtwoorden.  Wat we in dit artikel doet is het wachtwoord aanmeldingen via SSH uitschakelen.  Door het volledig verwijderen van de mogelijkheid om wachtwoorden te gebruiken beschermen we Linux VM tegen dit soort aanvallen met brute kracht.  Extra bonus is dat wij zal Linux SSHD aanmeldingen via SSH openbare en persoonlijke sleutels alleen dat veruit de veiligste manier om in te loggen om Linux te configureren.  De mogelijke combinaties van deze vereist te raden de persoonlijke sleutel is een zeer grote en daarom ontmoedigt bots uit zelfs proberen alles uit te proberen te brute kracht SSH sleutels.


## <a name="goals"></a>Doelstellingen

- Configureer de SSHD te verbieden:
  - Wachtwoord-aanmeldingen
  - Aanmelding van de gebruiker Root
  - Vraag/antwoord-verificatie
- Configureren van SSHD om toe te staan:
  - alleen SSH key-aanmeldingen
- Start SSHD terwijl u nog bent aangemeld
- Test de nieuwe SSHD configuratie

## <a name="introduction"></a>Inleiding

[SSH gedefinieerd](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD is de SSH-Server die wordt uitgevoerd op de Linux VM.  SSH is een client die wordt uitgevoerd vanuit een shell op uw MacBook of Linux-werkstation.  SSH is ook het protocol dat wordt gebruikt voor het beveiligen en coderen van de communicatie tussen uw werkstation en de Linux VM.

Voor dit artikel is het belangrijk dat een aanmelding voor uw Linux VM open voor hele doorlopen.  Om deze reden zal openen we twee terminals en SSH voor Linux VM van beide.  Breng de wijzigingen aan het configuratiebestand SSHDs en opnieuw opstarten van de SSHD service gebruiken we de eerste terminal.  De tweede terminal gebruiken we om te testen die wijzigingen zodra de service opnieuw is gestart.  Omdat we SSH wachtwoorden uitschakelen zijn en vertrouwen strikt op SSH-sleutels, als uw SSH-sleutels niet correct zijn en u de verbinding met de VM sluit, de VM permanent worden, vergrendeld en kan niemand zich kunnen aanmelden bij deze die worden verwijderd en opnieuw gemaakt.

## <a name="prerequisites"></a>Vereisten

- [SSH-sleutels maken op Linux en Mac voor Linux VMs in Azure](virtual-machines-linux-mac-create-ssh-keys.md)
- Azure-account
  - [gratis proefversie aanmelden](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure portal](http://portal.azure.com)
- Linux VM op azure
- SSH openbare en persoonlijke sleutelpaar in`~/.ssh/`
- Openbare SSH-sleutel in `~/.ssh/authorized_keys` op de Linux VM
- Sudo-rechten op de VM
- Poort open 22

## <a name="quick-commands"></a>Snelle opdrachten

_Doorgewinterde Linux Admins die de TLDR versie Hier starten.  Voor alle andere wil wordt deze sectie overslaan door de gedetailleerde uitleg en doorlopen._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Gedetailleerde doorlopen

Aanmelding voor Linux VM op terminal 1 (T1).  Aanmelding voor Linux VM op terminal 2 (T2).

We gaan de SSHD configuratiebestand bewerken op T2.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Hier zullen we alleen de opgegeven instellingen SSH-sleutel aanmeldingen en wachtwoorden uitschakelen als u wilt bewerken.  Er zijn veel instellingen in dit bestand die u moet onderzoeken en wijzigen om Linux & SSH zo veilig als u nodig hebt.

#### <a name="disable-password-logins"></a>Aanmeldingen wachtwoord uitschakelen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Verificatie met openbare sleutel inschakelen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Basis-aanmelding uitschakelen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Vraag/antwoord-verificatie uitschakelen

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>SSHD opnieuw starten

Controleer of u nog steeds bent aangemeld vanuit de shell T1.  Dit is essentieel, zodat u niet van de VM afgesloten doen als SSH sleutels niet correct omdat wachtwoorden zijn nu uitgeschakeld.  Als alle instellingen op uw Linux VM kunt u T1 sshd_config oplossen kloppen als u nog steeds geregistreerd in en SSH de verbinding actief tijdens de SSHD-service blijft opnieuw starten.

Van T2 uitvoeren:

##### <a name="on-the-debian-family"></a>Op de Debian-familie

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Op de RedHat-familie

```
username@macbook$ sudo service sshd restart
```

Wachtwoorden zijn nu uitgeschakeld op uw VM beschermt tegen brute kracht wachtwoord inlogpogingen.  Met alleen SSH-sleutels toegestaan worden sneller aanmelden en veel veiliger.
