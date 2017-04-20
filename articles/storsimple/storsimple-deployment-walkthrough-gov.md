<properties 
   pageTitle="Apparaat in de Portal regering StorSimple implementeren | Microsoft Azure"
   description="Beschrijving van de stappen en aanbevolen procedures voor het implementeren van de StorSimple Update 1-apparaat en de service in de portal Azure regering."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/17/2016"
   ms.author="v-sharos" />

# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal"></a>Implementatie van uw apparaat op gebouwen StorSimple in de regering Portal

[AZURE.INCLUDE [storsimple-version-selector-deploy-gov](../../includes/storsimple-version-selector-deploy-gov.md)]

## <a name="overview"></a>Overzicht

Welkom bij de implementatie van Microsoft Azure StorSimple apparaat. Deze distributie, zelfstudies van toepassing op de software-Update 1 uitgevoerd in de Portal Azure regering StorSimple 8000-serie. Deze reeks cursussen wordt beschreven hoe u het apparaat StorSimple en bevat een Configuratiecontrolelijst, configuratie vereisten en gedetailleerde configuratie stappen.

De informatie in deze zelfstudie wordt ervan uitgegaan dat u hebt gecontroleerd de voorzorgsmaatregelen en uitgepakt, racks en uw apparaat StorSimple bekabelde. Als u nog steeds moet u deze taken uitvoert, kunt u beginnen met het controleren van de [veiligheidsmaatregelen](storsimple-safety.md). Afhankelijk van het model van uw apparaat u vervolgens kunt uitpakken, rack mount en de kabel door de instructies te volgen in:

- [Uitpakken, rack-mount en uw 8100-kabel](storsimple-8100-hardware-installation.md)
- [Uitpakken, rack-mount en uw 8600 kabel](storsimple-8600-hardware-installation.md)

U moet administrator-bevoegdheden voor de installatie en configuratie voltooid. Wij raden u aan de configuratiecontrolelijst voor de te controleren voordat u begint. De implementatie en configuratie kan enige tijd duren.

