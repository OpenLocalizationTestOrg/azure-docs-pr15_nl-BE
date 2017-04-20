<properties 
    pageTitle="SSH-sleutels met Windows gebruiken voor Linux VMs | Microsoft Azure" 
    description="Informatie over het genereren en het gebruik van SSH sleutels op een Windows-computer verbinding maken met een virtuele Linux machine op Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Hoe gebruik SSH sleutels met Windows op Azure

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux/Mac](virtual-machines-linux-mac-create-ssh-keys.md)

Wanneer u verbinding met Linux virtuele machines (VMs) in Azure maakt, kunt u [cryptografie met openbare sleutels](https://wikipedia.org/wiki/Public-key_cryptography) bieden een veilige manier aanmelden bij uw Linux VM. Dit proces omvat een openbare en persoonlijke sleutels uitwisselen zelf in plaats van een gebruikersnaam en wachtwoord te verifiëren met behulp van de shellopdracht secure (SSH). Wachtwoorden zijn kwetsbaar voor aanvallen, met name op Internet facing VMs zoals webservers brute kracht. Dit artikel bevat een overzicht van de SSH-sleutels en het genereren van de juiste toetsen op een Windows-computer.


## <a name="overview-of-ssh-and-keys"></a>Overzicht van toetsen en SSH

U kunt veilig aanmelden om uw Linux VM met behulp van openbare en persoonlijke sleutels:

- De **openbare sleutel** wordt geplaatst op uw VM Linux of een andere service die u wilt gebruiken met cryptografie met openbare sleutels.
- De **persoonlijke sleutel** is wat u presenteren aan uw Linux VM wanneer u zich aanmeldt, om uw identiteit te verifiëren. Deze persoonlijke sleutel te beschermen. Deze niet delen.

Deze openbare en persoonlijke sleutels kunnen op meerdere VMs en services worden gebruikt. U hoeft niet een paar sleutels voor elke VM of de service die u wilt openen. Zie voor een gedetailleerder overzicht [cryptografie met openbare sleutels](https://wikipedia.org/wiki/Public-key_cryptography).

SSH is protocol een versleutelde verbinding waarmee beveiligde aanmeldingen via niet-beveiligde verbindingen. Het is het standaardverbindingsprotocol voor Linux VMs in Azure gehost. Maar SSH zelf een gecodeerde verbinding biedt, wordt met behulp van wachtwoorden met SSH-verbindingen nog steeds de VM kwetsbaar voor brute-force aanvallen of het raden van wachtwoorden. Een methode veiliger, en de voorkeur, verbinding maken met een VM met behulp van SSH is met behulp van deze openbare en persoonlijke sleutels, ook bekend als SSH-sleutels.

Als u niet SSH sleutels te gebruiken wilt, kunt u nog steeds aanmelden bij uw Linux VMs met een wachtwoord. Als uw VM is niet toegankelijk via Internet, kan gebruik van wachtwoorden voldoende zijn. U moet echter nog steeds voor elke Linux VM wachtwoorden te beheren en onderhouden gezonde wachtwoordbeleid en praktijken, zoals minimale wachtwoordlengte en deze regelmatig bij te werken. Het gebruik van SSH sleutels vermindert de complexiteit van het beheer van afzonderlijke gegevens over meerdere VMs.


## <a name="windows-packages-and-ssh-clients"></a>Windows-pakketten en SSH-clients

U verbinding maken met en Linux VMs in Azure met behulp van een **ssh** -client beheren. Windows-computers hebben geen meestal een **ssh** -client is geïnstalleerd. Algemene Windows SSH clients die kunt u installeren zijn opgenomen in de volgende pakketten:

- [GIT voor Windows](https://git-for-windows.github.io/)
- [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)

> [AZURE.NOTE] De meest recente Update voor Windows 10 Verjaardag bevat Bash voor Windows. Met deze functie kunt u het Windows-subsysteem voor Linux- en hulpprogramma's zoals een SSH-client uitvoeren. Bash voor Windows nog in ontwikkeling is en wordt beschouwd als een bètaversie. Zie voor meer informatie over Bash voor Windows [Bash op Ubuntu op Windows](https://msdn.microsoft.com/commandline/wsl/about).


## <a name="which-key-files-do-you-need-to-create"></a>Welke bestanden moet u maken?

Azure is vereist ten minste 2048-bits indeling **ssh-rsa** openbare en persoonlijke sleutels. Als u Azure resources met behulp van het model Klassiek implementatie beheert, moet u ook voor het genereren van een PEM (`.pem` bestand).

Hier vindt u het implementatiescenario's en de typen bestanden die u in elk gebruikt:

1. **SSH-rsa** -sleutels zijn vereist voor een implementatie met behulp van de [portal Azure](https://portal.azure.com)en Resource Manager implementaties met behulp van de [CLI Azure](../xplat-cli-install.md).
    - Deze sleutels zijn meestal alle de meeste mensen hebben.
2. `.pem`bestand is vereist voor het maken van VMs met de [klassieke portal](https://manage.windowsazure.com). Deze sleutels worden ook ondersteund in de Classic-implementaties die gebruikmaken van de [CLI Azure](../xplat-cli-install.md).
    - U hoeft deze extra sleutels en certificaten maken als u de bronnen die zijn gemaakt met behulp van het model Klassiek implementatie beheert.


## <a name="install-git-for-windows"></a>Git installeren voor Windows

De voorgaande sectie die meerdere colli met de `openssl` tool voor Windows. Dit programma is nodig voor het maken van openbare en persoonlijke sleutels. De volgende voorbeelden beschreven installeren en gebruiken van **Git voor Windows**, maar u kunt het gewenste pakket. **GIT voor Windows** hebt u toegang tot sommige andere open source software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) hulpmiddelen en hulpprogramma's die nuttig zijn kunnen bij het werken met Linux VMs.

1. Downloaden en **Git voor Windows** installeren vanaf de volgende locatie: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. De standaardopties accepteren tijdens de installatie, tenzij u precies wilt wijzigen.

3. **Git Bash** uitvoeren vanuit het **Menu Start** > **Git** > **Git Bash**. De console lijkt op het volgende voorbeeld:

    ![GIT voor Windows Bash-shell](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Een persoonlijke sleutel maken

1. Gebruik in het venster van de **Git Bash** `openssl.exe` voor het maken van een persoonlijke sleutel. Het volgende voorbeeld wordt een sleutel met de naam `myPrivateKey` en certificaat met de naam `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    De uitvoer ziet er uit zoals in het volgende voorbeeld:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Beantwoord de vragen voor het de landnaam, locatie, de naam van organisatie, enz.

3. Uw nieuwe persoonlijke sleutel en het certificaat worden gemaakt in de huidige werkmap. Voor aanbevolen procedures voor beveiliging, moet u de machtigingen van uw persoonlijke sleutel instellen zodat alleen u toegang hebt:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Als u ook naar klassieke bronnen beheren, converteren de `myCert.pem` op `myCert.cer` (DER X509 gecodeerd certificaat). Deze optionele stap alleen uitvoeren als u moet met name oudere klassieke om bronnen te beheren. 

    Hiermee converteert u het certificaat met de volgende opdracht:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Een persoonlijke sleutel maken voor stopverf

PuTTY is een algemene SSH-client voor Windows. U bent een SSH-client die u wilt gebruiken. Stopverf, hebt u nodig voor het maken van een extra type sleutel - een stopverf persoonlijke sleutel (PPK). Als u niet gebruiken, stopverf wilt, kunt u deze sectie overslaan.

Het volgende voorbeeld wordt een extra persoonlijke sleutel voor PuTTY gebruiken:

1. Gebruik **Git Bash** uw persoonlijke sleutel omzetten in een persoonlijke sleutel van RSA die PuTTYgen kan begrijpen. Het volgende voorbeeld wordt een sleutel met de naam `myPrivateKey_rsa` van de bestaande sleutel met de naam `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Voor aanbevolen procedures voor beveiliging, moet u de machtigingen van uw persoonlijke sleutel instellen zodat alleen u toegang hebt:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Downloaden en uitvoeren van PuTTYgen vanaf de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Klik op het menu: **bestand** > **laden een persoonlijke sleutel**

4. Zoek uw persoonlijke sleutel (`myPrivateKey_rsa` in het vorige voorbeeld). De standaardmap wanneer u **Git Bash start** is `C:\Users\%username%`. Wijzigen van het bestand filteren op **alle bestanden (\*.\*)**:

    ![De bestaande persoonlijke sleutel in PuTTYgen laden](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Klik op **openen**. Een prompt geeft aan dat de sleutel is geïmporteerd:

    ![Sleutel geïmporteerd PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Klik op **OK** om de opdrachtprompt te sluiten.

7. De openbare sleutel wordt weergegeven boven aan het venster **PuTTYgen** . U kopieert en plakt van deze openbare sleutel naar de Azure portal of sjabloon Azure Resource Manager bij het maken van een Linux VM. U kunt ook klikken op de **openbare sleutel opslaan** een kopie wilt opslaan op uw computer:

    ![Stopverf bestand met openbare sleutel opslaan](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    In het volgende voorbeeld ziet u hoe u kopiëren en plakken van deze openbare sleutel in de Azure portal bij het maken van een Linux VM. De openbare sleutel wordt vervolgens meestal opgeslagen in `~/.ssh/authorized_keys` op uw nieuwe VM.

    ![Openbare sleutel wordt gebruikt bij het maken van een VM in Azure portal](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Klik in **PuTTYgen**, op **persoonlijke sleutel opslaan**:

    ![Stopverf persoonlijke sleutel opslaan](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Er wordt gevraagd of u doorgaan wilt zonder een wachtwoordzin in te voeren voor uw sleutel Een wachtwoordzin is vergelijkbaar met een wachtwoord dat is gekoppeld aan uw persoonlijke sleutel. Zelfs als iemand uw persoonlijke sleutel ophalen, zou ze nog steeds niet kunnen worden geverifieerd met behulp van alleen de sleutel. Ook moeten ze de wachtwoordzin. Zonder een wachtwoordzin, als iemand uw persoonlijke sleutel verkrijgt kunnen zij registreren een VM of service met die sleutel. U maakt een wachtwoordzin wordt aangeraden. Als u de wachtwoordzin vergeet, is er echter geen manier om te herstellen.

    Als u een wachtwoordzin opgeven wilt, klikt u op **Nee**, een wachtwoordzin opgeven in het hoofdvenster van PuTTYgen en klik vervolgens nogmaals op **persoonlijke sleutel opslaan** . Klik op **Ja** om door te gaan zonder dat de optionele wachtwoordzin.

8. Voer een naam en locatie voor het opslaan van uw bestand PPK.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Gebruik stopverf op SSH met een Linux-Machine

Nogmaals, stopverf is een algemene SSH-client voor Windows. U bent een SSH-client die u wilt gebruiken. De volgende stappen wordt beschreven hoe u de persoonlijke sleutel wordt gebruikt voor het verifiëren van uw Azure VM via SSH. De stappen zijn vergelijkbaar in andere SSH sleutel clients in uw persoonlijke sleutel te laden dat u nodig hebt om de SSH-verbinding te verifiëren.

1. Downloaden en uitvoeren van stopverf vanaf de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Vul de hostnaam of het IP-adres van uw VM vanaf de Azure portal:

    ![Nieuwe stopverf verbinding openen](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Voordat u **Open**selecteert, klikt u op **verbinding** > **SSH** > **Auth** -tabblad. Blader naar en selecteer de persoonlijke sleutel:

    ![Selecteer uw stopverf persoonlijke sleutel voor verificatie](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Klik op **Open** verbinding met de virtuele machine
 

## <a name="next-steps"></a>Volgende stappen
U kunt ook de openbare en persoonlijke sleutels [met behulp van OS X en Linux](virtual-machines-linux-mac-create-ssh-keys.md)genereren.

Zie voor meer informatie over Bash voor Windows en de voordelen van OSS hulpprogramma's binnen handbereik op uw Windows-computer, [Bash op Ubuntu op Windows](https://msdn.microsoft.com/commandline/wsl/about).

Als u problemen met SSH verbinding met uw Linux VMs ondervindt, Zie [problemen met SSH-verbindingen naar een Linux Azure VM](virtual-machines-linux-troubleshoot-ssh-connection.md).