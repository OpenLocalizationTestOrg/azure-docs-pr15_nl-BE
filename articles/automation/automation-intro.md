<properties
    pageTitle="Wat is Azure automatisering | Microsoft Azure"
    description="Welke waarde u automatisering Azure biedt informatie en antwoorden op veelgestelde vragen, zodat u kunt aan de slag bij het maken, met behulp van runbooks en Azure automatisering DSC."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Wat is automatisering, azure automatisering, azure automatisering voorbeelden"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Overzicht van Azure automatisering

Automatisering van Microsoft Azure biedt een manier voor gebruikers voor het automatiseren van handmatige, langdurige, lastige en vaak herhaalde taken die vaak worden uitgevoerd in een wolk en enterprise-omgeving. Dit bespaart u tijd en verhoogt de betrouwbaarheid van de normale administratieve taken en zelfs ingesteld dat deze automatisch met regelmatige tussenpozen worden uitgevoerd. U kunt automatiseren van processen met behulp van runbooks of beheer van de configuratie met de gewenste status configuratie automatiseren. Dit artikel geeft beknopt overzicht van Azure automatisering en antwoorden op enkele veelgestelde vragen. U kunt verwijzen naar andere artikelen in deze bibliotheek voor meer gedetailleerde informatie over de verschillende onderwerpen.


## <a name="automating-processes-with-runbooks"></a>Automatiseren van processen met runbooks

Een runbook is een reeks taken die een geautomatiseerd proces in Azure automatisering uitvoeren. Het is mogelijk een eenvoudig proces, zoals het starten van een virtuele machine en een logboekvermelding te maken of er een complexe runbook die andere kleinere runbooks voor het uitvoeren van een complex proces over meerdere resources of zelfs meerdere wolken en op lokale omgevingen worden gecombineerd.  

Bijvoorbeeld u mogelijk hebt een bestaand handmatig proces voor het afkappen van een SQL-database als het maximum grootte met meerdere stappen nadert, zoals verbinding maken met de server, verbinding maken met de database, ophalen van de huidige grootte van de database, controleren als de drempel heeft overschreden afkappen en kennis van de gebruiker. In plaats van het handmatig uitvoeren van elk van deze stappen, kunt u een runbook die al deze taken als een afzonderlijk proces uitvoeren zou. U start de runbook, de vereiste gegevens, zoals de SQL servernaam, databasenaam en e- en vervolgens rustig terwijl het proces is voltooid. 


## <a name="what-can-runbooks-automate"></a>Wat kunt runbooks automatiseren?

Runbooks in Azure automatisering zijn gebaseerd op Windows PowerShell of de Windows PowerShell-Workflow, zodat ze doen alles wat PowerShell kunt doen. Als een toepassing of service een API heeft, kan vervolgens een runbook er mee werken. Als u een PowerShell-module voor de toepassing hebt, kunt u die module laden in Azure automatisering en de cmdlets opnemen in uw runbook. Azure automatisering runbooks uitgevoerd in de Azure cloud en kan toegang tot cloud resources of externe bronnen die kunnen worden benaderd vanuit de cloud. Met behulp van [Hybride Runbook werknemer](automation-hybrid-runbook-worker.md)kunt runbooks in uw lokale Datacenter beheren lokale bronnen uitvoeren. 


## <a name="getting-runbooks-from-the-community"></a>Runbooks ophalen uit de Gemeenschap

