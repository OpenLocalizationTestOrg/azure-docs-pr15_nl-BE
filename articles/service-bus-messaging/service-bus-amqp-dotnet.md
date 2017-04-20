<properties 
    pageTitle="Bus met .NET en AMQP 1.0 Service | Microsoft Azure"
    description="Bus van .NET Service gebruiken met AMQP"
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>Bus van .NET Service gebruiken met AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>De Bus Service SDK downloaden

Ondersteuning van AMQP 1.0 is beschikbaar in de SDK van Service Bus versie 2.1 of hoger. U kunt ervoor zorgen dat u de meest recente versie door het downloaden van de bits-Service Bus van [NuGet][]hebt.

## <a name="configuring-net-applications-to-use-amqp-10"></a>.NET toepassingen configureren voor gebruik van AMQP 1.0

Standaard wordt de bibliotheek Service Bus .NET client communiceert met de Service Bus-service via een toegewezen op basis van SOAP-protocol. Protocol vereist expliciete configuratie op de Bus Service connection string, AMQP 1.0 gebruikt in plaats van de standaard zoals beschreven in de volgende sectie. Dan deze wijziging ongewijzigd toepassingscode blijft wanneer AMQP 1.0 gebruikt.

In de huidige versie zijn er een aantal API-functies die niet worden ondersteund bij gebruik van AMQP. Deze niet-ondersteunde functies worden later vermeld in de sectie [niet-ondersteunde functies, beperkingen en verschillen doorgevoerd](#unsupported-features-restrictions-and-behavioral-differences). Enkele van de geavanceerde configuratie-instellingen ook hebben een andere betekenis bij het gebruik van AMQP.

### <a name="configuration-using-appconfig"></a>Configuratie via App.config

Het is raadzaam voor toepassingen in het configuratiebestand App.config gebruiken voor het opslaan van instellingen. Voor Service Bus toepassingen kunt u voor het opslaan van de instellingen voor de waarde van de Service Bus **ConnectionString** App.config. Een voorbeeld van App.config bestand is als volgt:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

De waarde van de `Microsoft.ServiceBus.ConnectionString` is ingesteld op de Service Bus verbindingsreeks die wordt gebruikt voor het configureren van de verbinding met de Service Bus. De indeling is als volgt:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Waar `[namespace]` en `SharedAccessKey` zijn verkrijgbaar bij de [Azure portal][]. Zie [aan de slag met Service Bus wachtrijen][]voor meer informatie.

Bij het gebruik van AMQP toevoegen de verbindingsreeks met `;TransportType=Amqp`. Deze notatie geeft aan de clientbibliotheek zodat de verbinding voor Service Bus met behulp van AMQP 1.0.

## <a name="message-serialization"></a>Bericht serialisatie

Als u het standaard-protocol gebruikt, is het standaardgedrag voor serialisatie van de .NET client library met behulp van het type [DataContractSerializer][] serialiseren van een exemplaar van de [BrokeredMessage][] voor het vervoer tussen de clientbibliotheek en de Bus Service service. Wanneer de transportmodus AMQP, gebruikt de clientbibliotheek het type AMQP systeem voor serialisatie van het [bericht brokered][BrokeredMessage] in een bericht van AMQP. Deze serialisatie kan het bericht wordt ontvangen en geïnterpreteerd door een ontvangende toepassing die mogelijk op een ander platform, bijvoorbeeld een Java-toepassing die de JMS-API voor toegang tot de Bus-Service wordt uitgevoerd.

Als u een exemplaar van de [BrokeredMessage][] , kunt u een .NET-object als een parameter van de constructor fungeren als de hoofdtekst van het bericht bieden. Voor objecten die kunnen worden toegewezen aan AMQP primitieve typen, wordt de tekst omgezet in AMQP gegevenstypen. Als het object kan niet rechtstreeks worden toegewezen aan een primitief type van AMQP; dat wil zeggen, een aangepast type is gedefinieerd door de toepassing, en vervolgens het object wordt omgezet met behulp van de [DataContractSerializer][]en de geserialiseerde bytes bij een AMQP gegevens worden verzonden.

Ter vergemakkelijking van de samenwerking met niet-.NET-clients gebruiken alleen .NET die rechtstreeks in de AMQP typen voor de hoofdtekst van het bericht kunnen worden geserialiseerd. De volgende tabel worden de typen en de bijbehorende toewijzing aan het systeem van AMQP type.

| .NET-instantie objecttype          | Toegewezen AMQP Type                          | AMQP lichaam sectietype                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| BOOL                           | Boole-waarde                                   | Waarde van AMQP                                                                                                                                                |
| byte                           | ubyte                                     | Waarde van AMQP                                                                                                                                                |
| USHORT                         | USHORT                                    | Waarde van AMQP                                                                                                                                                |
| uint                           | uint                                      | Waarde van AMQP                                                                                                                                                |
| ULONG                          | ULONG                                     | Waarde van AMQP                                                                                                                                                |
| SByte                          | byte                                      | Waarde van AMQP                                                                                                                                                |
| korte                          | korte                                     | Waarde van AMQP                                                                                                                                                |
| int                            | int                                       | Waarde van AMQP                                                                                                                                                |
| lange                           | lange                                      | Waarde van AMQP                                                                                                                                                |
| Float                          | Float                                     | Waarde van AMQP                                                                                                                                                |
| dubbel                         | dubbel                                    | Waarde van AMQP                                                                                                                                                |
| decimaal                        | decimal128                                | Waarde van AMQP                                                                                                                                                |
| CHAR                           | CHAR                                      | Waarde van AMQP                                                                                                                                                |
| Datum/tijd                       | tijdstempel                                 | Waarde van AMQP                                                                                                                                                |
| GUID                           | UUID                                      | Waarde van AMQP                                                                                                                                                |
| byte]                         | binair                                    | Waarde van AMQP                                                                                                                                                |
| tekenreeks                         | tekenreeks                                    | Waarde van AMQP                                                                                                                                                |
| System.Collections.IList       | lijst                                      | Waarde van AMQP: de items in de collectie kunnen alleen worden die in deze tabel zijn gedefinieerd.                                                             |
| System.Array                   | matrix                                     | Waarde van AMQP: de items in de collectie kunnen alleen worden die in deze tabel zijn gedefinieerd.                                                             |
| System.Collections.IDictionary | kaart                                       | Waarde van AMQP: de items in de collectie kunnen alleen worden die in deze tabel zijn gedefinieerd. Opmerking: alleen tekenreekssleutels worden ondersteund.                        |
| URI                            | String beschreven (Zie de volgende tabel) | Waarde van AMQP                                                                                                                                                |
| DateTimeOffset                 | Lange beschreven (Zie de volgende tabel)   | Waarde van AMQP                                                                                                                                                |
| TimeSpan                       | Lange beschreven (Zie hieronder)         | Waarde van AMQP                                                                                                                                                |
| Stream                         | binair                                    | Gegevens van AMQP (mogelijk meerdere). De secties gegevens bevatten de onbewerkte bytes van het Stream-object gelezen.                                                           |
| Andere objecten                   | binair                                    | Gegevens van AMQP (mogelijk meerdere). Bevat het serienummer binaire bestand van het object dat de DataContractSerializer of een serializer geleverd door de toepassing wordt gebruikt. |

