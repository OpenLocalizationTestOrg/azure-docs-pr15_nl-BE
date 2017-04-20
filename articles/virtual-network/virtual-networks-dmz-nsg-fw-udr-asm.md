<properties
   pageTitle="DMZ voorbeeld – een DMZ netwerken met een Firewall, UDR en NSG beschermen bouwen | Microsoft Azure"
   description="Bouwen van een DMZ met een Firewall, door de gebruiker gedefinieerde routering (UDR) en netwerk-beveiligingsgroepen (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Voorbeeld 3: een DMZ netwerken met een Firewall, UDR en NSG beschermen bouwen

[Terug naar de grens van beste praktijken beveiligingspagina][HOME]

In dit voorbeeld wordt een DMZ gemaakt met een firewall, vier windows-servers, gebruiker gedefinieerd routering, het doorsturen van IP- en beveiligingsgroepen netwerk. Het helpt ook via de desbetreffende opdrachten voor een beter begrip van de verschillende stappen. Er is ook een sectie verkeer Scenario met een gedetailleerde stapsgewijze hoe verkeer wordt uitgevoerd door de lagen van defensie in de DMZ. Ten slotte, in de referenties sectie is de volledige code en de instructies in deze omgeving om te testen en experimenteren met verschillende scenario's te bouwen. 

![Bi-directionele DMZ met NVA, NSG en UDR][1]

## <a name="environment-setup"></a>Omgeving opzetten
In dit voorbeeld is er een abonnement dat het volgende bevat:

- Drie cloud services: "SecSvc001", "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk "CorpNetwork" met drie subnetten: 'SecNet', 'FrontEnd' en 'End'
- Een virtueel netwerkapparaat, in dit voorbeeld een firewall, die is aangesloten op het subnet van de SecNet
- Een Windows-Server met een web application server ("IIS01")
- Twee windows-servers die toepassing terug end servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

In de sectie Verwijzingen hieronder is er een PowerShell script dat de meeste van de hierboven beschreven omgeving wordt opgebouwd. Bouwen van het VMs en virtuele netwerken Hoewel zijn gedaan door het voorbeeldscript zijn niet gedetailleerd beschreven in dit document.

Maken van de omgeving:

  1.    Het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de namen, de locatie en de IP-adressen overeenkomt met het gegeven scenario)
  2.    Bijwerken van de gebruikersvariabelen in het script aan het milieu wordt het script uitgevoerd tegen (abonnementen, servicenamen, enz)
  3.    Het script uitvoeren in PowerShell

**Opmerking**: het gebied dat wordt aangegeven in het PowerShell script moet overeenkomen met de regio die is aangegeven in het netwerk van het XML-configuratiebestand.

Zodra het script met succes wordt uitgevoerd kunnen de volgende na scriptstappen worden genomen:

1.  De firewall-regels instellen, hierover vindt u in de sectie met de titel: Beschrijving van de Firewall regel.
2.  Desgewenst zijn in de sectie Verwijzingen twee scripts voor het instellen van de webserver en server met een eenvoudige webtoepassing te kunnen testen met deze configuratie DMZ app.

Zodra het script met succes wordt uitgevoerd de firewall regels moeten worden voltooid, dit vindt u in de sectie met de titel: Firewall-regels.

## <a name="user-defined-routing-udr"></a>Door de gebruiker gedefinieerde routering (UDR)
De volgende systeem routes worden standaard gedefinieerd als:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

De VNETLocal is altijd de prefix(es) gedefinieerd adres van de VNet voor dat specifieke netwerk (ie verandert van VNet VNet afhankelijk van hoe elke specifieke VNet wordt gedefinieerd). De resterende systeem routes zijn statisch en standaard als hierboven.

Als prioriteit voor routes via de langste voorvoegsel Match (LPM)-methode worden verwerkt, waardoor de meest specifieke routes in de tabel gelden voor een bepaald doeladres.

Daarom zou verkeer (bijvoorbeeld naar de server DNS01, 10.0.2.4) voor het lokale netwerk (10.0.0.0/16) worden gerouteerd via de VNet naar de bestemming als gevolg van de 10.0.0.0/16-route. Met andere woorden, voor 10.0.2.4, de 10.0.0.0/16 route is de meest specifieke route, zelfs als de 10.0.0.0/8 en de 0.0.0.0/0 ook kunnen toepassen, maar omdat ze minder specifieke ze hebben geen invloed op het verkeer. Het verkeer naar 10.0.2.4 zou een volgende hop van de lokale VNet hebben en dus gewoon routeren naar de bestemming.

Als het verkeer is bedoeld voor 10.1.1.1 bijvoorbeeld de route 10.0.0.0/16 wouldn't toepassen, maar de 10.0.0.0/8 zou de meest specifieke en het verkeer is dit neergezet ("zwart gaan') de volgende hop is Null. 

Als de bestemming aan de voorvoegsels Null of de prefixen VNETLocal hebt toegepast, wordt deze aan de specifieke voldoen zou route, 0.0.0.0/0 en die met het Internet worden gerouteerd als de volgende hop en dus buiten de rand van de Azure-internet.

Als er twee identieke voorvoegsels in de tabel ziet hieronder u de volgorde van voorkeur op basis van het kenmerk van de "bron" routes:

1.  "VirtualAppliance" = een gebruiker gedefinieerde Route handmatig worden toegevoegd aan de tabel
2.  "VPNGateway" een dynamische Route (BGP gebruikt in combinatie met hybride netwerken) = toegevoegd door een dynamische netwerkprotocol, deze routes kunnen wijzigen als het protocol voor dynamische wordt automatisch in peered netwerk aangepast
3.  'Standaard' = het systeem Routes, de lokale VNet en statische vermeldingen zoals in de bovenstaande tabel.

>[AZURE.NOTE] U kunt nu gebruiker gedefinieerd routering (UDR) met ExpressRoute en VPN-gateway gebruiken om binnenkomende en uitgaande verkeer tussen lokale moet worden doorgestuurd naar een virtueel netwerkapparaat (NVA).

#### <a name="creating-the-local-routes"></a>De lokale routes maken

In dit voorbeeld zijn twee routeringstabellen nodig, één voor de front-end en back-end-subnetten. Elke tabel wordt geladen met statische routes die geschikt zijn voor een bepaald subnet. In dit voorbeeld heeft elke tabel drie routes:

1. Lokale subnetverkeer met geen volgende Hop gedefinieerd voor het lokale subnetverkeer op de firewall omzeilen
2. Virtuele netwerkverkeer met een volgende Hop gedefinieerd als firewall, hierdoor wordt de standaardregel die lokale VNet verkeer rechtstreeks doorsturen
3. Alle resterende verkeer (0/0) met een volgende Hop gedefinieerd als de firewall

Zodra de routeringstabellen zijn gemaakt zijn ze gebonden aan hun subnetten. Voor het subnet Frontend routeringstabel eenmaal gemaakt en gekoppeld aan het subnet ziet er zo uit:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


In dit voorbeeld de volgende opdrachten worden gebruikt om te bouwen de routetabel, een route door de gebruiker gedefinieerd toevoegen en vervolgens de routetabel binden aan een subnet (Opmerking; de items onder beginnen met een dollarteken (bijvoorbeeld: $BESubnet) door gebruiker gedefinieerde variabelen uit het script in de sectie Verwijzingen van dit document zijn):

