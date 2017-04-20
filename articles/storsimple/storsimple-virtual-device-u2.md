<properties 
   pageTitle="StorSimple virtueel apparaat Update 2 | Microsoft Azure"
   description="Informatie over het maken, distribueren en beheren van een virtueel apparaat StorSimple in een virtueel netwerk Microsoft Azure. (Van toepassing op StorSimple Update 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Implementeren en beheren van een virtueel apparaat StorSimple in Azure


##<a name="overview"></a>Overzicht
Het virtuele apparaat voor StorSimple 8000-serie is een extra mogelijkheid die wordt geleverd met uw Microsoft Azure StorSimple oplossing. Het virtuele apparaat StorSimple wordt uitgevoerd op een virtuele machine in een virtueel netwerk Microsoft Azure en u kunt deze back-up maken en gegevens uit uw hosts klonen. In deze zelfstudie wordt beschreven hoe u implementeren en beheren van een virtueel apparaat in Azure en is van toepassing op de virtuele apparaten met softwareversie Update 2 en lager.


#### <a name="virtual-device-model-comparison"></a>Virtueel apparaat model vergelijking

Het virtuele apparaat StorSimple is beschikbaar in twee modellen, een standaard 8010 (voorheen bekend als de 1100) en een premie 8020 (geïntroduceerd in Update 2). Een vergelijking van de twee modellen onder tabelindeling.


| Model met          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Maximale capaciteit**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure VM**              | Standard_A3 (4 cores, 7 GB geheugen)                                                                      | Standard_DS3 (4 cores, 14 GB geheugen)                                                                                                                          |
| **Versiecompatibiliteit** | Versies bijwerken vooraf 2 of hoger                                             | Versies met Update 2 of hoger                                                                                                  |
| **Beschikbaarheid van de regio**   | Alle Azure regio 's                                                         | Azure regio's die ondersteuning bieden voor Premium-opslag<br></br>Zie voor een lijst van regio's, [gebieden voor 8020 ondersteund](#supported-regions-for-8020) |
| **Opslagtype**          | Standaardopslag Azure gebruikt voor lokale schijven<br></br> Meer informatie over het [maken van een standaardopslag account]() | Azure Premium opslag gebruikt voor lokale schijven<sup>2</sup> <br></br>Meer informatie over het [maken van een rekening van de premie opslag](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)                                                               |
| **Werkbelasting richtlijnen**     | Item niveau voor het ophalen van bestanden van de back-ups                                              | Cloud dev en het testen van scenario's, lage latentie en hogere prestaties werkbelasting <br></br>Secundaire apparaat voor noodherstel                                                                                            |
 
<sup>1</sup> *voorheen bekend als de 1100*.

<sup>2</sup> *zowel de 8010 8020 gebruik Azure standaardopslag voor de laag van de wolk. Het verschil bestaat alleen in het lokale niveau binnen het apparaat*.

#### <a name="supported-regions-for-8020"></a>Ondersteunde regio's voor 8020

De opslag van Premium regio's die momenteel worden ondersteund voor 8020 zijn onder de tabelindeling. Deze lijst wordt voortdurend bijgewerkt zodra Premium opslag in meer regio's beschikbaar. 

| S. geen.                                                  | In regio's die momenteel worden ondersteund |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | Centrale VS                     |
| 2                                                       |  Oost-Verenigde Staten                       |
| 3                                                       |  Oost-Verenigde Staten 2                     |
| 4                                                       | West-Verenigde Staten                        |
| 5                                                       | Noord-Europa                   |
| 6                                                       | West-Europa                    |
| 7                                                       | Zuidoost-Azië                 |
| 8                                                       | East Japan                     |
| 9                                                       | West Japan                     |
| 10                                                      | Australië, Oost                 |
| 11                                                      | Zuidoost Australië *           |
| 12                                                      | Oost-Azië *                     |
| 13                                                      | Zuid-Centraal US *              |

* Premium opslag is onlangs gestart in deze geos.

Dit artikel bevat een stapsgewijze procedure voor het implementeren van een virtueel apparaat StorSimple in Azure. U wordt na het lezen van dit artikel:

- Begrijpen hoe het virtuele apparaat verschilt van het fysieke apparaat.

- Mogelijk te maken en het virtuele apparaat te configureren.

