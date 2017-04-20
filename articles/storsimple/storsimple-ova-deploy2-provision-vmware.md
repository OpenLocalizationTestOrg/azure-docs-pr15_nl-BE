<properties
   pageTitle="StorSimple virtuele matrix - bepaling in VMware implementeren"
   description="Deze tweede zelfstudie in StorSimple virtuele matrix implementatie reeks heeft betrekking op een virtueel apparaat in VMware inrichten."
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
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Implementeren van virtuele matrix StorSimple - inrichten van een virtuele matrix in VMware

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Overzicht 
Deze zelfstudie inrichten van toepassing is op de actieve release van maart 2016 algemene beschikbaarheid (GA) StorSimple virtuele matrices (ook bekend als StorSimple op ruimten virtuele apparaten of virtuele apparaten StorSimple). In deze zelfstudie wordt beschreven hoe u inrichten en verbinding maken met een virtuele matrix StorSimple op een hostsysteem met VMware ESXi 5.5 en hoger. Dit artikel is van toepassing op de implementatie van StorSimple virtuele matrices in Azure klassieke portal zoals Microsoft Azure overheid wolk.

U moet administrator-bevoegdheden om in te richten en maak verbinding met een virtueel apparaat. Het inrichten en de initiële setup kan ongeveer 10 minuten duren om te voltooien.


## <a name="provisioning-prerequisites"></a>Inrichten van vereisten

Hier vindt u de vereisten voor het inrichten van een virtueel apparaat op een hostsysteem met VMware ESXi 5.5 en hoger.

### <a name="for-the-storsimple-manager-service"></a>Voor de StorSimple Manager-service

Voordat u begint, zorg ervoor dat:

-   U kunt de stappen in [de portal voor virtuele matrix StorSimple voorbereiden](storsimple-ova-deploy1-portal-prep.md)hebt voltooid.

