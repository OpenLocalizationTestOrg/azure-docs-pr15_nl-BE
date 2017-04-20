<properties
   pageTitle="StorSimple virtuele matrix 3 - implementatie van het virtuele apparaat als bestandsserver instellen"
   description="Deze derde zelfstudie in StorSimple virtuele matrix implementatie zorgt u voor het instellen van een virtueel apparaat als bestandsserver."
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
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Implementeren van StorSimple virtuele matrix - Set up als bestandsserver

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Inleiding 

Dit artikel is van toepassing op Microsoft Azure StorSimple virtuele matrix (ook bekend als de StorSimple op ruimten virtuele apparaatstuurprogramma of virtueel apparaat StorSimple) uitgevoerd maart 2016 algemene beschikbaarheid (GA) release. In dit artikel wordt beschreven hoe installatie uitvoeren, uw bestandsserver StorSimple registreren, het apparaat instellen te voltooien en maken en verbinding maken met een SMB-shares. Dit is het laatste artikel in de reeks van distributie, zelfstudies volledig uw virtuele matrix implementeren als een bestandsserver of een iSCSI-server vereist.

De installatie en configuratie kan duren ongeveer 10 minuten te voltooien.


## <a name="setup-prerequisites"></a>Setup-vereisten

Voordat u configureren en u het virtuele apparaat StorSimple stelt, zorg ervoor dat:

-   U hebt een virtueel apparaat ingericht en verbonden als bedoeld in de [bepaling van een virtuele matrix StorSimple in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) of [bepaling een virtuele matrix StorSimple in VMware](storsimple-ova-deploy2-provision-vmware.md).

-   U hebt de sleutel voor de registratie van de StorSimple Manager-service die u hebt gemaakt voor het beheren van virtuele apparaten StorSimple. Zie voor meer informatie [stap 2: de registratie sleutel ophalen](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) voor virtuele matrix StorSimple.

-   Als dit de tweede of volgende virtueel apparaat dat u in een bestaande StorSimple Manager-service registreren wilt, moet u de sleutel data service hebben. Deze sleutel is gegenereerd nadat het eerste apparaat met deze service is geregistreerd. Als u deze sleutel hebt verloren, Zie [de coderingssleutel service gegevens ophalen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) voor uw virtuele matrix StorSimple.

## <a name="step-by-step-setup"></a>Stapsgewijze installatie

Stapsgewijze instructies voor het instellen en configureren van het virtuele apparaat StorSimple gebruiken.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Stap 1: Het lokale web UI-installatie is voltooid en het apparaat registreren 


#### <a name="to-complete-the-setup-and-register-the-device"></a>De installatie is voltooid en het apparaat registreren

1.  Open een browservenster en de verbinding met het lokale web UI. Type: 

    `https://<ip-address of network interface>`

    Gebruik de verbindings-URL in de vorige stap hebt genoteerd. Er verschijnt een foutbericht dat aangeeft dat er een probleem met het beveiligingscertificaat van de website is. Klik op **Doorgaan naar deze webpagina**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Aanmelden bij de on line gebruikersinterface van het virtuele apparaat als **StorSimpleAdmin**. Geef het administrator-wachtwoord voor het apparaat dat u hebt gewijzigd in stap 3: Start het virtuele apparaat in [een virtuele matrix StorSimple in Hyper-V-voorziening](storsimple-ova-deploy2-provision-hyperv.md) of in [een virtuele matrix StorSimple in VMware voorziening](storsimple-ova-deploy2-provision-vmware.md).

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  U zal worden genomen op **de introductiepagina** . Deze pagina beschrijft de verschillende instellingen die zijn vereist voor het configureren en het virtuele apparaat registreren bij de StorSimple Manager-service. Houd er rekening mee dat de **instellingen van het netwerk**, **Web proxy-instellingen**en **tijdinstellingen** optioneel zijn. De enige vereiste instellingen zijn **Instellingen** en **instellingen van de wolk**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  Op de pagina **netwerkinstellingen** onder **Netwerkinterfaces**0 gegevens automatisch geconfigureerd voor u. Elke netwerkinterface is standaard ingesteld om automatisch IP-adres (DHCP). Daarom kan een IP-adres, subnetmasker en gateway automatisch toegewezen (voor zowel IPv4 als IPv6).

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Als u meer dan één netwerkinterface toegevoegd tijdens het inrichten van het apparaat, kunt u deze hier. Opmerking dat u kunt de netwerkinterface alleen of als zowel IPv4 als IPv6 als IPv4 configureren. Alleen IPv6-configuraties worden niet ondersteund.

