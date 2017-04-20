<properties
   pageTitle="Integratie met Operations Management Suite (OMS) | Microsoft Azure"
   description="Standaardfuncties van OMS gebruiken, kunt u integreren met andere toepassingen en services voor het leveren van een hybride omgeving, beheer van aangepaste scenario's die uniek zijn voor uw omgeving, of om een aangepaste management ervaring voor uw klanten.  Dit artikel biedt een overzicht van de verschillende opties voor het integreren met OMS en koppelingen naar artikelen met gedetailleerde technische informatie."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Integratie met Operations Management Suite (OMS)

Operations Management Suite is Microsoft van cloud-gebaseerde IT-oplossing waarmee u beheren kunt en beveiligen van uw locatie op en cloud infrastructuur.  Standaardfuncties van OMS gebruiken, kunt u integreren met andere toepassingen en services voor het leveren van een hybride omgeving, beheer van aangepaste scenario's die uniek zijn voor uw omgeving, of om een aangepaste management ervaring voor uw klanten.  Dit artikel biedt een overzicht van de verschillende opties voor het integreren met OMS services en koppelingen naar artikelen met gedetailleerde technische informatie. 



## <a name="log-analytics"></a>Logboek Analytics
Gegevens verzameld door Analytics logboek is opgeslagen in een opslagruimte die wordt gehost in Azure.  Alle gegevens die zijn opgeslagen in de opslagplaats is beschikbaar in het logboek voor zoekopdrachten die voorzien in zeer grote hoeveelheden gegevens snel analyse.  Mogelijk is uw integratie-eisen aan de opslagplaats te vullen met nieuwe gegevens beschikbaar voor analyse te maken of ophalen van gegevens in de bibliotheek om een nieuwe visualisatie of integreren met een ander beheerprogramma.

Elk gedeelte van de gegevens in de bibliotheek wordt opgeslagen als een record.  Wanneer u de waarden voor de bibliotheek, moet u het recordtype dat u uw oplossing worden gebruikt en een beschrijving van de eigenschappen bieden gebruikers.  Wanneer u gegevens ophaalt, moet u deze informatie over de gegevens waarmee u werkt.

