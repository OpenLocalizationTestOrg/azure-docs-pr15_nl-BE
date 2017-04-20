<properties 
    pageTitle="Azure Service Bus | Microsoft Azure" 
    description="Een inleiding tot Service Bus verbinding Azure toepassingen om andere software te gebruiken." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Azure Service Bus

Of een toepassing of de service wordt uitgevoerd in de cloud of op locatie vaak nodig om te communiceren met andere toepassingen of services. Om een globaal handige manier om dit te doen, biedt Microsoft Azure Service Bus. Dit artikel gaat kijken of deze technologie, met een beschrijving van wat het is en waarom u zou willen gebruiken.

## <a name="service-bus-fundamentals"></a>Grondbeginselen van Service Bus

Verschillende situaties belt voor verschillende stijlen van communicatie. Soms is laat toepassingen verzenden en ontvangen van berichten via een eenvoudige wachtrij de beste oplossing. In andere gevallen, een gewone wachtrij niet genoeg; een wachtrij met een mechanisme voor publiceren en abonneren is beter. In sommige gevallen is het enige dat echt nodig is een verbinding tussen toepassingen. wachtrijen niet vereist. Bus service biedt alle drie de opties, waardoor uw toepassingen op verschillende manieren communiceren.

Service Bus is een huurder met meerdere cloud-service, wat betekent dat de service wordt gedeeld door meerdere gebruikers. Elke gebruiker, zoals een ontwikkelaar van toepassingen, maakt u een *naamruimte*en vervolgens de communicatie mechanismen die ze nodig in die naamruimte heeft definieert. In afbeelding 1 ziet u hoe dit eruitziet.

![][1]
 
**Figuur 1: Service Bus biedt meerdere huurder voor toepassingen te verbinden via de cloud.**

In een naamruimte, kunt u een of meer exemplaren van de vier verschillende communicatieprogramma mechanismen voor elk van die toepassingen op een andere manier verbindt. De opties zijn:

- *Wachtrijen*, waardoor een directionele communicatie. Elke wachtrij fungeert als een tussenpersoon (ook wel een *broker*) die wordt opgeslagen berichten verzonden totdat ze worden ontvangen. Elk bericht wordt ontvangen door een enkele ontvanger.
- *Onderwerpen*die een directionele communicatie met *abonnementen*- één onderwerp kan meerdere abonnementen hebben. Als een wachtrij, een onderwerp fungeert als een makelaar, maar elk abonnement kunt desgewenst een filter aan berichten die aan bepaalde criteria voldoen.
- *Relais*, welke bidirectionele communicatie. In tegenstelling tot wachtrijen en onderwerpen worden een relay niet opgeslagen berichten tijdens een vlucht; het is niet een broker. In plaats daarvan geeft het net deze bij de doeltoepassing.

Wanneer u een wachtrij, het onderwerp of de relay maakt, geven u het een naam. In combinatie met welke u de naamruimte genoemd, maakt deze naam een unieke id voor het object. Toepassingen kunnen Bus Service bieden deze naam en vervolgens die wachtrij, onderwerp of doorsturen gebruiken om te communiceren met elkaar. 

Als kunnen u een van deze objecten in het scenario relay, Windows-toepassingen gebruiken Windows Communication Foundation (WCF). Windows-toepassingen kunnen gedefinieerde Service Bus messaging API's gebruiken voor wachtrijen en onderwerpen. Als u deze objecten eenvoudiger in het gebruik van niet-Windows-toepassingen, biedt Microsoft SDK voor Java, Node.js en andere talen. U kunt ook toegang tot wachtrijen en onderwerpen met REST API's via HTTP (s). 