1.  DNS-servers zijn vereist omdat ze worden gebruikt wanneer uw apparaat probeert te communiceren met uw cloud storage service-providers of kunt oplossen door het apparaat met de naam geconfigureerd als bestandsserver. Op de pagina **netwerkinstellingen** onder de **DNS-servers**:

    1.  Een primaire en secundaire DNS-server automatisch geconfigureerd. Als u statische IP-adressen configureren, kunt u DNS-servers. Voor hoge beschikbaarheid, raden wij u aan dat u een primaire en een secundaire DNS-server configureren.

    2.  Klik op **toepassen**. Dit toepassen en valideren van de netwerkinstellingen.

2.  Op de pagina **Instellingen voor** :

    1.  Een unieke **naam** toewijzen aan het apparaat. Deze naam kan 1-15 tekens en letter, cijfers en koppelstreepjes kan bevatten.

    2.  Klik op het pictogram **bestandsserver** ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) voor het **Type** apparaat dat u wilt maken. Een bestandsserver kunt u gedeelde mappen maken.

    3.  Als het apparaat een bestandsserver is, moet u het apparaat toevoegen aan een domein. Geef een **domeinnaam**.

    1.  Klik op **toepassen**.

2.  Er wordt een dialoogvenster weergegeven. Voer uw referenties voor het domein in de opgegeven indeling. Klik op het pictogram. Referenties voor het domein worden gecontroleerd. Er wordt een foutbericht weergegeven als de referenties onjuist zijn.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Klik op **toepassen**. Dit zal toepassen en valideren van de instellingen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OU) van Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast op of overgenomen. Groepsbeleid kan toepassingen, zoals antivirussoftware installeren op de virtuele matrix StorSimple. Installeren van extra software wordt niet ondersteund en kan leiden tot beschadiging van gegevens. 

1.  De web proxy-server (optioneel) configureren. Hoewel de web proxy-configuratie is optioneel, houd er rekening mee dat als u een webproxy gebruikt, u alleen deze hier configureren kunt worden.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    In de **webproxy** -pagina:

    1.  De **Web proxy-URL** in de volgende notatie: *http://&lt;host-IP-adres of de FULLY&gt;: nummer van de poort*. Opmerking de HTTPS-URL's worden niet ondersteund.

    2.  **Verificatie** opgeven als **standaard** of **geen**.

    3.  Als verificatie gebruikt, moet u ook een **gebruikersnaam** en **wachtwoord**.

    4.  Klik op **toepassen**. Zo valideren en past u de geconfigureerde web proxy-instellingen.

1.  De instellingen voor uw apparaat, zoals de tijdzone en de primaire en secundaire NTP-servers (optioneel) configureren. NTP-servers zijn vereist omdat het apparaat tijd synchroniseren moet zodat deze met uw cloud-aanbieders verifiëren kan.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    Op de pagina **Instellingen voor tijd** :

    1.  In de vervolgkeuzelijst, selecteer de **tijdzone** op basis van de geografische locatie waar het apparaat wordt ingezet. De standaardtijdzone voor uw apparaat is een PST-bestand. Het apparaat wordt deze tijdzone gebruiken voor alle geplande activiteiten.

    2.  Geef een **primaire NTP-server** voor uw apparaat of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat uw netwerk NTP-verkeer staat voor het doorsturen van uw datacenter met het Internet.

    3.  Geef desgewenst een **secundaire NTP-server** voor uw apparaat.

    4.  Klik op **toepassen**. Dit zal valideren en de geconfigureerde instellingen toepassen.

