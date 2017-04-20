<properties
     pageTitle="De Azure portal gebruiken voor het maken van een IoT Hub | Microsoft Azure"
     description="Meer informatie over het maken en beheren van Azure IoT hubs via het portal Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Maak een IoT hub met de Azure portal

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Inleiding

In dit artikel wordt beschreven hoe de IoT Hub service vinden in de portal Azure, maken en beheren van IoT hubs.

## <a name="where-to-find-iot-hubs"></a>Waar vind ik IoT hubs

Er zijn verschillende plaatsen u de IoT hubs vindt.

1. **+ Nieuw**: **Azure IoT Hub** is een IoT-service en kan worden gevonden in de categorie **Internet van dingen**, onder **+ Nieuw**, vergelijkbaar met andere services.

2. Hubs IoT ook toegankelijk via de markt als de held service onder **Internet van dingen**.

## <a name="create-an-iot-hub"></a>Maak een hub IoT

U kunt een IoT hub met behulp van de volgende manieren maken:

- Maken van een hub IoT via de optie **+ Nieuw** leidt tot het blad weergegeven in de volgende schermafdruk. De stappen voor het maken van de hub IoT via deze methode en de markt zijn identiek.

- Maken van een hub IoT via de markt: op **maken** klikt, wordt een bladeserver die identiek is aan het vorige blad voor de **+ nieuwe** ervaring. De volgende secties lijst de verschillende stappen bij het maken van een hub IoT.

### <a name="choose-the-name-of-the-iot-hub"></a>Kies de naam van de hub IoT

Een hub IoT moet u de naam de hub. Deze naam moet uniek zijn binnen de hubs. Geen duplicatie van hubs is toegestaan op de back-end, zodat het wordt aanbevolen dat deze hub is een unieke naam hebben als mogelijk.

### <a name="choose-the-pricing-tier"></a>Kies de prijzen laag

U kunt kiezen uit vier lagen: **vrije**, **standaard 1** en **2 standaard**en **Standaard S3**. De vrij laag kan slechts 500 apparaten worden aangesloten op de hub IoT en maximaal 8.000 berichten per dag.

**Standaard S1**: IoT Hubs-S1 edition is ontworpen voor IoT oplossingen die een groot aantal apparaten genereren van relatief kleine hoeveelheden gegevens per apparaat. Elke eenheid van de S1 edition kunt maximaal 400.000 berichten per dag over alle aangesloten apparaten.

**Standaard S2**: IoT Hub S2 editie is bedoeld voor IoT oplossingen waarin apparaten grote hoeveelheden gegevens genereren. Elke eenheid van de S2 edition kunt maximaal 6 miljoen berichten per dag tussen alle aangesloten apparaten.

**Standaard S3**: IoT Hub S3 editie is bedoeld voor IoT oplossingen die grote hoeveelheden gegevens te genereren. Elke eenheid van de S3-editie kan maar liefst 300 miljoen berichten per dag tussen alle aangesloten apparaten.

![][4]

> [AZURE.NOTE] IoT Hub staat slechts één gratis hub per abonnement Azure.

### <a name="iot-hub-units"></a>IoT hub eenheden

Een hub IoT eenheid bevat een aantal berichten per dag. Het totale aantal berichten ondersteund voor deze hub is het aantal eenheden vermenigvuldigd met het aantal berichten per dag voor die laag. Bijvoorbeeld als u wilt dat de hub IoT ter ondersteuning van binnendringen van 700.000 berichten u twee eenheden van S1 laag.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Apparaat wolk partities en resourcegroep

U kunt het aantal partities voor een hub IoT wijzigen. Standaardpartities zijn ingesteld op 4; u kunt echter een ander aantal partities uit een vervolgkeuzelijst.

Voor resourcegroepen hoeft u niet expliciet een lege resourcegroep te maken. Wanneer u een resource maakt, kunt u een nieuwe resourcegroep maken of een bestaande brongroep wilt gebruiken.

![][5]

### <a name="choose-subscriptions"></a>Kies abonnementen

Azure IoT Hub wordt automatisch de lijst met Azure abonnementen waaraan de gebruikersaccount is gekoppeld. U kunt een van de opties de IoT hub met Azure abonnement koppelen.

### <a name="choose-the-location"></a>Kies de locatie

De optie locatie biedt een overzicht van de regio's waarin IoT Hub wordt aangeboden. IoT Hub beschikbaar is om te implementeren op de volgende locaties: Oost-Australië, Australië, Zuidoost, Oost-Azië, Zuidoost-Azië, Noord-Europa, West-Europa, Oost-Japan, Japan West ons Oost, West voor ons.

### <a name="create-the-iot-hub"></a>De hub IoT maken

Wanneer u alle voorgaande stappen zijn uitgevoerd, kan de IoT hub worden gemaakt. Klik op **maken** het back-end proces van het maken van deze hub IoT met specifieke opties te starten en te implementeren op de opgegeven locatie.

Het kan enkele minuten duren voordat de IoT hub als het duurt voor de back-end implementatie plaatsvindt in de juiste Locationservers worden gemaakt.

## <a name="change-the-settings-of-the-iot-hub"></a>Wijzig de instellingen van de hub IoT

Kunt u de instellingen van een bestaande IoT hub nadat deze is gemaakt van de bladeserver IoT Hub.

