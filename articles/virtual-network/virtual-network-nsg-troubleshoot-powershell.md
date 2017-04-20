<properties 
   pageTitle="Problemen met netwerk-beveiligingsgroepen - PowerShell | Microsoft Azure"
   description="Informatie over het oplossen van problemen met netwerk-beveiligingsgroepen in de bronnenbeheerder Azure implementatiemodel met Azure PowerShell."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Problemen met netwerk-beveiligingsgroepen met Azure PowerShell

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Als u netwerk-beveiligingsgroepen (NSGs) op de virtuele machine (VM) geconfigureerd en VM problemen ondervindt, overzicht in dit artikel een van diagnostische mogelijkheden voor NSGs om u te helpen oplossen verder.

NSGs kunt u bepalen welke typen verkeer stromen in en uit uw virtuele machines (VMs). NSGs kan worden toegepast op subnetten in een virtueel netwerk van Azure (VNet) of netwerkinterfaces (NIC). De effectieve regels toegepast op een Netwerkkaart zijn een samenvoeging van de regels die bestaan in de NSGs toegepast op een Netwerkkaart en het is verbonden met subnet. Regels over deze NSGs kunnen soms met elkaar in conflict en invloed van een VM verbinding met het netwerk.  

Ziet u alle regels voor de efficiënte beveiliging van uw NSGs, zoals deze wordt toegepast op de NIC's van uw VM. In dit artikel ziet u hoe u problemen met VM verbindingen met deze regels in het implementatiemodel Azure Resource Manager. Als u niet vertrouwd met VNet en de NSG concepten bent, lees het [virtuele netwerk](virtual-networks-overview.md) en [netwerk-beveiligingsgroepen](virtual-networks-nsg.md) overzicht artikelen.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Problemen met netwerkverkeer VM met effectieve regels

Het volgende scenario is een voorbeeld van een gemeenschappelijk probleem:

Een VM met de naam *VM1* maakt deel uit van een subnet genaamd *Subnet1* in een VNet met de naam *WestUS VNet1*. Een verbinding maken met de VM met RDP via TCP-poort 3389 niet lukt. NSGs worden toegepast op de NIC *VM1 NIC1* en de *Subnet1*van het subnet. Verkeer op TCP-poort 3389 is toegestaan in de NSG die is gekoppeld aan de netwerkinterface *VM1 NIC1*, maar wel TCP ping naar de VM1 poort 3389 mislukt.

In dit voorbeeld wordt de TCP-poort 3389, kunnen de volgende stappen worden gebruikt bepalen mislukte binnenkomende en uitgaande verbindingen via elke poort.

## <a name="detailed-troubleshooting-steps"></a>Gedetailleerde stappen voor probleemoplossing
Voer de volgende stappen uit om problemen met NSGs voor een VM:

1. Start een sessie Azure PowerShell en Azure-aanmelding. Als u niet vertrouwd bent met Azure PowerShell, lees het artikel voor [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) gebruiken.

