<properties
   pageTitle="DMZ voorbeeld – een DMZ-toepassingen met een Firewall en NSGs beschermen bouwen | Microsoft Azure"
   description="Bouwen van een DMZ met een Firewall en netwerk-beveiligingsgroepen (NSG)"
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

# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Voorbeeld 2: een DMZ ter bescherming van toepassingen met een Firewall en NSGs bouwen

[Terug naar de grens van beste praktijken beveiligingspagina][HOME]

In dit voorbeeld wordt een DMZ maken met een firewall, vier windows-servers en netwerk-beveiligingsgroepen. Het helpt ook via de desbetreffende opdrachten voor een beter begrip van de verschillende stappen. Er is ook een sectie verkeer Scenario met een gedetailleerde stapsgewijze hoe verkeer wordt uitgevoerd door de lagen van defensie in de DMZ. Ten slotte, in de referenties sectie is de volledige code en de instructies in deze omgeving om te testen en experimenteren met verschillende scenario's te bouwen. 

![Inkomende DMZ met NVA en NSG][1]

## <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat het volgende bevat:

- Twee cloud services: "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk 'CorpNetwork' met twee subnetten: 'FrontEnd' en 'End'
- Een enkel netwerk beveiligingsgroep die wordt toegepast op beide subnetten
- Een virtueel netwerkapparaat, in dit voorbeeld een Barracuda NextGen Firewall, aangesloten op het subnet Frontend
- Een Windows-Server met een web application server ("IIS01")
- Twee windows-servers die toepassing terug end servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

>[AZURE.NOTE] In dit voorbeeld wordt een Barracuda NextGen Firewall gebruikt, kunnen veel verschillende virtuele netwerk apparaten worden gebruikt voor dit voorbeeld.

In de sectie Verwijzingen hieronder is er een PowerShell script dat de meeste van de hierboven beschreven omgeving wordt opgebouwd. Bouwen van het VMs en virtuele netwerken Hoewel zijn gedaan door het voorbeeldscript zijn niet gedetailleerd beschreven in dit document.
 
Maken van de omgeving:

  1.    Het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de namen, de locatie en de IP-adressen overeenkomt met het gegeven scenario)
  2.    Bijwerken van de gebruikersvariabelen in het script aan het milieu wordt het script uitgevoerd tegen (abonnementen, servicenamen, enz)
  3.    Het script uitvoeren in PowerShell

**Opmerking**: het gebied dat wordt aangegeven in het PowerShell script moet overeenkomen met de regio die is aangegeven in het netwerk van het XML-configuratiebestand.

Zodra het script met succes wordt uitgevoerd kunnen de volgende na scriptstappen worden genomen:

1.  De firewall-regels instellen, hierover vindt u in de sectie met de titel: Firewall-regels.
2.  Desgewenst zijn in de sectie Verwijzingen twee scripts voor het instellen van de webserver en server met een eenvoudige webtoepassing te kunnen testen met deze configuratie DMZ app.

De volgende sectie wordt uitgelegd dat de meeste van de scripts overzichten ten opzichte van de netwerk-beveiligingsgroepen.

## <a name="network-security-groups-nsg"></a>Netwerk-beveiligingsgroepen (NSG)
In dit voorbeeld wordt een groep NSG gebouwd en vervolgens geladen met zes regels. 

>[AZURE.TIP] In het algemeen moet u eerst uw specifieke regels voor 'Toestaan' maken en vervolgens de algemene regels van de "Deny" laatste. De toegewezen prioriteit bepaalt welke regels zijn eerst geëvalueerd. Als verkeer toe te passen op een bepaalde regel wordt gevonden, worden geen verdere regels worden geëvalueerd. NSG regels kunnen toepassen op een of meer in de richting van de inkomende of uitgaande (vanuit het perspectief van het subnet).

Declaratief, worden de volgende regels voor binnenkomend verkeer gebouwd:

1.  Interne DNS-verkeer (poort 53) is toegestaan.
2.  RDP-verkeer (poort 3389) van het Internet voor een VM is toegestaan.
3.  HTTP-verkeer (poort 80) van het Internet naar de NVA (firewall) is toegestaan.
4.  Al het verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5.  Al het verkeer (alle poorten) van het Internet op de gehele VNet (beide subnetten) is geweigerd.
6.  Al het verkeer (alle poorten) van het subnet Frontend naar het subnet Backend is geweigerd.

