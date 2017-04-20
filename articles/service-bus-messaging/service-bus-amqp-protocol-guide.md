<properties 
    pageTitle="AMQP 1.0 in Azure Service Bus en Hubs gebeurtenis protocol gids | Microsoft Azure" 
    description="Protocol-handleiding voor expressies en beschrijving van AMQP 1.0 in Azure Service Bus en gebeurtenis-Hubs" 
    services="service-bus,event-hubs" 
    documentationCenter=".net" 
    authors="clemensv" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="07/01/2016"
    ms.author="clemensv;jotaub;hillaryc;sethm"/>

# <a name="amqp-10-in-azure-service-bus-and-event-hubs-protocol-guide"></a>AMQP 1.0 in Azure Service Bus en Hubs gebeurtenis protocol-handleiding

Geavanceerde Message Queuing-Protocol 1.0 is een gestandaardiseerde framing en overdracht protocol voor het overbrengen van berichten tussen twee partijen asynchroon, veilig en betrouwbaar. Het is het primaire protocol van Azure Service Bus berichten en Azure gebeurtenis Hubs. Beide services bieden ook ondersteuning voor HTTPS. De eigen SBMP-protocol dat wordt ondersteund is wordt geleidelijk vervangen door AMQP.

AMQP 1.0 is het resultaat van de samenwerking met brede industrie die bij elkaar gebracht middleware leveranciers, zoals Microsoft en Red Hat, met veel messaging middleware gebruikers zoals JP Morgan Chase die de bedrijfstak van de financiële diensten. De technische normalisatie-forum voor de specificaties van AMQP-protocol en de extensie is OASIS en deze formele goedkeuring als een internationale norm als ISO/IEC 19494 heeft behaald.

## <a name="goals"></a>Doelstellingen

Dit artikel bevat een overzicht van de belangrijkste begrippen van de AMQP 1.0-specificatie met een kleine set ontwerpspecificaties voor uitbreiding die momenteel nog in het technisch comité OASIS AMQP gewerkt wordt berichten kort en wordt uitgelegd hoe Azure Service Bus implementeert en bouwt voort op deze specificaties.

Het doel is voor ontwikkelaars met behulp van een bestaande verzameling van AMQP 1.0-client te kunnen werken met Azure Service Bus via AMQP 1.0 op elk platform.

Algemene stapels AMQP 1.0 voor algemene doeleinden, zoals Apache Proton of AMQP.NET Lite, implementeren al alle core AMQP 1.0 bewegingen. Deze bewegingen moeten beschikken over zijn soms verpakt met een hoger niveau API; Apache-Proton biedt zelfs twee, de dwingende Messenger API en de reactieve Reactor-API.

In de volgende bespreking wordt ervan uitgegaan dat het beheer van AMQP verbindingen, sessies en koppelingen en de verwerking van overboekingen frame en datatransportbesturing worden verwerkt door de respectieve stack (zoals Apache Proton-C) en hoeven niet veel eventuele specifieke aandacht van ontwikkelaars van toepassingen. We zullen abstract wordt ervan uitgegaan dat het bestaan van een aantal API-primitieven zoals de mogelijkheid om verbinding te maken en een vorm van *afzender* en *ontvanger* abstraction om objecten te maken die vervolgens hebben enkele vorm van `send()` en `receive()` bewerkingen, respectievelijk.

Bij het bespreken van geavanceerde mogelijkheden van Azure Service Bus, zoals bericht bladeren of beheren van sessies, worden die worden beschreven in termen van AMQP, maar ook als een gelaagde pseudo-implementatie op deze veronderstelde API abstraction.

## <a name="what-is-amqp"></a>Wat is AMQP?

AMQP is een protocol voor framing en overdracht. Framing betekent dat deze structuur biedt voor streams met binaire gegevens die in beide richtingen van een netwerkverbinding stromen. De structuur biedt uitstippelen voor afzonderlijke blokken gegevens – frames – moet worden uitgewisseld tussen de verbonden partijen. De capaciteit voor gegevensoverdracht Zorg ervoor dat beide communicerende partijen overeenstemming over over wanneer frames worden overgedragen en als overschrijvingen beschouwd volledig tot stand kunnen brengen.

In tegenstelling tot eerder verlopen conceptversies geproduceerd door de werkgroep AMQP die nog in gebruik is door een paar bericht makelaars, schrijven de werkgroep definitief en gestandaardiseerde AMQP 1.0-protocol niet voor de aanwezigheid van een broker bericht of een bepaalde topologie voor entiteiten in een bericht broker.

Het protocol kan worden gebruikt voor de symmetrische peer-to-peer communicatie, voor de interactie met de makelaars van bericht wachtrijen en entiteiten publiceren/abonneren, volgens Azure Service Bus wordt ondersteund. Het kan ook worden gebruikt voor interactie met messaging-infrastructuur waar de patronen van de interactie zijn anders dan gewone wachtrijen, zoals het geval met Azure gebeurtenis Hubs. Een gebeurtenis Hub fungeert als een wachtrij wanneer gebeurtenissen worden verzonden, maar meer fungeert als een seriële storage-service wanneer gebeurtenissen worden gelezen het lijkt enigszins op een tapestation. De client haalt een verschuiving in de stroom van de beschikbare gegevens en vervolgens alle gebeurtenissen van die verschuiving naar de meest recente beschikbare wordt bediend.