Het is belangrijk dat u begrijpt dat, hoewel Service Bus zelf wordt uitgevoerd in de cloud (dat wil zeggen in Azure datacenters van Microsoft), toepassingen die gebruikmaken van deze overal kunnen uitvoeren. Bus Service kunt u verbinding maken met toepassingen die worden uitgevoerd op Azure, bijvoorbeeld, of toepassingen worden uitgevoerd in uw eigen datacenter. U kunt het ook gebruiken om een toepassing wordt uitgevoerd op Azure of een andere verbinding te wolk platform met een toepassing op bedrijfsruimten of tablets en telefoons. Het is zelfs mogelijk huishoudelijke apparaten, sensoren en andere apparaten verbinding maken met een centrale toepassing of naar een andere. Service Bus is een mechanisme voor de communicatie in de cloud die toegankelijk is vanaf vrijwel elke locatie. Hoe u afhankelijk het van wat uw toepassingen nodig.

## <a name="queues"></a>Wachtrijen

Stel dat u besluit om twee toepassingen met behulp van een wachtrij Bus Service verbinding te maken. Figuur 2 illustreert deze situatie.

![][2]
 
**Afbeelding 2: Service Bus wachtrijen bieden asynchrone queuing één richting.**

Het proces is eenvoudig: een afzender een bericht verzendt naar een wachtrij Service Bus en een ontvanger wordt opgehaald dat bericht op een later tijdstip. Een wachtrij kan slechts één ontvanger, hebben, zoals in figuur 2 ziet. Of meerdere toepassingen dezelfde wachtrij kunnen lezen. In deze laatste situatie wordt elk bericht gelezen door slechts één ontvanger. Voor een service met meerdere cast, moet u een onderwerp gebruiken.

Elk bericht bestaat uit twee delen: een reeks eigenschappen voor elk een sleutel/waarde-paar en de berichttekst binaire. Hoe deze worden gebruikt, is afhankelijk van wat een toepassing probeert te doen. Een toepassing voor het verzenden van een bericht weergegeven over een recente verkoop kan bijvoorbeeld de eigenschappen *Verkoper = "Ava"* en *bedrag 10000 =*. De hoofdtekst van het bericht kan bevatten een gescande afbeelding van de koopovereenkomst ondertekend, of als er niet is, maar blijven leeg.

Een ontvanger kan op twee verschillende manieren een bericht lezen uit een wachtrij Service Bus. De eerste optie, genaamd *ReceiveAndDelete*, wordt een bericht uit de wachtrij verwijderd en onmiddellijk verwijderd. Dit is eenvoudig, maar als de ontvanger vastloopt voordat het klaar is met het verwerken van het bericht, het bericht niet verloren. Omdat deze wordt verwijderd uit de wachtrij, geen andere ontvanger toegang tot het. 

De tweede optie, *PeekLock*, is bedoeld om te helpen met dit probleem. Zoals **ReceiveAndDelete**verwijdert **PeekLock** -Lees een bericht uit de wachtrij. Zou verwijderen het bericht echter niet. Deze cel vergrendelt u het bericht, waardoor deze onzichtbaar voor andere ontvangers, en wacht op een van de drie gebeurtenissen:

- Als de ontvanger het bericht met succes worden verwerkt, wordt **voltooid**en de wachtrij wordt het bericht verwijderd. 
- Als de ontvanger beslist dat deze het bericht niet kan verwerken, wordt **verlaten**. De wachtrij wordt vervolgens de vergrendeling uit het bericht verwijderd en wordt het toegankelijk voor andere ontvangers.
- Als de ontvanger geen van deze binnen een configureerbare periode (standaard 60 seconden), de wachtrij wordt ervan uitgegaan dat de ontvanger is mislukt. In dit geval alsof het de ontvanger had genoemd **verlaten**, het bericht beschikbaar maken voor andere ontvangers.

Zoals u ziet, wat hier kan gebeuren: hetzelfde bericht kan worden geleverd twee keer, misschien met twee verschillende ontvangers. Toepassingen die gebruikmaken van de Service Bus wachtrijen moeten worden voorbereid voor dit. Controle op dubbele om gemakkelijker te maken, dat elk bericht heeft een unieke **MessageID** -eigenschap standaard hetzelfde, ongeacht hoe vaak blijft wordt het bericht gelezen uit een wachtrij. 