1.  Configureer de instellingen van de wolk voor uw apparaat. In deze stap maakt u voltooit de lokale configuratie en registreert u het apparaat in uw StorSimple Manager-service.

    1.  Voer de **registratie sleutel** die u hebt gekregen in [stap 2: ophalen van de sleutel voor de registratie van](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) voor virtuele matrix StorSimple.

    2.  Sla deze stap over als dit is het eerste apparaat registreren bij deze service en Ga naar de volgende stap. Als dit niet het eerste apparaat dat u met deze service registreren wilt moet u de **sleutel data codering**opgeven. Deze sleutel is vereist bij de registratie sleutel extra apparaten registreren bij de StorSimple Manager-service. Raadpleeg voor meer informatie als u de [sleutel data encryption](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) in het lokale web UI.

    3.  Klik op **registreren**. Hiermee wordt het apparaat opnieuw gestart. Wellicht moet u wachten tot 2-3 minuten voordat het apparaat correct is geregistreerd. Nadat u het apparaat opnieuw is opgestart, wordt u naar de aanmeldingspagina worden genomen.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Terug naar de klassieke Azure portal. Controleer of dat het apparaat is met de service verbonden door te zoeken naar de status op de pagina **apparaten** . Status van het apparaat moet **actief**zijn.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Stap 2: Vul de vereiste apparaat instellen

Als u de configuratie van uw apparaat StorSimple wilt moet u:

-   Selecteer een opslag-account koppelen aan uw apparaat.

-   Kies instellingen voor codering voor de gegevens die worden verzonden naar de cloud.