De AMQP 1.0-protocol is ontworpen om te worden verlengd, waardoor verdere specificaties voor het verbeteren van de mogelijkheden ervan. De extensie van drie specificaties we in dit document bespreken illustreren dit. Een specificatie definieert voor de communicatie via bestaande HTTPS/WebSockets-infrastructuur, waarbij de oorspronkelijke AMQP TCP-poorten configureren kan lastig zijn, hoe laag AMQP via WebSockets. De AMQP specificatie definieert voor interactie met de messaging-infrastructuur in een aanvraag/reactie voor beheerdoeleinden of geavanceerde functionaliteit kan bieden, de vereiste elementaire interactie-primitieven. De claims-beveiliging op basis van-AMQP specificatie definieert voor de autorisatie van federatieve model-integratie, hoe koppelen en vergunning tokens die zijn gekoppeld aan de koppelingen vernieuwen.

## <a name="basic-amqp-scenarios"></a>Basisscenario's voor AMQP

Deze sectie wordt uitgelegd dat het basisgebruik van AMQP 1.0 met Azure Service Bus met verbindingen, sessies en koppelingen maken en de overdracht van berichten en Service Bus entiteiten zoals wachtrijen, onderwerpen en abonnementen.

De meest gezaghebbende bron voor meer informatie over de werking van AMQP is de AMQP 1.0-specificatie, maar de specificatie is geschreven om nauwkeurig uitvoering en niet om te leren van het protocol. In dit gedeelte ligt de nadruk op de invoering van zoveel terminologie als nodig is voor het beschrijven hoe Service Bus AMQP 1.0 gebruikt. Voor een uitgebreide inleiding tot AMQP, alsmede een uitgebreidere bespreking van AMQP 1.0, kunt u [deze video cursus][]bekijken.

### <a name="connections-and-sessions"></a>Verbindingen en sessies

![][1]

AMQP roept de communicerende programma *containers*; de bevatten *knooppunten*die de communicatie diensten in deze containers zijn. Een wachtrij kan worden dat een knooppunt. AMQP staat voor multiplexing, zodat een verbinding kan worden gebruikt voor veel communicatiepaden tussen knooppunten; een toepassingsclient kan bijvoorbeeld tegelijkertijd ontvangen van de ene wachtrij en naar een andere wachtrij verzenden via dezelfde netwerkverbinding.

De netwerkverbinding is dus verankerd op de container. Het is gestart door de container in de rol van de client een uitgaande TCP-socket u verbinding maakt met een container in de rol van de ontvanger die luistert naar binnenkomende TCP-verbindingen accepteert. De handshake verbinding omvat onderhandeling over de protocolversie declareren of onderhandelen over het gebruik van Transport Level Security (TLS/SSL) en een verificatie-handshake voor het verbindingsbereik op basis van SASL.

Azure Service Bus vereist het gebruik van TLS te allen tijde. Het ondersteunt verbindingen via TCP-poort, 5671, waarbij de TCP-verbinding eerst met TLS bedekt wordt voordat u het protocol-handshake AMQP invoert en biedt ook ondersteuning voor verbindingen via TCP-poort 5672 waarbij de server direct biedt een verplichte upgrade van verbinding met TLS met behulp van de AMQP voorgeschreven model. De AMQP WebSockets binding wordt gemaakt van een tunnel via TCP-poort 443, die vervolgens gelijk is aan AMQP 5671 verbindingen.

Na het instellen van de verbinding en TLS, biedt Service Bus twee opties voor SASL-mechanisme:

-   SASL zonder opmaak wordt vaak gebruikt om de gebruikersnaam en wachtwoord referenties worden doorgegeven aan een server. Service Bus heeft geen accounts, maar met de naam [gedeelde toegang regels](service-bus-shared-access-signature-authentication.md), die rechten verleent en zijn gekoppeld aan een sleutel. De naam van een regel wordt gebruikt als de gebruikersnaam en de sleutel (als base64-tekst gecodeerde) wordt gebruikt als het wachtwoord. De bewerkingen toegestaan voor deze verbinding wordt bepaald door de rechten die aan de regel gekozen.

-   ANONIEME SASL wordt gebruikt voor het overslaan van SASL vergunning wanneer de client wil met het objectmodel van vorderingen op basis-beveiliging (CBS) die verderop worden beschreven. Met deze optie wordt worden een clientverbinding anoniem vastgesteld voor een korte periode gedurende welke de client kan alleen communiceren met het eindpunt van de CB's en de CBS-handshake moet voltooien.

Nadat de transportverbinding is gevestigd, de recipiënten de maximale framegrootte verklaren zij bereid zijn te verwerken, en na welke time-out voor inactiviteit zij zult eenzijdig verbreken als er geen activiteit op de verbinding.

Zij verklaren ook hoeveel gelijktijdige kanalen worden ondersteund. Een kanaal is een overdracht van unidirectionele, uitgaande, virtuele pad op de verbinding. Een sessie duurt een kanaal uit elk van de recipiënten onderling verbonden aan een pad bidirectionele communicatie.

Sessies hebben een venster gebaseerde flow control model; Wanneer een sessie wordt gemaakt, wordt elke partij verklaart hoeveel frames is bereid in het venster. Als de partijen exchange-frames venster en overdrachten stoppen wanneer het venster vol is en totdat het venster wordt opnieuw ingesteld of uitgebreid met behulp van de *flow performative* overgebrachte frames vulling (*performative* is de term AMQP voor protocol niveau bewegingen tussen de twee partijen uitgewisseld).

Dit model op basis van het venster is ongeveer gelijk aan die van het concept van het TCP-venster gebaseerde datatransportbesturing, maar op het niveau van de sessie in de socket. Begrip van het protocol voor meerdere gelijktijdige sessies bestaat, zodat netwerkverkeer met hoge prioriteit kan worden meest voorbij het normale verkeer beperkt, net als op een snelweg express lane.

