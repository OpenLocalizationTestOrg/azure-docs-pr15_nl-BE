<properties 
    pageTitle="Het controleren van een cloud service | Microsoft Azure" 
    description="Informatie over het cloud-services controleren met behulp van de klassieke Azure portal." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Cloud-Services controleren

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

U kunt controleren `key` prestatiegegevens voor uw cloud-services in de klassieke Azure portal. U kunt het instellen van minimale en uitgebreide voor elke rol service monitoring en de controle wordt weergegeven kunt aanpassen. Uitgebreide controlegegevens worden opgeslagen in een opslagruimte-account toegang buiten de portal tot. 

Controle wordt in de klassieke Azure portal zijn uiterst configureerbaar. U kunt de maateenheden die u wilt controleren in de lijst parameters op de pagina van de **Monitor** en kunt u kiezen welke maatstaven worden uitgezet in metrische grafieken op de pagina van de **Monitor** en het dashboard. 

## <a name="concepts"></a>Concepten

Standaard is minimale controle beschikbaar voor een nieuwe wolk service met behulp van prestatiemeteritems die worden verzameld uit de host-besturingssysteem voor de rollen exemplaren (virtuele machines). De minimale criteria zijn beperkt tot het CPU Percentage, Data gegevens uit, doorvoer voor schijf lezen en schrijven schijf, doorvoersnelheid. Als u uitgebreide controle configureert, kunt u extra statistieken op basis van prestatiegegevens in de virtuele machines (rol exemplaren) ontvangen. De uitgebreide statistieken kunnen dichter bij analyse van de problemen die zich tijdens de toepassing voordoen.

Standaard prestatiemeteritemgegevens van rol exemplaren bemonsterd en overgebracht van de rol-instantie om de 3 minuten. Wanneer u uitgebreide controle inschakelt, wordt de onbewerkte gegevens van prestatiemeteritems voor elk exemplaar van de rol en over de rol van instanties voor elke rol samengevoegd met intervallen van 5 minuten, 1 uur en 12 uur. De samengevoegde gegevens wordt na 10 dagen verwijderd.

Nadat u uitgebreide controle inschakelt, wordt het geaggregeerde controlegegevens opgeslagen in tabellen in uw account voor opslag. Als u uitgebreide controle voor een rol, moet u een verbindingsreeks diagnostische gegevens die is gekoppeld aan de account van de opslag. U kunt accounts verschillende opslag voor verschillende rollen.

Houd er rekening mee dat uw opslagkosten in verband met de opslag van gegevens, overdracht van gegevens en transacties van de opslag als u een uitgebreide controle inschakelen verhogen zal. Minimale controle hoeft niet een account voor opslag. De gegevens voor de parameters die beschikbaar zijn op het niveau van minimale controle worden niet opgeslagen in uw account voor opslag, zelfs als u het niveau van toezicht ingesteld op uitgebreid.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Procedure: bewaking van cloud-services configureren

Gebruik de volgende procedures voor het configureren van uitgebreide of Minimale controle in de klassieke Azure portal. 

### <a name="before-you-begin"></a>Voordat u begint

- Maak een account opslagruimte voor het opslaan van de meetgegevens. U kunt accounts verschillende opslag voor verschillende rollen. Zie voor meer informatie help voor **Opslag rekeningen**of zien [hoe u een Account opslag maken](/manage/services/storage/how-to-create-a-storage-account/).

- Azure Diagnostics inschakelen voor uw service cloud rollen. Zie [Diagnostische gegevens voor Cloud Services configureren](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore).

Zorg ervoor dat de verbindingsreeks diagnostics aanwezig in de configuratie van de functie is. U inschakelen uitgebreide controle tot Azure diagnostiek en er een verbindingsreeks diagnostische gegevens in de configuratie van de functie opnemen niet.   

> [AZURE.NOTE] Projecten gericht op Azure SDK 2.5 bevat automatisch geen verbindingsreeks van de diagnostische gegevens in de projectsjabloon. Voor deze projecten moet u de verbindingstekenreeks diagnostische gegevens handmatig toevoegen aan de configuratie van de functie.

**Verbindingsreeks diagnostische gegevens handmatig toevoegen aan de configuratie van de functie**

