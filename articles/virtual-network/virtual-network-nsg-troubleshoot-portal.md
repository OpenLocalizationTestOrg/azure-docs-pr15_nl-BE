<properties 
   pageTitle="Problemen met netwerk-beveiligingsgroepen - Portal | Microsoft Azure"
   description="Informatie over het oplossen van problemen met netwerk-beveiligingsgroepen in het implementatiemodel voor Azure Resource Manager met behulp van de Portal Azure."
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

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Problemen met netwerk-beveiligingsgroepen met behulp van de Portal Azure

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

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Efficiënte beveiligingsregels weergeven voor een virtuele machine

Voer de volgende stappen uit om problemen met NSGs voor een VM:

U kunt volledige lijst met regels voor de effectieve weergeven op een NIC, uit de VM zelf. U kunt ook toevoegen, wijzigen en verwijderen van NIC en een subnetmasker NSG-regels van de bladeserver effectieve regels hebt u machtigingen voor het uitvoeren van deze bewerkingen.

1. Aanmelden bij de Azure portal op https://portal.azure.com.
2. Klik op **meer services**en klik vervolgens op **virtuele machines** in de lijst die wordt weergegeven.
3. Selecteer een VM oplossen in de lijst die wordt weergegeven en een VM-blade met opties wordt weergegeven.
4. Klik op **Diagnose & oplossen van problemen** en selecteer vervolgens een veelvoorkomend probleem. Bijvoorbeeld: **ik kan geen verbinding maken met mijn Windows VM** is geselecteerd. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Stappen worden weergegeven onder het probleem, zoals in de volgende afbeelding: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Klik op *regels voor effectieve groep* in de lijst met aanbevolen stappen.

