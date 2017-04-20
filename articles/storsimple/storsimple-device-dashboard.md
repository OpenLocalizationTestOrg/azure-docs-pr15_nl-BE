<properties
   pageTitle="Gebruik het apparaat Manager StorSimple dashboard | Microsoft Azure"
   description="De StorSimple Manager service apparaat dashboard en hoe opslag metrics en verbonden initiators weergeven en het serienummer en de IQN vinden met beschrijft."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-device-dashboard"></a>Gebruik het apparaat Manager StorSimple dashboard

## <a name="overview"></a>Overzicht

Het apparaat Manager StorSimple dashboard biedt u een overzicht van de informatie voor een bepaald apparaat StorSimple, in tegenstelling tot het servicedashboard, waarmee u informatie over alle apparaten die deel uitmaken van de oplossing met Microsoft Azure StorSimple.

![Dashboardpagina apparaat](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Het dashboard bevat de volgende informatie:

- **Grafiekgebied** : ziet u de statistieken betreffende opslag in het grafiekgebied aan de bovenkant van het dashboard. In deze grafiek kunt u parameters voor de totale primaire opslag (de hoeveelheid gegevens die zijn geschreven door hosts op het apparaat) en de totale cloud opslag worden gebruikt door het apparaat in een bepaalde tijdsperiode weergeven.

     In deze context, *primaire opslag* verwijst naar het totale bedrag van de gegevens die door de host worden geschreven en kan worden uitgesplitst naar volumetype: *primaire gelaagde opslag* omvat zowel lokaal opgeslagen gegevens en gegevens doorverbonden naar de cloud; *primaire lokaal vastgemaakt opslag* bevat alleen gegevens lokaal opgeslagen. *Cloud opslag*, is aan de andere kant een meting van de totale hoeveelheid gegevens die zijn opgeslagen in de cloud. Dit omvat gegevens trapsgewijze en back-ups. Houd er rekening mee dat gegevens die zijn opgeslagen in de cloud is deduplicated en gecomprimeerd, dat primaire opslag geeft aan de hoeveelheid opslagruimte die wordt gebruikt dat voordat de gegevens worden deduplicated en gecomprimeerd. (U kunt deze twee getallen om een idee krijgen van de compressie-snelheid vergelijken.) Voor zowel de primaire en cloud opslag, de bedragen zullen worden gebaseerd op de tracking frequentie die u configureert. Bijvoorbeeld, als u een frequentie van één week, worden vervolgens de grafiek gegevens weergegeven voor elke dag in de vorige week.

     U kunt het diagram als volgt configureren:

     - Als u wilt zien hoeveel cloud opslag verbruikt na verloop van tijd, de optie **CLOUD opslag gebruikt** . Overzicht van de totale opslagruimte die is geschreven door de host, selecteert de **Primaire GELAAGDE opslag gebruikt** en **Primaire lokaal VASTGEMAAKT opslag gebruikt** . Beide opties zijn geselecteerd in de illustratie; de grafiek wordt daarom bedragen voor zowel cloud als primaire opslag opslag. Houd er rekening mee dat alle primaire opslag gebruikt voor het installeren van Update 2 wordt vertegenwoordigd door de regel voor **Primaire GELAAGDE opslag gebruikt** .
     - Gebruik de vervolgkeuzelijst in de rechterbovenhoek van het diagram aan een periode van 1 week, 1 maand, 3 maanden of 1 jaar. Opmerking de grafiek op het hoogste niveau wordt slechts één keer per dag vernieuwd en daarom geeft de totalen van de vorige dag.

     Zie [werken met de StorSimple Manager-service voor het controleren van uw apparaat StorSimple](storsimple-monitor-device.md)voor meer informatie.

- **Overzicht gebruik** – In het gebied **Gebruik overzicht** ziet u de hoeveelheid primaire opslag gebruikt, de hoeveelheid opslagruimte ingericht en de maximale opslagcapaciteit voor uw apparaat. Vergelijk deze getallen gebruik om de maximale hoeveelheid opslagruimte die beschikbaar is, ziet u in één oogopslag als u extra opslagruimte verkrijgen. Opmerking Dit overzicht wordt elke 15 minuten bijgewerkt en door het verschil in de updatefrequentie, kan verschillende nummers weergeven dan die wordt weergegeven in het diagram hierboven, die dagelijks wordt bijgewerkt. Zie [werken met de StorSimple Manager-service voor het controleren van uw apparaat StorSimple](storsimple-monitor-device.md)voor meer informatie.


- **Waarschuwingen** – het gebied met **meldingen** bevat een overzicht van de waarschuwingen voor uw apparaat. Waarschuwingen zijn gegroepeerd op prioriteit en een telling van het aantal waarschuwingen op elk prioriteitsniveau wordt geleverd. De ernst van de waarschuwing te klikken opent een bereik weergave van het tabblad waarschuwingen waarschuwingen die ernst voor dit apparaat weergeven.

- **Taken** : de module **projecten** ziet u het resultaat van de recente activiteiten van de taak. Dit kan u verzekeren dat het systeem werkt zoals verwacht, of u weet kunt dat u moet corrigerende maatregelen nemen. Als u meer informatie over de onlangs voltooide taken, klikt u op **taken is voltooid in de laatste 24 uur**.

- Het gebied van het **Overzicht** aan de rechterzijde van het dashboard biedt nuttige informatie zoals apparaat-model, serienummer, status, beschrijving en aantal volumes.

U kunt ook configureren van failover en verbonden initiators van het apparaat dashboard weergeven.

Algemene taken die kunnen worden uitgevoerd op deze pagina zijn:

- Verbonden initiators weergeven

- Het serienummer van het apparaat zoeken

- Het doel apparaat IQN zoeken

## <a name="view-connected-initiators"></a>Verbonden initiators weergeven

U kunt de iSCSI-initiators die zijn aangesloten op uw apparaat door te klikken op de **weergave initiators verbonden** koppeling in het gedeelte **Overzicht** van het dashboard van uw apparaat weergeven. Deze pagina biedt een overzicht in tabelvorm van de initiators die verbinding met het apparaat hebt gemaakt. Voor elke initiator Zie:

- De iSCSI Qualified Name (IQN) van de aanvrager van de verbonden.

- De naam van de access control record (ACR) waarmee deze verbonden initiator.

- Het IP-adres van de aanvrager van de verbonden.

- De netwerkinterfaces die op de initiator is aangesloten op uw opslagapparaat. Deze kunnen variëren van gegevens van 0 tot en met 5 van de gegevens.

- Alle volumes die de aanvrager van de verbinding is afhankelijk van de huidige configuratie van ACR toegang tot.

Als u onverwachte initiators in deze lijst wordt weergegeven of niet de verwachte resultaten ziet, bekijk de ACR-configuratie. Maximaal 512 initiators kunt aansluiten op uw apparaat.

## <a name="find-the-device-serial-number"></a>Het serienummer van het apparaat zoeken

Mogelijk moet u het serienummer van het apparaat bij het configureren van Microsoft Multipath I/O (MPIO) op het apparaat. Voer de volgende stappen uit om het serienummer van het apparaat.

#### <a name="to-find-the-device-serial-number"></a>Het serienummer van het apparaat zoeken

1. Ga naar **apparaten** > **Dashboard**.

2. Ga naar het gedeelte **Overzicht** in het rechterdeelvenster van het dashboard.

3. Schuif naar beneden en zoek het serienummer.

## <a name="find-the-device-target-iqn"></a>Het doel apparaat IQN zoeken

Mogelijk moet u het doel apparaat IQN bij het configureren van het Challenge Handshake Authentication Protocol (CHAP) op uw apparaat StorSimple. Voer de volgende stappen uit om het doel apparaat IQN.

### <a name="to-find-the-device-target-iqn"></a>Het doel apparaat IQN zoeken

1. Ga naar **apparaten** > **Dashboard**.

1. Ga naar het gedeelte **Overzicht** in het rechterdeelvenster van het dashboard.

1. Schuif naar beneden en zoek naar het doel IQN.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [StorSimple Manager servicedashboard](storsimple-service-dashboard.md).
- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