1.  Eerst moet de basis routeringstabel worden gemaakt. In dit fragment wordt het maken van de tabel voor het subnet Backend. In het script is ook een bijbehorende tabel gemaakt voor het subnet Frontend.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Als de tabel is gemaakt, kunnen specifieke gebruiker gedefinieerde routes worden toegevoegd. In deze snipped worden al het verkeer (0.0.0.0/0) verstuurd via het virtuele toestel (een variabele, $VMIP [0], wordt gebruikt om door te geven in het IP-adres toegewezen wanneer het virtuele toestel eerder in het script is gemaakt). In het script, wordt ook een corresponderende regel gemaakt in de tabel Frontend.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. De bovenstaande route post overschrijft de standaard "0.0.0.0/0" route, maar de standaardregel 10.0.0.0/16 nog steeds bestaande waardoor verkeer binnen de VNet worden doorgestuurd naar de bestemming en niet op het netwerk virtuele toestel. Juist dit probleem op de volgende regel moet worden toegevoegd.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. Op dit moment is er een keuze worden gemaakt. Met de bovenstaande twee routes doorsturen alle verkeer naar de firewall voor de beoordeling, zelfs verkeer binnen één subnet. Dit kan nodig zijn, maar dat verkeer binnen een subnet te routeren lokaal zonder betrokkenheid van de firewall van een derde, zeer specifieke regel kan worden toegevoegd. Deze route dat elk adres voor het lokale subnet kan gewoon destine route er rechtstreeks (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Ten slotte met de routeringstabel gemaakt en gevuld met een door de gebruiker gedefinieerd routes, moet de tabel nu worden gebonden aan een subnet. In het script, is de routetabel front-end ook afhankelijk van het subnet Frontend. Hier is het script binding voor het subnet van de back-end.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>Doorsturen via IP
Een aanvullende functie voor UDR, is IP-doorsturen. Dit is een instelling op een virtueel toestel waarmee het verkeer niet specifiek gericht op het toestel ontvangen en doorgestuurd dat verkeer naar de uiteindelijke bestemming.

Als u bijvoorbeeld als u verkeer van AppVM01 een aanvraag naar de server DNS01 zou UDR route dit met de firewall. Met IP-Forwarding ingeschakeld, wordt het verkeer voor de bestemming van de DNS01 (10.0.2.4) worden geaccepteerd door het toestel (adres 10.0.0.4) en vervolgens doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Doorsturen via IP op de Firewall ingeschakeld, zonder dat zou verkeer niet aanvaardbaar zijn door het toestel, hoewel de routetabel de firewall als de volgende hop is. 

>[AZURE.IMPORTANT] Het is essentieel om te onthouden IP-doorsturen inschakelen in combinatie met de gebruiker gedefinieerde routering.

Doorsturen via IP instellen is één opdracht en op tijd VM kan worden uitgevoerd. Voor de stroom van dit voorbeeld is het codefragment aan het einde van het script en gegroepeerd met de opdrachten UDR:

1.  Bel de VM-instantie die in dit geval uw virtueel toestel, de firewall en IP-doorsturen inschakelen (Opmerking; een willekeurig item in rode die beginnen met een dollarteken (bijvoorbeeld: $VMName[0]) is een door gebruiker gedefinieerde variabele van het script in de sectie Verwijzingen van dit document. De nul tussen vierkante haken [0], vertegenwoordigt de eerste VM in de matrix van VMs, voor het voorbeeldscript zonder wijzigingen, de eerste VM (VM 0) moet de firewall):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Netwerk-beveiligingsgroepen (NSG)
In dit voorbeeld wordt een groep NSG gebouwd en vervolgens met een enkele regel worden geladen. Deze groep wordt vervolgens uitsluitend gekoppeld aan de front-end en back-end-subnetten (niet SecNet). Declaratief wordt opgebouwd voor de volgende regel:

1.  Al het verkeer (alle poorten) van het Internet naar het hele VNet (alle subnetten) is geweigerd.

Hoewel in dit voorbeeld NSGs gebruikt worden, is het belangrijkste doel als een secundaire verdedigingslinie tegen onjuiste voor handmatige configuratie. We willen de Frontend voor alle binnenkomend verkeer vanaf het internet te blokkeren of back-end subnetten, verkeer alleen via het subnet SecNet met de firewall moeten de stroming (en vervolgens indien nodig bij de front-end- of back-end subnetten). Plus, met de UDR regels, zou alle verkeer dat het in de front-end- of back-end subnetten maken worden geleid uit met de firewall (dankzij UDR). De firewall ziet dit als een asymmetrische stroom en het uitgaande verkeer zou dalen. Er zijn dus drie lagen van beveiliging beschermt de Frontend en Backend subnetten; 1) geen open eindpunten op de FrontEnd001 en BackEnd001 cloud services, 2) NSGs verkeer vanaf het Internet, 3) de firewall weghalen asymmetrische verkeer te weigeren.

Een interessant punt met betrekking tot de beveiligingsgroep netwerk in dit voorbeeld is bevat slechts één regel, die hieronder worden weergegeven die is het blokkeren van internet-verkeer op het gehele virtuele netwerk, waaronder de beveiliging subnet. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Echter, aangezien de NSG is alleen afhankelijk van de front-end en back-end subnetten, de regel niet verwerkt op verkeer in het subnet van de beveiliging. Als gevolg hiervan, hoewel de NSG volgens de regel geen Internet-verkeer op elk adres in de VNet, omdat de NSG is nooit afhankelijk van het subnet van de beveiliging, zal verkeer doorstromen naar het subnet van de beveiliging.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Firewall-regels
Op de firewall moet doorsturen van regels worden gemaakt. Aangezien de firewall verkeer blokkeert of doorsturen van alle inkomende, uitgaande en intra-VNet zijn veel firewall-regels nodig. Bovendien wordt al het binnenkomende verkeer Security Service openbare IP-adres (op verschillende poorten), druk moet worden verwerkt door de firewall. Beste is het diagram van de logische stroom voor het instellen van de subnetten en firewall-regels om te voorkomen dat later bijwerken. In de volgende afbeelding is een logische weergave van de firewall-regels voor dit voorbeeld:
 
![Logische weergave van de Firewall-regels][2]

>[AZURE.NOTE] Op basis van het virtuele netwerk-toestel gebruikt, zal de poorten voor beheertaken variëren. In het volgende voorbeeld wordt verwezen naar een Barracuda NextGen Firewall die poort 22, 801 en 807 gebruikt. Raadpleeg de documentatie van de leverancier toestel om te vinden welke poorten gebruikt voor het beheer van het apparaat wordt gebruikt.