Met deze regels gebonden aan elk subnet als een HTTP-verzoek inkomende vanaf het Internet naar de webserver, worden beide regels 3 is (toestaan) en 5 (weigeren) zou gelden, maar omdat de regel 3 heeft een hogere prioriteit alleen zou worden toegepast en regel 5 niet in het spel komen zou. Het HTTP-verzoek zou dus de firewall worden toegestaan. Als dat hetzelfde verkeer is probeert te bereiken van de server DNS01, zou regel 5 (weigeren) de eerste om toe te passen en het verkeer niet mogen doorgeven aan de server. Regel 6 (weigeren) blokkeert het subnet Frontend in gesprekken met de back-end-subnet (met uitzondering van toegestane netwerkverkeer in de regels 1 en 4), beveiligt u hiermee de Backend-netwerk in geval een aanvaller compromissen de webtoepassing op de Frontend, de aanvaller zou hebben beperkte toegang tot de back-end 'beveiligde' netwerk (alleen voor bronnen op de server AppVM01 blootgesteld).

Er is een standaard uitgaande regel die verkeer van het internet. In dit voorbeeld zijn we uitgaand verkeer toestaan en uitgaande regels niet wijzigen. Vergrendelen van verkeer in beide richtingen, gebruiker gedefinieerd routering is vereist, dit onder de loep genomen in een ander voorbeeld dat u kunt vinden in het [hoofdpakket met beveiligingsupdates grens document][HOME].

De hierboven besproken NSG regels zijn vergelijkbaar met de regels van de NSG in [Voorbeeld 1 - bouwen van een eenvoudige DMZ met NSGs][Example1]. Lees de beschrijving van de NSG in dat document voor een uitgebreide beschrijving van elke regel NSG en de kenmerken.

## <a name="firewall-rules"></a>Firewall-regels
Een management-client moet zijn geïnstalleerd op een PC voor het beheren van de firewall en de configuraties nodig maken. Zie de documentatie van uw firewall (of andere NVA)-leverancier voor het beheren van het apparaat. De rest van deze sectie beschrijft de configuratie van de firewall zelf, door middel van de leveranciers management-client (dat wil zeggen niet de Azure portal of PowerShell).