Azure Service Bus gebruikt momenteel precies één sessie voor elke verbinding. De Service Bus maximale framegrootte is 262.144 bytes (256K bytes) voor de Service Bus Standard en Hubs gebeurtenis. Het is 1.048.576 (1 MB) voor de Service Bus premie. Service Bus leggen een bepaalde sessie niveau bandbreedteregeling windows geen, maar het venster regelmatig wordt opnieuw ingesteld als onderdeel van de datatransportbesturing niveau koppelen (Zie [het volgende gedeelte](#links)).

Zijn tijdelijke verbindingen, kanalen en sessies. Als de onderliggende verbinding is samengevouwen, verbindingen, TLS-tunnel SASL vergunning context en sessies moeten opnieuw worden gemaakt.

### <a name="links"></a>Koppelingen

![][2]

AMQP worden berichten overgebracht via koppelingen. Een koppeling is een communicatiepad gemaakt via een sessie waarmee inbrengende berichten in één richting; de overdracht status onderhandelingen is via de koppeling en bi-directionele tussen verbonden partijen.

Koppelingen kunnen worden gemaakt door een container op elk gewenst moment en over een bestaande sessie maakt AMQP verschilt van veel andere protocollen, zoals HTTP- en MQTT, waarbij de opening van de overdracht en het pad van de overdracht is een exclusieve bevoegdheid van de partij die de socketverbinding te maken.

De container voor het initiëren van de koppeling wordt u gevraagd de tegenovergestelde container te aanvaarden van een koppeling en een functie van de afzender of ontvanger worden gekozen. Daarom een container kan initiëren unidirectionele maken of bidirectionele communicatiepaden, met de laatste gemodelleerd als paren van koppelingen.

Koppelingen worden genoemd en die zijn gekoppeld aan knooppunten. Zoals vermeld in het begin, zijn knooppunten de communicerende entiteiten binnen een container.

Een knooppunt is in Azure Service Bus, direct gelijk aan een wachtrij, een onderwerp, een abonnement of een wachtrij voor onderdelen van een wachtrij of een abonnement. De knooppuntnaam gebruikt in AMQP is derhalve de relatieve naam van de entiteit in de naamruimte Service Bus. Als de naam van **rapportberichten**, die ook de naam van het knooppunt AMQP is een wachtrij. Een abonnement onderwerp volgt het Verdrag HTTP API door worden gesorteerd in een "abonnementen" resource-collectie en dus ook een abonnement **sub** of een onderwerp **mytopic** heeft de AMQP knooppunt naam **abonnementen-mytopic/sub**.

De client is ook vereist voor het gebruik van een naam voor het lokale knooppunt voor het maken van koppelingen. Bus service is geen detail over de namen van de nodes en zal niet interpreteren. Stapels van AMQP 1.0-client wordt meestal gebruikt een schema om te waarborgen dat deze tijdelijke knooppuntnamen uniek binnen het bereik van de client zijn.

### <a name="transfers"></a>Transfers

![][3]

Zodra u een koppeling tot stand is gebracht, kunnen berichten via die koppeling worden overgedragen. In AMQP, wordt een overdracht uitgevoerd met een penbeweging voor expliciete protocol (de *overdracht* performative) die een bericht van de afzender naar de ontvanger via een koppeling aflegt. Een overdracht is voltooid als het "betaald", wat betekent dat beide partijen overeenstemming over de uitkomst van deze overschrijving hebben vastgesteld.

In het eenvoudigste geval kunt de afzender verzenden "vooraf afgewikkeld", wat betekent dat de client is niet geïnteresseerd in het resultaat en de ontvanger geen feedback over het resultaat van de bewerking. Deze modus is Azure Service Bus op het niveau van AMQP-protocol wordt ondersteund, maar niet beschikbaar zijn in een van de client-API's.

Het normale geval is dat berichten worden verzonden niet-vereffende, en de ontvanger zal vervolgens acceptatie of afkeuring met *bestemming* performative. Weigering treedt op wanneer de ontvanger het bericht om een bepaalde reden niet accepteren en de afwijzing bericht bevat informatie over de reden dat een fout-structuur die is gedefinieerd door de AMQP is. Als er berichten worden geweigerd vanwege interne fouten in Azure Service Bus, wordt de extra informatie in die structuur die kan worden gebruikt voor het leveren van diagnostische tips voor het ondersteunend personeel als u ondersteuningsaanvragen indient. Meer details over fouten leert u verderop meer.

Een speciale vorm van weigering is de status *vrijgegeven* geeft aan dat de ontvanger heeft geen technisch bezwaar tegen de overdracht, maar ook geen belang bij het vereffenen van de overdracht. Dat geval bestaat, bijvoorbeeld wanneer een bericht wordt bezorgd in een Bus Service client en de client wil het bericht "verlaten" omdat het werk ten gevolge van het bericht te verwerken terwijl de aflevering van het bericht niet defect is kan niet worden uitgevoerd. Een variatie van die staat is de toestand *is gewijzigd* , zodat wijzigingen in het bericht bij de release. Die staat op dit moment niet gebruikt door Service Bus.

De AMQP 1.0-specificatie definieert een verdere bestemming staat *ontvangen* waarmee specifiek voor het verwerken van koppeling herstellen. Koppeling herstel kunt de status van een koppeling en de lopende leveringen op een sessie, wanneer de sessie en eerdere verbinding verloren gegaan zijn en een nieuwe verbinding opnieuw samenstellen op basis.

Azure Service Bus biedt geen ondersteuning voor koppeling herstellen; Als de client de verbinding met Bus Service met een niet-vereffende bericht verbroken transfer in behandeling, de overdracht van dat bericht is verloren en de client moet opnieuw de koppeling herstellen en opnieuw de overdracht.

Als zodanig bieden Azure Service Bus en gebeurtenis-Hubs ondersteuning voor "ten minste" transfers waar de afzender van het bericht is opgeslagen en geaccepteerd kan worden gewaarborgd, maar wordt niet ondersteund 'eenmalige' transfers op het niveau van AMQP, waar het systeem probeert de koppeling te herstellen en doorgaan om te onderhandelen over de leveringsstaat om te voorkomen dat het bericht is verzonden.

Ter compensatie van de mogelijk dubbele record verzendt, Azure Service Bus ondersteunt dubbele detectie als een optionele functie van wachtrijen en onderwerpen. Dubbele records detectie de bericht-id's van alle inkomende berichten tijdens een door de gebruiker gedefinieerde tijdvenster en alle berichten die worden verzonden met de dezelfde bericht-id's in hetzelfde venster stil neerzetten.

### <a name="flow-control"></a>Datatransportbesturing

![][4]

Elke koppeling heeft eigen flow control model naast het niveau sessie flow control model die eerder zijn besproken. Niveau sessie datatransportbesturing voorkomt dat te veel frames aan het verwerken zodra de datatransportbesturing niveau koppelen de toepassing die verantwoordelijk is voor hoeveel berichten wil verwerken via een koppeling wordt geplaatst en wanneer de container.

Op een koppeling, overboekingen kunnen het geval zijn wanneer de afzender heeft onvoldoende 'koppeling krediet". Koppeling krediet is een item door de ontvanger met de *stroom* van performative, die is binnen het bereik van een koppeling instellen. Als en wanneer de afzender koppeling creditnota is toegewezen, probeert te gebruiken dat kredietinstellingen bezorgen van berichten. Elke levering bericht verlaagt de resterende koppeling kredietinstellingen met één. Wanneer het krediet link omhoog wordt gebruikt, stopt de leveringen.

Wanneer Service Bus in de rol van de ontvanger is krijgt zo meteen de afzender met ruime koppeling krediet, zodat berichten onmiddellijk kunnen worden verzonden. Zoals credit koppeling wordt gebruikt, stuurt Service Bus af en toe een *stroom* performative aan de afzender het creditsaldo van de koppeling bijwerken.

In de rol van de afzender sturen Service Bus berichten gebruik van ieder krediet dat uitstekende link enthousiast.

Een oproep 'ontvangen' op het niveau van de API worden vertaald in een *stroom* performative door de client worden verzonden naar Service Bus en Service Bus die credit verbruiken door het nemen van de eerste beschikbare, niet-vergrendelde bericht uit de wachtrij, deze vergrendelen en overdragen. Als er geen bericht direct beschikbaar voor levering, uitstaande kredieten door een koppeling tot stand gebracht met bepaalde entiteit blijven, zoals vastgelegd in de volgorde van aankomst en berichten worden vergrendeld en overgedragen zodra deze beschikbaar is voor alle openstaande creditnota gebruiken.

De vergrendeling van een bericht wordt vrijgegeven wanneer de overdracht wordt vereffend in een van de terminal-Staten *geaccepteerd*, *geweigerd*of *vrijgegeven*. Wanneer de status van de terminal *geaccepteerd is*, wordt het bericht verwijderd uit de Bus Service. Het blijft in de Bus-Service en de volgende ontvanger wordt geleverd wanneer de overdracht van de andere lidstaten wordt bereikt. Bus service gaat u automatisch het bericht in de wachtrij van de entiteit bij de levering van maximum aantal toegestaan voor de entiteit als gevolg van herhaalde afwijzingen of releases.

Hoewel de officiële Service Bus-API's deze optie vandaag nog niet direct zichtbaar, kunt een lager niveau AMQP protocol client de koppeling krediet model de interactie 'pull '-stijl van het uitgeven van één eenheid van het krediet voor elke ontvangen aanvraag in een 'push '-stijl model door afgifte van een zeer groot aantal credits koppeling inschakelen en vervolgens berichten ontvangen zodra deze beschikbaar zijn zonder verdere tussenkomst. Push wordt ondersteund door de instellingen van de eigenschap [MessagingFactory.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.prefetchcount.aspx) of [MessageReceiver.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.prefetchcount.aspx) . Wanneer ze niet nul zijn, de AMQP-client wordt gebruikt als het krediet link.

In deze context is het belangrijk dat u begrijpt dat de klok voor het verloop van de vergrendeling van het bericht in de entiteit wordt gestart wanneer het bericht van de entiteit, en niet wanneer het bericht wordt opgeslagen op de kabel wordt gehouden. Wanneer de client geeft aan dat de bereidheid om berichten te ontvangen via de uitgifte van koppeling krediet, daarom zal naar verwachting worden de berichten in het netwerk actief trekken en klaar zijn om deze. Anders kan de vergrendeling bericht verlopen voordat het bericht wordt ook geleverd. Het gebruik van link-krediet datatransportbesturing moet rechtstreeks weerspiegelen omgaan met beschikbare berichten verzonden naar de ontvanger direct gereed.

Kortom vindt hieronder u een schematisch overzicht van de performative stroom tijdens verschillende API-interacties. Beschrijving van elke sectie een andere logische bewerking. Sommige van deze interacties worden 'lazy,"wat betekent dat ze kunnen enkel uitgevoerd worden eenmaal vereist. De afzender van een bericht maken mogelijk niet een interactie netwerk totdat het eerste bericht is verzonden of aangevraagd.

De pijlen stroomrichting de performative.

#### <a name="create-message-receiver"></a>Bericht ontvanger maken

| Client                                                                                                                                                | Bus service                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------    |--------------------------------------------------------------------------------------------------------------------------------------------   |
| --> koppelen ()<br/>naam = {koppelingsnaam}<br/>afgehandeld = {numerieke ingang}<br/>rol =**ontvanger**,<br/>bron = {entiteitsnaam}<br/>TARGET = {client koppeling-id}<br/>)         | Client is gekoppeld aan de entiteit als ontvanger                                                                                                         |
| U koppelt het einde van de koppeling Service Bus-antwoorden                                                                                                     | <--koppelen ()<br/>naam = {koppelingsnaam}<br/>afgehandeld = {numerieke ingang}<br/>rol =**afzender**,<br/>bron = {entiteitsnaam}<br/>TARGET = {client koppeling-id}<br/>)       |

