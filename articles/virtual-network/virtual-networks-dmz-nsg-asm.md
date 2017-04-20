<properties
   pageTitle="DMZ voorbeeld – een eenvoudige DMZ met NSGs bouwen | Microsoft Azure"
   description="Bouwen van een DMZ met netwerk-beveiligingsgroepen (NSG)"
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

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>Voorbeeld 1: een eenvoudige DMZ bouwen met NSGs

[Terug naar de grens van beste praktijken beveiligingspagina][HOME]

In dit voorbeeld wordt een eenvoudige DMZ gemaakt met vier windows-servers en netwerk-beveiligingsgroepen. Het helpt ook via de desbetreffende opdrachten voor een beter begrip van de verschillende stappen. Er is ook een sectie verkeer Scenario met een gedetailleerde stapsgewijze hoe verkeer wordt uitgevoerd door de lagen van defensie in de DMZ. Ten slotte, in de referenties sectie is de volledige code en de instructies in deze omgeving om te testen en experimenteren met verschillende scenario's te bouwen. 

![Inkomende DMZ met NSG][1]

## <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat het volgende bevat:

- Twee cloud services: "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk, 'CorpNetwork' met twee subnetten; 'FrontEnd' en 'End'
- Een netwerk-beveiligingsgroep die wordt toegepast op beide subnetten
- Een Windows-Server met een web application server ("IIS01")
- Twee windows-servers die toepassing terug end servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

In de sectie Verwijzingen hieronder is er een PowerShell script dat de meeste van de hierboven beschreven omgeving wordt opgebouwd. Bouwen van het VMs en virtuele netwerken Hoewel zijn gedaan door het voorbeeldscript zijn niet gedetailleerd beschreven in dit document. 

Bouwen van het milieu;

  1.    Het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de namen, de locatie en de IP-adressen overeenkomt met het gegeven scenario)
  2.    Bijwerken van de gebruikersvariabelen in het script aan het milieu wordt het script uitgevoerd tegen (abonnementen, servicenamen, enz)
  3.    Het script uitvoeren in PowerShell

**Opmerking**: het gebied dat wordt aangegeven in het PowerShell script moet overeenkomen met de regio die is aangegeven in het netwerk van het XML-configuratiebestand.

Zodra het script wordt uitgevoerd met succes extra optionele stappen kunnen worden genomen, in de sectie Verwijzingen twee scripts voor het instellen van de webserver en server met een eenvoudige webtoepassing te kunnen testen met deze configuratie DMZ app.

In de volgende secties wordt een gedetailleerde beschrijving van het netwerk, beveiligingsgroepen en hoe ze werken voor dit voorbeeld door stap voor stap de regels van het PowerShell script bevatten.

## <a name="network-security-groups-nsg"></a>Netwerk-beveiligingsgroepen (NSG)
In dit voorbeeld wordt een groep NSG gebouwd en vervolgens geladen met zes regels. 

>[AZURE.TIP] In het algemeen moet u eerst uw specifieke regels voor 'Toestaan' maken en vervolgens de algemene regels van de "Deny" laatste. De toegewezen prioriteit bepaalt welke regels zijn eerst geëvalueerd. Als verkeer toe te passen op een bepaalde regel wordt gevonden, worden geen verdere regels worden geëvalueerd. NSG regels kunnen toepassen op een of meer in de richting van de inkomende of uitgaande (vanuit het perspectief van het subnet).

Declaratief, worden de volgende regels voor binnenkomend verkeer gebouwd:

1.  Interne DNS-verkeer (poort 53) is toegestaan.
2.  RDP-verkeer (poort 3389) van het Internet voor een VM is toegestaan.
3.  HTTP-verkeer (poort 80) van de Internet web server (IIS01) is toegestaan.
4.  Al het verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5.  Al het verkeer (alle poorten) van het Internet op de gehele VNet (beide subnetten) is geweigerd.
6.  Al het verkeer (alle poorten) van het subnet Frontend naar het subnet Backend is geweigerd.