De [Galerie met Runbook](automation-runbook-gallery.md#runbooks-in-runbook-gallery) bevat runbooks van Microsoft en de Gemeenschap dat u gebruiken kunt in uw omgeving ongewijzigd of aanpassen voor uw eigen doeleinden. Ze zijn ook handig voor als verwijzingen naar informatie over het maken van uw eigen runbooks. U kunt zelfs uw eigen runbooks naar de galerie waarin u denkt dat anderen nuttig kunnen bijdragen. 


## <a name="creating-runbooks-with-azure-automation"></a>Runbooks maken met Azure automatisering 

U kunt [uw eigen runbooks maken](automation-creating-importing-runbook.md) nieuwe of runbooks uit de [Galerie met Runbook](http://msdn.microsoft.com/library/azure/dn781422.aspx) voor uw eigen behoeften aanpassen. Er zijn drie verschillende [typen runbook](automation-runbook-types.md) die u kunt kiezen uit op basis van uw vereisten en ervaring met PowerShell. Als u liever werkt rechtstreeks met de PowerShell code, kunt vervolgens u een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) of u off line of met de [tekst editor](http://msdn.microsoft.com/library/azure/dn879137.aspx) in de portal Azure [PowerShell Workflow runbook](automation-runbook-types.md#powershell-workflow-runbooks) . Als u liever een runbook zonder te worden blootgesteld aan de onderliggende code bewerken, kunt u een [grafische runbook](automation-runbook-types.md#graphical-runbooks) met de [grafische editor](automation-graphical-authoring-intro.md) in de Azure portal maken. 

Het beste kijken naar lezen? Bekijk de onderstaande video van Microsoft Ignite-sessie in mei 2015. Opmerking: De basisbegrippen en functies die worden beschreven in deze video zijn juiste dat Azure automatisering veel is gevorderd, aangezien deze video is vastgelegd, het nu heeft een uitgebreidere gebruikersinterface in de portal Azure en extra mogelijkheden ondersteunt.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Beheer van de configuratie met de gewenste status configuratie automatiseren 

[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) is een platform voor gegevensbeheer waarmee u kunt beheren, distribueren en configuratie voor hosts fysieke en virtuele machines met behulp van een declaratieve syntaxis voor PowerShell afdwingen. Op een centrale DSC Pull-Server waarmee de doelcomputers automatisch kunnen ophalen en kunt toepassen, kunt u configuraties definiëren. DSC biedt een set van PowerShell-cmdlets die u gebruiken kunt om configuraties en bronnen te beheren.  

[Azure automatisering DSC](automation-dsc-overview.md) is een cloud-gebaseerde oplossing voor PowerShell DSC waarmee services die vereist zijn voor bedrijfsomgevingen.  U kunt uw resources DSC in Azure automatisering beheren en configuraties van toepassing op virtuele of fysieke machines die een DSC Pull-Server in de cloud Azure ophalen.  Het biedt ook een reporting services die op de hoogte dat van belangrijke gebeurtenissen, zoals wanneer de knooppunten afwijken van de toegewezen configuratie en wanneer een nieuwe configuratie is toegepast. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Maken van uw eigen DSC-configuraties met Azure automatisering

Geef de gewenste status van een knooppunt op [DSC-configuraties](automation-dsc-overview.md#azure-automation-dsc-terms) .  Meerdere knooppunten kunnen toepassen om te garanderen dat ze allemaal een identiek status behouden dezelfde configuratie.  U kunt een configuratie met behulp van een willekeurige tekst editor op uw lokale computer maken en vervolgens importeren in Azure automatisering waar u het kunt compileren en deze knooppunten toe te passen.


## <a name="getting-modules-and-configurations"></a>Modules en configuraties 

U kunt [modules PowerShell](automation-runbook-gallery.md#modules-in-powershell-gallery) cmdlets die u in uw runbooks en configuraties in de [Galerie met PowerShell](http://www.powershellgallery.com/)DSC gebruiken kunt met krijgen. U kunt deze galerie van het portal voor Azure starten en modules rechtstreeks importeren in Azure automatisering of u kunt downloaden en deze handmatig te importeren. U kunt de modules niet rechtstreeks vanaf de portal Azure installeren, maar kunt u deze downloaden installeren als elke andere module. 


## <a name="example-practical-applications-of-azure-automation"></a>Voorbeeld van de praktische toepassingen van Azure automatisering 

Hier volgen een paar voorbeelden van wat de soorten automatiseringsscenario's met Azure automatisering zijn. 

* Maken en kopiëren van virtuele machines in verschillende Azure abonnementen. 
* Schemabestand kopieert van een lokale computer naar een container Azure Blob-opslag. 
* Automatiseren van beveiligingsfuncties, zoals het weigeren van een client vraagt wanneer een denial of service-aanval wordt gedetecteerd. 
* Controleer geconfigureerde beveiligingsbeleid machines voortdurend afgestemd.
* Continue implementatie van toepassingscode beheren via cloud en infrastructuur ruimten. 
* Bouwen van een Active Directory-forest in Azure voor uw testomgeving. 
* Een tabel in een SQL-database afkappen als DB maximumgrootte is bereikt. 
* De instellingen voor een website Azure extern bijwerken. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Hoe het verband tussen Azure automatisering en andere hulpprogramma's voor automatisering

[Service Management automatisering (SMA)](http://technet.microsoft.com/library/dn469260.aspx) is bedoeld voor het automatiseren van taken in de private cloud. Het is lokaal geïnstalleerd in uw datacenter als onderdeel van [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/). SMA en Azure automatisering gebruiken dezelfde runbook indeling op basis van Windows PowerShell en Windows PowerShell-Workflow, maar SMA biedt geen ondersteuning voor [grafische runbooks](automation-graphical-authoring-intro.md).  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) is bedoeld voor de automatisering van de middelen voor gebouwen. Het gebruikt een verschillende runbook dan Azure automatisering en Service Management-automatisering en heeft een grafische interface voor runbooks zonder een script te maken. De runbooks bestaan uit activiteiten van integratie Packs die speciaal voor Orchestrator geschreven zijn. 


## <a name="where-can-i-get-more-information"></a>Waar vind ik meer informatie? 

Een aantal bronnen beschikbaar zijn voor u om meer te weten over Azure automatisering en het maken van uw eigen runbooks. 

* **Azure Automation-bibliotheek** is waar u zich nu. De artikelen in deze bibliotheek biedt volledige documentatie voor de configuratie en het beheer van Azure automatisering en voor het schrijven van uw eigen runbooks. 
* [Azure PowerShell-cmdlets](http://msdn.microsoft.com/library/jj156055.aspx) geeft informatie over het automatiseren van Azure bewerkingen met Windows PowerShell. Runbooks deze cmdlets gebruiken om te werken met Azure bronnen. 
* [Beheer Blog](https://azure.microsoft.com/blog/tag/azure-automation/) biedt de meest recente informatie op Azure automatisering en andere technologieën van Microsoft. U moet zich abonneren op deze blog blijft up-to-date met de nieuwste uit het team Azure automatisering. 
* [Automatisering-Forum](http://go.microsoft.com/fwlink/p/?LinkId=390561) kunt u vragen stellen over Azure automatisering worden aangepakt door Microsoft en de Gemeenschap automatisering. 
* [Azure automatisering Cmdlets](https://msdn.microsoft.com/library/mt244122.aspx) geeft informatie over de beheertaken te automatiseren. Cmdlets voor het beheren van accounts automatiseren, activa, runbooks, DSC bevat.


## <a name="can-i-provide-feedback"></a>Kan ik feedback geven? 

**Geef ons feedback!** Als u naar een oplossing voor automatisering Azure runbook of een integratiemodule zoekt, boekt u een aanvraag voor een Script in het Script Center. Als u feedback of functie aanvragen voor het automatiseren van Azure, kunt u deze op [Gebruiker gesproken](http://feedback.windowsazure.com/forums/34192--general-feedback)boeken. Hartelijk dank! 


