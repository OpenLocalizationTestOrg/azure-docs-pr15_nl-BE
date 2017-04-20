<properties
   pageTitle="Azure Resource Manager verzoeklimieten | Microsoft Azure"
   description="Beschrijving van het gebruik beperken met Azure Resource Manager aanvragen wanneer abonnementen is bereikt."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Bandbreedtebeperking Resource Manager aanvragen

Voor elk abonnement en de huurder Resource Manager aanvragen naar 15.000 euro per uur voor lezen en schrijven aanvragen naar 1.200 per uur. Als uw toepassing of het script deze limiet is bereikt, moet u uw aanvragen te beperken. In dit onderwerp ziet u het bepalen van de resterende verzoeken die u voordat de limiet bereikt hebben, en hoe te reageren als de limiet is bereikt.

Wanneer de limiet is bereikt, wordt de HTTP-statuscode **429 te veel aanvragen voor**.

Het aantal aanvragen is binnen het bereik van uw abonnement of de huurder. Als er meerdere gelijktijdige toepassingen voor aanvragen die bij uw abonnement, het aanvragen van deze toepassingen worden toegevoegd om te bepalen van het aantal resterende aanvragen.

Binnen het bereik van abonnement aanvragen zijn die door de betrokken bij uw abonnement-id, zoals het ophalen van de resource wordt doorgegeven in uw abonnement groepen. Binnen het bereik van huurder aanvragen bevatten niet uw abonnements-id, zoals het ophalen van geldige Azure locaties.

## <a name="remaining-requests"></a>Overige opdrachten

Het aantal resterende aanvragen kunt u bepalen door het onderzoeken van antwoordheaders. Elke aanvraag bevat waarden voor het aantal resterende lees- en schrijfaanvragen. De volgende tabel beschrijft de antwoordheaders die kunt u deze waarden bestuderen:

| Response-header | Beschrijving |
| --------------- | ----------- |
| x-MS-ratelimit-Remaining-Subscription-reads | Abonnement binnen het bereik wordt gelezen resterende |
| x-MS-ratelimit-Remaining-Subscription-Writes | Binnen het bereik van abonnement schrijft resterende |
| x-MS-ratelimit-Remaining-tenant-reads | Binnen het bereik van huurder leest resterende |
| x-MS-ratelimit-Remaining-tenant-Writes | Binnen het bereik van huurder schrijft resterende |
| x-MS-ratelimit-Remaining-Subscription-resource-Requests | Resource type aanvragen nog binnen het bereik van abonnement.<br /><br />Deze headerwaarde wordt alleen geretourneerd als een service de standaardlimiet is opgeheven. Resource Manager, wordt deze waarde in plaats van het abonnement lees- of schrijfbewerkingen. |
| x-MS-ratelimit-Remaining-Subscription-resource-ENTITIES-Read | Resource type collectie aanvragen nog binnen het bereik van abonnement.<br /><br />Deze headerwaarde wordt alleen geretourneerd als een service de standaardlimiet is opgeheven. Deze waarde geeft het aantal resterende collectie aanvragen (lijst resources). |
| x-MS-ratelimit-Remaining-tenant-resource-Requests | Resource type aanvragen nog binnen het bereik van huurder.<br /><br />Deze header alleen voor aanvragen op het niveau van de huurder wordt toegevoegd, en alleen als een service heeft de standaardlimiet overschreven. Resource Manager, wordt deze waarde in plaats van de huurder lees- of schrijfbewerkingen. |
| x-MS-ratelimit-Remaining-tenant-resource-ENTITIES-Read | Resource type collectie aanvragen nog binnen het bereik van huurder.<br /><br />Deze header alleen voor aanvragen op het niveau van de huurder wordt toegevoegd, en alleen als een service heeft de standaardlimiet overschreven. |

## <a name="retrieving-the-header-values"></a>De waarden van koptekst ophalen

Deze headerwaarden in uw code of script ophalen is niet anders dan het ophalen van de waarde van de header. 

Bijvoorbeeld, in **C#**ophalen u de koptekst waarde van een **HttpWebResponse** -object met de naam **antwoord** met de volgende code:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

In **PowerShell**, kunt u de waarde van de header van een bewerking Invoke WebRequest ophalen.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Of wilt zien van de overige aanvragen voor foutopsporing, kunt u opgeven als de **-Debug** parameter op de **PowerShell** -cmdlet.

    Get-AzureRmResourceGroup -Debug
    
Die als resultaat een grote hoeveelheid informatie, met inbegrip van de waarde van het volgende antwoord:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

In **Azure CLI**, kunt u de waarde van de header ophalen met behulp van de meer uitgebreide optie.

    azure group list -vv --json

Die een grote hoeveelheid informatie, met inbegrip van het volgende object als resultaat:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Wacht voordat de volgende aanvraag verzenden

Wanneer u de aanvraaglimiet bereikt, Resource Manager de **429** HTTP-statuscode retourneert en een **Nieuwe pogingen na** in de header. Het **Opnieuw proberen na** waarde geeft het aantal seconden moet uw toepassing wachten (of slaapstand) voordat de volgende aanvraag wordt verzonden. Als u een aanvraag verzendt voordat de waarde van de nieuwe poging is verstreken, uw aanvraag is niet verwerkt en wordt een nieuwe poging waarde geretourneerd.
