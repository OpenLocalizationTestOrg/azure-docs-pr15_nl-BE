<properties 
   pageTitle="Het apparaat StorSimple bewaken | Microsoft Azure"
   description="Beschrijving van het gebruik van de service Manager StorSimple i/o-prestaties, bezettingsgraad netwerkdoorvoer en prestaties van een apparaat controleren."
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
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>De StorSimple Manager-service gebruiken voor het controleren van uw apparaat StorSimple 

## <a name="overview"></a>Overzicht

De StorSimple Manager-service kunt u specifieke apparaten binnen uw oplossing StorSimple te controleren. U kunt aangepaste grafieken op basis van i/o-prestaties, bezettingsgraad netwerkdoorvoer en prestatiemetingen apparaat maken. 

De controle om informatie te bekijken voor een specifiek apparaat in de klassieke Azure portal, selecteert u de StorSimple Manager-service. Klik op het tabblad **Monitor** en selecteer vervolgens in de lijst met apparaten. **De pagina** bevat de volgende informatie.

## <a name="io-performance"></a>I/o-prestaties 

Nummers van **i/o-** prestatiegegevens gerelateerd aan het aantal gelezen en schrijfbewerkingen tussen een iSCSI-initiator interfaces op de server en het apparaat of het apparaat en de cloud. Deze prestaties kan worden gemeten voor een specifiek volume of een bepaald volume container alle containers van het volume.

In onderstaande tabel ziet u de I/O voor de initiator naar het apparaat voor de volumes voor een productie-apparaat. Cijfers over de uitgezet worden gelezen en geschreven bytes per seconde, lezen en i/o-bewerkingen per seconde, schrijven en lezen en schrijven vertragingstijden.