Wachtrijen zijn handig in een aantal situaties. Ze kunnen toepassingen communiceren zelfs wanneer beide niet uitgevoerd op hetzelfde moment, iets dat is vooral handig met batch- en mobiele toepassingen. Een wachtrij met meerdere ontvangers biedt ook automatische taakverdeling, aangezien de verzonden berichten verspreid zijn over deze ontvangers.

## <a name="topics"></a>Onderwerpen

Handig deze wachtrijen worden niet altijd de juiste oplossing. Service Bus onderwerpen zijn soms beter. In afbeelding 3 wordt dit idee.

![][3]
 
**Afbeelding 3: Gebaseerd op het filter de toepassing van een abonnement neemt geeft, kunt wordt sommige of alle berichten verzonden naar een Service Bus onderwerp ontvangen.**

Een *onderwerp* lijkt in veel opzichten op een wachtrij. Afzenders verzenden berichten naar een onderwerp op dezelfde manier als ze verzenden berichten naar een wachtrij en deze berichten zien er hetzelfde uit als bij wachtrijen. Het grote verschil is dat onderwerpen elke ontvangende toepassing een eigen *abonnement* maken kunt door een *filter*te definiëren. Een abonnee ziet vervolgens alleen de berichten die overeenkomen met het filter. In figuur 3 ziet u bijvoorbeeld een verzender en een onderwerp met drie abonnees, elk met een eigen filter:

- 1-abonnee ontvangt alleen berichten met de eigenschap *Verkoper = "Ava"*.
- 2-abonnee ontvangt berichten met de eigenschap *Verkoper = "Ruby"* en/of een eigenschap van een *bedrag* waarvan de waarde hoger dan 100.000 is bevatten. Ruby is misschien de Verkoopmanager zodat ze wil haar eigen verkoop en alle grote verkopen, ongeacht wie ze is.
- Abonnee 3 heeft het filter ingesteld op *True*, wat betekent dat het alle berichten ontvangt. Bijvoorbeeld, deze toepassing is mogelijk verantwoordelijk voor het onderhouden van een audittrail en moeten daarom worden alle berichten.

Net als bij wachtrijen, kunnen abonnees van een onderwerp berichten met behulp van **ReceiveAndDelete** of **PeekLock**. In tegenstelling tot wachtrijen kan echter één bericht verzonden naar een onderwerp worden ontvangen door meerdere abonnementen. Deze benadering genoemd *publiceren en abonneren* (of *pub/sub*) is handig wanneer meerdere toepassingen in dezelfde berichten zijn geïnteresseerd. Als u het juiste filter, toegang elke abonnee hebben tot alleen het gedeelte van de berichtenstroom die nodig zijn om te zien.

## <a name="relays"></a>Relais

Beide wachtrijen en onderwerpen bieden unilaterale asynchrone communicatie via een makelaar. Verkeer in één richting loopt en er is geen directe verbinding tussen verzenders en ontvangers. Maar wat gebeurt er als u dit niet wilt? Stel dat uw toepassingen moeten beide berichten verzenden en ontvangen, of misschien wilt u een rechtstreekse koppeling tussen hen en hoeft u niet een makelaar voor het opslaan van berichten. Scenario's adres zoals deze Bus Service biedt *doorgeeft*, zoals in figuur 4 ziet.

![][4]
 
**Figuur 4: Relay Service Bus biedt een synchrone, tweerichtingscommunicatie mogelijk tussen toepassingen.**

De voor de hand liggende vraag over relais is dit: Waarom zou ik die gebruiken? Zelfs als ik geen wachtrijen, waarom maken toepassingen die communiceren via een cloud service in plaats van gewoon rechtstreeks communiceren? Het antwoord is dat praten rechtstreeks harder worden kan dan u denkt.

