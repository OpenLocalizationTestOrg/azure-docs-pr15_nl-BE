<properties 
    pageTitle="Azure Relay hybride verbindingen Protocol | Microsoft Azure"
    description="zure Relay hybride verbindingen Protocol Guide."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure Relay hybride verbindingen-Protocol

Azure Relay is een van de pijlers van de belangrijkste mogelijkheden van het platform Azure Service Bus. De nieuwe 'Hybride verbindingen'-mogelijkheid van de Relay is een open protocol, veilige ontwikkeling op basis van HTTP- en WebSockets.

Vervangt de voormalige, evenredig met de naam 'BizTalk-Services'-functie die is gebaseerd op een eigen protocol foundation. De integratie van hybride verbindingen in Azure App Services blijven als-is.

'Hybride verbindingen' kunnen tot stand brengen van communicatie tussen twee netwerktoepassingen, waarbij een of beide van de partijen zich achter NAT's of Firewalls bevinden kunnen bi-directionele, binaire stream.

Dit document beschrijft de client-side interacties met de relay hybride verbindingen bij verbindingen met clients in listener en afzender rollen en hoe listeners nieuwe verbindingen geaccepteerd.

## <a name="interaction-model"></a>Interactie-model

De relay hybride verbindingen verbindt twee partijen door middel van een punt van rendezvous in de Azure cloud die beide partijen kunnen ontdekken en vanuit het perspectief van hun eigen netwerk verbinding maken met. Dat punt rendezvous heet 'Hybride verbinding' in deze en andere documentatie, in de API's, en ook in de Portal Azure. De service-eindpunt hybride verbindingen zal worden verwezen als "service" voor de rest van dit document.

De interactie model leans van de nomenclatuur die door veel andere API met netwerk:

Er is een listener die eerst geeft de gereedheid voor het verwerken van binnenkomende verbindingen en vervolgens accepteert bij aankomst. Aan de andere kant is er een client die verbinding maakt die verbinding de listener maakt, verwacht de verbinding voor het tot stand brengen van een pad bidirectionele communicatie wordt geaccepteerd. 'Verbinding maken', 'Luisteren', 'Accepteren' zijn dezelfde voorwaarden u op de meeste socket API's vindt.

Een indirecte communicatiemodel is een van de partijen uitgaande verbindingen naar een service-eindpunt dat wordt 'de listener' ook een 'client' gewone gebruikt en mogelijk ook andere overbelastingen terminologie; de exacte terminologie we dus bij hybride verbindingen gebruiken is als volgt:

De programma's aan beide zijden van een verbinding worden genoemd 'client', aangezien deze clients met de service. De client wacht en verbindingen geaccepteerd is "de listener" of "listener rol" worden genoemd. De client die verbinding maakt nieuwe naar een listener die via de service wordt genoemd de "verzender" of in de functie"afzender".

## <a name="listener-interactions"></a>Listener interacties

De listener heeft vier interacties met de service; alle details van de draad worden verderop in dit document in de sectie beschreven.

### <a name="listen"></a>Luisteren

Om aan te geven van de gereedheid voor de service die een listener is gereed voor het accepteren van verbindingen, wordt een socketverbinding uitgaande web. De handshake verbinding, wordt de naam van een hybride-verbinding geconfigureerd in de Relay-naamruimte en een beveiligingstoken dat rechts op de "Listen" verleent die een naam geven. Wanneer de web-socket is geaccepteerd door de service, de registratie is voltooid en de socket gevestigde web als "opdrachtkanaal" voor het inschakelen van alle opeenvolgende interacties in leven wordt gehouden. De service kunnen maximaal 25 gelijktijdige luisteraars op een hybride verbinding. Als er 2 of meer actieve luisteraars, binnenkomende verbindingen worden in evenwicht gebracht over deze in willekeurige volgorde; eerlijke verdeling is niet gegarandeerd.

### <a name="accept"></a>Accepteren

Wanneer een afzender een nieuwe verbinding voor de service opent, wordt de service kiezen en één van de actieve listeners op de hybride verbinding in kennis. De melding is verzonden naar de listener via open opdrachtkanaal JSON met de URL van het eindpunt van Web socket die de listener voor het accepteren van de verbinding verbinding moet maken.

