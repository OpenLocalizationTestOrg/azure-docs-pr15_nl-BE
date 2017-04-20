<properties
   pageTitle="Virtuele matrix StorSimple systeemvereisten"
   description="Meer informatie over de software- en netwerkvereisten voor uw virtuele matrix StorSimple"
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
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>Virtuele matrix StorSimple systeemvereisten

## <a name="overview"></a>Overzicht

Dit artikel wordt beschreven dat belangrijke vereisten voor uw Microsoft Azure StorSimple Virtual Array (ook bekend als StorSimple op-premises virtuele apparaatstuurprogramma of virtueel apparaat StorSimple) en de opslag-clients toegang hebben tot de matrix. Wij raden aan dat u lees de informatie zorgvuldig door voordat u uw systeem StorSimple te implementeren en vervolgens terug naar deze indien nodig tijdens de implementatie en verdere bewerking verwijzen.

De systeemvereisten zijn:

-   **Softwarevereisten voor clients opslag** - beschrijving van de ondersteunde virtualisatie-platforms, webbrowsers, iSCSI-initiators, SMB-clients, virtueel apparaat minimale eisen en eventuele aanvullende voorschriften voor deze besturingssystemen.

-   **Vereisten voor het apparaat StorSimple networking** - geeft informatie over de poorten die zijn geopend in de firewall moeten voor iSCSI, cloud of beheer van verkeer.

StorSimple systeem informatie over de vereisten, gepubliceerd in dit artikel alleen van toepassing op virtuele StorSimple-Arrays.

- Ga naar [systeemvereisten voor uw apparaat StorSimple 8000-serie](storsimple-system-requirements.md)voor 8000-serie-apparaten.
 
- Ga naar [systeemvereisten voor uw apparaat StorSimple 5000-7000 series](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements)7000 serie apparaten.


## <a name="software-requirements"></a>Softwarevereisten

De softwarevereisten bevat de informatie over de ondersteunde webbrowsers, SMB-versies, platforms voor virtualisatie en de vereisten voor de minimale virtueel apparaat.

### <a name="supported-virtualization-platforms"></a>Virtualisatie ondersteunde platforms


| **Hypervisor** | **Versie**                          |
|----------------|--------------------------------------|
| Hyper-V        | Windows Server 2008 R2 SP1 en hoger |
| VMware ESXi    | 5.5 en hoger                        |

### <a name="virtual-device-requirements"></a>Vereisten voor virtuele apparaten

| **Onderdeel**                                | **Vereiste**            |
|----------------------------------------------|----------------------------|
| Minimum aantal virtuele processors (cores) | 4                          |
| Minimale geheugen (RAM)                         | 8 GB                       |
| Schijfruimte<sup>1</sup>                       | OS schijf - 80 GB <br></br>Gegevensschijf - 500 GB tot 8 TB|
| Minimum aantal netwerk-interface (s)       | 1                          |
| Minimale Internet bandbreedte<sup>2</sup>       | 5 Mbps                     |

<sup>1</sup> - dun ingericht.

<sup>2</sup> - netwerkvereisten kunnen variëren afhankelijk van de snelheid van het dagelijkse gegevens wijzigen. Bijvoorbeeld als een apparaat een back-up van 10 GB of meer wijzigingen gedurende de dag een moet, kan vervolgens de dagelijkse back-up via een 5 Mbps-verbinding krijgen tot 4,25 uur (als de gegevens kan niet worden gecomprimeerd of deduplicated).

### <a name="supported-web-browsers"></a>Ondersteunde webbrowsers

| **Onderdeel**     | **Versie** | **Aanvullende eisen en/of notities** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Nieuwste versie  |                                   |
| Internet Explorer | Nieuwste versie  | Getest met Internet Explorer 11  |
| Google Chrome     | Nieuwste versie  | Getest met 46 chroom             |

### <a name="supported-storage-clients"></a>Opslag van ondersteunde clients 

De volgende softwarevereisten zijn voor de iSCSI-initiators die toegang uw virtuele StorSimple-matrix tot (geconfigureerd als een iSCSI-server).

| **Ondersteunde besturingssystemen** | **Vereiste versie** | **Aanvullende eisen en/of notities** |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012, 2012R2 |StorSimple kunt maken, dun ingerichte en volledig ingerichte volumes. Er kan geen gedeeltelijk ingerichte volumes maken. StorSimple iSCSI-volumes worden alleen ondersteund voor: <ul><li>Eenvoudige volumes op standaardschijven voor Windows.</li><li>Windows NTFS voor het formatteren van een volume.</li>|


De volgende software-eisen zijn voor de SMB-clients die toegang hebben tot uw StorSimple Virtual Array (geconfigureerd als bestandsserver).