6. De blade **effectieve regels ophalen** wordt weergegeven, zoals in de volgende afbeelding:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    U ziet in de volgende gedeelten van de afbeelding:

    - **Toepassingsgebied:** Ingesteld op *VM1*, de VM hebt geselecteerd in stap 3.
    - **Network interface:** *VM1 NIC1* is geselecteerd. Een VM kan hebben meerdere netwerkinterfaces (NIC). Unieke effectieve regels voor hebben elke Netwerkkaart. Wanneer problemen wilt oplossen, moet u wellicht de effectieve regels weergeven voor elke NIC
    - **Die NSGs:** NSGs kan worden toegepast op zowel de Netwerkkaart en het de Netwerkkaart is verbonden met subnet. In de afbeelding is een NSG toegepast op zowel de Netwerkkaart en het verbonden met subnet. U kunt klikken op de NSG-namen voor het direct wijzigen van regels in de NSGs.
    - **VM1 nsg tabblad:** De lijst met regels die worden weergegeven in de afbeelding is voor de NSG toegepast op de NIC. Verschillende standaardregels worden gemaakt door Azure wanneer een NSG wordt gemaakt. De standaardregels kunt u niet verwijderen, maar u kunt deze overschrijven met de voorschriften van een hogere prioriteit. Voor meer informatie over de standaardregels, lees het artikel [Overzicht NSG](virtual-networks-nsg.md#default-rules) .
    - **Doelkolom:** Sommige regels hebben de tekst in de kolom, anderen adresprefixen. De tekst is de naam van de standaardlabels worden toegepast op de regel toen deze werd gemaakt. De tags voor zijn systeem-id's die staan voor meerdere voorvoegsels. Selecteren van een regel met een code, zoals *AllowInternetOutBound*, worden de prefixen in de blade **-adresprefixen** .
    - **Download:** De lijst met regels kan lang zijn. U kunt een CSV-bestand van de regels voor off line analyse downloaden door te klikken op **downloaden** en opslaan van het bestand.
    - **AllowRDP** Binnenkomende regel: deze regel kunt u RDP-verbindingen naar de VM.
7. Klik op het tabblad **Subnet1 NSG** om de effectieve regels uit de NSG toegepast op het subnet, zoals in de volgende afbeelding: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Let op de regel van de **inkomende** *denyRDP* . Binnenkomende regels toegepast op het subnet worden geëvalueerd voordat de regels die worden toegepast op de netwerkinterface. Aangezien deze regel wordt toegepast op het subnet, mislukt de aanvraag voor verbinding met TCP-3389, omdat de regel voor toestaan op de NIC is nooit geëvalueerd. 

    De regel *denyRDP* is de reden waarom de RDP-verbinding is mislukt. Het verwijderen van moet het probleem oplossen.

    >[AZURE.NOTE]Als de VM die is gekoppeld aan de Netwerkkaart niet actief is of NSGs zijn toegepast op de NIC of subnet, worden geen regels weergegeven.

8. Om de NSG regels bewerken, klikt u op *Subnet1 NSG* in de sectie **NSGs is gekoppeld** .
   Hiermee opent u het blad **Subnet1 NSG** . U kunt de regels direct bewerken door te klikken op **binnenkomende regels**.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. Na het verwijderen van de binnenkomende regel *denyRDP* in de **Subnet1 NSG** en het toevoegen van een regel *allowRDP* , ziet de lijst met effectieve regels in de volgende afbeelding:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Controleer of TCP-poort 3389 open door openen van een RDP-verbinding naar de VM of met behulp van het hulpprogramma PsPing. U kunt meer over PsPing lezen op de [pagina PsPing downloaden](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Effectieve regels voor een netwerkinterface weergeven

Als uw VM verkeersstroom wordt gebracht voor een specifieke NIC, kunt u een volledige lijst van de effectieve regels voor de Netwerkkaart uit de context netwerk interfaces weergeven door de volgende stappen uit te voeren:

1. Aanmelden bij de Azure portal op https://portal.azure.com.
2. Klik op **meer services**en klik op **Netwerkinterfaces** in de lijst die wordt weergegeven.
3. Selecteer een netwerkinterface. In de volgende afbeelding wordt een NIC met de naam *VM1 NIC1* geselecteerd.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    U ziet dat de **Scope** is ingesteld op de netwerkinterface die is geselecteerd. Lees voor meer informatie over de aanvullende informatie weergegeven, in stap 6 van het **Oplossen van problemen met NSGs voor een VM** -sectie van dit artikel.

    >[AZURE.NOTE] Als een NSG wordt verwijderd uit een netwerkinterface, het subnet NSG nog steeds van kracht is op de opgegeven NIC. De uitvoer zou in dit geval alleen regels uit het subnet NSG weergeven. Regels worden alleen weergegeven als de Netwerkkaart is aangesloten op een VM.

4. U kunt regels rechtstreeks bewerken voor NSGs die zijn gekoppeld aan een Netwerkkaart en een subnetmasker. Voor meer informatie over hoe, lees stap 8 van de sectie **weergave effectieve regels voor een virtuele machine** van dit artikel.

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Effectieve regels voor een beveiligingsgroep netwerk (NSG) weergeven

Bij NSG regels wijzigen, kunt u bekijken de impact van de regels op een bepaalde VM wordt toegevoegd. U kunt een volledige lijst van de effectieve regels weergeven voor alle NIC's die een bepaalde NSG wordt toegepast zonder dat u hoeft over te schakelen van de context van de gegeven NSG blade. Effectieve regels binnen een NSG oplossen, voert u de volgende stappen uit:

1. Aanmelden bij de Azure portal op https://portal.azure.com.
2. **Meer services**, klik op **netwerk-beveiligingsgroepen** in de lijst die wordt weergegeven.
3. Selecteer een NSG. In de volgende afbeelding is een met de naam VM1 nsg NSG geselecteerd.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    In de volgende gedeelten van de afbeelding hierboven ziet:

    - **Toepassingsgebied:** Stel op de NSG geselecteerd.
    - **Virtual machine:** Wanneer een NSG op een subnet wordt toegepast, wordt deze toegepast op alle netwerkinterfaces zijn gekoppeld aan alle VMs aangesloten op het subnet. Deze lijst toont alle VMs deze NSG wordt toegepast. U kunt een VM uit de lijst selecteren.

    >[AZURE.NOTE] Als een NSG met alleen een lege subnet wordt toegepast, wordt de VMs worden niet weergegeven. Als een NSG wordt toegepast op een NIC die niet gekoppeld aan een VM is, worden deze NIC's ook niet weergegeven. 
    - **Network Interface:** Een VM kan hebben meerdere netwerkinterfaces. U kunt een netwerkinterface die is gekoppeld aan de geselecteerde VM.
    - **AssociatedNSGs:** Op elk moment kan een NIC hebben twee doeltreffende NSGs toegepast op de Netwerkkaart en het andere naar het subnet. Hoewel het bereik is geselecteerd als VM1-nsg als de NIC een effectieve subnet NSG heeft, wordt de uitvoer beide NSGs weergegeven.
4. U kunt regels rechtstreeks bewerken voor NSGs die zijn gekoppeld aan een Netwerkkaart of een subnet. Voor meer informatie over hoe, lees stap 8 van de sectie **weergave effectieve regels voor een virtuele machine** van dit artikel.

Lees voor meer informatie over de aanvullende informatie weergegeven, stap 6 van de sectie **weergave effectieve regels voor een virtuele machine** van dit artikel.

>[AZURE.NOTE] Hoewel een subnet- en NIC elk slechts één die NSG toegepast hebben kan, is een NSG kan worden gekoppeld aan meerdere NIC's en meerdere subnetten.

## <a name="considerations"></a>Overwegingen met betrekking tot

Houd rekening met de volgende punten bij het oplossen van problemen met netwerkverbindingen:

- Standaard NSG regels worden binnenkomende toegang vanaf het internet blokkeren en staan alleen de VNet binnenkomend verkeer. Regels moeten expliciet worden toegevoegd aan het toestaan van binnenkomende toegang van Internet, zoals vereist.
- Als er geen regels voor NSG waardoor de verbinding met het netwerk van een VM mislukt, wordt het probleem mogelijk vanwege:
    - Firewall-software in het besturingssysteem van de VM draaien
    - Routes die zijn geconfigureerd voor virtuele apparaten of verkeer op gebouwen. Internet-verkeer kan worden omgeleid naar op bedrijven via gedwongen tunneling. Een RDP/SSH-verbinding via het Internet voor uw VM werkt niet met deze instelling is afhankelijk van hoe de netwerkhardware op gebouwen dit verkeer verwerkt. Lees het artikel [Routes voor het oplossen van problemen](virtual-network-routes-troubleshoot-powershell.md) om te leren hoe route diagnosticeren die kunnen worden belemmeren de doorstroming van het verkeer in en uit de VM. 
- Als u hebt VNets, standaard peered, wordt automatisch de code van de VIRTUAL_NETWORK uitgebreid met voorvoegsels voor peered VNets. U kunt deze voorvoegsels weergeven in de lijst **ExpandedAddressPrefix** oplossen van eventuele problemen met betrekking tot peering verbinding VNet. 
- Effectieve regels worden alleen weergegeven als er een NSG van de VM NIC en of het subnet gekoppeld is. 
- Als er geen NSGs die is gekoppeld aan de Netwerkkaart of subnet en u een openbaar IP-adres dat is toegewezen aan uw VM hebt, alle poorten worden geopend voor binnenkomend en uitgaand verkeer. Als de VM een openbaar IP-adres heeft, wordt NSGs toepassen op de NIC of het subnet sterk aanbevolen.