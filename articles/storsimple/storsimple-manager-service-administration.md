<properties 
   pageTitle="Servicebeheer StorSimple | Microsoft Azure"
   description="Informatie over het beheren van uw StorSimple-apparaat met behulp van de StorSimple Manager-service in de klassieke Azure portal."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>De StorSimple Manager-service gebruiken voor het beheren van uw apparaat StorSimple

## <a name="overview"></a>Overzicht

Dit artikel beschrijft de StorSimple Manager-interface, zoals verbinding maken met deze verschillende opties en koppelingen om naar de specifieke werkstromen die via deze gebruikersinterface kunnen worden uitgevoerd. Deze richtlijn is van toepassing op beide; de StorSimple fysiek en virtueel apparaat.

Na het lezen van dit artikel leert u het:

- Verbinding maken met de service Manager StorSimple
- De gebruikersinterface voor het beheer van StorSimple navigeren
- Het apparaat StorSimple via de StorSimple Manager-service beheren


## <a name="connect-to-storsimple-manager-service"></a>Verbinding maken met de service Manager StorSimple

De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple apparaten. U kunt een centrale Microsoft Azure klassieke portal wordt uitgevoerd in een browser gebruiken om deze apparaten te beheren. Voor de verbinding met de service Manager StorSimple wordt door het volgende te doen.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service.

1. Ga naar [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Met behulp van de referenties van uw Microsoft-account, aanmelden bij de Microsoft Azure klassieke portal (te vinden op de rechterbovenhoek van het deelvenster).

1. Blader in het linkernavigatievenster voor de StorSimple Manager-service.


## <a name="navigate-storsimple-manager-service-ui"></a>Service Manager StorSimple UI navigeren

De hiërarchie van de navigatie voor de service Manager StorSimple gebruikersinterface wordt weergegeven in de volgende tabel.

- Portaalpagina **StorSimple Manager** gaat u naar de UI-service pagina's van toepassing is op alle apparaten binnen een service.

- Pagina **apparaten** gaat u naar de apparaat-UI pagina's die van toepassing zijn op een specifiek apparaat.

- **Volume, Containers** pagina gaat u naar de pagina van het volume waarin alle volumes die zijn gekoppeld aan een apparaat.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Hiërarchie van Manager StorSimple service navigatie

|Portaalpagina|Service pagina 's|Apparaat-pagina 's|Apparaat-pagina 's|
|---|---|---|---|
|StorSimple Manager-service|Service-dashboard|Apparaat dashboard||
||Apparaten →|Monitor|
||Back-catalogus|Volume containers→|Volumes|
||(Service) configureren|Back-beleid||
||Taken|(Apparaat) configureren|
||Waarschuwingen|Onderhoud|

![Video beschikbaar](./media/storsimple-manager-service-administration/Video_icon.png) **Video beschikbaar**

Om een video die u de gebruikersinterface van de StorSimple Manager-service doorloopt, klik [hier](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>StorSimple apparaat met StorSimple Manager-service beheren

De volgende tabel bevat een overzicht van de algemene beheertaken en complexe workflows die kunnen worden uitgevoerd binnen de service Manager StorSimple UI. Deze taken zijn geordend op basis van de UI-pagina's waarop ze worden gestart.

Voor meer informatie over elke workflow, klikt u op de desbetreffende procedure in de tabel.

#### <a name="storsimple-manager-workflows"></a>Werkstromen StorSimple Manager

|Als u dit wilt doen...|Ga naar deze pagina...|Gebruik deze procedure.|
|---|---|---|
|Een service maken</br>Een service verwijderen</br>Service registratie sleutel ophalen</br>Registratie sleutel opnieuw genereren|StorSimple Manager-service|[Implementatie van een service Manager StorSimple](storsimple-manage-service.md)
|Wijzigen van de sleutel data service</br>De logboeken weergeven|Manager service → StorSimple Dashboard|[De Manager StorSimple servicedashboard gebruiken](storsimple-service-dashboard.md)|
|Een apparaat deactiveren</br>Een apparaat verwijderen|StorSimple Manager service → apparaten|[Een apparaat verwijderen of deactiveren](storsimple-deactivate-and-delete-device.md)|
|Meer informatie over herstel na storing herstel- en failover</br>Failover naar een fysiek apparaat</br>Failover naar een virtueel apparaat</br>Business continuity noodherstel (BCDR)|StorSimple Manager service → apparaten|[Failover en disaster recovery voor uw apparaat StorSimple](storsimple-device-failover-disaster-recovery.md)|
|Lijst met back-ups voor een volume</br>Selecteer een back-upset</br>Een back-upset verwijderen|Manager service → StorSimple back-catalogus|[Back-ups beheren](storsimple-manage-backup-catalog.md)|
|Klonen van een volume|Manager service → StorSimple back-catalogus|[Klonen van een volume](storsimple-clone-volume.md)|
|Een back-upset herstellen|Manager service → StorSimple back-catalogus|[Een back-upset herstellen](storsimple-restore-from-backup-set.md)|
|Over opslag accounts</br>Een opslag toevoegen</br>Een opslag account bewerken</br>Een opslag-account verwijderen</br>Key rotatie van opslag rekeningen|StorSimple Manager service → configureren|[Beheren opslag](storsimple-manage-storage-accounts.md)|
|Bandbreedte-sjablonen</br>Een bandbreedte sjabloon toevoegen</br>Een bandbreedte sjabloon bewerken</br>Een bandbreedte-sjabloon verwijderen</br>Een standaardsjabloon bandbreedte gebruiken</br>Een hele dag duurt bandbreedte sjabloon maken die op de opgegeven tijd gestart|StorSimple Manager service → configureren|[Bandbreedte beheren](storsimple-manage-bandwidth-templates.md)|
|Over access control-records</br>Record voor een besturingselement maken</br>Record voor een besturingselement bewerken</br>Een access control-record verwijderen|StorSimple Manager service → configureren|[Access control-records beheren](storsimple-manage-acrs.md)|
|Details van taak</br>Een taak annuleren|StorSimple Manager service → projecten|[Taken beheren](storsimple-manage-jobs.md)
|Meldingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen bekijken|StorSimple Manager service → waarschuwingen|[Weergeven en beheren van waarschuwingen StorSimple](storsimple-manage-alerts.md)
|Verbonden initiators weergeven</br>Het serienummer van het apparaat zoeken</br>Het doel van IQN zoeken|StorSimple Manager service → apparaten → Dashboard|[Gebruik het apparaat StorSimple dashboard](storsimple-device-dashboard.md)|
|Controle grafieken maken|StorSimple Manager service → apparaten → controleren|[Het apparaat StorSimple controleren](storsimple-monitor-device.md)|
|Toevoegen van een volume-container</br>Een container volume wijzigen</br>Een container voor volume verwijderen|StorSimple Manager service → apparaten → Volume Containers|[Volume-containers beheren](storsimple-manage-volume-containers.md)|
|Een volume toevoegen</br>Een volume wijzigen</br>Een volume off line nemen</br>Een volume verwijderen</br>Een volume controleren|Manager service → apparaten → Volume Containers → StorSimple Volumes|[Volumes beheren](storsimple-manage-volumes.md)|
|Apparaatinstellingen wijzigen</br>Tijdinstellingen wijzigen</br>DNS.md-instellingen wijzigen</br>Netwerkinterfaces configureren|StorSimple Manager service → apparaten → configureren|[Apparaatconfiguratie voor het apparaat StorSimple wijzigen](storsimple-modify-device-config.md)|
|Web proxy-instellingen weergeven|StorSimple Manager service → apparaten → configureren|[Webproxy voor het apparaat configureren](storsimple-configure-web-proxy.md)|
|Apparaat administrator-wachtwoord wijzigen</br>StorSimple Snapshot Manager-wachtwoord wijzigen|StorSimple Manager service → apparaten → configureren|[StorSimple wachtwoorden wijzigen](storsimple-change-passwords.md)|
|Extern beheer configureren|StorSimple Manager service → apparaten → configureren|[Extern verbinding maken met uw apparaat StorSimple](storsimple-remote-connect.md)|
|Instellingen voor waarschuwingen|StorSimple Manager service → apparaten → configureren|[Weergeven en beheren van waarschuwingen StorSimple](storsimple-manage-alerts.md)|
|CHAP configureren voor uw apparaat StorSimple|StorSimple Manager service → apparaten → configureren|[CHAP configureren voor uw apparaat StorSimple](storsimple-configure-chap.md)|
|Een back-up beleid toevoegen</br>Toevoegen of wijzigen van een schema</br>Een back-beleid verwijderen</br>Een handmatige back-up maken</br>Een aangepast beleid voor back-up maken met meerdere volumes en planningen|StorSimple Manager service → apparaten → back-up beleid|[Back-beleid beheren](storsimple-manage-backup-policies.md)|
|Apparaatcontrollers stoppen</br>Apparaatcontrollers opnieuw starten</br>Apparaatcontrollers afsluiten</br>Uw apparaat naar de standaardwaarden opnieuw instellen</br>(Alleen voor ruimten apparaat zijn)|StorSimple Manager service → apparaten → onderhoud|[Apparaatbesturing StorSimple beheren](storsimple-manage-device-controller.md)|
|Meer informatie over hardwareonderdelen StorSimple</br>Controleren, hardwarestatus</br>(Alleen voor ruimten apparaat zijn)|StorSimple Manager service → apparaten → onderhoud|[Monitor hardwareonderdelen](storsimple-monitor-hardware-status.md)|
|Maak een ondersteuningspakket|StorSimple Manager service → apparaten → onderhoud|[Maken en beheren van een Support-pakket](storsimple-create-manage-support-package.md)|
|Software-updates installeren|StorSimple Manager service → apparaten → onderhoud|[Uw apparaat bijwerken](storsimple-update-device.md)|


##<a name="next-steps"></a>Volgende stappen
Als u problemen met de dagelijkse werking van uw apparaat StorSimple of met de hardware-onderdelen ondervindt, Raadpleeg:

- [Een operationele apparaat oplossen](storsimple-troubleshoot-operational-device.md)
- [StorSimple controle indicator LED's gebruiken](storsimple-monitoring-indicators.md)

Als oplossing voor de problemen en moet u een serviceaanvraag maken, raadpleegt u [Contact Microsoft Support](storsimple-contact-microsoft-support.md).
