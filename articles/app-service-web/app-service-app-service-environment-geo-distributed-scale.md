<properties 
    pageTitle="Geo Distributed schaal met App Service-omgevingen" 
    description="Informatie over het horizontaal schalen apps met behulp van geo-verdeling en beheer van netwerkverkeer en App Service-omgevingen." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>Geo Distributed schaal met App Service-omgevingen

## <a name="overview"></a>Overzicht ##
Scenario's voor toepassingen waarvoor zeer hoge schaal kunnen overschrijden compute resourcecapaciteit voor een implementatie van een app beschikbaar.  Stemmen toepassingen, zijn sportieve gebeurtenissen en televisie-entertainment alle voorbeelden van scenario's waarvoor zeer hoge schaal. Schaal hoge eisen kunnen worden voldaan door apps, horizontaal schalen met implementaties met meerdere app binnen één enkel, maar ook tussen regio's, voor het verwerken van extreme belasting eisen gesteld.

App Service omgevingen zijn een ideaal platform voor horizontale schaal uit.  Eenmaal een App-omgeving configuratie is geselecteerd die een snelheid van bekende verzoek kan ondersteunen, ontwikkelaars kunnen implementeren meer App serviceomgevingen in "uitsteekvorm" mode ter verwezenlijking van de gewenste piek laadvermogen.

Stel bijvoorbeeld dat een app actief op de configuratie van een App-omgeving is getest om te verwerken van 20K-aanvragen per seconde (RPS).  Als het gewenste maximum laadvermogen 100K RPS, kunnen vervolgens vijf (5) App Service-omgevingen worden gemaakt en geconfigureerd, zodat de toepassing de verwachte maximale belasting kan verwerken.

Omdat klanten doorgaans toegang krijgen toepassingen met behulp van een aangepaste (of aangepaste) domein tot, moeten ontwikkelaars aanvragen app verdelen over alle App-omgeving.  Een goede manier om dit te bereiken is het aangepaste domein met een [Azure verkeer van Verbindingsbeheer]oplossen[AzureTrafficManagerProfile].  Wijst alle van de afzonderlijke App Service-omgevingen kan het verkeer Manager profiel worden geconfigureerd.  Verkeer Manager verwerkt automatisch door de distributie van klanten over alle van de App Service omgeving op basis van de instellingen in het profiel van het beheer van verkeer voor taakverdeling.  Deze methode werkt ongeacht of alle van de Service App omgevingen worden geïmplementeerd in een bepaalde regio Azure, of wereldwijd toegepast tussen meerdere Azure regio's.

Voorts omdat apps klanten via het domein ijdelheid benaderen, klanten, zijn niet op de hoogte van het aantal App Service-omgevingen met een app.  Daardoor kunnen ontwikkelaars snel en eenvoudig toevoegen en verwijderen, App Service omgeving op basis van de geconstateerde belasting.

De conceptuele diagrammen hieronder ziet u een horizontaal geschaald uit over drie App serviceomgevingen binnen één enkel app.

![Conceptuele architectuur][ConceptualArchitecture] 

De rest van dit onderwerp, doorloopt de stappen die betrokken zijn bij het instellen van een gedistribueerde topologie voor de toepassing van het monster met behulp van omgevingen met meerdere App-Service.

## <a name="planning-the-topology"></a>De topologie plannen ##
Voor het bouwen van een gedistribueerde toepassing footprint, is het handig om een paar stukken informatie vooraf.

