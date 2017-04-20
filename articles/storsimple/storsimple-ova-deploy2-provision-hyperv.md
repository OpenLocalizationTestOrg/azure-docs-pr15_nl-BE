<properties
   pageTitle="StorSimple virtuele matrix - bepaling in Hyper-V installeren"
   description="Deze tweede zelfstudie in StorSimple virtuele matrix implementatie omvat een virtueel apparaat in Hyper-V wordt ingericht."
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
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Implementeren van virtuele matrix StorSimple - inrichten van een virtuele matrix in Hyper-V

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Overzicht

Deze zelfstudie inrichten van toepassing is op de actieve release van maart 2016 algemene beschikbaarheid (GA) Microsoft Azure StorSimple virtuele matrices (ook bekend als StorSimple op ruimten virtuele apparaten of StorSimple virtuele apparaten). In deze zelfstudie wordt beschreven hoe u een virtuele matrix StorSimple op een hostsysteem waarop Hyper-V wordt uitgevoerd op Windows Server 2012 R2, Windows Server 2012- of Windows Server 2008 R2 inrichten. Dit artikel is van toepassing op de implementatie van StorSimple virtuele matrices in Azure klassieke portal zoals Microsoft Azure overheid wolk.

U moet administrator-bevoegdheden om in te richten en een virtueel apparaat te configureren. Het inrichten en de initiële setup kan ongeveer 10 minuten duren om te voltooien.


## <a name="provisioning-prerequisites"></a>Inrichten van vereisten

Hier vindt u de vereisten voor het inrichten van een virtueel apparaat op een hostsysteem waarop Hyper-V wordt uitgevoerd op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2.

### <a name="for-the-storsimple-manager-service"></a>Voor de StorSimple Manager-service

Voordat u begint, zorg ervoor dat:

-   U kunt de stappen in [de portal voor virtuele matrix StorSimple voorbereiden](storsimple-ova-deploy1-portal-prep.md)hebt voltooid.

-   Vanaf de portal Azure, hebt u de afbeelding virtueel apparaat gedownload voor Hyper-V. Zie voor meer informatie [stap 3: Download het image virtueel apparaat](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] De software die wordt uitgevoerd op de virtuele matrix StorSimple kan alleen worden gebruikt in combinatie met de Storsimple Manager-service.

### <a name="for-the-storsimple-virtual-device"></a>Voor het virtuele apparaat StorSimple

Voordat u een virtueel apparaat implementeert, zorg ervoor dat:

-   U toegang hebt tot een hostsysteem met Hyper-V in Windows Server 2008 R2 of hoger die kan worden gebruikt om een bepaling van een apparaat.

-   Het hostsysteem kan besteden aan het inrichten van het virtuele apparaat de volgende bronnen:

    -   Minimaal 4 cores.

    -   Minimaal 8 GB aan RAM-geheugen.

    -   Een netwerk-interface.

    -   Een 500 GB virtuele schijf systeemgegevens.

### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter

Voordat u begint, bekijk de netwerkvereisten te implementeren een virtueel apparaat StorSimple en het datacenter-netwerk op de juiste wijze configureren. Zie [virtuele matrix StorSimple netwerkvereisten](storsimple-ova-system-requirements.md#networking-requirements)voor meer informatie.

## <a name="step-by-step-provisioning"></a>Stapsgewijze inrichten

Als u wilt inrichten en verbinding met een virtueel apparaat, moet u de volgende stappen uitvoeren:

1.  Zorg ervoor dat de hostsysteem over voldoende middelen beschikt om te voldoen aan de eisen van het minimum aantal virtuele apparaatstuurprogramma.

2.  Een virtueel apparaat in de hypervisor inrichten.

3.  Het virtuele apparaat start en het IP-adres.

Elk van deze stappen wordt uitgelegd in de volgende secties.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Stap 1: Zorg ervoor dat de hostsysteem aan de minimale virtueel apparaat voldoet

U maakt een virtueel apparaat, hebt u het volgende nodig:

-   De functie Hyper-V is geïnstalleerd op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 SP1.

-   Microsoft Hyper-V-beheer op een Microsoft Windows-client verbinding met de host.

U moet ervoor zorgen dat de onderliggende hardware (hostsysteem) waarop u het virtuele apparaat maakt, kan de volgende bronnen reserveren voor het virtuele apparaat:

- Minimaal 4 cores.
- Minimaal 8 GB aan RAM-geheugen.
- Een netwerk-interface.
- Een 500 GB virtuele schijf systeemgegevens.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Stap 2: Een virtueel apparaat in hypervisor inrichten

Voer de volgende stappen uit om een apparaat in de hypervisor inrichten.

#### <a name="to-provision-a-virtual-device"></a>Inrichten van een virtueel apparaat

1.  Op de Windows Server-host door de afbeelding virtueel apparaat te kopiëren naar een lokale schijf. Dit is de afbeelding die u hebt gedownload via de portal Azure (VHD of VHDX). Maak een notitie van de locatie waar u de afbeelding gekopieerd als u van deze later in de procedure gebruikmaakt.

2.  Open **Serverbeheer**. Klik op **Extra** en selecteer **Hyper-V-beheer**in de rechterbovenhoek.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Als u Windows Server 2008 R2 uitvoert, opent u de Hyper-V-beheer. Klik in Server Manager op **rollen > Hyper-V > Hyper-V-beheer**.

1.  In **Hyper-V-beheer**in het bereikvenster met de rechtermuisknop op het knooppunt van het systeem om het contextmenu te openen en klik vervolgens op **Nieuw** > **virtuele Machine**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Klik op **volgende**op de pagina **voordat u begint** van de Wizard Nieuwe virtuele Machine.

1.  Het virtuele apparaat voorzien van een **naam** op de pagina **Geef de naam en locatie** . Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  Kies het type apparaat afbeelding en klik op **volgende**op de pagina **Geef genereren** . Deze pagina wordt niet weergegeven als u werkt met Windows Server 2008 R2.

    * **Generatie 2** kiezen als u een afbeelding van de .vhdx voor Windows Server 2012 of later hebt gedownload.
    * Kies de **generatie 1** als u een .vhd-installatiekopie voor Windows Server 2008 R2 of hoger gedownload.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  Geef op de pagina **toewijzen van geheugen** een **opstarten geheugen** ten minste **8192 MB**, niet dynamisch geheugen inschakelen en klik vervolgens op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  Geef de virtual switch die is verbonden met het Internet en klik op **volgende**op de pagina **netwerk configureren** .

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  Kies **een bestaande virtuele harde schijf gebruikt**, geeft u de locatie van de afbeelding virtueel apparaat (.vhdx of VHD) aan en klik op **volgende**op de pagina **verbinding maken met virtuele harde schijf** .

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Bekijk het **Overzicht** en klik op **Voltooien** om de virtuele machine maken. Maar niet gaan nog - moet u nog steeds bepaalde processor-cores en een tweede schijf toe te voegen. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Om te voldoen aan de minimale vereisten, moet u 4 cores. Virtuele processors toevoegen aan uw hostsysteem dat is geselecteerd in het venster **Hyper-V-beheer** in het rechterdeelvenster onder de lijst met **virtuele Machines**, Ga naar de virtuele machine die u zojuist hebt gemaakt. Selecteer en klik met de rechtermuisknop op de naam van de computer en selecteer **Instellingen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Klik op de pagina **-Instellingen** in het linkerdeelvenster op **Processor**. Instellen **aantal virtuele processors** in 4 (of meer) in het rechterdeelvenster. Klik op **toepassen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Om te voldoen aan de minimale vereisten, moet u ook een virtuele data 500 GB schijf toevoegen. In de pagina **-Instellingen** :

    1.  Selecteer in het linkerdeelvenster de **SCSI-Controller**.
    2.  **Harde schijf,** Selecteer in het rechterdeelvenster en klik op **toevoegen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  Selecteer de optie **virtuele vaste schijf** en op **Nieuw**op de pagina **harde schijf** . Hiermee start u de **Wizard Nieuwe virtuele hardeschijf**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Klik op **volgende**op de pagina **voordat u begint** van de Wizard Nieuwe virtuele hardeschijf.

1.  Accepteer de standaardwaarde van de **VHDX** op de **pagina schijfindeling kiezen**. Klik op **volgende**. Dit scherm wordt niet weergegeven als u werkt met Windows Server 2012 R2 of Windows Server 2008 R2.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  Op de **pagina Type schijf kiest**, Geef virtuele harde schijf als een **dynamisch uitbreidbare** (aanbevolen). Als u **de grootte van de vaste** schijf, werkt ook, maar moet u een lange wachttijd. Wij raden aan dat u niet de optie **Differencing** gebruikt. Klik op **volgende**. Let erop dat **dynamisch uitbreidbare** standaard in Windows Server 2012 R2 en Windows Server 2012. Standaard wordt in Windows Server 2008 R2 **vaste grootte**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Bieden een **naam** als **locatie** (u kunt bladeren naar een) voor de gegevensschijf op de pagina **naam en locatie opgeven** . Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  Selecteer de optie **een nieuwe lege virtuele schijf maken** en de grootte opgeven als **500 GB** (of meer) op de pagina **Schijf configureren** . 500 GB is de minimale vereiste, kunt u altijd een grotere schijf inrichten. Houd er rekening mee dat u niet kunt vergroten of verkleinen van de schijf eenmaal ingericht. Bekijk de sizing sectie in het document met [Aanbevolen procedures](storsimple-ova-best-practices.md#configuration-best-practices) voor meer informatie over de grootte van de schijf naar de voorziening. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  De details van de van virtuele gegevensschijf controleren op de pagina **Samenvatting** en indien wordt voldaan, klikt u op **Voltooien** om het maken van de schijf. De wizard wordt gesloten en een virtuele harde schijf naar uw computer worden toegevoegd.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  U keert terug naar de pagina **Instellingen** . Klik op **OK** om de pagina **Instellingen** te sluiten en terug te keren naar Hyper-V-beheer.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Stap 3: Het virtuele apparaat begint en het OT

Voer de volgende stappen uit om het virtuele apparaat starten en verbinding te maken.

#### <a name="to-start-the-virtual-device"></a>Het virtuele apparaat starten

1.  Het virtuele apparaat starten.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Nadat het apparaat actief is, selecteert u het apparaat, klik met de rechtermuisknop en selecteer **verbinding maken**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  U moet wachten 5-10 minuten voor het apparaat gereed is. Een statusbericht wordt weergegeven op de console om de voortgang te geven. Nadat het apparaat klaar is, gaat u naar de **actie**. Druk op `Ctrl + Alt + Delete` aan te melden bij het virtuele apparaat. De standaardgebruiker is *StorSimpleAdmin* en is het standaardwachtwoord *Wachtwoord1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Om veiligheidsredenen verloopt het apparaat administrator-wachtwoord bij de eerste aanmelding op. U wordt gevraagd het wachtwoord te wijzigen.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Geef een wachtwoord op dat ten minste 8 tekens bevat. Het wachtwoord moet ten minste 3 van de volgende 4 voorwaarden voldoen: hoofdletters, kleine letters, cijfers en speciale tekens. Geef het wachtwoord ter bevestiging opnieuw. U wordt gewaarschuwd dat het wachtwoord is gewijzigd.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Het virtuele apparaat opnieuw opgestart nadat het wachtwoord is gewijzigd. Wacht tot het apparaat te starten.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    De Windows PowerShell-console van het apparaat en een voortgangsbalk weergegeven.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  Stap 6-8 zijn alleen van toepassing wanneer in een omgeving met DHCP niet starten. Als u in een omgeving met DHCP, slaat u deze stappen over en gaat u naar stap 9. Als u van uw apparaat in niet-DHCP-omgeving wordt opgestart, verschijnt het volgende scherm.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Nu moet u het netwerk configureren.

1.  Gebruik de `Get-HcsIpAddress` opdracht om een lijst van de netwerkinterfaces op het virtuele apparaat is ingeschakeld. Als uw apparaat een afzonderlijke netwerkinterface is ingeschakeld heeft, wordt de standaardnaam die wordt toegewezen aan deze interface is `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Gebruik de `Set-HcsIpAddress` cmdlets voor het configureren van het netwerk. Hieronder vindt u een voorbeeld:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Nadat de installatie voltooid is en het apparaat omhoog is opgestart, ziet u de bannertekst apparaat. Noteer het IP-adres en de URL in de tekst van het vaandel wordt weergegeven voor het beheren van het apparaat. U kunt dit IP-adres verbinding maken met het web UI van het virtuele apparaat en de lokale instellingen en de registratie te voltooien.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Optioneel) Deze stap alleen uitvoeren als u het apparaat in de Cloud overheid implementeert. U kunt nu de Amerikaanse Federal Information Processing Standard (FIPS) modus op uw apparaat. De FIPS 140-standaard definieert de cryptografische algoritmen die zijn goedgekeurd voor gebruik door ons federale overheidssystemen voor de bescherming van gevoelige gegevens.
    1. Als u de FIPS-modus, voert u de volgende cmdlet:

        `Enter-HcsFIPSMode`

    2. Het apparaat opnieuw nadat u de FIPS-modus hebt ingeschakeld zodat de cryptografische validaties van kracht.

        > [AZURE.NOTE] U kunt inschakelen of de FIPS-modus uitschakelt op uw apparaat. Afwisselend van het apparaat tussen FIPS en niet-FIPS-modus wordt niet ondersteund.

Als het apparaat niet aan de minimale configuratievereisten voldoet, ziet u een fout in de tekst van het vaandel (Zie hieronder). U moet de configuratie zodanig wijzigen dat er voldoende middelen om te voldoen aan de minimale vereisten. U kunt vervolgens opnieuw starten en verbinding maken met het apparaat. Verwijzen naar de minimale configuratievereisten in [stap 1: Zorg ervoor dat het hostsysteem voldoet aan minimale virtueel apparaat](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Als u een andere fout tijdens de eerste configuratie met behulp van het lokale web UI wordt geconfronteerd, raadpleegt u de volgende workflows in [uw virtuele matrix StorSimple beheren met behulp van het lokale web UI](storsimple-ova-web-ui-admin.md).

-   Diagnostische tests uitvoeren [web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)oplossen.

-   [Logboek-pakket genereren en logboekbestanden weergeven](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![video pictogram](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **Video beschikbaar**

Bekijk de video om te zien hoe u een virtuele matrix StorSimple in Hyper-V kunt inrichten.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Volgende stappen

-   [Uw virtuele matrix StorSimple ingesteld als een bestandsserver](storsimple-ova-deploy3-fs-setup.md)

-   [Uw StorSimple Virtual Array als een iSCSI-server instellen](storsimple-ova-deploy3-iscsi-setup.md)