De volgende stappen uitvoeren in de [Azure klassieke portal](https://manage.windowsazure.com/) de installatie vereist apparaat te voltooien.

#### <a name="to-complete-the-minimum-device-setup"></a>De minimaal vereiste apparaatstuurprogramma installatie te voltooien

1.  Selecteer op de pagina **apparaten** het apparaat dat u zojuist hebt gemaakt. Dit apparaat zou weergegeven als **actief**. Klik op de pijl op de naam van het apparaat en klik vervolgens op **Snel starten**.

2.  Klik op **volledige apparaat instellen** om het apparaat configureren wizard te starten.

3.  In de wizard configureren apparaat op de pagina **Algemene instellingen** het volgende doen:

    1.  Geef een opslag-account moet worden gebruikt bij het apparaat. U kunt een bestaande account voor opslag in dit abonnement in de vervolgkeuzelijst selecteren of **meer toevoegen** om een account uit een ander abonnement.

    2.  Definieer de versleutelingsinstellingen voor alle de gegevens in rust (AES-codering) die worden verzonden naar de cloud. Controleer de keuzelijst met invoervak **cloud opslag coderingssleutel**inschakelen voor het coderen van uw gegevens. Voer een cloud opslag codering die 32 tekens bevat. Opnieuw op de toets om het te bevestigen. Een 256-bits AES-sleutel wordt gebruikt door de gebruiker gedefinieerde sleutel voor de codering.

    3.  Klik op het pictogram voor ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

De instellingen wordt nu bijgewerkt. Nadat de instellingen zijn bijgewerkt, wordt de complete inrichting setup knop grijs weergegeven. U gaat terug naar de pagina **Quick Start** van apparaat.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> U kunt alle andere apparaatinstellingen op elk gewenst moment wijzigen via de pagina **configureren** .

## <a name="step-3-add-a-share"></a>Stap 3: Share toevoegen

De volgende stappen uitvoeren in de [Azure klassieke portal](https://manage.windowsazure.com/) een share te maken.

#### <a name="to-create-a-share"></a>Een share maken

1.  Klik op **een share toevoegen**op de pagina apparaat **Snel starten** . Hiermee start u de wizard delen toevoegen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Op de pagina **Algemene instellingen** het volgende doen:

    1.  Geef een unieke naam voor de share. De naam moet een tekenreeks met 3 tot en met 127 tekens.

    2.  (Optioneel) Geef een beschrijving voor de share. De beschrijving helpt de eigenaars delen te identificeren.

    3.  Selecteer een gebruikstype voor de share. Het gebruikstype is **Tiered** of **lokaal vastgemaakt**, met trapsgewijs geschakeld worden. Selecteer voor werkbelasting waarvoor lokale garanties, lage vertragingstijden en hogere prestaties, een share **lokaal vastgemaakt** . Selecteer een **Tiered** share voor alle andere gegevens.

    Een lokaal vaste delen dik is ingericht en zorgt ervoor dat de primaire gegevens op de share lokaal op het apparaat blijft en Mors geen naar de cloud. Een gelaagde share is aan de andere kant dun ingericht. Wanneer u een gelaagde share maakt, wordt 10% van de ruimte is ingericht op het lokale niveau en 90% van de ruimte is ingericht in de cloud. Bijvoorbeeld als u een volume van 1 TB ingericht, 100 GB zou zich bevinden in de lokale ruimte en 900 GB in de cloud zou worden gebruikt wanneer de niveaus van de gegevens. Dit betekent op zijn beurt als u weinig lokale ruimte op het apparaat, kunt u een gelaagde share niet kunt inrichten.

1.  Geef de ingerichte capaciteit voor de share. Houd er rekening mee dat de opgegeven capaciteit kleiner zijn dan de beschikbare capaciteit moet. Als een gelaagde share, wordt de grootte van de share moet tussen 500 GB en 20 TB. Geef de grootte van een aandeel tussen 50 GB en 2 TB voor een lokaal vaste delen. De beschikbare capaciteit gebruiken als richtlijn voor het inrichten van een share. Als de beschikbare lokale capaciteit 0 GB, zal vervolgens u niet mag inrichten van lokale of gelaagde aandelen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Klik op het pijlpictogram ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) om naar de volgende pagina te gaan.

1.  Op de pagina **Extra instellingen** moet u machtigingen toewijzen aan de gebruiker of de groep die toegang deze share tot. Geef de naam van de gebruiker of de groep in *<john@contoso.com>* indeling. U wordt aangeraden een groep (in plaats van één gebruiker) admin toegangsrechten voor deze shares toestaan. Nadat u hier de machtigingen hebt toegewezen, kunt u vervolgens Windows Verkenner gebruiken om deze machtigingen te wijzigen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Klik op het pictogram voor ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Een share wordt gemaakt met de opgegeven instellingen. Controle en back-up zal worden standaard voor de share.

## <a name="step-4-connect-to-the-share"></a>Stap 4: Verbinding maken met de share

Nu moet u verbinding maken met de share(s) die u in de vorige stap hebt gemaakt. Deze stappen uitvoeren op de Windows Server-host.

#### <a name="to-connect-to-the-share"></a>Verbinding maken met de share.

1.  Druk op ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. Geef in het venster uitvoeren op de * \\ * als het pad vervangen door een *bestandsnaam voor de server* de naam van het apparaat dat u hebt toegewezen aan uw bestandsserver. Klik op **OK**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Deze wordt Explorer geopend. Nu moet u kunnen zien van de aandelen die u hebt gemaakt als mappen. Selecteer en dubbelklik op een share (map) om de inhoud weer te geven.

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  U kunt nu bestanden toevoegen aan deze aandelen en een back-up te nemen.

![video pictogram](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **Video beschikbaar**

Bekijk de video om te zien hoe u kunt configureren en een virtuele matrix StorSimple registreren als een bestandsserver.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van het lokale web UI voor het [beheer van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md).