| .NET-type      | Toegewezen AMQP beschreven Type                                                                                              | Notities                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| TimeSpan       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Niet-ondersteunde functies en beperkingen doorgevoerd verschillen

De volgende functies van de Service Bus .NET API worden momenteel niet ondersteund bij gebruik van AMQP:

-   Transacties

-   Verzenden via overdracht bestemming

Er zijn ook enkele kleine verschillen in het gedrag van de Bus .NET API-Service bij het gebruik van AMQP, vergeleken met de standaard-protocol:

-   De eigenschap [OperationTimeout][] wordt genegeerd.

-   `MessageReceiver.Receive(TimeSpan.Zero)`geïmplementeerd als `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Berichten voltooien door tokens vergrendeling kan alleen worden uitgevoerd door de ontvangers van berichten die in eerste instantie de berichten te ontvangen.

## <a name="controlling-amqp-protocol-settings"></a>AMQP protocol-instellingen beheren

De API's van .NET verschillende instellingen voor de configuratie van het protocol van AMQP worden blootgesteld:

-   **MessageReceiver.PrefetchCount**: Hiermee bepaalt u het oorspronkelijke krediet toegepast op een koppeling. De standaardwaarde is 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: besturingselementen die de maximale framegrootte van AMQP tijdens de onderhandeling bij de verbinding aangeboden opnieuw wordt geopend. De standaardwaarde is 65.536 bytes.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: als de overdracht van batchable, deze waarde bepaalt de maximale vertraging voor het verzenden van dispositions. Door afzenders/ontvangers overgenomen standaard. Afzonderlijke verzender/ontvanger kunt negeren de standaardwaarde 20 milliseconden is.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: Hiermee bepaalt u of AMQP verbindingen tot stand worden gebracht via een SSL-verbinding. De standaardwaarde is **true**.

## <a name="next-steps"></a>Volgende stappen

Wilt u meer informatie? Ga naar de volgende koppelingen:

- [Overzicht Service Bus AMQP]
- [AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen]
- [AMQP in Bus Service voor Windows Server]

  [Aan de slag met wachtrijen Service Bus]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Overzicht Service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in Bus Service voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
