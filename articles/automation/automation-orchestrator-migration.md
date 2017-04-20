<properties
   pageTitle="Migratie van Orchestrator naar Azure automatisering | Microsoft Azure"
   description="Beschrijving van het runbooks en integratie packs System Center Orchestrator migreren naar Azure automatisering."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migratie van Orchestrator naar Azure automatisering (bèta)

Runbooks in [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) zijn gebaseerd op de activiteiten van integratie packs die speciaal voor Orchestrator geschreven zijn terwijl runbooks in Azure automatisering zijn gebaseerd op Windows PowerShell.  [Grafische runbooks](automation-runbook-types.md#graphical-runbooks) in Azure automatisering hebben een vergelijkbare weergave voor Orchestrator runbooks met hun activiteiten die de PowerShell cmdlets onderliggende runbooks en activa.

De [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) bevat hulpprogramma's om u te helpen omzetten in runbooks van Orchestrator Azure automatisering.  Naast het converteren van de runbooks zelf, moet u de packs integratie met de activiteiten die gebruikmaken van de runbooks converteren naar de integratiemodules met Windows PowerShell-cmdlets.  

Hieronder ziet u het basisproces voor het Orchestrator runbooks converteren naar Azure automatisering.  Elk van deze stappen in de volgende secties gedetailleerd beschreven.

1.  Downloaden van de [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) bevat de hulpprogramma's en modules die in dit artikel besproken.
2.  [Standaard activiteiten Module](#standard-activities-module) in Azure automatisering importeren.  Dit omvat de geconverteerde versie van de standaard Orchestrator activiteiten die kunnen worden gebruikt door de geconverteerde runbooks.
3.  [System Center Orchestrator integratiemodules](#system-center-orchestrator-integration-modules) importeren in Azure automatisering voor deze integratie packs die wordt gebruikt door de runbooks die toegang hebben tot System Center.
4.  Aangepast en derden packs voor integratie met het [Conversieprogramma voor integratie Pack](#integration-pack-converter) omzetten en importeren in Azure automatisering.
5.  Orchestrator runbooks met het [Runbook Converter](#runbook-converter) converteren en installeren in Azure automatisering.
6.  Maak handmatig vereist Orchestrator activa Azure automatisering omdat het conversieprogramma Runbook deze bronnen niet geconverteerd.
7.  Configureer een [Hybride Runbook werknemer](#hybrid-runbook-worker) in uw lokale Datacenter geconverteerde runbooks die toegang lokale bronnen tot uitvoeren.

## <a name="service-management-automation"></a>Service Management automatisering

[Service Management automatisering](http://technet.microsoft.com/library/dn469260.aspx) (SMA) worden opgeslagen en wordt runbooks uitgevoerd in uw lokale Datacenter zoals Orchestrator en van dezelfde integratiemodules als Azure automatisering wordt gebruikt. De [Converter van Runbook](#runbook-converter) omgezet in Orchestrator runbooks grafische runbooks maar die niet worden ondersteund in SMA.  U kunt nog steeds de [Standaardmodule activiteiten](#standard-activities-module) en [System Center Orchestrator integratiemodules](#system-center-orchestrator-integration-modules) in SMA installeren, maar u moet handmatig [de runbooks te herschrijven](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybride Runbook werknemer

Runbooks in Orchestrator worden opgeslagen op een databaseserver en worden uitgevoerd op servers van runbook, zowel in het midden van uw lokale gegevens.  Runbooks in Azure automatisering in de Azure cloud worden opgeslagen en kunnen worden uitgevoerd in uw lokale datacenter met behulp van een [Werknemer van hybride Runbook](automation-hybrid-runbook-worker.md).  Dit is hoe u meestal runbooks geconverteerd van Orchestrator, omdat ze zijn ontworpen voor lokale servers wordt uitgevoerd.

## <a name="integration-pack-converter"></a>Integration Pack Converter

De Integration Pack Converter converteert integratie packs die zijn gemaakt met de [Orchestrator Integration Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) op basis van Windows PowerShell die kunnen worden geïmporteerd in Azure automatisering of Service Management automatisering integratie-modules.  

Als u het conversieprogramma Integration Pack uitvoert, verschijnt er met een wizard waarmee u een integratie pack (.oip)-bestand te selecteren.  De wizard geeft een overzicht van de activiteiten die in dat pakket integratie vervolgens en u kunt selecteren die wordt gemigreerd.  Wanneer u de wizard hebt voltooid, wordt een integratiemodule met een bijbehorende cmdlet voor elk van de activiteiten in het oorspronkelijke integration pack gemaakt.


### <a name="parameters"></a>Parameters

De eigenschappen van een activiteit in het integratie-pack worden geconverteerd naar de parameters van de overeenkomstige cmdlet in de integratiemodule.  Windows PowerShell-cmdlets hebben [gemeenschappelijke parameters](http://technet.microsoft.com/library/hh847884.aspx) die kunnen worden gebruikt met alle cmdlets.  Bijvoorbeeld de uitgebreide parameter - zorgt ervoor dat een cmdlet voor uitvoer van gedetailleerde informatie over de werking ervan.  Geen cmdlet kan een parameter met dezelfde naam als een parameter gemeenschappelijk hebben.  Als een activiteit een eigenschap met dezelfde naam als een algemene parameter heeft, vraagt de wizard u een andere naam opgeven voor de parameter.

### <a name="monitor-activities"></a>Activiteiten controleren

Monitor runbooks in Orchestrator beginnen met een [activiteiten controleren](http://technet.microsoft.com/library/hh403827.aspx) en voortdurend wachten om te worden aangeroepen door een bepaalde gebeurtenis worden uitgevoerd.  Azure automatisering ondersteunt geen monitor runbooks, zodat alle activiteiten van de monitor in het integratie-pack worden niet geconverteerd.  In plaats daarvan wordt een tijdelijke aanduiding cmdlet gemaakt in de integratiemodule voor de activiteit van de monitor.  Deze cmdlet heeft geen functies, maar u kunt een geconverteerde runbook die wordt gebruikt om te worden geïnstalleerd.  Deze runbook kan niet worden uitgevoerd in Azure automatisering, maar kan worden geïnstalleerd, zodat u deze kunt wijzigen.

### <a name="integration-packs-that-cannot-be-converted"></a>Integratie van pakketten die niet kunnen worden geconverteerd

Integratie van pakketten die niet zijn gemaakt met OIT kunnen niet worden geconverteerd met het conversieprogramma Integration Pack. Er zijn ook sommige integratie van pakketten die met dit hulpprogramma Microsoft die op dit moment kan niet worden geconverteerd.  Geconverteerde versies van deze integratie packs zijn [beschikbaar voor download](#system-center-orchestrator-integration-modules) , zodat ze kunnen worden geïnstalleerd in Azure automatisering of Service Management automatisering.


## <a name="standard-activities-module"></a>Standaard activiteiten Module

Orchestrator bevat een set van [standaard activiteiten](http://technet.microsoft.com/library/hh403832.aspx) die niet zijn opgenomen in een pack integratie maar door veel runbooks worden gebruikt.  De standaard activiteiten module is een integratiemodule met een cmdlet equivalent voor elk van deze activiteiten.  Deze integratiemodule moet in Azure automatisering u installeren voordat u een geconverteerde runbooks met een standaard-activiteit.

Geconverteerde runbooks ondersteunt, kan de cmdlets in de module standaard activiteiten worden gebruikt door iemand die bekend zijn met Orchestrator maken van nieuwe runbooks in Azure automatisering.  Terwijl de functionaliteit van de standaard activiteiten met cmdlets kan worden uitgevoerd, kunnen ze anders werken.  De cmdlets in de module geconverteerde standaard activiteiten zal werken op dezelfde manier als hun overeenkomstige activiteiten en dezelfde parameters gebruiken.  Hierdoor kan de auteur van de bestaande Orchestrator runbook in hun overgang naar Azure automatisering runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator integratiemodules

Microsoft biedt [integratie packs](http://technet.microsoft.com/library/hh295851.aspx) voor het bouwen van runbooks voor het automatiseren van System Center-onderdelen en andere producten.  Sommige van deze integratie packs momenteel zijn gebaseerd op OIT maar momenteel kan niet worden geconverteerd naar integratiemodules door bekende problemen.  [System Center Orchestrator integratiemodules](https://www.microsoft.com/download/details.aspx?id=49555) bevat geconverteerde versies van deze integratie packs die kunnen worden geïmporteerd in Azure automatisering en Service Management-automatisering.  

Bijgewerkte versies van de integratie van verpakkingen op basis van OIT die kan worden omgezet met het conversieprogramma Integration Pack gepubliceerd door de RTM-versie van dit hulpprogramma.  Ook instructies krijgt om u te helpen met de activiteiten van de integratie-pakketten niet op basis van OIT runbooks voor het converteren.

## <a name="runbook-converter"></a>Runbook-Converter

Het conversieprogramma Runbook zet Orchestrator runbooks in [grafische runbooks](automation-runbook-types.md#graph-runbooks) die kunnen worden geïmporteerd in Azure automatisering.  

Runbook Converter is geïmplementeerd als een PowerShell-module met een cmdlet genoemd **ConverterenVan SCORunbook** , die de conversie uitvoert.  Wanneer u het programma installeert, wordt er een snelkoppeling naar een PowerShell-sessie die wordt geladen de cmdlet gemaakt.   

Hieronder ziet u het basisproces waarmee u een runbook Orchestrator omzetten en importeren in Azure automatisering.  In de volgende secties bieden verdere details over het gebruik van het hulpprogramma en werken met geconverteerde runbooks.

1. Een of meer runbooks van Orchestrator exporteren.
2. Integratiemodules voor alle activiteiten in de runbook verkrijgen.
3. De runbooks Orchestrator in het geëxporteerde bestand converteren.
4. Bekijk de informatie in de logboeken voor het valideren van de conversie en eventuele vereiste taken handmatig bepalen.
4. Geconverteerde runbooks importeren in Azure automatisering.
5. Maak alle vereiste elementen in Azure automatisering.
6. De runbook in Azure automatisering voor het wijzigen van de vereiste activiteiten bewerken.

### <a name="using-runbook-converter"></a>Met behulp van Runbook-Converter

De syntaxis voor **ConverterenVan-SCORunbook** is als volgt:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - pad naar het exportbestand met het runbooks om te zetten.
- Module - door komma's gescheiden lijst met integratiemodules met activiteiten in de runbooks.
- OutputFolder - pad naar de map voor het maken van geconverteerd grafische runbooks. 


Met de volgende opdracht converteert de runbooks in een exportbestand **MyRunbooks.ois_export**genoemd.  Deze runbooks gebruiken voor de integratie van Active Directory en Data Protection Manager packs.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Logboekbestanden

Het conversieprogramma voor Runbook maakt de volgende logboekbestanden in dezelfde locatie als de geconverteerde runbook.  Als de bestanden reeds bestaan, wordt deze overschreven met de gegevens van de laatste conversie.

| Bestand | Inhoud |
|:---|:---|
| Runbook Converter - Progress.log | Gedetailleerde stappen van de conversie met inbegrip van informatie voor elke activiteit die is geconverteerd en een waarschuwing voor elke activiteit niet geconverteerd. |
| Runbook Converter - Summary.log  | Overzicht van de laatste conversie met inbegrip van eventuele waarschuwingen en taken die u uitvoeren moet zoals het maken van een variabele die is vereist voor de geconverteerde runbook te volgen.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Exporteren van runbooks van Orchestrator

De Runbook-Converter werkt met een exportbestand van Orchestrator met een of meer runbooks.  Wordt er een bijbehorende Azure automatisering runbook voor elke Orchestrator runbook gemaakt in het exportbestand.  

Een runbook van Orchestrator exporteren, klik met de rechtermuisknop op de naam van de runbook in Runbook Designer en selecteer **exporteren**.  Aan alle runbooks in een map exporteren, klik met de rechtermuisknop op de naam van de map en selecteer **exporteren**.


### <a name="runbook-activities"></a>Runbook activiteiten

Elke activiteit in de runbook Orchestrator converteert de Runbook Converter naar een bijbehorende activiteit in Azure automatisering.  Voor de activiteiten die niet kunnen worden geconverteerd, wordt de activiteit van een tijdelijke aanduiding in de runbook met de waarschuwingstekst gemaakt.  Nadat u de geconverteerde runbook in Azure automatisering importeert, moet u een van deze activiteiten vervangen door geldige activiteiten die de vereiste functionaliteit worden uitgevoerd.

Activiteiten in de [Standaardmodule activiteiten](#standard-activities-module) Orchestrator worden geconverteerd.  Er zijn enkele standaard Orchestrator activiteiten die niet in deze module wel en niet worden geconverteerd.  **Platformgebeurtenis Send** heeft bijvoorbeeld geen equivalent Azure automatisering omdat de gebeurtenis specifiek voor Orchestrator is.

[Monitor activiteiten](https://technet.microsoft.com/library/hh403827.aspx) worden niet geconverteerd, omdat er geen equivalent voor hen in Azure automatisering.  De uitzondering zijn activiteiten in [geconverteerd integratie packs](#integration-pack-converter) die worden geconverteerd naar de tijdelijke aanduiding activiteit monitor.

Als u het pad naar de integratiemodule met de parameter **modules** bieden worden enige activiteit van een [integration pack geconverteerd](#integration-pack-converter) geconverteerd.  System Center integratie Packs kunt u de [System Center Orchestrator integratiemodules](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator resources

De Runbook-Converter converteert alleen runbooks, geen andere bronnen zoals tellers, variabelen of verbindingen Orchestrator.  Items worden niet ondersteund in Azure automatisering.  Variabelen en verbindingen worden ondersteund, maar moet u ze handmatig maken.  De logboekbestanden wordt u op de hoogte als de runbook is vereist voor deze middelen en bijbehorende resources die u maken in Azure automatisering voor de geconverteerde runbook wilt werking opgeven.

Een runbook kan bijvoorbeeld een variabele gebruikt voor het vullen van een bepaalde waarde in een activiteit.  De geconverteerde runbook wordt de activiteit converteren en geeft u een variabele actief in Azure automatisering met dezelfde naam als de variabele Orchestrator.  Dit zijn vastgelegd in de **Runbook Converter - Summary.log** -bestand dat is gemaakt na de conversie.  U moet handmatig deze variabele activa in Azure automatisering te maken voordat u de runbook. 

 
### <a name="input-parameters"></a>Invoerparameters

Runbooks in Orchestrator accepteren invoerparameters activiteit van de **Gegevens worden geïnitialiseerd** .  Als de runbook wordt geconverteerd, deze activiteit bevat, wordt voor elke parameter in de activiteit een [invoerparameter](automation-graphical-authoring-intro.md#runbook-input-and-output) in de automatisering van Azure runbook gemaakt.  Een [besturingselement Script Workflow](automation-graphical-authoring-intro.md#activities) activiteit wordt gemaakt in de geconverteerde runbook die worden opgehaald en elke parameter als resultaat gegeven.  Alle activiteiten in de runbook die gebruikmaken van een invoerparameter verwijst naar de uitvoer van deze activiteit.

De reden dat deze strategie wordt gebruikt is het beste een mirror van de functionaliteit in het runbook Orchestrator.  Activiteiten in de nieuwe grafische runbooks moeten verwijzen naar direct input parameters met behulp van een bron van Runbook ingevoerde gegevens.


### <a name="invoke-runbook-activity"></a>Activiteit Runbook aanroepen

Andere runbooks beginnen Runbooks in Orchestrator met het **Aanroepen van Runbook** -activiteit. Als de runbook wordt geconverteerd, deze activiteit bevat en de optie **wachten op voltooiing** is ingesteld, vervolgens een runbook activiteit wordt gemaakt voor het in de geconverteerde runbook.  Als de optie **wachten op voltooiing** niet is ingesteld, wordt een werkstroom Script activiteit gemaakt die gebruikmaakt van **Start AzureAutomationRunbook** Start de runbook.  Nadat u de geconverteerde runbook in Azure automatisering importeert, moet u deze activiteit met de informatie die is opgegeven in de activiteit wijzigen.




## <a name="related-articles"></a>Verwante artikelen

- [System Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Service Management automatisering](https://technet.microsoft.com/library/dn469260.aspx)
- [Hybride Runbook werknemer](automation-hybrid-runbook-worker.md)
- [Standaard orchestrator-activiteiten](http://technet.microsoft.com/library/hh403832.aspx)
- [Download System Center Orchestrator Migration Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