Instructies voor het downloaden van de client en verbinding te maken met de Barracuda gebruikt in dit voorbeeld vindt u hier: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Op de firewall moet doorsturen van regels worden gemaakt. Omdat in dit voorbeeld wordt alleen routes internetverkeer in gebonden aan de firewall en vervolgens naar de webserver, wordt slechts één doorsturen NAT regel nodig. De Barracuda NextGen firewall gebruikt in dit voorbeeld zou de regel een bestemming NAT regel ('Dst NAT") door te geven van het verkeer zijn.

Vanaf de client Barracuda NG Admin dashboard, Ga naar het tabblad configuratie, in de operationele configuratie sectie Ruleset klikt u op de volgende regel maken (of bestaande standaardregels controleren). Een raster wordt genoemd "Main regels" geeft de bestaande actieve als gedeactiveerde regels op de firewall. In de rechterbovenhoek van deze raster is een kleine, groene "+" knop, klik hierop om een nieuwe regel te maken (Opmerking: de firewall kan worden "vergrendeld" voor wijzigingen, als er een knop gemarkeerd "Vergrendelen" en niet maken of bewerken van regels, klikt u op deze knop om te 'ontgrendelen' de ruleset en bewerken toestaan). Als u wilt voor het bewerken van een bestaande regel, selecteert u die regel met de rechtermuisknop op en selecteer de regel bewerken.

Maak een nieuwe regel en geef een naam, bijvoorbeeld 'WebTraffic'. 

Het pictogram bestemming NAT regel er als volgt uit: ![Bestemming NAT-pictogram][2]

De regel zelf zou als volgt uitzien:

![Firewallregel][3]

Hier een adres die de Firewall inkomende probeert te bereiken, HTTP (poort 80 of 443 voor HTTPS) wordt verzonden "Lokaal IP-DHCP1"-interface van de Firewall en omgeleid naar de Server met het IP-adres van de 10.0.1.5. Aangezien het verkeer op poort 80 komende en gaan naar de webserver op poort 80 is geen wijziging van de poort die nodig is. Echter, de doellijst had kunnen 10.0.1.5:8080 als onze webserver wordt geluisterd op poort 8080 dus vertalen van de binnenkomende poort 80 op de firewall op inkomende poort 8080 op de webserver.

Een verbindingsmethode moet ook worden aangegeven, voor de regel van de bestemming van het Internet, "Dynamische SNAT" is het meest geschikt. 

Hoewel slechts één regel is gemaakt is het belangrijk dat de prioriteit correct is ingesteld. Als u in het raster van alle regels op de firewall dat deze nieuwe regel aan de onderkant (onder de regel 'BLOCKALL') is het nooit komen in het spel. Controleer de nieuw gemaakte regel voor webverkeer boven de regel BLOCKALL.

Nadat de regel is gemaakt, moet de firewall wordt geduwd en wordt geactiveerd, als dit niet gebeurt de regel wijziging niet doorgevoerd. Het proces van push en activering wordt beschreven in de volgende sectie.

## <a name="rule-activation"></a>Regel activeren
Met de ruleset gewijzigd als deze regel wilt toevoegen, moet de ruleset worden geüpload naar de firewall en geactiveerd.

![Firewall regel activeren][4]

In de rechterbovenhoek van de client management zijn een cluster van knoppen. Klik op de knop 'Wijzigingen verzenden' om de gewijzigde regels om de firewall te verzenden en klik vervolgens op de knop 'Activeren'.

In dit voorbeeld omgeving build is met de activering van de ruleset firewall voltooid. Eventueel de post build scripts in de sectie Verwijzingen kunnen worden uitgevoerd om een toepassing toevoegen aan deze omgeving voor het testen van de onderstaande scenario's verkeer.

>[AZURE.IMPORTANT] Het is essentieel om te realiseren dat u niet de server rechtstreeks bereikt wordt. Wanneer een browser een HTTP-pagina van FrontEnd001.CloudApp.Net aanvraagt, verkeer het eindpunt HTTP (poort 80) het op de firewall niet op de webserver. De firewall wordt, volgens de regel gemaakt boven NAT's die naar de webserver aanvragen.

## <a name="traffic-scenarios"></a>Verkeer-scenario 's

#### <a name="allowed-web-to-web-server-through-firewall"></a>(Toegestaan) Web-Web-Server via een Firewall
1.  HTTP-pagina van Internet gebruiker aanvragen van FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2.  Cloud service gangen verkeer via open eindpunt voor poort 80 naar lokale firewall-interface op 10.0.1.4:80
3.  Frontend subnet begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) niet van toepassing, naar de volgende regel gaan
  3.    NSG regel 3 (Internet met Firewall) is van toepassing, het verkeer is toegestaan, zelfs niet meer verwerking
4.  Verkeer treffers interne IP-adres van de firewall (10.0.1.4)
5.  Doorsturen firewallregel Zie dit verkeer voor poort 80 is, wordt deze omgeleid naar de webserver IIS01
6.  IIS01 voor Internet-verkeer luistert, dit verzoek ontvangt en start de verwerking van de aanvraag
7.  IIS01 wordt u gevraagd de SQL-Server op AppVM01 voor meer informatie
8.  Geen uitgaande regels op de Frontend subnet verkeer is toegestaan.
9.  Het subnet van de back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) niet van toepassing, naar de volgende regel gaan
  3.    NSG regel 3 (Internet met Firewall) niet van toepassing, naar volgende regel gaan
  4.    NSG regel 4 (IIS01 tot AppVM01) is van toepassing, het verkeer is toegestaan, verwerking stoppen
10. AppVM01 de SQL-Query ontvangen en beantwoord
11. En omdat er geen regels voor uitgaande verbindingen op het subnet van de back-end is antwoord toegestaan
12. Frontend subnet begint verwerking van binnenkomende regel:
  1.    Er is geen NSG-regel die voor inkomend geldt verkeer vanaf de back-end-subnet aan de Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