2. Voer de volgende opdracht om te retourneren van alle NSG-regels toegepast op een *VM1-NIC1* met de naam in de resourcegroep *RG1*NIC:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Als u de naam van een NIC niet weet, voert u de volgende opdracht om de namen van alle netwerkadapters in een groep: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    De volgende tekst is een voorbeeld van de effectieve regels uitvoer voor de *VM1 NIC1* NIC geretourneerd:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Houd rekening met de volgende gegevens in de uitvoer:

    - Er zijn twee secties van het **NetworkSecurityGroup** : een is gekoppeld aan een subnet (*Subnet1*) en is gekoppeld aan een NIC (*VM1 NIC1*). In dit voorbeeld wordt is een NSG toegepast op elk.
    - **Koppeling** wordt de resource (subnet of NIC) een bepaald NSG is gekoppeld. Als de resource NSG verplaatst/ontkoppeld onmiddellijk voordat u deze opdracht, moet u mogelijk Wacht een paar seconden om de wijziging door te geven in de uitvoer van de opdracht. 
    - De namen die worden voorafgegaan door *defaultSecurityRules*: wanneer een NSG is gemaakt, verschillende regels voor standaard worden gemaakt in het. Standaardregels kunnen niet worden verwijderd, maar ze kunnen worden opgeheven met hogere prioriteitregels.
     Lees het artikel [NSG-overzicht](virtual-networks-nsg.md#default-rules) voor meer informatie over regels voor standaard NSG.
    - **ExpandedAddressPrefix** de adresprefixen NSG standaardlabels voor uitgevouwen. Tags vertegenwoordigen meerdere adresprefixen. Uitbreiding van de codes is handig bij het oplossen van verbindingsproblemen van bepaalde adresprefixen VM. Bijvoorbeeld met VNET peering, VIRTUAL_NETWORK code uitgebreid peered VNet voorvoegsels in de vorige uitvoer weergeven.

        >[AZURE.NOTE] De opdracht alleen geeft effectieve regels als een NSG gekoppeld aan een subnet, een Netwerkkaart of beide is. Een VM kan hebben meerdere NIC's met verschillende NSGs toegepast. Als u wilt oplossen, voert u de opdracht voor elke NIC
        
3. Om over een groter aantal regels NSG filteren, typ de volgende opdrachten verder oplossen: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Een filter voor RDP-verkeer (TCP-poort 3389), wordt toegepast op de rasterweergave, zoals in de volgende afbeelding:

    ![Lijst met regels](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Zoals u in de rasterweergave zien kunt, zijn er zowel toestaan en weigeren van regels voor RDP. De uitvoer uit stap 2 wordt aangegeven dat de regel *DenyRDP* in de NSG toegepast op het subnet. Voor binnenkomende regels worden toegepast op het subnet NSGs eerst verwerkt. Als een overeenkomst wordt gevonden, wordt de NSG toegepast op de netwerkinterface wordt niet verwerkt. In dit geval blokkeert de *DenyRDP* -regel van het subnet RDP voor VM (**VM1**).

    >[AZURE.NOTE] Een VM kan hebben meerdere NIC's is gekoppeld. Elk kan worden aangesloten op een ander subnet. Omdat de opdrachten in de vorige stappen voor een NIC worden uitgevoerd, is het belangrijk om ervoor te zorgen dat u opgeeft dat de NIC u de verbinding niet ondervindt. Als u niet zeker weet, kunt u altijd de opdrachten uitvoeren voor elke Netwerkkaart die is gekoppeld aan de VM.

5. Met RDP in VM1, wijzigen in de regel voor *Weigeren RDP (3389)* *RDP(3389) toestaan* in de **Subnet1 NSG** NSG. Controleer of TCP-poort 3389 open door openen van een RDP-verbinding naar de VM of met behulp van het hulpprogramma PsPing. Meer informatie over PsPing door te lezen op de [pagina PsPing downloaden](https://technet.microsoft.com/sysinternals/psping.aspx)

    U kunt regels of verwijderen uit een NSG met behulp van de gegevens in de uitvoer van de volgende opdracht:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Overwegingen met betrekking tot

Houd rekening met de volgende punten bij het oplossen van problemen met netwerkverbindingen:

- Standaard NSG regels worden binnenkomende toegang vanaf het internet blokkeren en staan alleen de VNet binnenkomend verkeer. Regels moeten expliciet worden toegevoegd aan het toestaan van binnenkomende toegang van Internet, zoals vereist.
- Als er geen regels voor NSG waardoor de verbinding met het netwerk van een VM mislukt, wordt het probleem mogelijk vanwege:
    - Firewall-software in het besturingssysteem van de VM draaien
    - Routes die zijn geconfigureerd voor virtuele apparaten of verkeer op gebouwen. Internet-verkeer kan worden omgeleid naar op bedrijven via gedwongen tunneling. Een RDP/SSH-verbinding via het Internet voor uw VM werkt niet met deze instelling is afhankelijk van hoe de netwerkhardware op gebouwen dit verkeer verwerkt. Lees het artikel [Routes voor het oplossen van problemen](virtual-network-routes-troubleshoot-powershell.md) om te leren hoe route diagnosticeren die kunnen worden belemmeren de doorstroming van het verkeer in en uit de VM. 
- Als u hebt VNets, standaard peered, wordt automatisch de code van de VIRTUAL_NETWORK uitgebreid met voorvoegsels voor peered VNets. U kunt deze voorvoegsels weergeven in de lijst **ExpandedAddressPrefix** oplossen van eventuele problemen met betrekking tot peering verbinding VNet. 
- Effectieve regels worden alleen weergegeven als er een NSG van de VM NIC en of het subnet gekoppeld is. 
- Als er geen NSGs die is gekoppeld aan de Netwerkkaart of subnet en u een openbaar IP-adres dat is toegewezen aan uw VM hebt, alle poorten worden geopend voor binnenkomend en uitgaand verkeer. Als de VM een openbaar IP-adres heeft, wordt NSGs toepassen op de NIC of het subnet sterk aanbevolen.  