### <a name="logical-rule-description"></a>Beschrijving van de logische regel
In het bovenstaande logisch diagram het subnet beveiliging niet weergegeven omdat de firewall de enige bron op dat subnet is, en in dit diagram wordt weergegeven, de firewall-regels en hoe ze logisch toestaan of verkeersstromen en niet de werkelijke gerouteerde pad weigeren. Ook de externe poorten geselecteerd voor het RDP-verkeer zijn hogere varieerde poorten (8014 – 8026) en enigszins uitlijnen met de laatste twee octets van het lokale IP-adres voor de leesbaarheid gemakkelijker zijn geselecteerd (bijvoorbeeld adres van de lokale server 10.0.1.4 is gekoppeld aan een externe poort 8014), maar geen hogere niet-conflicterende poorten kunnen worden gebruikt.

In dit voorbeeld hebben we 7 typen regels nodig, deze typen worden beschreven als volgt:

- Externe regels (voor binnenkomend verkeer):
  1.    Management firewallregel: Deze regel App omleiding kunt verkeer worden doorgegeven aan de poorten voor beheertaken van het virtuele netwerk-toestel.
  2.    RDP-regels (voor elke windows server): beheer van de afzonderlijke servers via RDP zal toestaan dat deze vier regels (één voor elke server). Dit kan ook worden gebundeld in één regel zijn afhankelijk van de mogelijkheden van het virtuele netwerk-toestel wordt gebruikt.
  3.    Regels voor het verkeer van toepassing: Er zijn twee toepassing verkeer regels, de eerste voor het web-front-end verkeer en de tweede voor de back-end-verkeer (bv webserver gegevenslaag). De configuratie van deze regels zal afhankelijk zijn van de netwerkarchitectuur (waar uw servers worden geplaatst) en stromen verkeer (welke richting de verkeersstromen en welke poorten worden gebruikt).
      - De eerste regel kan de eigenlijke toepassing verkeer de application server. Terwijl de andere regels voor de beveiliging, beheer, enz., worden regels van toepassing zijn wat kunnen externe gebruikers of services voor toegang tot de toepassingen. In dit voorbeeld is er één webserver op poort 80, dus een firewallregel voor één toepassing inkomend verkeer omleiden naar de externe IP-adres, aan het interne IP-adres van de web-servers. De sessie omgeleid verkeer zou worden NAT moest de interne server.
      - De tweede regel van toepassing verkeer is de regel van de back-end Neem contact op met de AppVM01-server (maar niet AppVM02) van de webserver via elke poort toestaan.
- Interne regels (voor verkeer van intra-VNet)
  4.    Uitgaande Internet-regel: deze regel wordt verkeer met een netwerk worden doorgegeven aan de geselecteerde netwerken toestaan. Deze regel is meestal een standaardregel voor reeds op de firewall, maar uitgeschakeld. Deze regel moet worden ingeschakeld voor dit voorbeeld.
  5.    DNS-regel: Deze regel kan alleen het verkeer worden doorgegeven aan de DNS-server DNS (poort 53). Voor deze omgeving die meeste verkeer van de front-end naar de Backend wordt geblokkeerd, staat deze regel specifiek DNS van een lokaal subnet.
  6.    Subnet-Subnet regel: deze regel is dat elke server op de back-end subnet verbinding maken met een server op het subnet-front-end (maar niet andersom).
- Fail-Safe regel (voor verkeer dat niet voldoet aan een van de bovenstaande):
  7.    Alle verkeer regel weigeren: Dit moet altijd de laatste regel (in termen van prioriteit), en als zodanig als een verkeersstromen niet overeenkomt met een van bovenstaande regels die er door deze regel worden weggehaald. Dit is een standaardregel en meestal geactiveerd, worden geen wijzigingen in het algemeen nodig.

>[AZURE.TIP] Elke poort is toegestaan voor eenvoudig voorbeeld in een echte scenario de meest specifieke poort op de tweede toepassing verkeer regel en adresbereiken moeten worden gebruikt voor het verminderen van de kwetsbaarheid van deze regel.

<br />

>[AZURE.IMPORTANT] Zodra alle van de bovenstaande regels zijn gemaakt, is het belangrijk dat u de prioriteit van elke regel zodat het verkeer wordt toegestaan of geweigerd indien gewenst. In dit voorbeeld worden de regels in volgorde van prioriteit. Het is eenvoudig om de firewall als gevolg van verkeerd bestelde regels worden vergrendeld. Zorg ervoor dat het beheer van de firewall zelf is altijd de absolute hoogste prioriteit regel ten minste.

### <a name="rule-prerequisites"></a>Voorwaarden voor de regel
Een eerste vereiste voor de virtuele Machine met de firewall zijn openbare eindpunten. De eindpunten openbaar moeten zijn geopend voor de firewall verkeer verwerken. Er zijn drie typen verkeer in dit voorbeeld; RDP-verkeer 1) beheer van verkeer voor het beheer van de firewall en firewall-regels, 2) om de windows-servers en 3) de programmaoverdracht. Dit zijn de drie kolommen van de typen verkeer in de bovenste helft van logische weergave van de firewall-regels hierboven.

>[AZURE.IMPORTANT] Hier een belangrijke takeway is te onthouden dat **al** het verkeer via de firewall wordt geleverd. Dus met extern bureaublad naar de server IIS01, ook al dit in de Front End Cloud-Service en klik op het subnet-Front-End is voor toegang tot deze server we RDP moet om de firewall op poort 8014 en laat de firewall aan intern de aanvraag wordt doorgestuurd naar de RDP-poort IIS01. De Azure portal "Verbinden" knop werkt niet omdat er geen directe RDP pad naar IIS01 (voorzover de portal zien kan). Dit betekent dat alle verbindingen vanaf het internet worden op de Security Service en een poort, bijvoorbeeld secscv001.cloudapp.net:xxxx (Zie de bovenstaande afbeelding voor de toewijzing van de externe poort en interne IP en poort).

Een eindpunt kan worden geopend op het moment van VM maken of bouwen boeken als is gedaan in het voorbeeldscript in dit codefragment hieronder (Opmerking; elk item die beginnen met een dollarteken (bijvoorbeeld: $VMName[$i]) is een door gebruiker gedefinieerde variabele van het script in de sectie Verwijzingen van dit document. "$I" tussen haakjes, [$i], geeft het nummer van de matrix van een specifiek VM in een array van VMs):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Hoewel het niet duidelijk wordt aangetoond dat dit als gevolg van het gebruik van variabelen, maar de eindpunten zijn **alleen** geopend op de Cloud Security Service. Dit is om ervoor te zorgen dat al het binnenkomende verkeer wordt behandeld (gerouteerd, NAT was, verloren) door de firewall.

Een management-client moet zijn geïnstalleerd op een PC voor het beheren van de firewall en de configuraties nodig maken. Zie de documentatie van uw firewall (of andere NVA)-leverancier voor het beheren van het apparaat. De rest van deze sectie en de volgende sectie, maken van de Firewall-regels, wordt de configuratie van de firewall zelf, door middel van de leveranciers management-client (dat wil zeggen niet de Azure portal of PowerShell) beschreven.

Instructies voor het downloaden van de client en verbinding te maken met de Barracuda gebruikt in dit voorbeeld vindt u hier: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Nadat u bent aangemeld op de firewall, maar voordat u firewallregels maakt, zijn er twee vereiste objectklassen waardoor maakt de regels gemakkelijker; Netwerk & Service-objecten.

