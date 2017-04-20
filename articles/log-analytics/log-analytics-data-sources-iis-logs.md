<properties
   pageTitle="IIS vastgelegd in het logboek Analytics | Microsoft Azure"
   description="Internet Information Services (IIS) gebruikersactiviteiten in logboekbestanden die kunnen worden verzameld door Analytics logboek opgeslagen.  In dit artikel wordt beschreven hoe collectie van IIS-logboeken en de details van de records die zij in de opslagplaats OMS maken configureren."
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

# <a name="iis-logs-in-log-analytics"></a>IIS vastgelegd in het logboek Analytics
Internet Information Services (IIS) gebruikersactiviteiten in logboekbestanden die kunnen worden verzameld door Analytics logboek opgeslagen.  

![IIS-logboeken](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Configureren van IIS-logboeken
Logboek voor Analytics worden gegevens verzameld uit de logboekbestanden die door IIS, dus u [IIS voor logboekregistratie configureren moet](https://technet.microsoft.com/library/hh831775.aspx).

Logboek Analytics IIS-logboekbestanden opgeslagen in de W3C-indeling ondersteunt alleen en biedt geen ondersteuning voor aangepaste velden of IIS-geavanceerde registratie.  
Logboek Analytics verzamelt geen logboeken in NCSA of IIS de oorspronkelijke indeling.

IIS-logboeken configureren in logboek Analytics in het [menu Data in Analytics logboekinstellingen](log-analytics-data-sources.md#configuring-data-sources).  Er is geen configuratie vereist dan **IIS verzamelen W3C-indeling van logboekbestanden**selecteren.

Wij raden u aan wanneer u IIS logboek collectie inschakelt, u het IIS logboek rollover instelling op elke server configureren moet.


## <a name="data-collection"></a>Het verzamelen van gegevens

Logboek Analytics verzamelt logboekvermeldingen van IIS uit elke bron verbonden ongeveer elke 15 minuten.  De agent worden geregistreerd zijn plaats in elk logboek wordt verzameld uit.  Als de agent off line gaat, vervolgens verzamelt logboek Analytics gebeurtenissen van waar het laatst gestopt, zelfs als de gebeurtenissen die zijn gemaakt terwijl de agent off line was.


## <a name="iis-log-record-properties"></a>Eigenschappen van IIS record

Logboekrecords IIS een soort **W3CIISLog** en hebben de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--|:--|
| Computer | De naam van de computer waarop de gebeurtenis is verzameld uit. |
| Overschrijving | IP-adres van de client. |
| csMethod | De methode van de aanvraag, zoals GET of POST. |
| csReferer | De site is dat de gebruiker een koppeling tussen de huidige site heeft gevolgd. |
| csUserAgent | Het browsertype van de client. |
| csUserName | De naam van de geverifieerde gebruiker die toegang de server tot. Anonieme gebruikers worden aangegeven met een afbreekstreepje. |
| csUriStem | Het doel van de aanvraag, zoals een webpagina. |
| csUriQuery | Query, indien aanwezig, die de client heeft geprobeerd uit te voeren. |
| ManagementGroupName | De naam van de groep management voor agenten Operations Manager.  Voor andere functionarissen, is dit AOI -\<-ID voor werkruimte\> |
| RemoteIPCountry | Land van het IP-adres van de client. |
| RemoteIPLatitude | De geografische breedte van het IP-adres van de client. |
| RemoteIPLongitude | De lengte van het IP-adres van de client. |
| scStatus | HTTP-statuscode. |
| scSubStatus | Substatusfoutcode. |
| scWin32Status | Windows-statuscode. |
| sIP | IP-adres van de webserver. |
| SourceSystem  | OpsMgr |
| sPort | Poort op de server de client is verbonden. |
| sSiteName | De naam van de IIS-site. |
| TimeGenerated | De datum en het tijdstip waarop die de post is vastgelegd. |
| TimeTaken | De lengte van de tijd voor verwerking van de aanvraag in milliseconden. |

## <a name="log-searches-with-iis-logs"></a>Gezocht met de IIS-logboeken logboek

De volgende tabel bevat voorbeelden van logboek-query's die IIS logboekrecords ophalen.

| Query | Beschrijving |
|:--|:--|
| Type = IISLog | Records voor alle IIS-logboek. |
| Type = IISLog EventLevelName = fout | Alle Windows-gebeurtenissen met de ernst van de fout. |
| Type = W3CIISLog & #124; Count() meten per overschrijving | Telling van IIS logboekvermeldingen van client-IP-adres. |
| Type = W3CIISLog csHost = "www.contoso.com" & #124; Count() meten door csUriStem | Telling van IIS logboekvermeldingen door de URL voor de host www.contoso.com. |
| Type = W3CIISLog & #124; De Sum(csBytes) meten door Computer & #124; Top 500000| Totaal aantal bytes dat is ontvangen door de IIS-computer. |

## <a name="next-steps"></a>Volgende stappen

- Configureer logboek Analytics voor het verzamelen van andere [gegevensbronnen](log-analytics-data-sources.md) voor analyse.
- Informatie over het [logboek zoeken](log-analytics-log-searches.md) voor het analyseren van de gegevens die worden verzameld uit gegevensbronnen en oplossingen.
- Waarschuwingen in logboek Analytics te informeren over belangrijke voorwaarden gevonden in de IIS-logboeken configureren.
