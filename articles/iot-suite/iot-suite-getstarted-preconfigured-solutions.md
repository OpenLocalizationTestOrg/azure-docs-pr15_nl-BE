<properties
    pageTitle="Aan de slag met vooraf geconfigureerde oplossingen | Microsoft Azure"
    description="Volg deze zelfstudie voor meer informatie over het implementeren van een Suite van Azure IoT vooraf geconfigureerde oplossing."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Zelfstudie: Aan de slag met de vooraf geconfigureerde oplossingen

## <a name="introduction"></a>Inleiding

Azure IoT Suite [vooraf geconfigureerde oplossingen] [ lnk-preconfigured-solutions] combineren meerdere IoT Azure services tot end-to-end oplossingen die bedrijfsscenario's IoT implementeren. De *externe controle* vooraf geconfigureerde oplossing verbonden met en bewaakt uw apparaten. U kunt de oplossing voor het analyseren van de stroom van gegevens van uw apparaten en zakelijke resultaten verbeteren door processen automatisch reageren op deze stroom van gegevens.

In deze zelfstudie wordt beschreven hoe u de externe controle vooraf geconfigureerde oplossing te creëren. Ook begeleidt u bij de basisvoorzieningen van de oplossing voor externe controle. U kunt toegang krijgen tot veel van deze functies via het dashboard oplossing die samen met de vooraf geconfigureerde oplossing implementeert:

![Externe controle vooraf geconfigureerde oplossing dashboard][img-dashboard]

Als u deze zelfstudie hebt voltooid, moet u een geldig abonnement Azure.

> [AZURE.NOTE]  Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Het dashboard oplossing bekijken

Het dashboard oplossing kunt u de geïmplementeerde oplossing te beheren. U kunt bijvoorbeeld telemetrie weergeven, toevoegen van apparaten en regels configureren.

1.  De inrichting is voltooid en de tegel voor uw vooraf geconfigureerde oplossing geeft **nu**aan, klik op **starten** als u wilt uw remote monitoring oplossing portal in een nieuw tabblad openen.

    ![De vooraf geconfigureerde oplossing starten][img-launch-solution]

2.  Standaard bevat het portaal oplossing het *dashboard oplossing*. U kunt andere weergaven met behulp van het menu links.

    ![Externe controle vooraf geconfigureerde oplossing dashboard][img-dashboard]

Het dashboard wordt de volgende informatie weergegeven:

- De kaart wordt de locatie van elk apparaat dat is verbonden met de oplossing. Wanneer u de oplossing voor het eerst uitvoert, zijn er vier gesimuleerde apparaten. De gesimuleerde apparaten worden geïmplementeerd als Azure, WebJobs en de oplossing de Bing Maps-API gebruikt om informatie op de kaart uitzetten.
- Het deelvenster **Historie telemetrie** telemetrie vochtigheid en temperatuur op een geselecteerd apparaat in in de buurt van real-time en geeft statistische gegevens zoals maximum, minimum en gemiddelde vochtigheid worden uitgezet.
- Het deelvenster **Historie Alarm** bevat recente gebeurtenissen alarm wanneer een waarde telemetrie een drempel heeft overschreden. U kunt uw eigen alarm Bekijk de voorbeelden gemaakt door de vooraf geconfigureerde oplossing definiëren.

## <a name="view-the-device-list"></a>De lijst met apparaten weergeven

De lijst met weergegeven alle geregistreerde apparaten in de oplossing. U weer Apparaatmetagegevens bewerken, toevoegen of verwijderen van apparaten en opdrachten verzenden naar apparaten.

1.  Klik op **apparaten** in het menu links van de *lijst met apparaten* voor deze oplossing.

    ![Lijst met apparaten in het dashboard][img-devicelist]

2.  Er zijn vier gesimuleerde apparaten gemaakt door het inrichtingsproces ziet u de lijst met apparaten.

3.  Klik op een apparaat in de lijst met apparaten het Apparaatdetails bekijken.

    ![Apparaatdetails van het in dashboard][img-devicedetails]

Het deelvenster **Details van het apparaat** bevat drie secties:

- In de sectie **Acties** worden de acties die u op het apparaat uitvoeren kunt. Als u het apparaat uitschakelt, is het niet langer toegestaan telemetrie verzenden of ontvangen van opdrachten. Als u een apparaat uitschakelt, vervolgens kunt u het opnieuw. U kunt een regel die is gekoppeld aan het apparaat die een alarm activeert wanneer een telemetrie-waarde hoger is dan een drempel toevoegen. U kunt ook een opdracht om een apparaat te verzenden. Wanneer een apparaat eerst verbinding maakt, krijgt de oplossing de opdrachten die kan op reageren.
- In de sectie **Eigenschappen van een apparaat** worden de Apparaatmetagegevens. Sommige van deze metagegevens afkomstig zijn van het apparaat zelf (zoals de fabrikant) en enkele wordt gegenereerd door de oplossing (zoals de tijd gemaakt). U kunt de Apparaatmetagegevens vanaf hier bewerken.
- In de sectie **Verificatiesleutels** worden de sleutels die het apparaat gebruiken kunt voor het verifiëren van de oplossing.