-   U hebt de afbeelding virtueel apparaat voor VMware gedownload vanaf de portal Azure. Zie voor meer informatie [stap 3: Download het image virtueel apparaat](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>Voor het virtuele apparaat StorSimple 

Voordat u een virtueel apparaat implementeert, zorg ervoor dat:

-   U toegang hebt tot een hostsysteem met Hyper-V (2008 R2 of later) die kunnen worden gebruikt om een bepaling van een apparaat.

-   Het hostsysteem kan besteden aan het inrichten van het virtuele apparaat de volgende bronnen:

    -   Minimaal 4 cores.

    -   Minimaal 8 GB aan RAM-geheugen.

    -   Een netwerk-interface.

    -   Een 500 GB virtuele schijf systeemgegevens.

### <a name="for-the-network-in-datacenter"></a>Voor het netwerk in het datacenter 

Voordat u begint, zorg ervoor dat:

-   U hebt de netwerkvereisten voor de implementatie van een virtueel apparaat StorSimple herzien en het datacenter-netwerk aan de hand van de eisen geconfigureerd. Zie [virtuele matrix StorSimple systeemvereisten](storsimple-ova-system-requirements.md)voor meer informatie.

## <a name="step-by-step-provisioning"></a>Stapsgewijze inrichten 

Als u wilt inrichten en verbinding met een virtueel apparaat, moet u de volgende stappen uitvoeren:

1.  Zorg ervoor dat de hostsysteem over voldoende middelen beschikt om te voldoen aan de eisen van het minimum aantal virtuele apparaatstuurprogramma.

2.  Een virtueel apparaat in de hypervisor inrichten.

3.  Het virtuele apparaat start en het IP-adres.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Stap 1: Controleer hostsysteem voldoet aan minimale virtueel apparaat

U maakt een virtueel apparaat, hebt u het volgende nodig:

-   Toegang tot een hostsysteem met VMware ESXi Server 5.5 en hoger.

-   VMware vSphere client op uw systeem voor het beheren van de ESXi host.

    -   Minimaal 4 cores.

    -   Minimaal 8 GB aan RAM-geheugen.

    -   Een netwerkinterface verbonden met het netwerk kan routeren van verkeer op Internet. De minimale bandbreedte van Internet moet 5 Mbps voor optimale werking van het apparaat.

    -   Een 500 GB virtuele schijf voor gegevens.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Stap 2: Een virtueel apparaat in hypervisor inrichten

Voer de volgende stappen uit om een virtueel apparaat in de hypervisor inrichten.

1.  Kopieer de afbeelding virtueel apparaat op uw systeem. Dit is de afbeelding die u hebt gedownload via de klassieke Azure portal. 
    1.  Zorg ervoor dat dit het laatste bestand dat u hebt gedownload. Als u de afbeelding eerder hebt gedownload, het downloaden opnieuw om dat de laatste afbeelding. De meest recente afbeelding heeft twee bestanden (in plaats van één).
    2.  Maak een notitie van de locatie waar u de afbeelding gekopieerd als u van deze later in de procedure gebruikmaakt.

2.  Log in op de server ESXi is met de vSphere client. U moet administrator-bevoegdheden voor het maken van een virtuele machine.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  Selecteer de ESXi Server in de vSphere client, in de sectie van de voorraad in het linkerdeelvenster.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  U wordt eerst de VMDK uploaden naar de server ESXi. Ga naar het tabblad **configuratie** in het rechterdeelvenster. Selecteer onder **Hardware**, **opslag**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  Selecteer in het rechterdeelvenster, onder **Datastores**, het gegevensarchief waar u de VMDK te uploaden. Het gegevensarchief moet voldoende vrije ruimte voor het OS en data schijven hebben.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Klik met de rechtermuisknop en selecteer **Datastore bladeren**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  Een **Gegevensarchief** browservenster wordt weergegeven.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Klik in de werkbalk op ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) het pictogram een nieuwe map te maken. Naam van de map opgeven en noteer deze. U gebruikt deze mapnaam later bij het maken van een virtuele machine (aanbevolen beste praktijken). Klik op **OK**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  De nieuwe map wordt weergegeven in het linkerdeelvenster van de **Datastore Browser**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Klik op het pictogram uploaden ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) en selecteer **Bestand uploaden**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  U moet nu bladeren en wijs de VMDK bestanden die u hebt gedownload. Er worden twee bestanden. Selecteer een bestand te uploaden.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Klik op **openen**. Hiermee start u nu het uploaden van de VMDK bestand met de opgegeven gegevensarchief. Het kan enkele minuten duren voordat het bestand te uploaden.


1.  Nadat het uploaden voltooid is, ziet u het bestand in het gegevensarchief in de map die u hebt gemaakt. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Nu moet u de tweede VMDK bestand uploaden naar de dezelfde datastore.

