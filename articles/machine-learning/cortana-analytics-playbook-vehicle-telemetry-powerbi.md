<properties 
    pageTitle="Installatie-instructies voertuig telemetrie analytics oplossing sjabloon PowerBI Dashboard | Microsoft Azure" 
    description="Met de functionaliteit van Cortana Intelligence te krijgen van real-time en voorspellende inzichten voertuig gezondheid en rijden gewoonten." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Voertuig telemetrie analytics oplossing sjabloon Dashboard PowerBI installatie-instructies

Dit **menu** bevat koppelingen naar de hoofdstukken in deze playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


De oplossing van het voertuig telemetrie Analytics toonbeeld geeft van hoe auto dealers, fabrikanten van auto's en verzekerings-ondernemingen kunnen gebruik Cortana Intelligence krijgen real-time en voorspellende inzichten op de gezondheid van het voertuig en aangedreven gewoonten streven naar verbeteringen op het gebied van klant-ervaring, O & O- en marketingcampagnes. Dit document bevat stapsgewijze instructies voor het configureren van de rapporten voor PowerBI en dashboard nadat de oplossing wordt geïmplementeerd in uw abonnement. 


## <a name="prerequisites"></a>Vereisten
1.  Het voertuig telemetrie Analytics oplossing implementeren door naar [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3) te gaan  
2.  [Microsoft Power BI bureaublad installeren](http://www.microsoft.com/download/details.aspx?id=45331)
3.  Een [abonnement op Azure](https://azure.microsoft.com/pricing/free-trial/). Als u geen abonnement op een Azure, aan de slag met Azure gratis abonnement
4.  PowerBI Microsoft-account
    

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence Suite-onderdelen
De Cortana-inlichtingendiensten zijn geïmplementeerd als onderdeel van de sjabloon voertuig telemetrie Analytics oplossing in uw abonnement.

- **Gebeurtenis Hubs** voor miljoenen voertuig telemetrie gebeurtenissen in Azure ingesting.
- **Stream analytisch**s voor real-time inzicht op de gezondheid van het voertuig en die gegevens in de opslag voor langere termijn voor rijkere batch analytics zich blijft voordoen.
- **Machine Learning** voor de detectie van de afwijking in real-time en batch-verwerking voorspellende inzichten te krijgen.
- **HDInsight** wordt gebruikt om gegevens op schaal transformeren
- **Data Factory** verwerkt orchestration, Bronnenbeheer plannen, en de controle van de pijpleiding batch-verwerking.

**Power BI** biedt deze oplossing een rijke dashboard voor realtime-gegevens en voorspellende analytics visualisaties. 

De oplossing maakt gebruik van twee verschillende gegevensbronnen: **gesimuleerde voertuig signalen en diagnostische dataset** en **voertuig catalogus**.

Een voertuig telematica simulator maakt deel uit van deze oplossing. Het zendt diagnostische gegevens en signalen overeenkomt met de staat van het voertuig en de proefcyclus op een bepaald moment in de tijd. 

De catalogus van het voertuig is een verwijzing dataset met Chassisnummer model toewijzen aan


## <a name="powerbi-dashboard-preparation"></a>Dashboard PowerBI voorbereiding

### <a name="deployment"></a>Implementatie

Zodra de installatie is voltooid, ziet u in het volgende diagram met al deze onderdelen in het groen gemarkeerd. 

- Ga naar de bijbehorende services wilt controleren of deze hebt geïmplementeerd, klikt u op de pijl in de rechterbovenhoek van de groene knooppunten.
- Om te downloaden van het pakket gegevens simulator, klikt u op de pijl op de bovenste rechts op het knooppunt **Voertuig telematica Simulator** . Opslaan en pak de bestanden lokaal op uw computer. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Nu bent u klaar voor het configureren van het dashboard PowerBI met rijke visualisaties krijgen real-time en voorspellende inzichten voertuig gezondheid en rijden gewoonten. Het duurt ongeveer 45 minuten tot een uur alle rapporten maken en configureren van het dashboard. 


### <a name="setup-power-bi-real-time-dashboard"></a>Power BI realtime Dashboard instellen

**Gesimuleerde gegevens genereren**

1. Ga naar de map waarin u het voertuig telematica Simulator-pakket hebt uitgepakt op uw lokale machine![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  De ***CarEventGenerator.exe***van de toepassing worden uitgevoerd.
3.  Het zendt diagnostische gegevens en signalen overeenkomt met de staat van het voertuig en de proefcyclus op een bepaald moment in de tijd. Dit wordt gepubliceerd naar een exemplaar van Azure gebeurtenis Hub die is geconfigureerd als onderdeel van uw distributie.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**Start de dashboardtoepassing real-time**

De oplossing bevat een toepassing die een realtime dashboard in PowerBI genereert. Deze toepassing luistert naar een gebeurtenis Hub instantie, waaruit Stream Analytics wordt gepubliceerd de gebeurtenissen continu. Voor elke gebeurtenis die deze toepassing ontvangt, verwerkt de gegevens met behulp van een Machine Learning aanvraag/antwoord-score eindpunt. De resulterende gegevensset wordt gepubliceerd naar de PowerBI push API's voor visualisatie. 

De toepassing te downloaden:

1.  Klik op het knooppunt PowerBI in de diagramweergave en klikt op de **Real-time dashboardtoepassing downloaden**' koppeling in het deelvenster Eigenschappen.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Uitpakken en de toepassing lokaal opslaan![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  De **RealtimeDashboardApp.exe** van de toepassing uitvoeren
4.  Geldige referenties voor Power BI, log in en klik op **accepteren**
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>PowerBI rapporten configureren
De real-time rapporten en het dashboard nemen ongeveer 30-45 minuten te voltooien. Ga naar [http://powerbi.com](http://powerbi.com) en login.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Een nieuwe gegevensset wordt gegenereerd in Power BI. Klik op de **ConnectedCarsRealtime** dataset.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Sla het lege rapport met behulp van **Ctrl + s**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Naam *voertuig telemetrie Analytics Real-time - rapporten*bevatten.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Realtime rapporten
Er zijn drie real-time rapporten in deze oplossing:

1.  Voertuigen in bewerking
2.  Voertuigen vereisen onderhoud
3.  Gezondheidsstatistieken voertuigen

U kunt de drie real-time rapporten configureren of stop na elke fase en gaat u verder met de volgende sectie voor het configureren van de batch-rapporten. We raden u aan het maken van de drie rapporten de volledige inzichten van het real-time pad van de oplossing te visualiseren.  

### <a name="1-vehicles-in-operation"></a>1. voertuigen in bewerking
  
Dubbelklik op **pagina 1** en de naam "Voertuigen in bewerking"  
    ![Auto's - voertuigen in bewerking verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

**Vin** veld selecteren in **velden** en kies visualisatie als **"Kaart"**.  

Kaart visualisatie wordt gemaakt, zoals in figuur.  
    ![Verbonden Auto - Select vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

**Plaats** en **vin** uit velden selecteert. Visualisatie wijzigen in **"Map"**. **Vin** Sleep in het waardengebied. Sleep de **stad** uit velden naar **legenda** .   
    ![Auto - kaart visualisatie verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
Sectie **Opmaak** van **Visualisaties**selecteren, klikt u op de **titel** en de **tekst** wijzigen in **"Voertuigen in gebruik door de stad"**.  
    ![Auto's - voertuigen in bewerking per plaats verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Laatste visualisatie ziet eruit zoals weergegeven in figuur.    
    ![Verbonden auto's - laatste visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Selecteer **stad** en **vin**, visualisatie wijzigen in een **Gegroepeerd kolomdiagram**. Het veld **plaats** in **as-** en **chassisnummer** in **waarde** te garanderen  

Sorteren van de grafiek door **"Aantal vin"**  
    ![Auto's verbonden - telling van vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Grafiek **titel** wijzigt in **"Voertuigen in gebruik door de stad"**  

Klik op de sectie **Opmaak** en selecteer vervolgens **Kleuren voor gegevens**, klikt u op de **'Op'** Alles **Weergeven**  
    ![Verbonden auto's - weergeven alle gegevens kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

De kleur van afzonderlijke plaats wijzigen door te klikken op het Kleurenpictogram.  
    ![Aangesloten auto - kleuren wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

**Gegroepeerd kolomdiagram** visualisatie van visualisaties selecteren, sleept u het veld **plaats** in gebied **as** **Model** in **legenda** gebied en het **chassisnummer** in **waarde** .  
    ![Auto's verbonden - gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Verbonden auto - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
Alle visualisatie op deze pagina rangschikken zoals aangegeven in figuur.  
    ![Auto's verbonden - visualisaties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

De real-time rapport 'Voertuigen van de bewerking' hebt geconfigureerd. De volgende real-time rapport maken of hier stoppen en configureren van het dashboard kunt u doorgaan. 

### <a name="2-vehicles-requiring-maintenance"></a>2. voertuigen vereisen onderhoud
  
Klik op ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om een nieuw rapport toevoegen, wijzig de naam **' voertuigen vereisen onderhoud '**

![Verbonden auto - onderhoud vereisen voertuigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Selecteer veld **vin** en visualisatie wijzigen in **kaart**.  
    ![Verbonden auto's - Vin kaart visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

We hebben een veld met de naam 'MaintenanceLabel' In de gegevensset. Dit veld kan een waarde van "0" of "1" hebben." Door het model Azure Machine Learning ingericht als onderdeel van de oplossing en geïntegreerd met de real-time pad ingesteld. De waarde "1" geeft dat een voertuig vereist onderhoud. 

Filter toe te voegen een **Niveau van de pagina** voor het weergeven van gegevens van de voertuigen, die onderhoud vereisen: 

1. Sleep het veld **'MaintenanceLabel'** in de **Pagina niveau Filters**.  
![Auto's verbonden - paginaniveau Filters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. Klik op menu aanwezig aan de onderkant van MaintenanceLabel pagina niveau Filter **Eenvoudig filteren** .  
![Auto's - verbonden Basic filteren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  De waarde voor het filter ingesteld op **"1"**    
![Auto's verbonden - waarde voor het Filter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

**Gegroepeerd kolomdiagram** selecteren van visualisaties  
![Auto's verbonden - Vind kaart visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Auto's verbonden - gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Sleep het veld **Model** in het gebied van **as** , **chassisnummer** op gebied van **waarde** Visualisatie door **aantal vin**vervolgens sorteren.  Grafiek **titel** wijzigt in **"Voertuigen vereisen onderhoud door model"**  

**Vin** velden slepen in de **Kleurverzadiging** aanwezig zijn op de **velden** ![velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) sectie van **visualisatie** , tabblad  
![Auto's verbonden - kleurverzadiging](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

**Gegevens** wijzigen in visualisaties van sectie **Opmaak**  
Minimum kleur te wijzigen: **F2C812**  
Maximale kleur te wijzigen: **FF6300**  
![Auto's - kleurwijzigingen verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Auto's - nieuwe visualisatie kleuren verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

**Gegroepeerd kolomdiagram** van visualisaties selecteren, Sleep **vin** -veld in het gebied van de **waarde** , sleept u het veld **plaats** in het gebied van de **as** . Grafiek door **"Aantal vin"**sorteren. Grafiek **titel** wijzigt in **"Voertuigen vereisen onderhoud stad"**   
![Verbonden auto's - voertuigen vereisen onderhoud per plaats](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Visualisatie van de **Kaart met meerdere rijen** selecteren van visualisaties, **Model** en **chassisnummer** Sleep in **het gebied** .  
![Auto's verbonden - kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Herschikken van alle van de visualisatie, het definitieve rapport ziet er als volgt:  
![Auto's verbonden - kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

De real-time rapport 'Voertuigen vereisen onderhoud' hebt geconfigureerd. De volgende real-time rapport maken of hier stoppen en configureren van het dashboard kunt u doorgaan. 

### <a name="3-vehicles-health-statistics"></a>3. voertuigen gezondheidsstatistieken
  
Klik op ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om een nieuw rapport toevoegen, wijzig de naam **"gezondheidsstatistieken voertuigen"**  

**Spoorwijdte** visualisatie van visualisaties selecteren en sleep het veld **snelheid** in **waarde, de minimumwaarde, de maximumwaarde** gebieden.  
![Auto's verbonden - kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

De standaard-aggregatie van **snelheid** in **waarde** wijzigen in **gemiddelde** 

De standaard-aggregatie van de **snelheid** in het **gebied ten minste** tot **Minimum** wijzigen

De standaard-aggregatie van **snelheid** in **Maximum gebied** met **maximale** wijziging

![Auto's verbonden - kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Wijzig de naam van de **Titel van de meter** **"Gemiddelde snelheid"** 
 
![Auto's verbonden - meter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Een **profiel** voor **gemiddelde motorolie**, **gemiddelde brandstof**en **gemiddelde engine gematigde klimaatzone**op dezelfde manier toevoegen.  

Wijziging de standaardwaarde aggregatie van velden in elke meter als per boven stappen in **'gemiddelde-snelheid** meter.

![Auto's verbonden - meters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.

Selecteer **regel- en gegroepeerd kolomdiagram** van visualisaties, klik en sleep het veld **plaats** naar **As gedeeld**, slepen, **snelheid**, **tirepressure en engineoil velden** in het gebied voor de **Kolomwaarden** hun samenvoegingstype wijzigen in **gemiddelde**. 

Sleep het veld **engineTemperature** in het gebied **Waarden regels** , het samenvoegingstype wijzigen in **gemiddelde**. 

![Auto's verbonden - visualisaties velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

De **titel** van de grafiek wijzigen in **"gemiddelde snelheid, druk van de band, motorolie en motor temperatuur"**.  

![Auto's verbonden - visualisaties velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.

Selecteer **Treemap** visualisatie van visualisaties, sleept u het veld **Model** in het gebied van de **groep** en sleept u het veld **MaintenanceProbability** in het gebied **waarden** .

**'Voertuig modellen vereisen onderhoud'**de **titel** van de grafiek wijzigen.

![Aangesloten auto - titel van de grafiek wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.

**100% gestapeld staafdiagram** van visualisatie te selecteren, sleept u het veld **plaats** naar het gebied van de **as** en sleept u de **MaintenanceProbability**, **RecallProbability** -velden in het gebied voor de **waarde** .

![Verbonden auto's - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klik op **Opmaak**, **Gegevens kleuren**selecteren en de kleur van de **MaintenanceProbability** ingesteld op de waarde **'F2C80F'**.

De **titel** van de grafiek wijzigen in **' kans van voertuig onderhoud & intrekken door City '**.

![Verbonden auto's - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.

Selecteer **Vlakdiagram** in visualisatie van visualisaties, sleept u het veld **Model** in het gebied van de **as** en sleept u de velden **engineOil, tirepressure, snelheid en MaintenanceProbability** in het gebied **waarden** . Het samenvoegingstype wijzigen in **'Gemiddelde'**. 

![Auto's - aggregatie wijzigingstype verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

De titel van de grafiek wijzigen in **"Gemiddelde motorolie, druk, snelheid en onderhoud kans door model genoeg"**.

![Aangesloten auto - titel van de grafiek wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie:

1. Selecteer **Strooidiagram** visualisatie van visualisaties.
2. Sleep het veld **Model** naar het gebied **gegevens** en de **legenda** .
3. Sleep het veld **brandstof** naar het gebied van de **x-as** , de samenvoeging wijzigen in **gemiddelde**.
4. Sleep de **engineTemparature** in het **gebied van de y-as**, de samenvoeging wijzigen in **gemiddelde**
5. Sleep het veld **chassisnummer** in het gebied **formaat** .


![Verbonden auto's - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

De **titel** van de grafiek wijzigen in **"Gemiddelden van de brandstof, de temperatuur van de motor door Model"**.

![Aangesloten auto - titel van de grafiek wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Het definitieve rapport eruitziet zoals hieronder wordt weergegeven.

![Auto's definitief rapport verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>PIN visualisaties van de rapporten voor het real-time dashboard
  
Een lege dashboard maken door te klikken op het plusteken naast de Dashboards. U kan 'Voertuig telemetrie Analytics Dashboard' naam

![Auto's-Dashboard verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

De visualisatie van de bovengenoemde rapporten aan het dashboard vastmaken. 
 
![Auto's-Dashboard verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Het dashboard ziet er als volgt wanneer de drie rapporten worden gemaakt en de bijbehorende visualisaties zijn vastgemaakt aan het dashboard. Als u niet alle rapporten hebt gemaakt, het dashboard kan er anders uitzien. 

![Auto's-Dashboard verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gefeliciteerd! De real-time dashboard hebt gemaakt. Als u doorgaat met het uitvoeren van CarEventGenerator.exe en RealtimeDashboardApp.exe, ziet u live updates op het dashboard. Het moet ongeveer 10 tot 15 minuten duren voordat de volgende stappen uit te voeren.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>Power BI batch-verwerking dashboard instellen

>[AZURE.NOTE] Het duurt ongeveer twee uur (van de succesvolle afronding van de distributie) voor de pijplijn batchverwerking complete uitvoering te voltooien en een jaar ter waarde van de gegenereerde gegevens verwerken. Dus wachten op de verwerking te voltooien voordat u verdergaat met de volgende stappen. 

**Download het PowerBI designer-bestand**

-   Een vooraf geconfigureerde PowerBI designer-bestand maakt deel uit van de implementatie van
-   Klik op het knooppunt PowerBI in de weergave Netwerkdiagram en klik op de koppeling in het deelvenster Eigenschappen **de PowerBI designer-bestand downloaden**![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Lokaal opslaan

**PowerBI rapporten configureren**

-   De designer "VehicleTelemetryAnalytics - Desktop Report.pbix"-bestand openen met PowerBI Desktop. Als u niet hebt nog, installeer het bureaublad PowerBI uit [PowerBI Desktop installeren](http://www.microsoft.com/download/details.aspx?id=45331). 

-   Klik op de **query's te bewerken**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Dubbelklik op de **bron**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Server-verbindingstekenreeks bijwerken met de Azure SQL server die is ingericht als onderdeel van de implementatie. Klik op het knooppunt Azure SQL in het diagram en de naam van de server van het deelvenster Eigenschappen weergeven.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- Laat *connectedcar* **Database** .

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Klik op **OK**.
- **Windows-referenties** tabblad geselecteerd door standaard, wordt deze naar de **Database referenties** wijzigen door te klikken op het tabblad **Database** rechts wordt weergegeven.
- Geef de **gebruikersnaam** en het **wachtwoord** van uw Azure SQL-Database die is opgegeven tijdens de installatie van de implementatie.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Klik op **verbinding maken**
- Herhaal de bovenstaande stappen voor elk van de drie resterende query's aanwezig zijn op het rechter deelvenster en vervolgens de details van gegevensbron verbinding bijwerken.
- Klik op **sluiten en op te laden**. Power BI bureaublad bestand datasets zijn verbonden met databasetabellen van SQL Azure.
- **Sluiten** Power BI Desktop-bestand.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Klik op de knop **Opslaan** om de wijzigingen opslaan. 
 
U hebt nu alle rapporten die overeenkomt met het pad van de batch verwerking in de oplossing geconfigureerd. 


## <a name="upload-to-powerbicom"></a>Uploaden naar *powerbi.com*
 
1.  Ga naar het webportaal PowerBI op http://powerbi.com en aanmelding.
2.  Klik op **gegevens ophalen**  
3.  Upload het bestand Power BI-bureaublad.  
4.  Als u wilt uploaden, klikt u op **gegevens ophalen -> bestanden ophalen lokaal bestand ->**  
5.  Ga naar de **"Desktop Report.pbix VehicleTelemetryAnalytics –"**  
6.  Nadat het bestand is geüpload, wordt naar uw werkruimte Power BI.  

Een dataset, rapport en een lege dashboard zal worden gemaakt.  
 

PIN grafieken aan het bestaande **Voertuig telemetrie Analytics Dashboard** in **Power BI**-dashboard. Klik op het lege dashboard dat hierboven is gemaakt en Ga naar de sectie **rapporten** Klik het rapport onlangs geüpload.  

![Telemetrie-PowerBI.com voertuig](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Opmerking het rapport heeft zes pagina's:**  
Pagina 1: Voertuig dichtheid  
Pagina 2: Real-time voertuig gezondheid  
Pagina 3: Serieus aangedreven voertuigen   
Pagina 4: Teruggehaald voertuigen  
Pagina 5: Brandstof efficiënt aangedreven voertuigen  
Pagina 6: Contoso Logo  

![PowerBI.com verbonden auto 's](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Vanaf pagina 3**, PIN-code de volgende:  

1.  Telling van de VIN  
    ![PowerBI.com verbonden auto 's](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Serieus aangedreven voertuigen per model – waterval grafiek  
    ![Voertuig telemetrie - grafieken Pin 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Pagina 5 van**de volgende pin: 
 
1.  Telling van de vin    
    ![Voertuig telemetrie - Pin grafieken 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Brandstof voertuigen per model: gegroepeerd kolomdiagram  
    ![Voertuig telemetrie - grafieken Pin 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Pagina 4 van**de volgende pin:  

1.  Telling van de vin  
    ![Voertuig telemetrie - grafieken Pin 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Ingetrokken voertuigen per plaats, model: Treemap  
    ![Voertuig telemetrie - grafieken Pin 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Vanaf pagina 6**, PIN-code de volgende:  

1.  Contoso Motors logo  
    ![Voertuig telemetrie - grafieken Pin 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Het dashboard ordenen**  

1.  Ga naar het dashboard
2.  De muisaanwijzer op elke grafiek en naam wijzigen op basis van de namen die in de onderstaande afbeelding voor volledige dashboard. De grafieken rond ook verplaatsen naar het onderstaande dashboard eruit.  
    ![Voertuig telemetrie - 2 Dashboard ordenen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![Telemetrie-PowerBI.com voertuig](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Als u kunt alle rapporten hebt gemaakt, zoals vermeld in dit document, ziet het laatste voltooide dashboard er in de volgende afbeelding. 

![Voertuig telemetrie - 2 Dashboard ordenen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Gefeliciteerd! U hebt gemaakt met de rapporten en het dashboard te krijgen real-time, voorspellende inzichten voertuig gezondheid en rijden batch gewoonten.  