13. De IIS-server ontvangt de reactie van SQL en het HTTP-antwoord is voltooid en naar de aanvrager verzendt
14. Aangezien dit een NAT-sessie van de firewall, is het doel antwoord (aanvankelijk) voor de Firewall
15. De firewall de reactie van de webserver ontvangt en doorstuurt naar de Internet-gebruiker
16. Aangezien er geen regels voor uitgaande verbindingen op het subnet Frontend het antwoord is toegestaan en de Internet-gebruiker ontvangt de gevraagde pagina.

#### <a name="allowed-rdp-to-backend"></a>(Toegestaan) RDP naar back-end
1.  Server Admin op internet vraagt om RDP-sessie met AppVM01 op BackEnd001.CloudApp.Net:xxxxx, waarbij xxxxx het is willekeurig toegewezen poort voor RDP naar AppVM01 (de toegewezen poort kan worden gevonden op de Azure-Portal of via PowerShell)
2.  Omdat de Firewall alleen op het adres FrontEnd001.CloudApp.Net luistert, is het niet betrokken bij deze verkeersstroom
3.  Subnet back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) is van toepassing, het verkeer is toegestaan, zelfs niet meer verwerking
4.  Standaardregels zijn van toepassing met geen uitgaande regels en terugkerend verkeer is toegestaan.
5.  RDP-sessie is ingeschakeld.
6.  AppVM01 vraagt om gebruikersnaam wachtwoord

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web Server DNS-lookup op de DNS-server
1.  Web Server, IIS01, moet een feed op www.data.gov, maar moet u het adres.
2.  De netwerkconfiguratie voor de lijsten van VNet DNS01 (10.0.2.4 op de Backend-subnet) als de primaire DNS-server, IIS01 stuurt de DNS-aanvraag naar DNS01
3.  Geen uitgaande regels op de Frontend subnet verkeer is toegestaan.
4.  Subnet back-end begint verwerking van binnenkomende regel:
  1.     NSG regel 1 (DNS) is van toepassing, het verkeer is toegestaan, zelfs niet meer verwerking
5.  DNS-server ontvangt de aanvraag
6.  Geen DNS-server het adres in de cache opgeslagen en wordt u gevraagd een DNS-basisserver op het internet
7.  Geen uitgaande regels op de Backend subnet verkeer is toegestaan.
8.  Internet-DNS-server reageert, aangezien deze sessie is gestart, intern, is het antwoord toegestaan
9.  DNS-server slaat het antwoord en reageert op de eerste aanvraag terug naar IIS01
10. Geen uitgaande regels op de Backend subnet verkeer is toegestaan.
11. Frontend subnet begint verwerking van binnenkomende regel:
  1.    Er is geen NSG-regel die voor inkomend geldt verkeer vanaf de back-end-subnet aan de Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
12. IIS01 ontvangt de reactie van DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Toegestaan) Web Server access-bestand op AppVM01
1.  IIS01 vraagt om een bestand op AppVM01
2.  Geen uitgaande regels op de Frontend subnet verkeer is toegestaan.
3.  Het subnet van de back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) niet van toepassing, naar de volgende regel gaan
  3.    NSG regel 3 (Internet met Firewall) niet van toepassing, naar volgende regel gaan
  4.    NSG regel 4 (IIS01 tot AppVM01) is van toepassing, het verkeer is toegestaan, verwerking stoppen
4.  AppVM01 ontvangt de aanvraag en reageert met het bestand (ervan uitgaande dat toegang is toegestaan)
5.  En omdat er geen regels voor uitgaande verbindingen op het subnet van de back-end is antwoord toegestaan
6.  Frontend subnet begint verwerking van binnenkomende regel:
  1.    Er is geen NSG-regel die voor inkomend geldt verkeer vanaf de back-end-subnet aan de Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
7.  De IIS-server ontvangt het bestand