1.  Terug naar het venster vSphere client. Met server ESXi is geselecteerd, klik met de rechtermuisknop en selecteer **nieuwe virtuele Machine**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  Een **nieuwe virtuele Machine maken** -venster wordt weergegeven. Selecteer de optie **aangepast** op de pagina **configuratie** . Klik op **volgende**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Geef de naam van de virtuele machine op de pagina **naam en locatie** . Deze naam moet overeenkomen met de mapnaam (aanbevolen beste praktijken) die u eerder in stap 8 hebt opgegeven.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Selecteer op de pagina **opslag** een gegevensarchief dat u gebruiken wilt voor het inrichten van uw VM.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Selecteer op de pagina **Virtuele Machine versie** **versie van de virtuele Machine: 8**. Houd er rekening mee dat versie 8 tot en met 11 worden allemaal ondersteund.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  Selecteer op de pagina **gastbesturingssysteem** het **gastbesturingssysteem** als **Windows**. Selecteer in de vervolgkeuzelijst voor **versie**, **Microsoft Windows Server 2012 (64-bits)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  Op de pagina **CPU's** , het **aantal virtuele sockets** en **het aantal cores per socket virtuele** zodanig aanpassen dat het **totale aantal kernen** is 4 (of meer). Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Geef op de pagina **geheugen** 8 GB (of meer) RAM-geheugen. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Geef het nummer van de netwerkinterfaces op de pagina **netwerk** . De minimumvereiste is een netwerk-interface.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  Accepteer de standaardinstelling **LSI Logic SAS-controller**op de pagina **SCSI-Controller** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Kies op de pagina **Selecteer een schijf** **een bestaande virtuele schijf gebruiken**. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  Klik op de pagina **Bestaande schijf selecteren** onder **Pad naar schijf**, klikt u op **Bladeren**. Hiermee opent u een dialoogvenster **Bladeren Datastores** . Ga naar de locatie waar u de VMDK geüpload. Nu ziet u slechts één bestand in de map datastore als de twee bestanden die u in eerste instantie hebt geüpload zijn samengevoegd. Selecteer het bestand en klik op **OK**. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Accepteer de standaardinstelling op de pagina **Geavanceerde opties** en klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  Bekijk alle instellingen die zijn gekoppeld aan de nieuwe virtuele machine op de pagina **Gereed om te voltooien** . Controleer of **Bewerk de instellingen van de virtuele machine voor voltooiing**. Klik op **Doorgaan**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Zoek het apparaat in het tabblad **Hardware** op de pagina **Eigenschappen van virtuele Machines** . Selecteer de **nieuwe vaste schijf**. Klik op **toevoegen**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  **Hardware toevoegen** in het venster verschijnt. Op de pagina **Type apparaat** onder **Kies het type apparaat dat u wilt toevoegen**, selecteert u **harde schijf** en klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Kies **een nieuwe virtuele schijf maken**op de pagina **Selecteer een schijf** . Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  Wijzig de **Grootte van de schijf** tot 500 GB (of meer) op de pagina **maken van een schijf** . Selecteer onder **Schijf inrichten**, **Dunne bepaling**. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Accepteer de standaardinstelling op de pagina **Geavanceerde opties** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  Bekijk de opties voor Schijfcontrole op de pagina **Gereed om te voltooien** . Klik op **Voltooien**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  U keert nu terug naar de pagina eigenschappen van de virtuele Machine. Een nieuwe vaste schijf wordt toegevoegd aan uw virtuele machine. Klik op **Voltooien**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  Met de virtuele machine hebt geselecteerd in het rechterdeelvenster, Ga naar het tabblad **Samenvatting** . Controleer de instellingen voor de virtuele machine.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

De virtuele machine is nu ingericht. De volgende stap is een kracht op deze computer en het IP-adres.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Stap 3: Het virtuele apparaat begint en het OT

Voer de volgende stappen uit om het virtuele apparaat starten en verbinding te maken.

#### <a name="to-start-the-virtual-device"></a>Het virtuele apparaat starten

1.  Het virtuele apparaat starten. Selecteer het apparaat in de vSphere Configuration Manager in het linkerdeelvenster en klik met de rechtermuisknop om het contextmenu weer te geven. Selecteer **energie** en selecteer vervolgens **aan**. Dit moet voor energiebeheer op uw virtuele machine. U kunt de status bekijken in het deelvenster onder **Recente taken** van de vSphere client.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  De taken wordt een paar minuten in beslag nemen. Zodra het apparaat actief is, gaat u naar het tabblad **Console** . Ctrl + Alt + Delete aan te melden bij het apparaat verzenden. U kunt ook wijst de cursor op het consolevenster en druk op Ctrl + Alt + Insert. De standaardgebruiker is *StorSimpleAdmin* en is het standaardwachtwoord *Wachtwoord1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Om veiligheidsredenen verloopt het apparaat administrator-wachtwoord bij de eerste aanmelding op. U wordt gevraagd het wachtwoord te wijzigen.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Geef een wachtwoord op dat ten minste 8 tekens bevat. Het wachtwoord moet 3 van 4 van deze vereisten bevatten: hoofdletters, kleine letters, cijfers en speciale tekens. Geef het wachtwoord ter bevestiging opnieuw. U wordt gewaarschuwd dat het wachtwoord is gewijzigd.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Nadat het wachtwoord is gewijzigd, kan het virtuele apparaat opnieuw. Wachten op het opnieuw opstarten te voltooien. De Windows PowerShell-console van het apparaat en een voortgangsbalk weergegeven.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  Stap 6-8 zijn alleen van toepassing wanneer in een omgeving met DHCP niet starten. Als u in een omgeving met DHCP, slaat u deze stappen over en gaat u naar stap 9. Als u van uw apparaat in niet-DHCP-omgeving wordt opgestart, verschijnt het volgende scherm. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Nu moet u het netwerk configureren.

