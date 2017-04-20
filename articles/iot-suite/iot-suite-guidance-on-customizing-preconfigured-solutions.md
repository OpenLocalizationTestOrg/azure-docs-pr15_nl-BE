<properties
    pageTitle="Aanpassen, vooraf geconfigureerde oplossingen | Microsoft Azure"
    description="Biedt richtlijnen voor het aanpassen van de Azure IoT Suite vooraf geconfigureerde oplossingen."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Een vooraf geconfigureerde oplossing aanpassen

De vooraf geconfigureerde oplossingen voorzien zijn van de Suite Azure IoT aantonen dat de diensten in de suite werken samen om een end-to-end oplossing bieden. Vanuit dit uitgangspunt zijn er tal van plaatsen waar u kunt uitbreiden en aanpassen van de oplossing voor specifieke scenario's. De volgende secties beschrijven deze gemeenschappelijke punten hebben aangepast.

## <a name="finding-the-source-code"></a>De broncode zoeken

De broncode van de vooraf geconfigureerde oplossingen is beschikbaar op GitHub in de volgende bibliotheken:

- Externe controle: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Preventief onderhoud: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

De broncode van de vooraf geconfigureerde oplossingen vindt u voorbeelden van de patronen en procedures voor de implementatie van een oplossing voor IoT Azure IoT Suite met de functionaliteit voor end-to-end. Hier vindt u meer informatie wilt over het maken en distribueren van de oplossingen in de opslagplaatsen van de GitHub.

## <a name="changing-the-preconfigured-rules"></a>De vooraf geconfigureerde regels wijzigen

De externe monitoring oplossing omvat drie [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) taken apparaat informatie, Telemetrie en regels voor bedrijfslogica weergegeven voor de oplossing te implementeren.

En de syntaxis van de drie stream analytics taken wordt beschreven in de diepte in de [externe controle vooraf geconfigureerde oplossing scenario](iot-suite-remote-monitoring-sample-walkthrough.md). 

U kunt deze taken om het te wijzigen van de logica bewerken of specifieke logica toevoegen aan uw scenario. U kunt de Stream Analytics taken als volgt vinden:
 