#### <a name="denied-web-direct-to-web-server"></a>(Geweigerd) Webpagina rechtstreeks naar een webserver
Omdat de webserver, IIS01 en de Firewall zich in dezelfde Cloud Service delen zij hetzelfde openbare gerichte IP-adres. Dus zou alle HTTP-verkeer doorgestuurd naar de firewall. Terwijl de aanvraag zou worden aangeboden, deze kan niet gaan rechtstreeks naar de webserver, het doorgegeven, zoals ontworpen, via de Firewall eerst. Zie het eerste Scenario in deze sectie voor de verkeersstroom.

#### <a name="denied-web-to-backend-server"></a>(Geweigerd) Webpagina met back-end-Server
1.  Internet-gebruiker probeert toegang te krijgen tot een bestand op een AppVM01 via de service BackEnd001.CloudApp.Net
2.  Omdat er geen eindpunten voor een gedeeld bestand openen, dit zou de Cloud-Service niet doorgeven en wouldn't de server niet bereiken.
3.  Als de eindpunten geopend om een bepaalde reden zijn, wordt NSG regel 5 (Internet VNet) dit verkeer geblokkeerd

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Geweigerd) Web DNS-lookup op de DNS-server
1.  Internet-gebruiker probeert een interne DNS-record op DNS01 via de service BackEnd001.CloudApp.Net opzoeken
2.  Omdat er geen eindpunten open voor DNS, dit zou de Cloud-Service niet doorgeven en wouldn't de server niet bereiken.
3.  Als de eindpunten geopend om een bepaalde reden zijn, NSG regel 5 (Internet VNet) dit verkeer wordt geblokkeerd (Opmerking: deze regel 1 (DNS) om twee redenen niet van toepassing, eerst het bronadres is het internet, deze regel is alleen van toepassing op de lokale VNet als de bron, dit is tevens een regel voor toestaan, zodat het verkeer nooit zou weigeren)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Geweigerd) Web Access SQL via Firewall
1.  Internet-gebruiker opvraagt SQL gegevens uit FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2.  Omdat er geen eindpunten open voor SQL, dit zou de Cloud-Service niet doorgeven en wouldn't de firewall bereiken
3.  Als de eindpunten zijn geopend om een bepaalde reden, begint het subnet Frontend verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) niet van toepassing, naar de volgende regel gaan
  3.    NSG regel 2 (Internet met Firewall) is van toepassing, het verkeer is toegestaan, zelfs niet meer verwerking
4.  Verkeer treffers interne IP-adres van de firewall (10.0.1.4)
5.  Firewall heeft geen doorstuurregels voor SQL- en daalt het verkeer

## <a name="conclusion"></a>Conclusie
Dit is een vrij duidelijke manier van beveiligen van uw toepassing met een firewall en het subnet van de back-end van inkomend verkeer te isoleren.

Meer voorbeelden en een overzicht van de beveiligingsgrenzen netwerk vindt u [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en netwerkconfiguratie
Het volledige Script opslaan in een PowerShell script. De netwerk-configuratie opslaan in een bestand met de naam 'NetworkConf2.xml'.
De gebruiker gedefinieerde variabelen naar wens aanpassen. Voer het script uit en volg de bovenstaande van Firewall regel installatie instructie.

#### <a name="full-script"></a>Volledig Script
Dit script wordt op basis van de door gebruiker gedefinieerde variabelen:

1.  Verbinding maken met een Azure-abonnement
2.  Maak een nieuwe account voor opslag
3.  Een nieuwe VNet en twee subnetten, zoals gedefinieerd in het netwerk Config-bestand maken
4.  4 windows VMs server bouwen
5.  Configureer de NSG met inbegrip van:
  - Maken van een NSG
  - Het vullen met regels
  - De NSG koppelen aan de juiste subnetten

Deze PowerShell script werkt alleen lokaal op dat een internet aangesloten PC of server.

>[AZURE.IMPORTANT] Als dit script wordt uitgevoerd, kunnen er waarschuwingen of andere informatieve berichten die pop in PowerShell. Alleen foutberichten worden weergegeven in het rood zijn aanleiding tot bezorgdheid.


    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5
     
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
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
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
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
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
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
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
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inkomende DMZ met NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Bestemming NAT-pictogram"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Firewallregel"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Firewall regel activeren"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
