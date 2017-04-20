<properties
    pageTitle="Een IoT-oplossing maken met behulp van Analytics Stream | Microsoft Azure"
    description="Zelfstudie voor de Stream Analytics IoT-oplossing van een scenario tolhuisje Getting started"
    keywords="Functies venster-IOT oplossing"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Een IoT-oplossing maken met behulp van Analytics Stream

## <a name="introduction"></a>Inleiding

In deze zelfstudie leert u hoe Azure Stream Analytics met real-time inzicht krijgen in uw gegevens. Ontwikkelaars kunnen stromen van gegevens, zoals klikken streams en Logboeken gebeurtenissen gegenereerd met een apparaat gemakkelijk combineren met historische records of referentiegegevens voor het afleiden van inzicht. Azure Stream Analytics biedt een volledig beheerde, real-time stream berekenen van de service die wordt gehost in Microsoft Azure, ingebouwde tolerantie, lage latentie en schaalbaarheid om bent u in minuten wordt uitgevoerd.

Na het voltooien van deze zelfstudie, is het mogelijk om:

-   Lees de Azure Stream Analytics portal.
-   Configureren en implementeren van een streaming-project.
-   Kunt verwoorden concrete problemen en deze oplossen met behulp van de querytaal Stream Analytics.
-   Streaming oplossingen voor uw klanten met behulp van Analytics Stream met vertrouwen te ontwikkelen.
-   Gebruik de prestatiecontrole en -registratie ervaring oplossen van problemen.

## <a name="prerequisites"></a>Vereisten

U moet de volgende vereisten voor het voltooien van deze zelfstudie:

-   De nieuwste versie van [Azure PowerShell](../powershell-install-configure.md)
-   Visual Studio 2015 of de vrije [Visual Studio Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
-   Een [abonnement op Azure](https://azure.microsoft.com/pricing/free-trial/)
-   Beheerdersrechten op de computer
-   Downloaden van de [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) van het Microsoft Download Center
-   Optioneel: Broncode voor de TollApp gebeurtenis generator in [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Inleiding scenario: "Hello, gratis!"


Een gratis station is een algemeen verschijnsel. Tijdens deze vele autowegen, bruggen en tunnels over de hele wereld. Elk station gratis heeft meerdere gratis stands. Bij handmatige stands stopt u de gratis betalen met een daarmee gepaard gaande. Een sensor op elke stand scant op geautomatiseerde stands, een RFID-kaart die wordt aangebracht op de voorruit van uw voertuig terwijl u de gratis stand doorgeven. Het is makkelijk de doortocht door middel van deze gratis stations als de stroom van een gebeurtenis die interessante bewerkingen kunnen worden uitgevoerd.

![Afbeelding van auto's op gratis stands](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Binnenkomende gegevens

Deze zelfstudie werkt met twee stromen van gegevens. Sensoren in de in- en uitgangen van de stations gratis geïnstalleerd produceren de eerste stroom. De tweede stroom is een dataset statische opzoeken met de registratie van gegevens over het voertuig.

### <a name="entry-data-stream"></a>Post-gegevensstroom

De gegevensstroom post bevat informatie over auto's als ze gratis stations invoeren.


| TollID | EntryTime               | LicensePlate | Staat | Maken   | Model   | VehicleType | VehicleWeight | Gratis | Label       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 2014-09-10-12:01:00.000 | 7001 JNB     | NY    | Honda  | CRV     | 1           | 0             | 7    |           |
| 1      | 2014-09-10-12:02:00.000 | 1001 YXZ     | NY    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 2014-09-10-12:02:00.000 | ABC-1004     | CT    | Ford   | Taurus  | 1           | 0             | 5    | 456789123 |
| 2      | 2014-09-10-12:03:00.000 | XYZ 1003     | CT    | Toyota | Corolla | 1           | 0             | 4    |           |
| 1      | 2014-09-10-12:03:00.000 | BNJ-1007     | NY    | Honda  | CRV     | 1           | 0             | 5    | 789123456 |
| 2      | 2014-09-10-12:05:00.000 | COB-1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Hier volgt een korte beschrijving van de kolommen:

| Kolom | Beschrijving |
|--------------|--------------------------------------------------------------------|
| TollID       | De gratis stand-ID die een unieke aanduiding voor een gratis stand            |
| EntryTime    | De datum en tijd van binnenkomst van het voertuig aan de stand van de gratis in UTC |
| LicensePlate | Het kenteken van het voertuig                            |
| Staat        | Een staat in de Verenigde Staten                                           |
| Maken         | De fabrikant van de auto                                 |
| Model        | Het modelnummer van de auto                                 |
| VehicleType  | 1 voor personenwagens of 2 voor bedrijfsvoertuigen       |
| WeightType   | Voertuig gewicht in ton; 0 voor voertuigen voor personenvervoer                   |
| Gratis         | De waarde van de gratis in USD                                              |
| Label          | De e-Tag op de auto die betaling automatiseert; leeg wanneer de betaling is gedaan, handmatig |


### <a name="exit-data-stream"></a>De gegevensstroom afsluiten

De gegevensstroom afsluiten bevat informatie over auto's het verlaten van het station gratis.


| **TollId** | **ExitTime**                 | **LicensePlate** |
|------------|------------------------------|------------------|
| 1          | 2014-09-10T12:03:00.0000000Z | 7001 JNB         |
| 1          | 2014-09-10T12:03:00.0000000Z | 1001 YXZ         |
| 3          | 2014-09-10T12:04:00.0000000Z | ABC-1004         |
| 2          | 2014-09-10T12:07:00.0000000Z | XYZ 1003         |
| 1          | 2014-09-10T12:08:00.0000000Z | BNJ-1007         |
| 2          | 2014-09-10T12:07:00.0000000Z | COB-1007         |

Hier volgt een korte beschrijving van de kolommen:


| Kolom | Beschrijving |
|--------------|-----------------------------------------------------------------|
| TollID       | De gratis stand-ID die een unieke aanduiding voor een gratis stand         |
| ExitTime     | De datum en tijd van het verlaten van het voertuig gratis stand in UTC |
| LicensePlate | Het kenteken van het voertuig                         |

### <a name="commercial-vehicle-registration-data"></a>Registratiegegevens bedrijfsvoertuig

De zelfstudie wordt een statische momentopname van een bedrijfsvoertuig registratiedatabase.


| LicensePlate | Registratie-id | Verlopen |
|--------------|----------------|---------|
| 6023 SVT     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| BBV 1005     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Hier volgt een korte beschrijving van de kolommen:


| Kolom | Beschrijving |
|--------------|-----------------------------------------------------------------|
| LicensePlate | Het kenteken van het voertuig                         |
| Registratie-id     | Het Registratienummer van het voertuig |
| Verlopen | De registratiestatus van het voertuig: 0 als het voertuigregistratie actief is, 1 als de registratie is verlopen.                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>De omgeving voor Azure Stream Analytics instellen

Als u deze zelfstudie hebt voltooid, moet u een abonnement op Microsoft Azure. Microsoft biedt een gratis proefversie voor Microsoft Azure services.

Als u niet een Azure-account hebt, kunt u een [aanvraag voor een gratis evaluatieversie](http://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Als u wilt zich aanmelden voor een gratis proefperiode, moet u een mobiel apparaat kan ontvangen berichten met tekst en een geldige creditcard.

Zorg dat u de stappen in de sectie 'Uw account Azure opschonen' aan het einde van dit artikel zodat u optimaal gebruik maken van uw $200 vrij Azure creditnota kunt maken.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Azure bronnen die nodig zijn voor de zelfstudie inrichten

Deze zelfstudie is vereist twee gebeurtenis hubs *entry* en *exit* data streams te ontvangen. Azure SQL-Database voert de resultaten van de Stream Analytics taken. Azure opslag slaat referentiegegevens over voertuig-registraties.

U kunt het script Setup.ps1 in de map TollApp op GitHub maken alle vereiste bronnen. In het belang van de tijd, is het raadzaam dat u het uitvoert. Als u meer informatie over het configureren van deze bronnen in de portal Azure wilt, verwijzen naar de bijlage 'Configuring zelfstudie resources in Azure portal'.

Downloaden en opslaan van de ondersteunende map voor [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) en -bestanden.

Een **Microsoft Azure PowerShell** -venster openen _als beheerder_. Als u nog geen Azure PowerShell, volg de instructies in het [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) te installeren.

Omdat Windows automatisch geblokkeerd .ps1, .dll en .exe-bestanden, moet u het uitvoeringsbeleid instellen voordat u het script uitvoert. Zorg ervoor dat de Azure PowerShell-venster wordt uitgevoerd _als een beheerder_. **Onbeperkte Set uitvoeringsbeleid**worden uitgevoerd. Wanneer dat wordt gevraagd, typt u **Y**.

![Screenshot van 'Set-uitvoeringsbeleid onbeperkte"venster met Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

**Get uitvoeringsbeleid** uitvoeren om ervoor te zorgen dat de opdracht gewerkt.

![Screenshot van "Get-uitvoeringsbeleid" venster met Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Ga naar de map met de scripts en toepassing van de generator.

![Screenshot van "cd .\TollApp\TollApp' uitgevoerd in het venster Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Type **.\\ Setup.ps1** als u uw Azure-account instellen, maken en configureren van alle vereiste middelen en beginnen met het genereren van de gebeurtenissen. Het script neemt willekeurig een gebied voor het maken van uw resources. Als u expliciet een regio, geeft u de **-locatie** parameter als volgt:

**. \\Setup.ps1-locatie "Centraal US"**

![Schermafbeelding van de pagina voor Azure-in](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Het script wordt geopend **de aanmeldingspagina** voor Microsoft Azure. Geef uw accountreferenties.

> [AZURE.NOTE] Als uw account toegang tot meerdere abonnementen heeft, wordt u gevraagd de naam van het abonnement dat u wilt gebruiken voor de zelfstudie invoeren.

Het script kan enige tijd duren om uit te voeren. Nadat dit is voltooid, ziet de uitvoer de volgende schermafdruk.

![Schermafbeelding van de uitvoer van het script in het venster Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

U ziet ook een ander venster dat lijkt op het volgende screenshot. Deze toepassing verstuurt gebeurtenissen naar Azure gebeurtenis Hubs, is vereist voor het uitvoeren van de zelfstudie. Dus niet de toepassing stoppen en sluit dit venster totdat u klaar bent met de zelfstudie.

![Screenshot van "Gegevens verzenden gebeurtenis hub"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Worden alle gemaakte bronnen in Azure portal nu zien. Ga naar <https://manage.windowsazure.com>en meld u aan met uw referenties.

### <a name="azure-event-hubs"></a>Azure gebeurtenis Hubs

Klik op **SERVICE BUS** aan de linkerkant van de Azure portal overzicht hubs gebeurtenis die het script in de vorige sectie hebt gemaakt.

![Bus service](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Ziet u alle beschikbare naamruimten op uw abonnement. Klik op de instantie die met *tolldata begint* (tolldata4637388511 in ons voorbeeld). Klik op het tabblad **GEBEURTENIS HUBS** .

![Hubs tabblad gebeurtenis in de portal voor Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Ziet u twee gebeurtenis hubs *vermelding* met de naam en *af te sluiten* in deze naamruimte gemaakt.

![Screenshot van 'entry' en 'afsluiten' gebeurtenis hubs in Azure portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure opslag container

1. Klik op de **opslag** aan de linkerkant van de Azure portal overzicht van de container Azure opslag die in de zelfstudie wordt gebruikt.

    ![Menu-opslagitem](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Klik op de instantie die beginnen met *tolldata* (tolldata4637388511 in ons voorbeeld). Klik op het tabblad **CONTAINERS** voor de container gemaakt.

    ![Tabblad containers in Azure portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Klik op de container **tolldata** overzicht van de geüploade JSON-bestand met gegevens over het voertuig registratie.

    ![Screenshot van het bestand registration.json in de container](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Azure SQL-Database

1. Klik op **SQL-DATABASES** aan de linkerkant van de Azure portal overzicht van de SQL-database die wordt gebruikt in de zelfstudie.

    ![SQL-Databases](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Klik op **tolldatadb**.

    ![Schermafbeelding van de gemaakte SQL-database](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. Kopiëren zonder het poortnummer de naam van de server (*servername*. database.windows.net bijvoorbeeld).

## <a name="connect-to-the-database-from-visual-studio"></a>Verbinding maken met de database vanuit Visual Studio

Visual Studio gebruiken voor toegang tot de resultaten van de query in de Uitvoerdatabase.

Verbinding maken met de SQL-database (het doel) vanuit Visual Studio:

1. Visual Studio openen en klik op **Extra** > **verbinding maken met Database**.

2. Als u wordt gevraagd, klikt u op **Microsoft SQL Server** als gegevensbron.

    ![Het dialoogvenster gegevensbron wijzigen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. Plak in het veld **servernaam** de naam die u in de vorige sectie hebt gekopieerd vanaf de portal Azure (dat wil zeggen, de *servernaam*. database.windows.net).

4. Klik op **SQL Server-verificatie gebruiken**.

5. **Tolladmin** invoeren in het veld **gebruikersnaam** en **123toll!** in het veld **wachtwoord** .

6. **Selecteer of typ een databasenaam**op en selecteer **TollDataDB** als de database.

    ![Het dialoogvenster verbinding toevoegen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Klik op **OK**.

8. Server Explorer geopend.

    ![Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Zie vier tabellen in de database TollDataDB.

    ![Tabellen in de database TollDataDB](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Gebeurtenis generator: TollApp voorbeeldproject

Het PowerShell-script wordt automatisch gestart voor het verzenden van gebeurtenissen met het programma TollApp sample application. U hoeft niet alle aanvullende stappen uitvoeren.

Als u geïnteresseerd in de details van de uitvoering bent, kunt u de broncode van de toepassing van de TollApp vinden in GitHub [samples/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Schermafbeelding van de voorbeeldcode in Visual Studio worden weergegeven](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Maak een taak Stream Analytics

1. Open Stream Analytics en klik op **Nieuw** in de linkerbenedenhoek van de pagina een nieuwe analytics-taak maken in de portal voor Azure.

    ![Knop Nieuw](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Klik op **snel maken**. Selecteer dezelfde regio waar de andere bronnen worden gemaakt door het script.

3. Selecteer **Nieuwe opslag-ACCOUNT maken** voor de instelling van **Regionaal toezicht opslag ACCOUNT** , en een unieke naam geven. Azure Stream Analytics deze rekening gebruikt voor het opslaan van informatie over prestatiebewaking voor alle toekomstige projecten.

4. Klik op **STREAM ANALYTICS taak maken** onder aan de pagina.

    ![Stream Analytics taakoptie maken](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Invoerbronnen definiëren

1. Klik op de taak analytics gemaakt in de portal.

    ![Screenshot van de analytics-taak in de portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. Klik op het tabblad **invoer** om de brongegevens te definiëren.

    ![Het tabblad "inputs"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Klik op **een INGANG toevoegen**.

    ![Het toevoegen van een Input-optie](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. Klik op **de GEGEVENSSTROOM** op de eerste pagina.

    ![De optie gegevensstroom](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. Klik op **GEBEURTENIS HUB** op de tweede pagina van de wizard.

    ![De optie gebeurtenis Hub](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. **EntryStream** invoeren als **invoer ALIAS**.

7. Klik op de vervolgkeuzelijst **Hub van de gebeurtenis** en selecteer een die met 'TollData' (bijvoorbeeld TollData9518658221 begint).

8. Selecteer de **post** als de gebeurtenisnaam hub en **alle** als het beleid de naam van de hub.

    De instellingen zullen er als volgt uitzien:

    ![Gebeurtenis hub-instellingen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. Selecteer op de volgende pagina **JSON** voor **GEBEURTENIS SERIALISATIE-indeling** en **UTF8** voor **codering**.

    ![Instellingen voor serialisatie](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Klik op **OK** onder aan de pagina om de wizard te voltooien.

    ![Invoer in de portal Azure Screenshot van EntryStream](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Nu dat u de stroom post hebt gemaakt, volgt u dezelfde stappen uit als u wilt maken van de stroom afsluiten. Op de derde pagina van de wizard worden waarden als op de volgende screenshot.

    ![Instellingen voor de stroom afsluiten](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    U hebt twee input streams gedefinieerd:

    ![Gedefinieerde invoer streams in Azure portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Vervolgens voegt u gegevensinvoer verwijzing voor het blob bestand met Auto registratiegegevens.

11. Klik op **Invoer toevoegen**en klik vervolgens op **REFERENTIEGEGEVENS**.

    !["Een ingang toevoegen" opties referentiegegevens worden geselecteerd](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. Selecteer op de volgende pagina de opslag-rekening die met **tolldata begint**. De containernaam van de moet worden **tolldata**en de naam blob onder **Pad PATROON** moet worden **registration.json**. Deze naam is hoofdlettergevoelig en moet in kleine letters.

    ![Bloginstellingen opslag](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Selecteer de waarden, zoals in de volgende schermafdruk op de volgende pagina en klik vervolgens op **OK** om de wizard te voltooien.

    ![Selectie van JSON voor 'zelfs serialisatie format' en UTF8 voor 'Encoding'](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Alle ingangen zijn nu gedefinieerd.

![Screenshot van de drie ingangen gedefinieerd](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Uitvoer definiëren

1. Klik op het tabblad **uitvoer** en klik vervolgens op **Een uitvoer toevoegen**.

    ![De optie 'Uitvoer toevoegen' en het tabblad uitvoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Klik op **SQL-Database**.

3. Selecteer de naam van de server die is gebruikt in de sectie "Verbinding maken met Database van Visual Studio" van het artikel. De naam van de database is **TollDataDB**.

4. **Tolladmin** invoeren in het veld **gebruikersnaam** **123toll!** in het veld **wachtwoord** en **TollDataRefJoin** in **het tabelveld** .

    ![SQL-Database-instellingen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure Stream analytics query

Het tabblad **QUERY** bevat een SQL-query waarmee de binnenkomende gegevens worden omgezet.

![Een query toegevoegd aan het tabblad Query](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

In deze zelfstudie wordt geprobeerd te beantwoorden, diverse zakelijke vragen met betrekking tot de tolbruggen gegevens en constructies Stream Analytics query's die kunnen worden gebruikt in Azure Stream Analytics een relevante antwoord te geven.

Voordat u uw eerste taak van de Stream Analytics, verkennen we enkele scenario's en de querysyntaxis van de.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Inleiding tot de querytaal Azure Stream Analytics
-----------------------------------------------------

Stel, u moet het aantal voertuigen dat een gratis stand invoeren. Dit is een continue stroom van gebeurtenissen, hebt u voor het definiëren van een "periode." Laten we de vraag "hoeveel voertuigen invoeren voor een gratis kraam elke drie minuten?" te wijzigen. Dit is gewoonlijk het aantal gewor genoemd.

Bekijk de Azure Stream Analytics query deze vraag beantwoordt:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Zoals u ziet, Azure Stream Analytics maakt gebruik van een querytaal zoals SQL en een paar uitbreidingen om op te geven van tijd verband houdende aspecten van de query wordt toegevoegd.

Informatie over [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructies worden gebruikt in de query van MSDN voor meer informatie.

## <a name="testing-azure-stream-analytics-queries"></a>Azure Stream Analytics query's testen

Nu u uw eerste Azure Stream Analytics query hebt geschreven, is het tijd om te testen met voorbeeldbestanden van gegevens die zich in de map TollApp in het volgende pad:

**.. \\TollApp\\TollApp\\gegevens**

Deze map bevat de volgende bestanden:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Vraag 1: Aantal voertuigen een gratis stand

1. Open de Azure portal en Ga naar uw gemaakte Azure Stream Analytics baan. Klik op het tabblad **QUERY** en plakt u de query uit het vorige gedeelte.

    ![Query plakken in het tabblad Query](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. Klik op de knop **Test** voor het valideren van deze query op voorbeeldgegevens. In het dialoogvenster dat wordt geopend, gaat u naar Entry.json, een bestand met voorbeeldgegevens uit de stroom van de gebeurtenis **EntryTime** .

    ![Screenshot van het bestand Entry.json](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Valideren dat de uitvoer van de query is zoals verwacht:

    ![Resultaten van de test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Vraag 2: Totale tijd voor elke auto passeren de stand gratis rapport

De gemiddelde tijd die nodig is voor een auto door de gratis helpt voor de beoordeling van de efficiëntie van het proces en de ervaring van de klant.

Om de totale tijd, moet u lid worden van de stream ExitTime EntryTime stream. U zal deelnemen aan de streams in de kolommen TollId en LicencePlate. De **JOIN** -operator moet u tijdelijke speelruimte die het acceptabele tijdsverschil tussen de gekoppelde gebeurtenissen beschrijft opgeven. U kunt de functie **DATEDIFF** opgeven dat gebeurtenissen niet meer dan 15 minuten van elkaar moeten worden. U wordt ook de functie **DATEDIFF** om af te sluiten en de tijden van de post voor het berekenen van de werkelijke tijd die een auto in het station gratis doorbrengt toepassen. Opmerking het verschil tussen het gebruik van **DATEDIFF** wanneer het wordt gebruikt in een **SELECT** -instructie in plaats van een **JOIN** -voorwaarde.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Deze query testen, update voor de query op het tabblad **QUERY** van het project:

    ![Bijgewerkte query in het tabblad Query](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Klik op **testen** en monster invoerbestanden opgeven voor EntryTime en ExitTime.

    ![Screenshot van geselecteerde invoerbestanden](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Schakel het selectievakje in als u de query testen en weergeven van de uitvoer:

    ![De uitvoer van de test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Vraag 3: Alle bedrijfsvoertuigen met verlopen registratie rapporteren

Azure Stream Analytics met statische momentopnamen van gegevens kunt lid worden van tijdelijke gegevensstromen. Voorbeelden van deze mogelijkheid gebruik het volgende Voorbeeldvraag.

Als het bedrijf gratis een bedrijfsvoertuig is geregistreerd, wordt doorgegeven via de gratis stand zonder inspectie worden gestopt. U kunt registratie bedrijfsvoertuig opzoektabel identificeren alle bedrijfsvoertuigen die registraties zijn verlopen.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Als u wilt testen een query met referentiegegevens, moet u definiëren de invoerbron voor de verwijzingsgegevens, die u al hebt gedaan.

1. Met deze query testen, plakt u de query in het tabblad **QUERY** , klik op **testen**en geven de twee bronnen:

    ![Screenshot van geselecteerde invoerbestanden](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. Weergave van de uitvoer van de query:

    ![Screenshot van de query-uitvoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>De taak van de Stream Analytics start


Nu is het tijd om de configuratie te voltooien en start de taak. Sla de query van vraag 3, zal die uitvoer die overeenkomt met het schema van de uitvoertabel **TollDataRefJoin** produceren.

Ga naar het project **DASHBOARD**en klik op **START**.

![Screenshot van de Start-knop in het project-dashboard](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

In het dialoogvenster omzetten in de **Uitvoer START** tijd **tijd aangepast**. Wijzig het uur één uur vóór de huidige tijd. Deze wijziging zorgt ervoor dat alle gebeurtenissen van de hub gebeurtenis sinds het begin van het genereren van de gebeurtenissen aan het begin van de zelfstudie worden verwerkt. Klik op het selectievakje start de taak.

![Selectie van aangepaste tijd](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

De taak wordt gestart, kan een paar minuten duren. U kunt de status op de pagina op het hoogste niveau voor Analytics Stream bekijken.

![Schermafbeelding van de status van de taak](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>De resultaten van de in Visual Studio

1. Visual Studio Server Explorer geopend en met de rechtermuisknop op de tabel **TollDataRefJoin** .
2. Klik op **Gegevens in een tabel weergeven** als u wilt zien van de uitvoer van de taak.

    ![Selectie van 'Tabelgegevens weergeven' in Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Schaalbare Azure Stream Analytics taken

Azure Stream Analytics is ontworpen om elastically schaal zodanig dat deze kan omgaan met grote hoeveelheden gegevens. De query Azure Stream Analytics kunt een **Partitie BY** -component het systeem dat deze stap kan worden uitgebreid om te zien. **PartitionId** is een speciale kolom die in het systeem wordt toegevoegd aan de partitie-ID van de invoer (hub gebeurtenis).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. De huidige taak stoppen, de query bijwerken in het tabblad **QUERY** en open het tabblad **schaal** .

    **STREAMING eenheden** bepalen het bedrag van de rekenkracht die de taak kan ontvangen.

2. Verplaats de schuifregelaar naar 6.

    ![Screenshot van 6 streaming eenheden selecteren](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Ga naar het tabblad **UITGANGEN** en wijzig de naam van de SQL-tabel in **TollDataTumblingCountPartitioned**.

Als u de taak start nu kunt Azure Stream Analytics verdelen van werk over meer compute-bronnen en hogere doorvoersnelheid te behalen. Houd er rekening mee dat de toepassing van de TollApp ook gebeurtenissen die zijn gepartitioneerd met TollId verzendt.

## <a name="monitor"></a>Monitor

Het tabblad **MONITOR** bevat statistieken over de actieve taak.

![Screenshot van statistieken over het uitvoeren van taken](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

U kunt **Logboeken** openen via het tabblad **DASHBOARD** .

![De optie 'Logboeken'](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Screenshot van logboeken waar u de status van taken ziet](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Ga voor meer informatie over een bepaalde gebeurtenis op de gebeurtenis en klikt u vervolgens op de knop **DETAILS** .

![Screenshot van details over de geselecteerde gebeurtenis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Conclusie

Deze zelfstudie werd geïntroduceerd op de Azure Stream Analytics-service. Het configureren van ingangen en uitgangen voor het project Stream Analytics aangetoond. Met behulp van de gegevens niet gratis scenario, beschreven de zelfstudie voorkomende problemen die zich voordoen in de ruimte van de gegevens in beweging en hoe ze kunnen worden opgelost met een eenvoudige SQL-achtige query's in Azure Stream Analytics. De zelfstudie beschreven SQL extensie constructies voor het werken met tijdelijke gegevens. Het bleek lid worden van gegevensstromen, hoe de gegevensstroom met statische referentiegegevens verrijken en schaalaanpassing van een query voor een snellere doorvoer.

Maar deze handleiding een goede inleiding biedt, is het niet op enigerlei wijze voltooid. U kunt meer query patronen met behulp van de SAQL taal [Query](stream-analytics-stream-analytics-query-patterns.md)voorbeelden voor gemeenschappelijke Stream Analytics gebruikspatronen vinden.
Raadpleeg de [on line documentatie](https://azure.microsoft.com/documentation/services/stream-analytics/) voor meer informatie over Azure Stream Analytics.

## <a name="clean-up-your-azure-account"></a>Uw account Azure opschonen

1. Stop de taak Analytics Stream in Azure portal.

    Het script Setup.ps1 wordt gemaakt van twee gebeurtenis-hubs en een SQL-database. De volgende instructies kunnen u aan het einde van de zelfstudie resources opruimen.

2. Typ in een venster PowerShell **.\\ CleanUp.ps1** start het script waarmee bronnen die worden gebruikt in de zelfstudie worden verwijderd.

    > [AZURE.NOTE] Resources worden aangeduid met de naam. Zorg ervoor dat u elk item zorgvuldig controleren voordat de verwijdering te bevestigen.

    ![Screenshot van het opruimproces](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