Met deze regels gebonden aan elk subnet als een HTTP-verzoek inkomende vanaf het Internet naar de webserver, worden beide regels 3 is (toestaan) en 5 (weigeren) zou gelden, maar omdat de regel 3 heeft een hogere prioriteit alleen zou worden toegepast en regel 5 niet in het spel komen zou. Dus de HTTP-aanvraag naar de webserver mogen. Als dat hetzelfde verkeer is probeert te bereiken van de server DNS01, zou regel 5 (weigeren) de eerste om toe te passen en het verkeer niet mogen doorgeven aan de server. Regel 6 (weigeren) blokkeert het subnet Frontend in gesprekken met de back-end-subnet (met uitzondering van toegestane netwerkverkeer in de regels 1 en 4), beveiligt u hiermee de Backend-netwerk in geval een aanvaller compromissen de webtoepassing op de Frontend, de aanvaller zou hebben beperkte toegang tot de back-end 'beveiligde' netwerk (alleen voor bronnen op de server AppVM01 blootgesteld).

Er is een standaard uitgaande regel die verkeer van het internet. In dit voorbeeld zijn we uitgaand verkeer toestaan en uitgaande regels niet wijzigen. Om het verkeer in beide richtingen worden vergrendeld, is gebruiker gedefinieerd routering vereist, dit onder de loep genomen in 'Voorbeeld 3' hieronder.

Elke regel als volgt in detail beschreven (**Opmerking**: een item in de onderstaande lijst in die beginnen met een dollarteken (bijvoorbeeld: $NSGName) een variabele van het script in de sectie Verwijzingen van dit document is door de gebruiker gedefinieerd):

1. Een beveiligingsgroep netwerk moet eerst worden gemaakt om de regels houdt:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  De eerste regel in het volgende voorbeeld kunt DNS-verkeer tussen alle interne netwerken naar de DNS-server op de back-end-subnet. De regel heeft enkele belangrijke parameters:
  - "Type" geeft aan in welke richting verkeer deze regel van kracht; Dit is vanuit het perspectief van de virtuele Machine of het subnet (afhankelijk van waar deze NSG afhankelijk is). Dus als Type 'Inkomend' en verkeer is het subnetmasker invoeren, zou de regel van toepassing en verlaten van het subnet verkeer niet zouden worden beïnvloed door deze regel.
  - 'Priority' Hiermee stelt u de volgorde waarin netwerkverkeer wordt geëvalueerd. Hoe lager het getal hoe hoger de prioriteit. Als een regel van toepassing op specifiek netwerkverkeer is, worden geen verdere regels verwerkt. Dus als een regel met prioriteit 1 gegevensverkeer en weigert u verkeer, een regel met prioriteit 2 en beide regels van toepassing op het verkeer dan het verkeer mogen lopen (sinds 1 regel een hogere prioriteit had dit effect heeft en geen verdere regels zijn toegepast).
  - "Actie" geeft aan dat als gegevensverkeer waarop deze regel wordt geblokkeerd of toegestaan.

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  Deze regel kunt RDP-verkeer toe via het internet naar de RDP-poort op elke server in een subnet van de VNET. Deze regel maakt gebruik van twee speciale typen adresprefixen; 'VIRTUAL_NETWORK' en 'INTERNET'. Dit is een eenvoudige manier om een grotere adresprefixen categorie.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  Deze regel kunt binnenkomend gegevensverkeer te druk op de webserver. Dit verandert niet de werking van Routering; kan alleen verkeer dat bestemd is voor IIS01 door te geven. Dus als verkeer vanaf het Internet de webserver had als doel deze regel zou staan en het verwerken van meer regels stoppen. (In de regel prioriteit 140 alle andere binnenkomende internet-verkeer wordt geblokkeerd). Als u alleen HTTP-verkeer verwerkt bent, kan deze regel verder worden beperkt als u wilt dat alleen bestemming poort 80.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  Deze regel kunt verkeer van de IIS01-server naar de server AppVM01, een hogere regel blokkeert alle Frontend voor Backend-verkeer. Voor het verbeteren van deze regel, de poort is bekend dat moet worden toegevoegd. Bijvoorbeeld, als de IIS-server alleen SQL Server op AppVM01 raken wordt, het doelbereik voor de poort moet worden gewijzigd van ' * ' (willekeurig) naar 1433 (SQL-poort), waardoor een kleinere inkomende aanvallen op AppVM01 of moet de webtoepassing ooit worden aangetast.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Deze regel al dan niet toestaan van verkeer vanaf het internet naar alle servers in het netwerk. In combinatie met de regel prioriteit 110 en 120, kunnen alleen binnenkomende internet-verkeer op de firewall en RDP-poorten voor andere servers en blokkeert al het andere. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  De laatste regel al dan niet toestaan van verkeer van het subnet Frontend naar het subnet van de back-end. Aangezien dit een binnenkomende regel alleen, wordt reverse verkeer wordt toegestaan (vanaf de back-end om de Frontend).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Verkeer-scenario 's
#### <a name="allowed-web-to-web-server"></a>(*Toegestaan*) Web-Web-Server
1.  HTTP-pagina van Internet gebruiker aanvragen van FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2.  Cloud service gangen verkeer via open eindpunt op poort 80 naar IIS01 (webserver)
3.  Frontend subnet begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) niet van toepassing, naar de volgende regel gaan
  3.    NSG regel 3 (Internet op IIS01) is van toepassing, het verkeer is toegestaan, zelfs niet meer verwerking
