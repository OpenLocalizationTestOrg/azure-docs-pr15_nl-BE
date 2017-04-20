<properties
   pageTitle="StorSimple virtuele matrix 1 - Portal voorbereiding implementeren"
   description="Eerste zelfstudie implementeren StorSimple virtuele matrix bestaat uit de portal wordt voorbereid"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>StorSimple virtuele matrix implementeren - voorbereiden van de portal

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Overzicht

Dit artikel is van toepassing op Microsoft Azure StorSimple virtuele matrix (ook bekend als de StorSimple op ruimten virtuele apparaatstuurprogramma of virtueel apparaat StorSimple) uitgevoerd maart 2016 algemene beschikbaarheid (GA) release. Dit is het eerste artikel in de reeks van distributie, zelfstudies volledig uw virtuele matrix implementeren als een bestandsserver of een iSCSI-server vereist. Dit artikel beschrijft de bereiding vereist voor het maken en configureren van uw StorSimple Manager-service voor het inrichten van een virtuele matrix. In dit artikel ook koppelingen die naar een implementatie, controlelijst als configuratie vereisten.

U moet administrator-bevoegdheden voor de installatie en configuratie voltooid. Het is raadzaam om voordat u begint met de implementatie, Configuratiecontrolelijst te controleren. De portal voorbereiding duurt minder dan 10 minuten.

De informatie in dit artikel is van toepassing op de implementatie van StorSimple virtuele matrices in Azure klassieke portal zoals Microsoft Azure overheid wolk.

### <a name="get-started"></a>Aan de slag

