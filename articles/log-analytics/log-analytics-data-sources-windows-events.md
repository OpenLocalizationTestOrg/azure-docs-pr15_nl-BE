<properties 
   pageTitle="Windows-gebeurtenislogboek worden vastgelegd in het logboek Analytics | Microsoft Azure"
   description="Windows-gebeurtenislogboeken zijn een van de meest voorkomende gegevensbronnen door logboek Analytics gebruikt.  In dit artikel wordt beschreven hoe u van gebeurtenislogboeken van Windows-collectie en de details van de records die zij in de opslagplaats OMS maken."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windows-gebeurtenislogboek gegevensbronnen in logboek Analytics

Windows-gebeurtenislogboeken zijn een van de meest gebruikte [gegevensbronnen](log-analytics-data-sources.md) voor agenten voor Windows gebruikt, aangezien dit de methode voor het registreren van gegevens en fouten door de meeste toepassingen.  U kunt gebeurtenissen verzamelen van standaard logboeken zoals systeem- en naast het opgeven van alle aangepaste logboeken gemaakt door toepassingen die u wilt controleren.

![Windows-gebeurtenissen](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Configureren Windows Event logs

Windows-Logboeken in het [menu Data in Analytics instellingen](log-analytics-data-sources.md#configuring-data-sources)configureren.

Logboek Analytics verzamelt alleen gebeurtenissen uit de gebeurtenislogboeken van Windows die zijn opgegeven in de instellingen.  U kunt een nieuw logboek toevoegen door te typen in de naam van het logboek en klikt u op **+**.  Voor elk logboek, worden alleen gebeurtenissen met de geselecteerde severities verzameld.  Controleer de severities voor het logboek dat u wilt verzamelen.  U geen gebeurtenissen filteren als u meer criteria op te geven.

![Configureren van Windows-gebeurtenissen](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Het verzamelen van gegevens

Logboek Analytics verzamelt elke gebeurtenis die overeenkomt met een geselecteerde ernst van een gecontroleerde gebeurtenislogboek als de gebeurtenis is gemaakt.  De agent wordt zijn plaats in elk logboek wordt verzameld uit opgenomen.  Als de agent voor een periode van tijd off line gaat worden vervolgens Analytics logboek gebeurtenissen verzamelen van waar het laatst gestopt, zelfs als de gebeurtenissen die zijn gemaakt terwijl de agent off line was.


## <a name="windows-event-records-properties"></a>Windows-gebeurtenislogboek vastgelegd eigenschappen

Windows gebeurtenisrecords hebben een type **gebeurtenis** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Computer            | De naam van de computer waarop de gebeurtenis is verzameld uit. |
| Culture       | De categorie van de gebeurtenis. |
| EventData           | Alle gegevens in de indeling raw. |
| Gebeurtenis-id             | Het nummer van de gebeurtenis. |
| EventLevel          | De ernst van de gebeurtenis in numerieke vorm. |
| EventLevelName      | De ernst van de gebeurtenis in de tekstvorm. |
| EventLog            | De naam van het logboek van de gebeurtenis verzamelde. |
| ParameterXml        | Gebeurtenis parameterwaarden in XML-indeling. |
| ManagementGroupName | De naam van de groep management voor SCOM agenten.  Dit is van andere agents, AOI-<workspace ID> |
| RenderedDescription | Beschrijving van de gebeurtenis met parameterwaarden |
| Bron              | De bron van de gebeurtenis. |
| SourceSystem  | Type agent die van de gebeurtenis werd verzameld. <br> OpsManager – Windows agent, een directe verbinding of SCOM <br> Linux – alle Linux-agenten  <br> AzureStorage – Azure diagnostische gegevens |
| TimeGenerated       | De datum en tijd waarop die de gebeurtenis is gemaakt in Windows. |
| Gebruikersnaam            | De gebruikersnaam van de account die de gebeurtenis heeft vastgelegd. |



## <a name="log-searches-with-windows-events"></a>Logboek zoeken met de Windows-gebeurtenissen

De volgende tabel bevat voorbeelden van het logboek zoeken en ophalen van records van de Windows-gebeurtenislogboek.

| Query | Beschrijving |
|:--|:--|
| Type gebeurtenis = | Alle Windows-gebeurtenissen. |
| Type gebeurtenis EventLevelName = = fout | Alle Windows-gebeurtenissen met de ernst van de fout. |
| Type = gebeurtenis & #124; Maatregel count() per bron | Telling van Windows-gebeurtenissen op bron. |
| Type gebeurtenis EventLevelName = = fout & #124; Maatregel count() per bron | Foutgebeurtenissen op bron telling van Windows. |

## <a name="next-steps"></a>Volgende stappen

- Configureer logboek Analytics voor het verzamelen van andere [gegevensbronnen](log-analytics-data-sources.md) voor analyse.
- Informatie over het [logboek zoeken](log-analytics-log-searches.md) voor het analyseren van de gegevens die worden verzameld uit gegevensbronnen en oplossingen.  
- [Aangepaste velden](log-analytics-custom-fields.md) gebruiken om de vermeldingen worden geparseerd in afzonderlijke velden.
- [Verzameling van prestatiemeteritems](log-analytics-data-sources-performance-counters.md) van uw Windows-agents configureren.