4.  Verkeer treffers interne IP-adres van de webserver IIS01 (10.0.1.5)
5.  IIS01 voor Internet-verkeer luistert, dit verzoek ontvangt en start de verwerking van de aanvraag
6.  IIS01 wordt u gevraagd de SQL-Server op AppVM01 voor meer informatie
7.  Geen uitgaande regels op de Frontend subnet verkeer is toegestaan.
8.  Het subnet van de back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) niet van toepassing, naar de volgende regel gaan
  3.    NSG regel 3 (Internet met Firewall) niet van toepassing, naar volgende regel gaan
  4.    NSG regel 4 (IIS01 tot AppVM01) is van toepassing, het verkeer is toegestaan, verwerking stoppen
9.  AppVM01 de SQL-Query ontvangen en beantwoord
10. En omdat er geen regels voor uitgaande verbindingen op het subnet van de back-end is antwoord toegestaan
11. Frontend subnet begint verwerking van binnenkomende regel:
  1.    Er is geen NSG-regel die voor inkomend geldt verkeer vanaf de back-end-subnet aan de Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
12. De IIS-server ontvangt de reactie van SQL en het HTTP-antwoord is voltooid en naar de aanvrager verzendt
13. Aangezien er geen regels voor uitgaande verbindingen op het subnet Frontend het antwoord is toegestaan en de Internet-gebruiker ontvangt de gevraagde pagina.

#### <a name="allowed-rdp-to-backend"></a>(*Toegestaan*) RDP naar back-end
1.  Server Admin op internet vraagt om RDP-sessie met AppVM01 op BackEnd001.CloudApp.Net:xxxxx, waarbij xxxxx het is willekeurig toegewezen poort voor RDP naar AppVM01 (de toegewezen poort kan worden gevonden op de Azure-Portal of via PowerShell)
2.  Subnet back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) is van toepassing, het verkeer is toegestaan, zelfs niet meer verwerking
3.  Standaardregels zijn van toepassing met geen uitgaande regels en terugkerend verkeer is toegestaan.
4.  RDP-sessie is ingeschakeld.
5.  AppVM01 vraagt om gebruikersnaam wachtwoord

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Toegestaan*) Web Server DNS-lookup op de DNS-server
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

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Toegestaan*) Web Server access-bestand op AppVM01
1.  IIS01 vraagt om een bestand op AppVM01
2.  Geen uitgaande regels op de Frontend subnet verkeer is toegestaan.
3.  Het subnet van de back-end begint verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) niet van toepassing, naar de volgende regel gaan
  3.    NSG regel 3 (Internet IIS01) niet van toepassing, verplaatsen naar de volgende regel
  4.    NSG regel 4 (IIS01 tot AppVM01) is van toepassing, het verkeer is toegestaan, verwerking stoppen
