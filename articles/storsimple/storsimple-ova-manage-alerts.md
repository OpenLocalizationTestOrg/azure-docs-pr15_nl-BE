<properties 
   pageTitle="Weergeven en beheren van waarschuwingen StorSimple virtuele matrix | Microsoft Azure"
   description="Beschrijving van waarschuwing voorwaarden StorSimple virtuele matrix en ernst en het gebruik van de service Manager StorSimple om waarschuwingen te beheren."
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
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>Gebruik de StorSimple Manager-service weergeven en beheren van waarschuwingen voor de virtuele matrix StorSimple

## <a name="overview"></a>Overzicht

Het tabblad **meldingen** in de StorSimple Manager-service biedt een manier om te bekijken en meldingen StorSimple virtuele matrix betrekking op basis van real-time wissen. Vanuit dit tabblad kunt u de gezondheidsvraagstukken van de StorSimple virtuele matrices (ook bekend als StorSimple op-premises virtuele apparaten) en de totale oplossing van Microsoft Azure StorSimple centraal controleren.

In deze zelfstudie wordt beschreven hoe meldingen, gemeenschappelijke voorwaarden voor waarschuwing, waarschuwing niveaus configureren, weergeven en bijhouden van waarschuwingen. Daarnaast bevat het alert naslag tabellen waarmee u snel zoeken naar een specifieke waarschuwing en op de juiste wijze reageren.

