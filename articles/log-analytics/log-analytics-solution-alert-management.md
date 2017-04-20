<properties
   pageTitle="Waarschuw beheeroplossing in Operations Management Suite (OMS) | Microsoft Azure"
   description="De oplossing van het beheer van waarschuwingen in logboek Analytics kunt u analyseren alle signalen die in uw omgeving.  Naast waarschuwingen gegenereerd binnen OMS consolideren, waarschuwingen van geïmporteerd verbonden System Center Operations Manager (SCOM) beheer van groepen in het logboek voor Analytics."
   services="log-analytics"
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
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Waarschuwing beheeroplossing in Operations Management Suite (OMS)

![Waarschuwingspictogram voor beheer](media/log-analytics-solution-alert-management/icon.png) De oplossing voor beheer van waarschuwingen kunt u alle signalen die in uw omgeving te analyseren.  Naast waarschuwingen gegenereerd binnen OMS consolideren, waarschuwingen van geïmporteerd verbonden System Center Operations Manager (SCOM) beheer van groepen in het logboek voor Analytics.  In een omgeving met meerdere groepen van management krijgt oplossing voor het beheer van waarschuwingen een geconsolideerde weergave van waarschuwingen over alle groepen van het management.

## <a name="prerequisites"></a>Vereisten

- Deze oplossing vereist om waarschuwingen van SCOM importeren, een verbinding tussen uw werkruimte OMS en SCOM management groep met behulp van het proces beschreven in [Operations Manager verbinding maken met Analytics logboek](log-analytics-om-agents.md).  


## <a name="configuration"></a>Configuratie