#### <a name="create-message-sender"></a>Afzender maken

| Client                                                                                                            | Bus service                                                                                                           |
|------------------------------------------------------------------------------------------------------------------ |--------------------------------------------------------------------------------------------------------------------   |
| --> koppelen ()<br/>naam = {koppelingsnaam}<br/>afgehandeld = {numerieke ingang}<br/>rol =**afzender**,<br/>bron = {client koppeling-id},<br/>TARGET = {entiteitsnaam}<br/>)   | Geen actie                                                                                                                     |
| Geen actie                                                                                                                 | <--koppelen ()<br/>naam = {koppelingsnaam}<br/>afgehandeld = {numerieke ingang}<br/>rol =**ontvanger**,<br/>bron = {client koppeling-id},<br/>TARGET = {entiteitsnaam}<br/>)     |

#### <a name="create-message-sender-error"></a>Afzender (fout) maken

| Client                                                                                                            | Bus service                                                           |
|------------------------------------------------------------------------------------------------------------------ |---------------------------------------------------------------------  |
| --> koppelen ()<br/>naam = {koppelingsnaam}<br/>afgehandeld = {numerieke ingang}<br/>rol =**afzender**,<br/>bron = {client koppeling-id},<br/>TARGET = {entiteitsnaam}<br/>)   | Geen actie                                                                     |
| Geen actie                                                                                                                 | <--koppelen ()<br/>naam = {koppelingsnaam}<br/>afgehandeld = {numerieke ingang}<br/>rol =**ontvanger**,<br/>bron = null,<br/>TARGET = null<br/>)<br/><br/><--loskoppelen ()<br/>afgehandeld = {numerieke ingang}<br/>gesloten =**true**<br/>Fout = {info fout}<br/>)  |

