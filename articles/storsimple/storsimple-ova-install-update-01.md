<properties 
   pageTitle="Updates installeren op een virtuele matrix StorSimple | Microsoft Azure"
   description="Wordt beschreven hoe u de virtuele matrix StorSimple web UI updates via de portal en hotfix-methode toe te passen"
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
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Updates installeren op uw virtuele StorSimple-Array

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u updates te installeren op uw virtuele matrix StorSimple via het lokale web UI en via de klassieke Azure-portal. U moet software-updates of hotfixes voor het up-to-date houden van uw virtuele matrix StorSimple. 

Houd er rekening mee dat een update of hotfix installeert het apparaat opnieuw is opgestart. Gezien het feit dat de StorSimple virtuele matrix een apparaat met één knooppunt is, alle I/O in voortgang wordt onderbroken en uitvaltijd te maken krijgt met het apparaat. 

Voordat u een update toepast, wordt aangeraden dat u de volumes of -shares off line op de host eerste neemt en vervolgens het apparaat. Dit verkleint de mogelijkheid van beschadiging van gegevens.

> [AZURE.IMPORTANT] Als u Update 0,1 of GA softwareversies uitvoert, moet u de hotfix methode via het lokale web UI 0,3-update te installeren. Als u Update 0,2 uitvoert, wordt u aangeraden dat u de updates via de klassieke Azure portal installeert.

## <a name="use-the-local-web-ui"></a>Het lokale web UI gebruiken 
 
Er zijn twee stappen bij het gebruik van het lokale web UI:

- Download de update of hotfix
- Installeer de update of hotfix

### <a name="download-the-update-or-the-hotfix"></a>Download de update of hotfix

Voer de volgende stappen uit om de software-update downloaden vanaf de Microsoft Update-catalogus.

#### <a name="to-download-the-update-or-the-hotfix"></a>Downloaden van de update of hotfix

1. Start Internet Explorer en Ga naar [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Als dit de eerste keer dat u de Microsoft Update-catalogus op deze computer, klikt u op **installeren** wanneer u wordt gevraagd de Microsoft Update-catalogus-invoegtoepassing installeren.
  
3. Voer in het zoekvak van de Microsoft Update-catalogus, de Knowledge Base (KB)-nummer van de hotfix die u wilt downloaden. **3182061** voor Update 0,3 invoeren en klik op **Zoeken**.

    De hotfix-aanbieding wordt weergegeven, bijvoorbeeld **StorSimple virtuele matrix Update 0,3**.

    ![Catalogus doorzoeken](./media/storsimple-ova-install-update-01/download1.png)

4. Klik op **toevoegen**. De update wordt toegevoegd aan het winkelmandje.

5. Klik op **mandje weergeven**.

6. Klik op **downloaden**. Geef op of **Blader** naar een lokale locatie waar u de downloads worden weergegeven. De updates zijn gedownload naar de opgegeven locatie en geplaatst in een submap met dezelfde naam als de update. De map kan ook worden gekopieerd naar een netwerkshare die bereikbaar is via het apparaat.

7. De gekopieerde map opent, ziet u een bestand van Microsoft Update Standalone Package `WindowsTH-KB3011067-x64`. Dit bestand wordt gebruikt voor het installeren van de update of hotfix.


### <a name="install-the-update-or-the-hotfix"></a>Installeer de update of hotfix

Vóór de installatie van de update of hotfix, zorg ervoor dat u de update hebt of de hotfix gedownload op uw host lokaal of toegankelijk via een netwerkshare. 

Gebruik deze methode om updates op een NH-apparaat installeren of bijwerken van 0,1 softwareversies. Deze procedure duurt minder dan 2 minuten. Voer de volgende stappen uit om de update of hotfix hebt geïnstalleerd.


#### <a name="to-install-the-update-or-the-hotfix"></a>Installeren van de update of hotfix

1. Ga in het lokale web UI naar **onderhoud** > **Software-Update**.

    ![bijwerken van apparaat](./media/storsimple-ova-install-update-01/update1m.png)

2. Voer in het **pad naar het bestand bijwerken**, de naam van de update of hotfix. U kunt ook zoeken naar de update of hotfix-installatiebestand als op een netwerkshare wordt geplaatst. Klik op **toepassen**.

    ![bijwerken van apparaat](./media/storsimple-ova-install-update-01/update2m.png)

3.  Er wordt een waarschuwing weergegeven. Gezien dit is een apparaat met één knooppunt nadat de update is toegepast, het apparaat wordt opnieuw opgestart en er downtime. Klik op het pictogram.

    ![bijwerken van apparaat](./media/storsimple-ova-install-update-01/update3m.png)

4. De update wordt gestart. Nadat het apparaat is bijgewerkt, opnieuw wordt gestart. De gebruikersinterface van lokale is niet toegankelijk in deze duur.

    ![bijwerken van apparaat](./media/storsimple-ova-install-update-01/update5m.png)

5. Na het opnieuw opstarten voltooid is, wordt u geleid naar de pagina **aanmelden** . Om te controleren of de software is bijgewerkt, in het lokale web UI, Ga naar **onderhoud** > **Software-Update**. **10.0.0.0.0.10288.0** voor 0,3-Update moet zijn versie van de software weergegeven.

    > [AZURE.NOTE] We melden de versies van software in een iets andere manier in het lokale web UI en de klassieke Azure portal. Bijvoorbeeld het lokale web UI **10.0.0.0.0.10288** en de klassieke Azure portal verslagen **10.0.10288.0** voor dezelfde versie. 

    ![bijwerken van apparaat](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>De klassieke Azure portal gebruiken

Als 0,2 Update wordt uitgevoerd, wordt u aangeraden het installeren van updates via de klassieke Azure portal. De portal procedure vereist dat de gebruiker om te scannen, download en installeer vervolgens de updates. Deze procedure duurt ongeveer 7 minuten. Voer de volgende stappen uit om de update of hotfix hebt geïnstalleerd.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Nadat de installatie is voltooid (zoals aangegeven door de status van de taak op 100%), gaat u naar **apparaten > onderhoud > Software Updates**. 10.0.10288.0 moet zijn versie van de software weergegeven.

![bijwerken van apparaat](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md).
