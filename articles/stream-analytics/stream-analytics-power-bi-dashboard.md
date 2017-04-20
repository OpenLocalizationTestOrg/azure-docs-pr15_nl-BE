<properties
    pageTitle="Power BI-dashboard op stroom Analytics | Microsoft Azure"
    description="Real-time streaming Power BI dashboard business intelligence verzamelen en analyseren van grote hoeveelheden gegevens uit een stroom Analytics taak gebruiken."
    keywords="Analytics dashboard, realtime dashboard"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream-Analytics & Power BI: een real-time analytics dashboard voor gegevensstromen

Azure Stream Analytics kunt u profiteren van een van de toonaangevende business intelligence-hulpprogramma's, Microsoft Power BI. Informatie over het gebruik van Azure Stream Analytics voor het analyseren van grote hoeveelheden, streaming gegevens en het inzicht in een realtime dashboard voor Power BI analytics get.

Gebruik [Microsoft Power BI](https://powerbi.com/) voor het snel bouwen van een live dashboard. [Bekijk een video ter illustratie van het scenario](https://www.youtube.com/watch?v=SGUpT-a99MA).

In dit artikel wordt beschreven hoe uw eigen aangepaste hulpprogramma's voor bedrijfsinformatie maken met behulp van Power BI als een uitvoer voor uw taken Azure Stream Analytics en maken gebruik van een realtime dashboard.

## <a name="prerequisites"></a>Vereisten

* Microsoft Azure Account
* Een invoer voor de taak Analytics Stream streaming gegevens opnemen uit. Stream Analytics accepteert invoer van Azure gebeurtenis Hubs of Azure Blob-opslag.  
* Werk of school account voor Power BI

## <a name="create-azure-stream-analytics-job"></a>Azure Stream Analytics taak maken

Klik op **Nieuw, Data Services, Stream Analytics, snelle invoer**vanuit [Azure klassieke Portal](https://manage.windowsazure.com).

Geef de volgende waarden, klikt u vervolgens op **taak maken stroom Analytics**:

* **Taaknaam** - Voer een taaknaam. Bijvoorbeeld **DeviceTemperatures**.
* **Regio** : Selecteer de regio waar u het project zich bevindt. Houd rekening met het plaatsen van de taak en de gebeurtenis hub in hetzelfde gebied voor optimale prestaties en geen kosten voor data-overdracht tussen regio's aangaan.
* **Opslag-Account** - de opslag-Account die u gebruiken wilt voor het opslaan van de meetgegevens voor alle stroom Analytics taken die worden uitgevoerd binnen deze regio kiezen. U hebt de optie voor het kiezen van een bestaande Account voor opslag of maak een nieuwe.

Klik op **Stream Analytics** in het linkerdeelvenster om de Stream Analytics taken.

![graphic1][graphic1]

> [AZURE.TIP] Het nieuwe project wordt weergegeven met de status **Niet gestart**. U ziet dat de knop **Start** op de onderkant van de pagina is uitgeschakeld. Dit is normaal gedrag moet u de functie input, output, query, enzovoort voordat de taak start.

## <a name="specify-job-input"></a>Invoer van de taak opgeven

Voor deze zelfstudie wordt uitgegaan van een dat gebeurtenis Hub door u als invoer met JSON serialisatie en UTF-8-codering wordt gebruikt.

* Klik op de taaknaam van de.
* Klik op **"inputs"** vanaf de bovenkant van de pagina en klik op **Invoer toevoegen**. Het dialoogvenster dat wordt geopend kunt u via een aantal stappen voor het instellen van uw invoer.
*   **Gegevensstroom**selecteren en klik vervolgens op de knop rechts.
*   **Hub gebeurtenis**selecteren en klik vervolgens op de knop rechts.
*   Typ of Selecteer de volgende waarden op de derde pagina:
  * **Alias invoer** - Voer een beschrijvende naam voor deze taak invoeren. Houd er rekening mee dat u gebruikt deze naam in de query op.
  * **Gebeurtenis Hub** - als de gebeurtenis Hub die u hebt gemaakt in het abonnement hetzelfde als de taak Stream Analytics, selecteer de naamruimte waarin de gebeurtenis hub is.
*   Als uw hub-gebeurtenis zich in een ander abonnement, selecteert u **Gebeurtenis-Hub voor gebruik van een ander abonnement** en informatie voor **Service Bus Namespace**, **Hub gebeurtenisnaam** **Gebeurtenisnaam Hub beleid**, **Sleutel gebeurtenis Hub beleid**en **Aantal Hub partitie**handmatig invoeren.

> [AZURE.NOTE]  In dit voorbeeld wordt het aantal partities, is 16.

* **Gebeurtenisnaam Hub** - Selecteer de naam van de Azure gebeurtenis Hub die u hebt.
* **Gebeurtenis Hub beleidsnaam** - Selecteer de gebeurtenis Hub-beleid voor de gebeurtenis Hub die u gebruikt. Zorg ervoor dat dit beleid de machtigingen beheren heeft.
*   **Gebeurtenis Hub Consumer Group** : kunt u leeg laten of een groep consumenten is er op uw Hub-gebeurtenis. Houd er rekening mee dat elke groep van de consument van een gebeurtenis Hub lezers slechts 5 tegelijk kan hebben. Dus besluit dienovereenkomstig de juiste consument groep voor uw project. Als u het veld leeg laat, wordt de standaardgroep consument gebruikt.

*   Klik op de knop rechts.
*   De volgende waarden opgeven:
  * **Gebeurtenis Serializer indeling** - JSON
  * **Codering** - UTF8
*   Klik op de knop controleren om toe te voegen deze bron en om te controleren dat stroom Analytics verbinding met de Hub van de gebeurtenis maken kunnen.

## <a name="add-power-bi-output"></a>Output Power BI toevoegen

1.  Klik op de **uitvoer** van de bovenkant van de pagina en klik op **Uitvoer toevoegen**. Power BI vermeld als uitvoeroptie wordt weergegeven.

    ![graphic2][graphic2]  

2.  **Power BI** selecteren en klik vervolgens op de knop rechts.
3.  U ziet een scherm als volgt uit:

    ![graphic3][graphic3]  

4.  In deze stap een werk of school account voor de output Stream Analytics taak op te geven. Als u al Power BI-account hebt, selecteer **Nu machtigen**. Als dit niet het geval is, kiest u **Nu aanmelden**. [Hier is een goede blog stap voor stap de details van de Power BI aanmelden](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Vervolgens ziet u een scherm als volgt uit:

    ![graphic4][graphic4]  

Geef waarden zoals hieronder:

* **Uitvoeralias** – kunt u elke uitvoeralias die u gemakkelijk kunt verwijzen naar plaatsen. Deze uitvoeralias is vooral handig als u meerdere uitgangen voor uw project. In dat geval moet u verwijzen naar deze uitvoer in uw query. We gebruiken bijvoorbeeld de waarde van de alias output = "OutPbi".
* **Naam gegevensset** - bieden een dataset-naam die u uw Power BI output wilt te hebben. We gebruiken bijvoorbeeld 'pbidemo'.
*   **Tabelnaam** - bieden een tabelnaam in de dataset van de Power BI-uitvoer. Stel dat noemen we 'pbidemo'. Op dit moment Power BI-uitvoer van Stream Analytics taken alleen mogelijk een tabel in een dataset.
*   **Werkruimte** : Selecteer een werkruimte in uw Power BI huurder waarop de dataset wordt gemaakt.

>   [AZURE.NOTE] U moet niet expliciet maken deze dataset en -tabel in uw account Power BI. Deze wordt automatisch gemaakt wanneer u uw Stream Analytics taak starten en de taak reageert uitvoer in Power BI begint. Als uw query taak geen resultaten oplevert, wordt de gegevensset en de tabel niet gemaakt.

*   Klik op **OK**, **Verbinding testen** en nu u de configuratie is voltooid.

>   [AZURE.WARNING] Bedenk ook dat als Power BI al een dataset en een tabel met dezelfde naam als die u deze taak Stream Analytics verstrekt, de bestaande gegevens worden overschreven.


## <a name="write-query"></a>Query schrijven

Ga naar het tabblad **Query** van het project. De query, die de uitvoer van die u in uw Power BI wilt schrijven. Zo wordt bijvoorbeeld de volgende SQL-query:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Uw taak starten. Controleer of uw hub-gebeurtenis is ontvangen van gebeurtenissen en genereert de verwachte resultaten van de query. Als uw query uitvoer 0 rijen, wordt tabellen en Power BI dataset niet automatisch gemaakt.

## <a name="create-the-dashboard-in-power-bi"></a>Maken van het dashboard in Power BI

Ga naar [Powerbi.com](https://powerbi.com) en meld u aan met uw account voor werk of school. Als de taak Stream Analytics query resultaten oplevert, ziet u dat al uw gegevensset is gemaakt:

![graphic5][graphic5]

Ga naar de optie Dashboards voor het maken van het dashboard en een nieuw Dashboard maken.

![graphic6][graphic6]

In dit voorbeeld dat we label gaat het "Demo Dashboard".

Klik nu op de dataset die is gemaakt door uw taak Analytics Stream (pbidemo in het huidige voorbeeld). U zal worden genomen op een pagina voor het maken van een grafiek op deze dataset. Dit is maar één voorbeeld van de rapporten die u kunt maken:

Selecteer Σ temp en tijd-velden. Zij gaat automatisch naar waarde en as van de grafiek:

![graphic7][graphic7]

Hiermee krijgt automatisch u een grafiek zoals hieronder:

![graphic8][graphic8]

In het gedeelte waarde Klik op de vervolgkeuzelijst omlaag voor tijdelijke en **gemiddelde** voor de temperatuur en in de grafiek, kiest u op de **visualisatie** en kies **lijndiagram**:

![graphic9][graphic9]

Nu krijgt u een lijndiagram van gemiddelde na verloop van tijd.  De PIN-code gebruiken zoals hieronder, kunt u vastmaken dit aan uw dashboard dat u eerder hebt gemaakt:

![graphic10][graphic10]

Nu als u het dashboard met dit vaste rapport bekijkt, ziet u rapport wordt bijgewerkt in real-time. Probeer dat de gegevens in uw gebeurtenissen – temp Prikker of iets wijzigen en u ziet het real-time effect van die weergegeven in het dashboard.

Opmerking deze zelfstudie aangetoond dat zij het maken van maar één soort grafiek voor een dataset. Power BI kunt u andere klant business intelligence-hulpprogramma's voor uw organisatie maken. Bekijk de video [Aan de slag met Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) voor een ander voorbeeld van een Power BI-dashboard.

Raadpleeg de [sectie Power BI](stream-analytics-define-outputs.md#power-bi) [dat wat stroom Analytics uitvoer](stream-analytics-define-outputs.md "levert wat stroom Analytics")voor meer informatie over het configureren van een Power BI-uitvoer en gebruikmaken van de Power BI-groepen. Een andere handige bron voor meer informatie over het maken van Dashboards met Power BI is [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Beperkingen en best practices

Power BI gelijktijdigheid en de doorvoer van beperkingen in dienst zoals hier beschreven: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Power BI-prijzen")

Door die belandt Power BI zelf meest natuurlijke tot gevallen waarin Azure Stream Analytics een vermindering van belangrijke gegevens laden is.
Wij raden u aan met behulp van TumblingWindow of HoppingWindow om ervoor te zorgen dat gegevens push zou hoogstens 1 push/seconde en dat uw query binnen de doorvoer eisen belandt – u kunt de volgende vergelijking berekent u de waarde in seconden om het venster:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Voorbeeld: als er 1000 apparaten gegevens verzendt elke seconde, u bent op de Power BI Pro SKU die 1.000.000 rijen per uur ondersteunt en u gegevens wilt ophalen gemiddelde per apparaat op Power BI kunt u doen hooguit een push elke 4 seconden per apparaat (zoals hieronder):  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Wat betekent dat we zouden de oorspronkelijke query te wijzigen:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>PowerBI vernieuwen

Een veelgestelde vraag is "Waarom niet het dashboard automatisch bijwerken in PowerBI?".

Hiertoe gebruiken in PowerBI Q & A en waarin u wordt gevraagd een vraag zoals "Maximale waarde door temp waar tijdstempel vandaag is" en vastmaken aan het dashboard steen.

### <a name="renew-authorization"></a>Machtiging verlengen

U moet uw account Power BI opnieuw worden geverifieerd als het wachtwoord is gewijzigd nadat de taak is gemaakt of laatst geverifieerd. Als meerledige verificatie (MVR gesloten) is geconfigureerd op uw huurder Azure Active Directory (AAD) moet u ook Power BI vergunning elke 2 weken verlengen. Een symptoom van dit probleem is geen uitvoer van de taak en een 'Authenticate gebruikersfout' in de logboeken:

![graphic12][graphic12]

Als een taak probeert te starten terwijl het token is verlopen, treedt een fout op en het begin van de taak niet worden uitgevoerd. De fout zal er ongeveer uitzien zoals hieronder:

![Fout in PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

Dit probleem oplossen door de actieve taak stoppen en Ga naar de output Power BI.  Klik op de link "Vernieuwen vergunning" en start de taak van de laatste keer gestopt om gegevensverlies te voorkomen.

![Vernieuwing van de validatie van PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Wanneer de vergunning wordt vernieuwd met Power BI ziet u een waarschuwing voor een groen in het gebied van de vergunning:

![Vernieuwing van de validatie van PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
