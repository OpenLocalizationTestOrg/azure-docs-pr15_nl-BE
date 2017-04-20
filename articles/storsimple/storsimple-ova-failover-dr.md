<properties
   pageTitle="Herstel na storing herstel- en failover voor uw virtuele matrix StorSimple"
   description="Meer informatie over hoe u failover uw virtuele matrix StorSimple."
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
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>Herstel na storing herstel- en failover voor uw virtuele matrix StorSimple


## <a name="overview"></a>Overzicht

Het herstel na noodgevallen voor uw Microsoft Azure StorSimple Virtual Array (ook bekend als de StorSimple op ruimten virtueel apparaat) met inbegrip van de gedetailleerde stappen vereist om failover uitgevoerd naar een ander virtueel apparaat een storing beschreven. Een failover kunt u uw gegevens migreren van een *bron* -apparaat in het datacenter naar *een ander apparaat zich bevindt in dezelfde of een andere geografische locatie* . De overname van het apparaat is voor het gehele apparaat. Tijdens een overname, de cloud-gegevens voor de Bronapparaat verandert eigendom van het doelapparaat.

Failover van apparaat is ingedeeld met de functie disaster recovery (DR) en wordt geïnitieerd vanaf de pagina **apparaten** . Deze pagina registreert alle StorSimple apparaten verbonden met uw service Manager StorSimple. De beschrijvende naam, status, ingerichte en maximale capaciteit, type en model worden voor elk apparaat weergegeven.

![](./media/storsimple-ova-failover-dr/image15.png)

