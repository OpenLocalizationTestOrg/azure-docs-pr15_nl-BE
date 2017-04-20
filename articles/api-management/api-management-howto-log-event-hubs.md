<properties 
    pageTitle="Het registreren van gebeurtenissen met Azure gebeurtenis Hubs in Azure API Management | Microsoft Azure" 
    description="Informatie over gebeurtenissen op Azure gebeurtenis Hubs in Azure API beheer." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Het registreren van gebeurtenissen met Azure gebeurtenis Hubs in Azure API beheer

Azure gebeurtenis Hubs is een zeer schaalbare ingress service die u miljoenen gebeurtenissen per seconde nemen kunt, zodat u kunt verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door de aangesloten apparaten en toepassingen. Hubs gebeurtenis fungeert als de "voordeur" voor een gebeurtenis pijplijn en wanneer gegevens worden verzameld op een gebeurtenis hub, kunnen worden omgezet en opgeslagen met behulp van een real time analytics provider of batchen/opslag adapters. Gebeurtenis Hubs decouples de productie van een stroom van de gebeurtenissen van de consumptie van deze gebeurtenissen, zodat de gebeurtenis consumenten toegang hebben tot de gebeurtenissen in hun eigen planning.

Dit artikel is een aanvulling op de video [Integreren Azure API beheer met Hubs gebeurtenis](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) en wordt beschreven hoe u gebeurtenissen van beheer-API met Azure gebeurtenis Hubs.

## <a name="create-an-azure-event-hub"></a>Een Hub Azure gebeurtenis maken

Een nieuwe gebeurtenis Hub, aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com) en klik op **Nieuw**->**App Services**->**Service Bus**->**Gebeurtenis Hub**->**Snelle invoer**. Voer een naam voor de gebeurtenis Hub, regio, selecteert u een abonnement en een naamruimte. Als u een naamruimte kunt u maken door een naam te typen in het tekstvak **Namespace** eerder hebt gemaakt. Nadat alle eigenschappen zijn geconfigureerd, klikt u op **een nieuwe gebeurtenis Hub maken** de Hub gebeurtenis maken.

![Gebeurtenis hub maken][create-event-hub]

Vervolgens gaat u naar het tabblad **configureren** voor uw nieuwe gebeurtenis Hub en twee **gedeelde-beleid**te maken. Naam van de eerste afbeelding **verzenden** en geef deze machtigingen **verzenden** .

![Verzenden van beleid][sending-policy]

Naam van de tweede **ontvangen**, machtigen het **luisteren** en klikt u op **Opslaan**.

![Beleid ontvangen][receiving-policy]

Elk beleid voor gedeelde toegang kan toepassingen verzenden en ontvangen van gebeurtenissen en naar de gebeurtenis Hub. Als u de tekenreeksen voor verbindingen voor dit beleid, gaat u naar het tabblad **Dashboard** van de gebeurtenis Hub en **verbindingsgegevens**op.

![Verbindingsreeks][event-hub-dashboard]

De verbindingsreeks **verzenden** wordt gebruikt bij het vastleggen van gebeurtenissen en de verbindingsreeks **ontvangen** bij het downloaden van de gebeurtenissen van de Hub van de gebeurtenis wordt gebruikt.

![Verbindingsreeks][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Maak een logboek van beheer-API

U hebt een gebeurtenis Hub, is de volgende stap een [logboek](https://msdn.microsoft.com/library/azure/mt592020.aspx) in uw API-Management-service zodanig configureren dat deze gebeurtenissen in de Hub gebeurtenis vastleggen kunt.

Beheer-API loggers worden geconfigureerd met behulp van de [API beheer REST API](http://aka.ms/smapi). Voordat u de REST API voor de eerste keer gebruikt, de [vereisten](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) hebt gecontroleerd en hebt u [toegang tot de REST API ingeschakeld](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Een logboek maken, moet een aanvraag HTTP plaats met behulp van de volgende URL van sjabloon.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Vervangen `{your service}` met de naam van de API Management service-exemplaar.
-   Vervangen `{new logger name}` met de gewenste naam voor uw nieuwe logger. U verwijst naar deze naam wanneer u het [logboek-eventhub -](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) beleid configureren

De volgende headers toevoegen aan de aanvraag.

-   Content-Type: application/json
-   Vergunning: SharedAccessSignature uid =...
    -   Voor instructies voor het genereren van de `SharedAccessSignature` Zie [Azure API beheer REST API-verificatie](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Het hoofdgedeelte van de aanvraag met de volgende sjabloon opgeven.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`moet worden ingesteld op `AzureEventHub`.
-   `description`biedt een optionele beschrijving van het logboek en een tekenreeks met lengte nul als gewenst.
-   `credentials`bevat de `name` en `connectionString` van uw Hub Azure-gebeurtenis.

Wanneer u de aanvraag, als het logboek wordt gemaakt een statuscode van de `201 Created` wordt geretourneerd. 

>[AZURE.NOTE] Zie voor andere mogelijke geretourneerde codes en hun redenen, [maken een Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Om te zien hoe andere bewerkingen uitvoeren, zoals lijst, update en delete, raadpleegt u de documentatie van de entiteit [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx) .

## <a name="configure-log-to-eventhubs-policies"></a>Logboek-eventhubs-beleid configureren

Wanneer het logboek in de beheer-API is geconfigureerd, kunt u het logboek-eventhubs-beleid om de gewenste gebeurtenissen. Het log-eventhubs-beleid kan worden gebruikt in de beleidssectie voor inkomende of de beleidssectie voor uitgaande.

Configureren van beleid, aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com)navigeren uw service Management API en op **uitgever portal** of **beheren** voor toegang tot de portal van publisher.

![Publisher-portal][publisher-portal]

Klik op **beleid** in de beheer-API menu aan de linkerkant, selecteer het gewenste product en de API en op **toevoegen beleid**. In dit voorbeeld toevoegt is een beleid aan de **API van Echo** in het product **onbeperkt** .

![Beleid toevoegen][add-policy]

Plaats de cursor in het `inbound` beleid sectie en klik op het **logboek EventHub** beleid in te voegen de `log-to-eventhub` beleidssjabloon.

![Systeembeleid-editor][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Vervangen `logger-id` met de naam van de beheer-API logger u in de vorige stap hebt geconfigureerd.

U kunt elke expressie die een tekenreeks retourneert als de waarde voor de `log-to-eventhub` element. In dit voorbeeld wordt een tekenreeks geretourneerd met de datum en tijd, naam van de service aanvraag-id, aanvraag IP-adres en naam van de bewerking vastgelegd.

Klik op **Opslaan** om het opslaan van de configuratie van het bijgewerkte beleid. Het beleid actief is en gebeurtenissen worden geregistreerd op de opgegeven gebeurtenis Hub zodra het wordt opgeslagen.

## <a name="next-steps"></a>Volgende stappen

-   Meer informatie over Azure gebeurtenis Hubs
    -   [Aan de slag met Azure gebeurtenis Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Ontvangen berichten met EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Gebeurtenis Hubs programming guide](../event-hubs/event-hubs-programming-guide.md)
-   Meer informatie over de integratie van de beheer-API en gebeurtenis Hubs
    -   [Verwijzing naar Logger entiteit](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [logboek voor eventhub beleidsverwijzing](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [De API's met Azure API Management, gebeurtenis Hubs en Runscope controleren](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Bekijk een video-overzicht

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






