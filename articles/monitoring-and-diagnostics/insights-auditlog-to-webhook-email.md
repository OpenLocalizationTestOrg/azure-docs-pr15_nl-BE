<properties
    pageTitle="Een webhook voor logboek met faxactiviteit Azure waarschuwingen configureren | Microsoft Azure"
    description="Zie het logboek voor faxactiviteit om waarschuwingen te gebruiken om aan te roepen webhooks. "
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Een webhook in een logboek met faxactiviteit Azure waarschuwingen configureren

Webhooks kunt u een waarschuwingsbericht Azure doorsturen naar andere systemen voor naverwerkingsprogramma of aangepaste acties. U kunt een webhook van een waarschuwing rondsturen naar services die SMS-bericht verzenden, meld bugs, kennis van een team via chat/messaging services of een willekeurig aantal andere acties uitvoeren. In dit artikel wordt beschreven hoe een webhook instellen voor een logboek met faxactiviteit Azure waarschuwing en de nettolading van de HTTP POST naar een webhook ziet eruit als. Voor informatie over de installatie en het schema voor een metrische Azure signaal, [Zie deze pagina in plaats daarvan](insights-webhooks-alerts.md). U kunt ook een logboek met faxactiviteit waarschuwing instellen per e-mail wanneer geactiveerd.

>[AZURE.NOTE] Deze functie is momenteel in de voorvertoning, dus verwacht variabele kwaliteit en prestaties.

U kunt een logboek met faxactiviteit waarschuwing [Azure PowerShell Cmdlets](insights-powershell-samples.md#create-alert-rules), [Cross-Platform CLI](insights-cli-samples.md#work-with-alerts)of [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)instellen.

## <a name="authenticating-the-webhook"></a>Verificatie van de webhook
TThe webhook kan worden geverifieerd met behulp van deze methoden:

1. **Verificatie op basis van het token** - URI van de webhook wordt opgeslagen met een token-ID, bv. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Basisverificatie** - URI van de webhook wordt opgeslagen met een gebruikersnaam en wachtwoord, bv. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Schema Payload
De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle waarschuwingen op basis van het logboek voor faxactiviteit. Dit schema is vergelijkbaar zijn met die door waarschuwingen op basis van metric gebruikt.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Elementnaam       |Beschrijving|
|---                |---|
|status             |Gebruikt voor metrische waarschuwingen. Altijd ingesteld op 'geactiveerd' voor waarschuwingen in logboek voor faxactiviteit.|
|context            |De context van de gebeurtenis.|
|resourceProviderName|De provider van de bron van de bron van het risico.|
|conditionType      |Altijd 'gebeurtenis'.|
|naam               |De naam van de huidige regel.|
|ID                 |Bron-ID van de waarschuwing.|
|Beschrijving        |Beschrijving van de waarschuwing ingesteld tijdens het maken van de waarschuwing.|
|subscriptionId     |Azure abonnement-ID.|
|tijdstempel          |De tijd waarop de gebeurtenis is gegenereerd door de Azure service die de aanvraag verwerkt.|
|resourceId         |Bron-ID van de resource die risico.|
|resourceGroupName  |De naam van de resourcegroep voor de bron van risico|
|Eigenschappen         |Instellen van `<Key, Value>` paren (dat wil zeggen `Dictionary<String, String>`) die informatie over de gebeurtenis bevat.|
|gebeurtenis              |Het element die metagegevens bevatten over de gebeurtenis.|
|autorisatie      |De RBAC-eigenschappen van de gebeurtenis. Deze bestaan meestal uit de 'actie', 'rol' en de 'scope'.|
|categorie           |De categorie van de gebeurtenis. Ondersteunde waarden zijn: administratieve, waarschuwing, beveiliging, ServiceHealth, aanbeveling.|
|beller             |E-mailadres van de gebruiker die heeft de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid. Kan niet null zijn voor bepaalde systeemaanroepen.|
|correlationId      |Meestal een GUID in tekenreeksindeling. Gebeurtenissen met correlationId behoren tot de grotere dezelfde actie en delen meestal een correlationId.|
|eventDescription   |Statische tekstbeschrijving van de gebeurtenis.|
|eventDataId        |De unieke id voor de gebeurtenis.|
|Gebeurtenisbron        |De naam van de Azure service of infrastructuur die de gebeurtenis heeft gegenereerd.|
|httpRequest        |Meestal bevat het "clientRequestId", "clientIpAddress" en "methode" (bv. HTTP-methode PUT).|
|niveau              |Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Informatie' en 'Uitgebreid'.|
|Bewerkingsnummer        |Meestal een GUID gedeeld door de gebeurtenissen die overeenkomt met één bewerking.|
|operationName      |De naam van de bewerking.|
|Eigenschappen         |Eigenschappen van de gebeurtenis.|
|status             |De tekenreeks. De status van de bewerking. Gemeenschappelijke waarden zijn: "Gestart", "In uitvoering", "Geslaagd", "Mislukt", 'Actief', 'Opgelost'.|
|subStatus          |Meestal bevat de HTTP-statuscode van de bijbehorende REST-aanroep. Het kan ook andere tekenreeksen met een beschrijving van een substatus bevatten. Algemene substatus waarden zijn: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), onjuiste aanvraag (HTTP-statuscode: 400), niet vinden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), Gateway time-out (HTTP-statuscode: 504)|

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over het logboek voor faxactiviteit](monitoring-overview-activity-logs.md)
- [Azure automatiseringsscripts (Runbooks) uitvoeren op Azure waarschuwingen](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Gebruik logica App voor het verzenden van een SMS-bericht via de Twilio van een waarschuwing Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden aangepast voor gebruik met een waarschuwing voor logboek met faxactiviteit.
- [Gebruik logica App een bericht toegestane vertraging van een Azure waarschuwing te verzenden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden aangepast voor gebruik met een waarschuwing voor logboek met faxactiviteit.
- [Gebruik logica App voor het verzenden van een bericht naar een wachtrij Azure uit een waarschuwing Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden aangepast voor gebruik met een waarschuwing voor logboek met faxactiviteit.