## <a name="send-a-command-to-a-device"></a>Een opdracht naar een apparaat sturen

Het detailvenster apparaat bevat alle opdrachten die door een bepaald apparaat worden ondersteund en kunt u voor het verzenden van opdrachten naar een apparaat. Wanneer u een apparaat de eerste keer start, wordt informatie over de ondersteunde opdrachten verzonden naar de oplossing.

1.  Klik op **opdrachten** in het detailvenster van het apparaat voor het geselecteerde apparaat.

    ![Apparaatopdrachten in het dashboard][img-devicecommands]

2.  Selecteer **PingDevice** in de opdrachtenlijst.

3.  Klik op de **opdracht verzenden**.

4.  Hier ziet u de status van de opdracht in de opdrachtgeschiedenis.

    ![Opdrachtstatus in het dashboard][img-pingcommand]

De oplossing houdt de status van elke opdracht die wordt verzonden. Het resultaat is in eerste instantie **in behandeling**. Wanneer het apparaat wordt gemeld dat het de opdracht is uitgevoerd, wordt het resultaat ingesteld op **succes**.

## <a name="add-a-new-simulated-device"></a>Een nieuwe gesimuleerde apparaat toevoegen

Als u de vooraf geconfigureerde oplossing implementeert, worden automatisch vier monster-apparaten die u in de lijst met apparaten zien kunt inrichten. Deze apparaten zijn *gesimuleerde apparaten* in een Azure-WebJob uitgevoerd. Gesimuleerde apparaten maken het gemakkelijk voor u om te experimenteren met de vooraf geconfigureerde oplossing zonder te hoeven implementeren echte apparaten. Zie [verbinding maken met het apparaat naar de externe controle vooraf geconfigureerde oplossing] als u een echt apparaat verbinden met de oplossing wilt,[ lnk-connect-rm] zelfstudie.

De volgende stappen laten zien hoe u een gesimuleerde apparaat aan de oplossing toevoegen:

1.  Ga terug naar de lijst met apparaten.

2.  Klik op **+ toevoegen van een apparaat** in de linkerbenedenhoek op een apparaat toevoegen.

    ![Een apparaat toevoegen aan de vooraf geconfigureerde oplossing][img-adddevice]

3.  Klik op **Nieuwe toevoegen** in de tegel van het **Gesimuleerde apparaat** .

    ![Apparaatdetails van het nieuwe instellen in dashboard][img-addnew]
    
    Naast het maken van een nieuw gesimuleerde apparaat, kunt u ook een fysiek apparaat toevoegen als u een **Aangepast apparaat**maken. Zie [verbinding maken met het apparaat op de externe controle vooraf geconfigureerde oplossing IoT-Suite]voor meer informatie over het aansluiten van de fysieke apparaten die op de oplossing,[lnk-connect-rm].

4.  Selecteer **Ik wil mijn eigen apparaat-ID definiëren**en voer een unieke ID naam zoals **mydevice_01**.

5.  Klik op **maken**.

    ![Opslaan van een nieuw apparaat][img-definedevice]

6. In stap 3 van **een gesimuleerde apparaat toevoegen**, klikt u op **Gereed** om terug te keren naar de lijst met apparaten.

7. U kunt uw apparaat **uitgevoerd** weergeven in de lijst met apparaten.

    ![Nieuw apparaat in de lijst met apparaten weergeven][img-runningnew]