In dit voorbeeld moet drie benoemde netwerkobjecten zijn gedefinieerd (een voor het subnet van de front-end en de back-end-subnet, ook een netwerkobject voor het IP-adres van de DNS-server). Voor het maken van een netwerk met de naam. vanaf de client Barracuda NG Admin dashboard, Ga naar het tabblad configuratie, in de sectie configuratie van operationele Ruleset, vervolgens klikt u op 'Netwerken' onder het menu Firewall-objecten, klik op Nieuw in het menu Bewerken-netwerken. Het netwerkobject kan nu worden gemaakt, door de naam en het voorvoegsel toe te voegen:

![Een netwerk FrontEnd-Object maken][3]
 
Hiermee maakt u een netwerk met de naam voor het subnet FrontEnd, een vergelijkbaar object moet worden gemaakt voor het subnet BackEnd ook. De subnetten kunnen nu eenvoudig verwezen met de naam in de firewall-regels.

Voor het Object op de DNS-Server:

![Een DNS-Server-Object maken][4]

In een DNS-regel in het document later dit één IP-Adresverwijzing gebruikt.

De tweede vereiste objecten zijn objecten van de Services. Deze staan voor de poorten van de RDP-verbinding voor elke server. Aangezien het bestaande RDP-object is gebonden aan de standaard RDP-poort, kunnen 3389, nieuwe diensten worden gemaakt voor verkeer vanaf de externe poorten (8014 8026). De nieuwe poorten kunnen ook worden toegevoegd aan de bestaande RDP-service, maar voor de demonstratie te vereenvoudigen, een regel voor elke server worden gemaakt. Voor het maken van een nieuwe RDP-regel voor een server. vanaf de client Barracuda NG Admin dashboard, Ga naar het tabblad configuratie, klik in de sectie configuratie van operationele Ruleset, en vervolgens klikt u op 'Services' in het menu Firewall-objecten, schuif naar beneden in de lijst met services en selecteer de 'RDP' service. Klik met de rechtermuisknop en selecteer kopiëren, en vervolgens met de rechtermuisknop op en selecteer plakken. Er is nu een RDP-Copy1-Object dat kan worden bewerkt. RDP-Copy1 met de rechtermuisknop en selecteer bewerken, de Service Object bewerken venster omhoog zoals hier wordt weergegeven:

![Kopie van de standaard RDP-regel][5]

De waarden kunnen worden bewerkt voor de RDP-service voor een specifieke server. Voor AppVM01 de bovenstaande standaard RDP-regel moet worden gewijzigd om aan te geven een nieuwe naam, beschrijving en externe RDP-poort die wordt gebruikt in het diagram in figuur 8 (Opmerking: de poorten zijn gewijzigd van de standaard RDP van 3389 aan de externe poort voor deze specifieke server wordt gebruikt, in het geval van AppVM01 is de externe poort 8025) de gewijzigde service wordt hieronder weergegeven. :

![AppVM01 regel][6]
 
Dit proces moet worden herhaald voor het maken van de RDP-Services op de resterende servers; AppVM02, DNS01 en IIS01. Het maken van deze Services maakt het maken van regels eenvoudiger en duidelijker in de volgende sectie.

>[AZURE.NOTE] Een RDP-service voor de Firewall niet nodig is om twee redenen; 1) eerst de firewall VM een afbeelding op basis van Linux is SSH op poort 22 zou worden gebruikt voor VM-beheer in plaats van RDP en 2) poort 22 en twee andere poorten voor beheertaken zijn toegestaan in de eerste beheerregel in zoals hieronder beschreven voor beheer verbindingen toestaan.

### <a name="firewall-rules-creation"></a>Maken van de firewall-regels
Er zijn drie typen firewallregels in dit voorbeeld gebruikt, deze hebben allemaal verschillende pictogrammen:

De regel voor het omleiden van toepassing: ![Toepassingspictogram omleiden][7]

De bestemming NAT regel: ![Bestemming NAT-pictogram][8]

De Pass regel: ![Pictogram doorgeven][9]

U vindt meer informatie over deze regels op de website van Barracuda.

Vanaf de client Barracuda NG Admin dashboard, Ga naar het tabblad configuratie, in de operationele configuratie sectie Ruleset klikt u op de volgende regels maken (of bestaande standaardregels controleren). Een raster wordt genoemd "Main regels" worden de bestaande actieve als gedeactiveerde regels weergegeven op deze firewall. In de rechterbovenhoek van deze raster is een kleine, groene "+" knop, klik hierop om een nieuwe regel te maken (Opmerking: de firewall kan worden "vergrendeld" gewijzigd, verschijnt een knop "Vergrendelen" gemarkeerd als niet maken of bewerken van regels, klikt u op deze knop om te 'ontgrendelen' de regelset en bewerken toestaan). Als u wilt voor het bewerken van een bestaande regel, selecteert u die regel met de rechtermuisknop op en selecteer de regel bewerken.

Zodra uw regels zijn gemaakt en/of gewijzigd, moet de firewall wordt geduwd en wordt geactiveerd, als dit niet gebeurt de regel wijzigingen worden niet doorgevoerd. Het proces van push en activering vindt u hieronder de beschrijvingen van de regel details.

De specifieke kenmerken van elke regel die in dit voorbeeld worden als volgt beschreven:

- **Management firewallregel**: regel omleiden van deze App kan verkeer worden doorgegeven aan de poorten van het beheer van het netwerk virtuele apparaat, in dit voorbeeld een Barracuda NextGen Firewall. De poorten voor beheertaken zijn 801, 807 en eventueel 22. De interne en externe poorten zijn hetzelfde (dat wil zeggen geen port translation). Deze regel, SETUP-MGMT-toegang, is een standaardregel en standaard ingeschakeld (in Firewall voor Barracuda NextGen versie 6.1).

    ![Firewallregel Management][10]

>[AZURE.TIP] De bron-adresruimte in deze regel is, als het beheer van IP-adresbereiken bekend zijn, waardoor deze scope zou ook Verklein het aanvalsoppervlak om de poorten voor beheertaken.

- **RDP-regels**: deze bestemming NAT regels kunnen het beheer van de afzonderlijke servers via RDP.
Er zijn vier belangrijke velden die nodig zijn om deze regel te maken:
  1.    Bron: RDP dat overal de verwijzing "Willekeurig" wordt gebruikt in het veld bron.
  2.    Service – met de juiste serviceobject eerder hebt gemaakt, in dit geval "RDP AppVM01", externe poorten doorsturen naar het lokale IP-adres en poort 3386 (de standaard RDP-poort). Deze specifieke regel is voor RDP-toegang tot AppVM01.
  3.    Doel: moet de *lokale poort op de firewall*, 'DCHP 1 lokale IP-' of eth0 als statische IP-adressen gebruikt. De ordinale waarde (eth0, eth1, etc) kan afwijken als uw netwerk toestel meerdere lokale interfaces heeft. Dit is de poort van de firewall is verzenden (mogelijk dezelfde als de ontvangende poort), de werkelijke bestemming van de route is in het veld doel.
  4.    Omleiding – deze sectie vertelt het virtuele toestel waar uiteindelijk dit verkeer omleiden. De eenvoudigste omleiding is om het IP en de poort (optioneel) in het veld doel. Als er geen poort wordt gebruikt is de bestemmingspoort van de binnenkomende aanvraag (ie geen vertaling) gebruikt als een poort is de poort toegewezen worden ook NAT zou samen met het IP adres.

    ![RDP firewallregel][11]

    Een totaal van vier RDP-regels moet worden gemaakt: 

  	|   Naam van de regel    | Server  |   Service   |  Doellijst  |
  	|----------------|---------|-------------|---------------|
  	| RDP-IIS01-   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP-DNS01-   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP-AppVM01- | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP-AppVM02- | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] U het bereik van de bron- en velden beperken vermindert aanvallen. De meest beperkte mogelijkheden waardoor functionaliteit moet worden gebruikt.

