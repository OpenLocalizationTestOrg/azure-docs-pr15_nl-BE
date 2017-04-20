<properties 
   pageTitle="Uitschakelen en verwijderen van een apparaat StorSimple | Microsoft Azure"
   description="Beschrijving van het StorSimple apparaat van de service verwijderen door deze eerst te deactiveren en het vervolgens te verwijderen."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Uitschakelen en verwijderen van een apparaat StorSimple

## <a name="overview"></a>Overzicht

U wilt een StorSimple apparaat buiten dienst te nemen (bijvoorbeeld als u uw apparaat bijwerken of vervangen of als u niet meer StorSimple). Als dit het geval is, moet u het apparaat uitschakelen voordat u deze kunt verwijderen. Deactiveren, verbreekt de verbinding tussen het apparaat en de bijbehorende StorSimple Manager-service. In deze zelfstudie wordt uitgelegd hoe een apparaat StorSimple uit de service verwijderen door deze eerst te deactiveren en het vervolgens te verwijderen. 

Wanneer u een apparaat uitschakelt, zijn alle gegevens die lokaal is opgeslagen op het apparaat niet meer worden gebruikt. Alleen de gegevens die zijn gekoppeld aan het apparaat dat is opgeslagen in de cloud kunnen worden hersteld.  

>[AZURE.WARNING] Deactiveren niet ongedaan maken en kan niet ongedaan worden gemaakt. Een gedeactiveerde apparaat kan niet worden geregistreerd met de service Manager StorSimple tenzij het eerst opnieuw wordt ingesteld door de fabriek. 
>
>De factory reset proces verwijdert alle gegevens die lokaal is opgeslagen op uw apparaat. Daarom is het essentieel dat u een momentopname wolk van al uw gegevens voordat u een apparaat deactiveren. Hierdoor kunt u alle gegevens in een later stadium te herstellen.

In deze zelfstudie wordt uitgelegd hoe u kunt:

- Een apparaat deactiveren en de gegevens verwijderen
- Een apparaat deactiveren en bewaren van de gegevens

Ook wordt uitgelegd hoe deactiveren en te verwijderen op een virtueel apparaat StorSimple werkt.

>[AZURE.NOTE] Voordat u een StorSimple fysiek of virtueel apparaat deactiveren, moet stoppen of verwijderen van clients en hosts die afhankelijk van dat apparaat zijn.

## <a name="deactivate-and-delete-data"></a>Deactiveren en gegevens verwijderen

Als u geïnteresseerd bent in het volledig verwijderen van het apparaat niet wilt bewaren van de gegevens op het apparaat, vervolgens de volgende stappen.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Het apparaat uitschakelen en verwijderen van de gegevens  

1. Voor een apparaat deactiveren, moet u het volume alle containers (en de volumes) die is gekoppeld aan het apparaat. Nadat u de bijbehorende back-ups hebt verwijderd, kunt u containers volume verwijderen.

2. Het apparaat als volgt uitschakelen:

    1. Selecteer het apparaat dat u wilt deactiveren en klik onderaan de pagina op **deactiveren**op de pagina StorSimple Manager service- **apparaten** .

    2. Er wordt een bevestigingsbericht weergegeven. Klik op **Ja** om door te gaan. Het proces deactiveren start en een paar minuten duren.

3. Na deactiveren, kunt u het apparaat volledig verwijderen. Een apparaat verwijdert, wordt deze uit de lijst met apparaten die zijn verbonden met de service. De service kan niet meer dan het verwijderde apparaat beheren. Gebruik de volgende stappen uit om het apparaat te verwijderen:

    1. Selecteer op de pagina StorSimple Manager service **apparaten** een gedeactiveerde apparaat dat u wilt verwijderen.

    2. Klik onderaan op de pagina **verwijderen**.

    3. U wordt om bevestiging gevraagd. Klik op **Ja** om door te gaan.

    Duurt het enkele minuten duren voordat het apparaat moet worden verwijderd.

## <a name="deactivate-and-retain-data"></a>Deactiveren en bewaren van gegevens

Als u bent geïnteresseerd in het apparaat te verwijderen, maar u wilt bewaren van de gegevens, vervolgens de volgende stappen.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Een apparaat deactiveren en bewaren van de gegevens 

1. Het apparaat uitschakelen. Alle containers van het volume en de momentopnamen van het apparaat blijven.

    1. Selecteer het apparaat dat u wilt deactiveren en klik onderaan de pagina op **deactiveren**op de pagina StorSimple Manager service- **apparaten** .

    2. Er wordt een bevestigingsbericht weergegeven. Klik op **Ja** om door te gaan. Het proces deactiveren start en een paar minuten duren.

2. U kunt nu het volume containers en de bijbehorende momentopnamen failover. Ga naar [Failover en disaster recovery voor uw apparaat StorSimple](storsimple-device-failover-disaster-recovery.md)voor procedures.

3. U kunt het apparaat volledig verwijderen na deactiveren en overname bij storingen. Een apparaat verwijdert, wordt deze uit de lijst met apparaten die zijn verbonden met de service. De service kan niet meer dan het verwijderde apparaat beheren. Voer de volgende stappen uit om het apparaat te verwijderen:
 
    1. Selecteer op de pagina StorSimple Manager service **apparaten** een gedeactiveerde apparaat dat u wilt verwijderen.

    2. Klik onderaan op de pagina **verwijderen**.

    3. U wordt om bevestiging gevraagd. Klik op **Ja** om door te gaan.

    Duurt het enkele minuten duren voordat het apparaat moet worden verwijderd.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deactiveren en een virtueel apparaat verwijderen

Voor een virtueel apparaat StorSimple deallocates deactiveren de virtuele machine. Vervolgens kunt u de virtuele machine en de bronnen die zijn gemaakt toen het werd ingericht verwijderen. Nadat het virtuele apparaat is gedeactiveerd, kan deze niet worden hersteld naar de vorige staat. 

De resultaten van de deactiveren in de volgende acties:

- Het virtuele apparaat StorSimple wordt verwijderd.

- De OSDisk en gemaakt voor het virtuele apparaat StorSimple gegevensschijven zijn verwijderd.

- De Service gehost en virtuele netwerken die zijn gemaakt tijdens het inrichten blijven behouden. Als u deze entiteiten niet gebruikt, moet u deze handmatig verwijderen.

- Wolk momentopnamen gemaakt door het virtuele apparaat StorSimple blijven behouden.

## <a name="next-steps"></a>Volgende stappen
- Wilt het uitgeschakelde apparaat terugzetten naar fabrieksinstellingen, gaat u naar [het apparaat standaardinstellingen opnieuw instellen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Voor technische ondersteuning [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).

- Voor meer informatie over het gebruik van de StorSimple Manager-service, gaat u naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md). 