![Vullen van de opslagplaats OMS](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>De opslagplaats logboek Analytics vullen
Er zijn meerdere methoden voor het vullen van de opslagplaats OMS.  De methode die u gebruikt hangt af van factoren als waar de gegevens zich bevindt, de indeling van de gegevens, en die u nodig hebt voor de ondersteuning van clients.  Zodra de gegevens zijn opgeslagen in de bibliotheek, maakt het geen verschil hoe ze werden verzameld.

De volgende secties worden de verschillende opties voor het vullen van de opslagplaats OMS.

#### <a name="connected-sources-and-data-sources"></a>Verbonden bronnen en gegevensbronnen 
Verbonden bronnen zijn de locaties waar de gegevens kunnen worden opgehaald voor de opslagplaats OMS.  Gegevensbronnen en oplossingen op aangesloten bronnen worden uitgevoerd en definieert de specifieke gegevens die worden verzameld.  Als uw toepassing gegevens naar een van deze gegevensbronnen schrijft, kunt vervolgens u deze door het configureren van de gegevensbron.  Bijvoorbeeld, als uw toepassing wordt gemaakt van Syslog gebeurtenissen, kunnen vervolgens zij worden verzameld door de gegevensbron Syslog op een Linux-agent.

- [Gegevensbronnen in logboek Analytics](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Oplossingen

Oplossingen van de mogelijkheden van OMS.  Een oplossing kan gegevens verzamelen van de verbonden gegevensbron of kan deze analyse uitvoeren op records al verzameld in de bibliotheek.  Elke oplossing die Microsoft heeft een afzonderlijk artikel met de details over de gegevens die worden verzameld.

- [Oplossingen in logboek Analytics](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>HTTP-gegevensverzamelaarset API

Logboek Analytics HTTP-gegevens verzamelen API is een REST API waarmee u de JSON-gegevens toevoegen aan de bibliotheek logboek Analytics.  U kunt deze API gebruiken wanneer u een toepassing hebt die geen gegevens via een van de andere gegevensbronnen of oplossingen bieden.  Het kan worden gebruikt voor het vullen van de opslagplaats van alle clients die de API kan worden aangeroepen en niet afhankelijk is van de planning voor het verzamelen van een gegevensbron of een oplossing.

- [Meld u Analytics HTTP gegevensverzamelaarset API](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Gegevens ophalen uit de opslagplaats logboek Analytics

Er zijn meerdere methoden voor het ophalen van gegevens uit de opslagplaats OMS.  U kunt wilt dat gebruikers het ophalen van gegevens met behulp van de console OMS en bieden verschillende soorten visualisaties en -analyse.  U kunt ook de gegevens ophalen uit een extern proces zoals een andere oplossing.

#### <a name="log-searches"></a>Logboek voor zoekopdrachten

Alle gegevens die zijn opgeslagen in de opslagplaats OMS is beschikbaar via het logboek zoeken.  Gebruikers kunnen hun eigen ad hoc analyse uitvoeren in de console OMS of een dashboard maken met een visualisatie voor een bepaald logboek zoeken.  Oplossingen kunnen aangepaste weergaven bevatten met visualisaties op basis van vooraf gedefinieerde zoekopdrachten.  Kunt u het logboek Zoek-API toegang heeft tot gegevens in de opslagplaats OMS vanuit een externe toepassing of management tool.  

- [Zoekopdrachten in Analytics logboek logboek](../log-analytics/log-analytics-log-searches.md)
- [Logboek Analytics zoekfunctie REST API](../log-analytics/log-analytics-log-search-api.md)
- [Logboek Analytics cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Aangepaste weergaven 
De ontwerpfunctie voor weergaven kunt u aangepaste weergaven maken in de console voor OMS die gebruikers visualisatie en analyse van de gegevens in de oplossing bieden.  Elke weergave bevat een tegel die wordt weergegeven op de hoofdpagina van de console en een willekeurig aantal visualisatie-onderdelen die zijn gebaseerd op het logboek voor zoekopdrachten die u definieert.
  
- [Ontwerp van logboek Analytics weergeven](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Logboek Analytics kunt automatisch gegevens exporteren uit de opslagplaats OMS in Power BI zodat u van de visualisaties en analyseprogramma's profiteren kunt.  Voert deze exportbewerking volgens een planning zodat de gegevens actueel wordt gehouden. 

- [Logboek voor Analytics-gegevens exporteren naar Power BI](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automatisering

OMS kunt automatiseren van processen om te reageren op verzamelde gegevens of andere beheerfuncties uitvoeren.  Het kan gegevens verzamelen van uw toepassing en plaats deze in de opslagplaats OMS of kan u de correctie van een bekend probleem in reactie op in de opslagplaats gevonden gegevens automatiseren. 

![Automatisering](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Runbooks in Azure automatisering workflows en PowerShell scripts uitvoeren in de cloud Azure.  U kunt deze bronnen in Azure of andere bronnen die toegankelijk is vanuit de cloud beheren.  Runbooks kan ook worden uitgevoerd in een lokale datacenter met hybride Runbook werknemer.  U kunt een runbook starten vanaf de portal Azure of externe processen met behulp van een aantal methoden zoals PowerShell of de Automation-API.

- [Een runbook starten in Azure automatisering](../automation/automation-starting-a-runbook.md)
- [Azure automatisering-cmdlets](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automatisering REST API](https://msdn.microsoft.com/library/mt662285.aspx)
- [.NET-automatisering](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Waarschuwingen

Waarschuwingsregels automatisch logboek zoekopdrachten volgens een schema uitgevoerd.  Als de resultaten voldoen aan bepaalde criteria kan de resulterende waarschuwing een runbook start in Azure automatisering of bel een webhook die met een extern proces kan starten.  Beide van deze antwoorden kunnen details bevatten over de waarschuwing met inbegrip van de gegevens die worden geretourneerd in het logboek zoeken.

- [Waarschuwingen in logboek Analytics](../log-analytics/log-analytics-alerts.md)
- [Logboek Analytics Alert API](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Back-up en herstel van de Site

Azure back-up en herstel van de Site biedt services voor bedrijfsgegevens beschermen en ervoor te zorgen dat de beschikbaarheid van servers en toepassingen.  U kunt gebruikmaken van deze services voor het uitvoeren van dergelijke scenario's als back-up verzorgt voor uw toepassing of een failover van een virtuele machine wordt gestart.

- [Azure Backup-cmdlets](https://msdn.microsoft.com/library/mt619253.aspx)
- [Azure-Site-Recovery REST API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Azure-Site-Recovery-Cmdlets](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Aangepaste oplossingen

U kunt de logica van de integratie verpakken in een aangepaste oplossing voor het uitvoeren van uw werkruimte of in de werkruimte van de klant.  Uw oplossing kan een van de methoden voor de integratie in dit artikel, naast andere bronnen om een scenario volledig beheer bevatten.  De bronnen in de oplossing worden geleverd, dat als de oplossing wordt verwijderd, alle bronnen die zij gemaakt zijn verwijderd uit het Kantoorbeheersysteem werkruimte en Azure-abonnement.

De oplossing kan bijvoorbeeld een runbook automatisering te verzamelen en verwerken van gegevens en vul het logboek Analytics opslagplaats met de API van HTTP-gegevens verzamelen.  U kunt ook een aangepaste weergave die presenteert en analyseert de verzamelde gegevens opnemen.  

- Maken van aangepaste oplossingen (binnenkort beschikbaar)    

## <a name="next-steps"></a>Volgende stappen
- Verwijzen naar de [OMS SDK](operations-management-suite-sdk.md) voor technische informatie over het automatiseren van het Kantoorbeheersysteem.  