#### <a name="close-message-receiversender"></a>Bericht sluiten ontvanger/afzender

| Client                                            | Bus service                                       |
|-------------------------------------------------  |-------------------------------------------------  |
| --> loskoppelen ()<br/>afgehandeld = {numerieke ingang}<br/>gesloten =**true**<br/>)    | Geen actie                                                 |
| Geen actie                                                 | <--loskoppelen ()<br/>afgehandeld = {numerieke ingang}<br/>gesloten =**true**<br/>)    |

#### <a name="send-success"></a>Verzenden (succes)

| Client                                                                                                                        | Bus service                                                                                           |
|------------------------------------------------------------------------------------------------------------------------------ |------------------------------------------------------------------------------------------------------ |
| --> overdracht)<br/>levering-id = {numerieke ingang}<br/>levering-tag = {binaire ingang}<br/>vereffend =**false**,, meer =**false**<br/>toestand =**null**,<br/>hervatten =**false**<br/>)   | Geen actie                                                                                                     |
| Geen actie                                                                                                                             | <--disposition ()<br/>rol = ontvanger,<br/>eerst = {levering-id}<br/>laatste = {levering-id}<br/>vereffend =**true**<br/>status =**geaccepteerd**<br/>)   |

#### <a name="send-error"></a>Verzenden (fout)

| Client                                                                                                                        | Bus service                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------------------ |-----------------------------------------------------------------------------------------------------------------------------  |
| --> overdracht)<br/>levering-id = {numerieke ingang}<br/>levering-tag = {binaire ingang}<br/>vereffend =**false**,, meer =**false**<br/>toestand =**null**,<br/>hervatten =**false**<br/>)   | Geen actie                                                                                                                             |
| Geen actie                                                                                                                             | <--disposition ()<br/>rol = ontvanger,<br/>eerst = {levering-id}<br/>laatste = {levering-id}<br/>vereffend =**true**<br/>toestand =**afgewezen**()<br/>Fout = {info fout}<br/>)<br/>)     |

#### <a name="receive"></a>Ontvangen

| Client                                                                                                | Bus service                                                                                                                   |
|------------------------------------------------------------------------------------------------------ |------------------------------------------------------------------------------------------------------------------------------ |
| stroom (--><br/>koppeling credit = 1<br/>)                                                                                 | Geen actie                                                                                                                             |
| Geen actie                                                                                                     | < transfer ()<br/>levering-id = {numerieke ingang}<br/>levering-tag = {binaire ingang}<br/>vereffend =**false**<br/>meer =**false**<br/>toestand =**null**,<br/>hervatten =**false**<br/>)     |
| bestemming (--><br/>rol =**ontvanger**,<br/>eerst = {levering-id}<br/>laatste = {levering-id}<br/>vereffend =**true**<br/>status =**geaccepteerd**<br/>)   | Geen actie                                                                                                                             |

#### <a name="multi-message-receive"></a>Met meerdere berichten ontvangen