8. U kunt ook de gesimuleerde telemetrie van uw nieuwe apparaat op het dashboard weergeven:

    ![Telemetrie van het nieuwe apparaat weergeven][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Het Apparaatmetagegevens bewerken

Wanneer een apparaat eerst verbinding maakt met de oplossing, stuurt de metagegevens aan de oplossing. Als u de Apparaatmetagegevens via het dashboard oplossing bewerkt, verzendt de nieuwe metagegevenswaarden naar het apparaat en worden de nieuwe waarden opgeslagen in de database van de DocumentDB oplossing. Zie voor meer informatie, [apparaat-id-register en DocumentDB][lnk-devicemetadata].

1.  Ga terug naar de lijst met apparaten.

2.  Het nieuwe apparaat selecteert in de **Lijst met apparaten**en klik vervolgens op **bewerken** om de **Eigenschappen van een apparaat**:

    ![Apparaatmetagegevens bewerken][img-editdevice]

3. Schuif naar beneden en een wijziging aanbrengt in de breedte- en lengtewaarden vales. Klik vervolgens op **wijzigingen in het register apparaat opslaan**.

    ![Apparaatmetagegevens bewerken][img-editdevice2]

4. Ga terug naar het dashboard, de locatie van het apparaat is gewijzigd op de kaart:

    ![Apparaatmetagegevens bewerken][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Een regel voor het nieuwe apparaat toevoegen

Er zijn geen regels voor het nieuwe apparaat dat u zojuist hebt toegevoegd. In deze sectie kunt toevoegen u een regel waarmee een alarm wordt gegenereerd wanneer de temperatuur gemeld door het nieuwe apparaat meer dan 47 graden. Voordat u begint, u ziet dat de telemetrie-geschiedenis van het nieuwe apparaat op het dashboard geeft dat de temperatuur van het apparaat nooit meer dan 45 graden.

1.  Ga terug naar de lijst met apparaten.

2.  Het nieuwe apparaat selecteert in de **Lijst met apparaten**en klik vervolgens op **regel toevoegen** om een regel voor het apparaat toevoegen.

3. Een regel maken die **temperatuur** gebruikt als het gegevensveld en gebruikt u **AlarmTemp** als de uitvoer wanneer de temperatuur hoger is dan 47 graden:

    ![De regel van een apparaat toevoegen][img-adddevicerule]

4. Klik op **Opslaan en regels weergeven** als uw wijzigingen wilt opslaan.

5.  Klik op **opdrachten** in het detailvenster van het apparaat voor het nieuwe apparaat.

    ![De regel van een apparaat toevoegen][img-adddevicerule2]

6.  **ChangeSetPointTemp** selecteren uit de opdrachtenlijst met- en **SetPointTemp** ingesteld op 45. Vervolgens klikt u op de **Opdracht verzenden**:

    ![De regel van een apparaat toevoegen][img-adddevicerule3]

7.  Ga terug naar het dashboard oplossing. Na een korte tijd ziet u een nieuwe vermelding in het deelvenster **Historie Alarm** als de temperatuur gemeld door het nieuwe apparaat groter is dan de drempelwaarde 47 graden:

    ![De regel van een apparaat toevoegen][img-adddevicerule4]

8. U kunt bekijken en bewerken van alle regels op de pagina **regels** van het dashboard:

    ![Lijst apparaat regels][img-rules]

9. U kunt bekijken en bewerken van de acties die kunnen worden ondernomen in reactie op een regel op de pagina **Acties** van het dashboard:

    ![Lijst apparaat acties][img-actions]

> [AZURE.NOTE] Het is mogelijk voor het definiëren van acties die u kunnen een e-mailbericht of een SMS in antwoord op een regel versturen of integreren met een bedrijfs systeem via een [App logica][lnk-logic-apps]. Zie voor meer informatie de [logica App verbinding naar uw Azure IoT Suite externe controle vooraf geconfigureerde oplossing][lnk-logicapptutorial].

## <a name="other-features"></a>Andere functies

Met behulp van de oplossing-portal die kunt u zoeken voor apparaten met specifieke kenmerken, zoals een model:

![Zoeken naar een apparaat][img-search]

U kunt een apparaat uitschakelen en nadat deze is uitgeschakeld kunt u deze verwijderen:

![Uitschakelen en verwijderen van een apparaat][img-disable]

## <a name="behind-the-scenes"></a>Achter de schermen

Wanneer u een vooraf geconfigureerde oplossing implementeert, maakt het implementatieproces meerdere resources in de Azure abonnement dat u hebt geselecteerd. U kunt deze bronnen weergeven in Azure [portal][lnk-portal]. Het implementatieproces maakt een **bronnengroep** met een naam op basis van de naam die u voor uw vooraf geconfigureerde oplossing kiest:

![Vooraf geconfigureerde oplossing in Azure portal][img-portal]

U kunt de instellingen van elke resource weergeven door deze te selecteren in de lijst met resources in de resourcegroep.

U kunt ook de broncode van de vooraf geconfigureerde oplossing weergeven. De externe controle vooraf geconfigureerde oplossing broncode in de [azure-iot-afstandsbediening-controle] is[ lnk-rmgithub] GitHub opslagplaats:

- De map **DeviceAdministration** bevat de broncode van het dashboard.
- De map **Simulator** bevat de broncode van het gesimuleerde apparaat.
- De map **EventProcessor** bevat de broncode voor de back-end proces dat zorgt voor de inkomende telemetrie.

Wanneer u klaar bent, kunt u de vooraf geconfigureerde oplossing verwijderen uit uw Azure abonnement op de [azureiotsuite.com] [ lnk-azureiotsuite] site. Deze site kunt u gemakkelijk verwijderen van alle bronnen die tijdens het maken van de vooraf geconfigureerde oplossing zijn ingericht.

> [AZURE.NOTE] Om ervoor te zorgen dat u alles met betrekking tot de vooraf geconfigureerde oplossing verwijderen, verwijderen van de [azureiotsuite.com] [ lnk-azureiotsuite] site en de resourcegroep in de portal niet gewoon verwijderen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een werkende vooraf geconfigureerde oplossing hebt geïmplementeerd, kunt u gaan aan de slag met IoT Suite door te lezen de volgende artikelen:

- [Externe controle vooraf geconfigureerde oplossing scenario][lnk-rm-walkthrough]
- [Verbind het apparaat met de externe controle vooraf geconfigureerde oplossing][lnk-connect-rm]
- [Machtigingen voor de site azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