- **Regels voor het verkeer van toepassing**: Er zijn twee toepassing verkeer regels, de eerste voor het web-front-end verkeer en de tweede voor de back-end-verkeer (bv webserver gegevenslaag). Deze regels zullen afhankelijk zijn van de netwerkarchitectuur (waar uw servers worden geplaatst) en stromen verkeer (welke richting de verkeersstromen en welke poorten worden gebruikt).

    Eerst worden besproken, is de regel front-end voor webverkeer:

    ![Web firewallregel][12]
 
    Deze regel bestemming NAT kan het verkeer toepassing te bereiken van de application server. Terwijl de andere regels voor de beveiliging, beheer, enz., worden regels van toepassing zijn wat kunnen externe gebruikers of services voor toegang tot de toepassingen. In dit voorbeeld is er één webserver op poort 80, dus de firewallregel voor één toepassing inkomend verkeer omleiden naar de externe IP-adres, aan het interne IP-adres van de web-servers.

    **Opmerking**: dat er geen poort is toegewezen in het veld doel, dus de binnenkomende poort 80 (of 443 voor de geselecteerde Service) wordt gebruikt in de omleiding van de webserver. Als de server luistert op een andere poort, bijvoorbeeld poort 8080, het veld doel kan worden bijgewerkt naar 10.0.1.4:8080 voor de poort-omleiding toestaan.

    De volgende regel van toepassing verkeer is de back-end regel toestaan Neem contact op met de AppVM01-server (maar niet AppVM02) van de webserver via een service:

    ![Firewallregel AppVM01][13]

    Deze Pass regel kunt u een IIS-server op het subnet Frontend voor het bereiken van de AppVM01 (IP-adres 10.0.2.5) op elke poort, met behulp van elk Protocol dat toegang heeft tot gegevens die nodig zijn voor de webtoepassing.

    In deze schermafbeelding een "\<expliciete doel\>' in het veld doel wordt gebruikt om 10.0.2.5 als doel aan te geven. Dit kan zijn van een expliciete zoals een naam of netwerk-Object (zoals in de vereisten voor de DNS-server). Dit is de taak van de beheerder van de firewall welke methode wordt gebruikt. 10.0.2.5 als een Explict Desitnation toevoegen, dubbelklikt u op de eerste lege rij onder \<expliciete doel\> en voer het adres in het venster dat verschijnt.

    Met deze regel geven geen NAT nodig aangezien dit interne verkeer, zodat de verbindingsmethode kan worden ingesteld op "No SNAT".

    **Opmerking**: de Bronnetwerk in deze regel wordt elke resource in het subnet FrontEnd als er slechts één of een bekende bepaald aantal webservers, een Object Network resource kan worden gemaakt om meer specifiek zijn voor die exacte IP-adressen in plaats van het gehele Frontend subnet.

>[AZURE.TIP] Deze regel maakt gebruik van de service "Any" de voorbeeldtoepassing om beter te maken en te gebruiken, ook hierdoor ICMPv4 (ping) in een enkele regel. Dit is echter niet aanbevolen. De poorten en protocollen ('Services') moeten op de minimaal mogelijke toepassing bewerking Verklein het aanvalsoppervlak over deze grens kan worden teruggebracht.

<br />

>[AZURE.TIP] Hoewel deze regel bevat een verwijzing naar een expliciete doel wordt gebruikt, moet een consistente aanpak in de configuratie van de firewall worden gebruikt. Het verdient aanbeveling het netwerkobject benoemd worden gebruikt in de gehele voor eenvoudiger leesbaarheid en ondersteuning. Expliciete doel wordt hier alleen gebruikt om een alternatieve referentiemethode weergeven en wordt over het algemeen niet aanbevolen (vooral voor complexe configuraties).

- **Uitgaande Internet-regel**: doorgeven van deze regel kan verkeer van elk Bronnetwerk moet worden doorgegeven aan de geselecteerde bestemming netwerken. Deze regel is een standaardregel voor meestal al op de firewall Barracuda NextGen, maar is uitgeschakeld. Met de rechtermuisknop op deze regel, kan toegang krijgen tot de opdracht regel activeren. De hier weergegeven regel is de twee lokale subnetten die zijn gemaakt als de verwijzingen in de vereiste sectie van dit document toevoegen aan het bronkenmerk van deze regel gewijzigd.

    ![Uitgaande firewallregel][14]

- **DNS-regel**: doorgeven van deze regel kan alleen DNS (poort 53) verkeer worden doorgegeven aan de DNS-server. Voor deze omgeving die meeste verkeer van de front-end naar de BackEnd wordt geblokkeerd, kan deze regel specifiek DNS.

    ![DNS-firewallregel][15]

    **Opmerking**: In dit scherm de verbindingsmethode shot is opgenomen. Omdat deze regel is voor interne IP verkeer van interne IP-adres, geen NATing is vereist, wordt deze methode voor de verbinding is ingesteld op "No SNAT" voor deze regel Pass.

- **Subnet-Subnet regel**: doorgeven van deze regel is een standaardregel die is geactiveerd en dat elke server op de back-end subnet verbinding maken met een server op het subnet-front-end is gewijzigd. Deze regel is alle interne verkeer, zodat de verbindingsmethode kan worden ingesteld op Nee SNAT.

    ![Firewallregel Intra-VNet][16]

    **Opmerking**: het selectievakje bidirectionele niet is ingeschakeld (het is evenmin gecontroleerd in de meeste regels), dit is van belang voor deze regel is het deze regel "eenrichtings", een verbinding kan worden gestart vanaf de back-end-subnet aan de front-end-netwerk, maar niet andersom. Als dit selectievakje is ingeschakeld, zou deze regel bi-directionele verkeer, dat uit onze logisch diagram niet inschakelen.

- **Alle verkeer regel weigeren**: dit moet altijd de laatste regel (in termen van prioriteit), en als zodanig als een verkeersstromen niet overeenkomt met een van de voorgaande regels genegeerd door deze regel. Dit is een standaardregel en meestal geactiveerd, worden geen wijzigingen in het algemeen nodig. 

    ![Firewall regel weigeren][17]

>[AZURE.IMPORTANT] Zodra alle van de bovenstaande regels zijn gemaakt, is het belangrijk dat u de prioriteit van elke regel zodat het verkeer wordt toegestaan of geweigerd indien gewenst. In dit voorbeeld worden de regels in de volgorde waarin die ze moeten worden weergegeven in het raster van de belangrijkste regels in de Barracuda Management-Client door te sturen.