De URL kan en moet worden gebruikt door de listener zonder extra werk; de gecodeerde informatie is alleen geldig voor een korte periode, in wezen voor zolang de afzender is bereid te wachten voor de verbinding met het ingestelde end-to-end worden, maar met een maximum van 30 seconden. De URL kan alleen worden gebruikt voor een geslaagde poging. Zodra de socketverbinding Web met de rendezvous-URL is gevestigd, wordt alle verdere activiteiten op deze webpagina socket doorgegeven van en naar de afzender, zonder tussenkomst of interpretatie door de service.

### <a name="renew"></a>Vernieuwen 

Het beveiligingstoken dat moet worden gebruikt voor de listener registreren en onderhouden van het besturingskanaal mogelijk verloopt terwijl de listener actief is. Permanente verbindingen niet van invloed op het verstrijken van de token, maar wordt het besturingskanaal moeten worden weggehaald door de service bij of kort na het moment van verstrijken. De penbeweging voor het 'vernieuwen' is een JSON-bericht dat de listener verzenden kunt naar het token dat is gekoppeld aan het besturingskanaal vervangen zodat het besturingskanaal gedurende langere perioden kan worden beheerd.

### <a name="ping"></a>Ping 

Als het besturingskanaal actief gedurende lange tijd tussenpersonen op de manier waarop blijft, zoals belasting kunnen balancers of NAT-apparaten weghalen de TCP-verbinding. De beweging 'ping' voorkomt dat door een kleine hoeveelheid gegevens op het kanaal dat iedereen op de netwerkroute die bedoeld is als de verbinding in leven en het fungeert ook als een test liveness voor de listener de herinnering te sturen. Als de opdracht ping niet werkt, het besturingskanaal onbruikbaar moet worden beschouwd en de listener moet opnieuw verbinding maken.

## <a name="sender-interaction"></a>Interactie van afzender

De afzender heeft slechts één interactie met de service, de verbinding wordt gemaakt.

### <a name="connect"></a>Verbinding maken

De beweging 'verbinden' opent een webpagina socket op de service, met de naam van de verbinding van de hybride en een (optioneel, maar vereist standaard)-beveiligingstoken dat de machtiging 'Verzenden' in de query-tekenreeks. De service vervolgens de listener op de hierboven beschreven manier in wisselwerking en de listener een rendezvous verbinding maakt die zal worden gekoppeld met deze Web-socket hebben. Nadat de socket Web is geaccepteerd, dus alle verdere interacties op de socket Web wordt met een listener verbonden.

## <a name="interaction-summary"></a>Interactie-overzicht

Het resultaat van deze interactie-model is dat de client van de afzender afkomstig is uit de handshake met een 'schone' Web-socket die is verbonden met een listener en die geen verdere preambles of het preparaat moet. Hierdoor kunnen vrijwel alle bestaande implementatie socket client Web gemakkelijk gebruik te maken van de service hybride verbindingen door een correct geconstrueerde URL opgeven in de weblaag socket-client.

De rendezvous-verbinding Web Socket die de listener via de interactie accepteren verkrijgt ook schoon is en kan worden afgegeven aan een bestaande Web socket serverimplementatie met een minimale extra abstractie die wordt onderscheid tussen 'accepteren'-bewerkingen op het lokale netwerk-listeners van hun framework en hybride-verbindingen 'accepteren' externe bewerkingen gemaakt.

## <a name="protocol-reference"></a>Protocol verwijzing

Deze sectie beschrijft de details van de hierboven beschreven protocol-interacties.

Alle Web-socketverbindingen zijn er op poort 443 als een upgrade van HTTPS 1.1, die vaak door sommige socket Web framework of API beschouwing. Hier de beschrijving blijven uitvoering neutraal, zonder een specifieke kader voorgesteld.

## <a name="listener-protocol"></a>Listener-protocol

De listener-protocol bestaat uit twee bewegingen die verbinding en drie bewerkingen van het bericht.

### <a name="listener-control-channel-connection"></a>Listener control channel-verbinding

