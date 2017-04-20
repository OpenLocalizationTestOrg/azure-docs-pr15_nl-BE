<properties 
    pageTitle="Azure wachtrijen en Service Bus wachtrijen - vergeleken en tegenstelling tot | Microsoft Azure"
    description="Analyseert de verschillen en overeenkomsten tussen twee typen wachtrijen die worden aangeboden via Azure."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure wachtrijen en Service Bus wachtrijen - vergeleken en tegenstelling tot

In dit artikel analyseert de verschillen en overeenkomsten tussen de twee typen wachtrijen die worden aangeboden door Microsoft Azure vandaag: Azure wachtrijen en Service Bus wachtrijen. Met behulp van deze informatie kunt u vergelijken en contrast van de desbetreffende technologieën en tot een beter geïnformeerde beslissing nemen over welke oplossing het beste aan uw behoeften voldoet.

## <a name="introduction"></a>Inleiding

Microsoft Azure ondersteunt twee soorten mechanismen wachtrij: **Azure wachtrijen** en **Wachtrijen met Service Bus**.

**Azure wachtrijen**, die deel uitmaken van de infrastructuur voor de [opslag van Azure](https://azure.microsoft.com/services/storage/) , uitgerust met een eenvoudige interface op basis van REST Get/Put/Peek, biedt betrouwbare, permanente messaging binnen en tussen services.

**Service Bus wachtrijen** zijn onderdeel van een bredere [Azure messaging](https://azure.microsoft.com/services/service-bus/) -infrastructuur die ondersteunt queuing publiceren/abonneren, Web service remoting en integratie van patronen. Zie voor meer informatie over Service Bus wachtrijen, onderwerpen/abonnementen en relais, [overzicht van messaging Service Bus](service-bus-messaging-overview.md).

Terwijl beide technologieën queuing gelijktijdig bestaan, zijn wachtrijen Azure eerst als een opslagmechanisme voor specifieke wachtrij gebaseerd op de Azure storage-services geïntroduceerd. Service Bus wachtrijen zijn gebaseerd op de bredere 'brokered messaging' infrastructuur ontworpen voor integratie van toepassingen of onderdelen waarover meerdere communicatieprotocollen, gegevenscontracten, domeinen vertrouwen en/of omgevingen.

Dit artikel vergelijkt de twee wachtrij technologieën van Azure door het bespreken van de verschillen in het gedrag en de uitvoering van de functies die door elk. Het artikel bevat ook instructies voor het kiezen van welke functies mogelijk wens application development.

## <a name="technology-selection-considerations"></a>Overwegingen bij het selecteren van de technologie

Zowel de wachtrijen Azure Service Bus wachtrijen zijn implementaties van de message Queuing-service op dit moment worden aangeboden op Microsoft Azure. Elk heeft een iets andere functieset, wat betekent kiezen een of beide, afhankelijk van de behoeften van zakelijke en technische probleem u het oplossen van bepaalde oplossing of gebruiken.

Bij het bepalen van welke queuing technologie past het doel voor een bepaalde oplossing, oplossing, architecten en ontwikkelaars moeten rekening houden met onderstaande aanbevelingen. Zie de volgende sectie voor meer informatie.

Als solution architect/ontwikkelaar, **moet u overwegen Azure wachtrijen** wanneer:

- Uw toepassing moet opslaan van meer dan 80 GB aan berichten in een wachtrij, waar de berichten een levensduur van minder dan 7 dagen hebben.

- Uw toepassing wil bijhouden voor het verwerken van een bericht in de wachtrij wordt verwerkt. Dit is handig als de werknemer verwerken van een bericht vastloopt. Een latere werknemer kunt die informatie om door te gaan vanaf waar de werknemer vooraf gebleven was.

- U moet kant serverlogboeken van alle transacties die worden uitgevoerd op uw wachtrijen.

Als solution architect/ontwikkelaar, **moet u overwegen de wachtrijen Service Bus** wanneer:

- De oplossing moet worden berichten kan ontvangen zonder te vragen voor de wachtrij. Met de Service Bus, dit kan worden bereikt door het gebruik van de lange-polling bewerking met behulp van het TCP-protocollen Bus-Service ondersteunt ontvangen.

- Uw oplossing is vereist voor de wachtrij voor een gegarandeerde first-in-first-out (FIFO) besteld levering.

- U wilt een symmetrische ervaring in Azure en Windows Server (private cloud). Zie [Service Bus voor Windows Server](https://msdn.microsoft.com/library/dn282144.aspx)voor meer informatie.

- De oplossing moet kunnen ondersteuning voor automatische detectie van dubbele.

- U wilt dat uw toepassing op berichten als parallelle stromen van langdurige (berichten zijn gekoppeld aan een stroom met behulp van de eigenschap [SessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) in het bericht). In dit model concurreert op elk knooppunt in het beslag toepassing voor gegevensstromen in plaats van berichten. Wanneer een gegevensstroom wordt gegeven aan een knooppunt in beslag nemen, kan het knooppunt de status van de stream toepassingsstatus transacties kunt bekijken.

- Uw oplossing is vereist voor transactionele gedrag en atomiciteit bij het verzenden of ontvangen van meerdere berichten uit een wachtrij.

- De time-to-live (TTL) kenmerk van de werkbelasting voor specifieke applicaties kan overschrijden de periode van 7 dagen.

- Uw toepassing berichten verwerkt die langer zijn dan 64 KB, maar zal waarschijnlijk niet zullen aanpak 256 KB beperken.

- U werkt met een vereiste is om een model op basis van een rol aan de wachtrijen en andere rechten machtigingen gelden voor de afzenders en ontvangers.

- De grootte van de wachtrij groeit niet groter zijn dan 80 GB.

- U kunt de AMQP op standaarden gebaseerde messaging protocol 1.0 gebruiken. Zie voor meer informatie over AMQP [Service Bus AMQP overzicht](./service-bus-amqp-overview.md).

- U kunt een uiteindelijke migratie van op basis van wachtrijen point-to-point communicatie met een message exchange patroon waarmee de naadloze integratie van extra ontvangers (abonnees), die elk onafhankelijk kopieën van sommige of alle berichten die worden verzonden naar de wachtrij ontvangt voor ogen hebt. Deze verwijst naar de mogelijkheid publiceren/abonneren worden geleverd door de Service Bus.

- Uw oplossing voor expresberichten moeten kunnen ondersteunen de leveringszekerheid 'Meest-eens' zonder dat u de onderdelen van de aanvullende infrastructuur te bouwen.

- U wilt kunnen publiceren en batches van berichten in beslag nemen.

- U moet de volledige integratie met de stack van Windows Communication Foundation (WCF) communicatie in het .NET Framework.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Azure wachtrijen en Service Bus wachtrijen vergelijken

De tabellen in de volgende secties bieden een logische groepering van de wachtrij en kunnen u vergelijken in één oogopslag de mogelijkheden die beschikbaar zijn in zowel Azure-wachtrijen en wachtrijen met Service Bus.

## <a name="foundational-capabilities"></a>Moeten beschikken over mogelijkheden

In deze sectie worden enkele van de fundamentele queuing mogelijkheden van Azure wachtrijen en Service Bus wachtrijen vergeleken.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Garantie te bestellen|**Nee** <br/><br>Zie de eerste opmerking in de sectie 'Meer informatie' voor meer informatie.</br>|**Ja - First In First Out (FIFO)**<br/><br>(met behulp van messaging sessies)|
|Garantie van bezorging|**In de minst eens**|**In de minst eens**<br/><br/>**In de meeste eens**|
|Atomaire bewerking ondersteuning|**Nee**|**Ja**<br/><br/>|
|Gedrag ontvangen|**Niet-blokkerende**<br/><br/>(uitgevoerd direct als er geen nieuw bericht is gevonden)|**Met of zonder time-out blokkeren**<br/><br/>(aanbiedingen lange navragen of de ["Komeet techniek"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Niet-blokkerende**<br/><br/>(met behulp van .NET managed API alleen)|
|API voor push-stijl|**Nee**|**Ja**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) en **OnMessage** sessies .NET API.|
|Modus ontvangen|**Peek & Lease**|**Peek & vergrendelen**<br/><br/>**Ontvangen & verwijderen**|
|Exclusieve toegangsmodus|**Op basis van lease**|**Op basis van vergrendeling**|
|Duur, lease/vergrendelen|**30 seconden (standaard)**<br/><br/>**7 dagen (maximaal)** (U kunt vernieuwen of vrijgeven van de lease van een bericht met behulp van de [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API).|**60 seconden (standaard)**<br/><br/>U kunt een bericht slot met behulp van de [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API verlengen.|
|Lease/precision-Lock|**Berichtniveau**<br/><br/>(elk bericht kan hebben een andere time-outwaarde, die u vervolgens naar wens bijwerken kunt tijdens het verwerken van het bericht met behulp van de [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API)|**Wachtrijniveau**<br/><br/>(elke wachtrij heeft een lock precisie toegepast op alle van de berichten, maar u kunt de vergrendeling met behulp van de [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API verlengen.)|
|Batch verwerkt ontvangen|**Ja**<br/><br/>(expliciet op te geven aantal berichten bij het ophalen van berichten met een maximum van 32 berichten)|**Ja**<br/><br/>(inschakelen van een eigenschap ophalen vóór impliciet of expliciet via transacties)|
|Batch verzenden|**Nee**|**Ja**<br/><br/>(met behulp van transacties of batchen van client-side)|

### <a name="additional-information"></a>Als u meer informatie

- Berichten in wachtrijen Azure zijn meestal first-in-first-out, maar soms kunnen worden geplaatst; bijvoorbeeld als duur van time-out zichtbaarheid van een bericht verloopt (bijvoorbeeld als gevolg van een clienttoepassing vastlopen tijdens de verwerking). Wanneer de zichtbaarheid time-out is verstreken, wordt het bericht opnieuw in de wachtrij voor een andere werknemer aan deze wachtrij zichtbaar. Op dat punt kan het bericht pas zichtbaar in de wachtrij (worden uit wachtrij opnieuw geplaatst) worden geplaatst nadat een bericht dat oorspronkelijk in wachtrij werd na.

- Als u al met Azure opslag BLOB's of tabellen en u start met behulp van wachtrijen, krijg je gegarandeerd 99,9% beschikbaarheid. Als u BLOB's of tabellen met wachtrijen Service Bus gebruikt, hebt u lagere beschikbaarheid.

- De gegarandeerde FIFO patroon in wachtrijen Service Bus vereist het gebruik van de messaging-sessies. In het geval dat de toepassing vastloopt tijdens het verwerken van een bericht in de modus **Peek & Lock** ontvangen, wordt de volgende keer dat de ontvanger van een wachtrij een messaging-sessie accepteert gestart met het bericht is mislukt na het verstrijken van de time-to-live (TTL) is verstreken.

- Azure wachtrijen zijn ontworpen voor ondersteuning van standaard queuing scenario's, zoals toepassingsonderdelen ontkoppeling verhogen schaalbaarheid en tolerantie voor fouten, laden herverdeling en proces werkstromen te bouwen.

- Service Bus wachtrijen ondersteunen de leveringszekerheid *In de minst eens* . Bovendien kunnen *In de meeste eens* semantische worden ondersteund met behulp van de sessiestatus voor het opslaan van de status van de toepassing en met behulp van transacties atomically berichten ontvangen en bijwerken van de status van de sessie.

- Azure wachtrijen voorzien in een uniform en consistent programmeermodel wachtrijen, tabellen en BLOB's – zowel voor ontwikkelaars en operations teams.

- Wachtrijen voor Bus-service bieden ondersteuning voor lokale transacties in het kader van één wachtrij.

- **Ontvangen en verwijder** -modus ondersteund door de Service Bus biedt de mogelijkheid te verminderen messaging item (en de bijbehorende kosten) in ruil voor de levering van verlaagde zekerheid.

- Leases bieden Azure wachtrijen de mogelijkheid om uit te breiden de leases voor berichten. Hierdoor kunnen de werknemers te handhaven korte leases op berichten. Dus als een werknemer vastloopt, kan het bericht snel opnieuw worden verwerkt door een andere werknemer. Daarnaast kan een werknemer de lease op een bericht uitbreiden als deze langer is dan de huidige leasetijd moeten verwerken.

- Azure wachtrijen bieden een zichtbaarheid-out die u kunt instellen op de enqueueing of waarbij van een bericht. U kunt ook een bericht bijwerken met afwijkende lease-waarden tijdens de uitvoering en verschillende waarden bijwerken op berichten in de wachtrij met dezelfde. Service Bus lock-outs zijn gedefinieerd in de metagegevens van de wachtrij. u kunt de vergrendeling echter verlengen door het aanroepen van de methode [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) .

- De maximale time-out is voor bewerking in wachtrijen Service Bus een blokkering ontvangen 24 dagen. Time-outs voor REST hebben echter een maximale waarde van 55 seconden.

- Client-side batchen geleverd door Service Bus, kunnen clients wachtrij om meerdere berichten in een enkele verzendbewerking batch te verwerken. Batchverwerking is alleen beschikbaar voor het verzenden van asynchrone bewerkingen.

- Functies zoals het maximum 200 TB van Azure wachtrijen (meer als accounts virtualiseren) en onbeperkte wachtrijen maken het een ideaal platform voor SaaS-aanbieders.

- Azure wachtrijen bieden een flexibele en zodat overgedragen mechanisme voor toegangsbeheer.

## <a name="advanced-capabilities"></a>Geavanceerde mogelijkheden

In deze sectie worden geavanceerde mogelijkheden van Azure wachtrijen en Service Bus wachtrijen vergeleken.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Geplande levering|**Ja**|**Ja**|
|Automatische dode letters|**Nee**|**Ja**|
|Wachtrij-TTL-waarde verhogen|**Ja**<br/><br/>(via de update op de plaats van zichtbaarheid time-out)|**Ja**<br/><br/>(geleverd via een speciale API-functie)|
|Ondersteuning voor message verontreinigen|**Ja**|**Ja**|
|In-place update|**Ja**|**Ja**|
|Transactielogboek van server-side|**Ja**|**Nee**|
|Opslag metrics|**Ja**<br/><br/>**Minuut Metrics**: biedt real-time metrics voor beschikbaarheid, TPS, API aanroepen telt, telt het aantal fout en meer in realtime (per minuut worden samengevoegd en gerapporteerd binnen enkele minuten uit wat net is er gebeurd in de productie. Zie voor meer informatie [Over opslag Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Ja**<br/><br/>(bulk-query's door het aanroepen van [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Statusbeheer|**Nee**|**Ja**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Automatisch doorsturen van berichten|**Nee**|**Ja**|
|Functie van de wachtrij wissen|**Ja**|**Nee**|
|Bericht-groepen|**Nee**|**Ja**<br/><br/>(met behulp van messaging sessies)|
|De status van de toepassing per groep bericht|**Nee**|**Ja**|
|Opsporing van dubbele contactpersonen|**Nee**|**Ja**<br/><br/>(aan de kant van de afzender te configureren)|
|WCF-integratie|**Nee**|**Ja**<br/><br/>(biedt out-of-the-box WCF bindingen)|
|WF-integratie|**Aangepaste**<br/><br/>(vereist voor het bouwen van een aangepaste activiteit voor WF)|**Native**<br/><br/>(biedt out-of-the-box WF activiteiten)|
|Groepen berichten bladeren|**Nee**|**Ja**|
|Ophalen van berichten uitwisselen op ID|**Nee**|**Ja**|

### <a name="additional-information"></a>Als u meer informatie

- Beide queuing technologieën kunnen een bericht moet worden gepland voor levering op een later tijdstip.

- Wachtrij automatisch doorsturen kan duizenden wachtrijen automatisch doorsturen van hun berichten naar één wachtrij, waarvan de ontvangende toepassing het bericht gebruikt. U kunt dit mechanisme een beveiliging, Transportbesturing gebruiken en opslag tussen elke uitgever bericht isoleren.

- Azure wachtrijen bieden ondersteuning voor het bijwerken van de inhoud van het bericht. U kunt deze functionaliteit voor persistent maken voor informatie over de status en van incrementele voortgangsupdates in het bericht zodat deze kan worden verwerkt van de laatste bekende checkpoint in plaats van een geheel nieuwe publicatie. Met wachtrijen Service Bus kunt u berichten uitwisselen via het gebruik van hetzelfde scenario. Sessies kunnen u opslaan en ophalen van de verwerkingsstatus van toepassing (met behulp van [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) en [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)).

- [Dode letters](service-bus-dead-letter-queues.md)die alleen wordt ondersteund door de Service Bus wachtrijen, is handig voor het isoleren van berichten die niet kunnen worden verwerkt door de ontvangende toepassing of wanneer berichten hun bestemming als gevolg van een eigenschap verlopen time-to-live (TTL) kunnen bereiken. De TTL-waarde geeft aan hoe lang een bericht blijft in de wachtrij. Met Bus-Service, wordt het bericht verplaatst naar een speciale wachtrij $DeadLetterQueue aangeroepen wanneer de TTL zijn verstreken.

- "Poison" om berichten te zoeken in Azure wachtrijen, wanneer een bericht waarbij de toepassing controleert de eigenschap **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** van het bericht. Als **DequeueCount** dan een bepaalde drempel is, wordt de toepassing het bericht verplaatst naar een wachtrij toepassing gedefinieerd 'onbestelbaar'.

- Azure wachtrijen kunnen u een gedetailleerd logboek van alle transacties die worden uitgevoerd op de wachtrij, als ook als geaggregeerde statistieken te verkrijgen. Beide opties zijn nuttig voor foutopsporing en het begrip van hoe uw toepassing gebruikmaakt van Azure wachtrijen. Ze zijn ook handig voor-prestaties optimaliseren uw toepassing en de kosten van het gebruik van wachtrijen.

- Het begrip "bericht sessies' worden ondersteund door de Service Bus kan berichten die bij een logische groep behoren moet worden gekoppeld aan een bepaalde ontvanger, die op zijn beurt een sessie-achtige affiniteit tussen berichten en hun respectieve ontvangers maakt. Deze geavanceerde functies in Service Bus kunt u door de [sessie-id](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) -eigenschap van een bericht. Ontvangers kunnen vervolgens luisteren op een specifieke sessie-ID en ontvangen van berichten die delen van de opgegeven sessie-id.

- De functionaliteit voor duplicatie detectie automatisch worden ondersteund door de Service Bus wachtrijen verwijdert dubbele berichten worden verzonden naar een wachtrij of onderwerp, op basis van de waarde van de eigenschap [MessageID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) .

## <a name="capacity-and-quotas"></a>Capaciteit en quota

In deze sectie worden vergeleken wachtrijen wachtrijen Azure en Service Bus vanuit het perspectief van de [capaciteit en de quota's](service-bus-quotas.md) die mogelijk van toepassing.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Maximale grootte|**200 TB**<br/><br/>(beperkt tot een enkele account opslagcapaciteit)|**1 GB, 80 GB**<br/><br/>(gedefinieerd bij het maken van een wachtrij en het [inschakelen van partitionering](service-bus-partitioning.md) – Zie de sectie 'Meer informatie')|
|Maximale berichtgrootte|**64 KB**<br/><br/>(48 KB wanneer met **Base64** -codering)<br/><br/>Azure ondersteunt grote berichten door de combinatie van wachtrijen en BLOB's – op dat moment u plaatsen kunt tot 200GB voor een enkel item.|**256 KB** of **1 MB**<br/><br/>(met inbegrip van zowel de koptekst en de hoofdtekst, maximale header-grootte: 64 KB).<br/><br/>Afhankelijk van het [niveau van de service](service-bus-premium-messaging.md).|
|Maximale bericht TTL|**7 dagen**|**`TimeSpan.Max`**|
|Maximum aantal wachtrijen|**Onbeperkt**|**10.000**<br/><br/>(per Servicenaamruimte kan worden verhoogd)|
|Maximum aantal gelijktijdige clients|**Onbeperkt**|**Onbeperkt**<br/><br/>(limiet van 100 gelijktijdige verbindingen alleen van toepassing op TCP-protocol-communicatie)|

### <a name="additional-information"></a>Als u meer informatie

- Bus service zorgt ervoor dat de maximale grootte wachtrij. De maximale grootte kan wordt opgegeven bij het maken van de wachtrij en een waarde tussen 1 en 80 GB. Als de waarde voor de wachtrij instellen voor het maken van de wachtrij is bereikt, extra inkomende berichten geweigerd en wordt een uitzondering wordt ontvangen door de aanroepende code. Zie voor meer informatie over quota's in de Bus Service [Service Bus quota](service-bus-quotas.md).

- U kunt de Service Bus wachtrijen maken in 1, 2, 3, 4 of 5 GB formaten (de standaardinstelling is 1 GB). Met partitionering ingeschakeld (dit is de standaardinstelling), Service Bus 16 partities maakt voor elke GB die u opgeeft. Als zodanig, als u een wachtrij die is 5 GB groot maakt, is door de partities van 16 de maximale grootte (5 * 16) = 80 GB. Door te kijken naar de vermelding op de [Azure portal][]ziet u de maximale grootte van de wachtrij van gepartitioneerde of onderwerp.

- Met Azure wachtrijen, als de inhoud van het bericht geen XML-veilige, zijn moet deze **Base64** -gecodeerde. Als u een **Base64**-coderen van het bericht, de nettolading van de gebruiker kunt u maximaal 48 KB, in plaats van 64 KB.

- Met Service Bus wachtrijen, elk bericht in een wachtrij opgeslagen bestaat uit twee delen: een kop en een instantie. De totale grootte van het bericht kan niet groter zijn dan de maximale berichtgrootte die worden ondersteund door de servicelaag.

- Wanneer clients met wachtrijen Bus Service via het TCP-protocol communiceren, is het maximum aantal gelijktijdige verbindingen met één wachtrij Service Bus beperkt tot 100. Dit getal wordt gedeeld tussen verzenders en ontvangers. Dit quotum is bereikt, wordt de volgende aanvragen voor extra verbindingen worden geweigerd als een uitzondering wordt ontvangen door de aanroepende code. Deze limiet niet ingesteld op clients die verbinding maken met de REST gebaseerde API met wachtrijen.

- U kunt als u meer dan 10.000 wachtrijen in één naamruimte Bus Service nodig hebt, neem contact op met het ondersteuningsteam van Azure en vragen een verhoging. Als u wilt schalen dan 10.000 wachtrijen met Bus-Service, kunt u aanvullende naamruimten met de [Azure portal][]maken.

## <a name="management-and-operations"></a>Beheer en de exploitatie

In deze sectie worden de beheerfuncties van wachtrijen wachtrijen Azure en Service Bus vergeleken.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Management-protocol|**HTTP/HTTPS-boven**|**PLAATS via HTTPS**|
|Runtime-protocol|**HTTP/HTTPS-boven**|**PLAATS via HTTPS**<br/><br/>**AMQP 1.0 standaard (TCP met TLS)**|
|.NET managed API|**Ja**<br/><br/>(.NET managed Client-API opslag)|**Ja**<br/><br/>(Beheerde brokered messaging API)|
|Native C++|**Ja**|**Nee**|
|Java API|**Ja**|**Ja**|
|PHP-API|**Ja**|**Ja**|
|Node.js API|**Ja**|**Ja**|
|Ondersteuning voor willekeurige metagegevens|**Ja**|**Nee**|
|Regels voor naamgeving van wachtrij|**Maximaal 63 tekens lang**<br/><br/>(Letters in de naam van de wachtrij moet in kleine letters.)|**Maximaal 260 tekens lang**<br/><br/>(Namen en paden van de wachtrij zijn niet hoofdlettergevoelig.)|
|De functie lengte wachtrij ophalen|**Ja**<br/><br/>(Geschatte waarde als berichten na de TTL verlopen zonder wordt verwijderd.)|**Ja**<br/><br/>(Exacte punt in tijd waarde.)|
|Functie van Peek|**Ja**|**Ja**|

### <a name="additional-information"></a>Als u meer informatie

- Azure wachtrijen bieden ondersteuning voor willekeurige kenmerken die kunnen worden toegepast op de omschrijving van de wachtrij, in de vorm van naam/waarde-paren.

- Beide technologieën wachtrij bieden de mogelijkheid om een bericht bekijken zonder te vergrendelen, dat kan handig zijn bij het implementeren van een explorer-browser wachtrij hulpprogramma.

- De Bus Service .NET brokered messaging API hefboomwerking full-duplex TCP-verbindingen voor hogere prestaties in vergelijking met de REST via HTTP en ondersteuning van de standaard AMQP 1.0-protocol.

- Namen van Azure wachtrijen kan 3-63 tekens lang, kan bevatten kleine letters, cijfers en streepjes. Zie [namen van wachtrijen en metagegevens](https://msdn.microsoft.com/library/azure/dd179349.aspx)voor meer informatie.

- Wachtrijnamen Service Bus kunnen 260 tekens lang zijn en minder beperkende regels voor naamgeving. Wachtrijnamen Service Bus kunnen letters, cijfers, punten, koppeltekens en onderstrepingstekens bevatten.

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie

In dit gedeelte wordt de verificatie en autorisatie functies wordt ondersteund door de Service Bus-Azure wachtrijen en wachtrijen beschreven.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Verificatie|**Symmetrische sleutel**|**Symmetrische sleutel**|
|Beveiligingsmodel|Gedelegeerde toegang via SAS-tokens.|SAS|
|Identity provider federation|**Nee**|**Ja**|

### <a name="additional-information"></a>Als u meer informatie

- Elke aanvraag van een van de queuing technologieën moet worden geverifieerd. Openbare wachtrijen met anonieme toegang worden niet ondersteund. [SAS](service-bus-sas-overview.md)kunt u dit scenario adres door het publiceren van een alleen-schrijven SAS, SAS alleen-lezen of zelfs een volledige toegang SAS.

- De verificatiemethode die door wachtrijen Azure heeft betrekking op het gebruik van een symmetrische sleutel, een hash-based Message Authentication Code (HMAC) wordt, met het SHA-256 algoritme berekend en gecodeerd als een **Base64** -tekenreeks. Zie [verificatie voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dd179428.aspx)voor meer informatie over het respectieve protocol. Service Bus wachtrijen ondersteuning voor een vergelijkbaar model met symmetrische sleutels. Zie [Gedeelde handtekening verificatie met Bus Service](service-bus-shared-access-signature-authentication.md)voor meer informatie.

## <a name="cost"></a>Kosten

In deze sectie worden vergeleken wachtrijen wachtrijen Azure en Service Bus vanuit financieel oogpunt.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Wachtrij transactiekosten|**$0.0036**<br/><br/>(per 100.000 transacties)|**Fundamentele niveaus**: **$0,05**<br/><br/>(per miljoen bewerkingen)|
|Factureerbare transacties.|**Alle**|**Verzenden/ontvangen alleen**<br/><br/>(gratis voor andere verrichtingen)|
|Niet-actieve transacties|**Te factureren**<br/><br/>(Een lege wachtrij zoeken telt als een transactie factureerbare.)|**Te factureren**<br/><br/>(Een ontvangen tegen een lege wachtrij wordt beschouwd als een factureerbare bericht.)|
|Kosten opslag|**$0,07**<br/><br/>(per GB/maand)|**fl 0,00**|
|Kosten van uitgaande gegevensoverdracht|**$0,12 - $0.19**<br/><br/>(Afhankelijk van de geografische.)|**$0,12 - $0.19**<br/><br/>(Afhankelijk van de geografische.)|

### <a name="additional-information"></a>Als u meer informatie

- Gegevensoverdracht in rekening worden gebracht op basis van het totale bedrag van gegevens zonder de Azure datacenters via het internet in een bepaalde betalingsperiode.

- Gegevensoverdracht tussen Azure services zich bevinden in hetzelfde gebied vallen niet onder de kostprijs.

- Schrijven van dit, alle overdrachten van inkomende gegevens worden niet gratis.

- Gegeven de ondersteuning voor lange polling, kan met Service Bus wachtrijen worden voordelige in situaties waarin de levering lage latentie vereist is.

>[AZURE.NOTE] Alle kosten worden gewijzigd. Deze tabel geeft de huidige prijzen en bevat niet alle aanbiedingen die momenteel beschikbaar zijn. Zie de pagina [Azure prijzen](https://azure.microsoft.com/pricing/) voor actuele informatie over de prijzen Azure. Voor meer informatie over Service Bus prijzen Zie [prijzen Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="conclusion"></a>Conclusie

Door een beter begrip van deze twee technologieën, is het mogelijk om een beter geïnformeerde beslissing nemen over welke wachtrij-technologie te gebruiken, en wanneer. De beslissing over het gebruik van wachtrijen wachtrijen of Azure Service Bus is duidelijk een aantal factoren afhankelijk. Deze factoren kunnen sterk afhankelijk zijn van de specifieke behoeften van uw toepassing en de architectuur. Als al uw toepassing gebruikmaakt van de kernfuncties van Microsoft Azure, kunt u desgewenst kiezen Azure wachtrijen, vooral als u eenvoudige communicatie en messaging tussen services of noodzaak wachtrijen die groter zijn dan 80 GB in grootte.

Omdat wachtrijen Service Bus een aantal geavanceerde functies, zoals de sessies, transacties bieden, detectie, automatische dubbele mogelijkheden dode letters en duurzaam publiceren/abonneren, kunnen ze zijn een uitstekende keuze als u een toepassing hybride bouwt of als uw toepassing vereist is voor deze functies.

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen bevatten meer advies en informatie over het gebruik van wachtrijen wachtrijen of Azure Service Bus.

- [Het gebruik van wachtrijen Bus](service-bus-dotnet-get-started-with-queues.md)
- [Het gebruik van de wachtrij Storage-Service](../storage/storage-dotnet-how-to-use-queues.md)
- [Aanbevolen procedures voor een Service Bus met prestatieverbeteringen brokered messaging](service-bus-performance-improvements.md)
- [Introductie van wachtrijen en onderwerpen in Azure Service Bus](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [De Developer's Guide to Service Bus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Met behulp van de wachtrijservice in Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Wat is Azure opslag facturering: bandbreedte, transacties en capaciteit?](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure portal]: https://portal.azure.com
 