## <a name="rule-activation"></a>Regel activeren
Met de ruleset gewijzigd in de specificatie van het diagram logica, moet de ruleset worden geüpload naar de firewall en vervolgens geactiveerd.

![Firewall regel activeren][18]
 
In de rechterbovenhoek van de client management zijn een cluster van knoppen. Klik op de knop 'Wijzigingen verzenden' om de gewijzigde regels om de firewall te verzenden en klik vervolgens op de knop 'Activeren'.
 
In dit voorbeeld omgeving build is met de activering van de ruleset firewall voltooid.

## <a name="traffic-scenarios"></a>Verkeer-scenario 's
>[AZURE.IMPORTANT] Een belangrijke takeway is te onthouden dat **al** het verkeer via de firewall wordt geleverd. Dus met extern bureaublad naar de server IIS01, ook al dit in de Front End Cloud-Service en klik op het subnet-Front-End is voor toegang tot deze server we RDP moet om de firewall op poort 8014 en laat de firewall aan intern de aanvraag wordt doorgestuurd naar de RDP-poort IIS01. De Azure portal "Verbinden" knop werkt niet omdat er geen directe RDP pad naar IIS01 (voorzover de portal zien kan). Dit betekent dat alle verbindingen vanaf het internet worden op de Security Service en een poort, bijvoorbeeld secscv001.cloudapp.net:xxxx.

Voor deze scenario's moet de firewall-regels in de volgende plaats:

1.  Beheer van Firewall
2.  RDP naar IIS01
3.  RDP naar DNS01
4.  RDP naar AppVM01
5.  RDP naar AppVM02
6.  Verkeer op het Web App
7.  App verkeer naar AppVM01
8.  Uitgaande Internet
9.  Frontend voor DNS01
10. Intra-Subnet-verkeer (back-end naar alleen front-end)
11. Alles weigeren

De werkelijke firewall ruleset waarschijnlijk heeft daarnaast veel regels, moet de regels op een bepaalde firewall ook verschillende prioriteitsnummers dan de hier vermelde. Deze lijst en de bijbehorende nummers zijn voor relevantie tussen alleen deze elf regels en de relatieve prioriteit onder hen. Met andere woorden; de "RDP naar IIS01" kan de werkelijke firewall regelnummer 5 zijn, maar zo is het onder de regel "Firewall Management" en boven de regel 'RDP-DNS01' wilt uitlijnen met de bedoeling van deze lijst. De lijst helpt ook bij de onderstaande scenario's waarin kort te houden; bijvoorbeeld ' FW regel 9 (DNS) '. Ook kort te houden, de vier RDP regels zullen worden genoemd, "RDP regels" wanneer het verkeer scenario is gerelateerd aan RDP.

Ook intrekken of beveiligingsgroepen netwerk ter plaatse zijn voor binnenkomend gegevensverkeer op de front-end en back-end-subnetten.

#### <a name="allowed-internet-to-web-server"></a>(Toegestaan) Internet Web Server
1.  HTTP-pagina van Internet gebruiker aanvragen van SecSvc001.CloudApp.Net (Internet Facing Cloud Service)
2.  Cloud service gangen verkeer via open eindpunt voor poort 80 op de interface van de firewall op 10.0.0.4:80
3.  Geen NSG toegewezen aan beveiliging subnet, zodat NSG Systeemregels verkeer firewall toestaan
4.  Verkeer treffers interne IP-adres van de firewall (10.0.1.4)
5.  Firewall begint regel verwerking:
  1.    FW-regel 1 (Mgmt FW) niet van toepassing, naar volgende regel gaan
  2.    FW-regels (regels met RDP) 2-5 niet toepassen, naar de volgende regel gaan
  3.    FW regel 6 (App: Web) is van toepassing, verkeer wordt toegestaan, NAT firewall aan de 10.0.1.4 (IIS01)
6.  Het subnet Frontend begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (Internet blokkeren) is niet van toepassing (dit verkeer is NAT zou door de firewall, het bronadres is dus nu de firewall die op het subnet van de beveiliging en het subnet Frontend NSG "local" verkeer te zien en dus is toegestaan), verplaatsen naar de volgende regel
  2.    NSG standaardregels subnet-subnet verkeer toestaan, verkeer wordt toegestaan, NSG verwerking stoppen
7.  IIS01 voor Internet-verkeer luistert, dit verzoek ontvangt en start de verwerking van de aanvraag
8.  IIS01 pogingen om een FTP-sessie met AppVM01 op de Backend-subnet wordt geïnitieerd
9.  De route UDR op subnet Frontend maakt de firewall de volgende hop
10. Geen uitgaande regels op de Frontend subnet verkeer is toegestaan.
11. Firewall begint regel verwerking:
  1.    FW-regel 1 (Mgmt FW) niet van toepassing, naar volgende regel gaan
  2.    FW-regel 2-5 (RDP-regels) niet van toepassing, naar volgende regel gaan
  3.    FW regel 6 (App: Web) niet van toepassing, naar de volgende regel gaan
  4.    FW regel 7 (App: back-end) is van toepassing, verkeer is toegestaan, de firewall wordt doorgestuurd verkeer naar 10.0.2.5 (AppVM01)
12. Het subnet van de back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar volgende regel gaan
  2.    NSG standaardregels subnet-subnet verkeer toestaan, verkeer wordt toegestaan, NSG verwerking stoppen
13.  AppVM01 ontvangt de aanvraag en de sessie start en reageert
14. De route UDR op subnet Backend maakt de firewall de volgende hop
15. En omdat er geen uitgaande NSG-regels in het antwoord is toegestaan back-end subnet
16. Omdat dit verkeer op een bestaande sessie als resultaat stuurt de firewall het antwoord naar de server (IIS01)
17. Frontend subnet begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar volgende regel gaan
  2.    NSG standaardregels subnet-subnet verkeer toestaan, verkeer wordt toegestaan, NSG verwerking stoppen
18. De IIS-server ontvangt het antwoord, de transactie met AppVM01 en voltooit het HTTP-antwoord te bouwen, wordt dit HTTP-antwoord verzonden naar de aanvrager
19. Aangezien er geen uitgaande regels NSG op het subnet Frontend het antwoord is toegestaan
20. Het HTTP-antwoord raakt de firewall en omdat dit het antwoord op een bestaande NAT-sessie door de firewall wordt geaccepteerd
21. Vervolgens stuurt het antwoord naar de Internet-gebruiker de firewall
22. En omdat er geen uitgaande ontvangt regels NSG of UDR hops op het subnet Frontend is het antwoord is toegestaan en de Internet-gebruiker de gevraagde webpagina wordt weergegeven.

