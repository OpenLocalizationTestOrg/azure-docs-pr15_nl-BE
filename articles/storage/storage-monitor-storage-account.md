<properties
    pageTitle="Het controleren van een rekening voor opslag | Microsoft Azure"
    description="Informatie over het controleren van een rekening voor opslag in Azure via de Portal Azure."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Een account voor opslag in de Portal Azure controleren

## <a name="overview"></a>Overzicht

U kunt uw account opslag vanaf de [Portal Azure](https://portal.azure.com)controleren. Bij het configureren van uw account opslag voor de controle via de portal gebruikt Azure opslag [Opslag Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) te volgen voor uw account en gegevens te melden.

> [AZURE.NOTE]Extra kosten zijn verbonden met de meetgegevens in de [Portal Azure](https://portal.azure.com)onderzoeken. Zie <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">opslag Analytics en facturering</a>voor meer informatie. <br />

> Azure bestandsopslag op dit moment ondersteunt opslag Analytics metrics, maar ondersteunt nog geen logboekregistratie. U kunt de metrics voor Azure bestandsopslag via de [Portal Azure](https://portal.azure.com)inschakelen.

> Opslag-rekeningen van het replicatietype van Zone redundante opslag (ZRS) beschikt niet over de metrics of de mogelijkheid tot het vastleggen op dit moment is ingeschakeld. 

> Zie voor een uitgebreide handleiding over het gebruik van Analytics voor opslag en andere hulpprogramma's te identificeren, opsporen en oplossen van problemen met Azure opslag met [Monitor, vaststellen, en Microsoft Azure opslag oplossen](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Procedure: controleren voor een account opslag configureren

1. In de [Portal Azure](https://portal.azure.com)op **opslag**en klik vervolgens op de naam van de opslag voor het openen van het dashboard.

2. Klik op **configureren**en blader naar de **controle** -instellingen voor de blob-, tabel- en wachtrijservices.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. Stel in **controle**, het toezicht en het bewaarbeleid gegevens voor elke service:

    -  Als u het niveau van de controle, selecteert u een van de volgende:

      **Minimale** - statistieken verzameld zoals ingress/egress, beschikbaarheid, vertraging en succes percentages die worden samengevoegd voor de blob-, tabel- en wachtrijservices.

      **Uitgebreid** - verzamelt naast de minimale maatstaven dezelfde set parameters voor elke opslagbewerking in de Azure Storage Service-API. Uitgebreide statistieken kunnen dichter bij analyse van de problemen die zich tijdens de toepassing voordoen.

      **Uit** - schakelt u controle. Bestaande gegevens controleren wordt tot aan het einde van de bewaarperiode vastgelegd.

- Als het beleid voor het bewaren van gegevens in de **handhaving (in dagen)**, typt u het aantal dagen van de gegevens te bewaren van 1 tot 365 dagen. Als u niet een bewaarbeleid instellen wilt, nul invoeren. Als er geen bewaarbeleid, dient u de controlegegevens te verwijderen. Wij raden u aan een op basis van hoe lang u bewaren van opslag analytics-gegevens voor uw account wilt, zodat oude en ongebruikte analytics-gegevens kunnen worden verwijderd door het systeem zonder kosten bewaarbeleid instellen.

4. Klik op **Opslaan**wanneer u klaar bent met de configuratie controleren.

U moet beginnen met het zien van de controle van de gegevens op het dashboard en de **Monitor** na ongeveer een uur.

Totdat u controle voor een opslag configureren, geen controlegegevens worden verzameld en de metrics grafieken op het dashboard en de **Monitor** pagina leeg zijn.

Nadat u de niveaus van controle en bewaarbeleid instellen, kunt u kiezen welke van de beschikbare Maatstelsel van controleren in de [Portal Azure](https://portal.azure.com)en welke parameters bij metrics grafieken uitzetten. Een reeks cijfers wordt weergegeven op elk niveau van de controle. Toevoegen of verwijderen van parameters uit de lijst met parameters kunt u **Parameters toevoegen** .

Parameters worden opgeslagen in de account van de opslag in de vier tabellen met de naam $MetricsTransactionsBlob, $MetricsTransactionsTable, $MetricsTransactionsQueue en $MetricsCapacityBlob. Zie voor meer informatie [Over opslag Analytics Metrics](http://msdn.microsoft.com/library/azure/hh343258.aspx).


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Procedure: het dashboard voor de controle aanpassen

Op het dashboard, kunt u maximaal zes cijfers worden uitgezet in de grafiek maatstaven van de negen beschikbare statistieken. Voor elke service (blob-, tabel- en wachtrij) zijn de beschikbaarheid, succespercentage en totaal aantal aanvragen beschikbaar. De cijfers die beschikbaar is op het dashboard zijn hetzelfde voor de minimale of uitgebreide controle.

1. In de [Portal Azure](https://portal.azure.com)op **opslag**en klik vervolgens op de naam van de account van de opslag voor het openen van het dashboard.

2. Om de criteria die in de grafiek worden uitgezet wijzigen, neemt u een van de volgende acties:

    - Om een nieuwe waarde toevoegen aan de grafiek, klikt u op het gekleurde selectievakje in naast de koptekst van de metrische gegevens in de tabel onder de grafiek.

    - Als u wilt een metric die is uitgezet in de grafiek verbergen, het selectievakje gekleurde naast de koptekst van de metrische.

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. Standaard wordt de grafiek trends, alleen de huidige waarde van elke maateenheid (de **relatieve** optie boven aan de grafiek) weer te geven. Y-as weergegeven zodat u de absolute waarden kunt zien, selecteert u **Absolute**.

4. Als selecteert u het tijdsbereik de metrics grafiek wordt weergegeven, 6 uur, 24 uur of 7 dagen aan het begin van de grafiek.


## <a name="how-to-customize-the-monitor-page"></a>Procedure: de pagina van het beeldscherm aanpassen

Op de pagina **Monitor** ziet u de volledige set parameters voor uw account opslag.

- Als uw account opslag Minimale controle is geconfigureerd heeft, worden uit de blob, een tabel en een wachtrijservices Maatstelsel van de ingress/egress, beschikbaarheid, vertraging en succes percentages samengevoegd.

- Als uw account opslag uitgebreide controle geconfigureerd heeft, zijn de cijfers beschikbaar op een betere resolutie van de individuele opslag naast de aggregaten serviceniveau.

Gebruik de volgende procedures om te kiezen welke opslag statistieken weer te geven in de statistieken, grafieken en tabellen die worden weergegeven op **de pagina** . Deze instellingen hebben geen invloed op de verzameling, aggregatie en opslag van gegevens in de opslag-account controleren.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Procedure: parameters toevoegen aan de tabel parameters


1. In de [Portal Azure](https://portal.azure.com)op **opslag**en klik vervolgens op de naam van de account van de opslag voor het openen van het dashboard.

2. Klik op de **Monitor**.

    **De pagina** wordt geopend. Standaard worden weergegeven in de tabel parameters een subset van de parameters die beschikbaar voor controle zijn. In de afbeelding ziet de standaardweergave van de Monitor voor de account van een opslag met uitgebreide monitoring voor alle drie services geconfigureerd. Gebruik **Parameters toevoegen** om de cijfers die u wilt controleren van alle beschikbare parameters.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Wanneer u het maatstelsel selecteert, moet u rekening houden met kosten. Er zijn transactie- en egress kosten in verband met het vernieuwen van de controle wordt weergegeven. Zie [opslag Analytics en facturering](http://msdn.microsoft.com/library/azure/hh360997.aspx)voor meer informatie.

3. Klik op **parameters toevoegen**.

    De statistische parameters die beschikbaar in een minimale controle zijn worden boven aan de lijst. Als het selectievakje is ingeschakeld, wordt de metric weergegeven in de lijst met parameters.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. De muisaanwijzer op de rechterkant van het dialoogvenster weer te geven van een schuifbalk die u slepen kunt als u wilt extra statistieken in beeld schuiven.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Klik op de pijl-omlaag door het uitbreiden van een lijst met bewerkingen die als bereik om op te nemen de metric de metric-waarde. Selecteer elke bewerking die u wilt weergeven in de tabel parameters in de [Portal Azure](https://portal.azure.com).

    In de volgende afbeelding wordt is de metric FOUTPERCENTAGE vergunning uitgevouwen.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Nadat u de parameters voor alle services, klikt u op OK (vinkje) om te werken de controleconfiguratie. De geselecteerde parameters worden toegevoegd aan de tabel parameters.

7. Om een waarde uit de tabel wilt verwijderen, klikt u op de waarde om deze te selecteren en klik vervolgens op **Verwijderen Metric**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Procedure: de metrics grafiek op de pagina van het beeldscherm aanpassen

1. Selecteer op de pagina **Monitor** voor de account opslag in de tabel parameters maximaal 6 cijfers worden uitgezet in de grafiek maatstaven. Selecteer een metric, klikt u op het selectievakje aan de linkerkant. Om een waarde uit de grafiek verwijderen, schakel het selectievakje.

2. Als u de grafiek tussen relatieve waarden (laatste waarde wordt alleen weergegeven) en de absolute waarden (Y-as weergegeven), selecteer **relatief** of **absoluut** boven aan de grafiek.

3.  Als u wilt wijzigen de tijd variëren de metrics grafiek bevat, selecteren, **6 uur**, **24 uur**of **7 dagen** aan het begin van de grafiek.



## <a name="how-to-configure-logging"></a>Procedure: logboekregistratie configureren

Diagnostische logboeken kunt opslaan voor aanvragen voor lezen, schrijven aanvragen en/of Delete-aanvragen voor elk van de storage-services beschikbaar zijn met uw account opslag (blob-, tabel- en wachtrij) en het bewaarbeleid gegevens kunt instellen voor elk van de services.

1. In de [Portal Azure](https://portal.azure.com)op **opslag**en klik vervolgens op de naam van de account van de opslag voor het openen van het dashboard.

2. Klik op **configureren**en de pijl-omlaag op het toetsenbord gebruiken om te bladeren naar de **registratie**.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Voor elke service (blob-, tabel- en wachtrij), configureert u het volgende:

    - De typen van de aanvraag aan te melden: aanvragen voor lezen, schrijven verzoeken en Delete-aanvragen.

    - Het aantal dagen te bewaren van gegevens in het logboek. Nul invoeren als u niet wilt een bewaarbeleid instellen. Als u een bewaarbeleid niet instelt, is het u de logboeken te verwijderen.

4. Klik op **Opslaan**.

De diagnostische logboeken worden opgeslagen in een blob-container met de naam $logs in uw account voor opslag. Zie [Opslag Analytics Logging](http://msdn.microsoft.com/library/azure/hh343262.aspx)voor informatie over de toegang tot de container $logs.
