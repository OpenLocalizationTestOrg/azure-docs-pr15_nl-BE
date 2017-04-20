<properties
    pageTitle="Linux gasten op Azure Stack | Microsoft Azure"
    description="Meer informatie over hoe Linux-gebaseerde virtuele machines op Azure stapel te maken."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Linux virtuele machines op Azure Stack implementeren

Door een afbeelding op basis van Linux toe te voegen in de Stack Azure Marketplace kunt u Linux virtuele machines op de Stack Azure Haalbaarheidstest implementeren. Verschillende leveranciers van Linux afbeeldingen die kunnen worden toegevoegd in een stapel Azure Implementatiemodel hebt opgegeven of u kunt bouwen uw eigen.

## <a name="download-an-image"></a>Een afbeelding downloaden

 1. Bereid uw eigen downloaden en een Azure Stack-compatibele afbeelding ophalen uit de volgende koppelingen:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [16.04 Ubuntu LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Pak de afbeelding VHD indien nodig en het [toevoegen van de afbeelding op de markt](azure-stack-add-vm-image.md). Zorg ervoor dat de `OSType` parameter is ingesteld op `Linux`.
 
 3. Nadat u de afbeelding hebt toegevoegd aan de markt, een markt-item is gemaakt en u een virtuele Linux machine kunt implementeren.
  
## <a name="prepare-your-own-image"></a>Bereid uw eigen afbeelding

1. Bereid uw eigen Linux-afbeelding met een van de volgende instructies:
 - [Op basis van centOS verdelingen](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Red Hat Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Download en installeer de [Azure Linux Agent](https://github.com/Azure/WALinuxAgent/)

    De Agent in Azure Linux versie 2.1.3 of hoger is vereist voor het inrichten van uw Linux VM in Azure Stack. Veel van de hierboven reeds genoemde verdelingen deze versie van de agent of hoger in een pakket opnemen in hun opslagplaatsen (gewoonlijk genoemd `WALinuxAgent` of `walinuxagent`). Echter, als de versie van het pakket Azure agent minder dan 2.1.3 is (2.0.18 of lager), en vervolgens moet u de agent handmatig installeren. De geïnstalleerde versie kan worden bepaald uit de pakketnaam of door `/usr/sbin/waagent -version` op de VM.

    Volg de onderstaande instructies handmatig te installeren de Azure Linux agent-

 - Eerst de meest recente Linux Azure-agent downloaden van [Github](https://github.com/Azure/WALinuxAgent/releases), voorbeeld:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - De agent Azure uitpakken:

            # tar -vzxf v2.2.0.tar.gz

 - Installeer python-setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - De Azure agent installeren:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Systemen met Python 2.x en 3.x geïnstalleerd naast elkaar Python moet mogelijk de volgende opdracht:

        # sudo python3 setup.py install --register-service

    Zie de Azure Linux Agent [Leesmij-bestand](https://github.com/Azure/WALinuxAgent/blob/master/README.md)voor meer informatie.

3. [Toevoegen van de afbeelding op de markt](azure-stack-add-vm-image.md). Zorg ervoor dat de `OSType` parameter is ingesteld op `Linux`.

4. Nadat u de afbeelding hebt toegevoegd aan de markt, een markt-item is gemaakt en u een virtuele Linux machine kunt implementeren.

## <a name="next-steps"></a>Volgende stappen

[Veelgestelde vragen over Stack Azure](azure-stack-faq.md)