#### <a name="allowed-internet-rdp-to-backend"></a>(Toegestaan) Internet RDP naar back-end
1.  Server Admin op internet vraagt om RDP-sessie met AppVM01 via SecSvc001.CloudApp.Net:8025, waarbij 8025 het poortnummer van de gebruiker die is toegewezen voor de firewallregel "RDP-AppVM01 is"
2.  De cloud-service geeft verkeer via het eindpunt openen op poort 8025 aan de interface van de firewall op 10.0.0.4:8025
3.  Geen NSG toegewezen aan beveiliging subnet, zodat NSG Systeemregels verkeer firewall toestaan
4.  Firewall begint regel verwerking:
  1.    FW-regel 1 (Mgmt FW) niet van toepassing, naar volgende regel gaan
  2.    FW-regel 2 (RDP IIS) niet van toepassing, naar volgende regel gaan
  3.    FW regel 3 (RDP-DNS01) niet van toepassing, naar de volgende regel gaan
  4.    FW regel 4 (RDP-AppVM01) is van toepassing, het verkeer is toegestaan, firewall NAT te 10.0.2.5:3386 (RDP-poort op AppVM01)
5.  Het subnet van de back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (Internet blokkeren) is niet van toepassing (dit verkeer is NAT zou door de firewall, het bronadres is dus nu de firewall die op het subnet van de beveiliging en door de back-end-subnet NSG "local" verkeer worden gezien en is dus toegestaan), verplaatsen naar de volgende regel
  2.    NSG standaardregels subnet-subnet verkeer toestaan, verkeer wordt toegestaan, NSG verwerking stoppen
6.  AppVM01 voor RDP-verkeer luistert en reageert
7.  Met geen uitgaande regels NSG standaardregels toepassen en terugkerend verkeer is toegestaan.
8.  UDR routes voor uitgaand verkeer op de firewall als de volgende hop
9.  Omdat dit verkeer op een bestaande sessie als resultaat stuurt de firewall het antwoord naar de gebruiker internet
10. RDP-sessie is ingeschakeld.
11. AppVM01 vraagt om gebruikersnaam wachtwoord

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web Server DNS-lookup op de DNS-server
1.  Web Server, IIS01, moet een feed op www.data.gov, maar moet u het adres.
2.  De netwerkconfiguratie voor de lijsten van VNet DNS01 (10.0.2.4 op de Backend-subnet) als de primaire DNS-server, IIS01 stuurt de DNS-aanvraag naar DNS01
3.  UDR routes voor uitgaand verkeer op de firewall als de volgende hop
4.  Geen regels voor uitgaande NSG zijn afhankelijk van het subnet Frontend, is verkeer toegestaan
5.  Firewall begint regel verwerking:
  1.    FW-regel 1 (Mgmt FW) niet van toepassing, naar volgende regel gaan
  2.    FW-regel 2-5 (RDP-regels) niet van toepassing, naar volgende regel gaan
  3.    FW regels 6 & 7 (App regels) niet van toepassing, naar de volgende regel gaan
  4.    FW regel 8 (met Internet) niet van toepassing, naar de volgende regel gaan
  5.    FW regel 9 (DNS) is van toepassing, het verkeer is toegestaan, firewall wordt doorgestuurd verkeer naar 10.0.2.4 (DNS01)
6.  Het subnet van de back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar volgende regel gaan
  2.    NSG standaardregels subnet-subnet verkeer toestaan, verkeer wordt toegestaan, NSG verwerking stoppen
7.  DNS-server ontvangt de aanvraag
8.  Geen DNS-server het adres in de cache opgeslagen en wordt u gevraagd een DNS-basisserver op het internet
9.  UDR routes voor uitgaand verkeer op de firewall als de volgende hop
10. Geen uitgaande regels NSG op subnet Backend verkeer is toegestaan.
11. Firewall begint regel verwerking:
  1.    FW-regel 1 (Mgmt FW) niet van toepassing, naar volgende regel gaan
  2.    FW-regel 2-5 (RDP-regels) niet van toepassing, naar volgende regel gaan
  3.    FW regels 6 & 7 (App regels) niet van toepassing, naar de volgende regel gaan
  4.     FW regel 8 (met Internet) is van toepassing, verkeer wordt toegestaan, sessie wordt SNAT van basis-DNS-server op het Internet
12. Internet-DNS-server reageert, aangezien deze sessie is gestart vanuit de firewall, de reactie wordt geaccepteerd door de firewall
13. Aangezien dit een bestaande sessie, stuurt de firewall het antwoord naar de oorspronkelijke server, DNS01
14. Het subnet van de back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar volgende regel gaan
  2.    NSG standaardregels subnet-subnet verkeer toestaan, verkeer wordt toegestaan, NSG verwerking stoppen
15. De DNS-server ontvangt het antwoord in cache opgeslagen en vervolgens antwoord op de eerste aanvraag terug naar IIS01
16. De route UDR op subnet backend maakt de firewall de volgende hop
17. Geen regels voor uitgaande NSG bestaat in het subnet van de Backend, is verkeer toegestaan
18. Dit is een bestaande sessie op de firewall, het antwoord wordt doorgestuurd door de firewall naar de IIS-server
19. Frontend subnet begint verwerking van binnenkomende regel:
  1.    Er is geen NSG-regel die voor inkomend geldt verkeer vanaf de back-end-subnet aan de Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
20. IIS01 ontvangt de reactie van DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Toegestaan) Front-end server back-end-server
1.  Beheerder aangemeld bij AppVM02 via RDP vraagt een bestand rechtstreeks vanaf de server IIS01 via windows Verkenner
2.  De route UDR op subnet Backend maakt de firewall de volgende hop
3.  En omdat er geen uitgaande NSG-regels in het antwoord is toegestaan back-end subnet
4.  Firewall begint regel verwerking:
  1.    FW-regel 1 (Mgmt FW) niet van toepassing, naar volgende regel gaan
  2.    FW-regel 2-5 (RDP-regels) niet van toepassing, naar volgende regel gaan
  3.    FW regels 6 & 7 (App regels) niet van toepassing, naar de volgende regel gaan
  4.    FW regel 8 (met Internet) niet van toepassing, naar de volgende regel gaan
  5.    FW regel 9 (DNS) niet van toepassing, naar de volgende regel gaan
  6.    FW regel 10 (Intra-Subnet) is van toepassing, het verkeer is toegestaan, firewall verkeer doorgeeft aan 10.0.1.4 (IIS01)
5.  Frontend subnet begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar volgende regel gaan
  2.    NSG standaardregels subnet-subnet verkeer toestaan, verkeer wordt toegestaan, NSG verwerking stoppen
6.  Als de juiste verificatie en autorisatie, IIS01 accepteert de aanvraag en reageert
7.  De route UDR op subnet Frontend maakt de firewall de volgende hop
8.  Aangezien er geen uitgaande regels NSG op het subnet Frontend het antwoord is toegestaan
9.  Als dit een bestaande sessie op de firewall is dit antwoord is toegestaan en de firewall retourneert het antwoord naar AppVM02
10. Subnet back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (blok Internet) niet van toepassing, naar volgende regel gaan
  2.    NSG standaardregels subnet-subnet verkeer toestaan, verkeer wordt toegestaan, NSG verwerking stoppen
11. Het antwoord wordt ontvangen AppVM02