![Pagina waarschuwingen](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Instellingen voor waarschuwingen

U kunt kiezen of u wilt worden gewaarschuwd via e-mail alert voorwaarden voor elk van uw StorSimple virtuele apparaten. Daarnaast kunt u andere geadresseerden signaalberichten identificeren door hun e-mailadres invoeren in het vak **andere geadresseerden** , gescheiden door puntkomma's.

>[AZURE.NOTE] U kunt maximaal 20 e-mailadressen per virtueel apparaat.

Nadat u een e-mailbericht voor een virtueel apparaat inschakelt, ontvangt de leden van de lijst van de kennisgeving een e-mailbericht wanneer een kritieke waarschuwing optreedt. De berichten worden verzonden uit *storsimple-alerts-noreply@mail.windowsazure.com* en wordt de waarschuwing staat beschrijven. Ontvangers klikt op **Afmelden** om u zelf te verwijderen uit de lijst met e-mailmelding.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>E-mailmeldingen van waarschuwingen voor een virtuele apparaat inschakelen

1. Ga naar **apparaten** > **configuratie** voor het virtuele apparaat. Ga naar de sectie **Instellingen voor meldingen** .

    ![instellingen voor waarschuwingen](./media/storsimple-ova-manage-alerts/alerts2.png)

2. **Instellingen van waarschuwingen op**het volgende instellen:

    1. Selecteer **Ja**in het veld **verzenden per e-mail** .

    2. In het veld **e-mailadres servicebeheerders** Selecteer **Ja** als u wilt de servicebeheerder hebt en alle CO-beheerders de meldingen ontvangen.

    3. Voer in het veld **andere geadresseerden** de e-mailadressen van alle andere geadresseerden die de waarschuwingsberichten moeten ontvangen. Voer de namen in de notatie *someone@somewhere.com*. Gebruik een puntkomma te scheiden van de e-mailadressen. U kunt maximaal 20 e-mailadressen per virtueel apparaat configureren. 

        ![Meldingsconfiguratie waarschuwingen](./media/storsimple-ova-manage-alerts/alerts3.png)

3. Onderaan op de pagina, klikt u op **Opslaan** om de configuratie op te slaan.

4. Een test-e-mailbericht worden verzonden, klikt u op het pijlpictogram naast **test e-mailbericht verzenden**. De StorSimple Manager-service worden statusberichten weergegeven als de melding test wordt doorgestuurd. 

5. Wanneer het volgende bericht wordt weergegeven, klikt u op **OK**. 

    ![Meldingen testen verzonden e-mailmelding](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Als de melding test kan niet worden verzonden, wordt de StorSimple Manager-service een toepasselijk bericht weergegeven. Klik op **OK**, wacht een paar minuten en probeer het vervolgens opnieuw te verzenden uw testbericht. 

    De melding test is vergelijkbaar met het volgende.

    ![Voorbeeld van de e-mail waarschuwingen test](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Algemene voorwaarden voor waarschuwingen

Uw virtuele matrix StorSimple waarschuwingen genereert in reactie op een aantal voorwaarden. Dit zijn de meest voorkomende waarschuwing voorwaarden:

- **Verbindingsproblemen** : deze signalen optreden wanneer er problemen zijn bij het overbrengen van gegevens. Communicatieproblemen kunnen optreden tijdens de overdracht van gegevens naar en vanuit de Azure opslag account of door gebrek aan verbinding tussen de virtuele apparaten en de StorSimple Manager-service. Problemen in de communicatie zijn enkele van de moeilijkst op te lossen, omdat er zo veel punten van de fout. Moet u altijd eerst controleren of verbinding met het netwerk en toegang tot het Internet beschikbaar zijn voordat u doorgaat naar geavanceerde probleemoplossing. Ga naar [StorSimple virtuele matrix systeemvereisten](storsimple-ova-system-requirements.md)voor meer informatie over de poorten en de firewallinstellingen. Voor hulp bij het oplossen van problemen, gaat u naar [problemen met de cmdlet verbinding testen](storsimple-troubleshoot-deployment.md).

- **Problemen met prestaties** – deze waarschuwingen worden veroorzaakt wanneer het systeem is niet optimaal, bijvoorbeeld wanneer het gaat om onder een zware belasting.

Zie ook waarschuwingen over beveiliging, updates of storingen van de taak.

## <a name="alert-severity-levels"></a>Waarschuwing prioriteitsniveaus

Waarschuwingen hebben verschillende prioriteitsniveaus, afhankelijk van de impact die de waarschuwing situatie en de noodzaak van een antwoord op de waarschuwing. De niveaus zijn:

- **Kritische** – deze waarschuwing is in reactie op een voorwaarde die gevolgen voor de succesvolle uitvoering van het systeem heeft. Actie is vereist om ervoor te zorgen dat de StorSimple niet wordt onderbroken.

- **Waarschuwing** : deze voorwaarde die kritiek niet opgelost kan worden. U moet de situatie onderzoeken en schakelt u het probleem moet actie ondernemen.

- **Informatie** – deze waarschuwing bevat informatie die nuttig zijn kan in het bijhouden en beheren van uw systeem.

## <a name="view-and-track-alerts"></a>Waarschuwingen weergeven en bijhouden

De Manager StorSimple servicedashboard biedt u een snelle blik op het aantal waarschuwingen op uw virtuele apparaten, gerangschikt op prioriteitsniveau.

![Waarschuwingen-dashboard](./media/storsimple-ova-manage-alerts/alerts6.png)

Op het prioriteitsniveau van de te klikken, opent het tabblad **Alerts** . De resultaten bevatten alleen de waarschuwingen die overeenkomen met die ernst.

![Waarschuwingen rapport binnen het bereik van berichttype](./media/storsimple-ova-manage-alerts/alerts7.png)

Een waarschuwing in de lijst te klikken op biedt u aanvullende gegevens voor de waarschuwing, de laatste keer dat de waarschuwing is gemeld, waaronder het aantal exemplaren van de waarschuwing op het apparaat en de aanbevolen actie voor het oplossen van de waarschuwing.

Als u de gegevens te verzenden naar Microsoft Support nodig hebt, kunt u de informatie in de waarschuwing kopiëren naar een tekstbestand. Nadat u hebt gevolgd van de aanbeveling en de voorwaarde alert op ruimten opgelost, moet u de waarschuwing van de waarschuwing selecteren op het tabblad **Alerts** en **schakelt**u op wissen. Om meerdere meldingen wilt wissen, selecteert u elke waarschuwing, klikt u op alle kolommen behalve de kolom **Waarschuwing** en klik op **wissen** nadat u de waarschuwingen worden gewist hebt geselecteerd. Houd er rekening mee dat sommige waarschuwingen automatisch uitgeschakeld als het probleem opgelost is of als de waarschuwing wordt bijgewerkt met nieuwe informatie.

Als u op **wissen**klikt, hebt u de gelegenheid om opmerkingen over de waarschuwing en de stappen die u hebt ondernomen om het probleem te verhelpen. 

![Waarschuwing-opmerkingen](./media/storsimple-ova-manage-alerts/clear-alert.png)

Klik op het pictogram ![pictogram](./media/storsimple-ova-manage-alerts/check-icon.png) opslaan van uw opmerkingen.

Sommige gebeurtenissen worden gewist door het systeem als een gebeurtenis wordt geactiveerd met nieuwe informatie. In dat geval ziet u het volgende bericht.

![Waarschuwingsbericht wissen](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Berichten sorteren en bekijken

Maximaal 250 meldingen op het tabblad **waarschuwingen** weergeven. Als dat aantal waarschuwingen is overschreden, wordt niet alle waarschuwingen worden weergegeven in de standaardweergave. U kunt de volgende velden als u wilt aanpassen welke waarschuwingen combineren:

- **Status** : **actief** of **uitgeschakeld** waarschuwingen kunnen worden weergegeven. Actieve waarschuwingen zijn nog steeds geactiveerd op het systeem, terwijl de gewiste waarschuwingen zijn handmatig door een beheerder worden uitgeschakeld of programmatisch gewist omdat het systeem de waarschuwing voorwaarde met nieuwe informatie bijgewerkt.

- **Ernst** – u kunt waarschuwingen van alle niveaus (kritieke, waarschuwing, informatie) of alleen een bepaalde prioriteit, zoals alleen kritieke waarschuwingen weergeven.

- **Bron** : meldingen van alle bronnen weergeven of de berichten die afkomstig van de service of een of alle virtuele apparaten zijn te beperken.

- **Tijdsbereik** – door te geven de datums **van** en **naar** en tijdstempels, kunt u bekijken waarschuwingen tijdens de periode waarin u geïnteresseerd bent.

## <a name="alerts-quick-reference"></a>Waarschuwingen-beknopte naslaginformatie

De volgende tabellen bevatten een aantal Microsoft Azure StorSimple waarschuwingen die u tegenkomen kunt, en aanvullende informatie en aanbevelingen indien beschikbaar. StorSimple virtueel apparaat meldingen vallen in een van de volgende categorieën:

- [Waarschuwingen voor cloud-connectiviteit](#cloud-connectivity-alerts)

- [Configuratie van waarschuwingen](#configuration-alerts)

- [Foutmeldingen](#job-failure-alerts)

- [Prestatiesignalen](#performance-alerts)

- [Beveiligingswaarschuwingen](#security-alerts)

- [Meldingen van updates](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Waarschuwingen voor cloud-connectiviteit

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Apparaat *<device name>* is niet verbonden met de cloud.|De benoemde apparaat verbinding geen met de cloud. |Kan geen verbinding met de cloud. Dit kan een van de volgende oorzaken zijn:<ul><li>Het is mogelijk een probleem met de instellingen op uw apparaat.</li><li>Het is mogelijk een probleem met de referenties van de opslag.</li></ul>Ga naar het [lokale web UI](storsimple-ova-web-ui-admin.md) van het apparaat voor meer informatie over het oplossen van verbindingsproblemen.|


### <a name="configuration-alerts"></a>Configuratie van waarschuwingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Over lokalen virtuele configuratie wordt niet ondersteund.|Trage prestaties.|De huidige configuratie kan leiden tot verminderde prestaties. Zorg ervoor dat uw server voldoet aan de minimale configuratie. Ga naar [StorSimple virtuele matrix eisen](storsimple-ova-system-requirements.md)voor meer informatie. 
|U worden ingerichte schijfruimte op <*naam*> uitgevoerd.|Waarschuwing: schijfruimte.|U weinig schijfruimte ingericht. Om ruimte vrij te maken, kunt u werkbelasting aan een ander volume of share verplaatsen of verwijderen van gegevens.

### <a name="job-failure-alerts"></a>Foutmeldingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Back-up van <*naam*> kan niet worden voltooid.|Fout van back-uptaak.|Kan niet een back-up maken. Houd rekening met het volgende:<ul><li>Verbindingsproblemen wordt mogelijk verhinderd door de back-up met succes voltooid. Zorg ervoor dat er geen problemen zijn. Ga naar het [lokale web UI](storsimple-ova-web-ui-admin.md) voor het virtuele apparaat voor meer informatie over het oplossen van verbindingsproblemen.</li><li>De beschikbare opslaglimiet bereikt. Om ruimte vrij te maken, kunt u overwegen alle back-ups die u niet meer nodig hebt verwijderen.</li></ul> De problemen hebt opgelost, schakelt u de waarschuwing en probeer het opnieuw.|
|Het herstellen van <*naam*> kan niet worden voltooid.|Mislukte taak herstellen.|Kan niet herstellen vanaf back-up. Houd rekening met het volgende:<ul><li>De lijst met back-up mogelijk niet geldig. Vernieuw de lijst om te controleren of dat het nog steeds geldig is.</li><li>Verbindingsproblemen wordt mogelijk verhinderd door de bewerking voor terugzetten is voltooid. Zorg ervoor dat er geen problemen zijn.</li><li>De beschikbare opslaglimiet bereikt. Om ruimte vrij te maken, kunt u overwegen alle back-ups die u niet meer nodig hebt verwijderen.</li></ul>De problemen hebt opgelost, schakelt u de waarschuwing en proberen de bewerking voor terugzetten.|
|Klonen van <*naam*> kan niet worden voltooid.|Mislukte taak klonen.|Kan een kloon niet maken. Houd rekening met het volgende:<ul><li>De lijst met back-up mogelijk niet geldig. Vernieuw de lijst om te controleren of dat het nog steeds geldig is.</li><li>Verbindingsproblemen wordt mogelijk verhinderd door de kopieerbewerking is voltooid. Zorg ervoor dat er geen problemen zijn.</li><li>De beschikbare opslaglimiet bereikt. Om ruimte vrij te maken, kunt u overwegen alle back-ups die u niet meer nodig hebt verwijderen.</li></ul>De problemen hebt opgelost, schakelt u de waarschuwing en probeer het opnieuw.|

### <a name="performance-alerts"></a>Prestatiesignalen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Er onverwachte vertragingen bij de overdracht van gegevens.|Langzame gegevensoverdracht.|Bandbreedteregeling fouten optreden wanneer u de schaalbaarheid doelen van storage-service overschrijden. De storage-service doet dit om ervoor te zorgen dat geen enkele client of de huurder de service ten koste van anderen gebruiken kan. Ga naar de [Monitor, vaststellen, en Microsoft Azure opslag oplossen](storage-monitoring-diagnosing-troubleshooting.md)voor meer informatie over het oplossen van uw opslag Azure-account.
|U weinig lokale reservering schijfruimte op <*naam*>.|Trage reactietijd.|10% van de totale ingerichte grootte voor <*naam*> is gereserveerd voor het lokale apparaat en u nu weinig op de gereserveerde ruimte. De werkbelasting van <*naam*> is een hogere snelheid van lospeuteren genereren of u onlangs een grote hoeveelheid gegevens hebt gemigreerd. Dit kan leiden tot verminderde prestaties. Houd rekening met een van de volgende handelingen uit om dit probleem oplossen:<ul><li>Verhoog de wolk bandbreedte aan dit apparaat.</li><li>Verminderen of werklasten naar een ander volume of share.</li></ul>

### <a name="security-alerts"></a>Beveiligingswaarschuwingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Wachtwoord voor <*naam*> zal in <*getal*> dagen verlopen.|Waarschuwing: wachtwoord.| Uw wachtwoord verloopt < getal < dagen. Het is een goed idee om uw wachtwoord. Ga naar [de virtuele matrix StorSimple apparaat administrator-wachtwoord wijzigen](storsimple-ova-change-device-admin-password.md)voor meer informatie.

### <a name="update-alerts"></a>Meldingen van updates

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Nieuwe updates zijn beschikbaar voor uw apparaat.|De virtuele matrix StorSimple updates zijn beschikbaar.|U kunt nieuwe updates installeren vanaf de pagina **onderhoud** .|
|Kan niet scannen op nieuwe updates op <*naam*>.|Fout bij bijwerken. |Er is een fout opgetreden tijdens het installeren van nieuwe updates. U kunt de updates handmatig installeren. Als het probleem zich blijft voordoen, neem dan contact op met [Microsoft Support](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over de virtuele matrix StorSimple](storsimple-ova-overview.md).


