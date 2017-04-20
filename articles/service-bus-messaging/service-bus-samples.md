<properties 
    pageTitle="Overzicht messaging service Bus monsters | Microsoft Azure"
    description="Ingedeeld en monsters met koppelingen naar elke messaging Service-Bus wordt beschreven."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-messaging-samples"></a>Messaging service Bus-voorbeelden

Messaging Service Bus monsters tonen aan belangrijke functies in [messaging Service Bus](https://azure.microsoft.com/services/service-bus/) (cloud-service) en [Bus Service voor Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). In dit artikel worden ingedeeld en beschrijving van de monsters beschikbaar zijn, met koppelingen naar elk.

>[AZURE.NOTE] Service Bus monsters worden niet geïnstalleerd met de SDK. De monsters, Bezoek de [pagina met Azure SDK-voorbeelden](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Er is bovendien een bijgewerkte reeks Service Bus berichten monsters [hier](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (schrijven van dit, ze worden niet beschreven in dit artikel).  

Zie voor voorbeelden van relay [Service Bus relay monsters](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Messaging service Bus

De volgende voorbeelden illustreren hoe u toepassingen die gebruikmaken van messaging Service Bus.

Houd er rekening mee dat de berichtenafhandeling monsters een verbindingstekenreeks vereist voor toegang tot uw Service Bus-naamruimte.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Een verbindingsreeks opvragen voor Azure Service Bus

1. Meld u op de [portal Azure](http://portal.azure.com).

1. Klik in de linkerkolom op **Service Bus**.

1. Klik op de naam van de naamruimte in de lijst.

3. Klik op **beleid voor gedeelde toegang**in de naamruimte-blade.

4. Klik op **RootManageSharedAccessKey**in de blade **Shared-beleid** .

6. De tekenreeks naar het Klembord kopiëren.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Een verbindingsreeks opvragen voor Bus-Service voor Windows Server

1. De volgende PowerShell-cmdlet uitvoert:

    ```
    get-sbClientConfiguration
    ```

2. De verbindingsreeks in het bestand App.config voor het monster te plakken.

### <a name="getting-started-samples"></a>Ophalen van monsters gestart

Deze monsters beschreven messaging basisfunctionaliteit.

|Voorbeeldnaam|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[Aan de slag: Messaging met wachtrijen](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Laat zien hoe Microsoft Azure Service Bus gebruikt voor het verzenden en ontvangen van berichten uit een wachtrij.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Aan de slag: Messaging met onderwerpen](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Laat zien hoe u met Microsoft Azure Service Bus berichten verzenden en ontvangen van een onderwerp met meerdere abonnementen.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|

### <a name="exploring-features"></a>Functies

De volgende voorbeelden laten zien van verschillende functies van Service Bus.

|Voorbeeldnaam|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[HTTP-Token Providers](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Toont de verschillende manieren van de verificatie van een client HTTP/REST met Service Bus.|2.1|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Service Bus HTTP-Client](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Laat zien hoe de berichten te verzenden en ontvangen van berichten van de Bus-Service via HTTP/REST.|2.3|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Service Bus Autoforwarding](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Laat zien hoe u berichten uit een wachtrij, wachtrij of abonnement automatisch doorsturen naar een andere wachtrij of onderwerp. Ook ziet u hoe u een bericht in een wachtrij of een onderwerp via een overdracht wachtrij verzenden.|2.3|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: WCF-kanaal sessie monster](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Laat zien hoe Microsoft Azure Service Bus met behulp van Windows Communication Foundation (WCF) kanalen gebruiken. Het monster wordt het gebruik van WCF-kanalen voor het verzenden en ontvangen van berichten via een wachtrij Service Bus. In het voorbeeld ziet zowel sessie en niet-sessie communicatie via de Service Bus.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: transacties](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Atomically ziet u het gebruik van de Microsoft Azure Service Bus messaging functies binnen het transactiebereik van een met het oog batches van messaging bewerkingen worden doorgevoerd.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Beheertaken uit te voeren met behulp van REST](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Laat zien hoe het van beheerbewerkingen uitvoeren op Service Bus met de REST.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Resource Provider REST API 's](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Laat zien hoe de nieuwe Service Bus RDFE REST API's gebruiken voor het beheren van naamruimten en messaging-diensten.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: WCF Service sessie monster](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Laat zien hoe Microsoft Azure Service Bus met behulp van de WCF service-model gebruiken. Het monster wordt het gebruik van het model WCF service voor sessie-communicatie via een wachtrij Service Bus.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Aanvraag-antwoord](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Laat zien hoe de Microsoft Azure Service Bus en de aanvraag/reactie-functionaliteit te gebruiken. Het voorbeeld wordt een eenvoudige clients en servers communiceren via een wachtrij Service Bus.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: wachtrij voor onbestelbare berichten](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Laat zien hoe Microsoft Azure Service Bus en messaging 'wachtrij voor onbestelbare berichten' functies gebruiken. In het voorbeeld ziet een eenvoudige afzender en ontvanger communiceren via een wachtrij Service Bus.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Berichten uitgesteld](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Laat zien hoe u de functie bericht uitstel van Microsoft Azure Service Bus. In het voorbeeld ziet een eenvoudige afzender en ontvanger communiceren via een wachtrij Service Bus.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Berichten van sessie](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Laat zien hoe Microsoft Azure Service Bus en Messaging-sessie functies gebruiken. Het voorbeeld wordt een eenvoudige afzenders en ontvangers voor communicatie via een wachtrij Service Bus.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Aanvraag onderwerp](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Laat zien hoe u de aanvraag/reactie-patroon met behulp van Microsoft Azure Service Bus onderwerpen en abonnementen te implementeren. Het voorbeeld wordt een eenvoudige clients en servers communiceren via een onderwerp Service Bus.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Aanvraag antwoordwachtrij](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Laat zien hoe Microsoft Azure Service Bus en de aanvraag/reactie-functionaliteit te gebruiken. Het voorbeeld wordt een eenvoudige clients en servers communiceren via twee Service Bus wachtrijen.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Controle op dubbele](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Laat zien hoe Microsoft Azure Service Bus dubbele bericht detectie met wachtrijen gebruiken. Twee wachtrijen, één met dubbele-detectie wilt inschakelen en andere één zonder dubbele detectie wordt gemaakt.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Asynchrone berichtafhandeling](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Laat zien hoe u met Microsoft Azure Service Bus berichten verzenden en ontvangen asynchroon uit een wachtrij. De wachtrij biedt losgekoppelde, asynchrone communicatie tussen een verzender en een willekeurig aantal ontvangers (in dit geval een enkele ontvanger).|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Geavanceerde Filters](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Laat zien hoe u met Microsoft Azure Service Bus publiceren/abonneren geavanceerde filters. Het wordt gemaakt van een onderwerp en 3 abonnementen met verschillende filterdefinities verzendt berichten naar het onderwerp en alle berichten ontvangt van abonnementen.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Brokered Messaging: Berichten Prefetch](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Laat zien hoe de Microsoft Azure Service Bus berichten prefetch gebruiken. Deze geeft aan hoe u de functie van de prefetch berichten na ontvangen.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|

## <a name="service-bus-reference-tools"></a>Bus service reference tools

De volgende voorbeelden laten zien van verschillende functies van de service.

|Voorbeeldnaam|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[Service Bus Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|De Service Bus Explorer kunnen gebruikers verbinding maken met een naamruimte Bus Service service en messaging entiteiten beheren op een eenvoudige manier. De tool biedt geavanceerde functies, zoals de import/export-functie en de mogelijkheid expresberichten entiteiten en relay services te testen.|1.8|Microsoft Azure Service Bus; Bus service voor Windows Server|
|[Vergunning: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|Dit voorbeeld demonstreert het maken en beheren van service-identiteiten in Microsoft Azure Active Directory-toegangsbeheer (ook bekend als Access Control-Service of ACS) voor gebruik met de Service Bus.|N.V.T.|Microsoft Azure Service Bus|

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor overzichten van concepten van Service Bus.

- [Messaging service Bus-overzicht](service-bus-messaging-overview.md)
- [Service Bus-architectuur](service-bus-architecture.md)
- [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