> [AZURE.NOTE] De StorSimple distributie-informatie gepubliceerd op de website van Microsoft Azure is van toepassing op StorSimple 8000-serie alleen apparaten. Voor volledige informatie over de apparaten uit de 7000-serie, gaat u naar: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Zie de [StorSimple systeem introductiehandleiding](http://onlinehelp.storsimple.com/111_Appliance/)voor 7000-serie informatie over implementatie.

## <a name="deployment-steps"></a>Implementatiestappen

Deze vereiste stappen voor het configureren van het apparaat StorSimple en verbind deze met de StorSimple Manager-service uitvoeren. Naast de vereiste stappen zijn optionele stappen en procedures u tijdens de installatie moet. De stapsgewijze instructies geven wanneer u deze optionele stappen moet uitvoeren.


| Stap                                                                                   | Beschrijving                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **VEREISTEN**                                                                      | Deze moeten worden uitgevoerd ter voorbereiding van de toekomstige implementatie.                                                                                        |
| Implementatie, Configuratiecontrolelijst.                                                     | Gebruik deze controlelijst voor het verzamelen en vastleggen van informatie voorafgaand aan en tijdens de implementatie.                                                                       |
| Vereisten voor implementatie.                                                               | Deze valideren dat de omgeving gereed is voor implementatie.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **STAPSGEWIJZE IMPLEMENTATIE**                                                                   | Deze stappen zijn vereist voor de implementatie van uw apparaat StorSimple in productie.                                                                                      |
| Stap 1: Een nieuwe service maken.                                                         | Cloud management en opslag voor uw apparaat StorSimple instellen. Deze stap overslaan als u een bestaande service voor andere apparaten StorSimple.                |
| Stap 2: De registratie sleutel ophalen.                                               | Met deze sleutel kunt registreren & StorSimple aansluit met de management-service.                                                                         |
| Stap 3: Configureer en het apparaat via Windows PowerShell voor StorSimple registreren.    | Het apparaat aansluiten op uw netwerk en het registreren met Azure om het gebruik van de service management setup te voltooien.                                            |
| Stap 4: Vul minimaal apparaat instellen</br>Optioneel: Het apparaat StorSimple bijwerken.      | De management-service gebruiken om de instellingen van het apparaat en in te schakelen voor opslag.                                                                      |
| Stap 5: Een container voor volume maken.                                                      | Een container voor bepaling volumes maken. Een container volume heeft een rekening voor opslag, bandbreedte en coderingsinstellingen voor alle volumes die dit.    |
| Stap 6: Een volume maken.                                                                | Bepaling opslag volume (s) op het apparaat StorSimple voor uw servers.                                                                                        |
| Stap 7: Koppel, initialiseren en formatteren van een volume.</br>Optioneel: Configureer MPIO.            | Verbinding maken met uw servers op de iSCSI-opslag die door het apparaat. Configureer eventueel MPIO om ervoor te zorgen dat uw servers koppeling, het netwerk en een fout tolereren kunnen.                                                                                                                                                              |
| Stap 8: Nemen een back-up.                                                                  | Uw back-beleid instellen om uw gegevens te beschermen                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **ANDERE PROCEDURES**                                                                   | Wellicht moet u verwijzen naar de volgende procedures als u uw oplossing implementeert.                                                                                        |
| Een nieuwe opslag-account voor de service configureren.                                      |                                                                                                                                                               |
| Met stopverf verbinding maken met de seriële console apparaat.                                    |                                                                                                                                                               |
| Voor scannen en updates toe te passen.                                                   |                                                                                                                                                               |
| De IQN van een Windows Server-host ophalen.                                                   |                                                                                                                                                               |
| Een handmatige back-up maken.                                                                 | 
| MPIO configureren.                                                                          |


## <a name="deployment-configuration-checklist"></a>Implementatie, controlelijst

De volgende controlelijst voor implementatie configuratie beschrijft de informatie die u verzamelen moet en voordat u de software op uw apparaat StorSimple configureren. Sommige van deze informatie tijdig voorbereiden helpen stroomlijnen van het apparaat StorSimple in uw omgeving implementeren. Met deze controlelijst kunt u ook de configuratiegegevens noteren als u het apparaat implementeren.

| Werkgebied                                  | Parameter                                         | Details                                                                                                                                                                | Waarden |
|----------------------------------------|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| **Kabel van het apparaat**                      | Seriële toegang                                     | Eerste configuratie                                                                  | Ja/Nee |
|   |   |  |  |
| **Configureren en registreren van apparaat**          | Netwerkinstellingen Data 0                           | 0 gegevens IP-adres:</br>Subnetmasker:</br>Gateway:</br>Primaire DNS-server:</br>Primaire NTP-server:</br>Web proxyserver IP/FQDN-naam (optioneel):</br>Web proxypoort:|        |
|                                        | Apparaat administrator-wachtwoord                     | Wachtwoord moet tussen 8 en 15 tekens met kleine letters, hoofdletters, numerieke en speciale tekens. |        |
|                                        | StorSimple Snapshot Manager-wachtwoord              | Wachtwoord moet 14 of 15 tekens met kleine letters, hoofdletters, numerieke en speciale tekens.|        |
|                                        | Sleutel voor de inschrijving                          | Deze sleutel wordt gegenereerd vanuit de portal Azure.    |        |
|                                        | Service gegevens coderingssleutel                       | Deze sleutel wordt gemaakt wanneer het apparaat wordt geregistreerd bij het management-service via de Windows PowerShell voor StorSimple. Deze sleutel kopiëren en opslaan op een veilige plaats.|  |
|   |   |  |  |
| **Complete inrichting voor minimale installatie**          | Beschrijvende naam voor uw apparaat                     | Dit is een beschrijvende naam voor het apparaat. |        |
|                                        | TimeZone                                          | Het apparaat wordt deze tijdzone gebruiken voor alle geplande activiteiten.  |        |
|                                        | Secundaire DNS-server                              | Dit is een vereiste configuratie.                                  |        |
|                                        | Netwerkinterface: 0 verantwoordelijke vaste IP-adressen                                     | Van deze IP moet worden omgeleid naar het Internet.</br>Controller 0 vaste IP adres:</br>Controller 1 vaste IP adres:|
|   |   |  |  |
| **Extra network interface-instellingen**  | Netwerk-interface: 1 gegevens</br>Als iSCSI ingeschakeld, configureert u de Gateway niet.      | Doel: ISCSI-wolk/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|                                        | Netwerkinterface: gegevens 2</br>Als iSCSI ingeschakeld, configureert u de Gateway niet.      | Doel: ISCSI-wolk/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|                                        | Netwerk-interface: 3-gegevens</br>Als iSCSI ingeschakeld, configureert u de Gateway niet.      | Doel: ISCSI-wolk/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|                                        | Netwerkinterface: 4 gegevens</br>Als iSCSI ingeschakeld, configureert u de Gateway niet.      | Doel: ISCSI-wolk/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|                                        | Netwerk-interface: 5-gegevens</br>Als iSCSI ingeschakeld, configureert u de Gateway niet.      | Doel: ISCSI-wolk/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|   |   |  |  |
| **Een container volume maken**                      | Volumenaam container:                            | Naam van de container                                                                                                                                                 |        |
|                                        | Azure opslag account:                            | Opslag account naam & sneltoets aan deze container volume koppelen                                                                                              |        |
|                                        | Coderingssleutel cloud-opslag:                     | Coderingssleutel voor opslag in elke container                                                                                                                           |        |
|   |   |  |  |
| **Een volume maken**                        | Details voor elk volume                           | De naam van het volume:                                                                                                                                                           |        |
|                                        |                                                   | Grootte:                                                                                                                                                                  |        |
|                                        |                                                   | Soort gebruik:                                                                                                                                                            |        |
|                                        |                                                   | ACR naam:                                                                                                                                                              |        |
|                                        |                                                   | Standaard back-beleid:                                                                                                                                                 |        |
|   |   |  |  |
| **Koppelen, initialiseren en formatteren van een volume** | Details voor elke host-server verbinding te maken met de opslag | Naam van de Windows Server:                                                                                                                                                   |        |
|                                        |                                                   | Windows Server IQN:                                                                                                                                                    |        |
|                                        |                                                   | Volumenaam Windows Server:                                                                                                                                                   |        |
|                                        |                                                   | NTFS-koppelpunten punt/stationsletter:                                                                                                                                      |        |


## <a name="deployment-prerequisites"></a>Vereisten voor implementatie

In de volgende gedeelten leggen de vereisten van de configuratie voor uw StorSimple Manager-service en de StorSimple-apparaat.

### <a name="for-the-storsimple-manager-service"></a>Voor de StorSimple Manager-service

Voordat u begint, zorg ervoor dat:

- U hebt uw Microsoft-account met toegangsreferenties.

- U hebt uw account Microsoft Azure opslag met toegangsreferenties.

- Uw abonnement op Microsoft Azure is voor de StorSimple Manager-service ingeschakeld. Het abonnement moet worden aangeschaft via de [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).

- U hebt toegang tot de terminal-emulatiesoftware zoals stopverf.

### <a name="for-the-device-in-the-datacenter"></a>Voor het apparaat in het datacenter

Voordat u het apparaat configureert, zorg ervoor dat:

- Het apparaat is volledig hebt uitgepakt, gemonteerd in een rek en volledig bekabelde voor energie, seriële en netwerktoegang, zoals beschreven in:

    -  [Uitpakken, rack-mount en het apparaat 8100-kabel](storsimple-8100-hardware-installation.md)
    -  [Uitpakken, rack-mount en het apparaat 8600 kabel](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter

Voordat u begint, zorg ervoor dat:

- De poorten in de firewall van uw datacenter worden geopend als u wilt toestaan voor iSCSI- en cloud verkeer zoals beschreven in [vereisten voor het apparaat StorSimple toegang](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

## <a name="step-by-step-deployment"></a>Stapsgewijze implementatie

Gebruik de volgende stapsgewijze instructies voor de implementatie van uw apparaat StorSimple in het datacenter.

## <a name="step-1-create-a-new-service"></a>Stap 1: Een nieuwe service maken

Een StorSimple Manager-service kan meerdere StorSimple apparaten beheren. Voer de volgende stappen uit om een nieuw exemplaar van de service Manager StorSimple maken.

[AZURE.INCLUDE [storsimple-create-new-service-gov](../../includes/storsimple-create-new-service-gov.md)]

> [AZURE.IMPORTANT] Als u het automatisch maken van een account voor de opslag niet met uw service heeft ingeschakeld, moet u ten minste één opslag-account maken nadat u een service hebt gemaakt. Deze account opslag wordt gebruikt bij het maken van een volume-container. 
>
> * Als u een opslag-account niet automatisch gemaakt hebt, gaat u naar [een nieuwe opslag-account voor de service configureren](#configure-a-new-storage-account-for-the-service) voor gedetailleerde instructies. 
> * Als u het automatisch maken van een account voor opslag hebt ingeschakeld, gaat u naar [stap 2: de registratie sleutel ophalen](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Stap 2: De registratie sleutel ophalen

Nadat de StorSimple Manager-service actief is, moet u de registratie sleutel ophalen. Deze sleutel wordt gebruikt om te registreren en de StorSimple apparaat verbinding met de service.

De volgende stappen uitvoeren in de Portal van de overheid.

[AZURE.INCLUDE [storsimple-get-service-registration-key-gov](../../includes/storsimple-get-service-registration-key-gov.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Stap 3: Configureren en het apparaat via Windows PowerShell voor StorSimple registreren

Gebruik Windows PowerShell voor StorSimple voor het voltooien van de oorspronkelijke installatie van het apparaat StorSimple zoals beschreven in de volgende procedure. U moet de terminal-emulatiesoftware gebruiken om deze stap te voltooien. Voor meer informatie, Zie [Gebruik stopverf verbinding maken met de seriële console apparaat](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-gov](../../includes/storsimple-configure-and-register-device-gov.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Stap 4: Vul minimaal apparaat instellen

Voor de minimale configuratie van uw apparaat StorSimple bent u verplicht om: 

- De secundaire DNS-server instellen.
- ISCSI op ten minste één netwerkinterface inschakelen.
- Vaste IP-adressen toewijzen aan zowel de domeincontrollers.

De volgende stappen uitvoeren in de Portal van de overheid de minimaal vereiste apparaatstuurprogramma installatie te voltooien.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Stap 5: Een container voor volume maken

Een container volume heeft een rekening voor opslag, bandbreedte en coderingsinstellingen voor alle volumes die dit. U moet een container volume maken voordat u kunt beginnen met het inrichten van de volumes op uw apparaat StorSimple. 

De volgende stappen uitvoeren in de Portal van de overheid voor het maken van een volume-container.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Stap 6: Een volume maken

Nadat u een container volume hebt gemaakt, kunt u een opslagvolume op het apparaat StorSimple inrichten voor uw servers. De volgende stappen uitvoeren in de Portal van de overheid om een volume te maken.

> [AZURE.IMPORTANT] Azure StorSimple kan alleen dun ingerichte volumes maken.  U kunt volledig ingerichte of gedeeltelijk ingerichte maken volumes op een Azure StorSimple systeem. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Stap 7: Koppel, initialiseren en formatteren van een volume

Deze stappen uitvoeren op de Windows Server-host.

> [AZURE.IMPORTANT]

> - De beschikbaarheid van uw oplossing StorSimple aangeraden u MPIO configureren op de hostservers (optioneel) voor iSCSI-configureren. MPIO-configuratie op de host-servers zal ervoor zorgen dat de servers mogelijk zonder een koppeling, een netwerk of een storing in de netwerkinterface.

> - MPIO- en iSCSI-installatie en configuratie op Windows Server host, Zie [MPIO configureren voor uw apparaat StorSimple](storsimple-configure-mpio-windows-server.md). Ook wordt hierbij de stappen om te koppelen, initialiseren en StorSimple volumes formatteren.

> - Ga naar [MPIO configureren voor uw host StorSimple Linux](storsimple-configure-mpio-on-linux.md) voor MPIO- en iSCSI-installatie en configuratie-instructies op een Linux-host,

Als u niet op MPIO configureren, voeren de volgende stappen uit als u wilt koppelen, initialiseren en formatteren uw StorSimple volumes op een Windows Server-host.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Stap 8: Een back-up maken

Back-ups een point-in-time bescherming van volumes en herstel verbeteren terwijl u hersteltijden tot een minimum beperken. U kunt twee typen back-ups uitvoeren op uw apparaat StorSimple: lokale momentopnamen en momentopnamen van de wolk. Elk van deze back-uptypen zijn **gepland** of **handmatig**. 

De volgende stappen uitvoeren in de Portal van de overheid voor het maken van een geplande back-up.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

U kunt een handmatige back-up nemen op elk gewenst moment. Ga naar [een handmatige back-up maken](#create-a-manual-backup)voor instructies. 

## <a name="configure-a-new-storage-account-for-the-service"></a>Een nieuwe opslag-account voor de service configureren

Dit is een optionele stap die u alleen uitvoeren moet als u het automatisch maken van een opslag-account met uw service niet heeft ingeschakeld. Een account van Microsoft Azure opslag is vereist voor het maken van een volume StorSimple container.

Als u een account Azure opslag maken in een andere regio, Zie [Azure opslag rekeningen](../storage/storage-create-storage-account.md) voor stapsgewijze instructies.

De volgende stappen uitvoeren in de Portal overheid op de pagina **StorSimple Manager-service** .

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Verbinding maken met de seriële console apparaat via stopverf

Verbinding van Windows PowerShell voor StorSimple, moet u terminal-emulatiesoftware zoals PuTTY gebruiken. Stopverf kunt u gebruiken wanneer u het apparaat via de seriële console of door een Telnet-sessie openen vanaf een externe computer.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Zoeken en toepassen van updates

Bijwerken van uw apparaat kan enkele uren duren. De volgende stappen uitvoeren om te zoeken en het toepassen van updates op uw apparaat.

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Uw apparaat bijwerken

1.  Klik op de pagina apparaat **Quick Start** **apparaten**. Selecteer het fysieke apparaat, klik op **onderhoud** en klik vervolgens op **Updates scannen**.  
2.  Een taak voor het zoeken naar beschikbare updates wordt gemaakt. Als er updates beschikbaar zijn, verandert het **Scannen van Updates** **Updates installeren**. Klik op **Updates installeren**. 
3.  Een bijwerktaak wordt gemaakt. De status van de update door te navigeren naar **taken**controleren.

    > [AZURE.NOTE] Wanneer de taak update wordt gestart, worden direct de status als 50 procent weergegeven. Alleen nadat de update voltooid is, verandert de status tot 100 procent. Er is geen real-time status voor het updateproces.

4.  Nadat het apparaat is bijgewerkt, gegevens 2 en 3 gegevens netwerkinterfacekaarten inschakelen als deze zijn uitgeschakeld.

## <a name="get-the-iqn-of-a-windows-server-host"></a>De IQN van een Windows Server-host ophalen

Doe het volgende als u de iSCSI Qualified Name (IQN) van een Windows-host met Windows Server® 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Een handmatige back-up maken

De volgende stappen uitvoeren in de Portal van de overheid voor het maken van een handmatige reservekopie op verzoek voor één volume op het apparaat StorSimple.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup-gov.md)]

## <a name="configure-mpio"></a>MPIO configureren

MPIO (Multipath I/O) is een optionele functie en wordt niet standaard geïnstalleerd op Windows Server. Deze moet worden geïnstalleerd als een functie via Serverbeheer. Ga naar [MPIO configureren voor uw apparaat StorSimple](storsimple-configure-mpio-windows-server.md)voor MPIO installatie-instructies.

Ga naar [MPIO configureren voor uw Linux-host](storsimple-configure-mpio-on-linux.md)voor MPIO installatie-instructies voor een verbinding met een host Linux StorSimple-apparaat.

> [AZURE.NOTE] MPIO wordt niet ondersteund op een virtueel apparaat StorSimple. 

## <a name="next-steps"></a>Volgende stappen

- Een [virtueel apparaat](storsimple-virtual-device-u2.md)configureren.

- De [StorSimple Manager-service](https://msdn.microsoft.com/library/azure/dn772396.aspx) gebruiken voor het beheren van uw apparaat StorSimple.
 