1. Ga naar [Azure portal](https://portal.azure.com).
2. Ga naar de resourcegroep met dezelfde naam als uw oplossing IoT. 
3. Selecteer de Azure Stream Analytics taak die u wilt wijzigen. 
4. De taak stoppen door te **stoppen**te selecteren in de lijst met opdrachten. 
5. Bewerken, query, in- en uitgangen.

    Een eenvoudige wijziging is het wijzigen van de query voor de taak van de **regels voor** gebruik van een **"<"** in plaats van een **' > '**. De portal oplossing worden nog steeds **">"** wanneer u een regel bewerken, maar u zult u merken dat het probleem door de wijziging in de onderliggende taak wordt gespiegeld.

6. De taak starten

> [AZURE.NOTE] Het remote monitoring dashboard afhankelijk van specifieke gegevens, zodat het dashboard niet wijzigen van de taken kan veroorzaken.

## <a name="adding-your-own-rules"></a>Uw eigen regels toe te voegen

Naast de vooraf geconfigureerde Azure Stream Analytics baan verandert, kunt u de Azure portal voor nieuwe taken toevoegen of nieuwe query's toevoegen aan bestaande projecten.

## <a name="customizing-devices"></a>Apparaten aanpassen

Een van de meest voorkomende activiteiten van extensie werkt met apparaten die specifiek zijn voor uw scenario. Er zijn verschillende methoden voor het werken met apparaten. Deze methoden omvatten een gesimuleerde apparaat zodat deze overeenkomen met uw scenario wijzigen of met behulp van de [SDK van IoT apparaat][] verbinding maken met het fysieke apparaat de oplossing.

Zie voor een stapsgewijze handleiding voor het toevoegen van apparaten naar de externe controle vooraf geconfigureerde oplossing, [Iot Suite verbinding maken met apparaten](iot-suite-connecting-devices.md) en [Externe bewaking C SDK monster](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) dat is ontworpen voor gebruik met de externe controle vooraf geconfigureerde oplossing.

### <a name="creating-your-own-simulated-device"></a>Maken van uw eigen gesimuleerde apparaat

Opgenomen in de externe monitoring oplossing broncode (waarnaar hierboven wordt verwezen), is een .NET-simulator. Deze simulator is een ingericht als onderdeel van de oplossing en andere metagegevens, telemetrie verzenden of reageren op verschillende opdrachten kan worden gewijzigd.

De vooraf geconfigureerde simulator in de externe controle vooraf geconfigureerde oplossing is een koeler dat telemetrie, temperatuur en vochtigheid genereert, kunt u de simulator in het [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) -project wanneer u hebt de opslagplaats GitHub forked.

### <a name="available-locations-for-simulated-devices"></a>Beschikbare locaties voor gesimuleerde apparaten

Er is een reeks locaties in Seattle/Redmond, Washington, Verenigde Staten van Amerika. Kunt u deze locaties in [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Opstellen en gebruiken van uw eigen (fysiek) apparaat

De [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) bieden bibliotheken voor het verbinden van verschillende typen apparaten (talen en besturingssystemen) in IoT oplossingen.

## <a name="modifying-dashboard-limits"></a>Wijzigen maximum van dashboard

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Aantal apparaten weergegeven in de vervolgkeuzelijst voor dashboard

De standaardwaarde is 200. Kunt u dit nummer in [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Aantal pins in Bing Map control weergeven

De standaardwaarde is 200. Kunt u dit nummer in [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Periode van telemetrie grafiek

De standaardwaarde is 10 minuten. U kunt dit wijzigen in [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Handmatig instellen van de toepassingsrollen

De volgende procedure wordt beschreven hoe u **Admin** en **ReadOnly** Toepassingsrollen toevoegen aan een vooraf geconfigureerde oplossing. Rekening mee dat vooraf geconfigureerde oplossingen ingericht op de site azureiotsuite.com, al de rollen **beheerder** en **alleen-lezen** .

Leden van de **alleen-lezen** -rol ziet het dashboard en de lijst met apparaten, maar mogen geen apparaten toevoegen of opdrachten verzenden apparaat kenmerken wijzigen.  Leden van de rol **Admin** hebben volledige toegang tot alle functies in de oplossing.

1. Ga naar de [Azure klassieke portal][lnk-classic-portal].

2. Selecteer **Active Directory**.

3. Klik op de naam van de AAD huurder die u gebruikt bij het inrichten van uw oplossing.

4. Klik op **toepassingen**.

5. Klik op de naam van de toepassing die overeenkomt met de naam van de vooraf geconfigureerde oplossing. Als uw toepassing in de lijst niet wordt weergegeven, wordt select **Mijn bedrijf eigenaar is van toepassingen** in de **Show** vervolgkeuzelijst en klikt u op het vinkje.

6.  Onderaan op de pagina, klik op **Beheren Manifest** **Manifest downloaden**.

7. Dit een .json-bestand op uw lokale computer gedownload.  Dit bestand openen voor bewerken in een teksteditor van uw keuze.

8. Op de derde regel van het bestand .json vindt u:

  ```
  "appRoles" : [],
  ```
  Dit bestand vervangen door het volgende:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Sla het bijgewerkte .json-bestand (u kunt het bestaande bestand overschrijven).

10.  Selecteer **Manifest beheren** en vervolgens **Uploaden Manifest** voor het uploaden van de .json-bestand dat u in de vorige stap hebt opgeslagen in Azure Management Portal onder aan de pagina.

11. U hebt nu de rollen **beheerder** en **alleen-lezen** toegevoegd aan uw toepassing.

12. Een van deze rollen toewijzen aan een gebruiker in de directory, Zie [machtigingen op de site azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Feedback

Hebt u een aanpassing u zou willen zien in dit document vallen? Voeg functie suggesties voor [Gebruiker spraak](https://feedback.azure.com/forums/321918-azure-iot)of opmerking bij dit artikel hieronder. 

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de opties voor het aanpassen van de vooraf geconfigureerde oplossingen, Zie:

- [Logica App verbinding met uw Azure IoT Suite externe controle vooraf geconfigureerde oplossing][lnk-logicapp]
- [Gebruik dynamische telemetrie van de vooraf geconfigureerde oplossing voor externe controle][lnk-dynamic]
- [Metagegevens voor apparaten informatie in de externe controle vooraf geconfigureerde oplossing][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT apparaat SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
