<properties
    pageTitle="Update de Azure Linux Agent van GitHub | Microsoft Azure"
    description="Meer informatie over hoe u de update Azure Linux Agent voor uw Linux VM in Azure naar de versie lateset van Github"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>De Agent Azure Linux op een VM bijwerken naar de nieuwste versie van GitHub

Als u wilt uw [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) op een Linux VM in Azure bijwerken, moet u al:

1. Een actieve Linux VM in Azure.
2. Een verbinding met dat Linux VM via SSH.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Als u deze taak worden uitgevoerd vanaf een computer met Windows, kunt u stopverf aan SSH in uw Linux-machine. Zie voor meer informatie [hoe u zich aanmeldt bij een virtuele Machine wordt uitgevoerd op Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Azure geviseerd Linux distros het pakket Azure Linux Agent hebt ingevoegd in de opslagplaatsen, dus controleer en installeer de nieuwste versie van die Distro opslagplaats eerst indien mogelijk.  

Voor Ubuntu, typ:

    #sudo apt-get install walinuxagent

En op CentOS, typ:

    #sudo yum install waagent


Voor Linux, Oracle, zorg ervoor dat de `Addons` bibliotheek is ingeschakeld. Kiest u voor het bewerken van het bestand `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), en wijzig de regel `enabled=0` te `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in dit bestand.

De meest recente versie van de Agent Azure Linux installeren, typ:


    #sudo yum install WALinuxAgent

Als u de invoegtoepassing bibliotheek niet kunt vinden kunt u gewoon deze regels toevoegen aan het einde van het bestand .repo volgens uw Oracle Linux-versie:

Voor Oracle Linux 6 virtuele machines:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Voor Oracle Linux 7 virtuele machines:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Typ vervolgens:

    #sudo yum update WALinuxAgent

Normaal is dit alles u nodig hebt, maar als om een bepaalde reden moet u direct installeren vanaf https://github.com gebruikt u de volgende stappen uit.


## <a name="install-wget"></a>Wget installeren

Aanmelden bij uw VM met behulp van SSH.

Wget (Er zijn enkele distros die niet door standaard zoals Redhat, CentOS en Oracle Linux versie 6.4 en 6.5 installeren) installeren door te typen `#sudo yum install wget` op de opdrachtregel.


## <a name="download-the-latest-version"></a>Download de nieuwste versie

[De release van Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) te openen in een webpagina en lees het meest recente versienummer. (U kunt uw huidige versie vinden door te typen `#waagent --version`.)

### <a name="for-version-20x-type"></a>Voor versie 2.0.x, type:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Versie 2.0.14 wordt als voorbeeld gebruikt de volgende regel:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Voor versie 2.1.x of hoger, typ:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Versie 2.1.0 wordt als voorbeeld gebruikt de volgende regel:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>De Azure Linux Agent installeren

### <a name="for-version-20x-use"></a>Voor versie 2.0.x, gebruik:

 Waagent uitvoerbaar maken:

    #chmod +x waagent

 Nieuwe kunt u het uitvoerbare bestand naar /usr/sbin/kopiëren.

  Voor de meeste Linux gebruiken:

    #sudo cp waagent /usr/sbin

  CoreOS, gebruik:

    #sudo cp waagent /usr/share/oem/bin/

  Als dit een nieuwe installatie van de Linux Azure Agent, worden uitgevoerd:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Voor versie 2.1.x, gebruik:

Mogelijk moet u het pakket installeren `setuptools` eerst--Zie [hier](https://pypi.python.org/pypi/setuptools). Vervolgens worden uitgevoerd:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

Voor de meeste linux-Distros:

    #sudo service waagent restart

Voor Ubuntu, gebruikt u:

    #sudo service walinuxagent restart

CoreOS, gebruik:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Controleer de versie van Linux-Agent Azure

    #waagent -version

Voor CoreOS werkt de bovenstaande opdracht niet.

U ziet dat de Agent voor Azure Linux-versie is bijgewerkt naar de nieuwe versie.

Zie voor meer informatie over de Agent Azure Linux [Azure Linux Agent Leesmij-bestand](https://github.com/Azure/WALinuxAgent).
