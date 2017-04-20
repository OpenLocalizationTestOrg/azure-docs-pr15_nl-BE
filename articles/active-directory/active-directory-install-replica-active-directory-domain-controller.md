<properties
    pageTitle="Een replica Active Directory-domeincontroller installeren in Azure | Microsoft Azure"
    description="Een zelfstudie waarin wordt uitgelegd hoe u een domeincontroller van een Active Directory-forest op gebouwen op een Azure virtuele machine installeren."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Een replica Active Directory-domeincontroller installeren in een virtueel netwerk van Azure

Dit onderwerp bevat het installeren van extra domeincontrollers (ook bekend als replica DCs) voor Active Directory-domein op ruimten op Azure virtuele machines (VMs) in een virtueel netwerk van Azure.

Ook worden geïnteresseerd in deze Verwante onderwerpen:

-  Desgewenst kunt u een nieuw Active Directory-forest installeren op een virtueel netwerk van Azure. Zie [een nieuw Active Directory-forest op een Azure virtueel netwerk installeren](../active-directory/active-directory-new-forest-virtual-machine.md)voor deze stappen.
-  Zie [richtlijnen voor de implementatie van Windows Server Active Directory Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)voor conceptuele informatie over het installeren van Active Directory Domain Services (AD DS) op een virtueel netwerk van Azure.


## <a name="scenario-diagram"></a>Scenario-diagram

In dit scenario moeten de externe gebruikers toegang krijgen tot toepassingen die worden uitgevoerd op servers die deel uitmaakt van een domein. Het VMs die de application servers worden uitgevoerd en de replica-DC's worden geïnstalleerd in een virtueel netwerk van Azure. Het virtuele netwerk kan worden verbonden met het netwerk op gebouwen door een [site naar site VPN-](../vpn-gateway/vpn-gateway-site-to-site-create.md) verbinding, zoals in het volgende diagram of kunt u [ExpressRoute](../expressroute/expressroute-locations-providers.md) voor een snellere verbinding.

De toepassingsservers en de DC's worden geïmplementeerd binnen afzonderlijke cloud services distribueren compute verwerking en [beschikbaarheid sets](../virtual-machines/virtual-machines-windows-manage-availability.md) voor verbeterde fouttolerantie.
De DCs repliceren met elkaar en met de DCs voor ruimten met behulp van Active Directory-replicatie. De programma's geen synchronisatie nodig.

