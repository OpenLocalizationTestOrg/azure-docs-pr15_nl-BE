<properties 
   pageTitle="Virtuele matrix StorSimple web UI beheer | Microsoft Azure"
   description="Beschrijving van het uit te voeren fundamentele apparaat via de virtuele matrix StorSimple web UI."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Gebruik de on line gebruikersinterface voor het beheer van uw virtuele StorSimple-Array

![Processtroom](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Overzicht

De zelfstudies in dit artikel van toepassing op de Microsoft Azure StorSimple virtuele matrix (ook bekend als de StorSimple op ruimten virtueel apparaat) met de release van maart 2016 algemene beschikbaarheid (GA). In dit artikel worden enkele van de werkstromen en de taken die kunnen worden uitgevoerd op de virtuele matrix StorSimple beschreven. U kunt de StorSimple virtuele matrix met StorSimple Manager beheren UI (hierna aangeduid als de portal UI) en het lokale web UI voor het apparaat. Dit artikel is gericht op de taken die u kunt uitvoeren met de webgebruikersinterface.

Dit artikel bevat de volgende cursussen:

- Ophalen van de sleutel data service
- Web UI setup-fouten oplossen
- Een pakket logboekbestand genereren
- Afsluiten of opnieuw opstarten van uw apparaat

## <a name="get-the-service-data-encryption-key"></a>Ophalen van de sleutel data service

Wanneer u het eerste apparaat met de StorSimple Manager-service registreert een coderingssleutel service gegevens gegenereerd. Deze sleutel is vereist met de sleutel voor de inschrijving extra apparaten registreren bij de StorSimple Manager-service.

Als u uw coderingssleutel service-gegevens en de noodzaak te halen zijn geraakt, voert u de volgende stappen uit in het lokale web UI van het apparaat geregistreerd bij uw service.

#### <a name="to-get-the-service-data-encryption-key"></a>Ophalen van de sleutel data service

1. Verbinding maken met het lokale web UI. Ga naar **configuratie** > **Cloud-instellingen**.
  

2. Klik op **de sleutel data encryption**onderaan de pagina. Een sleutel wordt weergegeven. Kopiëren en deze sleutel opslaan.
    
    ![service-sleutel voor bestandscodering 1 ophalen](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Web UI setup-fouten oplossen

In sommige gevallen wanneer u het apparaat via het lokale web UI, configureert kunt u uitvoeren in fouten. Als u wilt vaststellen en oplossen van dergelijke fouten, kunt u de diagnostische tests kunt uitvoeren.

#### <a name="to-run-the-diagnostic-tests"></a>De diagnostische tests uitvoeren

1. Ga in het lokale web UI **Troubleshooting** > **diagnostische tests**.

    ![de diagnostische gegevens van 1](./media/storsimple-ova-web-ui-admin/image29.png)

2. Klik onderaan de pagina **Diagnostische Tests worden uitgevoerd**. Deze tests voor het bepalen van mogelijke problemen met uw netwerk, apparaat, webproxy, initiëren tijd of cloud-instellingen. U wordt gewaarschuwd dat het apparaat tests wordt uitgevoerd.

3. Nadat u de tests hebt voltooid, worden de resultaten weergegeven. In het volgende voorbeeld ziet u de resultaten van diagnostische tests. Houd er rekening mee dat de web proxy-instellingen niet op dit apparaat zijn geconfigureerd en de web proxy-test is daarom niet uitgevoerd. De andere tests voor de netwerkinstellingen, DNS-server en tijdinstellingen zijn aangebracht.

    ![de diagnostische gegevens van 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Een pakket logboekbestand genereren

Een logboek pakket bestaat uit alle relevante logboeken die Microsoft Support helpen kunnen bij het oplossen van problemen met een apparaat. In deze release, kan een pakket logboek worden gegenereerd via het lokale web UI.

#### <a name="to-generate-the-log-package"></a>Het pakket logboekbestand genereren

1. Ga in het lokale web UI **Troubleshooting** > **het systeemlogboek in Logboeken**.

    ![genereren van logboekbestanden pakket 1](./media/storsimple-ova-web-ui-admin/image31.png)

2. Onderaan op de pagina, klikt u op **logboek-pakket maken**. Een pakket in het systeemlogboek in Logboeken worden gemaakt. Dit kan enkele minuten duren.

    ![genereren van logboekbestanden pakket 2](./media/storsimple-ova-web-ui-admin/image32.png)

    U krijgt bericht wanneer het pakket is gemaakt en de pagina wordt bijgewerkt om aan te geven van de tijd en datum waarop het pakket is gemaakt.

    ![genereren van logboekbestanden pakket 3](./media/storsimple-ova-web-ui-admin/image33.png)

3. Klik op **logboek downloaden**. Een ZIP-pakket worden op uw systeem gedownload.

    ![genereren van logboekbestanden pakket 4](./media/storsimple-ova-web-ui-admin/image34.png)

4. U kunt unzip het pakket gedownload logboek en de logboekbestanden van het systeem weergeven.

## <a name="shut-down-and-restart-your-device"></a>Afsluiten en opnieuw opstarten van uw apparaat

U kunt afsluiten of opnieuw opstarten van het virtuele apparaat met behulp van het lokale web UI. We aanbevolen dat voordat u opnieuw start, worden de volumes of shares op de host en klik vervolgens op het apparaat. Hiermee minimaliseert u iedere mogelijkheid van beschadiging van gegevens. 

#### <a name="to-shut-down-your-virtual-device"></a>Het virtuele apparaat afgesloten

1. Ga in het lokale web UI naar **onderhoud** > **de instellingen voor energiebeheer**.

2. Onderaan op de pagina, klikt u op **Afsluiten**.

    ![afsluiten voor apparaat 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Een waarschuwing wordt weergegeven met de mededeling dat alle i/o die werden uitgevoerd, wat resulteert in een wordt onderbroken door een afsluiting van het apparaat. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Waarschuwing voor apparaat afsluiten](./media/storsimple-ova-web-ui-admin/image37.png)

    U krijgt het afsluiten is gestart.

    ![apparaat afgesloten gestart](./media/storsimple-ova-web-ui-admin/image38.png)

    Het apparaat wordt nu afgesloten. Als u het apparaat start, moet u dat doen via de Hyper-V-beheer.

#### <a name="to-restart-your-virtual-device"></a>Het virtuele apparaat opnieuw starten

1. Ga in het lokale web UI naar **onderhoud** > **de instellingen voor energiebeheer**.

2. Onderaan op de pagina, klikt u op **opnieuw opstarten**.

    ![apparaat opnieuw starten](./media/storsimple-ova-web-ui-admin/image36.png)

3. Een waarschuwing wordt weergegeven met de melding dat het opnieuw starten van het apparaat een IOs werden uitgevoerd, wat resulteert in een wordt onderbroken. Klik op het pictogram ![Controleer het pictogram](./media/storsimple-ova-web-ui-admin/image3.png).

    ![waarschuwing opnieuw starten](./media/storsimple-ova-web-ui-admin/image37.png)

    U krijgt het opnieuw is gestart.

    ![opnieuw gestart](./media/storsimple-ova-web-ui-admin/image39.png)

    Tijdens het opnieuw opstarten, verliest u de verbinding met de gebruikersinterface. Het opnieuw opstarten kunt u controleren door het regelmatig vernieuwen van de gebruikersinterface. U kunt ook de status van het apparaat opnieuw op te starten via de Hyper-V Manager controleren.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat](storsimple-manager-service-administration.md).
