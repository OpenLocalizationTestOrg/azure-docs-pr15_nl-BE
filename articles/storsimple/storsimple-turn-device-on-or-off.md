<properties 
   pageTitle="Het apparaat StorSimple inschakelen of uitschakelen | Microsoft Azure"
   description="Wordt uitgelegd hoe u een nieuw StorSimple apparaat inschakelen en uitschakelen van een actief apparaat uitschakelen op een apparaat dat is afgesloten of stroomvoorziening."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Het apparaat StorSimple inschakelen of uitschakelen 

## <a name="overview"></a>Overzicht

Een apparaat met Microsoft Azure StorSimple afsluiten is niet vereist als onderdeel van de normale regeling. U wilt echter een nieuw apparaat of een apparaat dat moest worden afgesloten. In het algemeen is een afsluiting vereist in gevallen waarin u moet vervangen van defecte hardware, fysiek verplaatsen van een eenheid of een apparaat buiten dienst te nemen. In deze zelfstudie wordt de vereiste procedure voor het inschakelen en het afsluiten van het apparaat StorSimple in verschillende scenario's beschreven.

De volgende tabel geeft een overzicht van verschillende scenario's voor het inschakelen en het afsluiten van het apparaat StorSimple en koppelingen naar de juiste procedures.

|Scenario|Naslagonderwerpen|
|:-------|:---------------|
|Een nieuw apparaat inschakelen|[Een nieuw apparaat inschakelen](#turn-on-a-new-device)<ul><li>[Nieuw apparaat met alleen de primaire behuizing](#new-device-with-primary-enclosure-only)</li><li>[Nieuw apparaat met EBOD behuizing](#new-device-with-ebod-enclosure)</li></ul>|
|Een apparaat inschakelen na het afsluiten|[Een apparaat inschakelen na het afsluiten](#turn-on-a-device-after-shutdown)<ul><li>[Apparaat met alleen de primaire behuizing](#device-with-primary-enclosure-only)</li><li>[Apparaat met EBOD behuizing](#device-with-ebod-enclosure)</li></ul>|
|Een apparaat inschakelen na stroomuitval|[Een apparaat inschakelen na stroomuitval](#turn-on-a-device-after-a-power-loss)<ul><li>[Apparaat met alleen de primaire behuizing](#8100)</li><li>[Apparaat met EBOD behuizing](#8600)</li></ul>|
|Een apparaat inschakelen nadat de primaire behuizing en EBOD verbinding wordt verbroken|[Een apparaat inschakelen nadat de primaire en EBOD behuizing verbinding wordt verbroken](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Een actief apparaat afgesloten|[Een actief apparaat uitschakelen](#turn-off-a-running-device)<ul><li>[Apparaat met alleen de primaire behuizing](#8100a)</li><li>[Apparaat met EBOD behuizing](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Een nieuw apparaat inschakelen

De stappen voor het inschakelen van een apparaat StorSimple voor de eerste keer is afhankelijk van of het apparaat een 8100 of een model 8600. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing met een primaire behuizing en een EBOD behuizing is. De gedetailleerde stappen voor beide modellen worden behandeld in de volgende secties.

- [Nieuw apparaat met alleen de primaire behuizing](#new-device-with-primary-enclosure-only)

- [Nieuw apparaat met EBOD behuizing](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Nieuw apparaat met alleen de primaire behuizing

Het model 8100 StorSimple is een apparaat met één behuizing. Het apparaat bevat redundante voeding en koeling Modules (PCMs). Beide PCMs worden geïnstalleerd en aangesloten op andere energiebronnen, hoge beschikbaarheid garanderen.

Voer de volgende stappen uit om het apparaat voor power kabel.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Ga voor complete inrichting, installatie en bekabeling van instructies, om uw StorSimple 8100-apparaat te [installeren](storsimple-8100-hardware-installation.md). Zorg ervoor dat u de instructies exact.

### <a name="new-device-with-ebod-enclosure"></a>Nieuw apparaat met EBOD behuizing

Het model StorSimple 8600 heeft zowel een primaire behuizing en een EBOD behuizing. Moet hiervoor de eenheden die samen worden kabels voor Serial Attached SCSI (SAS) verbinding en geen stroom.

Wanneer u dit apparaat voor de eerste keer instelt, voer de stappen uit voor SAS kabels eerst en vervolgens de stappen voor power kabels.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Ga voor complete inrichting, installatie en bekabeling van instructies, om uw StorSimple 8600-apparaat te [installeren](storsimple-8600-hardware-installation.md). Zorg ervoor dat u de instructies exact.

## <a name="turn-on-a-device-after-shutdown"></a>Een apparaat inschakelen na het afsluiten

De stappen voor het inschakelen van een apparaat StorSimple nadat deze is afgesloten, zijn afhankelijk van of het apparaat een 8100 of een model 8600. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing met een primaire behuizing en een EBOD behuizing is.

- [Apparaat met alleen de primaire behuizing](#device-with-primary-enclosure-only)

- [Apparaat met EBOD behuizing](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Apparaat met alleen de primaire behuizing

Na een afsluiting, gebruik de volgende procedure om te schakelen op een apparaat StorSimple met een primaire behuizing en geen EBOD behuizing.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Een apparaat met een primaire behuizing alleen inschakelen

1. Zorg ervoor dat de stroom op beide Power wordt en koeling Modules (PCMs) in de OFF-stand zijn. Als u de schakelopties zijn niet in de OFF-stand, naar de OFF-positie spiegelen en wacht tot de lampjes om uit te gaan.

2. Schakel het apparaat door de netschakelaars op beide PCMs naar de ON positie spiegelen. Het apparaat in te schakelen.

3. Controleer het volgende als u wilt controleren of het apparaat is volledig:

    1. De OK-LED's op beide PCM-modules zijn groen.

    2. De status-LED's op beide domeincontrollers zijn groen.

    3. Op een van de blauwe LED knippert, waarmee wordt aangegeven dat de domeincontroller actief is.

    Als een van deze voorwaarden niet wordt voldaan, vervolgens is het apparaat beschadigd. Neem [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Apparaat met EBOD behuizing

Na een afsluiting, gebruik de volgende procedure om te schakelen op een apparaat StorSimple met een primaire behuizing en een EBOD behuizing. Voer elke stap in volgorde precies zoals beschreven. Niet doet kan leiden tot verlies van gegevens.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Om te schakelen op een apparaat met een primaire en een EBOD-behuizing

1. Zorg ervoor dat de ruimte EBOD is aangesloten op de primaire behuizing. Zie [de StorSimple 8600-apparaat installeren](storsimple-8600-hardware-installation.md)voor meer informatie.

2. Zorg ervoor dat de voeding en koeling Modules (PCMs) op de EBOD en de primaire behuizingen in de OFF-stand zijn. Als u de schakelopties zijn niet in de OFF-stand, naar de OFF-positie spiegelen en wacht tot de lampjes om uit te gaan.

3. De behuizing van de EBOD eerst inschakelen door de netschakelaars op beide PCMs naar de ON positie spiegelen. De PCM-LED's moeten groen zijn. Een groen EBOD controller LED op deze eenheid geeft aan dat de behuizing van de EBOD op.

4. De primaire behuizing inschakelen door de netschakelaars op beide PCMs naar de ON positie spiegelen. Het hele systeem worden nu op.

5. Controleer of de SAS-LED's groen, die ervoor zorgt dat de verbinding tussen de EBOD-behuizing en de primaire goed is.

## <a name="turn-on-a-device-after-a-power-loss"></a>Een apparaat inschakelen na stroomuitval

Een stroomstoring of een onderbreking kan een apparaat StorSimple afsluiten. De stroomstoring kan gebeuren op een van de voedingseenheden of beide voedingen. De stappen zijn afhankelijk van of het apparaat een model 8100 of een 8600 is. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing met een primaire behuizing en een EBOD behuizing is. In deze sectie wordt de herstelprocedure voor elk scenario beschreven.

- [Apparaat met alleen de primaire behuizing](#8100)

- [Apparaat met EBOD behuizing](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Apparaat met alleen de primaire behuizing<a name="8100">

Het systeem kan de normale werking doorgaan als er een stroomstoring op een van de voedingseenheden. Echter de beschikbaarheid van het apparaat, zodat terugzetten power voor de voeding zo spoedig mogelijk.

Als er een stroomstoring of een stroomstoring op beide voedingen, zal het systeem zich in een ordelijke, gecontroleerde wijze afgesloten. Het systeem wordt automatisch ingeschakeld wanneer de stroomtoevoer is hersteld.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Apparaat met EBOD behuizing<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Stroomstoring op een power supply

Het systeem kan de normale werking doorgaan als er een stroomstoring op een van de voedingen op de primaire behuizing of op de behuizing van de EBOD. Echter om maximale beschikbaarheid van het apparaat, zet kracht voor de voeding zo spoedig mogelijk.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Stroomstoring op beide voedingen op primaire en EBOD-behuizingen

Als er een stroomstoring of een stroomstoring op beide voedingen, de behuizing van de EBOD onmiddellijk wordt afgesloten en de primaire ruimte op een ordelijke en gecontroleerde wijze wordt afgesloten. Wanneer de stroomtoevoer is hersteld, wordt het toestel automatisch gestart.

Als het energiebeheerschema handmatig is uitgeschakeld, nemen vervolgens de volgende stappen uit om de macht om het systeem te herstellen.

1. Schakel in de EBOD behuizing.

2. Nadat u de behuizing van de EBOD is ingeschakeld, schakelt u de primaire behuizing.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Stroomstoring op beide voedingen op EBOD behuizing

Wanneer u de kabels hebt ingesteld, moet u ervoor zorgen dat de EBOD nooit alleen met een afzonderlijke PDU verbonden is. Als de primaire behuizing en de EBOD niet op hetzelfde moment, zal het systeem herstellen.

Als u alleen de behuizing van de EBOD niet op beide voedingen, het systeem zal niet automatisch worden hersteld. Voer de volgende stappen om het systeem inschakelen en deze terugzetten in een gezonde staat:

1. Als de primaire behuizing is ingeschakeld, schakelt u zowel energie en koeling Modules (PCMs) uit.

2. Wacht enkele minuten duren voordat het systeem af te sluiten.

3. Schakel in de EBOD behuizing.

4. Nadat u de behuizing van de EBOD is ingeschakeld, schakelt u de primaire behuizing.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Een apparaat inschakelen nadat de primaire en EBOD behuizing verbinding wordt verbroken

Als de verbinding verbroken tussen de stand-by-controller en de bijbehorende EBOD-controller wordt, het apparaat werkt nog steeds. De verbinding tussen het actieve systeem-controller en de bijbehorende EBOD-controller wordt verbroken, failover moet worden uitgevoerd als het apparaat moet normaal blijven.

Wanneer beide Serial Attached SCSI (SAS)-kabels worden verwijderd of de verbinding tussen de EBOD-behuizing en de primaire behuizing is verbroken, werkt het apparaat niet meer. Op dit moment de volgende stappen uitvoeren.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Het apparaat inschakelen nadat de verbinding is verbroken

1. Toegang tot de achterzijde van het apparaat.

2. Als de SAS-kabelverbinding tussen de EBOD-behuizing en de primaire verbroken wordt, worden alle Beveiligingskoppelingen lane LED's op de behuizing van de EBOD uit.

3. Zowel de voeding en koeling Modules (PCMs) afgesloten de ruimte EBOD en de primaire behuizing.

4. Wacht tot de lampjes op de achterzijde van de beide bijlagen uitschakelen.

5. SAS-kabels, en moet ervoor zorgen dat er een goede verbinding tussen de EBOD-behuizing en de primaire.

6. De behuizing van de EBOD eerst inschakelen door beide schakelopties PCM omdraait naar de positie op.

7. Zorg ervoor dat de behuizing van de EBOD op door te controleren dat de groene LED ingeschakeld is.

8. Schakel in de primaire behuizing.

9. Zorg ervoor dat de primaire behuizing op door te controleren dat de controller groene LED ingeschakeld is.

10. Controleer of de EBOD behuizing verbinding met de primaire behuizing is goed door te controleren dat de baan SAS LED's (vier per EBOD controller) zijn op alle.

>[AZURE.IMPORTANT] Als de SAS-kabels beschadigd zijn of als de verbinding tussen de ruimte EBOD en de primaire behuizing niet goed is wanneer u het systeem inschakelt, gaat het in herstelmodus. Neem [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) als dit gebeurt.

## <a name="turn-off-a-running-device"></a>Een actief apparaat uitschakelen

Een actieve StorSimple apparaat moet worden afgesloten als het wordt verplaatst, genomen uit de service, of een defect onderdeel moet worden vervangen. De stappen zijn afhankelijk van of het apparaat StorSimple een 8100 of een model 8600. De 8100 heeft één primaire behuizing, terwijl de 8600 een dual-behuizing met een primaire behuizing en een EBOD behuizing is. In deze sectie worden de stappen voor het afsluiten van een actief apparaat.

- [Apparaat met primaire behuizing](#8100a)

- [Apparaat met EBOD behuizing](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Apparaat met primaire behuizing<a name="8100a"> 

Als u wilt op het apparaat op een ordelijke en gecontroleerde wijze wordt afgesloten, kunt u dit doen via de klassieke Azure portal of via de Windows PowerShell voor StorSimple. 

>[AZURE.IMPORTANT] Niet afgesloten een actief apparaat met behulp van de knop op de achterzijde van het apparaat.
>
>Zorg ervoor dat alle apparaatonderdelen in orde zijn vóór het afsluiten van het apparaat. In de klassieke Azure portal, Ga naar **apparaten** > **onderhoud** > **Status van de Hardware**, en controleer of de status van alle onderdelen van groen. Dit geldt alleen voor een gezond systeem. Als het systeem is dat hier een slecht werkend onderdeel vervangen, u ziet een mislukte (rood) of verslechterd (geel) status voor het desbetreffende onderdeel in de **Status van de Hardware**.

Wanneer u de Windows PowerShell voor StorSimple of de klassieke Azure portal, de stappen in [een StorSimple apparaat afgesloten](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Apparaat met EBOD behuizing<a name="8600a">

>[AZURE.IMPORTANT] Voordat u de primaire behuizing en de EBOD afsluit, zorgen ervoor dat alle apparaatonderdelen in orde. In de klassieke Azure portal, Ga naar **apparaten** > **onderhoud** > **Status van de Hardware**, en controleer of alle onderdelen in orde zijn.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Afsluiten van een actief apparaat met een EBOD-behuizing

1. Volg de stappen die zijn vermeld in [een StorSimple apparaat afgesloten](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) voor de primaire behuizing.

2. Nadat de primaire behuizing is afgesloten, sluit het EBOD af spiegelen uit zowel de voeding en koeling Module (PCM).

3. Als u wilt controleren of de EBOD is afgesloten, Controleer of alle lampjes op de achterzijde van de behuizing van de EBOD uit.

>[AZURE.NOTE] De SAS-kabels die worden gebruikt voor de behuizing van de EBOD verbinding met de primaire behuizing moeten niet tot worden verwijderd nadat het systeem is afgesloten.

## <a name="next-steps"></a>Volgende stappen

[Contact Microsoft Support](storsimple-contact-microsoft-support.md) als u problemen hebt bij het inschakelen of het afsluiten van een StorSimple apparaat.