Stel dat u twee op ruimten toepassingen verbinding wilt maken, actief zowel in zakelijke datacenters. Elk van deze toepassingen bevindt zich achter een firewall en elk datacenter waarschijnlijk gebruik van NAT (Netwerkadresomzetting). De firewall blokkeert binnenkomende gegevens op alle, maar een paar poorten en NAT houdt in dat de computer die elke toepassing wordt uitgevoerd op een vast IP-adres dat u rechtstreeks van buiten het datacenter bereiken kunt geen. Zonder extra hulp is het problematisch deze toepassingen via het openbare internet verbinding te maken.

Een relay Service Bus kan helpen. Bi andersom communiceren via een relay, elke toepassing een uitgaande TCP-verbinding met de Bus-Service tot stand brengt en vervolgens blijft open. Alle communicatie tussen de twee toepassingen via deze verbindingen wordt gezonden. Omdat elke verbinding is gemaakt uit in het datacenter, kan de firewall binnenkomend verkeer op elke toepassing zonder nieuwe poorten geopend. Deze aanpak wordt ook het probleem oplossen door NAT, omdat elke toepassing een consistente eindpunt in de wolk in de communicatie heeft. Door het uitwisselen van gegevens via de relay, de toepassingen te voorkomen dat de problemen die zouden anders communicatie moeilijk maken. 

Voor het gebruik van de Service Bus relais vertrouwen toepassingen op Windows Communication Foundation (WCF). Service Bus biedt WCF bindingen die eenvoudig Windows-toepassingen kunnen communiceren via een relais. Toepassingen die al gebruikmaken van WCF kunnen meestal slechts een van deze bindingen en vervolgens met elkaar praten via een relay. In tegenstelling tot wachtrijen en onderwerpen echter moet relais van niet-Windows-toepassingen, terwijl het mogelijk programmeren moeite; geen standaard bibliotheken zijn beschikbaar.

In tegenstelling tot wachtrijen en onderwerpen maken niet toepassingen relais expliciet. In plaats daarvan als een toepassing die u wenst te ontvangen van berichten tot stand een TCP-verbinding met de Bus-Service brengt, wordt een relay automatisch gemaakt. Wanneer de verbinding wordt verbroken, wordt de relay verwijderd. Als u wilt dat een toepassing de relay gemaakt door een specifieke listener vinden, biedt Service Bus een register waarmee toepassingen aan een specifieke relay zoeken op naam.

Relais zijn de juiste oplossing wanneer u directe communicatie mogelijk tussen toepassingen. Neem bijvoorbeeld een luchtvaartmaatschappij reserveringssysteem in een datacenter in ruimten die vanuit in-kiosken, mobiele apparaten en andere computers moet worden uitgevoerd. Toepassingen die worden uitgevoerd op alle systemen kunnen vertrouwen op de Service Bus relais in de cloud te communiceren, zoveel mogelijk worden uitgevoerd.

## <a name="summary"></a>Samenvatting

Toepassingen te verbinden is onderdeel van het bouwen van complete oplossingen en het bereik van de scenario's waarvoor de toepassingen en services kunnen communiceren met elkaar is ingesteld op verhoogd als meer toepassingen en apparaten zijn aangesloten op het Internet. Dankzij de cloud-gebaseerde technologieën voor de verwezenlijking van deze wachtrijen, onderwerpen en relais beoogt Service Bus deze essentiële functie gemakkelijker te implementeren en meer algemeen beschikbaar te maken.

## <a name="next-steps"></a>Volgende stappen

U hebt de basisprincipes van Azure Service Bus geleerd, klik op deze koppelingen voor meer informatie.

- Het gebruik van [wachtrijen Service Bus](service-bus-dotnet-get-started-with-queues.md)
- Het gebruik van de [Service Bus onderwerpen](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- Het gebruik van [relay Service Bus](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Voorbeelden van Service Bus](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