| Client                                                                                                    | Bus service                                                                                                                       |
|--------------------------------------------------------------------------------------------------------   |--------------------------------------------------------------------------------------------------------------------------------   |
| stroom (--><br/>koppeling credit = 3<br/>)                                                                                 | Geen actie                                                                                                                                 |
| Geen actie                                                                                                         | < transfer ()<br/>levering-id = {numerieke ingang}<br/>levering-tag = {binaire ingang}<br/>vereffend =**false**<br/>meer =**false**<br/>toestand =**null**,<br/>hervatten =**false**<br/>)     |
| Geen actie                                                                                                         | < transfer ()<br/>levering-id = {numerieke ingang + 1},<br/>levering-tag = {binaire ingang}<br/>vereffend =**false**<br/>meer =**false**<br/>toestand =**null**,<br/>hervatten =**false**<br/>)   |
| Geen actie                                                                                                         | < transfer ()<br/>levering-id = {numerieke ingang + 2},<br/>levering-tag = {binaire ingang}<br/>vereffend =**false**<br/>meer =**false**<br/>toestand =**null**,<br/>hervatten =**false**<br/>)   |
| bestemming (--><br/>rol = ontvanger,<br/>eerst = {levering-id}<br/>laatste = {id levering + 2}<br/>vereffend =**true**<br/>status =**geaccepteerd**<br/>)     | Geen actie                                                                                                                                 |

### <a name="messages"></a>Berichten

De volgende gedeelten wordt uitgelegd welke eigenschappen van de standaard AMQP bericht secties worden gebruikt door de Service Bus en hoe ze worden toegewezen aan de officiële Service Bus-API's.

#### <a name="header"></a>koptekst

| Veldnaam        | Gebruik                             | API-naam          |
|----------------   |-------------------------------    |---------------    |
| duurzame           | -                                 | -                 |
| prioriteit          | -                                 | -                 |
| TTL               | TTL voor dit bericht     | [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)     |
| eerste verwerver    | -                                 | -                 |
| levering-count    | -                                 | [DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx)   |

#### <a name="properties"></a>Eigenschappen

| Veldnaam            | Gebruik                                                                                                                             | API-naam                                      |
|---------------------- |---------------------------------------------------------------------------------------------------------------------------------  |--------------------------------------------   |
| bericht-id            | De toepassing, vrije vorm id voor dit bericht. Gebruikt voor detectie van dubbele.                                         | [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)                                   |
| gebruikers-id               | Toepassing gedefinieerde gebruikers-id niet geïnterpreteerd door de Service Bus.                                                              | Niet toegankelijk via de Bus Service API.   |
| Aan                    | Toepassing gedefinieerd doel-id niet geïnterpreteerd door de Service Bus.                                                       | [Aan](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.to.aspx)                                             |
| Onderwerp               | Toepassing gedefinieerd bericht doel-id, niet geïnterpreteerd door de Service Bus.                                                   | [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)                                       |
| beantwoorden              | Antwoord-pad toepassing gedefinieerde indicator niet geïnterpreteerd door de Service Bus.                                                         | [ReplyTo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replyto.aspx)                                       |
| correlatie-id        | Toepassingsspecifieke correlatie-id niet geïnterpreteerd door de Service Bus.                                                       | [CorrelationId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.correlationid.aspx)                               |
| inhoudstype          | De toepassing inhoudstype indicator voor de hoofdtekst, niet geïnterpreteerd door de Service Bus.                                          | [ContentType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx)                                   |
| codering van inhoud      | Toepassing gedefinieerde inhoud coderen-indicator voor de hoofdtekst, niet geïnterpreteerd door de Service Bus.                                      | Niet toegankelijk via de Bus Service API.   |
| absolute verlooptijd  | Verklaart op welke absolute instant het bericht is verlopen. Op de input genegeerd (koptekst ttl wordt waargenomen), bindend voor uitvoer.   | [ExpiresAtUtc](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.expiresatutc.aspx)                                 |
| gemaakt om         | Verklaart op welk tijdstip het bericht is gemaakt. Niet wordt gebruikt door de Service Bus                                                           | Niet toegankelijk via de Bus Service API.   |
| groep-id              | Toepassing gedefinieerde id voor een set verwante berichten. Voor Service Bus-sessies gebruikt.                                      | [Sessie-id](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx)                                   |
| volgorde van de groep        | Teller het relatieve volgnummer van het bericht in een sessie te identificeren. Genegeerd door de Service Bus.                         | Niet toegankelijk via de Bus Service API.   |
| antwoord-naar-group-id     | -                                                                                                                                 | [ReplyToSessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replytosessionid.aspx)                             |

## <a name="advanced-service-bus-capabilities"></a>Geavanceerde mogelijkheden voor Service Bus

Deze sectie bevat geavanceerde mogelijkheden van Azure Service Bus die zijn gebaseerd op uitbreidingen van AMQP op dit moment in het technisch comité OASIS ontwikkeld voor AMQP concept. Azure Service Bus implementeert de meest recente status van deze concepten en wijzigingen Als deze concepten standaard status bereiken zal vaststellen.

