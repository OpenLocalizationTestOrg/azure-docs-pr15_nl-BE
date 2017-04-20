<properties
    pageTitle="Oplossing voor bedrijfsbeheer in OMS werken | Microsoft Azure"
    description="Dit artikel is bedoeld om te begrijpen hoe deze oplossing wordt gebruikt voor het beheren van updates voor Windows en Linux-computers."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Voor updatebeheer in OMS](./media/oms-solution-update-management/update-management-solution-icon.png) Voor updatebeheer in OMS

De Update-oplossing in OMS kunt u updates voor Windows en Linux-computers beheren.  U kunt snel beoordelen van de status van updates beschikbaar op alle agentcomputers en start het proces van het installeren van de vereiste updates voor servers. 

## <a name="prerequisites"></a>Vereisten

-   Windows agenten moeten ofwel worden geconfigureerd om te communiceren met een server met Windows Server Update Services (WSUS) of toegang hebben tot Microsoft Update.  

    >[AZURE.NOTE] De Windows-agent worden niet door System Center Configuration Manager gelijktijdig beheerd.  
  
-   Agenten Linux moeten toegang hebben tot de opslagplaats voor een update.  De Agent OMS voor Linux kan worden gedownload van [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit om de Update-oplossing toevoegen aan uw werkruimte OMS en agenten Linux toe te voegen.  Windows-agenten worden automatisch toegevoegd aan geen aanvullende configuratie.

1.  De Update-oplossing toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen OMS toevoegen](../log-analytics/log-analytics-add-solutions.md) uit de galerie met oplossingen.  
2.  Selecteer **Instellingen** en **Aangesloten bronnen**in de portal OMS.  Opmerking de **ID van de werkruimte** en de **primaire sleutel** of **secundaire sleutel**.
3.  De volgende stappen uitvoeren voor elke computer Linux.

    een.  De meest recente versie van de Agent OMS voor Linux installeren door de volgende opdrachten uit te voeren.  Vervangen <Workspace ID> met de ID van de werkruimte en <Key> met de primaire of secundaire sleutel.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Als u wilt dat de agent, moet u de volgende opdracht uitvoeren.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Beheerpakketten

Als uw management System Center Operations Manager groep is verbonden met uw werkruimte OMS, wordt vervolgens de volgende beheerpakketten geïnstalleerd in Operations Manager als u deze oplossing toevoegt. Er is geen configuratie of het onderhoud van deze management packs vereist. 

-   Microsoft System Center Advisor Update beoordeling Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   MP implementatie bijwerken