Het controle-kanaal wordt geopend met een Web-socketverbinding te maken:

*wss: / / {naamruimte adres} /* *$servicebus* */* *hybridconnection /**{path}? sb-hc-action =... & sb-hc-id =... & sb-hc-token =... "*

De *naamruimte-adres* is de FQDN-naam van de naamruimte Azure Relay die fungeert als host voor de hybride verbinding, meestal van de vorm {*mijnnaam}. servicebus.windows.net.*

De query string parameteropties zijn als volgt

| Param        | Vereist? | Beschrijving                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-hc-actie | Ja       | Voor de rol van de listener de parameter moet **sb-hc-actie listen =**                                                                                                                                |
| {path}       | Ja       | De gecodeerde URL-het naamruimtepad van de vooraf geconfigureerde verbinding hybride deze listener registreren op. Deze expressie wordt toegevoegd aan de vaste * **$servicebus**/**hybridconnection /*** padgedeelte. |
| SB-hc-token  | Ja\*     | De listener moet voorzien in een geldige, door de URL-gecodeerde Service Bus gedeeld Access Token naamruimte of hybride verbinding die het recht **luisteren geeft** .                                           |
| SB-hc-id     | Nee        | Deze client opgegeven optionele ID kunt diagnostische end-to-end-tracering.                                                                                                                             |

Als de Web-socketverbinding omdat het verbindingspad van hybride niet wordt geregistreerd of een token ongeldig of ontbreekt, of een andere fout mislukt, de fout feedback krijgt met de normale HTTP 1.1-status feedback model. Beschrijving van de status bevat een fout tracerings-id kan worden doorgegeven aan de ondersteuning van Azure:

| Code | Fout          | Beschrijving                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Niet gevonden      | De hybride **pad** is ongeldig of de basis-URL is onjuist |
| 401  | Niet-geautoriseerde   | Het beveiligingstoken is ontbreekt of onjuist of ongeldig                  |
| 403  | Verboden      | Het beveiligingstoken is niet geldig voor dit pad voor deze actie          |
| 500  | Interne fout | Er iets is misgegaan in de service                                    |

Als de Web-socketverbinding opzettelijk door de service afgesloten nadat het is in eerste instantie ingesteld, de reden voor het wel doet, worden verstrekt met behulp van een geschikte Web socket protocol foutcode samen met een omschrijving van de foutbericht dat u ook een tracking-id. De service wordt niet op het besturingskanaal afgesloten zonder dat er een fout optreedt. Een afgesloten is client geregeld.

| WS-Status | Beschrijving                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Het verbindingspad hybride is verwijderd of uitgeschakeld.                           |
| 1008      | Het beveiligingstoken is verlopen en is daarom het verificatiebeleid geschonden. |
| 1011      | Iets verkeerd is gegaan binnen de service.                                           |

### <a name="accept-handshake"></a>Handshake accepteren 

De kennisgeving accepteren is verzonden door de service aan de listener via de eerder vastgestelde besturingskanaal als bericht in een tekstframe Web socket JSON. Er is geen antwoord op dit bericht.

Het bericht bevat een JSON-object met de naam 'accepteren' op dit moment de volgende eigenschappen zijn gedefinieerd:

-   **adres** : de URL-reeks voor de vaststelling van de Web-Socket voor de service worden gebruikt voor het accepteren van een binnenkomende verbinding.

-   **id** – de unieke id voor deze verbinding. Als de id is opgegeven door de afzender client, is de waarde van de afzender opgegeven, anders is het systeem gegenereerde waarde.

-   **connectHeaders** – alle HTTP-headers die door de afzender, waaronder ook Sec-WebSocket-Protocol en de headers Sec-WebSocket-uitbreidingen naar het eindpunt van de Relay worden verstrekt.

| Bericht accepteren                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

De URL adres in de JSON-bericht wordt gebruikt door de listener aan de Web-Socket voor accepteren of afwijzen van de afzender socket tot stand brengen.

#### <a name="accepting-the-socket"></a>Acceptatie van de socket

Als u wilt accepteren, de listener een verbinding tot stand WebSocket naar het opgegeven adres.

Er rekening mee dat voor de proefperiode de URI van het adres een bare IP-adres gebruiken kan en de TLS-certificaat geleverd door de server de validatie op dat adres mislukt. Dit wordt rechtgezet worden tijdens de voorvertoning.

Als het bericht 'accepteren' koptekst 'Sec WebSocket Protocol', de verwachting is dat de listener alleen de WebSocket accepteren als het protocol ondersteunt en dat de kop is vastgesteld met de WebSocket wordt ingesteld.

Hetzelfde geldt voor de koptekst 'Sec WebSocket Extensions'. Als het kader een extensie ondersteunt, moet deze de kop ingesteld op het antwoord van de *server* -kant van de vereiste handshake 'Sec WebSocket Extensions' voor de uitbreiding.

De URL moet worden gebruikt als-is voor de vaststelling van de socket accepteren, maar bevat de volgende parameters:

| Param        | Vereist? | Beschrijving                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-hc-actie | Ja       | Voor het accepteren van een socket de parameter moet **sb-hc-action = accepteren**                                                                                                                                                                                                                          |
| {path}       | Ja       | De gecodeerde URL-het naamruimtepad van de vooraf geconfigureerde verbinding hybride deze listener registreren op. Deze expressie wordt toegevoegd aan de vaste * **$servicebus**/**hybridconnection /*** padgedeelte.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB-hc-id | No        | Zie beschrijving van **id** .                                                                                                                                                                                                                                                              |

Als er een fout is, kan de service als volgt beantwoorden:

| Code | Fout          | Beschrijving                         |
|------|----------------|-------------------------------------|
| 403  | Verboden      | De URL is niet geldig.               |
| 500  | Interne fout | Er iets is misgegaan in de service |

Nadat de verbinding tot stand is gebracht, wordt de server de socket Web afgesloten wanneer de afzender Web socket wordt afgesloten omlaag of met de volgende status

| WS-Status | Beschrijving                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | De afzender-client de verbinding wordt afgesloten                                        |
| 1001      | Het verbindingspad hybride is verwijderd of uitgeschakeld.                           |
| 1008      | Het beveiligingstoken is verlopen en is daarom het verificatiebeleid geschonden. |
| 1011      | Iets verkeerd is gegaan binnen de service.                                           |

#### <a name="rejecting-the-socket"></a>Weigeren de socket

Weigeren de socket na inspectie van het bericht 'accepteren' vereist een soortgelijk handshake, zodat de code en de beschrijving van status communicatie van de reden voor de afwijzing naar de afzender teruggezonden doorstromen kunnen.

Het protocol ontwerpkeuze hier is het gebruik van een WebSocket handshake (die is ontworpen om te eindigen in een bepaalde foutstatus) zodat de listener client implementaties kunnen blijven vertrouwen op een client WebSocket en niet hoeft te gebruiken een extra, bZijn HTTP-client.

Voor het weigeren van de socket, de client wordt de URI-adres van het bericht 'accepteren' en twee queryreeksparameters toegevoegd aan het:

| Param             | Vereist? | Beschrijving                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | Ja       | Numerieke HTTP-statuscode                |
| statusDescription | Ja       | Menselijke leesbare reden voor de afwijzing |

De resulterende URI wordt vervolgens gebruikt voor het maken van een verbinding WebSocket; nogmaals, er rekening mee dat het TLS-certificaat mogelijk niet overeen met het adres tijdens de voorvertoning, zodat de validatie moet worden uitgeschakeld.

Als het goed is voltooid, mislukken deze handshake opzettelijk met een HTTP-foutcode 410, aangezien er geen WebSocket is vastgesteld. Als er een fout optreedt, zijn de opties:

| Code | Fout          | Beschrijving                         |
|------|----------------|-------------------------------------|
| 403  | Verboden      | De URL is niet geldig.               |
| 500  | Interne fout | Er iets is misgegaan in de service |

### <a name="listener-token-renewal"></a>Vernieuwing van het token listener

Als het token van de listener is verlopen, kan het door een frame tekstbericht verzenden naar de service via het besturingskanaal vastgestelde vervangen. Het bericht bevat een JSON-object met de naam 'renewToken', waarin de volgende eigenschap op dit moment:

-   **token** : een geldig, URL-gecodeerde Service Bus gedeeld Access Token voor de naamruimte of hybride verbinding die het recht **luisteren geeft** .

| renewToken bericht                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Als de token validatie mislukt, de toegang is geweigerd en cloud service de websocket control kanaal met een fout wordt gesloten, anders is is er geen antwoord ontvangen.

| WS-Status | Beschrijving                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Het beveiligingstoken is verlopen en is daarom het verificatiebeleid geschonden. |

<a name="sender-protocol"></a>Afzender-protocol
---------------

Het protocol van de afzender is in feite gelijk aan hoe een listener is gevestigd. Het doel is maximale transparantie voor de end-to-end Web-socket. Verbinding maken met het adres is hetzelfde als voor de listener, maar verschilt van de "actie" en het token moet een andere machtiging:

*wss: / / {naamruimte adres} /* *$servicebus* */* *hybridconnection /**{path}? sb-hc-action =... & sb-hc-id =... \[& sbc-hc-token =... \]*

De *naamruimte-adres* is de FQDN-naam van de naamruimte Azure Relay die fungeert als host voor de hybride verbinding, meestal van de vorm {*mijnnaam}. servicebus.windows.net.*

De aanvraag bevat mogelijk willekeurige extra HTTP-headers, waaronder die van toepassing zijn gedefinieerd. Alle geleverde headers naar de listener vloeien en vindt u op het object 'connectHeader' van het besturingsbericht 'accepteren'.

De query string parameteropties zijn als volgt

| Param        | Vereist? | Beschrijving                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-hc-actie | Ja       | Voor de rol van de listener de parameter moet **actie = verbinding**                                                                                                                                                    |
| {path}       | Ja       | De gecodeerde URL-het naamruimtepad van de vooraf geconfigureerde verbinding hybride deze listener registreren op.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB-hc-token | Yes\*     | De listener moet een geldige, door de URL-gecodeerde Service Bus gedeeld Access Token opgeven voor de naamruimte of hybride verbinding die verleent het recht **verzenden** .                                                            | | SB-hc-id | No        | Een optionele ID kunt end-to-end diagnostische tracering en wordt beschikbaar gesteld aan de listener tijdens de handshake accepteren.                                                                                       |

Als de Web-socketverbinding omdat het verbindingspad van hybride niet wordt geregistreerd of een token ongeldig of ontbreekt, of een andere fout mislukt, de fout feedback krijgt met de normale HTTP 1.1-status feedback model. Beschrijving van de status bevat een fout tracerings-id kan worden doorgegeven aan de ondersteuning van Azure:

| Code | Fout          | Beschrijving                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Niet gevonden      | De hybride **pad** is ongeldig of de basis-URL is onjuist |
| 401  | Niet-geautoriseerde   | Het beveiligingstoken is ontbreekt of onjuist of ongeldig                  |
| 403  | Verboden      | Het beveiligingstoken is niet geldig voor dit pad voor deze actie          |
| 500  | Interne fout | Er iets is misgegaan in de service                                    |

Als de Web-socketverbinding opzettelijk door de service afgesloten nadat het is in eerste instantie ingesteld, de reden voor het wel doet, worden verstrekt met behulp van een geschikte Web socket protocol foutcode samen met een omschrijving van de foutbericht dat u ook een tracking-id.

| WS-Status | Beschrijving                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1000      | De listener is afgesloten van de socket.                                                 |
| 1001      | Het verbindingspad hybride is verwijderd of uitgeschakeld.                           |
| 1008      | Het beveiligingstoken is verlopen en is daarom het verificatiebeleid geschonden. |
| 1011      | Iets verkeerd is gegaan binnen de service.                                           |

## <a name="next-steps"></a>Volgende stappen:

- [Veelgestelde vragen over doorsturen](relay-faq.md)
- [Een naamruimte maken](relay-create-namespace-portal.md)
- [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Aan de slag met het knooppunt](relay-hybrid-connections-node-get-started.md)