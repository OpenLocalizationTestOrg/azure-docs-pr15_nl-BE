<properties 
    pageTitle="Relay service Bus monsters overzicht | Microsoft Azure"
    description="Ingedeeld en Service Bus relay monsters met koppelingen naar elk beschrijft."
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

# <a name="service-bus-relay-samples"></a>Voorbeelden van relay service Bus

De relay Service Bus voorbeelden demonstreren belangrijke functies in [relay Service Bus](https://azure.microsoft.com/services/service-bus/). In dit artikel worden ingedeeld en beschrijving van de monsters beschikbaar zijn, met koppelingen naar elk.

>[AZURE.NOTE] Service Bus monsters worden niet geïnstalleerd met de SDK. De monsters, Bezoek de [pagina met Azure SDK-voorbeelden](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Daarnaast wordt een bijgewerkte reeks Service Bus relay monsters [hier](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (schrijven van dit, ze worden niet beschreven in dit artikel).  

Zie [Service Bus berichten monsters](../service-bus-messaging/service-bus-samples.md)voor messaging monsters.

## <a name="service-bus-relay"></a>Relay service Bus

De volgende voorbeelden illustreren het schrijven van toepassingen die gebruikmaken van de relay Service Bus service.

Houd er rekening mee dat de monsters relay een verbindingsreeks toegang te krijgen tot de naamruimte Service Bus vereist.

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

## <a name="service-bus-relay"></a>Relay service Bus

Voorbeelden die aantonen de relay Service Bus dat.

### <a name="getting-started"></a>Aan de slag

|Voorbeeldnaam|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[Berichten doorgestuurd: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Laat zien hoe een client Service Bus en een service uitgevoerd op Azure. In dit voorbeeld wordt de Service Bus programmatisch geconfigureerd. Alleen milieu en beveiliging gegevens worden opgeslagen in de configuratiebestanden.|1.8|Microsoft Azure Service Bus|
|[Indirecte verificatie van berichten: Gedeeld geheim](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Laat zien hoe een naam uitgever en uitgever geheim gebruiken om te verifiëren met Service Bus.|1.8|Microsoft Azure Service Bus|
|[Messaging verificatie doorgegeven: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Laat zien hoe u een HTTP-service die client gebruikersverificatie niet vereist worden blootgesteld.|1.8|Microsoft Azure Service Bus|
|[Messaging bindingen doorgegeven: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Laat zien hoe de binding **WebHttpRelayBinding** gebruiken om binaire gegevens via het Web programmeermodel.|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging bindingen: NetTcp doorgegeven](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Laat zien hoe de binding **NetTcpRelayBinding** gebruiken.|1.8|Microsoft Azure Service Bus|

### <a name="exploring-features"></a>Functies

Voorbeelden die aantonen dat de verschillende functies van de Service Bus relay.

|Voorbeeldnaam|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[Indirecte verificatie van berichten: Eenvoudige WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Laat zien hoe een eenvoudige web token referentie gebruiken om te verifiëren met Service Bus. Het monster is vergelijkbaar met het monster Echo met een paar wijzigingen. In dit voorbeeld wordt met name een probleem in de ServiceHost (service) en de toepassingen van de ChannelFactory (client) toegevoegd.|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging: Taakverdeling](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Laat zien hoe Microsoft Azure Service Bus gebruiken om berichten te routeren naar meerdere ontvangers. Het bevat meerdere exemplaren van een eenvoudige service communiceert met een client via de **NetTcpRelayBinding** -binding|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging bindingen: Netto-gebeurtenis](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Laat zien met de **NetEventRelayBinding** -binding op Microsoft Azure Service Bus.|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging bindingen: WS2007Http sessie](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Laat zien met de **WS2007HttpRelayBinding** -binding met betrouwbare sessies is ingeschakeld. Ook ziet u hoe Service Bus referenties opgeven in het configuratiebestand in plaats van via programmacode.|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging bindingen: WS2007Http MsgSecCertificate](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Laat zien hoe de **WS2007HttpRelayBinding** binding met de veiligheid van de berichten gebruiken end-to-end-berichten beveiligen terwijl u nog steeds dat clients worden geverifieerd met Service Bus.|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging: Uitwisseling van metagegevens](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Laat zien hoe u een eindpunt voor metagegevens die gebruikmaakt van de relay-binding worden blootgesteld. **MetadataExchange** wordt ondersteund in de volgende relay-bindingen: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**en **WS2007HttpRelayBinding**.|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging bindingen: NetTcp-Direct](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Laat zien hoe de binding **NetTcpRelayBinding** ter ondersteuning van de verbindingsmodus **hybride** die eerst een indirecte verbinding tot stand brengt, en indien mogelijk automatisch overgeschakeld naar een directe verbinding tussen een client en een service configureren.|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging bindingen: NetTcp-MsgSec-gebruikersnaam](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Laat zien hoe de **NetTcpRelayBinding** -binding met de veiligheid van de berichten gebruiken.|1.8|Microsoft Azure Service Bus|
|[Indirecte Messaging bindingen: Netto Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Laat zien hoe blootstellen en een service-eindpunt met de binding van **NetOnewayRelayBinding** in beslag nemen.|1.8|Microsoft Azure Service Bus|
|[Bindingen voor indirecte Messaging: WS2007Http-eenvoudige](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Laat zien met de **WS2007HttpRelayBinding** -binding. Laat een eenvoudige service die geen beveiligingsopties gebruikt en hoeft geen clients moeten worden geverifieerd.|1.8|Microsoft Azure Service Bus|

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor overzichten van concepten van Service Bus.

- [Service Bus relay-overzicht](service-bus-relay-overview.md)
- [Service Bus-architectuur](../service-bus-messaging/service-bus-architecture.md)
- [Grondbeginselen van Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)