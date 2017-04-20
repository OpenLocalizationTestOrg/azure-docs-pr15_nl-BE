<properties 
   pageTitle="Wijzig uw wachtwoorden StorSimple | Microsoft Azure" 
   description="Beschrijving van de service Manager StorSimple uw StorSimple Snapshot Manager en apparaat administrator-wachtwoord wijzigen." 
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
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Gebruik van de service Manager StorSimple StorSimple wachtwoorden wijzigen

## <a name="overview"></a>Overzicht 

De klassieke Azure portal pagina **configureren** bevat alle apparaatparameters die u kunt configureren op een StorSimple apparaat dat wordt beheerd door een StorSimple Manager-service. In deze zelfstudie wordt uitgelegd hoe u kunt de pagina **configureren** om de beheerder van uw apparaat of StorSimple Snapshot Manager-wachtwoord te wijzigen.

## <a name="change-the-device-administrator-password"></a>Het apparaat administrator-wachtwoord wijzigen

Wanneer u Windows PowerShell-interface gebruiken voor toegang tot het apparaat StorSimple, moet u een beheerderswachtwoord apparaat opgeven. Wanneer de eerste StorSimple apparaat wordt geregistreerd bij een service, is het standaardwachtwoord voor deze interface *Wachtwoord1*. U moet dit wachtwoord aan het einde van het registratieproces wijzigen voor de beveiliging van uw gegevens. U kunt niet afsluiten van het registratieproces zonder dit wachtwoord te wijzigen. Zie voor meer informatie [stap 3: configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Het wachtwoord dat u eerst via de Windows PowerShell-interface is ingesteld tijdens de registratie kan vervolgens worden gewijzigd via de klassieke Azure portal. Voer de volgende stappen uit om het apparaat administrator-wachtwoord wijzigen.

#### <a name="to-change-the-device-administrator-password"></a>Het apparaat administrator-wachtwoord wijzigen

1. Klik op **apparaten**in de klassieke portal > **configureren** voor uw apparaat.

2. Ga naar de sectie **Apparaat Administrator-wachtwoord** . Een administrator-wachtwoord met van 8 tot 15 tekens bevatten. Het wachtwoord moet uit een combinatie van 3 of meer hoofdletters, kleine letters, cijfers en speciale tekens.

3. Bevestig het wachtwoord.

4. Klik op **Opslaan** onder aan de pagina.

Het beheerderswachtwoord voor het apparaat moet nu worden bijgewerkt. Dit gewijzigde wachtwoord kunt u toegang krijgen tot de Windows PowerShell-interface.

## <a name="change-the-storsimple-snapshot-manager-password"></a>De StorSimple Snapshot Manager-wachtwoord wijzigen

StorSimple Snapshot Manager software zich bevindt op de Windows-host en beheerders kunnen back-ups maken van uw apparaat StorSimple in de vorm van lokale en momentopnamen van de wolk.

Wanneer u een apparaat in StorSimple Snapshot Manager configureert, wordt u gevraagd om het IP-adres en een wachtwoord opgeven voor het verifiëren van uw opslagapparaat. Dit wachtwoord is eerst via de Windows PowerShell-interface geconfigureerd. Zie voor meer informatie [stap 3: configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Het wachtwoord dat u eerst via de Windows PowerShell-interface is ingesteld tijdens de registratie kan vervolgens worden gewijzigd via de klassieke portal. Voer de volgende stappen uit om de StorSimple Snapshot Manager-wachtwoord te wijzigen.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>De StorSimple Snapshot Manager-wachtwoord wijzigen

1. Klik op **apparaten**in de klassieke portal > **configureren** voor uw apparaat.

2. Ga naar de sectie **StorSimple Snapshot Manager** . Voer een wachtwoord van 14 of 15 tekens. Zorg ervoor dat het wachtwoord een combinatie van 3 of meer hoofdletters, kleine letters, cijfers en speciale tekens bevat.

3. Bevestig het wachtwoord.

4. Klik op **Opslaan** onder aan de pagina.

Het wachtwoord StorSimple Snapshot Manager moet nu worden bijgewerkt.
 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [beveiliging van StorSimple](storsimple-security.md).

- Meer informatie over het [wijzigen van de apparaatconfiguratie](storsimple-modify-device-config.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