![Diagram pf replica Active Directory-domeincontroller een Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Een Active Directory-site voor de Azure virtueel netwerk maken

Het is een goed idee om een site te maken in Active Directory die het netwerk gebied dat overeenkomt met het virtuele netwerk vertegenwoordigt. Die helpt bij het optimaliseren van verificatie, replicatie en andere bewerkingen DC locatie. De volgende stappen wordt uitgelegd hoe een site maken en Zie voor meer achtergrondinformatie, [een nieuwe Site toe te voegen](https://technet.microsoft.com/library/cc781496.aspx).

1. Open Active Directory: Sites en Services: **Server Manager** > **Extra** > **Active Directory: Sites en Services**.
2. Maken van een site voor de regio waar u een virtueel netwerk van Azure gemaakt: klik op **websites** > **actie** > **nieuwe site** > Typ de naam van de nieuwe site, zoals Azure ons West > Selecteer een site-koppeling > **OK**.
3. Een subnet maken en koppelen aan de nieuwe site: Dubbelklik op **Sites** > met de rechtermuisknop op **subnetten** > **Nieuw subnet** > typt u het IP-adresbereik van het virtuele netwerk (zoals 10.1.0.0/16 in het diagram scenario) > Selecteer de nieuwe Azure site > **OK**.

## <a name="create-an-azure-virtual-network"></a>Een Azure virtueel netwerk maken

1. Klik op **Nieuw**in [Azure klassieke portal](https://manage.windowsazure.com) > **Network Services** > **Virtueel netwerk** > **Aangepaste maken** en gebruik de volgende waarden om de wizard te voltooien.

    Op deze wizardpagina...  | Deze waarden opgeven
    ------------- | -------------
    **Virtueel netwerkdetails**  | <p>Naam: Typ een naam voor het virtuele netwerk, zoals WestUSVNet.</p><p>Regio: Kies de dichtstbijzijnde regio.</p>
    **DNS- en VPN-verbindingen**  | <p>DNS-Servers: De naam en het IP-adres van een of meer op lokalen, DNS-servers opgeven.</p><p>Connectiviteit: Selecteer **configureren met een VPN website**.</p><p>Lokaal netwerk: Geef een nieuw lokaal netwerk.</p><p>Zie [een verbinding ExpressRoute via een Exchange-Provider configureren](../expressroute/expressroute-locations-providers.md)als u ExpressRoute in plaats van een VPN.</p>
    **Verbinding van site naar site**  | <p>Naam: Typ een naam voor het netwerk op gebouwen.</p><p>VPN-apparaat IP-adres: Geef het openbare IP-adres van het apparaat dat verbinding met het virtuele netwerk. Het VPN-apparaat kan niet zich bevinden achter een NAT bevindt.</p><p>Adres: Geef de adresbereiken van de voor het netwerk op ruimten (zoals 192.168.0.0/16 in het diagram scenario).</p>
    **Virtueel netwerk adresruimten**  | <p>Address Space: Geef het IP-adresbereik voor VMs die u wilt uitvoeren in de Azure virtueel netwerk (zoals 10.1.0.0/16 in het diagram scenario). Dit adresbereik elkaar niet overlappen met de adresbereiken van het netwerk op gebouwen.</p><p>Subnetten: Geef een naam en adres van een subnet voor de toepassingsservers (zoals Frontend, 10.1.1.0/24) en voor de DC's (zoals back-end, 10.1.2.0/24).</p><p>Klik op **gateway subnet toevoegen**.</p>

2. Vervolgens kunt u het virtuele netwerkgateway wilt maken van een beveiligde VPN-verbinding van site naar site configureren. Zie [een virtuele netwerk-Gateway configureren](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) voor de instructies.
3. Maak de site naar site VPN-verbinding tussen het nieuwe virtuele netwerk en een VPN-apparaat van op gebouwen. Zie [een virtuele netwerk-Gateway configureren](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) voor de instructies.


## <a name="create-azure-vms-for-the-dc-roles"></a>Azure VMs voor de DC-rollen maken

Herhaal de volgende stappen voor het maken van VMs als host voor de rol van domeincontroller indien nodig. U moet ten minste twee virtuele DCs fouttolerantie en redundantie implementeren. Als de Azure virtueel netwerk omvat ten minste twee domeincontrollers die op dezelfde manier zijn geconfigureerd (ze zijn beide GC, DNS-server uitvoeren, en geen van beide bezit is van een FSMO-functie, enzovoort) plaats het VMs die die in een beschikbaar stellen voor verbeterde fouttolerantie DCs uitgevoerd.
Zie de VMs maken met behulp van Windows PowerShell in plaats van de gebruikersinterface [Met Azure PowerShell maken en vooraf configureren van Windows-gebaseerde virtuele Machines](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Klik op **Nieuw**in [Azure klassieke portal](https://manage.windowsazure.com) > **berekenen** > **virtuele Machine** > **Uit galerie**. De volgende waarden gebruiken om de wizard te voltooien. Accepteer de standaardwaarde voor de instelling, tenzij een andere waarde wordt voorgesteld of vereist.

    Op deze wizardpagina...  | Deze waarden opgeven
    ------------- | -------------
    **Een afbeelding kiezen**  | Windows Server 2012 R2 Datacenter
    **Configuratie van de virtuele Machine**  | <p>De naam van de virtuele Machine: Typ een naam met één label (bijvoorbeeld AzureDC1).</p><p>Nieuwe naam: Typ de naam van een gebruiker. Deze gebruiker is een lid van de lokale groep Administrators op de VM. U moet deze naam op voor de eerste keer aanmelden bij de VM. De ingebouwde account Administrator werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p>
    **Configuratie van de virtuele Machine**  | <p>Cloud-Service: Kies <b>een nieuwe wolk service maken</b> voor de eerste VM en die dezelfde naam van cloud service selecteren als u meer VMs die als host voor de rol van domeincontroller fungeert maakt.</p><p>DNS-naam van Service cloud: Een globaal unieke naam opgeven</p><p>Regio/affiniteit groep/virtueel netwerk: Geef de netwerknaam van het virtuele (bijvoorbeeld WestUSVNet).</p><p>Opslag Account: Kies <b>een account automatisch gegenereerde opslag gebruiken</b> voor de eerste VM en selecteer vervolgens die dezelfde opslag accountnaam als u meer VMs die als host voor de rol van domeincontroller fungeert maakt.</p><p>Set beschikbaarheid: Kies <b>een set beschikbaar maken</b>.</p><p>Beschikbaarheid van naam instellen: Typ een naam voor de beschikbaarheid instellen wanneer u de eerste VM maken en selecteer vervolgens dezelfde naam wanneer u meer VMs maakt.</p>
    **Configuratie van de virtuele Machine**  | <p>Selecteer <b>de VM-Agent installeren</b> en alle andere extensies die u nodig hebt.</p>
2. Een schijf toevoegen aan elke VM de DC-serverfunctie wordt uitgevoerd. Extra schijfruimte is vereist voor het opslaan van de AD-database, logboekbestanden en SYSVOL. Geef een grootte van de schijf (bijvoorbeeld 10 GB) en laat u de **Host-Cache voorkeur** ingesteld op **geen**. Zie voor het werk, [het koppelen van een schijf met gegevens naar een virtuele Windows-computer](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Nadat u eerst bij de VM aanmelden, opent u **Server Manager** > **Bestands- en opslagservices** voor het maken van een volume op deze vaste schijf met NTFS.
4. Een statisch IP-adres reserveren voor VMs die de rol van domeincontroller wordt uitgevoerd. U kunt een statisch IP-adres reserveren, downloaden van de Microsoft Web Platform Installer en [Installeer Azure PowerShell](../powershell-install-configure.md) en de cmdlet Set-AzureStaticVNetIP uitgevoerd. Bijvoorbeeld:

    ' Get-AzureVM - servicenaam AzureDC1-naam AzureDC1 | Set AzureStaticVNetIP - IP-adres adres 10.0.0.4 | Update AzureVM

Zie voor meer informatie over het instellen van een statisch IP-adres [configureren statische interne IP-adres voor een VM](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>AD DS installeren op Azure VMs

Aanmelden bij een VM en controleer of u verbinding via de site naar site VPN- of ExpressRoute verbinding met bronnen op het netwerk op gebouwen hebt. Vervolgens installeert u AD DS op de VMs Azure. U kunt dezelfde procedure waarmee u een extra domeincontroller installeren in uw netwerk op gebouwen (UI, Windows PowerShell of een antwoordbestand). Als u AD DS installeert, moet dat u het nieuwe volume opgeven voor de locatie van de AD-database, logboekbestanden en SYSVOL. Als u een refresher op AD DS-installatie, Zie [Installeren van Active Directory Domain Services (Level 100)](https://technet.microsoft.com/library/hh472162.aspx) of [installeert u een Replica Windows Server 2012-domeincontroller in een bestaand domein (niveau 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>DNS-server voor het virtuele netwerk configureren

1. Klik op de naam van het virtuele netwerk en klik vervolgens op het tabblad **configureren** [opnieuw configureren van de DNS-server IP-adressen voor het virtuele netwerk](../virtual-network/virtual-networks-manage-dns-in-vnet.md) gebruik van de statische IP-adressen zijn toegewezen aan de replica-DC's in plaats van de IP-adressen van de DNS-servers van een in de lokalen in [Azure klassieke portal](https://manage.windowsazure.com).

2. Klik op **virtuele Machines**om te controleren of alle replica VMs DC op het virtuele netwerk zijn geconfigureerd met DNS-servers op het virtuele netwerk gebruiken, klikt u op de statuskolom voor elke VM en klik op **opnieuw opstarten**. Wacht totdat de VM **waarop** staat ziet voordat u zich aanmeldt bij deze.

## <a name="create-vms-for-application-servers"></a>VMs maken voor toepassingsservers

1. Herhaal de volgende stappen voor het maken van VMs als application servers uitvoeren. Accepteer de standaardwaarde voor de instelling, tenzij een andere waarde wordt voorgesteld of vereist.

    Op deze wizardpagina...  | Deze waarden opgeven
    ------------- | -------------
    **Een afbeelding kiezen**  | Windows Server 2012 R2 Datacenter
    **Configuratie van de virtuele Machine**  | <p>De naam van de virtuele Machine: Typ een naam met één label (bijvoorbeeld AppServer1).</p><p>Nieuwe naam: Typ de naam van een gebruiker. Deze gebruiker is een lid van de lokale groep Administrators op de VM. U moet deze naam op voor de eerste keer aanmelden bij de VM. De ingebouwde account Administrator werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p>
    **Configuratie van de virtuele Machine**  | <p>Cloud-Service: Kies **een nieuwe wolk service maken** voor de eerste VM en selecteer die dezelfde naam van cloud-service bij het maken van meer VMs die als host voor de toepassing fungeert.</p><p>DNS-naam van Service cloud: Een globaal unieke naam opgeven</p><p>Regio/affiniteit groep/virtueel netwerk: Geef de netwerknaam van het virtuele (bijvoorbeeld WestUSVNet).</p><p>Opslag Account: Kies **een account automatisch gegenereerde opslag gebruiken** voor de eerste VM en schakel die dezelfde accountnaam voor de opslag bij het maken van meer VMs die als host voor de toepassing fungeert.</p><p>Set beschikbaarheid: Kies **een set beschikbaar maken**.</p><p>Beschikbaarheid van naam instellen: Typ een naam voor de beschikbaarheid instellen wanneer u de eerste VM maken en selecteer vervolgens dezelfde naam wanneer u meer VMs maakt.</p>
    **Configuratie van de virtuele Machine**  | <p>Selecteer <b>de VM-Agent installeren</b> en alle andere extensies die u nodig hebt.</p>

2. Na elke VM is ingericht, log in en aan het domein toevoegen. Klik in **Serverbeheer**op **Lokale Server** > **werkgroep** > **wijzigen...** **domein** selecteren en typ de naam van uw domein op gebouwen. Referenties van een gebruiker en de VM om het domeinlidmaatschap start.

Zie de VMs maken met behulp van Windows PowerShell in plaats van de gebruikersinterface [Met Azure PowerShell maken en vooraf configureren van Windows-gebaseerde virtuele Machines](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Zie [Aan de slag met Azure Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx) en [Azure Cmdlet referentie](https://msdn.microsoft.com/library/azure/jj554330.aspx)voor meer informatie over het gebruik van Windows PowerShell.

## <a name="additional-resources"></a>Aanvullende bronnen

-  [Richtlijnen voor de implementatie van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Het uploaden van bestaande op ruimten Hyper-V-domeincontrollers naar Azure met Azure PowerShell](http://support.microsoft.com/kb/2904015)
-  [Een nieuw Active Directory-forest installeren op een virtueel netwerk van Azure](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: Fundamentals (01) Virtual Machine](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en connectiviteit tussen ruimten](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure Management-Cmdlets](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