1. De Cloud Service-project in Visual Studio openen
2. Dubbelklik vervolgens op de **rol** van de ontwerper van de functie openen en selecteer het tabblad **Instellingen**
3. Zoek een instelling met de naam **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Als deze instelling niet aanwezig is klikt u op de knop **Instelling toevoegen** aan toe te voegen aan de configuratie en het type voor de nieuwe instelling wijzigt in **ConnectionString**
5. Stel de waarde voor de verbindingsreeks de door te klikken op de knop **...** . Hiermee opent u een dialoogvenster waarin u een opslag te selecteren.

    ![Instellingen voor Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Het niveau van de controle voor uitgebreide of minimale wijzigen

1. Open de pagina **configureren** voor de implementatie van de service cloud in [Azure klassieke portal](https://manage.windowsazure.com/).

2. **Niveau**, klikt u op **uitgebreid** of **Minimal**. 

3. Klik op **Opslaan**.

Nadat u uitgebreide controle inschakelt, kunt u de meetgegevens in de klassieke Azure portal zien binnen het uur moet beginnen.

De onbewerkte gegevens van prestatiemeteritems en geaggregeerde controlegegevens worden opgeslagen in de rekening van de opslag in tabellen gekwalificeerd door de installatie-ID voor de rollen. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Procedure: cloud service metrics alerts ontvangen

U kunt waarschuwingen op basis van uw cloud-service controleren metrics ontvangen. Klik op de pagina **Management Services** van de klassieke Azure portal kunt u een regel voor een waarschuwing wanneer de metric die u kiest een waarde die u opgeeft wordt bereikt. U kunt ook e-mailbericht verzonden wanneer u de waarschuwing hebt. Zie voor meer informatie [hoe: meldingen ontvangen en waarschuwingsregels beheren in Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Procedure: parameters toevoegen aan de tabel parameters

1. Open **de pagina voor de service cloud** in [Azure klassieke portal](http://manage.windowsazure.com/).

    Standaard worden weergegeven in de tabel parameters een subset van de beschikbare statistieken. De afbeelding ziet u de standaard uitgebreide statistieken voor een cloud-service beperkt tot het prestatiemeteritem megabytes (MB) geheugen met gegevens die zijn verzameld op het niveau van de rol is. Gebruik **Parameters toevoegen** om extra statistische en rol op gebruikersniveau statistieken om te controleren in de klassieke Azure portal.

    ![Uitgebreide weergave](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Parameters toevoegen aan de tabel Parameters:

    1. Klik op **Parameters toevoegen** als u wilt openen, **Kies Metrics**, hieronder weergegeven.

        De eerste beschikbare metric is uitgevouwen zodat de opties die beschikbaar zijn. Voor elke waarde wordt de optie geaggregeerde gegevens van monitoring voor alle rollen. Bovendien kunt u afzonderlijke functies om gegevens weer te geven.

        ![Metrische gegevens toevoegen](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Selecteer statistieken weer te geven

        - Klik op de pijl-omlaag door de metric om de controle-opties weer te geven.
        - Schakel het selectievakje in voor elke controle optie die u wilt weergeven.

        U kunt maximaal 50 cijfers in de tabel parameters weergeven.

        > [AZURE.TIP] In het uitgebreide controle, bevatten de lijst maatstelsel tientallen metrics. Een schuifbalk wordt weergegeven, de muisaanwijzer op de rechterkant van het dialoogvenster. Als u wilt filteren in de lijst, klikt u op het zoekpictogram en voer tekst in het zoekvak, zoals hieronder wordt weergegeven.
    
        ![Metrics zoekopdracht toevoegen](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Nadat u geselecteerd, metrics hebt, klikt u op OK (vinkje).

    De geselecteerde parameters worden toegevoegd aan de tabel parameters zoals hieronder wordt weergegeven.

    ![monitor statistieken](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Een metric verwijderen uit de tabel parameters, klikt u op de waarde om deze te selecteren en klik vervolgens op **Verwijderen Metric**. (Alleen ziet u **Metric verwijderen** wanneer u een waarde hebt geselecteerd.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Aangepaste metrische gegevens toevoegen aan de tabel met statistieken

**Uitgebreid** niveau controle bevat een lijst met standaard-parameters die u kunt controleren op de portal. Daarnaast kunt u een aangepaste metrische of prestatiemeteritems zijn gedefinieerd door de toepassing via de portal bijhouden.

De volgende stappen wordt ervan uitgegaan dat u **uitgebreid** niveau controleren hebt ingeschakeld en configureren van uw toepassing voor het verzamelen en overbrengen van aangepaste prestatiemeters. 

De aangepaste items weergegeven in de portal moet u de configuratie in af-besturingselement-container bijwerken:
 
1. Open de blob af in het hoofdprogramma besturingselementen in uw account diagnostische gegevens opslag. U kunt Visual Studio of andere Opslagverkenner om dit te doen.

    ![Visual Studio Server Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Navigeer in de blob-pad de configuratie voor uw exemplaar van de functie zoeken met het patroon **Rolnaam-DeploymentId/RoleInstance** . 

    ![Visual Studio Opslagverkenner](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Het configuratiebestand voor uw exemplaar van de rol van downloaden en bijwerken om aangepaste prestatiemeteritems bevatten. Voeg de volgende onder **PerformanceCounters\Subscriptions** knooppunt bijvoorbeeld om te controleren op het *station C* *schijf geschreven Bytes per seconde*

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. De wijzigingen opslaan en laden van het configuratiebestand terug naar dezelfde locatie als het bestaande bestand in de blob wordt overschreven.
5. Schakelen naar de uitgebreide modus in de Azure klassieke portal configuratie. Als u reeds in de uitgebreide modus moet u minimaal en voor uitgebreide in-of uitschakelen.
6. De aangepaste Prestatiemeter wordt nu beschikbaar zijn in het dialoogvenster **Parameters toevoegen** . 

## <a name="how-to-customize-the-metrics-chart"></a>Procedure: het maatstelsel van grafiek aanpassen

1. Selecteer maximaal 6 cijfers worden uitgezet in de grafiek maatstaven in de tabel parameters. Selecteer een metric, klikt u op het selectievakje aan de linkerkant. Als een metric uit de grafiek metrics verwijderen, schakel het vakje uit in de tabel parameters.

    Metrics dat u selecteert in de tabel parameters, worden de cijfers toegevoegd aan het maatstelsel. Op een klein scherm bevat een vervolgkeuzelijst **meer n** metrische headers die niet de weergave past.

 
2. Als u wilt schakelen tussen de weergave van de relatieve (definitieve waarde voor elke metrisch) en absolute waarden (Y-as weergegeven), selecteert u relatieve of Absolute boven aan de grafiek.

    ![Relatieve of Absolute](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Als selecteert u het tijdsbereik de metrics grafiek wordt weergegeven, 1 uur, 24 uur of 7 dagen aan het begin van de grafiek.

    ![Monitor weergave periode](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    De methode voor het uitzetten van metrics verschilt in het dashboard metrics-diagram. Een reeks cijfers beschikbaar en parameters worden toegevoegd of verwijderd door de koptekst van de metrische.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Voor het aanpassen van de grafiek statistieken op het dashboard

1. Open het dashboard voor de cloud-service.

2. Toevoegen of gegevens uit de grafiek verwijderen:

    - Als u wilt uitzetten in een nieuwe waarde, het selectievakje voor de metric in de aangegeven grafiek. Klik op de pijl-omlaag door ** *n*??metrics** metric die kan niet worden weergegeven in het gebied voor koptekst grafiek uitzetten op een klein scherm.

    - Een metric die is uitgezet in de grafiek verwijderen, schakel het selectievakje op de koptekst.

3. Schakelen tussen **relatieve** en **Absolute** geeft.

4. Kies 1 uur, 24 uur of 7 dagen van de gegevens weer te geven.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Procedure: toegang tot uitgebreide controle van gegevens buiten de klassieke Azure portal

Uitgebreide controlegegevens worden opgeslagen in tabellen in de opslag-accounts die u voor elke rol. Voor elke installatie van de service cloud worden zes tabellen gemaakt voor de rol. Twee tabellen worden gemaakt voor elke (5 minuten, 1 uur en 12 uur). Een van deze tabellen worden opgeslagen aggregaties rol niveau; de andere tabel bevat aggregaties voor exemplaren van de rol. 

De namen van de tabellen hebben de volgende notatie:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

waar:

- *deploymentID* is de GUID die is toegewezen aan de implementatie van de service cloud

- *aggregation_interval* = 5 M 1 H en 12 H

- rol op gebruikersniveau aggregaties = R

- aggregaties voor rol exemplaren RI =

De volgende tabellen slaan bijvoorbeeld uitgebreide bewaking verkregen gegevens worden samengevoegd met intervallen van 1 uur:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
