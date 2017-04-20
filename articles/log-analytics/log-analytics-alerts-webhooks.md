<properties
   pageTitle="Logboek Analytics alert webhook monster"
   description="Een van de acties die u in reactie op de waarschuwing in een logboek Analytics uitvoeren kunt is een *webhook*, waarmee u aan te roepen van een extern proces via een HTTP-aanvraag. Dit artikel vindt u een voorbeeld van het maken van een webhook actie in een waarschuwing in een logboek Analytics met vertraging."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks in logboekbestand Analytics waarschuwingen

Een van de acties die u in reactie op een [signaal logboek Analytics uitvoeren kunt](log-analytics-alerts.md) is een *webhook*, waarmee u een extern proces via een HTTP-aanvraag aanroepen.  U kunt lezen over de details van de waarschuwingen en webhooks in de [waarschuwingen in logboek Analytics](log-analytics-alerts.md)

In dit artikel bekijken we door een voorbeeld van het maken van een webhook actie in een waarschuwing in een logboek Analytics met vertraging is een messaging-service.

>[AZURE.NOTE] Hebt u een account toegestane vertraging voor het voltooien van dit voorbeeld.  U kunt zich aanmelden voor een gratis account op [slack.com](http://slack.com).

## <a name="step-1---enable-webhooks-in-slack"></a>Stap 1 - webhooks in de vertraging inschakelen
2.  Log in om de vertraging bij de [slack.com](http://slack.com).
3.  Een kanaal selecteren in de sectie **kanalen** in het linkerdeelvenster.  Dit is het kanaal dat het bericht ontvangt.  U kunt een van de standaard-kanalen, zoals **Algemeen** of **willekeurig**selecteren.  In een productiescenario maakt u waarschijnlijk een speciaal kanaal zoals **criticalservicealerts**. <br>

    ![Toegestane kanalen](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Klik op **een app toevoegen of aangepaste integratie** map van de toepassing te openen.
3.  Typ *webhooks* in het zoekvak en selecteer vervolgens **Binnenkomende WebHooks**. <br>

    ![Toegestane kanalen](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Klik op **installeren** naast de teamnaam van uw.
5.  Klik op **configuratie toevoegen**.
6.  Selecteer het het kanaal dat u wilt gebruiken voor dit voorbeeld en klik op de **integratie van de inkomende WebHooks toevoegen**.  
6. Kopieer de **URL Webhook**.  U plakt deze in de configuratie van waarschuwingen. <br>

    ![Toegestane kanalen](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Stap 2: waarschuwingsregel in Analytics logboek maken
1.  [Een waarschuwingsregel maken](log-analytics-alerts.md) met de volgende instellingen.
    - Query:```    Type=Event EventLevelName=error ```
    - Voor deze waarschuwing controleren elke: 5 minuten
    - Het aantal resultaten: meer dan 10
    - Via dit tijdvenster: 60 minuten
    - Selecteer **Ja** voor **Webhook** en **niet** voor de andere acties.
7. Plak de URL van de toegestane vertraging in het veld **URL Webhook** .
8. Selecteer de optie voor het **opnemen van een aangepaste JSON-nettolading**.
9. Toegestane verwacht een nettolading in JSON opgemaakt met een parameter met de naam *text*.  Dit is de tekst die wordt weergegeven in het bericht dat wordt gemaakt.  U kunt een of meer van de waarschuwing parameters met de *#* symbool zoals in het volgende voorbeeld.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![Voorbeeld van de JSON-nettolading](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Klik op **Opslaan** om de huidige regel opslaan.

10. Wacht voldoende tijd voor een signaal om te worden gemaakt, en toegestane vertraging voor een bericht met de volgende strekking worden.

    ![Voorbeeld van de webhook in de toegestane vertraging](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Geavanceerde webhook-nettolading voor toegestane vertraging

U kunt veel binnenkomende berichten met een toegestane vertraging. Zie [Binnenkomende Webhooks](https://api.slack.com/incoming-webhooks) op de toegestane website voor meer informatie. Dit is een meer complexe nettolading een RTF-bericht met opmaak maken:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Dit zou een bericht genereren in de toegestane vertraging van de volgende strekking.

![Voorbeeld van bericht in de toegestane vertraging](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Samenvatting

Met deze waarschuwingsregel in plaats zou er een bericht aan de toegestane vertraging, telkens wanneer de criteria wordt voldaan.  

Dit is slechts één voorbeeld van een actie die u in reactie op een signaal maken kunt.  U kunt een webhook actie die een andere externe service aanroept, een runbook actie start een runbook in Azure automatisering of een actie e-mailadres voor het verzenden van een e-mailbericht aan uzelf of aan andere ontvangers maken.   

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [waarschuwingen in logboek Analytics](log-analytics-alerts.md) , met inbegrip van andere acties.
- [Runbooks in Azure automatisering maken](../automation/automation-webhooks.md) die kunnen worden aangeroepen vanuit een webhook.
