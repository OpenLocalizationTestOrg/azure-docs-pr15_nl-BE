<properties 
   pageTitle="Waarschuwingen in logboek Analytics | Microsoft Azure"
   description="Waarschuwingen in logboek Analytics kunnen belangrijke informatie in uw bibliotheek OMS identificeren en proactief melding van problemen of roepen acties wilt corrigeren.  In dit artikel wordt beschreven hoe een waarschuwingsregel en details maken de verschillende acties die kunnen nemen."
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
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Waarschuwingen in logboek Analytics

Waarschuwingen in logboek Analytics identificeren belangrijke informatie in uw bibliotheek OMS.  Waarschuwingsregels automatisch zoeken en logboek volgens een schema en een record voor een waarschuwing maken als de resultaten voldoen aan bepaalde criteria.  De regel kan een of meer acties om proactief een melding van een waarschuwing of een ander proces starten vervolgens automatisch uitgevoerd.   

![Logboek Analytics waarschuwingen](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Een waarschuwingsregel
Een waarschuwing als regel wilt maken, begint u met het maken van een logboek zoeken naar records die de waarschuwing moet aanroepen.  De knop **Waarschuwing** zijn beschikbaar zodat u kunt maken en configureren van de waarschuwingsregel.

1.  Klik op **Logboek zoeken**vanuit de pagina OMS-overzicht.
2.  Maak een nieuw logboek zoekopdracht of Selecteer een zoekopdracht opgeslagen logboek. 
3.  Klik op **Waarschuwing** aan de bovenkant van de pagina om het scherm **Waarschuwingsregel toevoegen** te openen.
4. Raadpleeg de tabellen hieronder voor meer informatie over de opties voor het configureren van de waarschuwing.
5. Als u het tijdsinterval voor de waarschuwingsregel biedt, is het aantal bestaande records die overeenkomen met de zoekcriteria voor die tijdvenster wordt weergegeven.  Dit kunt u vaststellen van de frequentie waarmee u het aantal resultaten die u verwacht.
4.  Klik op **Opslaan** om de waarschuwingsregel.  Start het onmiddellijk uitgevoerd.


![Waarschuwingsregel toevoegen](media/log-analytics-alerts/add-alert-rule.png)

| Eigenschap | Beschrijving |
|:--|:--|
| **Informatie over waarschuwingen** | |
| Naam |  Unieke naam voor de huidige regel. |
| Ernst | De ernst van de waarschuwing die door deze regel wordt gemaakt. |
| Zoekopdracht | Selecteer **huidige zoekopdracht gebruiken** om de huidige query gebruiken of Selecteer een bestaande opgeslagen zoekopdracht in de lijst.  De querysyntaxis van de is beschikbaar in het tekstvak waar u kunt deze desgewenst wijzigen.  |
| Tijdvenster | Hiermee geeft u het tijdsbereik voor de query.  De query retourneert alleen de records die zijn gemaakt binnen dit bereik van de huidige tijd.  Dit is een waarde tussen 5 minuten en 24 uur.  Moet groter dan of gelijk aan de frequentie van waarschuwingen.  <br><br> Bijvoorbeeld als het venster is ingesteld op 60 minuten en de query wordt uitgevoerd om 13:15 uur, alleen de records die zijn gemaakt tussen 12:15 uur en 13:15 uur geretourneerd. |
| **Planning** |     
| Drempel | Criteria voor het maken van een waarschuwing.  Een melding wordt gemaakt als het aantal records dat wordt geretourneerd door de query aan deze criteria voldoet. |
| Frequentie van waarschuwingen | Hiermee geeft u op hoe vaak de query moet worden uitgevoerd.  Is een waarde tussen 5 minuten en 24 uur.  Moet gelijk zijn aan of kleiner is dan het venster. |
| Waarschuwingen onderdrukken | Wanneer u de onderdrukking van de waarschuwingsregel inschakelt, worden acties voor de regel voor een gedefinieerde periode na het maken van een nieuwe waarschuwing uitgeschakeld.  De regel is nog steeds actief en alert records maakt als aan de criteria is voldaan.  Dit is om dat u tijd het probleem te verhelpen zonder dubbele acties uitgevoerd. |
| **Acties** | |
| E-mailmeldingen | Geef **Ja** op als u een e-mailbericht moet worden verzonden wanneer de waarschuwing wordt gegeven. |
| Onderwerp    | Onderwerpen in de e-mail.  De hoofdtekst van het e-mailbericht kunt u niet wijzigen. |
| Geadresseerden | Adressen van alle geadresseerden.  Als u meer dan één adres opgeeft, vervolgens tussen de adressen een puntkomma (;). |
| Webhook | Geef **Ja** op als u bellen een webhook wilt wanneer het signaal wordt gegenereerd. |
| Webhook-URL | De URL van de webhook. |
| Aangepaste JSON-nettolading bevatten | Selecteer deze optie als u wilt de nettolading standaard vervangen door een aangepaste nettolading. |
| Geef uw aangepaste JSON-nettolading | De aangepaste payload voor de webhook.  Zie de vorige sectie voor meer informatie. |
| Runbook | Geef **Ja** op als u een runbook Azure Automatisering gestart wilt wanneer het signaal wordt gegenereerd. |
| Selecteer een runbook | Selecteer de runbook starten vanaf de runbooks in de automatisering-account geconfigureerd in uw oplossing voor automatisering. |
| Uitgevoerd op | Selecteer **Azure** voor het uitvoeren van de runbook in de cloud Azure.  Selecteer **Hybride werknemer** uit te voeren van de runbook op een [Hybride Runbook werknemer](..\automation\automation-hybrid-runbook-worker.md) in uw lokale omgeving. |


## <a name="manage-alert-rules"></a>Waarschuwingsregels beheren
U kunt een lijst met alle regels in het menu **waarschuwingen** in logboek Analytics- **Instellingen**.  

![Waarschuwingen beheren](./media/log-analytics-alerts/configure.png)

1. Selecteer de tegel **Instellingen** in de console OMS.
2. Selecteer **waarschuwingen**.

Vanuit deze weergave kunt u meerdere acties uitvoeren.

- Een regel uitschakelen door te selecteren **uit** ernaast.
- Een waarschuwingsregel bewerken door op het potloodpictogram te klikken.
- Een waarschuwingsregel verwijderen door te klikken op het pictogram **X** . 


## <a name="setting-time-windows"></a>Tijdvensters instellen 

### <a name="event-alerts"></a>Gebeurtenis-signalen

Gebeurtenissen zijn gegevensbronnen, zoals de Windows-gebeurtenislogboeken, Syslog, en aangepast worden.  U kunt een waarschuwing wanneer een bepaalde foutgebeurtenis wordt gemaakt of wanneer meerdere foutgebeurtenissen worden gemaakt binnen een bepaalde tijd te maken.

Waarschuw op een enkele gebeurtenis, zodanig dat het nummer van de resultaten groter is dan 0 en zowel de frequentie en de tijdsduur op 5 minuten.  Dat wordt de query uitgevoerd om de 5 minuten en controleren of het exemplaar van een enkele gebeurtenis die zijn gemaakt sinds de laatste keer dat de query is uitgevoerd.  Een langere frequentie kan vertragen de tijd tussen de gebeurtenis worden verzameld en de waarschuwing wordt gemaakt.

Sommige toepassingen kunnen zich aanmelden voor een occasionele fout die noodzakelijkerwijs een waarschuwing niet moet verhogen.  De toepassing kan bijvoorbeeld proberen het proces dat de foutgebeurtenis gemaakt en dan de volgende keer slagen.  In dit geval raadzaam niet het signaal maken, tenzij meerdere gebeurtenissen binnen een bepaalde tijd worden gemaakt.  

In sommige gevallen kunt u een melding maakt bij het ontbreken van een gebeurtenis.  Bijvoorbeeld kan een proces gebeurtenissen regelmatig om aan te geven dat deze correct werkt.  Als het niet een van deze gebeurtenissen binnen een bepaalde tijd vastleggen, moet een waarschuwing worden gemaakt.  In dit geval stelt u de drempel *minder dan*1.

### <a name="performance-alerts"></a>Prestatiesignalen

[Prestatiegegevens](log-analytics-data-sources-performance-counters.md) worden opgeslagen als de records in de opslagplaats OMS vergelijkbaar met de gebeurtenissen.  De waarde in elke record is het gemiddelde over de vorige 30 minuten gemeten.  Als u weten wilt wanneer een prestatiemeteritem een bepaalde drempelwaarde overschrijdt, moet deze drempel worden opgenomen in de query.

Bijvoorbeeld als u wilt worden gewaarschuwd wanneer de processor wordt uitgevoerd gedurende 30 minuten meer dan 90% gebruikt u een query als *Type = Perf ObjectName Processor CounterName = 'percentage processortijd' = tegenwaarde > 90* en de drempel voor de waarschuwingsregel *groter is dan 0*.  

 Aangezien de [prestatiegegevens](log-analytics-data-sources-performance-counters.md) worden elke 30 minuten, ongeacht de frequentie verzamelen van elk item worden samengevoegd, mogelijk een tijdvenster kleiner dan 30 minuten geen records als resultaat.  Het tijdvenster instellen op 30 minuten wordt ervoor zorgen dat u één record voor elke aangesloten bron waarmee het gemiddelde over die tijd.

## <a name="alert-actions"></a>Acties bij waarschuwingen

Naast het maken van een record voor een waarschuwing, kunt u de waarschuwingsregel een of meer acties automatisch worden uitgevoerd.  Acties kunnen proactief een melding van de waarschuwing of aanroepen van een proces waarmee wordt geprobeerd om het probleem is aangetroffen.  De volgende secties worden de acties die momenteel beschikbaar zijn.

### <a name="email-actions"></a>E-mail acties
Een e-mailbericht met de details van de waarschuwing verzenden acties e-mail naar een of meer geadresseerden.  Kunt u het onderwerp van het e-mailbericht, maar de inhoud is een standaardindeling die door Analytics logboek gemaakt.  Samenvattende informatie zoals de naam van de waarschuwing naast de tien records gevonden logboekbestand bevat.  Het bevat ook een koppeling naar een logboek zoeken in logboek Analytics die de hele set van records van die query wordt geretourneerd.   De afzender van het e-mailbericht is *Microsoft Operations Management Suite Team &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Webhook acties

Webhook acties kunt u een extern proces door middel van een HTTP POST-aanvraag aanroepen.  De service wordt aangeroepen moet ondersteuning voor webhooks en bepalen hoe de payload wordt gebruikt die worden ontvangen.  U kunt ook een REST API die specifiek geen webhooks ondersteunt als de aanvraag is in een indeling die de API begrijpt aanroepen.  Voorbeelden van het gebruik van een webhook in reactie op een signaal gebruikt een service zoals [vertraging](http://slack.com) voor het verzenden van een bericht met de details van de waarschuwing of het maken van een incident in een service zoals [PagerDuty](http://pagerduty.com/).  

Een volledig overzicht van een waarschuwingsregel maken met een webhook aan te roepen van een sample-service is beschikbaar op [Webhooks in het logboek Analytics waarschuwingen](log-analytics-alerts-webhooks.md).

Webhooks bestaan uit een URL en een nettolading in JSON die de gegevens verzonden naar de externe service is opgemaakt.  Standaard wordt de nettolading van de waarden in de volgende tabel bevatten.  U kunt deze payload vervangen door een aangepaste zelf.  In dat geval kunt u de variabelen in de tabel voor elk van de parameters aan hun waarde bevatten in de nettolading van uw aangepaste.


| Parameter | Variabele | Beschrijving |
|:--|:--|:--|
| AlertRuleName | #alertrulename | De naam van de huidige regel. |
| AlertThresholdOperator | #thresholdoperator | Drempel voor de operator voor de waarschuwingsregel.  *Groter dan* of *kleiner dan*. |
| AlertThresholdValue | #thresholdvalue | Drempelwaarde voor de waarschuwingsregel. |
| LinkToSearchResults | #linktosearchresults | Koppelen aan Analytics logboek logboek zoeken geeft als resultaat records van de query die de waarschuwing gemaakt. |
| ResultCount  | #searchresultcount | Het aantal records in de lijst met zoekresultaten. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | De eindtijd van de query in UTC-notatie. |
| SearchIntervalInSeconds | #searchinterval | Tijdvenster voor de waarschuwingsregel. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | De begintijd voor de query in UTC-indeling. |
| SearchQuery | #searchquery | Logboek zoekopdracht die wordt gebruikt door de huidige regel. |
| Zoekresultaten | Zie hieronder | Records die door de query in JSON-indeling.  Beperkt tot de eerste 5000 records. |
| WorkspaceID | #workspaceid | ID van uw werkruimte OMS. |


U kunt bijvoorbeeld de volgende aangepaste nettolading met een enkele parameter *tekst*genoemd opgeven.  De service die deze webhook wordt aangeroepen, zou deze parameter worden verwacht.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Dit voorbeeld payload zou omzetten in iets als volgt wanneer naar de webhook verzonden.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Voeg de volgende regel als een eigenschap van het hoogste niveau in de nettolading van json zodanig zoekresultaten in een aangepaste-nettolading.  

    "IncludeSearchResults":true

Bijvoorbeeld, u maakt een aangepaste nettolading met alleen de naam van het signaal en de zoekresultaten, kunt u het volgende. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


U kunt een volledig voorbeeld van een waarschuwingsregel maken met een webhook starten van een externe service op [logboek Analytics waarschuwen webhook monster](log-analytics-alerts-webhooks.md)doorlopen.

### <a name="runbook-actions"></a>Runbook acties

Een runbook starten Runbook acties in Azure automatisering.  Om dit type actie gebruikt, hebt u [automatiseringsoplossing](log-analytics-add-solutions.md) geïnstalleerd en geconfigureerd in uw werkruimte OMS.  Als u niet geïnstalleerd hebt wanneer u een nieuwe waarschuwingsregel maakt, wordt een koppeling naar de installatie weergegeven.  U kunt kiezen uit de runbooks in de automatisering die u hebt geconfigureerd in de oplossing voor automatisering.

Runbook acties starten de runbook met behulp van een [webhook](../automation/automation-webhooks.md).  Wanneer u de waarschuwingsregel maakt, maakt het automatisch een nieuwe webhook voor de runbook met de naam **OMS melding herstel** gevolgd door een GUID.  

U kan niet rechtstreeks waarden voor de parameters van de runbook, maar de [parameter $WebhookData](../automation/automation-webhooks.md) bevat de details van de waarschuwing, met inbegrip van de resultaten van het logboek zoeken die u hebt gemaakt.  De runbook moet **$WebhookData** definiëren als een parameter voor de toegang tot de eigenschappen van de waarschuwing.  De waarschuwingsgegevens is beschikbaar in json-indeling in een enkele eigenschap met de naam van **zoekresultaten** in de eigenschap **RequestBody** van **$WebhookData**.  Dit heeft met de eigenschappen in de volgende tabel.


| Knooppunt | Beschrijving |
|:--|:--|
| ID         | Pad en de GUID van de zoekactie. |
| __metadata | Informatie over de waarschuwing met inbegrip van het aantal records en de status van de zoekresultaten. |
|  waarde     |  Afzonderlijke vermelding voor elke record in de lijst met zoekresultaten.  De details van de post komt overeen met de eigenschappen en waarden van de record.   |

De volgende runbook zou bijvoorbeeld ophalen van de records die worden geretourneerd door de zoekfunctie en andere eigenschappen op basis van het type van elke record toewijzen.  Houd er rekening mee dat de runbook begint met het converteren van **RequestBody** van json zodat u deze kunt bewerken als een object in PowerShell.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Waarschuwing records

Waarschuwing records gemaakt door waarschuwingsregels in logboek Analytics hebben een **Type** **Waarschuwing** en een **SourceSystem** van **OMS**.  Ze hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Type          | *Een waarschuwing* |
| SourceSystem  | *OMS* |
| AlertSeverity | De ernst van de waarschuwing. |
| AlertName     | De naam van het signaal. |
| Query         | De tekst van de query die is uitgevoerd.  |
| QueryExecutionEndTime   | Einde van de periode voor de query. |
| QueryExecutionStartTime | Begin van de periode voor de query.  |
| TimeGenerated | De datum en tijd waarop de waarschuwing is gemaakt. |

Er zijn andere soorten alert records die zijn gemaakt door de [oplossing voor waarschuwing](log-analytics-solution-alert-management.md) en [Power BI wordt geëxporteerd](log-analytics-powerbi.md).  Deze hebben allemaal een **soort** van **Waarschuwing** , maar worden onderscheiden door hun **SourceSystem**.




## <a name="next-steps"></a>Volgende stappen

- Installeer de [oplossing voor beheer van waarschuwingen](log-analytics-solution-alert-management.md) voor het analyseren van waarschuwingen en meldingen verzameld van System Center Operations Manager (SCOM) in Analytics logboek gemaakt.
- Lees meer over [logboek zoekopdrachten](log-analytics-log-searches.md) die waarschuwingen kunt genereren.
- Een procedure voor het [configureren van een webook](log-analytics-alerts-webhooks.md) met een waarschuwingsregel te voltooien.  
- Informatie over het schrijven van [runbooks in Azure automatisering](https://azure.microsoft.com/documentation/services/automation) om een door waarschuwingen geconstateerde problemen te verhelpen.