De oplossing voor beheer van waarschuwingen toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [Software-oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.

## <a name="management-packs"></a>Beheerpakketten

Als uw SCOM management groep is verbonden met uw werkruimte OMS, vervolgens de volgende management packs geïnstalleerd in SCOM wanneer u deze oplossing toevoegen.  Er is geen configuratie of het onderhoud van deze management packs vereist.  

- Microsoft System Center adviseur beheer van waarschuwingen (Microsoft.IntelligencePacks.AlertManagement)

Zie voor meer informatie over hoe de beheerpakketten oplossing worden bijgewerkt, [Operations Manager verbinding maken met Analytics logboek](log-analytics-om-agents.md).

## <a name="data-collection"></a>Het verzamelen van gegevens

### <a name="agents"></a>Agenten

De volgende tabel beschrijft de gekoppelde bronnen die door deze oplossing worden ondersteund.

| Aangesloten bron | Ondersteuning | Beschrijving |
|:--|:--|:--|
| [Windows-agenten](log-analytics-windows-agents.md) | Nee | Directe Windows agenten genereren geen waarschuwingen voor SCOM. |
| [Linux-agenten](log-analytics-linux-agents.md) | Nee | Directe Linux agenten genereren geen waarschuwingen voor SCOM. |
| [SCOM management groep](log-analytics-om-agents.md) | Ja | Meldingen die worden gegenereerd op SCOM agenten zijn afgeleverd bij de management-groep en vervolgens doorgestuurd naar het logboek voor Analytics.<br><br>Een directe verbinding van de agent SCOM met Analytics logboek is niet vereist. Waarschuwingsgegevens uit de management-groep doorgestuurd naar de opslagplaats OMS. |
| [Azure opslag account](log-analytics-azure-storage.md) | Nee | SCOM waarschuwingen worden niet opgeslagen in Azure opslag rekeningen. |

### <a name="collection-frequency"></a>Frequentie van verzameling

Waarschuwingen gegenereerd binnen OMS zijn direct beschikbaar zijn voor de oplossing.  Waarschuwingsgegevens verzonden uit de groep SCOM management naar logboek Analytics om de 3 minuten.  

## <a name="using-the-solution"></a>Met behulp van de oplossing

Wanneer u de waarschuwing-oplossing aan uw werkruimte OMS toevoegen, zal de tegel **Beheer van waarschuwingen** worden toegevoegd aan het dashboard OMS.  Dit element geeft een aantal en een grafische weergave van het aantal actieve waarschuwingen die zijn gegenereerd in de afgelopen 24 uur.  U kunt deze periode niet wijzigen.

![Alert Management tegel](media/log-analytics-solution-alert-management/tile.png)

Klik op de tegel **Beheer van waarschuwingen** voor het openen van het **Beheer van waarschuwingen** dashboard.  Het dashboard bestaat uit de kolommen in de volgende tabel.  Elke kolom toont de top tien waarschuwingen met count overeenkomen met de criteria van de kolom voor het opgegeven bereik en het tijdsbereik.  U kunt een zoekfunctie waarmee u de hele lijst door te klikken op **alle** onder aan de kolom of door te klikken op de kolomkop kunt uitvoeren.

| Kolom| Beschrijving |
|:--|:--|
| Kritieke signalen | Alle berichten met een prioriteit van kritische gegroepeerd op de naam van waarschuwing.  Klik op de naam van een waarschuwing voor het uitvoeren van een logboek zoeken alle records voor deze waarschuwing. |
| Waarschuwing waarschuwingen | Alle berichten met een prioriteit van de waarschuwing die zijn gegroepeerd op de naam van waarschuwing.  Klik op de naam van een waarschuwing voor het uitvoeren van een logboek zoeken alle records voor deze waarschuwing. |
| Actieve SCOM waarschuwingen | Alle waarschuwingen voor SCOM met een staat andere dan *afgesloten* gegroepeerd op bron die de waarschuwing heeft gegenereerd. |
| Alle actieve waarschuwingen | Alle waarschuwingen met alle ernst gegroepeerd op de naam van waarschuwing. Bevat alleen meldingen met een staat die geen *gesloten*SCOM.|

Als u naar rechts schuift, vindt u verschillende algemene query's waarop u klikken kunt op voor het uitvoeren van een [zoekfunctie](log-analytics-log-searches.md) voor waarschuwingsgegevens in het dashboard.

![Alert Management dashboard](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Scope- en tijdbereik

Standaard is de omvang van de waarschuwingen geanalyseerd in de oplossing van het beheer van waarschuwingen uit alle gekoppelde beheer groepen gegenereerd binnen de laatste 7 dagen.  

![Alert Management scope](media/log-analytics-solution-alert-management/scope.png)

- Het beheer van groepen in de analyse opgenomen, klikt u op **bereik** op de bovenzijde van het dashboard.  U kunt ook selecteren **globaal** voor alle groepen van verbonden management of **Management Group door** een enkele management groep selecteren.

- Als u het tijdsbereik van waarschuwingen, selecteert de **gegevens op basis van** aan de bovenkant van het dashboard.  U kunt waarschuwingen in de laatste 7 dagen, 1 dag of 6 uur.  Of selecteer **aangepast** en geef een aangepast datumbereik.

## <a name="log-analytics-records"></a>Logboekrecords Analytics

De waarschuwing-oplossing voor analyseert elke record van het type **Waarschuwing**.  Ook worden berichten importeren uit SCOM en een record maken voor elk type **Waarschuwing** en een SourceSystem van **OpsManager**.  Deze records bevatten de eigenschappen in de volgende tabel.  

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *Een waarschuwing* |
| SourceSystem | *OpsManager* |
| AlertContext | Details van het gegevensitem dat de waarschuwing moet worden gegenereerd in XML-indeling hebben veroorzaakt. |
| AlertDescription | Gedetailleerde beschrijving van de waarschuwing. |
| AlertId | De GUID van de waarschuwing. |
| AlertName | De naam van het signaal. |
| AlertPriority | Prioriteitsniveau van het signaal. |
| AlertSeverity | De ernst van de waarschuwing. |
| AlertState | Meest recente status van de resolutie van de waarschuwing. |
| LastModifiedBy | De naam van de gebruiker die het bericht het laatst is gewijzigd. |
| ManagementGroupName | De naam van de management-groep waar het alarm is gegenereerd. |
| RepeatCount | Het aantal keer dat het dezelfde alarm is gegenereerd voor dezelfde gevolgd object sinds wordt herleid. |
| ResolvedBy | De naam van de gebruiker die de waarschuwing is opgelost. Leeg zijn als de waarschuwing nog niet opgelost is. |
| SourceDisplayName | De weergegeven naam van de controle-object die de waarschuwing heeft gegenereerd. |
| SourceFullName | De volledige naam van de controle-object die de waarschuwing heeft gegenereerd. |
| TicketId | Ticket-ID voor de waarschuwing als de omgeving van SCOM is geïntegreerd met een proces voor het toewijzen van tickets voor waarschuwingen.  Leeg van geen ticket-ID is toegewezen. |
| TimeGenerated | De datum en tijd waarop de waarschuwing is gemaakt. |
| TimeLastModified | Datum en tijd waarop het bericht het laatst is gewijzigd. |
| TimeRaised | Datum en tijd waarop het alarm is gegenereerd. |
| TimeResolved | Datum en tijd waarop de waarschuwing is opgelost. Leeg zijn als de waarschuwing nog niet opgelost is. |

## <a name="sample-log-searches"></a>Monster logboek zoeken

De volgende tabel bevat een voorbeeld logboek zoeken naar alert records verzameld door deze oplossing.  

| Query | Beschrijving |
|:--|:--|
| Type waarschuwing SourceSystem = OpsManager AlertSeverity = fout TimeRaised = > nu - 24 uur per dag | Signalen die optreedt tijdens de afgelopen 24 uur |
| Type waarschuwing AlertSeverity = waarschuwing TimeRaised = > nu - 24 uur per dag | Waarschuwing waarschuwingen gegenereerd tijdens de afgelopen 24 uur  |
| Type waarschuwing SourceSystem = OpsManager AlertState =! = gesloten TimeRaised > nu 24 uur & #124; count() meten als de telling door SourceDisplayName | Bronnen met actieve waarschuwingen gegenereerd tijdens de afgelopen 24 uur |
| Type waarschuwing SourceSystem = OpsManager AlertSeverity = fout TimeRaised = > nu - 24-UURS AlertState! = gesloten | Signalen die optreedt tijdens de afgelopen 24 uur die nog steeds actief zijn |
| Type waarschuwing SourceSystem = OpsManager TimeRaised = > nu - 24 uur per dag AlertState = gesloten | Waarschuwingen tijdens de afgelopen 24 uur die nu worden gesloten |
| Type waarschuwing SourceSystem = OpsManager TimeRaised = > nu 1 dag & #124; count() meten als de telling door AlertSeverity | Tijdens de afgelopen 1 dag gegroepeerd op hun ernst waarschuwingen |
| Type waarschuwing SourceSystem = OpsManager TimeRaised = > nu 1 dag & #124; RepeatCount desc sorteren | Waarschuwingen tijdens de afgelopen 1 dag gesorteerd op de waarde voor aantal herhalingen |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [waarschuwingen in logboek Analytics](log-analytics-alerts.md) voor meer informatie over het genereren van waarschuwingen in logboek Analytics.