- **Aangepaste domein voor de app:**  Wat is de naam van het aangepaste domein waarmee klanten toegang tot de app?  Voor de toepassing van het monster is de naam van het aangepaste domein *www.scalableasedemo.com*
- **Verkeer Manager-domein:**  Een domeinnaam moet worden gekozen als u een [profiel Azure verkeer Manager][AzureTrafficManagerProfile].  Deze naam wordt gecombineerd met een geregistreerd domein dat wordt beheerd door verkeer Manager in het achtervoegsel *trafficmanager.net* .  De naam van de gekozen is voor de toepassing van het monster, *schaalbare ase-demo*.  Hierdoor is de volledige domeinnaam die wordt beheerd door verkeer Manager *schaalbare ase-demo.trafficmanager.net*.
- **Strategie voor het schalen van het oppervlak van de app:**  Het oppervlak van de toepassing worden verdeeld over meerdere omgevingen van App-Service in een bepaalde regio?  Meerdere regio's?  Een mix-and-match van beide benaderingen?  De beslissing moet worden gebaseerd op verwachtingen van waar de klantverkeer oorsprong, alsmede hoe goed de rest van de Apps back-end infrastructuur ondersteunen kan worden geschaald.  Bijvoorbeeld bij een 100% stateless toepassingen kan een app worden massively geschaald met behulp van een combinatie van omgevingen met meerdere App Service per Azure regio, vermenigvuldigd met App serviceomgevingen voor meerdere Azure regio's implementeren.  Klanten met 15 + openbare Azure regio's beschikbaar om uit te kiezen, kunnen echt een footprint wereldwijd hyperscale '-toepassing maken.  Voor het monster app gebruikt voor dit artikel, zijn in één regio Azure (Zuid-Centraal US) drie App serviceomgevingen gemaakt.
- **De naamgevingsconventie voor de Service App omgevingen:**  Elke App-omgeving moet een unieke naam.  Na een of twee App-omgevingen is het handig om een naamgevingsconventie om vast te stellen van elke App-omgeving.  Voor de toepassing van het monster is een eenvoudige naamgevingsconventie gebruikt.  De namen van de drie App serviceomgevingen zijn *fe1ase*, *fe2ase*en *fe3ase*.
- **De naamgevingsconventie voor de apps:**  Aangezien meerdere sessies van de toepassing wordt geïmplementeerd, is voor elk exemplaar van een gedistribueerde toepassing een naam nodig.  Een weinig bekende, maar zeer handige functie van App serviceomgevingen is dat dezelfde app naam kan worden gebruikt in omgevingen met meerdere App-Service.  Aangezien elke App-omgeving een unieke domeinachtervoegsel heeft, ontwikkelaars zelf kunnen kiezen om de naam exact dezelfde app opnieuw te gebruiken in elke omgeving.  Een ontwikkelaar kan dus de volgende apps: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*, enz.  Voor het monster app maar elke instantie app heeft ook een unieke naam.  De app instantienamen gebruikt zijn *webfrontend1*, *webfrontend2*en *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Het verkeer Manager profiel instellen ##
Als u meerdere exemplaren van een toepassing worden geïmplementeerd in omgevingen met meerdere App-Service, kunnen de afzonderlijke app-exemplaren met verkeer Manager worden geregistreerd.  Voor de toepassing van het monster een Manager verkeer profiel nodig voor *schaalbare ase-demo.trafficmanager.net* die klanten naar een van de volgende exemplaren van de gedistribueerde toepassing doorsturen kunt:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Een exemplaar van het monster app geïmplementeerd op de eerste App-omgeving.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Een exemplaar van het monster app geïmplementeerd op de tweede App-omgeving.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Een exemplaar van het monster app geïmplementeerd op de derde App-omgeving.

De eenvoudigste manier om meerdere Azure App eindpunten, alle uitgevoerd in de **dezelfde** regio Azure, registreren is met de Powershell [Azure Resource Manager verkeer Manager ondersteunt][ARMTrafficManager].  

De eerste stap is het maken van een serviceprofiel Azure verkeer.  De volgende code ziet u hoe het profiel is gemaakt voor het monster app:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

U ziet hoe de parameter *RelativeDnsName* is ingesteld op *schaalbare ase-demo*.  Dit is hoe het domein naam *schaalbare ase-demo.trafficmanager.net* is gemaakt en die is gekoppeld aan een profiel verkeer Manager.

De parameter *TrafficRoutingMethod* bepaalt de taakverdeling beleid verkeer Manager wordt gebruikt om te bepalen hoe de klant belasting verspreid over alle beschikbare eindpunten.  In dit voorbeeld de *gewogen* werd methode gekozen.  Dit zal leiden tot aanvragen van klanten worden verspreid over alle van de eindpunten van de geregistreerde toepassingen op basis van het relatieve gewicht dat is gekoppeld aan elk eindpunt. 