- Verbinding maken met het virtuele apparaat.

- Informatie over het werken met het virtuele apparaat.

Deze zelfstudie is van toepassing op alle StorSimple virtuele apparaten met Update 2 en hoger. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Hoe het virtuele apparaat verschilt van het fysieke apparaat

Het virtuele apparaat StorSimple is een alleen-software versie van StorSimple die wordt uitgevoerd op één knooppunt in een Microsoft Azure Virtual Machine. Het virtuele apparaat ondersteunt het herstel na storing scenario's waarin het fysieke apparaat is niet beschikbaar en is geschikt voor gebruik in op itemniveau voor het ophalen van back-ups en noodherstel van lokalen wolk dev en test scenario's.

#### <a name="differences-from-the-physical-device"></a>Verschillen met het fysieke apparaat

De volgende tabel bevat enkele belangrijke verschillen tussen het virtuele apparaat StorSimple en de StorSimple fysiek apparaat.

|                             | Fysiek apparaat                                          | Virtueel apparaat                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Locatie**                    | Bevindt zich in het datacenter.                               | Uitgevoerd in Azure.                                                                            |
| **Netwerkinterfaces**          | Heeft zes netwerkinterfaces: gegevens van 0 tot en met 5 van de gegevens.                  | Heeft slechts één netwerkinterface: 0-gegevens.                                        |
| **Registratie**                | Geregistreerd tijdens de configuratiestap.                | Registratie is een afzonderlijke taak.                                                          |
| **Service gegevens coderingssleutel** | Op het fysieke apparaat genereren en vervolgens het virtuele apparaat bijwerkt met de nieuwe sleutel.           | Kan niet opnieuw worden gegenereerd uit het virtuele apparaat. |


## <a name="prerequisites-for-the-virtual-device"></a>Vereisten voor het virtuele apparaat