> [AZURE.NOTE] Een aanvraag/reactie-patroon Service Bus berichten geavanceerde bewerkingen worden ondersteund worden beschouwd. De details van deze bewerkingen worden beschreven in het document [AMQP 1.0 in Service Bus: aanvraag/reactie-bewerkingen](https://msdn.microsoft.com/library/azure/mt727956.aspx).

### <a name="amqp-management"></a>Beheer van AMQP

De AMQP specificatie is de eerste van de concept-extensies die we hier bespreken. Deze specificatie definieert een set protocol penbewegingen extra boven op het protocol AMQP management interacties met de infrastructuur voor berichtafhandeling via AMQP toestaan. De specificatie definieert de algemene bewerkingen, zoals *maken*, *lezen*, *bijwerken*en *verwijderen* voor het beheren van de entiteiten in een infrastructuur voor berichtafhandeling en een set van de query.

Alle bewegingen die een aanvraag/reactie-interactie tussen de client en de messaging-infrastructuur nodig en daarom de specificatie definieert het patroon interactie op AMQP model: de client maakt verbinding met de messaging-infrastructuur, een sessie start en maakt vervolgens een paar koppelingen. Op een koppeling naar de client fungeert als afzender en anderzijds het als ontvanger, waardoor er een paar links die als een bi-directionele kanaal fungeren kunnen.

| Logische bewerking            | Client                      | Bus service                 |
|------------------------------|-----------------------------|-----------------------------|
| Aanvraag antwoord pad maken | --> koppelen ()<br/>naam = {*koppelingsnaam*}<br/>afgehandeld = {*numerieke ingang*},<br/>rol =**afzender**,<br/>bron =**null**,<br/>TARGET = "myentity / $management"<br/>)                            |Geen actie                             |
|Aanvraag antwoord pad maken                              |Geen actie                             | \<--koppelen ()<br/>naam = {*koppelingsnaam*}<br/>afgehandeld = {*numerieke ingang*},<br/>rol =**ontvanger**,<br/>bron = null,<br/>TARGET = "myentity"<br/>)                            |
|Aanvraag antwoord pad maken                              | --> koppelen ()<br/>naam = {*koppelingsnaam*}<br/>afgehandeld = {*numerieke ingang*},<br/>rol =**ontvanger**,<br/>bron = "myentity / $management",<br/>TARGET = "id myclient$"<br/>)                            |                             |Geen actie
|Aanvraag antwoord pad maken                              |Geen actie                             | \<--koppelen ()<br/>naam = {*koppelingsnaam*}<br/>afgehandeld = {*numerieke ingang*},<br/>rol =**afzender**,<br/>bron = "myentity",<br/>TARGET = "id myclient$"<br/>)                            |

Met deze combinatie van koppelingen in plaats, de uitvoering van de aanvraag en respons is eenvoudig: een verzoek wordt een bericht verzonden naar een entiteit in de infrastructuur voor berichtenafhandeling die dit patroon begrijpt. In dat verzoek bericht is het veld *antwoord* in de sectie *Eigenschappen* ingesteld op de *doel* -id voor de koppeling naar die ervoor zorgen dat het antwoord. De entiteit verwerking verwerking van de aanvraag en het antwoord vervolgens bezorgen via de koppeling waarvan *doel* -id overeenkomt met de id opgegeven *Antwoordadres* .

Het patroon moet duidelijk dat de container van de client en de client gegenereerde id voor het doel antwoord uniek zijn voor alle clients en om veiligheidsredenen ook moeilijk te voorspellen.

De berichten uitgewisseld gebruikt voor het management-protocol en andere protocollen met hetzelfde patroon gebeuren op het toepassingsniveau van; Deze bepalen nieuwe AMQP protocol niveau bewegingen niet. Dat is een opzettelijke toepassingen profiteert direct van deze extensies met compatibele AMQP 1.0 stapels.

Azure Service Bus momenteel implementeert een van de hoofdfuncties van de specificatie, maar de aanvraag/reactie-patroon gedefinieerd door de specificatie van het management is basissysteem voor de functie claims-beveiliging op basis van- en voor bijna alle van de geavanceerde mogelijkheden in de volgende secties besproken.

### <a name="claims-based-authorization"></a>Op claims gebaseerde verificatie

Het concept van de specificatie AMQP vorderingen-op basis van verificatie (CBS) is gebaseerd op de specificatie van de aanvraag en respons patroon en een beschrijving van een algemene model voor federatieve beveiligingstokens gebruiken met AMQP.

Het standaard beveiligingsmodel van AMQP besproken in de inleiding is gebaseerd op SASL en geïntegreerd met de handshake AMQP verbinding. Met behulp van SASL heeft als voordeel dat een uitbreidbare model waarvoor een reeks mechanismen zijn gedefinieerd bevat in elk protocol dat formeel op SASL leans kan profiteren. Onder deze mechanismen zijn "PLAIN" voor de overdracht van gebruikersnamen en wachtwoorden 'Externe' binden aan TLS-level security, "Anoniem" het ontbreken van expliciete authentication/authorization en tal van aanvullende regelingen waardoor doorgeven van verificatiereferenties of verificatie en/of tokens Express.

SASL-integratie van AMQP heeft twee nadelen:

-   Alle referenties en tokens zijn binnen het bereik van de verbinding. Een infrastructuur voor berichtenafhandeling kunt bieden gedifferentieerde toegangsbeheer op basis van per entiteit. Bijvoorbeeld, waardoor de drager van een token te verzenden naar een wachtrij maar niet naar de wachtrij B. Met de machtiging context verankerd op de verbinding, is het niet mogelijk om slechts één verbinding en nog andere toegangstokens voor wachtrij A en B. wachtrij

-   Toegangstokens zijn normaal gesproken alleen geldig voor een beperkte tijd. Dit zorgt ervoor dat de gebruiker moet regelmatig opnieuw ophalen tokens en biedt de mogelijkheid om de tokenuitgever te weigeren de afgifte van een verse token als de machtigingen van de gebruiker zijn gewijzigd. Verbindingen van AMQP mogen gedurende lange perioden duren. Het model SASL biedt alleen een kans om een token tijdens de verbinding, wat dat de infrastructuur voor berichtenafhandeling betekent op de client verbreken wanneer de token vervalt of moet het accepteren van het risico op verdere communicatie met een client is ingesteld die is toegangsrechten is ingetrokken in de tussentijd.