Zie voor meer informatie over hoe de beheerpakketten oplossing worden bijgewerkt, [Operations Manager verbinding maken met Analytics logboek](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Het verzamelen van gegevens

### <a name="supported-agents"></a>Ondersteunde agenten

De volgende tabel beschrijft de gekoppelde bronnen die door deze oplossing worden ondersteund.

Aangesloten bron | Ondersteund | Beschrijving|
----------|----------|----------|
Windows-agenten | Ja | De oplossing wordt informatie verzameld over systeemupdates van agenten van Windows en start de installatie van de vereiste updates.|
Linux-agenten | Ja | De oplossing wordt informatie verzameld over systeemupdates van Linux-agenten.|
Operations Manager management groep | Ja | De oplossing wordt informatie verzameld over systeemupdates van agenten in een groep verbonden management.<br>Een directe verbinding van de Operations Manager agent met Analytics logboek is niet vereist. Gegevens uit de management-groep doorgestuurd naar de opslagplaats OMS.|
Azure opslag account | Nee | Azure opslag bevat geen informatie over systeemupdates van het.|  

### <a name="collection-frequency"></a>Frequentie van verzameling

Voor elke beheerde Windows-computer, wordt een scan uitgevoerd tweemaal per dag.  Wanneer een update wordt geïnstalleerd, wordt de informatie binnen 15 minuten bijgewerkt.  

Voor elke beheerde computer Linux wordt een scan uitgevoerd om 3 uur.  

## <a name="using-the-solution"></a>Met behulp van de oplossing

Wanneer u de Update-oplossing aan uw werkruimte OMS toevoegen, wordt de tegel **Updatebeheer** worden toegevoegd aan het dashboard OMS. Deze naast elkaar worden een aantal en een grafische weergave van het aantal computers in uw omgeving momenteel systeemupdates vereist.<br><br>
![Update Management samenvatting tegel](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Beoordelingen van de Update weergeven

Klik op de tegel **Update Management** voor het openen van het dashboard voor **Het beheer van updates** . Het dashboard bestaat uit de kolommen in de volgende tabel. Elke kolom bevat maximaal tien items die overeenkomen met de criteria van de kolom voor het opgegeven bereik en het tijdsbereik. U kunt een zoekfunctie waarmee alle records worden opgevraagd door te klikken op **alle** onder aan de kolom of door te klikken op de kolomkop kunt uitvoeren.

Kolom | Beschrijving|
----------|----------|
**Ontbrekende Updates computers** ||
Kritieke beveiligingsupdates of | De top tien computers ontbrekende updates is gesorteerd op het aantal updates lijsten zijn ontbreekt. Klik op de naam van de computer voor het uitvoeren van een logboek zoeken retourneren van dat alle records voor de computer bij te werken.|
Kritieke en beveiligingsupdates die ouder zijn dan 30 dagen| Geeft het aantal computers die kritieke ontbreekt of beveiligingsupdates die zijn gegroepeerd op de tijdsduur sinds de update is gepubliceerd. Klik op een van de items aan een logboek zoekactie retourneert alle ontbrekende en essentiële updates worden uitgevoerd.|
**Ontbrekende Updates vereist**||
Kritieke beveiligingsupdates of | Hier worden de classificaties van updates dat computers ontbreken gesorteerd op het aantal ontbrekende updates in de categorie computers. Klik op een classificatie voor het retourneren van dat alle records voor dat type update log zoekopdracht uitvoert.|
**Updates installeren**||
Updates installeren | Aantal geplande updates installeren en de duur tot de volgende geplande start.  Klik op de tegel op schema's weergeven die momenteel worden uitgevoerd en voltooide updates of een nieuwe implementatie plannen.|  
<br>  
![Update Management samenvatting Dashboard](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Update Management Dashboard Computer weergeven](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Management Dashboard pakket weergave bijwerken](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Updates installeren

Zodra de updates zijn beoordeeld voor alle Windows-computers in uw omgeving, kan de vereiste updates geïnstalleerd door het maken van een *Implementatie*.  De implementatie van een Update is een geplande installatie van de vereiste updates voor een of meer Windows-computers.  U geeft de datum en tijd voor de implementatie van een computer of groep computers die opgenomen worden moeten.  

Updates worden geïnstalleerd door runbooks in Azure automatisering.  U kunt deze runbooks momenteel niet weergeven en ze geen configuratie vereist.  Wanneer een Update-installatie wordt gemaakt, wordt een schema in die begint een master update runbook op het opgegeven tijdstip voor de opgenomen computers.  Dit model runbook begint een onderliggende runbook op elke Windows-agent die de installatie van de vereiste updates uitvoert.  

### <a name="viewing-update-deployments"></a>Update-installaties weergeven

Klik op de **Implementatie van de** tegel voor de lijst van bestaande Update-installaties.  Status: **gepland**, **uitgevoerd**en **voltooid**zijn gegroepeerd.<br><br> ![Pagina plannen update implementaties](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

De eigenschappen voor elke Update-installatie worden weergegeven worden in de volgende tabel beschreven.

Eigenschap | Beschrijving|
----------|----------|
Naam | De naam van de Update-installatie.|
Planning | Type schema.  *OneTime* is momenteel de enige mogelijke waarde.|
Begintijd|Datum en tijd waarop de implementatie wordt gepland te starten.|
Duur | Aantal minuten dat de implementatie van de Update mag worden uitgevoerd.  Als binnen deze duur niet alle updates zijn geïnstalleerd, moeten de resterende updates wachten tot de volgende Update-implementatie.|
Servers | Het aantal computers die worden beïnvloed door de installatie van de Update.|
Status | Huidige status van de Update-installatie.<br><br>Mogelijke waarden zijn:<br>-Niet gestart<br>-Actief<br>-Voltooid|  

Klik op de implementatie van een Update naar de details scherm waarin de kolommen in de volgende tabel.  Deze kolommen wordt niet gevuld als de implementatie van de Update is nog niet gestart.<br>

Kolom | Beschrijving|
----------|----------|
**Resultaten van de computer**||
Is voltooid | Toont het aantal computers in de Update-installatie met status.  Klik op de status van een logboek zoekopdracht retourneren van dat alle records met de status bijwerken voor de implementatie bijwerken uitvoert.|
Installatiestatus van computer| Hier worden de computers die betrokken zijn bij de implementatie en het percentage van de updates die zijn geïnstalleerd. Klik op een van de items aan een logboek zoekactie retourneert alle ontbrekende en essentiële updates worden uitgevoerd.|
**Resultaten exemplaar bijwerken**||
Installatie van de Status | Hier worden de classificaties van updates dat computers ontbreken gesorteerd op het aantal ontbrekende updates in de categorie computers. Klik op een computer in een logboek zoeken retourneren van dat alle records voor die computer bijwerken uitvoeren.|  
<br><br> ![Overzicht van de resultaten van de implementatie van Update](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Een Update-installatie maken

Maak een nieuwe implementatie door te klikken op de knop **toevoegen** aan de bovenkant van het scherm om de **Implementatie van de nieuwe** pagina te openen.  U moet waarden opgeven voor de eigenschappen in de volgende tabel.

Eigenschap | Beschrijving|
----------|----------|
Naam | Unieke naam voor de implementatie.|
Tijdzone | De tijdzone voor de begintijd.|
Begintijd | Datum en tijd de implementatie start.|
Duur | Aantal minuten dat de implementatie van de Update mag worden uitgevoerd.  Als binnen deze duur niet alle updates zijn geïnstalleerd, moeten de resterende updates wachten tot de volgende Update-implementatie.|
Computers | Namen van computers of computergroepen wilt opnemen in de installatie van de Update.  Selecteer een of meer posten uit de vervolgkeuzelijst.|
<br><br> ![Nieuwe Update distributie pagina](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Tijdsbereik

Het bereik van de gegevens geanalyseerd in de oplossing voor het beheer van updates is standaard van alle verbonden beheer groepen binnen de laatste dag 1 gegenereerd. 

Als u het tijdsbereik van de gegevens, selecteert u **gegevens op basis van** de bovenkant van het dashboard. U kunt records gemaakt of bijgewerkt in de laatste 7 dagen, 1 dag of 6 uur. Of selecteer **aangepast** en geef een aangepast datumbereik.<br><br> ![De optie aangepaste periode](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Logboekrecords Analytics

De Update-oplossing voor twee typen records in de opslagplaats OMS gemaakt.

### <a name="update-records"></a>Records bijwerken

Een record van het type **Update** wordt gemaakt voor elke update die wordt geïnstalleerd of op elke computer nodig. Bijwerkrecords, hebben de eigenschappen in de volgende tabel.

Eigenschap | Beschrijving|
----------|----------|
Type | *Update*|
SourceSystem | De bron die de installatie van de update wordt goedgekeurd.<br>Mogelijke waarden zijn:<br>-Microsoft Update<br>-Windows Update<br>-SCCM<br>-Linux Servers (die is opgehaald uit het pakket Managers)|
Goedgekeurd | Hiermee geeft u aan of de update is goedgekeurd voor installatie.<br> Voor Linux-servers is dit momenteel optioneel als patches is niet worden beheerd met OMS.|
Indeling voor Windows | Classificatie van de update.<br>Mogelijke waarden zijn:<br>-Toepassingen<br>-Essentiële Updates<br>-Definitie Updates<br>-Feature Packs<br>-Beveiligingsupdates<br>-Service Packs<br>-Updatepakketten<br>-Updates|
Classificatie voor Linux | Cassification van de update.<br>Mogelijke waarden zijn:<br>-Essentiële Updates<br>-Beveiligingsupdates<br>-Andere Updates|
Computer | De naam van de computer.|
InstallTimeAvailable | Geeft aan of de installatietijd van andere stoffen die de update geïnstalleerd.|
InstallTimePredictionSeconds | Van geschatte installatietijd in seconden op basis van andere stoffen die de update geïnstalleerd.|
KBID | De ID van het KB-artikel dat de update wordt beschreven.|
ManagementGroupName | De naam van de groep management voor SCOM agenten.  Dit is voor andere functionarissen, AOI -<workspace ID>.|
MSRCBulletinID | De ID van de beschrijving van de update van Microsoft-beveiligingsbulletin.|
MSRCSeverity | De ernst van het Microsoft security bulletin.<br>Mogelijke waarden zijn:<br>-Kritiek<br>-Belangrijk<br>-Matig|
Optioneel | Hiermee geeft u aan of de update optioneel is.|
Product | De naam van het product voor de update is.  Klik op **weergeven** om het artikel te openen in een browser.|
PackageSeverity | De ernst van het probleem is opgelost in deze update, zoals gerapporteerd door de leveranciers van Linux distro. | 
PublishDate | Datum en tijd waarop de update is geïnstalleerd.|
RebootBehavior | Hiermee geeft u aan als de update zorgt ervoor dat het opnieuw opstarten.<br>Mogelijke waarden zijn:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | Het revisienummer van de update.|
SourceComputerId | GUID ter identificatie van de computer.|
TimeGenerated | Datum en tijd waarop de record voor het laatst is bijgewerkt.|
Titel | De titel van de update.|
Update-id | De GUID als unieke identificatie voor de update.|
UpdateState | Hiermee geeft u aan of de update is geïnstalleerd op deze computer.<br>Mogelijke waarden zijn:<br>-Geïnstalleerd - is de update geïnstalleerd op deze computer.<br>-Nodig - de update niet is geïnstalleerd en op deze computer nodig is.|  

<br>
Wanneer u een zoekfunctie geeft als resultaat records met een soort **Update** , kunt u de **Updates** weergeven waarin een aantal tegels samenvatten van de updates die worden geretourneerd door de zoekactie uitvoert. U kunt klikken op de items in de **toegepaste updates en ontbrekende** en tegels van **verplichte en optionele updates** om het bereik van de weergave voor die reeks updates. Selecteer in de **lijst** , **tabel** of weergave de afzonderlijke records.<br> 

![Weergave van logboek-zoeken met Type Record bijwerken](./media/oms-solution-update-management/update-la-view-updates.png)  

In **de tabelweergave** kunt u klikken op **KBID** voor elke record een browser openen met het KB-artikel. Hiermee kunt u snel informatie lezen over de details van een bepaalde update.<br> 

![Logboek zoeken tabelweergave met tegels recordtype Updates](./media/oms-solution-update-management/update-la-view-table.png)

Klik op de link **weergeven** naast de KBID het KB-artikel openen in de weergave **lijst** .<br>

![Logboek zoeken lijst bekijken met tegels recordtype Updates](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary records

Een record met een soort **UpdateSummary** wordt gemaakt voor elke Windows-agentcomputer. Deze record wordt bijgewerkt telkens wanneer die de computer wordt gescand op updates. **UpdateSummary** records bevatten de eigenschappen in de volgende tabel.

Eigenschap | Beschrijving|
----------|----------|
Type | UpdateSummary|
SourceSystem | OpsManager |
Computer | De naam van de computer.|
CriticalUpdatesMissing | Aantal essentiële updates op de computer ontbreekt.|
ManagementGroupName | De naam van de groep management voor SCOM agenten. Dit is voor andere functionarissen, AOI -<workspace ID>.|
NETRuntimeVersion | De versie van .NET runtime op de computer geïnstalleerd.|
OldestMissingSecurityUpdateBucket | Emmer voor het categoriseren van de tijd sinds de oudste ontbrekende updates op deze computer is gepubliceerd.<br>Mogelijke waarden zijn:<br>-Oudere<br>-180 dagen geleden<br>-150 dagen geleden<br>-120 dagen geleden<br>-90 dagen geleden<br>-60 dagen geleden<br>-go 30 dagen<br>-Recente|
OldestMissingSecurityUpdateInDays | Aantal dagen sinds de oudste ontbrekende updates op deze computer is gepubliceerd.|
Versie_besturing | De versie van het besturingssysteem op de computer geïnstalleerd.|
OtherUpdatesMissing | Aantal andere updates op de computer ontbreekt.|
SecurityUpdatesMissing | Aantal beveiligingsupdates op de computer ontbreekt.|
SourceComputerId | GUID ter identificatie van de computer.|
TimeGenerated | Datum en tijd waarop de record voor het laatst is bijgewerkt.|
TotalUpdatesMissing |Totaal aantal updates op de computer ontbreekt.|
WindowsUpdateAgentVersion | Het versienummer van Windows Update agent op de computer.|
WindowsUpdateSetting | Instelling voor hoe de computer belangrijke updates automatisch geïnstalleerd.<br>Mogelijke waarden zijn:<br>-Uitgeschakeld<br>-Waarschuwen voordat de installatie<br>-Geplande installaties|
WSUSServer | URL van de WSUS-server als de computer is geconfigureerd voor het gebruik van een.|  

## <a name="sample-log-searches"></a>Monster logboek zoeken

De volgende tabel bevat een voorbeeld logboek zoekt u bijwerkrecords door deze oplossing wordt verzameld. 

Query | Beschrijving|
----------|----------|
Alle computers met ontbrekende updates | Type = Update UpdateState = optioneel nodig = false & #124; Selecteer de Computer, titel, KBID, classificatie, UpdateSeverity, PublishedDate|
Ontbrekende updates voor de computer 'COMPUTER01.contoso.com' (vervangen door de naam van uw eigen computer) | Type = Update UpdateState = optioneel nodig = false Computer = "COMPUTER01.contoso.com" & #124; Selecteer de Computer, titel, KBID, Product, UpdateSeverity, PublishedDate|
Alle computers met ontbrekende kritieke of beveiligingsupdates | Type = Update UpdateState = optioneel nodig = false (classificatie = 'Security Updates' of classificatie = 'Essentiële Updates')|
Kritische of security updates die nodig is voor machines waar updates handmatig worden toegepast | Type = Update UpdateState = optioneel nodig = false (classificatie = 'Security Updates' of de indeling = 'Essentiële Updates') IN Computer {Type = UpdateSummary WindowsUpdateSetting = handmatige & #124; Afzonderlijke Computer} & #124; Afzonderlijke KBID|
Foutgebeurtenissen voor machines met ontbrekende kritieke of security updates vereist | Type gebeurtenis EventLevelName = = fout IN Computer {Type = Update (classificatie = 'Security Updates' of classificatie = 'Essentiële Updates') UpdateState = optioneel nodig = false & #124; Afzonderlijke Computer}|
Alle computers met updatepakketten ontbreekt | Type = optionele Update = false classificatie = "Updatepakketten" UpdateState = nodig & #124; Selecteer de Computer, titel, KBID, classificatie, UpdateSeverity, PublishedDate|
Verschillende ontbrekende updates op alle computers | Type = Update UpdateState = optioneel nodig = false & #124; Duidelijke titel|
Lidmaatschap van de WSUS-computer | Type = UpdateSummary & #124; maatregel count() door WSUSServer|
Configuratie automatisch bijwerken | Type = UpdateSummary & #124; maatregel count() door WindowsUpdateSetting|
Computers met automatische updates is uitgeschakeld | Type = UpdateSummary WindowsUpdateSetting = handmatige|  
Lijst van alle Linux-computers die met een pakket is een update beschikbaar | Type = Update en waardereeksen = Linux en UpdateState! = 'Niet nodig' & #124; count() door Computer meten|
Lijst van alle Linux-computers die met een pakket is een update beschikbaar die kritiek of beveiliging beveiligingslek wordt gedicht | Type = Update en waardereeksen = Linux en UpdateState! = 'Niet nodig' en (classificatie = 'Essentiële Updates' of classificatie = "Beveiligingsupdates") & #124; count() door Computer meten|
Lijst van alle pakketten met een update beschikbaar | Type = Update en waardereeksen = Linux en UpdateState! = 'Niet nodig'|
Lijst van alle pakketten met een update beschikbaar die kritiek of beveiliging beveiligingslek wordt gedicht | Type = Update en waardereeksen = Linux en UpdateState! = 'Niet nodig' en (classificatie = 'Essentiële Updates' of classificatie = "Beveiligingsupdates")|
Lijst van de "Ubuntu" machines met een update beschikbaar | Type = Update en waardereeksen = Linux en OSName = Ubuntu & #124; count() door Computer meten|

## <a name="next-steps"></a>Volgende stappen

- Logboek-zoekopdrachten gebruiken in [Logboek Analytics](../log-analytics/log-analytics-log-searches.md) gedetailleerde updategegevens weergeven.

- [Maak uw eigen dashboards](../log-analytics/log-analytics-dashboards.md) tonen virusupdates voor uw beheerde computers.

- [Waarschuwingen maken](../log-analytics/log-analytics-alerts.md) wanneer essentiële updates worden vastgesteld als ontbrekend van computers of een computer met is automatische updates uitgeschakeld.  