Dit artikel is alleen van toepassing op virtuele StorSimple-Arrays. Failover van een apparaat 8000-serie, gaat u naar [Disaster Recovery van uw apparaat StorSimple en overname bij storingen](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Wat is noodherstel?

In een scenario disaster recovery (DR), het primaire apparaat uitvalt. In dit geval kunt u de cloud-gegevens die zijn gekoppeld aan het defecte apparaat naar een ander apparaat met behulp van het primaire apparaat als de *bron* en een ander apparaat opgeven als het *doel*te verplaatsen. Dit proces wordt *failover*genoemd. Tijdens een overname, alle volumes of de aandelen van het bronapparaat eigendom wijzigen en worden overgebracht naar het doelapparaat. Geen filtering van de gegevens is toegestaan.

DR wordt gemodelleerd als een volledige apparaat terugzetten met behulp van de warmte op basis van toewijzing trapsgewijs schakelen en te traceren. Een warmte-kaart wordt gedefinieerd door een calorische waarde toewijzen aan de gegevens die zijn gebaseerd op lezen en schrijven van patronen. Deze warmte toewijzen vervolgens lagen de laagste hitte gegevenssegmenten naar de cloud eerst de gegevenssegmenten hoge temperaturen (meest gebruikte) in de lokale laag te houden. Tijdens een DR, wordt de warmte-kaart gebruikt om te zetten en de gegevens van de wolk rehydrate. Het apparaat alle volumes/aandelen in de laatste recente back-up (zoals intern bepaald) kan worden opgehaald en terugzetten van back-up uitvoert. Het hele proces van DR is door het apparaat ingedeeld.


## <a name="prerequisites-for-device-failover"></a>Vereisten voor failover-apparaat


### <a name="prerequisites"></a>Vereisten

Voor een failover-apparaat, moeten aan de volgende vereisten worden voldaan:

- De Bronapparaat moet een status **gedeactiveerd** .

- Het doelapparaat moet weergeven als **actief** in de klassieke Azure portal. Moet u een virtueel apparaat van de dezelfde of een hogere capaciteit te creëren. Vervolgens moet u het lokale web UI te configureren en te registreren is het virtuele apparaat gebruiken.

    > [AZURE.IMPORTANT] Probeer niet de geregistreerde virtueel apparaat via de service configureren door te klikken op **complete apparaat instellen**. Er is geen apparaatconfiguratie moet worden uitgevoerd via de service.

- De bron- en doel-apparaat moet hetzelfde type. U kunt een virtueel apparaat geconfigureerd als bestandsserver naar een ander bestandsserver alleen failover. Hetzelfde geldt voor een iSCSI-server.

- Voor een bestandsserver DR, raden wij u aan het doelapparaat naar hetzelfde domein als dat van de bron te koppelen zodat de sharemachtigingen automatisch opgelost worden. Failover naar een doelapparaat in hetzelfde domein wordt ondersteund in deze release.

### <a name="other-considerations"></a>Andere overwegingen

- Wij raden aan dat u alle volumes of -shares op het bronapparaat off line neemt.

- Als een geplande failover wordt aangeraden dat u een back-up van het apparaat en ga vervolgens verder met de failover-functie om gegevensverlies te minimaliseren. Als het een niet-geplande failover, wordt de meest recente back-up terug te zetten van het apparaat worden gebruikt.

- De apparaten beschikbaar voor DR zijn apparaten die de dezelfde of een grotere ten opzichte van het bronapparaat capaciteit. De apparaten die zijn verbonden met uw service, maar die niet voldoen aan de criteria van voldoende ruimte is alleen beschikbaar als de doelapparaten.

### <a name="dr-prechecks"></a>DR prechecks

Voordat u begint met de DR, worden prechecks op het apparaat uitgevoerd. Deze controles ervoor te zorgen dat er geen fouten DR is gebracht, zal plaatsvinden. De prechecks omvatten:

- De opslag-account te valideren

- De verbinding met de cloud naar Azure controleren

- Beschikbare ruimte op het doelapparaat controleren

- Controleren of een iSCSI-server Bronapparaat een geldige naam van ACR heeft, IQN (niet meer dan 220 tekens) en CHAP het wachtwoord (12 en 16 tekens) die is gekoppeld aan de volumes

Als een van de bovenstaande prechecks mislukt, kunt niet u doorgaan met de DR. U moet deze kwesties op en probeer DR.

Nadat de DR is voltooid, wordt de eigendom van de wolkengegevens op de Bronapparaat overgebracht naar het doelapparaat. De Bronapparaat is niet meer beschikbaar in de portal. Toegang tot alle volumes/aandelen op het bronapparaat is geblokkeerd en het doelapparaat wordt geactiveerd.

> [AZURE.IMPORTANT]
> 
> Als het apparaat niet meer beschikbaar is, wordt de virtuele machine die u op het hostsysteem ingericht nog bronnen verbruiken. Zodra de DR met succes is voltooid is, kunt u deze virtuele machine verwijderen uit uw hostsysteem.

## <a name="fail-over-to-a-virtual-array"></a>Een failover uitvoeren naar een virtuele matrix

Wij raden aan dat u beschikt over een andere StorSimple virtuele matrix ingericht, via het lokale web UI geconfigureerd en geregistreerd bij de StorSimple Manager-service voordat u deze procedure uitvoert.


> [AZURE.IMPORTANT]
> 
> - U mag geen failover uitvoeren vanaf een apparaat StorSimple 8000-serie naar een virtueel apparaat 1200.
> - U kan worden uitgevoerd vanaf een virtueel apparaat geïmplementeerd in regering portal met een virtueel apparaat in Azure klassieke portal van Federal Information Processing Standard (FIPS) ingeschakeld. Het omgekeerde geldt ook.

Voer de volgende stappen uit om het apparaat op een virtueel apparaat StorSimple terugzetten.

1. Volumes/aandelen off line nemen op de host. Raadpleeg de instructies van het besturingssysteem-specifieke op de host off line nemen van de volumes/aandelen. Als dit niet al off line is, moet u alle volumes/aandelen off line op het apparaat in handen nemen **apparaten > aandelen** (of **apparaat > Volumes**). Selecteer een sharevolume en klik op **off line nemen** aan de onderkant van de pagina. Klik op **Ja**ter bevestiging. Herhaal dit proces voor alle aandelen/volumes op het apparaat.

2. Selecteer de voor failover-Bronapparaat op de pagina **apparaten** en klik op **deactiveren**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. U wordt om bevestiging gevraagd. Deactivering apparaat is een permanent proces kan niet ongedaan worden gemaakt. U wordt ook herinnerd te nemen van uw off line aandelen/volumes op de host.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Na bevestiging wordt door wordt de deactivering gestart. Nadat de deactivering is voltooid, wordt u krijgt.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Klik op de pagina **apparaten** verandert de status van het apparaat nu naar **gedeactiveerd**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Selecteer het uitgeschakelde apparaat en klik op **Failover**onderaan de pagina.

6. In de bevestigen failover-wizard die wordt geopend door het volgende te doen:

    1. Kies in de vervolgkeuzelijst met beschikbare apparaten, een **doelapparaat.** Alleen de apparaten die voldoende capaciteit worden in de vervolgkeuzelijst weergegeven.

    2. Bekijk de details van het bronapparaat zoals apparaatnaam en totale capaciteit, de namen van de shares die worden overgenomen.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. **Ik ga akkoord dat failover niet ongedaan maken en het bronapparaat wordt verwijderd nadat de failover is voltooid,**controleren.

8. Klik op het pictogram voor ![](./media/storsimple-ova-failover-dr/image1.png).


9. Een failover-taak wordt gestart en u wordt gewaarschuwd. Klik op **taak weergeven** als u wilt controleren de failover-functie.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. Op de pagina **taken** ziet u een failover-taak gemaakt voor de Bronapparaat. Deze taak voert de DR prechecks.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Nadat de prechecks DR geslaagd zijn, zal de failover-functie brengen terugzettaken voor elk aandeel/volume dat op het bronapparaat bestaat gang.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Nadat de failover is uitgevoerd, gaat u naar de pagina **apparaten** .

    een. Selecteer de virtuele StorSimple-apparaat dat is gebruikt als het doelapparaat voor de failover-procedure.

    b. Ga naar de pagina van **aandelen** (of - **Volumes** als iSCSI-server). Alle aandelen (volumes) van het oude apparaat moeten nu worden weergegeven.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Video beschikbaar**

In deze video laat zien hoe u kunt niet via StorSimple op ruimten virtueel apparaat naar een ander virtueel apparaat.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Business continuity noodherstel (BCDR)

Een business continuity (BCDR) herstelscenario treedt op wanneer het hele Azure datacenter uitvalt. Dit kan invloed hebben op uw StorSimple Manager-service en de bijbehorende StorSimple apparaten.

Als er StorSimple apparaten die zijn geregistreerd voordat een ramp is opgetreden, kunnen deze apparaten StorSimple moet worden verwijderd. Na de ramp zich voordoet, kunt u opnieuw maken en configureren van deze apparaten.

## <a name="errors-during-dr"></a>Fouten bij DR

**Wolk verbinding uitvalt tijdens DR**

Als de wolk connectiviteit wordt onderbroken wanneer Dr. Watson is gestart en voordat het apparaat herstellen voltooid is, de DR mislukt en u krijgt. Het doelapparaat dat is gebruikt voor de DR is wordt gemarkeerd als *onbruikbaar.* Hetzelfde doelapparaat kan niet klik worden gebruikt voor toekomstige DRs.

**Geen compatibele apparaten**

Als de beschikbare apparaten niet voldoende ruimte hebt, ziet u een fout op het effect dat er geen apparaten compatibel zijn.

**Controle vooraf storingen**

Als een van de prechecks niet is voldaan, ziet u fouten controle vooraf.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple Virtual Array met behulp van het lokale web UI](storsimple-ova-web-ui-admin.md).