#### <a name="denied-internet-direct-to-web-server"></a>(Geweigerd) Internet rechtstreeks naar een webserver
1.  Internet-gebruiker probeert toegang te krijgen tot de webserver, IIS01, via de service FrontEnd001.CloudApp.Net
2.  Omdat er geen eindpunten voor HTTP-verkeer geopend zijn, deze worden niet via de Cloud-Service en wouldn't de server niet bereiken.
3.  Als de eindpunten geopend om een bepaalde reden zijn, zou de NSG (Internet blokkeren) op subnet Frontend dit verkeer blokkeren
4.  Ten slotte de Frontend subnet UDR route zou al het uitgaande verkeer verzenden van IIS01 naar de firewall als de volgende hop en de firewall zou zien als asymmetrische verkeer en drop het uitgaande antwoord, dus er zijn ten minste drie onafhankelijke lagen van defensie tussen internet en IIS01 via de cloud-service te voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-internet-to-backend-server"></a>(Geweigerd) Internet met back-end-Server
1.  Internet-gebruiker probeert toegang te krijgen tot een bestand op een AppVM01 via de service BackEnd001.CloudApp.Net
2.  Omdat er geen eindpunten voor een gedeeld bestand openen, dit zou de Cloud-Service niet doorgeven en wouldn't de server niet bereiken.
3.  Als de eindpunten geopend om een bepaalde reden zijn, zou de NSG (Internet blokkeren) dit verkeer blokkeren
4.  Tot slot de route UDR zou al het uitgaande verkeer verzenden van AppVM01 naar de firewall als de volgende hop en de firewall zou zien als asymmetrische verkeer en drop het uitgaande antwoord, dus er zijn ten minste drie onafhankelijke lagen van defensie tussen internet en AppVM01 via de cloud-service te voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-frontend-server-to-backend-server"></a>(Geweigerd) Front-end server met back-end-Server
1.  Stel IIS01 in gevaar is gebracht en schadelijke code scant de subnet back-end servers wordt uitgevoerd.
2.  De Frontend subnet UDR route zou verzenden al het uitgaande verkeer van IIS01 naar de firewall als de volgende hop. Dit is niet iets dat kan worden gewijzigd door de gemanipuleerde VM.
3.  De firewall kan verwerken het verkeer, als de AppVM01 of de DNS-server voor DNS-lookups dat verkeer mogelijk kan worden toegestaan door de firewall (als gevolg van FW regels 7 en 9). Al het andere verkeer zouden worden geblokkeerd door FW regel 11 (Alles weigeren).
4.  Als de geavanceerde detectie bedreiging op de firewall is ingeschakeld (dat is niet beschreven in dit document, raadpleegt u de documentatie van de leverancier voor uw specifieke netwerk toestel geavanceerde mogelijkheden voor bedreiging) zelfs verkeer dat zou worden toegestaan door de eenvoudige doorstuurregels in dit document besproken kan worden voorkomen door het verkeer zich bekende handtekeningen of patronen die een bedreiging voor geavanceerde regel markeren.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Geweigerd) Internet-DNS-lookup op de DNS-server
1.  Internet-gebruiker probeert een interne DNS-record op DNS01 via BackEnd001.CloudApp.Net opzoeken 
2.  Omdat er geen eindpunten voor DNS-verkeer, deze worden niet via de Service Cloud en wouldn't de server niet bereiken.
3.  Als de eindpunten geopend om een bepaalde reden zijn, wordt de NSG-regel (Internet blokkeren) op subnet Frontend dit verkeer geblokkeerd
4.  Ten slotte de Backend subnet UDR route zou al het uitgaande verkeer verzenden van DNS01 naar de firewall als de volgende hop en de firewall zou zien als asymmetrische verkeer en drop het uitgaande antwoord, dus er zijn ten minste drie onafhankelijke lagen van defensie tussen internet en DNS01 via de cloud-service te voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Geweigerd) Internet Access SQL via Firewall
1.  Internet-gebruiker opvraagt SQL gegevens uit SecSvc001.CloudApp.Net (Internet Facing Cloud Service)
2.  Omdat er geen eindpunten open voor SQL, dit zou de Cloud-Service niet doorgeven en wouldn't de firewall bereiken
3.  Als de SQL-eindpunten zijn geopend om een bepaalde reden, beginnen de firewall regel verwerking:
  1.    FW-regel 1 (Mgmt FW) niet van toepassing, naar volgende regel gaan
  2.    FW-regels (regels met RDP) 2-5 niet toepassen, naar de volgende regel gaan
  3.    FW regel 6 & 7 (regels van toepassing) niet van toepassing, naar de volgende regel gaan
  4.    FW regel 8 (met Internet) niet van toepassing, naar de volgende regel gaan
  5.    FW regel 9 (DNS) niet van toepassing, naar de volgende regel gaan
  6.    FW regel 10 (Intra-Subnet) niet van toepassing, naar volgende regel gaan
  7.    FW regel 11 (Alles weigeren) is van toepassing, het verkeer wordt geblokkeerd, stop, verwerking


## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en netwerkconfiguratie
Het volledige Script opslaan in een PowerShell script. De netwerk-configuratie opslaan in een bestand met de naam 'NetworkConf2.xml'.
De gebruiker gedefinieerde variabelen naar wens aanpassen. Voer het script uit en volg de bovenstaande van Firewall regel installatie instructie.

#### <a name="full-script"></a>Volledig Script
Dit script wordt op basis van de door gebruiker gedefinieerde variabelen:

1.  Verbinding maken met een Azure-abonnement
2.  Maak een nieuwe account voor opslag
3.  Een nieuwe VNet en drie subnetten als omschreven in het netwerk Config-bestand maken
4.  Bouwen van vijf virtuele machines; firewall 1 en 4 windows server VMs
5.  Configureer UDR met inbegrip van:
  1.    Twee nieuwe routetabellen maken
  2.    Routes aan tabellen toevoegen
  3.    Tabellen koppelen aan passende subnetten
6.  Doorsturen via IP op de NVA inschakelen
7.  Configureer de NSG met inbegrip van:
  1.    Maken van een NSG
  2.    Een regel toevoegen
  3.    De NSG koppelen aan de juiste subnetten

Deze PowerShell script werkt alleen lokaal op dat een internet aangesloten PC of server.

>[AZURE.IMPORTANT] Als dit script wordt uitgevoerd, kunnen er waarschuwingen of andere informatieve berichten die pop in PowerShell. Alleen foutberichten worden weergegeven in het rood zijn aanleiding tot bezorgdheid.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
    

#### <a name="network-config-file"></a>Netwerk Config-bestand
Dit XML-bestand met de bijgewerkte locatie opslaan en de koppeling toevoegen aan dit bestand aan de variabele $NetworkConfigFile in het bovenstaande script.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Voorbeeldscripts voor toepassing
Als u een voorbeeldtoepassing installeren voor deze en andere voorbeelden DMZ wilt, één is opgegeven op de volgende koppeling: [Voorbeeldscript toepassing][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Bi-directionele DMZ met NVA, NSG en UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logische weergave van de Firewall-regels"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Een netwerk FrontEnd-Object maken"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Een DNS-Server-Object maken"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopie van de standaard RDP-regel"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Redirect toepassingspictogram"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Bestemming NAT-pictogram"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Pictogram doorgeven"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Firewallregel Management"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP firewallregel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Web firewallregel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Firewallregel AppVM01"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Uitgaande firewallregel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS-firewallregel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Firewallregel Intra-VNet"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Firewall regel weigeren"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Firewall regel activeren"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