![I/o-prestaties van de initiator naar apparaat](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

De i/o-bewerkingen zijn voor hetzelfde hulpmiddel uitgezet voor de gegevens van het apparaat naar de cloud voor alle containers van het volume. Op dit apparaat, de gegevens alleen in de lineaire laag en niets heeft zich verspreid naar de cloud. Er zijn geen lezen / schrijven-bewerkingen van het apparaat naar de cloud plaatsvindt. Daarom zijn de pieken in de grafiek met een interval van 5 minuten dat komt overeen met de frequentie waarmee de heartbeat tussen het apparaat en de service is ingeschakeld. 

![I/o-prestaties van het apparaat naar cloud](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


Voor het apparaat is een momentopname van een wolk genomen voor volumegegevens vanaf 2:00 uur. Dit resulteerde in gegevens die voortvloeien uit het apparaat naar de cloud. Leest schrijft zijn verzonden naar de cloud in deze duur. De i/o-grafiek bevat een piek in de verschillende maatstaven die overeenkomt met de tijd waarop de momentopname is gemaakt. 

![I/o-prestaties voor apparaat na momentopname cloud cloud](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>Capaciteit-gebruik 

**Bezettingsgraad** bijgehouden gegevens met betrekking tot de hoeveelheid opslagruimte die wordt gebruikt door de volumes, volume, containers of apparaat. U kunt rapporten op basis van het gebruik van de capaciteit van uw primaire opslag, de opslag van de wolk of uw opslag apparaat maken. Capaciteit gebruik kan worden gemeten op een specifiek volume of een bepaald volume container alle containers van het volume.


De primaire, cloud en opslagcapaciteit apparaat kan als volgt worden beschreven:

###<a name="primary-storage-capacity-utilization"></a>Gebruik van primaire opslag capaciteit
 
Deze grafieken tonen de hoeveelheid gegevens die naar StorSimple volumes geschreven voordat de gegevens worden deduplicated en gecomprimeerd. U kunt de primaire opslaggebruik bekijken door alle volumes of voor een enkel volume.

Wanneer u de primaire opslag volume capaciteit gebruik grafieken voor alle volumes ten opzichte van elk van de afzonderlijke volumes weergeven en de primaire gegevens in deze beide gevallen optellen, mogelijk niet overeen tussen de twee getallen. De totale primaire gegevens op alle volumes kunnen niet tot de totale som van de primaire gegevens van de afzonderlijke volumes toevoegen. Dit kan worden veroorzaakt door een van de volgende opties:

- **Momentopnamegegevens opgenomen voor alle volumes**: dit gedrag alleen als u versie eerder dan Update 3 wordt gezien. De primaire gegevens voor alle volumes weergegeven is de som van de primaire gegevens voor elk volume en de momentopname. De primaire gegevens weergegeven voor een bepaald volume komt overeen met de hoeveelheid gegevens op het volume is toegewezen (en omvat niet de bijbehorende volume momentopnamegegevens).

    Dit kan ook worden verklaard door de volgende vergelijking:

    *Primaire gegevens (alle volumes) = de som van de (primaire gegevens (volume i) + grootte van een van momentopnamegegevens (volume i))*
    
    *waar, primaire gegevens (volume i) = grootte van de primaire gegevens die zijn toegewezen aan het volume i*
 
    Als de momentopnamen worden verwijderd via de service, wordt de verwijdering asynchroon op de achtergrond uitgevoerd. Het kan even duren voor de grootte van het volume gegevens moeten worden bijgewerkt na het verwijderen van de momentopname. 

    Als u Update 3 of hoger, vervolgens de momentopnamegegevens niet opgenomen in de gegevens op het volume. En het primaire gebruik wordt als volgt berekend:

    * Primaire gegevens (alle volumes) = de som van (primaire gegevens (volume i)
    
    *waar, primaire gegevens (volume i) = grootte van de primaire gegevens die zijn toegewezen aan het volume i*
 
- **Volumes met controle uitgeschakeld in alle volumes opgenomen**: hebt u volumes op het apparaat waarvoor controle is uitgeschakeld, de bewaking verkregen gegevens voor de afzonderlijke volumes is alleen beschikbaar in de grafieken. De gegevens voor alle volumes in de grafiek bevat echter de volumes waarvoor controle is uitgeschakeld. 
 
- **Volumes met live gekoppelde back-ups opgenomen voor alle volumes verwijderd**: als volumes met momentopnamegegevens, worden verwijderd, maar de gekoppelde momentopnamen nog steeds bestaat, dan u niet overeen ziet mogelijk.

- **Verwijderde volumes opgenomen voor alle volumes**: oude volumes kunnen In sommige gevallen bestaan, hoewel deze zijn verwijderd. Het effect van verwijdering niet wordt gezien en het apparaat minder beschikbare capaciteit kan weergeven. U moet contact opnemen met Microsoft Support deze volumes verwijderen.

De volgende grafieken tonen het gebruik van primaire opslag capaciteit van een apparaat StorSimple vóór en na het maken van een momentopname van een wolk. Als u alleen de gegevens van het volume is, moet een momentopname van een wolk de primaire opslag niet wijzigen. Zoals u ziet, bevat de grafiek geen verschil tussen het gebruik van primaire capaciteit als gevolg van een momentopname van de wolk. De momentopname van de wolk begint bij ongeveer 2:00 pm op dat apparaat.

![Primaire bezettingsgraad voordat cloud momentopname](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Primaire bezettingsgraad na cloud momentopname](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Als u Update 2 of hoger, u kunt uitsplitsen naar het gebruik van primaire opslag capaciteit een afzonderlijk volume, alle volumes, alle gelaagde volumes en alle lokaal vastgezette volumes zoals hieronder wordt weergegeven. Splitsen door alle lokaal vastgezette volumes kunt u snel nagaan hoeveel van het lokale niveau wordt gebruikt.

![Primaire gebruik van capaciteit voor alle lokale vastgemaakt volumes](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Gebruik van cloud opslag capaciteit

Deze grafieken tonen het bedrag van de cloud opslag gebruikt. Deze gegevens zijn deduplicated en gecomprimeerd. Dit bedrag omvat wolk momentopnamen die mogelijk gegevens bevatten die niet wordt weergegeven in een primaire volume en wordt gehouden ter behoud van oude of vereist. U kunt de primaire vergelijken en cloud opslag verbruik cijfers krijgt u een indruk van de gegevenssnelheid vermindering, hoewel het nummer niet exact. De volgende grafieken tonen de cloud opslaggebruik van capaciteit van een apparaat StorSimple vóór en na het maken van een momentopname van een wolk. De momentopname van de wolk begint bij ongeveer 2:00 pm op dat apparaat en ziet u de bezettingsgraad van de wolk geschoten van bij het verhogen van 5.73 MB versie 4.04 GB.

![Bezettingsgraad van de wolk voor cloud momentopname](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Bezettingsgraad na cloud momentopname van de wolk](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Gebruik van apparaat opslag capaciteit

Deze diagrammen geven het totale gebruik van het apparaat, meer dan primaire opslag worden omdat deze de lineaire SSD-laag bevat. Deze laag bevat een hoeveelheid gegevens die ook op het apparaat, de andere lagen. De capaciteit van de lineaire reeks SSD volgens zodat wanneer nieuwe gegevens binnen komt, de oude gegevens naar de harde schijf laag (die is deduplicated en gecomprimeerd) en vervolgens naar de cloud wordt verplaatst.

Na verloop van tijd zal gebruik van primaire capaciteit en bezettingsgraad apparaat waarschijnlijk toenemen bij elkaar totdat de gegevens begint te worden doorverbonden naar de cloud. Op dat moment de bezettingsgraad van het apparaat waarschijnlijk begint te worden bereikt, maar het gebruik van primaire capaciteit wordt verhoogd als er meer gegevens worden geschreven.

De volgende grafieken tonen het gebruik van primaire opslag capaciteit van een apparaat StorSimple vóór en na het maken van een momentopname van een wolk. De momentopname wolk begonnen om 2:00 uur en de bezettingsgraad van het apparaat verlagen op dat moment. Het gebruik van het apparaat opslag capaciteit werd afgesloten van 11.58 GB 7.48 GB. Hiermee wordt aangegeven dat waarschijnlijk niet-gecomprimeerde gegevens in de lineaire SSD-laag is deduplicated, gecomprimeerd en naar het niveau van de harde schijf verplaatst. Houd er rekening mee dat als het apparaat al een grote hoeveelheid gegevens in de SSD en HDD lagen, u niet deze daling ziet mogelijk. In dit voorbeeld wordt heeft het apparaat een kleine hoeveelheid gegevens.

![Bezettingsgraad apparaat voordat cloud momentopname](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Bezettingsgraad apparaat na cloud momentopname](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Netwerkdoorvoer

**Netwerkdoorvoer** bijgehouden gegevens met betrekking tot de hoeveelheid gegevens die worden overgebracht van de netwerkinterfaces iSCSI-initiator op de hostserver en het apparaat en tussen het apparaat en de cloud. Deze metrische gegevens kunt u controleren voor elk van de iSCSI-netwerkinterfaces op uw apparaat.

Doorvoer van het netwerk weergeven de volgende grafieken voor gegevens 0 en 4 gegevens, zowel 1 GbE-netwerkinterfaces op uw apparaat. In dit geval is gegevens 0 cloud-ingeschakeld terwijl gegevens 4 iSCSI ingeschakeld was. Ziet u de binnenkomende en uitgaande verkeer voor uw apparaat StorSimple. De platte lijn in de grafiek vanaf 3:24 pm is ten gevolge van het feit dat we gegevens alleen om de 5 minuten verzamelen en moet worden genegeerd. 

![De netwerkdoorvoer van het voor Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![De netwerkdoorvoer van het voor Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Prestaties van een apparaat 

**Prestaties van een apparaat** wordt bijgehouden gegevens met betrekking tot de prestaties van het apparaat. In het volgende diagram ziet u de CPU-gebruik-statistieken voor een apparaat in de productie.

![CPU-gebruik voor apparaat](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van het dashboard StorSimple Manager service apparaat](storsimple-device-dashboard.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