![][8]

**Gedeelde-beleid**: dit beleid bepaalt de machtigingen voor de apparaten en services verbinding maken met IoT Hub. Toegang tot dit beleid door te klikken op **Gedeelde-beleid** onder **Algemeen**. In deze blade, kunt u bestaande beleid wijzigen of toevoegen van een nieuw beleid.

### <a name="create-a-policy"></a>Een beleid maken

- Klik op **toevoegen** om een blade. Hier kunt u de naam van het nieuwe beleid en de machtigingen die u koppelen aan dit beleid, wilt zoals aangegeven in de volgende afbeelding.

    Er zijn verschillende machtigingen die gekoppeld aan gedeelde beleid worden kunnen. De eerste twee beleidsregels, **register gelezen** en het **register schrijven**, subsidie lezen en schrijven van toegangsrechten aan de winkel van de identiteit apparaat of het register van de identiteit. Automatisch kiezen van de optie schrijven kiest en het lezen optie.

    De **Service verbinding** beleid verleent machtiging voor toegang tot de cloud-side eindpunten zoals de groep consumenten voor services verbinding maken met de IoT-hub. Het **apparaat verbinding** beleid verleent machtigingen voor het verzenden en ontvangen van berichten op de eindpunten van de apparaat-zijde van de hub IoT.

- Klik op **maken** om de nieuwe beleid aan de bestaande lijst toevoegen.

![][10]

## <a name="messaging"></a>Messaging

Klik op **berichten** voor een lijst van de messaging-eigenschappen voor de IoT-hub die wordt gewijzigd. Er zijn twee typen eigenschappen die u kunt wijzigen of kopiëren: **wolk naar apparaat** en het **apparaat naar de Cloud**.

- **Wolk naar apparaat** instellingen: deze instelling heeft twee subsettings: **wolk naar apparaat TTL** (time-to-live) en de **retentietijd** voor de berichten. Wanneer de IoT hub wordt gemaakt, worden deze beide instellingen met een standaardwaarde van één uur gemaakt. Deze waarden aanpassen, gebruikt u de schuifregelaars of typ de waarden.

- Apparaatinstellingen **naar Cloud** : deze instelling heeft verschillende subsettings, waarvan sommige met de naam/toegewezen zijn wanneer de IoT hub wordt gemaakt en kan alleen worden gekopieerd naar andere subsettings die kunnen aangepast worden. Deze instellingen worden weergegeven in de volgende sectie.

**Partities**: deze waarde wordt ingesteld wanneer de IoT hub wordt gemaakt en door deze instelling kan worden gewijzigd.

**Hub compatibele gebeurtenis- en eindpunt**: wanneer de IoT hub wordt gemaakt, wordt een gebeurtenis Hub gemaakt intern mogelijk moet u toegang tot onder bepaalde omstandigheden. Deze gebeurtenis Hub-compatibele naam en het eindpunt kan niet worden aangepast, maar is beschikbaar voor gebruik via de knop **kopiëren** .

**Retentietijd**: standaard ingesteld op één dag, maar andere waarden met behulp van de vervolgkeuzelijst kan worden aangepast. Deze waarde is in dagen voor het apparaat naar de Cloud en niet in uren, evenals het soortgelijke instelling voor wolk naar apparaat.

**Groepen consumenten**: groepen consumenten zijn een instelling die vergelijkbaar is met andere systemen voor berichtenafhandeling die kunnen worden gebruikt om gegevens op specifieke manieren verbinding maken met andere toepassingen of services op IoT-Hub. Elke hub IoT is gemaakt met een standaardgroep voor de consument. U kunt echter toevoegen of verwijderen van groepen consumenten aan uw IoT hubs.

> [AZURE.NOTE] De standaard consument-groep kan niet worden bewerkt of verwijderd.

![][11]

## <a name="pricing-and-scale"></a>Prijzen en schaal

De prijs van een bestaande IoT hub kan worden gewijzigd via de instellingen van de **prijzen** , met de volgende uitzonderingen:

- In de huidige implementatie een IoT-hub met een gratis SKU lagen niet wijzigen in een van de betaalde SKU's, of vice versa.
- Er kan alleen worden één vrij laag IoT hub in het abonnement op Azure.

![][12]

Verplaatsen van een hogere laag (S2 of S3) naar de onderste laag (S1 en S2) is alleen toegestaan als het aantal berichten voor die dag niet in conflict zijn. Bijvoorbeeld, als het aantal berichten per dag meer dan 400.000, kan de laag voor de hub IoT vervolgens worden gewijzigd. Echter, als u de laag S1 vervolgens de hub verminderd voor die dag.

## <a name="delete-the-iot-hub"></a>De hub IoT verwijderen

U kunt bladeren naar de IoT-hub die u verwijderen wilt door te klikken op **Bladeren**en kies de juiste hub te verwijderen. Klik op de knop **verwijderen** onder de hubnaam van de te verwijderen van de hub.

## <a name="next-steps"></a>Volgende stappen

Klik op deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

- [Bulk IoT apparaten beheren][lnk-bulk]
- [Gebruik cijfers][lnk-metrics]
- [Activiteiten controleren][lnk-monitor]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]
- [Beveilig uw IoT-oplossing uit de grond omhoog][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md