| **SMB-versie** |
|-------------|
| SMB 2.x     |
| SMB 3.0     |
| SMB 3.02    |

> [AZURE.IMPORTANT] Niet kopiëren of opslaan van bestanden die zijn beveiligd door Windows Encrypting File System (EFS) op de bestandsserver StorSimple virtuele matrix; Dit zal resulteren in een niet-ondersteunde configuratie. 

## <a name="networking-requirements"></a>Netwerk vereisten 

De volgende tabel worden de poorten die worden geopend in de firewall moeten voor iSCSI, SMB, cloud of beheer van verkeer. In deze tabel verwijst *in* of *inkomende* naar de richting waarin binnenkomende clientaanvragen toegang het apparaat tot. *Out* of *uitgaande* verwijst naar de richting waarin het apparaat StorSimple gegevens extern, na de implementatie verzendt: bijvoorbeeld, uitgaande met het Internet.

| **Poort nr.<sup>1</sup>** | **In- of uitzoomen** | **Bereik van poort** | **Vereist**              | **Notities**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80 (HTTP)            | Out           | WAN            | Nee                        | Uitgaande poort wordt gebruikt voor toegang tot het Internet voor het ophalen van updates. <br></br>De uitgaande webproxy is een gebruiker worden geconfigureerd. |
| TCP 443 (HTTPS)          | Out           | WAN            | Ja                       | Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud. <br></br>De uitgaande webproxy is een gebruiker worden geconfigureerd. |
| UDP 53 (DNS)             | Out           | WAN            | In sommige gevallen; Zie ' Opmerkingen '. | Deze poort is alleen vereist als u een Internet-based DNS-server gebruikt. <br></br> **Opmerking**: als een bestandsserver implementeert, raden we aan met behulp van lokale DNS-server.|
| UDP 123 (NTP)            | Out           | WAN            | In sommige gevallen; Zie ' Opmerkingen '. | Deze poort is alleen vereist als u een Internet-based NTP-server gebruikt.<br></br> **Opmerking:** Als een bestandsserver implementeert, raden we u aan tijd synchroniseren met uw Active Directory-domeincontrollers.  |
| TCP 80 (HTTP)           | In            | LAN            | Ja                       | Dit is de binnenkomende poort voor lokale UI op het apparaat StorSimple voor lokaal beheer. <br></br> **Opmerking**: toegang tot de lokale gebruikersinterface via HTTP worden automatisch omgeleid naar HTTPS.|
| TCP 443 (HTTPS)          | In            | LAN            | Ja                       | Dit is de binnenkomende poort voor lokale UI op het apparaat StorSimple voor lokaal beheer.|
| TCP-3260 (iSCSI)         | In            | LAN            | Nee                        | Deze poort wordt gebruikt voor toegang tot gegevens via iSCSI.|

<sup>1</sup> geen inkomende poorten moeten worden geopend op het openbare Internet.

> [AZURE.IMPORTANT] Zorg ervoor dat de firewall niet wijzigen of de SSL-verkeer tussen de StorSimple apparaat en Azure decoderen.


### <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor de firewall-regels 

Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van het inkomend en uitgaand verkeer configureren. Uw virtuele matrix en de StorSimple Manager-service zijn afhankelijk van andere Microsoft-toepassingen zoals Azure Service Bus, Azure Active Directory-toegangsbeheer opslag rekeningen en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt voor het configureren van de firewall-regels. Het is belangrijk te begrijpen dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Dit vereist op zijn beurt de netwerkbeheerder om te controleren en bijwerken van firewall-regels voor de StorSimple als en wanneer dat nodig is. 

Het is raadzaam dat u uw firewall-regels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen StorSimple ingesteld. Echter, kunt u onderstaande informatie over de geavanceerde firewall-regels die nodig zijn voor het maken van beveiligde omgevingen in te stellen.

> [AZURE.NOTE] 
> 
> - Het apparaat (bron) IP-adressen moet altijd worden ingesteld op alle interfaces van de cloud-netwerk. 
> - De bestemming die IP-adressen moet worden ingesteld op [IP-adresbereiken Azure datacenter](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


| URL-patroon                                                      | Component/functionaliteit                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple Manager-service<br>Access Control-Service<br>Azure Service Bus|
|`http://*.backup.windowsazure.com`|De registratie|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Het intrekken van certificaten |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure opslag rekeningen en controle |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-servers<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-pakket                                                  |
| `http://*.data.microsoft.com `                   | Telemetrie-service in Windows, Zie de [update voor de ervaring van de klant en diagnostische telemetrie](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Volgende stap

-   [De portal voor de implementatie van uw virtuele matrix StorSimple voorbereiden](storsimple-ova-deploy1-portal-prep.md)
