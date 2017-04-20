<properties 
   pageTitle="Uitschakelen en verwijderen van een virtuele matrix StorSimple | Microsoft Azure"
   description="Beschrijving van het StorSimple apparaat van de service verwijderen door deze eerst te deactiveren en het vervolgens te verwijderen."
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
   ms.date="06/20/2016"
   ms.author="alkohli" />

# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Deactiveren en een virtuele matrix StorSimple verwijderen

## <a name="overview"></a>Overzicht

Als u een virtuele matrix StorSimple deactiveert, verbreekt u de verbinding tussen het apparaat en de bijbehorende StorSimple Manager-service. Deactiveren niet ongedaan maken en kan niet ongedaan worden gemaakt. Een gedeactiveerde apparaat kan niet worden geregistreerd in de StorSimple Manager-service opnieuw.

Wellicht moet u uitschakelen en verwijderen van een virtueel apparaat StorSimple in de volgende scenario's:


- Uw apparaat is online en u van plan bent over dit apparaat is mislukt. Wellicht moet u dit doen als u van plan bent om te upgraden naar een groter apparaat. Nadat het apparaatgegevens worden overgedragen en de failover uitgevoerd is, kunt u het apparaat vervolgens verwijderen.

- Het apparaat offline is en u van plan bent over dit apparaat is mislukt. Dit kan gebeuren in geval van een ramp waar als gevolg van een storing in het datacenter, het primaire apparaat uitgeschakeld is. U van plan bent het apparaat op een secundaire apparaat failover. Nadat het apparaatgegevens worden overgedragen en de failover uitgevoerd is, kunt u het apparaat verwijderen.

- Wilt u schaft u het apparaat en vervolgens te verwijderen. 
 

Wanneer u een apparaat uitschakelt, zijn alle gegevens die lokaal is opgeslagen niet meer worden gebruikt. Alleen de gegevens die zijn opgeslagen in de cloud kunnen worden hersteld. Als u van plan bent om de apparaatgegevens na deactiveren, moet u een momentopname van een wolk van al uw gegevens nemen voordat u een apparaat deactiveren. Hierdoor kunt u alle gegevens in een later stadium te herstellen.


In deze zelfstudie wordt uitgelegd hoe u kunt:

- Een apparaat deactiveren 
- Een gedeactiveerde apparaat verwijderen


## <a name="deactivate-a-device"></a>Een apparaat deactiveren

Voer de volgende stappen uit om te deactiveren van uw apparaat.

#### <a name="to-deactivate-the-device"></a>Het apparaat deactiveren   

1. Ga naar de pagina **apparaten** . Selecteer het apparaat dat u wilt deactiveren.

    ![Selecteer apparaat deactiveren](./media/storsimple-ova-deactivate-and-delete-device/deactivate1m.png)

3. Klik onderaan de pagina op **deactiveren**.

    ![Klik op deactiveren](./media/storsimple-ova-deactivate-and-delete-device/deactivate2m.png)

4. Er wordt een bevestigingsbericht weergegeven. Klik op **Ja** om door te gaan. 

    ![Bevestig deactiveren](./media/storsimple-ova-deactivate-and-delete-device/deactivate3m.png)

    Het proces deactiveren start en een paar minuten duren.

    ![Deactiveren in uitvoering](./media/storsimple-ova-deactivate-and-delete-device/deactivate4m.png)

3. Na deactiveren, worden de lijst van de apparaten vernieuwd. 

    ![Volledige deactiveren](./media/storsimple-ova-deactivate-and-delete-device/deactivate5m.png)

    U kunt dit apparaat verwijderen. 

## <a name="delete-the-device"></a>Het apparaat verwijderen

Een apparaat moet eerst worden gedeactiveerd te verwijderen. Een apparaat verwijdert, wordt deze uit de lijst met apparaten die zijn verbonden met de service. De service kan niet meer dan het verwijderde apparaat beheren. De gegevens die zijn gekoppeld aan het apparaat blijft echter in de cloud. Let erop dat deze gegevens vervolgens toeslagen zullen toenemen. 

Voer de volgende stappen uit om het apparaat te verwijderen:

#### <a name="to-delete-the-device"></a>Het apparaat verwijderen 

 1. Selecteer op de pagina StorSimple Manager service **apparaten** een gedeactiveerde apparaat dat u wilt verwijderen.

    ![Selecteer het apparaat verwijderen](./media/storsimple-ova-deactivate-and-delete-device/deactivate5m.png)

 2. Klik onderaan op de pagina **verwijderen**.
 
    ![Klik op verwijderen](./media/storsimple-ova-deactivate-and-delete-device/deactivate6m.png)

 3. U wordt om bevestiging gevraagd. Typ de naam van het apparaat te verwijderen van apparaat bevestigen. Houd er rekening mee dat het apparaat niet verwijdert, worden de cloud-gegevens die zijn gekoppeld aan het apparaat. Klik op het pictogram om door te gaan.
 
    ![Verwijderen bevestigen](./media/storsimple-ova-deactivate-and-delete-device/deactivate7m.png) 

 5. Duurt het enkele minuten duren voordat het apparaat moet worden verwijderd. 

    ![Bezig met verwijderen](./media/storsimple-ova-deactivate-and-delete-device/deactivate8m.png)

    Nadat het apparaat is verwijderd, kunt u de lijst met apparaten wordt vernieuwd.

    ![Volledig verwijderen](./media/storsimple-ova-deactivate-and-delete-device/deactivate9m.png)


## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over het gebruik van de StorSimple Manager-service, gaat u naar [de StorSimple Manager-service voor het beheren van uw virtuele matrix StorSimple gebruiken](storsimple-ova-manager-service-administration.md). 