<properties 
   pageTitle="De StorSimple virtueel apparaat admin-wachtwoord wijzigen | Microsoft Azure"
   description="Beschrijving van het apparaat administrator-wachtwoord wijzigen met de klassieke Azure portal of het virtuele matrix StorSimple web UI."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>Het virtuele matrix StorSimple apparaat administrator-wachtwoord wijzigen

## <a name="overview"></a>Overzicht

Wanneer u de Windows PowerShell-interface voor toegang tot het virtuele apparaat StorSimple, moet u een beheerderswachtwoord apparaat opgeven. Wanneer het apparaat StorSimple eerst is ingericht en gestart, is het standaardwachtwoord *Wachtwoord1*. Het standaardwachtwoord is verlopen voor de beveiliging van uw gegevens de eerste keer dat u zich aanmeldt en u moet dit wachtwoord wijzigen.

U kunt ook het lokale web UI of de klassieke Azure portal apparaat administrator-wachtwoord wijzigen op elk gewenst moment nadat het apparaat is geïmplementeerd in uw productieomgeving. Elk van deze procedures wordt in dit artikel beschreven.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>De klassieke Azure portal gebruiken om het wachtwoord te wijzigen

Voer de volgende stappen uit als u wilt wijzigen van het beheerderswachtwoord apparaat via de klassieke Azure portal.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Wijzig het wachtwoord van de beheerder apparaat via de klassieke Azure portal

1. Klik op **apparaten**in de portal, > -**configuratie** voor het apparaat.

2. Ga naar de sectie **Apparaat Administrator-wachtwoord** . Een administrator-wachtwoord met van 8 tot 15 tekens bevatten. Het wachtwoord moet uit een combinatie van hoofdletters, kleine letters, cijfers en speciale tekens.

3. Bevestig het wachtwoord.

4. Klik op **Opslaan** onder aan de pagina.

Het beheerderswachtwoord voor het apparaat moet nu worden bijgewerkt. U kunt deze gewijzigde wachtwoord toegang tot het apparaat lokaal.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>De webgebruikersinterface StorSimple virtuele matrix gebruiken om het wachtwoord te wijzigen

Voer de volgende stappen uit als u wilt wijzigen van het administrator-wachtwoord van het apparaat via het lokale web UI.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Het administrator-wachtwoord van het apparaat via het lokale web UI wijzigen

1. Klik op **onderhoud**in het lokale web UI > **wachtwoord wijzigen** voor uw apparaat.

    ![Wachtwoord1 wijzigen](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Voer het **huidige wachtwoord**.

3. Een **Nieuw wachtwoord**opgeven. Het wachtwoord moet minimaal 8 tekens lang zijn. 3 van 4 van de volgende moet bevatten: hoofdletters, kleine letters, cijfers en speciale tekens.

    Houd er rekening mee dat uw wachtwoord kan niet hetzelfde als de laatste 24 wachtwoorden zijn.

3. Geef het wachtwoord ter bevestiging opnieuw.

    ![Wachtwoord2 wijzigen](./media/storsimple-ova-change-device-admin-password/image41.png)

4. Klik op **toepassen**onder aan de pagina. Het nieuwe wachtwoord wordt vervolgens toegepast. Als het wijzigen van het wachtwoord niet is gelukt, ziet u de volgende fout.

    ![Wachtwoordfout](./media/storsimple-ova-change-device-admin-password/image42.png)

    Nadat het wachtwoord is bijgewerkt, wordt je bericht. Vervolgens kunt u deze gewijzigde wachtwoord voor toegang tot het apparaat lokaal.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md).