1.  Gebruik de `Get-HcsIpAddress` opdracht om een lijst van de netwerkinterfaces op het virtuele apparaat is ingeschakeld. Als uw apparaat een afzonderlijke netwerkinterface is ingeschakeld heeft, wordt de standaardnaam die wordt toegewezen aan deze interface is `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Gebruik de `Set-HcsIpAddress` cmdlets voor het configureren van het netwerk. Hieronder vindt u een voorbeeld:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Nadat de installatie voltooid is en het apparaat omhoog is opgestart, ziet u de bannertekst apparaat. Noteer het IP-adres en de URL in de tekst van het vaandel wordt weergegeven voor het beheren van het apparaat. U kunt dit IP-adres verbinding maken met het web UI van het virtuele apparaat en de lokale instellingen en de registratie te voltooien.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Optioneel) Deze stap alleen uitvoeren als u het apparaat in de Cloud overheid implementeert. U kunt nu de Amerikaanse Federal Information Processing Standard (FIPS) modus op uw apparaat. De FIPS 140-standaard definieert de cryptografische algoritmen die zijn goedgekeurd voor gebruik door ons federale overheidssystemen voor de bescherming van gevoelige gegevens.
    1. Als u de FIPS-modus, voert u de volgende cmdlet:
        
        `Enter-HcsFIPSMode`

    2. Het apparaat opnieuw nadat u de FIPS-modus hebt ingeschakeld zodat de cryptografische validaties van kracht.

        > [AZURE.NOTE] U kunt inschakelen of de FIPS-modus uitschakelt op uw apparaat. Afwisselend van het apparaat tussen FIPS en niet-FIPS-modus wordt niet ondersteund.


Als het apparaat niet aan de minimale configuratievereisten voldoet, ziet u een fout in de tekst van het vaandel (Zie hieronder). U moet de configuratie zodanig wijzigen dat er voldoende middelen om te voldoen aan de minimale vereisten. U kunt vervolgens opnieuw starten en verbinding maken met het apparaat. Verwijzen naar de minimale configuratievereisten in [stap 1: Zorg ervoor dat het hostsysteem voldoet aan minimale virtueel apparaat](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Als u een andere fout tijdens de eerste configuratie met behulp van het lokale web UI wordt geconfronteerd, raadpleegt u de volgende workflows in [uw virtuele matrix StorSimple beheren met behulp van het lokale web UI](storsimple-ova-web-ui-admin.md).

-   Diagnostische tests uitvoeren [web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)oplossen.

-   [Logboek-pakket genereren en logboekbestanden weergeven](storsimple-ova-web-ui-admin.md#generate-a-log-package)...

## <a name="next-steps"></a>Volgende stappen

-   [Uw virtuele matrix StorSimple ingesteld als een bestandsserver](storsimple-ova-deploy3-fs-setup.md)

-   [Uw StorSimple Virtual Array als een iSCSI-server instellen](storsimple-ova-deploy3-iscsi-setup.md)

