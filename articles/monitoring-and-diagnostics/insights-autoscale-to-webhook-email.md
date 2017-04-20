<properties
    pageTitle="Automatisch schalen acties gebruiken voor het verzenden van e-mail en webhook meldingen. | Microsoft Azure"
    description="Zie de acties automatisch schalen moet bellen web-URL's of e-mailberichten verzenden in Azure Monitor. "
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
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Automatisch schalen acties gebruiken voor het verzenden van e-mail- en webhook-meldingen in Azure Monitor

In dit artikel ziet u hoe triggers zo instellen dat u kunt specifieke web-URL's bellen of verzenden van e-mails op basis van acties in Azure automatisch schalen.  

## <a name="webhooks"></a>Webhooks
Webhooks kunt u de Azure waarschuwingsmeldingen doorsturen naar andere systemen voor naverwerkingsprogramma of aangepaste meldingen. Bijvoorbeeld routering de waarschuwing naar services die een binnenkomende webaanvraag voor het verzenden van dat SMS, bugs, logboek, in een team via chat of messaging services kennis kunnen worden verwerkt, enz. De URI van de webhook moet een geldige HTTP- of HTTPS-eindpunt.

## <a name="email"></a>E-mail
E-mail kan worden verzonden naar een geldig e-mailadres. Beheerders en CO-beheerders van het abonnement waarop de regel wordt uitgevoerd ook krijgt.


## <a name="cloud-services-and-web-apps"></a>Cloud Services en Web Apps
U kunt opt-in van het portal voor Azure voor Cloud Services en Server-Farms (Web Apps).

- De metrische **schaal door te** kiezen.

![door schalen](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Virtuele Machine schaal sets
Voor nieuwere virtuele Machines met Resource Manager (virtuele Machine schaal sets) gemaakt, kunt u dit met behulp van REST API, Resource Manager sjablonen PowerShell en CLI. Een portal-interface is nog niet beschikbaar.
Wanneer u de sjabloon REST API of Resource Manager, bevatten het element meldingen met de volgende opties.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Veld                              |Verplicht? |Beschrijving|
|---                                |---        |---|
|bewerking                          |Ja        |de waarde moet "Scale"|
|sendToSubscriptionAdministrator    |Ja        |de waarde moet "true" of "false"|
|sendToSubscriptionCoAdministrators |Ja        |de waarde moet "true" of "false"|
|customEmails                       |Ja        |waarden zijn null [] of een matrix van string van e-mails|
|webhooks                           |Ja        |waarde is null of een geldige Uri|
|serviceUri                         |Ja        |een geldige Uri-https|
|Eigenschappen                         |Ja        |waarde moet zijn lege {} of een sleutel / waarde-paren|


## <a name="authentication-in-webhooks"></a>Verificatie in de webhooks
Er zijn twee vormen van verificatie URI:

1. Verificatie van token base, waar u de URI van de webhook met een token-ID als queryparameter opslaan. Bijvoorbeeld: https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Basisverificatie, waar u een gebruikersnaam en wachtwoord gebruiken. Bijvoorbeeld:https://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Automatisch schalen melding webhook payload schema
Wanneer de melding automatisch schalen wordt gegenereerd, wordt de volgende metagegevens opgenomen in de nettolading van webhook:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Veld  |Verplicht?|    Beschrijving|
|---|---|---|
|status |Ja    |De status die aangeeft dat een actie automatisch schalen is gegenereerd|
|bewerking| Ja |Voor een verhoging van de exemplaren worden "Scale Out" en voor een daling van de instanties, is 'Schaal In'|
|context|   Ja |Het kader van de actie automatisch schalen|
|tijdstempel| Ja |Tijdstempel als de actie automatisch schalen is geactiveerd.|
|ID |Ja|   Resource Manager-ID van de instelling automatisch schalen|
|naam   |Ja|   De naam van de instelling automatisch schalen|
|Details|   Ja |Uitleg van de actie die de service automatisch schalen heeft genomen en de wijziging in het aantal exemplaar|
|subscriptionId|    Ja |Abonnement-ID van het doel middel dat wordt aangepast|
|resourceGroupName| Ja|    Naam van het doel middel dat wordt geschaald resourcegroep|
|resourceName   |Ja|   Naam van de doel-resource die wordt aangepast|
|Brontype   |Ja|   De drie ondersteunde waarden: "microsoft.classiccompute/domainnames/slots/roles" - Service Cloud rollen, "microsoft.compute/virtualmachinescalesets" - schaal Sets van virtuele Machine, en "Microsoft.Web/serverfarms" - Web App|
|resourceId |Ja|Resource Manager-ID van het doel middel dat wordt aangepast|
|portalLink |Ja    |Azure portal koppelen aan de overzichtspagina van de doelbron|
|oldCapacity|   Ja |De huidige (oude) exemplaar tellen als automatisch schalen een schaal actie heeft|
|newCapacity|   Ja |Het nieuwe exemplaar tellen die automatisch schalen de bron geschaald|
|Eigenschappen|    Nee| Dit is optioneel. Set < sleutel, waarde > paren (bijvoorbeeld woordenlijst < String, String >). Eigenschappen van dit veld is optioneel. In een aangepaste gebruikersinterface of logica op basis van app workflow kunt u sleutels en waarden die kunnen worden doorgegeven met behulp van de nettolading. Een andere methode voor het doorgeven van aangepaste eigenschappen terug naar de uitgaande oproep voor webhook is het gebruik van de webhook URI zelf (als de query-parameters)|