In de volgende secties wordt de configuratie vereisten voor het virtuele apparaat StorSimple uitgelegd. Neem de [Beveiligingsoverwegingen voor het gebruik van een virtueel apparaat](storsimple-security.md#storsimple-virtual-device-security)vóór de implementatie van een virtueel apparaat.

#### <a name="azure-requirements"></a>Azure eisen

Voordat u het virtuele apparaat inrichten, moet u de volgende voorbereidingen treffen in uw Azure-omgeving:

- Voor het virtuele apparaat [configureren een virtueel netwerk op Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Als Premium-opslagruimte gebruikt, moet u een virtueel netwerk in een Azure regio waarin de premie opslag maken. Meer informatie over de [regio's die momenteel worden ondersteund voor 8020](#supported-regions-for-8020).
- Het is raadzaam de standaard DNS-server van Azure in plaats van de naam van uw eigen DNS-server gebruiken. Als de naam van uw DNS-server niet geldig is of als de DNS-server kan geen IP-adressen op de juiste wijze, mislukt het maken van het virtuele apparaat.
- Punt-tot-site en site naar site zijn optioneel, maar niet vereist. Als u wilt, kunt u deze opties voor geavanceerde scenario's. 
- [Azure virtuele Machines](../virtual-machines/virtual-machines-linux-about.md) (host-server) kunt u in het virtuele netwerk waarmee de volumes die door het virtuele apparaat kunt maken. Deze servers moeten voldoen aan de volgende eisen voldoen:                            
    - Worden Windows of Linux VMs met iSCSI-initiatorsoftware is geïnstalleerd.
    - Worden uitgevoerd in hetzelfde virtuele netwerk als het virtuele apparaat.
    - Mogelijk verbinding met het iSCSI-doel van het virtuele apparaat via het interne IP-adres van het virtuele apparaat.

- Zorg ervoor dat u de ondersteuning voor iSCSI- en cloud-verkeer op het virtuele netwerk hebt geconfigureerd.


#### <a name="storsimple-requirements"></a>StorSimple eisen

De volgende updates maken met uw service Azure StorSimple voordat u een virtueel apparaat maken:


- [Records uit access control](storsimple-manage-acrs.md) voor het VMs die tot de hostservers voor het virtuele apparaat toevoegen.

- Een [account voor opslag](storsimple-manage-storage-accounts.md#add-a-storage-account) in hetzelfde gebied, als het virtuele apparaat gebruiken. Opslag-accounts in verschillende regio's kunnen leiden tot slechte prestaties. Met het virtuele apparaat kunt u een standaard of opslag Premium-account. Meer informatie over het maken van een [standaardopslag-account]() of een [rekening van de premie opslag](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)

- Een andere opslag gebruiken voor het maken van een virtueel apparaat van die van uw gegevens. Met dezelfde account opslag kan leiden tot slechte prestaties.

Zorg ervoor dat u hebt de volgende informatie voordat u begint:

- Uw Azure klassieke portal account met toegangsreferenties.

- Een kopie van de sleutel data service van het fysieke apparaat.


## <a name="create-and-configure-the-virtual-device"></a>Maak en configureer het virtuele apparaat

Voordat u deze procedures uitvoert, moet u hebben voldaan aan de [vereisten voor het virtuele apparaat](#prerequisites-for-the-virtual-device). 

Nadat u hebt gemaakt van een virtueel netwerk, een StorSimple Manager-service geconfigureerd en uw fysieke StorSimple apparaat geregistreerd met de service, kunt u de volgende stappen uit voor het maken en configureren van een virtueel apparaat StorSimple. 

### <a name="step-1-create-a-virtual-device"></a>Stap 1: Maak een virtueel apparaat

Voer de volgende stappen uit om het virtuele apparaat StorSimple te maken.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Als het maken van het virtuele apparaat niet in deze stap wordt, kan er geen verbinding met het Internet. Ga voor meer informatie problemen met [Internet](#troubleshoot-internet-connectivity-errors) als een virtueel apparaat creatig.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Stap 2: Configureren en het virtuele apparaat registreren

Voordat u deze procedure begint, zorg ervoor dat er een kopie van de sleutel data service. De sleutel data service is gemaakt toen u uw eerste StorSimple apparaat geconfigureerd en u het op te slaan in een beveiligde locatie is gevraagd. Als u een kopie van de sleutel data service niet hebt, moet u contact opnemen met Microsoft Support voor hulp.

Voer de volgende stappen uit als u wilt configureren en de StorSimple virtueel apparaat registreren.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Stap 3: (Optioneel) wijzigen de configuratie-instellingen

De volgende sectie worden de configuratie-instellingen die nodig zijn voor de StorSimple virtueel apparaat als u wilt gebruiken CHAP, StorSimple Snapshot Manager of het apparaat Administrator-wachtwoord wijzigen.

#### <a name="configure-the-chap-initiator"></a>Configureer de CHAP initiator

Deze parameter bevat de referenties die het virtuele apparaat (doel) verwacht van de initiators die probeert toegang te krijgen tot de volumes (servers). De initiators biedt een CHAP-gebruikersnaam en een wachtwoord CHAP zich moeten identificeren met uw apparaat tijdens verificatie. Ga voor gedetailleerde stappen naar [CHAP configureren voor uw apparaat](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Configureer de CHAP target

Deze parameter bevat de referenties die het virtuele apparaat gebruikt als een ingeschakelde CHAP initiator wederzijdse of bi-directionele verificatie aanvraagt. Het virtuele apparaat wordt een omgekeerde CHAP-gebruikersnaam en wachtwoord omgekeerde CHAP gebruikt om zichzelf te identificeren naar de aanvrager tijdens dit verificatieproces. Houd er rekening mee dat CHAP target instellingen algemene instellingen zijn. Wanneer deze worden toegepast, wordt de volumes die zijn aangesloten op de virtuele opslagapparaat CHAP-verificatie gebruikt. Ga voor gedetailleerde stappen naar [CHAP configureren voor uw apparaat](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Wachtwoord StorSimple Snapshot Manager configureren

StorSimple Snapshot Manager software zich bevindt op de Windows-host en beheerders kunnen back-ups maken van uw apparaat StorSimple in de vorm van lokale en momentopnamen van de wolk.

>[AZURE.NOTE] De Windows-host is een Azure virtuele machine voor het virtuele apparaat.

Wanneer u een apparaat in de StorSimple Snapshot Manager configureert, wordt u gevraagd om de StorSimple IP-adres en een wachtwoord opgeven voor de verificatie van uw opslagapparaat. Ga naar [wachtwoord StorSimple Snapshot Manager configureren](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)voor gedetailleerde stappen.

#### <a name="change-the-device-administrator-password"></a>Het apparaat administrator-wachtwoord wijzigen 

Wanneer u de Windows PowerShell-interface voor toegang tot het virtuele apparaat gebruikt, moet u een apparaat administrator-wachtwoord in te voeren. Voor de beveiliging van uw gegevens bent u verplicht om dit wachtwoord te wijzigen voordat het virtuele apparaat kan worden gebruikt. Ga naar de [device administrator-wachtwoord configureren](storsimple-change-passwords.md#change-the-device-administrator-password)voor gedetailleerde stappen.

## <a name="connect-remotely-to-the-virtual-device"></a>Extern verbinding maken met het virtuele apparaat
Externe toegang tot het virtuele apparaat via de Windows PowerShell-interface is niet standaard ingeschakeld. U moet extern beheer op het virtuele apparaat eerst inschakelen en vervolgens op de client waarmee toegang tot het virtuele apparaat in te schakelen.

Het proces om op afstand verbinding te maken, wordt hieronder beschreven.

### <a name="step-1-configure-remote-management"></a>Stap 1: Extern beheer configureren

Voer de volgende stappen voor het configureren van extern beheer voor het virtuele apparaat StorSimple.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Stap 2: Op afstand toegang te krijgen tot het virtuele apparaat

Nadat u extern beheer op de configuratiepagina van StorSimple apparaat hebt ingeschakeld, kunt u Windows PowerShell remoting verbinding maken met het virtuele apparaat vanaf een virtuele machine binnen hetzelfde virtuele netwerk; Zo kunt u in de VM die u geconfigureerd en gebruikt voor het verbinden van iSCSI-host. In de meeste toepassingen zal u al geopend voor toegang tot de host VM die u gebruiken kunt voor toegang tot het virtuele apparaat een openbare-eindpunt.

>[AZURE.WARNING] **Voor een betere beveiliging wordt aangeraden gebruik van HTTPS verbinding met de eindpunten te klikken en vervolgens de eindpunten verwijderen nadat u uw remote PowerShell-sessie hebt voltooid.**

Volg de procedures in [verbinding maken met uw apparaat StorSimple op afstand](storsimple-remote-connect.md) om externe toegang voor het virtuele apparaat in te stellen.

## <a name="connect-directly-to-the-virtual-device"></a>Rechtstreeks verbinding maken met het virtuele apparaat

U kunt ook rechtstreeks met het virtuele apparaat verbinden. Als u wilt een rechtstreekse verbinding met het virtuele apparaat vanaf een andere computer buiten het virtuele netwerk of buiten de Microsoft Azure-omgeving, moet u extra eindpunten maken zoals wordt beschreven in de volgende procedure. 

Voer de volgende stappen uit om een openbare eindpunt op het virtuele apparaat te maken.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Het is raadzaam om verbinding te maken vanaf een andere virtuele machine binnen hetzelfde virtuele netwerk omdat deze oefening wordt het aantal openbare eindpunten in het virtuele netwerk beperkt. Wanneer u deze methode gebruikt, kunt u eenvoudig verbinding maken met de virtuele machine via een extern bureaublad-sessie en vervolgens configureren die virtuele machine voor gebruik als u een andere Windows-client in een lokaal netwerk. U hoeft niet het openbare poortnummer toevoegen omdat de poort al bekend zijn.

## <a name="work-with-the-storsimple-virtual-device"></a>Werken met het virtuele apparaat StorSimple

Nu u hebt gemaakt en de StorSimple virtueel apparaat geconfigureerd, bent u klaar om ermee te werken. U kunt werken met volume, containers, volumes en back-beleid op een virtueel apparaat net zoals op een fysieke StorSimple apparaat; het enige verschil is dat u nodig hebt om ervoor te zorgen dat het virtuele apparaat te uit de lijst met apparaten selecteren. Raadpleeg [de StorSimple Manager-service voor het beheren van een virtueel apparaat gebruiken](storsimple-manager-service-administration.md) voor stapsgewijze procedures van de verschillende beheertaken voor het virtuele apparaat.

De volgende secties worden enkele van de verschillen die u tegenkomen zult bij het werken met het virtuele apparaat.

### <a name="maintain-a-storsimple-virtual-device"></a>Een virtueel apparaat StorSimple onderhouden

Omdat het een apparaat alleen software, is onderhoud voor het virtuele apparaat minimaal in vergelijking met het onderhoud van het fysieke apparaat. U hebt de volgende opties:

- **Software-updates** : U kunt de datum weergeven waarop de software voor het laatst werd bijgewerkt, werken samen met de statusberichten. U kunt de knop **Scan updates** onder aan de pagina een handmatige scan uitvoeren als u wilt controleren op nieuwe updates. Als er updates beschikbaar zijn, klikt u op **Updates installeren** om de installatie. Omdat er slechts één interface op het virtuele apparaat, houdt dit in dat er een lichte service wordt onderbroken wanneer updates worden toegepast. Het virtuele apparaat zal afsluiten en opnieuw opstarten (indien nodig) toe te passen van de updates die zijn uitgebracht. Ga voor een stapsgewijze procedure voor het [bijwerken van het apparaat](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Pakket ondersteuning** – u kunt maken en een ondersteuningspakket om Microsoft Support voor het oplossen van problemen met het virtuele apparaat te uploaden. Ga voor een stapsgewijze procedure [maken](storsimple-create-manage-support-package.md)en beheren van een support-pakket.

### <a name="storage-accounts-for-a-virtual-device"></a>Accounts voor een apparaat voor virtuele opslag

Opslag-accounts zijn gemaakt voor gebruik door de StorSimple Manager-service, het virtuele apparaat en het fysieke apparaat. Wanneer u uw accounts opslag maakt, wordt u aangeraden een regio-id te gebruiken in de beschrijvende naam om ervoor te zorgen dat de regio alle systeemonderdelen consistent is. Voor een virtueel apparaat is het van belang dat alle onderdelen in hetzelfde gebied, om te voorkomen dat problemen met de prestaties.

Ga naar [een opslag toevoegen](storsimple-manage-storage-accounts.md#add-a-storage-account)voor een stapsgewijze procedure.

### <a name="deactivate-a-storsimple-virtual-device"></a>Een virtueel apparaat StorSimple deactiveren

Het deactiveren van een virtueel apparaat verwijdert de VM en de bronnen die zijn gemaakt toen het werd ingericht. Nadat het virtuele apparaat is gedeactiveerd, kan deze niet worden hersteld naar de vorige staat. Voordat u het virtuele apparaat deactiveren, moet u clients en hosts die afhankelijk van het zijn verwijderen of stoppen.

Het deactiveren van een virtueel apparaat resulteert in de volgende acties:

- Het virtuele apparaat wordt verwijderd.

- De OS schijf en gegevensschijven voor het virtuele apparaat gemaakt worden verwijderd.

- Het virtuele netwerk gemaakt tijdens het inrichten en gehoste service blijven behouden. Als u deze niet gebruikt, moet u deze handmatig verwijderen.

- Wolk momentopnamen gemaakt voor het virtuele apparaat blijven behouden.

Ga naar voor een stapsgewijze procedure [uitschakelen en verwijderen van het apparaat StorSimple](storsimple-deactivate-and-delete-device.md).

Als het virtuele apparaat wordt weergegeven als op de pagina StorSimple Manager-service is uitgeschakeld, kunt u het virtuele apparaat verwijderen uit de lijst met apparaten op de pagina **apparaten** .


### <a name="start-stop-and-restart-a-virtual-device"></a>Starten, stoppen en opnieuw starten van een virtueel apparaat
In tegenstelling tot het fysieke apparaat StorSimple is er geen stroom in- of uit-knop drukken op een virtueel apparaat StorSimple vermogen. Echter kan er gelegenheden waar u wilt stoppen en opnieuw starten van het virtuele apparaat. Sommige updates moet bijvoorbeeld dat de VM opnieuw worden opgestart om de update te voltooien. De eenvoudigste manier voor u om te starten, stoppen en opnieuw starten van een virtueel apparaat is het gebruik van de Console van het beheer van virtuele Machines.

Als u de beheerconsole bekijkt, is de status van het virtuele apparaat **uitgevoerd** omdat deze standaard wordt gestart nadat deze is gemaakt. U kunt starten, stoppen en opnieuw starten van een virtuele machine op elk gewenst moment.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Fabrieksinstellingen herstellen

Als u besluit dat u wilt beginnen met het virtuele apparaat, te deactiveren en deze verwijderen en een nieuwe maakt. Net als bij het fysieke apparaat opnieuw ingesteld, het nieuwe virtuele apparaat geen updates geïnstalleerd; Zorg er daarom voor om te controleren op updates voordat u deze gebruikt.


## <a name="fail-over-to-the-virtual-device"></a>Failover uitvoeren naar de virtueel apparaat

Disaster recovery (DR) is een van de belangrijke scenario's die voor het virtuele apparaat StorSimple is ontworpen. In dit scenario wordt de fysieke StorSimple apparaat of gehele datacenter mogelijk niet beschikbaar. Gelukkig kunt u een virtueel apparaat terugzetten op een alternatieve locatie. Tijdens de DR, containers die het volume van het bronapparaat eigendom wijzigen en overgebracht naar het virtuele apparaat. De vereisten voor de DR zijn dat het virtuele apparaat zijn gemaakt en geconfigureerd, de volumes in de container volume off line zijn genomen en de container volume een bijbehorende heeft wolk momentopname.

>[AZURE.NOTE] 
>
> - Wanneer u een virtueel apparaat als het secundaire apparaat voor DR, houd er rekening mee dat de 8010 30 TB opslagruimte norm 8020 heeft en 64 TB opslagruimte Premium. De hogere capaciteit 8020 virtueel apparaat mogelijk geschikter voor een DR scenario.
> - U kunt geen failover of kloon van een apparaat met Update 2 naar een apparaat 1 vóór Update-software wordt uitgevoerd. U kunt echter niet over een apparaat met 2 Update naar een apparaat met Update 1 (punten 1.1 en 1.2)

Ga naar [een virtueel apparaat](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device)voor een stapsgewijze procedure.
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Afsluiten of het virtuele apparaat verwijderen

Als u eerder hebt geconfigureerd en gebruikt een StorSimple virtueel apparaat maar nu niet meer wilt toerekenen van kosten berekenen voor het gebruik ervan, kunt u het virtuele apparaat uitschakelen. Het virtuele apparaat afgesloten wordt niet verwijderd besturingssysteem of de gegevensschijven in de opslag. Deze kosten toerekenen op uw abonnement stopt, maar opslag toeslagen voor het OS en data schijven wordt voortgezet.

Als u verwijderen of het virtuele apparaat afgesloten, er **offline** op de pagina apparaten van de StorSimple Manager-service. U kunt het apparaat verwijderen als u wilt ook de back-ups gemaakt door het virtuele apparaat verwijderen of deactiveren. Zie voor meer informatie, [uitschakelen en verwijderen van een apparaat StorSimple](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Internet connectivity fouten oplossen 

Tijdens het maken van een virtueel apparaat als er geen verbinding met het Internet, is mislukt de stap maken. Problemen oplossen als de storing door Internet-verbinding is, moet u de volgende stappen uitvoeren in de klassieke Azure portal:

1. Een Windows server 2012 virtuele machine maken in Azure. Deze virtuele machine moet dezelfde account voor opslag, VNet en subnet zoals gebruikt door het virtuele apparaat te gebruiken. Als u al een bestaande Windows Server-host in Azure met dezelfde account voor opslag, Vnet en subnet, kun je het oplossen van problemen met de internetverbinding.
2. Extern aanmelden in de virtuele machine in de vorige stap hebt gemaakt. 
3. Open een opdrachtvenster binnen de virtuele machine (Win + R en typ vervolgens `cmd`).
4. Run-de volgende opdracht bij de opdrachtprompt.

    `nslookup windows.net`

5. Als `nslookup` mislukt, en vervolgens Internet-verbinding mislukt verhindert het virtuele apparaat om de StorSimple Manager-service te registreren. 
6. Breng de gewenste wijzigingen aan het virtuele netwerk om ervoor te zorgen dat het virtuele apparaat toegang kunnen krijgen tot Azure sites zoals 'windows.net' is.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van een virtueel apparaat](storsimple-manager-service-administration.md).
 
- Begrijpen hoe [een StorSimple-volume vanaf een back-upset](storsimple-restore-from-backup-set.md)herstellen. 

