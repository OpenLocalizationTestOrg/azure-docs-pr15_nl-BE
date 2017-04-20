<properties 
   pageTitle="Weergeven en beheren van waarschuwingen StorSimple | Microsoft Azure"
   description="Beschrijving van waarschuwing voorwaarden StorSimple en ernst, het configureren van meldingen en de StorSimple Manager-service gebruiken om waarschuwingen te beheren."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>Gebruik de StorSimple Manager-service weergeven en beheren van waarschuwingen StorSimple

## <a name="overview"></a>Overzicht

Het tabblad **meldingen** in de StorSimple Manager-service biedt een manier om te controleren en de StorSimple apparaat – meldingen op basis van real-time wissen. Vanuit dit tabblad kunt u de problemen van de gezondheid van uw apparaten StorSimple en de totale oplossing van Microsoft Azure StorSimple centraal te controleren.

In deze zelfstudie wordt beschreven algemene waarschuwing voorwaarden, niveaus waarschuwingen en meldingen configureren. Daarnaast bevat het alert naslag tabellen waarmee u snel zoeken naar een specifieke waarschuwing en op de juiste wijze reageren.

![Pagina waarschuwingen](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Algemene voorwaarden voor waarschuwingen

Het apparaat StorSimple waarschuwingen genereert in reactie op een aantal voorwaarden. Dit zijn de meest voorkomende waarschuwing voorwaarden:

- **Hardwareproblemen** – deze meldingen geven over de gezondheid van uw hardware. Ze laten u weten als firmware-upgrades nodig zijn, als een netwerkinterface problemen heeft of als er een probleem met een van de gegevensstations van uw.

- **Verbindingsproblemen** : deze signalen optreden wanneer er problemen zijn bij het overbrengen van gegevens. Communicatieproblemen kunnen optreden tijdens de overdracht van gegevens naar en vanuit de Azure opslag account of door gebrek aan verbinding tussen de apparaten en de StorSimple Manager-service. Problemen in de communicatie zijn enkele van de moeilijkst op te lossen, omdat er zo veel punten van de fout. Moet u altijd eerst controleren of verbinding met het netwerk en toegang tot het Internet beschikbaar zijn voordat u doorgaat naar geavanceerde probleemoplossing. Voor hulp bij het oplossen van problemen, gaat u naar [problemen met de cmdlet verbinding testen](storsimple-troubleshoot-deployment.md).

- **Problemen met prestaties** – deze waarschuwingen worden veroorzaakt wanneer het systeem is niet optimaal, bijvoorbeeld wanneer het gaat om onder een zware belasting.

Zie ook waarschuwingen over beveiliging, updates of storingen van de taak.

## <a name="alert-severity-levels"></a>Waarschuwing prioriteitsniveaus

Waarschuwingen hebben verschillende prioriteitsniveaus, afhankelijk van de impact die de waarschuwing situatie en de noodzaak van een antwoord op de waarschuwing. De niveaus zijn:

- **Kritische** – deze waarschuwing is in reactie op een voorwaarde die gevolgen voor de succesvolle uitvoering van het systeem heeft. Actie is vereist om ervoor te zorgen dat de StorSimple niet wordt onderbroken.

- **Waarschuwing** : deze voorwaarde die kritiek niet opgelost kan worden. U moet de situatie onderzoeken en schakelt u het probleem moet actie ondernemen.

- **Informatie** – deze waarschuwing bevat informatie die nuttig zijn kan in het bijhouden en beheren van uw systeem.

## <a name="configure-alert-settings"></a>Instellingen voor waarschuwingen

U kunt kiezen of u wilt worden gewaarschuwd via e-mail alert voorwaarden voor elk van uw apparaten StorSimple. Daarnaast kunt u andere geadresseerden signaalberichten identificeren door hun e-mailadres invoeren in het vak **andere geadresseerden** , gescheiden door puntkomma's.

>[AZURE.NOTE] U kunt maximaal 20 e-mailadressen per apparaat.

Nadat u een e-mailbericht voor een apparaat hebt ingeschakeld, ontvangt de leden van de lijst van de kennisgeving een e-mailbericht wanneer een kritieke waarschuwing optreedt. De berichten worden verzonden uit *storsimple-alerts-noreply@mail.windowsazure.com* en wordt de waarschuwing staat beschrijven. Ontvangers klikt op **Afmelden** om u zelf te verwijderen uit de lijst met e-mailmelding.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>E-mailmeldingen van waarschuwingen voor een apparaat inschakelen

1. Ga naar **apparaten** > **configureren** voor het apparaat.

2. **Instellingen van waarschuwingen op**het volgende instellen:

    1. Selecteer **Ja**in het veld **verzenden per e-mail** .

    2. In het veld **e-mailadres servicebeheerders** Selecteer **Ja** als u wilt de servicebeheerder hebt en alle CO-beheerders de meldingen ontvangen.

    3. Voer in het veld **andere geadresseerden** de e-mailadressen van alle andere geadresseerden die de waarschuwingsberichten moeten ontvangen. Voer de namen in de notatie *someone@somewhere.com*. Gebruik een puntkomma te scheiden van de e-mailadressen. U kunt maximaal 20 e-mailadressen per apparaat. 

        ![Meldingsconfiguratie waarschuwingen](./media/storsimple-manage-alerts/AlertNotify.png)

3. Een test-e-mailbericht worden verzonden, klikt u op het pijlpictogram naast **test e-mailbericht verzenden**. De StorSimple Manager-service worden statusberichten weergegeven als de melding test wordt doorgestuurd. 

4. Wanneer het volgende bericht wordt weergegeven, klikt u op **OK**. 

    ![Meldingen testen verzonden e-mailmelding](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Als de melding test kan niet worden verzonden, wordt de StorSimple Manager-service een toepasselijk bericht weergegeven. Klik op **OK**, wacht een paar minuten en probeer het vervolgens opnieuw te verzenden uw testbericht. 

## <a name="view-and-track-alerts"></a>Waarschuwingen weergeven en bijhouden

De Manager StorSimple servicedashboard biedt u een snelle blik op het aantal waarschuwingen op uw apparaten, gerangschikt op prioriteitsniveau.

![Waarschuwingen-dashboard](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Op het prioriteitsniveau van de te klikken, opent het tabblad **Alerts** . De resultaten bevatten alleen de waarschuwingen die overeenkomen met die ernst.

![Waarschuwingen rapport binnen het bereik van berichttype](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Een waarschuwing in de lijst te klikken op biedt u aanvullende gegevens voor de waarschuwing, de laatste keer dat de waarschuwing is gemeld, waaronder het aantal exemplaren van de waarschuwing op het apparaat en de aanbevolen actie voor het oplossen van de waarschuwing. Als het een hardware-signaal is, zal het ook het hardwareonderdeel herkennen.

![Voorbeeld van waarschuwing op hardware](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Als u de gegevens te verzenden naar Microsoft Support nodig hebt, kunt u de informatie in de waarschuwing kopiëren naar een tekstbestand. Nadat u hebt gevolgd van de aanbeveling en de voorwaarde alert op ruimten opgelost, moet u de waarschuwing van het apparaat wissen door de waarschuwing op het tabblad **Alerts** te selecteren en te klikken op **verwijderen**. Om meerdere meldingen wilt wissen, selecteert u elke waarschuwing, klikt u op alle kolommen behalve de kolom **Waarschuwing** en klik op **wissen** nadat u de waarschuwingen worden gewist hebt geselecteerd. Houd er rekening mee dat sommige waarschuwingen automatisch uitgeschakeld als het probleem opgelost is of als de waarschuwing wordt bijgewerkt met nieuwe informatie.

Als u op **wissen**klikt, hebt u de gelegenheid om opmerkingen over de waarschuwing en de stappen die u hebt ondernomen om het probleem te verhelpen. Sommige gebeurtenissen worden gewist door het systeem als een gebeurtenis wordt geactiveerd met nieuwe informatie. In dat geval ziet u het volgende bericht.

![Waarschuwingsbericht wissen](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Berichten sorteren en bekijken

Wellicht efficiënter uitvoeren van rapporten voor waarschuwingen zodat u kunt bekijken en schakel ze in groepen. Bovendien kan het tabblad **Alerts** maximaal 250 meldingen weergeven. Als dat aantal waarschuwingen is overschreden, wordt niet alle waarschuwingen worden weergegeven in de standaardweergave. U kunt de volgende velden als u wilt aanpassen welke waarschuwingen combineren:

- **Status** : **actief** of **uitgeschakeld** waarschuwingen kunnen worden weergegeven. Actieve waarschuwingen zijn nog steeds geactiveerd op het systeem, terwijl de gewiste waarschuwingen zijn handmatig door een beheerder worden uitgeschakeld of programmatisch gewist omdat het systeem de waarschuwing voorwaarde met nieuwe informatie bijgewerkt.

- **Ernst** – u kunt waarschuwingen van alle niveaus (kritieke, waarschuwing, informatie) of alleen een bepaalde prioriteit, zoals alleen kritieke waarschuwingen weergeven.

- **Bron** : meldingen van alle bronnen weergeven of beperken van de berichten die afkomstig van de service of één of alle apparaten zijn.

- **Tijdsbereik** – door te geven de datums **van** en **naar** en tijdstempels, kunt u bekijken waarschuwingen tijdens de periode waarin u geïnteresseerd bent.

## <a name="alerts-quick-reference"></a>Waarschuwingen-beknopte naslaginformatie

De volgende tabellen bevatten een aantal Microsoft Azure StorSimple waarschuwingen die u tegenkomen kunt, en aanvullende informatie en aanbevelingen indien beschikbaar. StorSimple apparaat meldingen vallen in een van de volgende categorieën:

- [Waarschuwingen voor cloud-connectiviteit](#cloud-connectivity-alerts)

- [Cluster-waarschuwingen](#cluster-alerts)

- [Disaster recovery waarschuwingen](#disaster-recovery-alerts)

- [Hardware-waarschuwingen](#hardware-alerts)

- [Foutmeldingen](#job-failure-alerts)

- [Lokaal vaste volume waarschuwingen](#locally-pinned-volume-alerts)

- [Waarschuwingen voor netwerken](#networking-alerts)

- [Prestatiesignalen](#performance-alerts)

- [Beveiligingswaarschuwingen](#security-alerts)

- [Pakket waarschuwingen](#support-package-alerts)

- [Meldingen van updates](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Waarschuwingen voor cloud-connectiviteit

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Verbinding met <*wolk referentie naam*> kan niet worden vastgesteld.|Kan geen verbinding met de account van de opslag.|Blijkbaar is er mogelijk een probleem met het apparaat. Voer het `Test-HcsmConnection` cmdlet uit de Interface van Windows PowerShell voor StorSimple op uw apparaat te identificeren en oplossen van het probleem. Als de instellingen correct zijn, kan het probleem zijn met de referenties van de opslag-account waarvoor de waarschuwing is verheven. In dit geval gebruikt de `Test-HcsStorageAccountCredential` cmdlet om te bepalen of er zijn problemen die u kunt oplossen.<ul><li>Controleer de netwerkinstellingen.</li><li>Controleer de referenties van de opslag.</li></ul>|
|We hebben niet een levensteken ontvangen van het apparaat voor de laatste minuten van <*getal*>.|Geen verbinding met het apparaat.|Blijkbaar is er een probleem met het apparaat. Gebruik de `Test-HcsmConnection` cmdlet uit de Interface van Windows PowerShell voor StorSimple op uw apparaat te identificeren en oplossen van het probleem of neem contact op met de netwerkbeheerder.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>StorSimple gedrag wanneer de wolk verbinding mislukt

Wat gebeurt er als cloud verbinding mislukt voor mijn apparaat StorSimple is actief in de productie?

Als cloud-verbinding op uw apparaat StorSimple productie mislukt, vervolgens afhankelijk van de status van het apparaat, kan het volgende gebeuren: 

- **Voor de lokale gegevens op het apparaat**: voor enige tijd, zal er geen onderbrekingen en leest zal blijven worden aangeboden. Als het aantal uitstaande IOs verhoogt en een limiet overschrijdt, kunnen echter gelezen niet starten. 

    Afhankelijk van de hoeveelheid gegevens op het apparaat blijven de schrijft ook optreden voor de eerste paar uur na de onderbreking in de cloud-connectiviteit. Schrijfbewerkingen worden vervolgens vertragen en uiteindelijk te mislukken als de wolk connectiviteit wordt onderbroken voor enkele uren. (Er is tijdelijke opslagruimte op het apparaat voor de gegevens die moeten worden verplaatst naar de cloud. Dit gebied wordt leeggemaakt wanneer de gegevens worden verzonden. Als de verbinding mislukt, de gegevens in deze opslagruimte niet geduwd naar de cloud, en i/o mislukt.)   

 
- **Voor de gegevens in de cloud**: voor de meeste cloud-connectivity fouten, wordt een fout geretourneerd. Zodra de verbinding is hersteld, worden de IOs hervat zonder dat de gebruiker het volume on line brengen. In zeldzame gevallen mogelijk tussenkomst van de gebruiker vereist om het volume on line brengen van het klassieke Azure portal. 
 
- **Voor momentopnamen wolk uitgevoerd**: de bewerking wordt opnieuw geprobeerd een paar keer binnen 4-5 uur en als de verbinding niet is hersteld, niet de wolk momentopnamen.


### <a name="cluster-alerts"></a>Cluster-waarschuwingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Apparaat <*apparaatnaam*> overgenomen.|Apparaat is in de onderhoudsmodus.|Apparaat is mislukt op door binnenkomen of verlaten in de onderhoudsmodus. Dit is normaal en is geen actie vereist. Nadat u deze waarschuwing hebt bevestigd, schakelt u op de pagina waarschuwingen.|
|Apparaat <*apparaatnaam*> overgenomen.|Apparaatfirmware of software is zojuist bijgewerkt.|Er is een clusterfailover vanwege een bijwerking. Dit is normaal en is geen actie vereist. Nadat u deze waarschuwing hebt bevestigd, schakelt u op de pagina waarschuwingen.|
|Apparaat <*apparaatnaam*> overgenomen.|Controller is afgesloten of opnieuw opgestart.|Apparaat is mislukt op omdat de actieve domeincontroller is afgesloten of opnieuw wordt opgestart door een beheerder. Er is geen actie vereist. Nadat u deze waarschuwing hebt bevestigd, schakelt u op de pagina waarschuwingen.|
|Apparaat <*apparaatnaam*> overgenomen.|Geplande failover.|Controleren of dit is een geplande failover. Nadat u de passende maatregelen hebben genomen, schakelt u deze waarschuwing op de pagina waarschuwingen.|
|Apparaat <*apparaatnaam*> overgenomen.|Niet-geplande failover.|StorSimple is gebouwd om het automatisch herstellen van niet-geplande failover. Als u een groot aantal van deze berichten ziet, neem dan contact op met Microsoft Support.|
|Apparaat <*apparaatnaam*> overgenomen.|Overige/onbekende oorzaak.|Als u een groot aantal van deze berichten ziet, neem dan contact op met Microsoft Support. Nadat het probleem opgelost is, schakelt u deze waarschuwing op de pagina waarschuwingen.|
|Een service met essentiële apparaten wordt gerapporteerd als mislukt.|DataPath service is mislukt. |Neem contact op met Microsoft Support voor hulp.|
|Virtuele IP-adres voor de netwerkinterface <*gegevens #*> wordt gerapporteerd als mislukt. |Overige/onbekende oorzaak. |Soms tijdelijke omstandigheden kunnen deze waarschuwingen veroorzaken. Als dit het geval is, klikt u vervolgens deze waarschuwing automatisch gewist na enige tijd. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support.|
|Virtuele IP-adres voor de netwerkinterface <*gegevens #*> wordt gerapporteerd als mislukt.|Interfacenaam: <*gegevens #*> IP-adres <IP address> on line kan worden gebracht omdat het een dubbel IP-adres is gevonden op het netwerk. |Ervoor te zorgen dat de dubbele IP-adres van het netwerk is verwijderd of opnieuw configureren van de interface met een ander IP-adres.|


### <a name="disaster-recovery-alerts"></a>Disaster recovery waarschuwingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Nuttige toepassing van afvalstoffen kunnen alle instellingen voor deze service niet herstellen. De configuratiegegevens van apparaat is in een inconsistente toestand voor sommige apparaten.|Gegevens een inconsistentie gevonden na noodherstel.|Gecodeerde gegevens op de service is niet gesynchroniseerd met die op het apparaat. Toestaan dat het apparaat <*apparaatnaam*> uit StorSimple Manager tijdens de synchronisatie te starten. Gebruik de Windows PowerShell-Interface voor StorSimple uit te voeren de `Restore-HcsmEncryptedServiceData` op apparaat <*apparaatnaam*> cmdlet, bieden u het oude wachtwoord als invoer voor deze cmdlet de beveiligingsprofiel herstellen. Voer het `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet voor het bijwerken van de sleutel data service. Nadat u de passende maatregelen hebben genomen, schakelt u deze waarschuwing op de pagina waarschuwingen.|


### <a name="hardware-alerts"></a>Hardware-waarschuwingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Hardware-onderdeel <*onderdeel-ID*> wordt gerapporteerd als <*status*>.||Soms tijdelijke omstandigheden kunnen deze waarschuwingen veroorzaken. Als dit het geval is, wordt deze melding automatisch uitgeschakeld na enige tijd. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support.|
|Passieve controller defect.|De passieve controller (secundaire) functioneert niet.|Het apparaat is ingeschakeld maar een van uw domeincontrollers is defect. Probeer het opnieuw starten van controller. Als het probleem niet is opgelost, neem dan contact op met Microsoft Support.|

### <a name="job-failure-alerts"></a>Foutmeldingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Back-up van <*bron volume groep-ID*> is mislukt.|Back-uptaak is mislukt.|Verbindingsproblemen wordt mogelijk verhinderd door de back-up met succes voltooid. Als er geen problemen zijn, is het mogelijk dat u het maximum aantal back-ups hebt bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer de bewerking opnieuw. Nadat u de passende maatregelen hebben genomen, schakelt u deze waarschuwing op de pagina waarschuwingen.|
|Klonen van <*backup bronelement id*> <*bestemming volume serienummers*> is mislukt.|Kloon taak is mislukt.|Vernieuw de lijst back-up om te controleren of de back-up nog geldig is. Als de back-up geldig is, is het mogelijk dat cloud verbindingsproblemen verhinderen de kopieerbewerking is voltooid. Als er geen problemen zijn, is het mogelijk dat u de opslaglimiet heeft bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer de bewerking opnieuw. Nadat u hebt genomen passende maatregelen om het probleem te verhelpen, schakelt u deze waarschuwing op de pagina waarschuwingen.|
|Herstellen van <*backup bronelement id*> is mislukt.|Terugzetten is mislukt.|Vernieuw de lijst back-up om te controleren of de back-up nog geldig is. Als de back-up geldig is, is het mogelijk dat problemen met de cloud verhinderen de bewerking voor terugzetten is voltooid. Als er geen problemen zijn, is het mogelijk dat u de opslaglimiet heeft bereikt. Verwijder alle back-ups die niet langer nodig zijn en probeer de bewerking opnieuw. Nadat u hebt genomen passende maatregelen om het probleem te verhelpen, schakelt u deze waarschuwing op de pagina waarschuwingen.|

### <a name="locally-pinned-volume-alerts"></a>Lokaal vaste volume waarschuwingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Maken van een lokaal volume <*volumenaam*> is mislukt.| Een taak voor het maken van het volume is mislukt. <*Fout bericht overeenkomt met de mislukte foutcode*>.|Verbindingsproblemen wordt mogelijk verhinderd door de ruimte maken van bewerking met succes is voltooid. Lokaal vastgezette volumes dik zijn ingericht en voor het maken van ruimte hiervoor lopen trapsgewijs geschakelde volumes naar de cloud. Als er geen problemen zijn, hebt u mogelijk het lokale ruimte op het apparaat uitgeput. Bepalen of er ruimte op het apparaat bestaat voordat u deze opnieuw.|
|Uitbreiding van het lokale volume <*volumenaam*> is mislukt.|De taak van de wijziging volume is mislukt vanwege <*fout bericht overeenkomt met de mislukte foutcode*>.|Verbindingsproblemen wordt mogelijk verhinderd door de volume expansie bewerking met succes is voltooid. Lokaal vastgezette volumes dik zijn ingericht en voor het uitbreiden van de bestaande ruimte hiervoor lopen trapsgewijs geschakelde volumes naar de cloud. Als er geen problemen zijn, hebt u mogelijk het lokale ruimte op het apparaat uitgeput. Bepalen of er ruimte op het apparaat bestaat voordat u deze opnieuw.|
|Conversie van <*naam van volume*> volume is mislukt.|De taak van de conversie volume converteren het volumetype van lokaal vastgemaakt aan doorverbonden is mislukt.|Conversie van het volume van het type lokaal vastgemaakt aan gelaagde kan niet worden voltooid. Zorg ervoor dat er geen connectiviteit problemen te voorkomen dat de bewerking wordt voltooid. Ga naar [problemen met de cmdlet Test-HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet)voor het oplossen van verbindingsproblemen.<br>Het oorspronkelijke volume lokaal vastgemaakt is nu gemarkeerd als een gelaagde volume omdat bepaalde gegevens van het lokaal vaste volume tijdens de conversie naar de cloud is gemorst. Het resulterende gelaagde volume is nog steeds die lokale ruimte op het apparaat kan niet opnieuw worden gebruikt voor toekomstige lokale volumes.<br>Eventuele verbindingsproblemen oplossen, schakelt u de waarschuwing en dit volume terug naar het oorspronkelijke volumetype lokaal vaste op alle gegevens ter beschikking wordt gesteld lokaal opnieuw converteren.|
|Conversie van <*naam van volume*> volume is mislukt.|De taak van de conversie volume converteren van het volumetype uit geschakeld naar lokaal vastgemaakt is mislukt.|Conversie van het volume van het type doorverbonden naar lokaal vastgemaakt kan niet worden voltooid. Zorg ervoor dat er geen connectiviteit problemen te voorkomen dat de bewerking wordt voltooid. Ga naar [problemen met de cmdlet Test-HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet)voor het oplossen van verbindingsproblemen.<br>De oorspronkelijke gelaagde volume nu gemarkeerd als een lokaal vaste volume als onderdeel van het conversieproces heeft nog steeds gegevens die zich in de cloud, terwijl de dik ingerichte ruimte op het apparaat voor dit volume niet meer kunt opgeëist voor toekomstige lokale volumes.<br>Eventuele verbindingsproblemen oplossen, schakelt u de waarschuwing en dit volume converteren naar het oorspronkelijke gelaagde volumetype zodat de lokale ruimte dik is ingericht op het apparaat kan komen.|
|Verbruik van lokale ruimte voor lokale momentopnamen van bijna <*naam van volume*>|Lokale momentopnamen voor back-up beleid snel onvoldoende ruimte en ongeldig worden gemaakt om te voorkomen dat de host schrijven storingen.|Vaak lokale momentopnamen naast een hoge lospeuteren van de hoeveelheden die zijn gekoppeld aan deze groep met back-up beleid veroorzaken lokale ruimte op het apparaat snel worden verbruikt. Verwijder alle lokale momentopnamen die niet langer nodig zijn. Ook de lokale momentopname schema's voor deze back-up beleid minder vaak lokale momentopnamen en ervoor te zorgen dat cloud regelmatig momentopnamen worden bijwerken. Als deze acties niet zijn genomen, lokale ruimte voor deze momentopnamen kan snel uitgeput en worden automatisch verwijderd om ervoor te zorgen dat schrijfbewerkingen host blijven worden verwerkt.|
|Lokale momentopnamen voor <*groepsnaam volume*> is ongeldig gemaakt.|De lokale momentopnamen voor <*groepsnaam volume*> zijn ongeldig gemaakt en vervolgens verwijderd omdat ze zijn meer dan de lokale ruimte op het apparaat.|De lokale momentopname schema's voor deze back-beleid bekijken en verwijderen van een lokale momentopnamen die niet langer nodig zijn zodat deze zich niet meer voordoet in de toekomst. Vaak lokale momentopnamen naast een hoge lospeuteren van de hoeveelheden die zijn gekoppeld aan de groep Back-beleid ervoor zorgen dat lokale ruimte op het apparaat snel worden verbruikt.|
|Herstellen van <*backup bronelement id*> is mislukt.|De terugzettaak is mislukt.|Als u lokaal hebt vastgemaakt of een combinatie van lokaal vastgemaakt en trapsgewijs geschakelde volumes in deze back-up beleid vernieuwen de back-up lijst te controleren dat is de back-up nog steeds geldig. Als de back-up geldig is, is het mogelijk dat problemen met de cloud verhinderen de bewerking voor terugzetten is voltooid. Lokaal vastgezette volumes die zijn als onderdeel van deze groep snapshot wordt teruggezet niet al hun gegevens naar het apparaat downloadt en hebt u een combinatie van gelaagde en lokaal vastgemaakte volumes in deze groep momentopname, ze worden niet met elkaar gesynchroniseerd. Als de bewerking voor terugzetten is voltooid, worden de volumes in deze groep off line op de host en probeer het opnieuw herstellen. Houd er rekening mee dat elke wijziging van de gegevens die zijn uitgevoerd tijdens het terugzetten op volume verloren zullen gaan.|

### <a name="networking-alerts"></a>Waarschuwingen voor netwerken

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|StorSimple (s) kan niet worden gestart.|Fout gegevenspad |Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support.|
|Dubbele IP-adres gevonden voor 'Data0'.| |Een IP-adres "10.0.0.1" conflict is gevonden. De netwerkbron 'Data0' op het apparaat *<device1>* offline is. Zorg ervoor dat dit IP-adres niet wordt gebruikt door een andere entiteit in dit netwerk. Ga naar [problemen met de cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet)voor het oplossen van netwerkproblemen. Neem contact op met uw netwerkbeheerder voor hulp bij het oplossen van dit probleem. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support. |
|Adres van IPv4 (of IPv6) voor 'Data0' is off line.| |De netwerkbron 'Data0' met het IP-adres '10.0.0.1.' en de prefix-lengte '22' op het apparaat *<device1>* offline is. Zorg ervoor dat de poorten waarop deze interface is aangesloten operationeel. Ga naar [problemen met de cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet)voor het oplossen van netwerkproblemen. |
 

### <a name="performance-alerts"></a>Prestatiesignalen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|De belasting van het apparaat heeft <*drempel*> overschreden.|Langzamer dan verwacht responstijden.|Het apparaat meldt gebruik onder een zware belasting voor de invoer/uitvoer. Dit kan ervoor zorgen dat het apparaat werkt niet goed het moet. Bekijk de werklast hebt aangesloten op het apparaat en als die er zijn te bepalen dat kan worden verplaatst naar een ander apparaat of die niet langer nodig.<br>Om de huidige status te begrijpen, gaat u naar [de service Manager StorSimple te controleren van uw apparaat gebruiken](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Beveiligingswaarschuwingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Microsoft Support-sessie is gestart.|Derden toegang tot support-sessie.|Controleer of dat deze toegang is toegestaan. Nadat u de passende maatregelen hebben genomen, schakelt u deze waarschuwing op de pagina waarschuwingen.|
|Wachtwoord voor <*element*> zal <*tijd*> verlopen.|Verlopen wachtwoord bijna is bereikt.|Uw wachtwoord wijzigen voordat het verloopt.|
|Informatie over de beveiligingsconfiguratie voor <*element-ID*> ontbreekt.||De volumes die zijn gekoppeld aan deze container volume kunnen niet worden gebruikt om de configuratie van uw StorSimple te repliceren. Om ervoor te zorgen dat uw gegevens veilig worden opgeslagen, is het raadzaam dat u de container van het volume en de volumes die zijn gekoppeld aan de container volume verwijderen. Nadat u de passende maatregelen hebben genomen, schakelt u deze waarschuwing op de pagina waarschuwingen.|
|<*nummer*> inlogpogingen voor <*element-ID*> is mislukt.|Meerdere aanmeldingspogingen mislukte.|Het apparaat kan worden aangevallen of een gemachtigde gebruiker probeert verbinding te maken met een onjuist wachtwoord.<ul><li>Neem contact op met uw geautoriseerde gebruikers en controleren dat deze pogingen van een betrouwbare bron zijn. Als u nog een groot aantal mislukte inlogpogingen Zie, kunt u extern beheer uitschakelen en contact opnemen met de netwerkbeheerder. Nadat u de passende maatregelen hebben genomen, schakelt u deze waarschuwing op de pagina waarschuwingen.</li><li>Controleer of het Snapshot Manager exemplaren zijn geconfigureerd met het juiste wachtwoord. Nadat u de passende maatregelen hebben genomen, schakelt u deze waarschuwing op de pagina waarschuwingen.</li></ul>Ga naar [een verlopen apparaatwachtwoord wijzigen](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password)voor meer informatie.|
|Een of meer fouten opgetreden tijdens het wijzigen van de sleutel data service.||Er zijn fouten opgetreden bij het wijzigen van de sleutel data service. Nadat de fouten hebt opgelost, voert u de `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet uit de Interface van Windows PowerShell voor StorSimple op uw apparaat bij te werken van de service. Als dit probleem zich blijft voordoen, neem dan contact op met Microsoft support. Nadat u het probleem hebt opgelost, schakelt u deze waarschuwing op de pagina waarschuwingen.|

### <a name="support-package-alerts"></a>Pakket waarschuwingen

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Maken van het support-pakket is mislukt.|StorSimple kan niet het pakket te genereren.|Probeer deze bewerking opnieuw. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support. Nadat het probleem is opgelost, schakelt u deze waarschuwing op de pagina waarschuwingen.|

### <a name="update-alerts"></a>Meldingen van updates

|Waarschuwingstekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|De hotfix is geïnstalleerd.|Software/firmware-update is voltooid.|De hotfix is geïnstalleerd op uw apparaat.|
|Handmatige updates die beschikbaar zijn.|Melding van beschikbare updates.|Gebruik de Windows PowerShell-Interface voor StorSimple op het apparaat om deze updates te installeren. <br>Voor meer informatie gaat u naar het [bijwerken van het apparaat StorSimple 8000-serie](storsimple-update-device.md).|
|Nieuwe updates beschikbaar.|Melding van beschikbare updates.|U kunt deze updates installeren vanaf de pagina **onderhoud** of met behulp van de Windows PowerShell-Interface voor StorSimple op uw apparaat. <br>Voor meer informatie gaat u naar het [bijwerken van het apparaat StorSimple 8000-serie](storsimple-update-device.md).|
|Kan geen updates te installeren.|Updates zijn niet geïnstalleerd.|Het systeem is niet de updates te installeren. U kunt deze updates installeren vanaf de pagina **onderhoud** of met behulp van de Windows PowerShell-Interface voor StorSimple op uw apparaat. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support. <br>Voor meer informatie gaat u naar het [bijwerken van het apparaat StorSimple 8000-serie](storsimple-update-device.md).|
|Kan niet automatisch controleren op nieuwe updates.|Automatische controle is mislukt.|U kunt handmatig controleren op nieuwe updates van de pagina **onderhoud** .|
|Nieuwe WUA agent beschikbaar.|Melding van de update beschikbaar.|De meest recente Windows Update Agent downloaden en installeren vanaf de Windows PowerShell-interface.|
|Versie van firmware onderdeel <*onderdeel-ID*> komt niet overeen met de hardware.|Firmware-updates zijn niet geïnstalleerd.|Neem contact op met Microsoft Support.|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple fouten en problemen met een apparaat operationeel](storsimple-troubleshoot-operational-device.md).

