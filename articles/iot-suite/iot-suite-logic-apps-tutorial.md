<properties
  pageTitle="Azure IoT Suite en logica Apps | Microsoft Azure"
  description="Een zelfstudie over hoe Apps naar Azure IoT Suite logica aansluiten voor een bedrijfsproces."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Zelfstudie: Logica App verbinding met uw Azure IoT Suite externe controle vooraf geconfigureerde oplossing

De [Microsoft Azure IoT Suite] [ lnk-internetofthings] externe controle vooraf geconfigureerde oplossing is een goede manier om snel aan de slag met een end-to-end-functieset die een oplossing voor IoT verklaart. Deze zelfstudie doorloopt u App logica toevoegen aan uw Microsoft Azure IoT Suite-externe controle vooraf geconfigureerde oplossing. Deze stappen laten zien hoe u kunt ook uw IoT oplossing nog verder door het maken van verbinding met een bedrijfsproces.

_Als u een scenario voor het verstrekken een externe controle vooraf geconfigureerde oplossing zoekt, Zie [Zelfstudie: aan de slag met de vooraf geconfigureerde IoT oplossingen][lnk-getstarted]._

Voordat u deze zelfstudie, moet u:

- Externe controle dient vooraf geconfigureerde oplossing van uw abonnement op Azure.

- Maak een account SendGrid u voor het verzenden van een e-mailbericht waarmee het bedrijfsproces wordt geactiveerd. U kunt zich aanmelden voor een gratis proefperiode op [SendGrid](https://sendgrid.com/) door te klikken op **gratis proberen**. Nadat u hebt geregistreerd voor een gratis proefperiode account, moet u een [API-sleutel](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) te maken in SendGrid die machtigingen verleend aan de e-mailberichten verzenden. Deze API-sleutel moet u verderop in de handleiding.

Ervan uitgaande dat u al hebt ingericht uw externe controle vooraf geconfigureerde oplossing, gaat u naar de bronnengroep voor deze oplossing in [Azure portal][lnk-azureportal]. De resourcegroep heeft dezelfde naam als de naam van de oplossing bij het inrichten van uw remote monitoring oplossing gekozen. In de resourcegroep ziet u de ingerichte Azure bronnen voor uw oplossing, met uitzondering van de Azure Active Directory-toepassing die u in de klassieke Azure-Portal vinden kunt. De volgende schermafbeelding ziet u een voorbeeld van de **resourcegroep** blade voor een externe controle vooraf geconfigureerde oplossing:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Als u wilt, instellen de logica app gebruiken met de vooraf geconfigureerde oplossing.

## <a name="set-up-the-logic-app"></a>De logica App instellen

1. Klik op __toevoegen__ aan de bovenkant van de resource groep blade in Azure portal.

2. Zoeken naar __Logica App__, selecteer het en klik op **maken**.

3. Vul de __naam__ en dezelfde **resourcegroep** die u gebruikt bij het inrichten van uw oplossing voor externe controle en **abonnement** . Klik op __maken__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Wanneer de installatie is voltooid, ziet u dat de App logica wordt vermeld als een resource in de resourcegroep.

5. Klik op de App logica om te navigeren naar de blade logica App, selecteer de sjabloon **Lege logica App** de **Logica Apps Designer**te openen.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Selecteer __vragen__. Deze actie geeft aan dat een binnenkomende HTTP-verzoek met een specifieke JSON payload fungeert als een trigger opgemaakt.

7. Plak het volgende in het Schema instantie JSON aanvragen:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] U kunt de URL van de HTTP post kopiëren nadat u de app logica opslaan, maar u moet eerst een actie toevoegen.

8. Klik op __+ nieuwe stap__ onder de handmatige trigger. Klik vervolgens op **een actie toevoegen**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Zoeken naar **SendGrid - e-mailbericht verzenden** en klik erop.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Voer een naam voor de verbinding, zoals **SendGridConnection**, voert u de **API-sleutel SendGrid** u hebt gemaakt toen u uw SendGrid-account instellen en klik op **maken**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. E-mailadressen die u aan zowel de velden **van** en **naar** de eigenaar toevoegen. **Externe controle van de waarschuwing [DeviceId]** toevoegen aan het veld **Subject** . Voeg in het veld **E-mailadres Body** **apparaat [DeviceId] [measurementName] met waarde [measuredValue] heeft gemeld.** **[DeviceId]** **[measurementName]**en **[measuredValue]** kunt u toevoegen door te klikken in de sectie **kunt u gegevens uit de voorgaande stappen** .

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. Klik op __Opslaan__ in het bovenste menu.