4.  AppVM01 ontvangt de aanvraag en reageert met het bestand (ervan uitgaande dat toegang is toegestaan)
5.  En omdat er geen regels voor uitgaande verbindingen op het subnet van de back-end is antwoord toegestaan
6.  Frontend subnet begint verwerking van binnenkomende regel:
  1.    Er is geen NSG-regel die voor inkomend geldt verkeer vanaf de back-end-subnet aan de Frontend subnet, zodat geen van de NSG regels toepassen
  2.    De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
7.  De IIS-server ontvangt het bestand

#### <a name="denied-web-to-backend-server"></a>(*Geweigerd*) Webpagina met back-end-Server
1.  Internet-gebruiker probeert toegang te krijgen tot een bestand op een AppVM01 via de service BackEnd001.CloudApp.Net
2.  Omdat er geen eindpunten voor een gedeeld bestand openen, dit zou de Cloud-Service niet doorgeven en wouldn't de server niet bereiken.
3.  Als de eindpunten geopend om een bepaalde reden zijn, wordt NSG regel 5 (Internet VNet) dit verkeer geblokkeerd

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Geweigerd*) Web DNS-lookup op de DNS-server
1.  Internet-gebruiker probeert een interne DNS-record op DNS01 via de service BackEnd001.CloudApp.Net opzoeken
2.  Omdat er geen eindpunten open voor DNS, dit zou de Cloud-Service niet doorgeven en wouldn't de server niet bereiken.
3.  Als de eindpunten geopend om een bepaalde reden zijn, NSG regel 5 (Internet VNet) dit verkeer wordt geblokkeerd (Opmerking: deze regel 1 (DNS) om twee redenen niet van toepassing, eerst het bronadres is het internet, deze regel is alleen van toepassing op de lokale VNet als de bron, dit is tevens een regel voor toestaan, zodat het verkeer nooit zou weigeren)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Geweigerd*) Web Access SQL via Firewall
1.  Internet-gebruiker opvraagt SQL gegevens uit FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2.  Omdat er geen eindpunten open voor SQL, dit zou de Cloud-Service niet doorgeven en wouldn't de firewall bereiken
3.  Als de eindpunten zijn geopend om een bepaalde reden, begint het subnet Frontend verwerking van binnenkomende regel:
  1.    NSG regel 1 (DNS) niet van toepassing, naar de volgende regel gaan
  2.    NSG regel 2 (RDP) niet van toepassing, naar de volgende regel gaan
  3.    NSG regel 3 (Internet op IIS01) is van toepassing, het verkeer is toegestaan, zelfs niet meer verwerking
4.  Verkeer treffers interne IP-adres van de IIS01 (10.0.1.5)
5.  IIS01 is niet luisteren op poort 1433, dus geen antwoord op de aanvraag

## <a name="conclusion"></a>Conclusie
Dit is een relatief eenvoudige en duidelijke manier van het subnet van de back-end van inkomend verkeer te isoleren.

Meer voorbeelden en een overzicht van de beveiligingsgrenzen netwerk vindt u [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en netwerkconfiguratie
Het volledige Script opslaan in een PowerShell script. De netwerk-configuratie opslaan in een bestand met de naam 'NetworkConf1.xml'.
De gebruiker gedefinieerde variabelen naar wens aanpassen. Voer het script uit en volg de Firewall regel installatie instructie opgenomen in de sectie Voorbeeld 1 hierboven.

#### <a name="full-script"></a>Volledig Script
Dit script wordt op basis van de door gebruiker gedefinieerde variabelen;

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
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
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
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
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
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
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
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
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
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Inkomende DMZ met NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

