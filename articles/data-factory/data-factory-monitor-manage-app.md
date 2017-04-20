<properties 
    pageTitle="Bewaken en beheren van pijpleidingen Azure Data Factory" 
    description="Informatie over Monitoring en beheer App gebruiken om te bewaken en beheren van gegevens Azure fabrieken en pijpleidingen." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Bewaken en beheren van Azure Data Factory pijpleidingen met nieuwe Monitoring en beheer App
> [AZURE.SELECTOR]
- [Azure Portal/Azure PowerShell gebruiken](data-factory-monitor-manage-pipelines.md)
- [Met behulp van controle en beheer App](data-factory-monitor-manage-app.md)

In dit artikel wordt beschreven hoe bewaken, beheren en de pijpleidingen voor foutopsporing en meldingen op de hoogte worden gebracht van problemen met de **controle en beheer App**. U kunt ook de volgende video voor meer informatie over het gebruik van controle en beheer App bekijken.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Starten van de controle en beheer App een
Om de Monitor en Management App starten op **Monitoring & beheren** naast elkaar op het blad **DATA FACTORY** voor uw data factory genoemd.

![Toezicht op naast elkaar op de introductiepagina Data Factory](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Bewaking en beheer App gestart in een afzonderlijk tabblad/venster te zien.  

![Bewaking en beheer App](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Als de webbrowser vastzit op 'Machtigen...' wordt weergegeven, **cookies van derden blokkeren en sitegegevens** instelling uitschakelen/uitschakelen (of) blijft ingeschakeld en een uitzondering maken voor **login.microsoftonline.com** en probeer het opnieuw starten van de app.


Als u windows in de lijst onder activiteit niet ziet, klikt u op de knop **vernieuwen** op de werkbalk om de lijst te vernieuwen. Bovendien de juiste waarden voor de **Begintijd** en **eindtijd** filters instellen.  


## <a name="understanding-the-monitoring-and-management-app"></a>Wat het toezicht op en beheer App
Er zijn drie tabbladen (**Resource Explorer**, **Bewaking**en **waarschuwingen**) aan de linkerkant en het eerste tabblad (Resource Explorer) is standaard ingeschakeld. 

### <a name="resource-explorer"></a>Resource Explorer
U ziet het volgende: 

- Resource Explorer **boomstructuurweergave** in het linkerdeelvenster.
- De **Weergave Netwerkdiagram** boven.
- **Activiteit Windows** lijst onderaan in het middelste deelvenster.
- **Eigenschappen**/**Activiteit venster Explorer** tabs in het rechterdeelvenster. 

In de Resource Explorer ziet u alle resources (pijplijnen, datasets, gekoppelde services) in de fabriek van de gegevens in een boomstructuur. Wanneer u een object in de Resource Explorer selecteert, ziet u het volgende: 

- gekoppelde gegevens fabriek entiteit wordt gemarkeerd in de weergave Netwerkdiagram.
- windows activiteit is gekoppeld (Klik [hier](data-factory-scheduling-and-execution.md) voor meer informatie over windows activiteit) worden gemarkeerd in de lijst onder Windows activiteit.  
- Eigenschappen van het geselecteerde object in het venster Eigenschappen in het rechterdeelvenster. 
- JSON-definitie van het geselecteerde object, indien van toepassing. Bijvoorbeeld: een gekoppelde service of een dataset of een pijpleiding. 

![Resource Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Zie [planning en uitvoering van](data-factory-scheduling-and-execution.md) artikel voor gedetailleerde informatie over het activiteitvenster. 

### <a name="diagram-view"></a>Diagram weergeven
De weergave van het Diagram van een fabriek van gegevens biedt één deelvenster van glas, bewaken en beheren van de gegevens-fabriek en haar activa. Wanneer u een entiteit Data Factory (dataset/pijpleiding) in de weergave Netwerkdiagram selecteert, ziet u het volgende:
 
- de entiteit data factory is geselecteerd in de overzichtsweergave
- bijbehorende activiteit windows worden gemarkeerd in de lijst Windows activiteit.
- Eigenschappen van het geselecteerde object in het venster Eigenschappen

Wanneer de pijpleiding is ingeschakeld (niet in de status ' onderbroken '), wordt het weergegeven met een groene lijn. 

![Met Pipeline](./media/data-factory-monitor-manage-app/PipelineRunning.png)

U merkt dat er drie knoppen voor de pijpleiding die in de weergave Netwerkdiagram zijn. U kunt de tweede knop onderbreken van de pijpleiding. Onderbreken niet beëindigen van de activiteiten die momenteel wordt uitgevoerd en u kunt doorgaan met de voltooiing. Derde knop de pijpleiding wordt onderbroken en wordt de bestaande activiteiten uitvoeren. Eerste knop hervat de pijplijn. Wanneer de pijpleiding wordt onderbroken, ziet u de kleurwijziging voor de pijpleiding naast elkaar als volgt.

![Onderbreken/hervatten op tegel](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

U kunt meervoudige selectie van twee of meer pijplijnen (met CTRL) en met opdrachtbalkknoppen onderbreken/hervatten pijpleidingen met meerdere tegelijk.

![Onderbreken/hervatten op opdrachtbalk](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

De activiteiten in de pipeline, kunt u bekijken door met de rechtermuisknop op de tegel pijpleiding en **Open pijpleiding**.

![Pipeline-menu openen](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

In de weergave geopend pipeline ziet u alle activiteiten in de pijplijn. In dit voorbeeld is slechts één activiteit: activiteit kopiëren. Om terug te gaan naar de vorige weergave, klikt u op gegevens fabriek naam in de ' breadcrumb ' op het menu boven.

![Geopende pijpleiding](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

In de pipeline-weergave ziet wanneer u op een dataset uitvoer of wanneer u de muis op de dataset uitvoer plaatst u de pop-up Windows activiteit voor deze dataset.

![Pop-upvenster activiteit](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

U kunt klikken op een activiteitvenster om de details voor deze zien in **het eigenschappenvenster in het rechterdeelvenster** . 

![Eigenschappen van het venster activiteit](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Overschakelen naar **Activiteit venster Explorer** tabblad voor meer details in het rechterdeelvenster.

![Activiteit venster Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

U ziet ook **opgelost variabelen** voor elke activiteit uitvoeren in de sectie **pogingen** probeert. 

![Omgezette variabelen](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Ga naar het tabblad **Script** voor een overzicht van de definitie van de JSON-script voor het geselecteerde object.   

![Tabblad script](./media/data-factory-monitor-manage-app/ScriptTab.png)

U kunt windows activiteit op drie plaatsen zien:

- Activiteit Windows pop-up in de weergave Netwerkdiagram (het middelste deelvenster).
- Activiteit venster Explorer in het rechterdeelvenster.
- Activiteitenlijst Windows in het onderste deelvenster.

U kunt naar vorige week en volgende week met pijlen naar links en rechts schuiven in de activiteit Windows pop- en activiteit venster Explorer.

![Activiteit venster Explorer links/rechts pijlen](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

De onderkant van de weergave Netwerkdiagram ziet u knoppen wilt inzoomen, uitzoomen, in-/ uitzoomen past, 100% Zoom, lay-out te vergrendelen. De knop vergrendelen voorkomt dat u per ongeluk tabellen en pijpleidingen in de weergave Netwerkdiagram verplaatsen en is standaard ingeschakeld. U kunt deze uitschakelen en entiteiten in het diagram verplaatsen. Als u dit inschakelt, kunt u de laatste knop om automatisch tabellen en pijpleidingen. Kunt u ook inzoomen / uitzoomen met de muiswiel.

![Diagram Weergavezoom opdrachten](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Lijst met Windows
De activiteitenlijst van windows onder in het middelste deelvenster wordt weergegeven voor alle vensters van de activiteit voor de gegevensset die u hebt geselecteerd in de weergave Netwerkdiagram of de resource explorer. Standaard is de lijst in de aflopende volgorde, wat betekent dat u het laatste venster van de activiteit aan de bovenkant te zien. 

![Lijst met Windows](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Deze lijst wordt niet automatisch vernieuwen, dus de knop Vernieuwen op de werkbalk gebruiken om deze handmatig te vernieuwen.  


De activiteit windows zijn in een van de volgende statussen:

<table>
<tr>
    <th align="left">Status</th><th align="left">Substatus</th><th align="left">Beschrijving</th>
</tr>
<tr>
    <td rowspan="8">Een ogenblik geduld</td><td>ScheduleTime</td><td>De tijd is niet afkomstig van het activiteitvenster uit te voeren.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>De upstream afhankelijkheden zijn niet gereed.</td>
</tr>
<tr>
<td>ComputeResources</td><td>De compute-bronnen zijn niet beschikbaar.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alle exemplaren van de activiteit zijn bezet met andere windows activiteit.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Activiteit is onderbroken en de activiteit van windows kan niet worden uitgevoerd totdat deze wordt hervat.</td>
</tr>
<tr>
<td>Probeer het opnieuw</td><td>Uitvoering van de activiteit wordt opnieuw geprobeerd.</td>
</tr>
<tr>
<td>Validatie</td><td>Validatie is nog niet begonnen.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>In afwachting van de validatie opnieuw worden geprobeerd.</td>
</tr>
<tr>
<tr
<td rowspan="2">InProgress</td><td>Valideren</td><td>Validatie is uitgevoerd.</td>
</tr>
<td></td>
<td>Het activiteitvenster wordt verwerkt.</td>
</tr>
<tr>
<td rowspan="4">Is mislukt</td><td>Time-out</td><td>Uitvoering duurde langer dan die van de activiteit is toegestaan.</td>
</tr>
<tr>
<td>Geannuleerd</td><td>Geannuleerd door een gebruiker.</td>
</tr>
<tr>
<td>Validatie</td><td>Validatie is mislukt.</td>
</tr>
<tr>
<td></td><td>Genereren en/of het activiteitvenster valideren is mislukt.</td>
</tr>
<td>Gereed</td><td></td><td>Het activiteitvenster is gereed voor consumptie.</td>
</tr>
<tr>
<td>Overgeslagen</td><td></td><td>Het activiteitvenster wordt niet verwerkt.</td>
</tr>
<tr>
<td>Geen</td><td></td><td>Een activiteitvenster die bestaan met een andere status gebruikt, maar is opnieuw ingesteld.</td>
</tr>
</table>


Wanneer u een venster van de activiteit in de lijst klikt, ziet u meer informatie over het in het venster **Eigenschappen** of **Activiteit Windows Verkenner** op het recht.

![Activiteit venster Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Activiteit windows vernieuwen  
De details worden niet automatisch vernieuwd, zodat u de **vernieuwen** knop (tweede) op de werkbalk om handmatig te vernieuwen de activiteitenlijst van windows.  
 

### <a name="properties-window"></a>Het eigenschappenvenster
Het venster Eigenschappen wordt in het deelvenster rechts van de controle- en app. 

![Het eigenschappenvenster](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Eigenschappen van het item dat u hebt geselecteerd in de resource explorer (structuurweergave) (of) diagram weergeven (of) lijst met windows wordt weergegeven. 

### <a name="activity-window-explorer"></a>Activiteit venster Explorer

Het venster **Activiteit venster Explorer** is in het deelvenster rechts van de controle en beheer App. Meer informatie over het activiteitvenster die u hebt geselecteerd in het pop-activiteit-Windows of de activiteit Windows lijst wordt weergegeven. 

![Activiteit venster Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

U kunt overschakelen naar een ander venster van de activiteit door erop te klikken in de kalenderweergave boven. U kunt ook de **pijl-links**/knoppen**pijl-rechts** bovenin Zie windows activiteit van de vorige/volgende week.

U kunt de knoppen op de werkbalk in het onderste deelvenster **opnieuw uit** het activiteitvenster of **vernieuwen** de details in het deelvenster. 

### <a name="script"></a>Script 
U kunt het tabblad **Script** om de JSON-definitie van de geselecteerde entiteit Data Factory (gekoppelde service dataset en pijpleiding) weer te geven. 

![Tabblad script](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Met behulp van systeemweergaven
Bewaking en beheer App bevat vooraf gebouwde systeemweergaven (**recente activiteit windows** **mislukt activiteit windows**, **Windows activiteit In uitvoering**) waarmee u windows recente/is niet/lopende activiteiten voor de fabriek gegevens weergeven. 

Overschakelen naar het tabblad **Weergaven controleren** aan de linkerkant door erop te klikken. 

![Weergaven tabblad controle](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Er zijn momenteel drie systeemweergaven ondersteund. Selecteer een optie om te zien recente activiteit windows (of) mislukte activiteit windows (of) activiteit in de uitvoering van windows in de lijst Windows activiteit (onderaan in het middelste deelvenster). 

Als u **recente activiteit windows** selecteert, ziet u alle vensters in de recente activiteit in dalende volgorde van **tijd laatste poging**. 

U kunt de weergave **kan niet activiteit windows** vindt u alle mislukte activiteit windows in de lijst. Selecteer een activiteitvenster mislukte in de lijst voor informatie over het in het **Eigenschappen** venster (of) **Activiteit venster Explorer**. U kunt ook downloaden van alle logboeken voor een activiteitvenster is mislukt. 


## <a name="sorting-and-filtering-activity-windows"></a>Sorteren en filteren van de activiteit windows
De instellingen voor **de begintijd** en **eindtijd** in de opdrachtbalk filtervensters activiteit wijzigen. Nadat u begin- en eindtijd wijzigt, klikt u op de knop naast de eindtijd voor het vernieuwen van de lijst Windows activiteit.

![Begin- en eindtijden](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] Op dit moment zijn alle tijden in UTC-notatie in de Monitoring en Management App. 

Klik op de naam van een kolom in de **lijst Windows activiteit**(bijvoorbeeld: Status). 

![Lijst met Windows-upmenu ' kolom activiteit](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

U kunt het volgende doen:

- Sorteren in oplopende volgorde.
- Sorteren in aflopende volgorde.
- Filteren op een of meer waarden (gereed, wachten, enz.)

Als u een filter voor een kolom instellen, ziet u de filterknop ingeschakeld voor die kolom om aan te geven dat de waarden in de kolom gefilterde waarden zijn. 

![In de kolom activiteit Windows lijst filteren](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

U kunt het pop-upvenster dat dezelfde filters wissen. Schakelt u alle filters voor de activiteitenlijst van windows, klikt u op de knop filter wissen op de werkbalk. 

![Alle filters in de lijst Windows activiteit wissen](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Batch acties uitvoeren

### <a name="rerun-selected-activity-windows"></a>Geselecteerde activiteit windows opnieuw uitvoeren
Selecteer een activiteitvenster, klikt u op de pijl-omlaag voor de eerste knop en selecteer **opnieuw uitvoeren** / **opnieuw uit te voeren met upstream in de pijplijn**. Wanneer u optie **opnieuw uitvoeren met upstream in de pipeline** , opnieuw het alle upstream activiteiten windows ook. 
    ![Een venster van de activiteit opnieuw uitvoeren](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Ook meerdere vensters van de activiteit in de lijst selecteren en ze tegelijkertijd uitvoeren. U kunt filteren activiteit windows op basis van de status (bijvoorbeeld: **mislukt**) en de activiteit van mislukte windows start opnieuw na het corrigeren van het probleem dat ervoor zorgt de activiteit van windows dat worden uitgevoerd. Zie de volgende sectie voor meer informatie over het filteren van windows in de lijst activiteiten.  

### <a name="pauseresume-multiple-pipelines"></a>Meerdere pijpleidingen onderbreken/hervatten
U kunt meervoudige selectie van twee of meer pijplijnen (met CTRL) en gebruikt opdrachtbalkknoppen (gemarkeerd in een rode rechthoek in de volgende afbeelding) te onderbreken/hervatten ze tegelijk.

![Stand-by/hervatten op opdrachtbalk](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Waarschuwingen maken 
De pagina waarschuwingen kunt u een melding maakt, de bestaande waarschuwingen weergeven/bewerken/verwijderen. U kunt ook inschakelen/uitschakelen een waarschuwing. De waarschuwingen als pagina wilt weergeven, klikt u op het tabblad meldingen.

![Tabblad Alerts](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Een signaal maken

1. Klik op **Waarschuwing toevoegen** om een waarschuwing toevoegen. U ziet de pagina Details. 

    ![Waarschuwingen - pagina maken](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Geef de **naam** en **Beschrijving** voor de waarschuwing en klik op **volgende**. U ziet de pagina **Filters** .

    ![Waarschuwingen - pagina Filters maken](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Selecteer de **gebeurtenis**, **status**en **substatus** (optioneel) waarop de Data Factory service om u te waarschuwen en klik op **volgende**. U ziet de pagina **geadresseerden** .

    ![Waarschuwingen - ontvangers pagina maken](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. Selecteer de optie **E-mail abonnement beheerders** en/of **beheerder extra e-mailadres**invoeren klikt u op **Voltooien**. Hier ziet u de waarschuwing in de lijst. 
    
    ![Lijst met waarschuwingen](./media/data-factory-monitor-manage-app/AlertsList.png)

Gebruik de knoppen die zijn gekoppeld aan de waarschuwing om te bewerken/verwijderen/inschakelen/uitschakelen een waarschuwing in de lijst met waarschuwingen. 

### <a name="eventstatussubstatus"></a>Status-gebeurtenis/substatus
De volgende tabel bevat de lijst met beschikbare gebeurtenissen en statussen (en substatus).

Naam van evenement | Status | Substatus
-------------- | ------ | ----------
Activiteit uitvoeren gestart | De slag | Starten
Activiteit hebt uitgevoerd | Geslaagd | Geslaagd 
Activiteit hebt uitgevoerd | Is mislukt| Brontoewijzing is mislukt<br/><br/>Mislukte uitvoering<br/><br/>Time-out<br/><br/>De validatie is mislukt<br/><br/>Afgebroken
On-Demand HDI Cluster maken begonnen | De slag | &nbsp; |
On-Demand HDI Cluster gemaakt. | Geslaagd | &nbsp; |
On-Demand HDI Cluster verwijderd | Geslaagd | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>Om te bewerken/verwijderen/uitschakelen van waarschuwing


![Waarschuwingen-knoppen](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


