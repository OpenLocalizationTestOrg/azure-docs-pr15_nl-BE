<properties
    pageTitle="SSH-sleutels maken op Linux en Mac | Microsoft Azure"
    description="Genereren en gebruik SSH sleutels op Linux en Mac voor de resourcemanager en klassieke implementatiemodellen op Azure."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>SSH-sleutels maken op Linux en Mac voor Linux VMs in Azure

U kunt virtuele Machines met een SSH-sleutelpaar maken op Azure die standaard SSH sleutels gebruiken voor verificatie, hoeft u de wachtwoorden aan te melden.  Wachtwoorden kunnen worden geraden en open uw VMs tot aanhoudend felle aanvallen te raden uw wachtwoord. VMs gemaakt met Azure sjablonen of de `azure-cli` kunt uw openbare SSH-sleutel opnemen als onderdeel van de implementatie, een implementatieconfiguratie boeken verwijderen.  Als u verbinding met een Linux VM vanuit Windows maakt, raadpleegt u [Dit document.](virtual-machines-linux-ssh-from-windows.md)

Het artikel moet:

- een Azure-account ([gratis proberen](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure CLI](../xplat-cli-install.md) bent aangemeld`azure login`

- de modus Azure CLI _moet in_ Azure Resource Manager`azure config mode arm`

## <a name="quick-commands"></a>Snelle opdrachten

In de volgende opdrachten kunt u de voorbeelden vervangen door uw eigen keuzes.

SSH sleutels worden standaard opgeslagen in de `.ssh` directory.  

```bash
cd ~/.ssh/
```

Als u niet beschikt over een `~/.ssh` directory de `ssh-keygen` opdracht maakt het voor u met de juiste machtigingen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Voer de naam van het bestand dat is opgeslagen in de `~/.ssh/` map:

```bash
id_rsa
```

Typ de wachtwoordzin voor id_rsa:

```bash
correct horse battery staple
```

Er is nu een `id_rsa` en `id_rsa.pub` SSH-sleutelpaar in de `~/.ssh` directory.

```bash
ls -al ~/.ssh
```

Toevoegen van de zojuist gemaakte sleutel naar `ssh-agent` op Linux en Mac (ook toegevoegd aan de sleutelketen OSX):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

De openbare SSH-sleutel met de Linux-Server kopiëren:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Test de aanmelding toetsen gebruiken in plaats van een wachtwoord:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Gedetailleerde scenario

Met behulp van de openbare en persoonlijke sleutels SSH is de gemakkelijkste manier aan te melden uw Linux-servers. [Cryptografie met openbare sleutels](https://en.wikipedia.org/wiki/Public-key_cryptography) kunt veel veiliger dan wachtwoorden die brute gedwongen uiterst eenvoudig aanmelden op uw Linux of BSD VM in Azure. Uw openbare sleutel kan worden gedeeld met iedereen; maar alleen als u (of uw lokale infrastructuur) beschikken over uw persoonlijke sleutel.  De persoonlijke sleutel SSH een [zeer veilig wachtwoord](https://www.xkcd.com/936/) moet hebben (bron:[xkcd.com](https://xkcd.com)) te beschermen.  Dit wachtwoord is alleen toegang tot de persoonlijke sleutel voor SSH en **niet** het wachtwoord van de gebruiker.  Wanneer u een wachtwoord aan de SSH-sleutel toevoegt, wordt de persoonlijke sleutel gecodeerd zodat de persoonlijke sleutel zinloos zonder het wachtwoord is te ontgrendelen.  Als een aanvaller uw persoonlijke sleutel stelen en deze sleutel niet over een wachtwoord, is ze zou kunnen die persoonlijke sleutel wordt gebruikt voor aanmelding bij een server met de bijbehorende openbare sleutel.  Als u een persoonlijke sleutel is beveiligd met wachtwoord kan niet worden gebruikt door deze aanvaller, bieden een extra beveiligingslaag voor uw infrastructuur op Azure.

Dit artikel maakt *ssh-rsa* opgemaakt belangrijke bestanden die worden aanbevolen voor implementaties van de Resource Manager.  *SSH-rsa* -sleutels zijn vereist op de [portal](https://portal.azure.com) voor klassieke- en resourcemanager implementaties.

## <a name="create-the-ssh-keys"></a>De SSH-sleutels maken

Azure is vereist ten minste 2048-bits indeling ssh-rsa openbare en persoonlijke sleutels. Het gebruik van sleutels maken `ssh-keygen`, waarin wordt gevraagd een reeks vragen en schrijft u een persoonlijke sleutel en de bijbehorende openbare sleutel. Wanneer een Azure VM wordt gemaakt, de openbare sleutel wordt gekopieerd naar `~/.ssh/authorized_keys`.  SSH-sleutels in `~/.ssh/authorized_keys` worden gebruikt om de uitdaging van de client overeenkomt met de bijbehorende persoonlijke sleutel op een verbinding SSH-aanmelding.

## <a name="using-ssh-keygen"></a>Met behulp van ssh-keygen

Deze opdracht maakt u een wachtwoord beveiligde (gecodeerde) SSH-sleutelpaar met RSA 2048-bits en deze gemakkelijk kan vinden is toegelicht.  

Begin met het wijzigen van mappen, zodat alle uw ssh sleutels worden gemaakt in die map.

```bash
cd ~/.ssh
```

Als u niet beschikt over een `~/.ssh` directory de `ssh-keygen` opdracht maakt het voor u met de juiste machtigingen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Opdracht uitgelegd_

`ssh-keygen`het programma gebruikt om de sleutels te maken =

`-t rsa`= type sleutel te maken die de [RSA-indeling](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bits van de sleutel

`-C "myusername@myserver"`= een opmerking toegevoegd aan het einde van het bestand met openbare sleutel gemakkelijk kan vinden.  Normaal gesproken een e-mailadres wordt gebruikt als de opmerking, maar kunt u wat het beste werkt voor uw infrastructuur.

### <a name="using-pem-keys"></a>Met behulp van PEM-sleutels

Als u het klassieke model implementeren (Azure klassieke Portal of de CLI Azure Service Management `asm`), moet u mogelijk gebruik van PEM SSH sleutels voor toegang tot uw Linux VMs opgemaakt.  Hier is het maken van een PEM-sleutel uit een bestaande SSH openbare sleutel en een bestaande x509 certificaat.

U maakt een PEM opgemaakt sleutel uit een bestaande openbare SSH-sleutel:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Voorbeeld van de ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Opgeslagen bestanden:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

De naam van het sleutelpaar voor dit artikel.  Met de combinatie van een sleutel met de naam **id_rsa** is de standaard en sommige hulpprogramma's de naam **id_rsa** persoonlijke-sleutelbestand zou verwachten dus met een een goed idee is. De map `~/.ssh/` is de standaardlocatie voor SSH sleutelparen en het SSH config-bestand.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Een overzicht van de `~/.ssh` directory. `ssh-keygen`Hiermee maakt u de `~/.ssh` directory als deze niet aanwezig is en ook de juiste modi van eigendom en de bestandsnaam ingesteld.

Wachtwoord:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`verwijst naar een wachtwoord als 'een wachtzin'.  Het wordt *ten zeerste* aanbevolen een wachtwoord toevoegen aan uw sleutelparen. Zonder een wachtwoord beschermen het sleutelpaar, kunt iedereen met een bestand met de persoonlijke sleutel aanmelden bij een server met de bijbehorende openbare sleutel. Toevoegen van een wachtwoord biedt meer bescherming in geval van iemand kunnen toegang krijgen tot uw persoonlijke-sleutelbestand krijgt u tijd om andere toetsen gebruikt om te verifiëren u.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Met behulp van ssh-agent voor het opslaan van uw wachtwoord voor persoonlijke sleutel

Als u wilt voorkomen dat uw wachtwoord voor persoonlijke-sleutelbestand met elke SSH-aanmelding te typen, kunt u `ssh-agent` voor het opslaan van het wachtwoord voor persoonlijke-sleutelbestand. Als u een Mac gebruikt, de sleutelketen OSX de persoonlijke sleutels, wachtwoorden veilig opgeslagen wanneer u `ssh-agent`.

Controleer eerst `ssh-agent` wordt uitgevoerd

```bash
eval "$(ssh-agent -s)"
```

De persoonlijke sleutel tot nu toe te voegen `ssh-agent` met de opdracht `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Het wachtwoord voor persoonlijke sleutel wordt nu opgeslagen in `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Maken en configureren van een SSH config-bestand

Aanbevolen best practice maken en configureren is een `~/.ssh/config` te versnellen bestand inloggen en voor het optimaliseren van het gedrag van de SSH-client.

In het volgende voorbeeld ziet u een standaardconfiguratie.

### <a name="create-the-file"></a>Het bestand maken

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Bewerk het bestand als u wilt toevoegen van de nieuwe SSH-configuratie:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Voorbeeld van de `~/.ssh/config` bestand:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Deze SSH config kunt u secties voor elke server die elk een eigen specifieke sleutelpaar hebt. De standaardinstellingen (`Host *`) zijn voor hosts die niet overeenkomen met een van de specifieke hosts hogerop in het config-bestand.


### <a name="config-file-explained"></a>Configuratiebestand uitgelegd

`Host`= de naam van de host op de terminal wordt aangeroepen.  `ssh fedora22`vertelt `SSH` met de waarden in het blok instellingen gebruikt het label `Host fedora22` Opmerking: dit is een label dat is logisch voor uw gebruik en niet de werkelijke hostnaam van elke server bevatten.

`Hostname 102.160.203.241`= IP-adres of DNS-naam voor de server die wordt geopend.

`User myusername`= de externe gebruikersaccount die u wilt gebruiken bij het aanmelden bij de server.

`PubKeyAuthentication yes`= SSH vertelt u wilt gebruiken een SSH-sleutel aan te melden.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= de SSH persoonlijke sleutel en de bijbehorende openbare sleutel voor verificatie.


## <a name="ssh-into-linux-without-a-password"></a>SSH in Linux zonder wachtwoord

Nu u een SSH-sleutelpaar en een geconfigureerde SSH config-bestand hebt, bent u kunt zich aanmelden bij uw Linux VM snel en veilig. De eerste keer dat u zich aanmeldt op een server met een SSH-sleutel de opdrachtprompts u voor de wachtwoordzin voor dat bestand.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Opdracht uitgelegd

Wanneer `ssh fedora22` wordt uitgevoerd SSH eerst zoekt en laadt de instellingen van de `Host fedora22` blokkeren en de overige instellingen van het laatste blok geladen `Host *`.

## <a name="next-steps"></a>Volgende stappen

Volgende omhoog is Azure Linux VMs met de nieuwe openbare SSH-sleutel te maken.  Azure VMs die zijn gemaakt met een openbare SSH-sleutel als de aanmelding zijn beter beveiligd dan VMs gemaakt met de standaard login methode wachtwoorden.  Azure VMs gemaakt met behulp van SSH-sleutels zijn standaard geconfigureerd met wachtwoorden is uitgeschakeld, raden pogingen brute gedwongen te vermijden.

- [Een veilige Linux VM met behulp van een Azure-sjabloon maken](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Een veilige Linux VM met de Azure portal maken](virtual-machines-linux-quick-create-portal.md)
- [Een veilige Linux VM met behulp van de CLI Azure maken](virtual-machines-linux-quick-create-cli.md)