Met het profiel is gemaakt, elk exemplaar app toegevoegd aan het profiel als een native Azure eindpunt.  De volgende code haalt een verwijzing naar elke front-end web app, en vervolgens elke app toegevoegd als een eindpunt van het beheer van verkeer in de parameter *TargetResourceId* .


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
U ziet hoe er één aanroep *Toevoegen AzureTrafficManagerEndpointConfig* voor elke individuele app-exemplaar is.  De parameter *TargetResourceId* in elke opdracht Powershell verwijst naar een van de drie exemplaren van de gedistribueerde toepassing.  Het profiel van het beheer van verkeer zal laden verspreid over alle drie eindpunten worden geregistreerd in het profiel.

Alle van de drie eindpunten (10) dezelfde waarde voor de parameter *gewicht* gebruiken.  Dit resulteert in verkeer Manager verspreiding klantverzoeken in alle exemplaren van de drie app vrij gelijkmatig. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Aangepaste domein op het domein verkeer Manager van de toepassing aan te wijzen ##
De laatste stap die nodig is om het aangepaste domein van de toepassing op het verkeer Manager-domein.  Dit betekent wijzen *schaalbare ase-demo.trafficmanager.net* *www.scalableasedemo.com* voor de toepassing van het monster.  Deze stap moet worden voltooid met de domeinregistratieservice dat het aangepaste domein beheert.  

Met hulpprogramma's van uw domeinregistratieservice, records een CNAME moet worden gemaakt die het aangepaste domein in het verkeer Manager-domein wijst.  In de volgende afbeelding ziet u een voorbeeld van deze CNAME-configuratie ziet als eruit:

![CNAME voor aangepaste domein][CNAMEforCustomDomain] 

Maar niet in dit onderwerp wordt behandeld, houd er rekening mee dat elke individuele app-exemplaar moet het aangepaste domein geregistreerd bij deze ook hebben.  Anders als een aanvraag kunt u met een app-exemplaar en de toepassing geen het aangepaste domein geregistreerd met de app heeft, zal het verzoek mislukken.  

In dit voorbeeld wordt de aangepaste domein *www.scalableasedemo.com*en elk toepassingsexemplaar heeft het aangepaste domein gekoppeld.

![Aangepaste domein][CustomDomain] 

Zie het volgende artikel op [aangepaste domeinen registreren]voor een recap van een aangepast domein registreren met Azure App Service apps[RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Uitproberen van de gedistribueerde topologie ##
Het eindresultaat van het beheer van netwerkverkeer en DNS-configuratie is dat de aanvragen voor *www.scalableasedemo.com* via de volgende volgorde vloeien zullen:

1. Een browser of apparaat, wordt er een DNS-zoekactie voor *www.scalableasedemo.com*
2. De CNAME-post bij de domeinregistratieservice zorgt ervoor dat de DNS-zoekopdracht om te leiden tot Azure verkeer Manager.
3. Een DNS-lookup wordt gemaakt voor *schaalbare ase-demo.trafficmanager.net* tegen een van de Azure verkeer Manager DNS-servers.
4. Op basis van het beleid (de parameter *TrafficRoutingMethod* eerder gebruikt bij het maken van het serviceprofiel verkeer) voor taakverdeling, verkeer-beheer selecteert u een van de eindpunten van de geconfigureerde en de FQDN-naam van dat eindpunt naar de browser of apparaat terug.
5.  Aangezien de FQDN-naam van het eindpunt de Url van een app-exemplaar op een App-omgeving uitgevoerd is, vraagt de browser of het apparaat een Microsoft Azure-DNS-server voor het omzetten van de FQDN-naam naar een IP-adres. 
6. De browser of het apparaat sturen het HTTP/S verzoek naar het IP-adres.  
7. De aanvraag aankomt in een van de app-exemplaren op een van de Service App omgevingen.

De console van de volgende afbeelding ziet u een DNS-zoekactie voor aangepaste domein van de monster-app is te herleiden tot een app-exemplaar wordt uitgevoerd op één van de drie monster App Service-omgevingen (in dit geval de tweede van de drie App serviceomgevingen):

![DNS-zoekopdracht][DNSLookup] 

## <a name="additional-links-and-information"></a>Als u meer koppelingen en informatie ##
Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Documentatie over de Powershell [Azure Resource Manager verkeer Manager ondersteunt][ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