13. Klik op de trigger **aanvragen** en de waarde van de __Http Post naar deze URL__ kopiëren. Deze URL moet u verderop in deze handleiding.

> [AZURE.NOTE] Logica Apps kunnen u [verschillende soorten actie] uitvoeren[ lnk-logic-apps-actions] met inbegrip van acties in Office 365. 

## <a name="set-up-the-eventprocessor-web-job"></a>De EventProcessor Web taak instellen

In deze sectie kunt u de vooraf geconfigureerde oplossing de logica App die u hebt gemaakt. Om deze taak, kunt u de URL om te activeren, de App logica voor een actie die wordt geactiveerd wanneer een apparaat sensor waarde hoger is dan een drempel toevoegen.

1. Gebruik uw client git klonen van de meest recente versie van de [azure-iot-afstandsbediening-controle op github opslagplaats][lnk-rmgithub]. Bijvoorbeeld:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Open de __RemoteMonitoring.sln__ uit de lokale kopie van de bibliotheek in Visual Studio.

3. Open het bestand __ActionRepository.cs__ in de **infrastructuur\\opslagplaats** map.

4. De **actionIds** -woordenlijst bijwerken met de __Http Post naar deze URL__ die u van uw App logica genoteerd als volgt:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Sla de wijzigingen in de oplossing en afsluiten van Visual Studio.

## <a name="deploy-from-the-command-line"></a>Implementeren vanaf de opdrachtregel

In deze sectie kunt implementeren u de bijgewerkte versie van de te vervangen door de versie die wordt uitgevoerd in Azure externe monitoring oplossing.

1. Na de [set-up dev] [ lnk-devsetup] instructies voor het instellen van uw omgeving voor de implementatie.

2.  Voor de implementatie van lokaal, gaat u als volgt de [lokale implementatie] [ lnk-localdeploy] instructies.

3.  Wilt implementeren naar de cloud en uw bestaande wolk implementatie bijwerken, gaat u als volgt de [implementatie van de wolk] [ lnk-clouddeploy] instructies. De naam van de oorspronkelijke implementatie gebruiken als de naam van de implementatie. Voorbeeld als de oorspronkelijke installatie werd genoemd **demologicapp**, de volgende opdracht gebruiken:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Wanneer de build-script wordt uitgevoerd, moet u dezelfde Azure account, abonnement, regio en Active Directory-exemplaar die u hebt gebruikt bij het inrichten van de oplossing.

## <a name="see-your-logic-app-in-action"></a>Zie uw App logica in actie

De externe controle vooraf geconfigureerde oplossing heeft twee regels zijn standaard ingesteld wanneer u gegevens leveren voor een oplossing. Beide regels zijn op het **SampleDevice001** -apparaat:

* Temperatuur > 38.00
* Vochtigheid > 48,00

De temperatuur regel de gebeurtenis wordt geactiveerd **Alarm verhogen** en activeert de actie **SendMessage** de vochtigheid-regel. Als dat u de klasse **ActionRepository** dezelfde URL voor beide acties gebruikt, uw logica app voor elke regel wordt geactiveerd. Beide regels met SendGrid kunt een e-mail sturen naar het adres **op** met de details van de waarschuwing.

> [AZURE.NOTE] De logica App blijft geactiveerd telkens wanneer de drempel is bereikt. Om te voorkomen dat onnodige e-mails, kunt u de regels in uw oplossing portal uitschakelen of uitschakelen van de logica in de [portal Azure]App[lnk-azureportal].

Naast e-mails ontvangen, kunt u ook zien wanneer de App logica wordt uitgevoerd in de portal:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u een App logica een bedrijfsproces met de vooraf geconfigureerde oplossing gebruikt hebt, kunt u meer informatie over de opties voor het aanpassen van de vooraf geconfigureerde oplossingen:

- [Gebruik dynamische telemetrie van de vooraf geconfigureerde oplossing voor externe controle][lnk-dynamic]
- [Metagegevens voor apparaten informatie in de externe controle vooraf geconfigureerde oplossing][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