De werkstroom implementatie bestaat uit het voorbereiden van de portal, een virtuele matrix in uw gevirtualiseerde omgeving inrichten en de installatie is voltooid. Om te beginnen met de implementatie StorSimple virtuele matrix als een bestandsserver of een iSCSI-server, moet u verwijzen naar de volgende bronnen gebruikmaking (artikelen en video's).

#### <a name="deployment-articles"></a>Implementatie van artikelen

Raadpleeg de volgende artikelen in de voorgeschreven volgorde voor de implementatie van uw virtuele matrix StorSimple.

| **#** | **In deze stap**                          | **Doet u dit...**                                                         | **Deze documenten gebruiken.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **De klassieke Azure portal instellen**       | Maak en configureer uw StorSimple Manager-service voor het inrichten van een virtueel apparaat StorSimple.  |[Voorbereiden van de portal](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **Bepaling van de virtuele matrix**           | Voor Hyper-V, richt en maak verbinding met een virtueel apparaat StorSimple op een hostsysteem waarop Hyper-V wordt uitgevoerd op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2. <br></br> <br></br> Voor VMware, inrichten en verbinding maken met een StorSimple op ruimten virtueel apparaat op een hostsysteem met VMware ESXi 5.5 en hoger.<br></br>| [Inrichten van een virtuele matrix in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Inrichten van een virtuele matrix in VMware](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **De virtuele matrix instellen**              | Eerste setup uitvoeren voor de bestandsserver registreert uw bestandsserver StorSimple en de instellingen van het apparaat. U kunt SMB-shares richt. <br></br> <br></br> Eerste setup uitvoeren voor uw iSCSI-server uw StorSimple iSCSI-server registreren en de instellingen van het apparaat. U kunt vervolgens de iSCSI-volumes inrichten.| [Virtuele matrix ingesteld als bestandsserver](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[Virtuele matrix als iSCSI-server instellen](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Video's voor implementatie

| **Voer deze stap...** |  **Bekijk deze video.**|
|----------------|-------------|
| Stapsgewijze instructies om aan de slag met de StorSimple virtuele matrix. | [Aan de slag met de StorSimple virtuele matrix](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Stapsgewijze instructies voor het inrichten van een virtuele matrix StorSimple in Hyper-V.|[Maak een virtuele matrix StorSimple](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Stapsgewijze instructies voor het configureren en een virtuele matrix StorSimple registreren|[Een virtuele matrix StorSimple configureren](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Stapsgewijze instructies voor het maken van aandelen, back-up van aandelen, en gegevens terugzetten op een virtuele matrix van StorSimple geconfigureerd als bestandsserver|[De virtuele matrix StorSimple gebruiken](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Stapsgewijze instructies voor het failover- en disaster recovery van een virtuele matrix StorSimple|[StorSimple virtuele matrix Disaster Recovery](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

Nu kunt u beginnen met het instellen van de klassieke Azure portal.

## <a name="configuration-checklist"></a>Configuratiecontrolelijst voor

De configuratiechecklist beschrijft de informatie die u verzamelen moet voordat u de software op uw apparaat StorSimple configureren. Deze informatie tijdig voorbereiden helpen stroomlijnen van het apparaat StorSimple in uw omgeving implementeren. Afhankelijk van of het virtuele apparaat StorSimple wordt geïmplementeerd als een bestandsserver of een iSCSI-server, moet u een van de volgende controlelijsten.

-   De [StorSimple virtuele matrix bestand Server configuratiechecklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf)downloaden.

-   De [iSCSI Virtual Array StorSimple configuratiechecklist Server](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf)downloaden.

## <a name="prerequisites"></a>Vereisten

Hier vindt u de vereisten van de configuratie voor uw StorSimple Manager-service en de StorSimple virtueel apparaat het datacenter-netwerk.

### <a name="for-the-storsimple-manager-service"></a>Voor de StorSimple Manager-service

Voordat u begint, zorg ervoor dat:

-   U hebt uw Microsoft-account met toegangsreferenties.

-   U hebt uw account Microsoft Azure opslag met toegangsreferenties.

-   Uw abonnement op Microsoft Azure moet voor StorSimple Manager-service worden ingeschakeld.

### <a name="for-the-storsimple-virtual-device"></a>Voor het virtuele apparaat StorSimple

Voordat u een virtueel apparaat implementeert, zorg ervoor dat:

-   U toegang hebt tot een hostsysteem met Hyper-V op Windows Server 2008 R2 of later of VMware (ESXi 5.5 of hoger) die kan worden gebruikt om een bepaling van een apparaat.

-   Het hostsysteem kan besteden aan het inrichten van het virtuele apparaat de volgende bronnen:

    -   Minimaal 4 cores.

    -   Minimaal 8 GB aan RAM-geheugen.

    -   Een netwerk-interface.

    -   Een 500 GB virtuele schijf systeemgegevens.

### <a name="for-the-datacenter-network"></a>Voor het datacenter-netwerk

Voordat u begint, zorg ervoor dat:

-   Het netwerk in uw datacenter is geconfigureerd aan de hand van de netwerkvereisten voor het apparaat StorSimple. Zie de [StorSimple virtuele matrix systeemvereisten](storsimple-ova-system-requirements.md)voor meer informatie.

-   Het virtuele apparaat StorSimple heeft een speciale 5 Mbps Internet bandbreedte (of meer) beschikbaar te allen tijde. Deze bandbreedte mag niet worden gedeeld met andere toepassingen.

## <a name="step-by-step-preparation"></a>Stapsgewijze voorbereiding

Stapsgewijze instructies voor het voorbereiden van uw portal voor de StorSimple Manager-service gebruiken.

## <a name="step-1-create-a-new-service"></a>Stap 1: Een nieuwe service maken

Slechts één exemplaar van de StorSimple Manager-service kan meerdere StorSimple 1200 apparaten beheren. Voer de volgende stappen uit om een nieuw exemplaar van de service Manager StorSimple maken. Als u een bestaande StorSimple Manager-service voor het beheren van uw apparaten 1200, sla deze stap over en gaat u naar [stap 2: de registratie sleutel ophalen](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Als u het automatisch maken van een account voor de opslag niet met uw service heeft ingeschakeld, moet u ten minste één opslag-account maken nadat u een service hebt gemaakt.
>

> - Als u een opslag-account niet automatisch gemaakt hebt, gaat u naar [een nieuwe opslag-account voor de service configureren](#optional-step-configure-a-new-storage-account-for-the-service) voor gedetailleerde instructies.
>

> - Als u het automatisch maken van een account voor opslag hebt ingeschakeld, gaat u naar [stap 2: de registratie sleutel ophalen](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Stap 2: De registratie sleutel ophalen


Nadat de StorSimple Manager-service actief is, moet u de registratie sleutel ophalen. Deze sleutel wordt gebruikt om te registreren en de StorSimple apparaat verbinding met de service.

De volgende stappen uitvoeren in de [Azure klassieke portal](https://manage.windowsazure.com/).


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> De sleutel voor de inschrijving wordt gebruikt voor het registreren van alle StorSimple Manager-apparaten die moeten worden geregistreerd in de StorSimple Manager-service.

## <a name="step-3-download-the-virtual-device-image"></a>Stap 3: Download het image virtueel apparaat

Nadat u de sleutel voor de inschrijving hebt, moet u de juiste virtuele apparaatstuurprogramma afbeelding om het creëren van een virtueel apparaat op uw hostsysteem te downloaden. Het virtuele apparaatafbeeldingen zijn specifieke besturingssysteem en kunnen worden gedownload van de pagina Quick Start in de klassieke Azure portal.

> [AZURE.IMPORTANT] De software die wordt uitgevoerd op de virtuele matrix StorSimple kan alleen worden gebruikt in combinatie met de Storsimple Manager-service.


De volgende stappen uitvoeren in de [Azure klassieke portal](https://manage.windowsazure.com/).

#### <a name="to-get-the-virtual-device-image"></a>Het virtuele apparaat afbeelding ophalen

1.  Klik op de pagina **StorSimple Manager-service** op de service die u hebt gemaakt. Hiervoor gaat u naar de pagina **Quick Start** . (U kunt klikken op het pictogram van de werkbalk Snel starten ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) op de pagina **Quick Start** op elk gewenst moment openen.)

1.  Klik op de koppeling die overeenkomt met de afbeelding die u wilt downloaden vanaf het Microsoft Download Center. De afbeeldingsbestanden zijn ongeveer 4,8 GB.

    -   VHDX voor Hyper-V op Windows Server 2012 en later

    -   VHD voor Hyper-V in Windows Server 2008 R2 en hoger

    -   VMDK VMWare ESXi 5.5 en hoger

2.  Download en unzip het bestand naar een lokale schijf, waardoor een notitie van waar het bestand uitgepakt zich bevindt.

![video pictogram](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **Video beschikbaar**

Bekijk de video voor stapsgewijze instructies aan de slag met de StorSimple virtuele matrix.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Optionele stap: een nieuwe opslag-account voor de service configureren

Dit is een optionele stap die worden alleen uitgevoerd moet als u het automatisch maken van een opslag-account met uw service niet heeft ingeschakeld.

Als u nodig hebt voor het maken van een account Azure opslag in een andere regio, raadpleegt u [een opslag-account maken](storage-create-storage-account.md#create-a-storage-account) voor stapsgewijze instructies.

De volgende stappen uitvoeren in [Azure klassieke portal](https://manage.windowsazure.com/) op de pagina StorSimple Manager service met een bestaande Microsoft Azure opslag account toevoegen.

#### <a name="to-add-a-storage-account"></a>Een opslag toevoegen

1.  Selecteer uw service op de portaalpagina StorSimple Manager-service, en dubbelklik erop. Hiervoor gaat u naar de pagina **Quick Start** . Selecteer de pagina **configureren** .

2.  Klik op **Add/edit opslag account**. Klik in het dialoogvenster **Add/Edit opslag Account** het volgende doen:

    1.  Klik op **Nieuwe toevoegen**.

    1.  Geef een naam voor de account van uw opslag.

    1.  De primaire **Sleutel van Access** voor uw opslag Microsoft Azure account opgeven.

    1.  Selecteer **SSL inschakelen modus** een beveiligd kanaal voor communicatie tussen het apparaat en de cloud maken. Schakel het selectievakje **SSL inschakelen modus** alleen als u in een private cloud werkt.

    1.  Klik op het pictogram voor ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). U krijgt bericht wanneer de opslag-account is gemaakt.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  De gemaakte opslag account wordt weergegeven op de pagina **configureren** onder **opslag rekeningen**. Klik op **Opslaan** om de gemaakte opslag account opgeslagen. Klik op **OK** wanneer u om bevestiging wordt gevraagd.


## <a name="next-step"></a>Volgende stap

De volgende stap is het inrichten van een virtuele machine voor het virtuele apparaat StorSimple. Zie de gedetailleerde instructies in uw host-besturingssysteem:

-   [Inrichten van een virtuele matrix StorSimple in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md)

-   [Inrichten van een virtuele matrix StorSimple in VMware](storsimple-ova-deploy2-provision-vmware.md)
