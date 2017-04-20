<properties
   pageTitle="Maken en uploaden van een afbeelding FreeBSD VM | Microsoft Azure"
   description="Meer informatie over maken en uploaden van een virtuele harde schijf (VHD) met het besturingssysteem FreeBSD een Azure virtuele machine maken"
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
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Maken en uploaden van een FreeBSD VHD naar Azure

In dit artikel wordt beschreven hoe u voor het maken en uploaden van een virtuele harde schijf (VHD) met het besturingssysteem FreeBSD. Nadat u deze hebt geüpload, kunt u het als uw eigen afbeelding te maken van een virtuele machine (VM) in Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u de volgende items:

- **Abonnement op een Azure**--als u geen account hebt, kunt u in een paar minuten. Als u een MSDN-abonnement hebt, raadpleegt u [krediet voor Visual Studio-abonnees maandelijks Azure.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Anders wordt informatie over het [maken van een gratis proefperiode account](https://azure.microsoft.com/pricing/free-trial/).  

- **Azure PowerShell extra**--de Azure PowerShell module moet worden geïnstalleerd en geconfigureerd voor het gebruik van uw abonnement op Azure. Downloaden van de module Zie [Azure downloads](https://azure.microsoft.com/downloads/). Een zelfstudie waarin wordt beschreven hoe installeer en configureer de module is hier beschikbaar. Gebruik de cmdlet [Azure Downloads](https://azure.microsoft.com/downloads/) voor het uploaden van de VHD.

- **FreeBSD besturingssysteem is geïnstalleerd in het VHD-bestand**--een ondersteund FreeBSD besturingssysteem moet worden geïnstalleerd op een virtuele harde schijf. Meerdere hulpprogramma's beschikbaar om de VHD-bestanden maken. U kunt bijvoorbeeld een virtualisatie-oplossing zoals de Hyper-V het VHD-bestand maken en installeren van het besturingssysteem. Zie voor instructies over het installeren en gebruiken van Hyper-V [Hyper-V installeren en het maken van een virtuele machine](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] De nieuwere VHDX-indeling wordt niet ondersteund in Azure. U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet [converteren vhd](https://technet.microsoft.com/library/hh848454.aspx). Bovendien is er een [zelfstudie over MSDN over het gebruik van FreeBSD met Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Deze taak omvat de volgende vijf stappen.

## <a name="step-1-prepare-the-image-for-upload"></a>Stap 1: Voorbereiden op de afbeelding uploaden

Op de virtuele machine waarop u het besturingssysteem FreeBSD geïnstalleerd, voert u de volgende procedures:

1. DHCP inschakelen.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. SSH activeren.

    SSH is standaard ingeschakeld na de installatie vanaf cd. Als het om een bepaalde reden niet is ingeschakeld of als u FreeBSD VHD rechtstreeks gebruiken, typt u het volgende:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Een seriële console instellen.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Sudo installeren.

    Het root-account is uitgeschakeld in Azure. Dit betekent dat u moet gebruiken sudo van een onbevoegde gebruiker opdrachten uitvoeren met verhoogde bevoegdheden.

        # pkg install sudo
;
5. Vereisten voor Azure Agent.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Azure-Agent te installeren.

    Altijd de meest recente versie van de Agent Azure vindt op [github](https://github.com/Azure/WALinuxAgent/releases). De 2.0.10 + officieel ondersteunt FreeBSD 10 & 10.1, en de versie 2.1.4 officieel ondersteunt FreeBSD 10.2 en latere versies.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    Voor 2.0, gebruikt u 2.0.16 laten we als voorbeeld:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Voor 2.1, 2.1.4 we als voorbeeld te gebruiken:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Nadat u de Azure Agent hebt geïnstalleerd, is het een goed idee om te controleren of deze wordt uitgevoerd:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Deprovision van het systeem.

    Deprovision het systeem schoon, waardoor het geschikt is voor het opnieuw toewijzen van bronnen. De volgende opdracht worden ook de laatste ingerichte gebruikersaccount en de bijbehorende gegevens verwijderd:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    U kunt nu uw VM afgesloten.

## <a name="step-2-create-a-storage-account-in-azure"></a>Stap 2: Maak een account voor opslag in Azure ##

U moet een account voor opslag in Azure VHD-bestand uploaden zodat deze kan worden gebruikt voor het maken van een virtuele machine. De klassieke Azure portal kunt u een account opslag maken.

1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com).

2. Selecteer **Nieuw**op de opdrachtbalk.

3. Selecteer **Data Services** > **opslag** > **snel tabellen maken**.

    ![Snel een opslag-account maken](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Vul de velden als volgt in:

    - Typ de subdomeinnaam van een te gebruiken in de URL van de opslag in het veld **URL** . De post kan van 3-24 cijfers en kleine letters bevatten. Deze naam wordt de hostnaam in de URL die wordt gebruikt om de Azure Blob-opslag of Azure Queue storage opslagbronnen Azure tabel voor het abonnement.

    - Kies de **locatie of de groep affiniteit** voor de opslag in de vervolgkeuzelijst **Locatie/affiniteit groep** . Een groep affiniteit plaatst u de cloud-services en opslag in het midden met dezelfde gegevens.

    - In het veld **replicatie** bepalen of **Geo-redundante** replicatie wordt gebruikt voor de opslag. Geo-replicatie is standaard ingeschakeld. Deze optie wordt uw gegevens gerepliceerd naar een secundaire locatie, zonder kosten, zodat de opslag wordt overgenomen door naar die locatie als er een storing optreedt op de primaire locatie. De tweede locatie wordt automatisch toegewezen en kan niet worden gewijzigd. Als u meer controle over de locatie van uw cloud-gebaseerde opslag vanwege wettelijke vereisten of de organisatie-beleid, kunt u geo-replicatie uitschakelen. Bedenk echter dat als u later op geo-replicatie, een vergoeding van de eenmalige gegevens overdracht brengt aan uw bestaande gegevens worden gerepliceerd naar de secundaire locatie. Zonder geo-replicatie Storage-services worden aangeboden met een korting. Meer informatie over het beheren van geo-replicatie van opslag accounts vindt u hier: [maken, beheren, of een opslag account verwijderen](../storage-create-storage-account/#replication-options).

    ![Voer de details van opslag](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Selecteer de **opslag-Account maken**. De account wordt nu weergegeven onder **opslag**.

    ![Opslag-account gemaakt](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Maak vervolgens een container voor de VHD geüploade bestanden. Selecteer de naam van de opslag, en selecteer vervolgens **Containers**.

    ![Opslag account details](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Selecteer **een Container**.

    ![Opslag account details](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. Typ in het veld **naam** een naam voor de container. Selecteer vervolgens welk type-beleid dat u wilt in de vervolgkeuzelijst **toegang** .

    ![Naam van container](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Standaard wordt de container is persoonlijk en alleen toegankelijk voor de eigenaar van de account. Als u openbare leestoegang tot de BLOB's in de container, maar niet de containereigenschappen en metagegevens, gebruikt u de optie **Openbare Blob** . Gebruik de optie **Openbare Container** zodat volledige openbare leestoegang voor de container en BLOB's.

## <a name="step-3-prepare-the-connection-to-azure"></a>Stap 3: Bereid de verbinding met Azure

Als u VHD-bestand wilt uploaden, moet u een beveiligde verbinding tussen uw computer en uw abonnement Azure. U kunt de methode Azure Active Directory (AD Azure) of het certificaat om dit te doen.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Gebruik de methode Azure AD VHD-bestand uploaden

1. Open de console Azure PowerShell.

2. Typ de volgende opdracht:  
    `Add-AzureAccount`

    Deze opdracht opent een-in-venster waarin u met uw werk of school-account ondertekenen kunt.

    ![Venster PowerShell](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure worden geverifieerd en de referentie-informatie wordt opgeslagen. Vervolgens wordt het venster gesloten.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Gebruik de methode certificaat VHD-bestand uploaden

1. Open de console Azure PowerShell.

2. Type:  `Get-AzurePublishSettingsFile`.

3. Een venster wordt geopend en krijgt u een .publishsettings-bestand te downloaden. Dit bestand bevat informatie en een certificaat voor uw abonnement op Azure.

    ![Pagina met downloadinformatie](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Sla het bestand .publishsettings.

4. Type:  `Import-AzurePublishSettingsFile <PathToFile>`, waarbij `<PathToFile>` is het volledige pad naar het bestand .publishsettings.

   Zie [aan de slag met Azure-cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx)voor meer informatie.

   Zie voor meer informatie over het installeren en configureren van PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Stap 4: Het VHD-bestand uploaden

Als u het VHD-bestand uploadt, kunt u deze overal plaatsen in de Blob-opslag. Hier volgen enkele termen die u gebruikt wanneer u het bestand uploaden:
-  **BlobStorageURL** is de URL voor de opslag-account die u in stap 2 hebt gemaakt.
-  **YourImagesFolder** is de container in de Blob-opslag waar u uw afbeeldingen op te slaan.
- **VHDName** is het label dat wordt weergegeven in de portal voor Azure klassieke ter identificatie van de virtuele harde schijf.
- **PathToVHDFile** is het volledige pad en de naam van het VHD-bestand.


Typ in het venster Azure PowerShell dat u in de vorige stap gebruikt:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Stap 5: Een VM met het geüploade VHD-bestand maken
Nadat u het VHD-bestand uploadt, kunt u het als een afbeelding toevoegen aan de lijst met aangepaste afbeeldingen die zijn gekoppeld aan uw abonnement en een virtuele machine maken met deze aangepaste afbeelding.

1. Typ in het venster Azure PowerShell dat u in de vorige stap gebruikt:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Het type besturingssysteem Linux gebruiken. De huidige versie van Azure PowerShell accepteert alleen "Linux" of "Windows" als een parameter.

2. Nadat u de voorgaande stappen hebt voltooid, wordt de nieuwe afbeelding weergegeven wanneer u het tabblad **afbeeldingen** op de klassieke Azure portal.  

    ![Een afbeelding kiezen](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Een virtuele machine maken in de galerie. Deze nieuwe afbeelding is nu beschikbaar onder **Mijn afbeeldingen**.
4. Selecteer de nieuwe afbeelding. Ga vervolgens de aanwijzingen voor het instellen van een host-naam, wachtwoord, SSH-sleutel, enzovoort.

    ![Aangepaste afbeelding](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Nadat u het aanbod hebt voltooid, ziet u uw FreeBSD VM in Azure uitgevoerd.

    ![Afbeelding van FreeBSD in azure](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