De CBS AMQP specificatie geïmplementeerd door Azure Service Bus, kunt u een elegante oplossing voor deze problemen: het kan een client access tokens koppelen aan elk knooppunt en die tokens bijwerken voordat ze verlopen, zonder dat de berichtenstroom.

CBS definieert een beheer van virtuele knooppunt met de naam *$cbs*, die de infrastructuur voor berichtafhandeling. Het knooppunt beheer van accepteert tokens voor eventuele andere knooppunten in de infrastructuur voor berichtenafhandeling.

De beweging van het protocol is een uitwisseling aanvraag/antwoord zoals gedefinieerd door de specificatie van het management. Dit betekent dat de client tot stand brengt een paar koppelingen met het knooppunt *$cbs* geeft vervolgens een verzoek op de uitgaande link en wacht tot het antwoord op de binnenkomende verbinding.

Het aanvraagbericht heeft de volgende eigenschappen:

| Sleutel        | Optioneel | Soort waarde | De inhoud van waarde                             |
|------------|----------|------------|--------------------------------------------|
| bewerking  | Nee       | tekenreeks     | **Put-token**                                |
| type       | Nee       | tekenreeks     | Het type van het token wordt genomen.            |
| naam       | Nee       | tekenreeks     | Het "publiek", het token is van toepassing. |
| vervaldatum | Ja      | tijdstempel  | De verlooptijd van de token.              |

De eigenschap *name* geeft de entiteit waarmee het token gekoppeld wordt. In Service Bus is het pad naar de wachtrij of onderwerp/abonnement. De eigenschap *type* geeft het type token:

| Type token                      | Token-beschrijving      | Hoofdtekst           | Notities                                                    |
|---------------------------------|------------------------|---------------------|----------------------------------------------------------|
| amqp:jwt                        | JSON Web Token (JWT)   | Waarde van AMQP (tekenreeks) | Nog niet beschikbaar.  |
| amqp:SWT                        | Eenvoudige website-Token (SWT) | Waarde van AMQP (tekenreeks) | Alleen ondersteund voor SWT-tokens die zijn uitgegeven door AAD/ACS          |
| servicebus.Windows.NET:sastoken | Service Bus SAS Token  | Waarde van AMQP (tekenreeks) | -                                                        |

Tokens verleent rechten. Bus service kent drie fundamentele rechten: 'Verzenden' kunnen verzenden 'Luisteren' kunnen ontvangen en "Beheren" manipuleren van entiteiten. SWT-tokens die zijn uitgegeven door AAD/ACS expliciet opnemen die rechten als vorderingen. Service Bus SAS tokens verwijzen naar de regels die zijn geconfigureerd op de naamruimte of entiteit en de regels die zijn geconfigureerd met de rechten. Het token ondertekenen met de sleutel die is gekoppeld aan die regel dus kunt u de token express de respectieve rechten. Het token dat is gekoppeld aan een entiteit met *put-token* wordt de aangesloten client voor interactie met de entiteit per de token rechten toestaan. Een koppeling waarbij de client de rol van de *afzender neemt* moet de "verzenden" rechts op de rol van de *ontvanger* moet het recht 'Luisteren'.

Dit bericht heeft de volgende waarden voor de *Eigenschappen van de toepassing*

| Sleutel                | Optioneel | Soort waarde | De inhoud van waarde                    |
|--------------------|----------|------------|-----------------------------------|
| statuscode        | Nee       | int        | HTTP-antwoordcode **[RFC2616]**. |
| Beschrijving van de status | Ja      | tekenreeks     | Beschrijving van de status.        |

De client kunt aanroepen *put token* herhaaldelijk en voor een entiteit in een infrastructuur voor berichtafhandeling. De tokens zijn binnen het bereik van de huidige client en verankerd op de huidige verbinding, wat betekent dat de server wordt een ingehouden tokens neerzetten wanneer de verbinding uitvalt.

De huidige implementatie van Service Bus kunt u alleen CBS in combinatie met de methode SASL 'Anoniem'. SSL/TLS-verbinding moet altijd aanwezig zijn voordat de SASL-handshake.

De anonieme mechanisme moet daarom worden ondersteund door de gekozen AMQP 1.0-client. Anonieme toegang betekent dat de eerste verbinding handshake, zoals het maken van de eerste sessie gebeurt zonder Service Bus weten wie de verbinding wordt gemaakt.

Zodra de verbinding en de sessie tot stand is gebracht, zijn de koppelingen toevoegen aan de *$cbs* knooppunt en het verzenden van de aanvraag *put token* de enige toegestane bewerkingen. Een geldige token moet worden ingesteld met succes met behulp van een aanvraag voor een *put-token* voor sommige knooppunt entiteit binnen 20 seconden nadat de verbinding tot stand is gebracht, anders eenzijdig de verbinding wordt verbroken door Service Bus.

De client is vervolgens verantwoordelijk voor het bijhouden van token verlopen. Als een token verlopen, drop Service Bus zo spoedig mogelijk alle koppelingen op de verbinding met de desbetreffende entiteit. Om dit te voorkomen, kan de client het token voor het knooppunt vervangen door een nieuwe op elk gewenst moment via het knooppunt beheer van virtuele *$cbs* met de penbeweging voor dezelfde *put token* en zonder getting werpen de nettolading verkeer die op verschillende koppelingen doorloopt.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppelingen voor meer informatie over AMQP:

- [Overzicht Service Bus AMQP]
- [AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen]
- [AMQP in Bus Service voor Windows Server]

[Deze video cursus]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp/amqp1.png
[2]: ./media/service-bus-amqp/amqp2.png
[3]: ./media/service-bus-amqp/amqp3.png
[4]: ./media/service-bus-amqp/amqp4.png

[Overzicht Service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in Bus Service voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx