<properties 
   pageTitle="Virtuele matrix van StorSimple Manager administration | Microsoft Azure"
   description="Informatie over het beheren van uw virtuele matrix van StorSimple in ruimten met behulp van de StorSimple Manager-service in de klassieke Azure portal."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>De StorSimple Manager-service gebruiken voor het beheer van uw virtuele StorSimple-Array

![Processtroom](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Overzicht

Dit artikel beschrijft de StorSimple Manager service interface, inclusief hoe u verbinding maakt en de verschillende opties die beschikbaar zijn, en koppelingen naar de specifieke werkstromen die kunnen worden uitgevoerd via deze gebruikersinterface. 

Na het lezen van dit artikel, weet u hoe:

- Verbinding maken met de service Manager StorSimple
- De gebruikersinterface voor het beheer van StorSimple navigeren
- Uw virtuele matrix StorSimple via de StorSimple Manager-service beheren

> [AZURE.NOTE] De opties beschikbaar voor het apparaat StorSimple 8000-serie, Ga naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Verbinding maken met de service Manager StorSimple

De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere virtuele StorSimple-Arrays. U kunt een centrale Microsoft Azure klassieke portal wordt uitgevoerd in een browser gebruiken om deze apparaten te beheren. Voor de verbinding met de service Manager StorSimple wordt door het volgende te doen.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service.

1. Ga naar [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Met behulp van de referenties van uw Microsoft-account, aanmelden bij de Microsoft Azure klassieke portal (te vinden op de rechterbovenhoek van het deelvenster).

3. Blader in het linkernavigatievenster voor de StorSimple Manager-service.

    ![Ga naar service](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>De service Manager StorSimple UI navigeren

De hiërarchie van de navigatie voor de service Manager StorSimple gebruikersinterface wordt weergegeven in de volgende tabel.

- De openingspagina **StorSimple Manager** gaat u naar de UI-service pagina's van toepassing op alle virtuele matrices binnen een service.

- De pagina **apparaten** gaat u naar de apparaat-UI pagina's voor een specifieke virtuele matrix.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Hiërarchie van Manager StorSimple service navigatie

|Portaalpagina|Service pagina 's|Apparaat-pagina 's|
|---|---|---|
|StorSimple Manager-service|Dashboard (service)|Dashboard (apparaat)|
||Apparaten →|Monitor|
||Back-catalogus|Aandelen (bestandsserver) of </br>Volumes (iSCSI-server)|
||(Service) configureren|(Apparaat) configureren|
||Taken|Onderhoud|
||Waarschuwingen|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>De StorSimple Manager-service gebruiken voor het uitvoeren van beheertaken

De volgende tabel bevat een overzicht van de algemene beheertaken en complexe workflows die kunnen worden uitgevoerd binnen de service Manager StorSimple UI. Deze taken zijn geordend op basis van de UI-pagina's waarop ze worden gestart.

Voor meer informatie over elke workflow, klikt u op de desbetreffende procedure in de tabel.

#### <a name="storsimple-manager-workflows"></a>Werkstromen StorSimple Manager

|Als u dit wilt doen...|Ga naar deze pagina...|Gebruik deze procedure|
|---|---|---|
|Een service maken</br>Een service verwijderen</br>Ophalen van de sleutel voor de inschrijving</br>De registratie sleutel opnieuw genereren|StorSimple Manager-service|[De StorSimple Manager-service implementeren](storsimple-ova-manage-service.md)|
|Wijzigen van de sleutel data service</br>De bewerkingen logboeken weergeven|Manager service → StorSimple Dashboard|[Gebruik het dashboard StorSimple service](storsimple-ova-service-dashboard.md)|
|Een virtuele matrix deactiveren</br>Een virtuele matrix verwijderen|StorSimple Manager service → apparaten|[Een virtuele matrix verwijderen of deactiveren](storsimple-ova-deactivate-and-delete-device.md)|
|Herstel na storing herstel- en failover</br>Vereisten voor failover-</br>Failover naar een virtueel apparaat</br>Business continuity noodherstel (BCDR)</br>Fouten tijdens noodherstel|StorSimple Manager service → apparaten|[Herstel na storing herstel- en failover voor uw virtuele matrix StorSimple](storsimple-ova-failover-dr.md)|
|Back-up van aandelen en volumes</br>Een handmatige back-up maken</br>Het back-upschema wijzigen</br>Bestaande back-ups weergeven|Catalogus StorSimple Manager → back-up|[Back-up van uw virtuele StorSimple-Array](storsimple-ova-backup.md)|
|Aandelen terugzetten vanaf een back-upset</br>Volumes terugzetten vanaf een back-upset</br>Herstel op itemniveau (bestandsserver)|Manager service → StorSimple back-catalogus|[Terugzetten vanaf een back-up van uw virtuele matrix StorSimple](storsimple-ova-restore.md)|
|Over opslag accounts</br>Een opslag toevoegen</br>Een opslag account bewerken</br>Een opslag-account verwijderen|StorSimple Manager service → configureren|[Opslag beheren voor de virtuele matrix StorSimple](storsimple-ova-manage-storage-accounts.md)|
|Over access control-records</br>Toevoegen of wijzigen van de record voor een besturingselement </br>Een access control-record verwijderen|StorSimple Manager service → configureren|[Access control-records beheren voor de virtuele matrix StorSimple](storsimple-ova-manage-acrs.md)|
|Details van taak|StorSimple Manager service → projecten| [Virtuele matrix StorSimple taken beheren](storsimple-ova-manage-jobs.md)|
|Instellingen voor waarschuwingen</br>Meldingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen bekijken|StorSimple Manager service → waarschuwingen|[Weergeven en beheren van waarschuwingen voor de virtuele matrix StorSimple](storsimple-ova-manage-alerts.md)|
|Het apparaat administrator-wachtwoord wijzigen|StorSimple Manager service → apparaten → configureren|[Het virtuele matrix StorSimple apparaat administrator-wachtwoord wijzigen](storsimple-ova-change-device-admin-password.md)|
|Software-updates installeren|StorSimple Manager service → apparaten → onderhoud|[Bijwerken van uw virtuele matrix](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] U moet het [lokale web UI](storsimple-ova-web-ui-admin.md) voor de volgende taken:
>
>- [Ophalen van de sleutel data service](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Maak een ondersteuningspakket](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Stoppen en opnieuw starten van een virtuele matrix](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Volgende stappen
Ga voor informatie over het web UI en het gebruik ervan, [de StorSimple on line gebruikersinterface voor het beheer van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md)gebruiken.
