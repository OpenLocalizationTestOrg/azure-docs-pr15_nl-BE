<properties
    pageTitle="Een Active Directory-forest installeren op een virtueel netwerk van Azure | Microsoft Azure"
    description="Een zelfstudie waarin wordt uitgelegd hoe u een nieuw Active Directory-forest te maken op een virtuele machine (VM) op een virtueel netwerk Azure."
    services="active-directory, virtual-network"
    keywords="Active directory virtuele machine, active directory-forest installeren azure active directory video 's "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Een nieuw Active Directory-forest installeren op een virtueel netwerk van Azure

Dit onderwerp bevat een nieuwe Windows Server Active Directory-omgeving op een Azure virtueel netwerk op een virtuele machine (VM) maken op een [virtueel netwerk Azure](../virtual-network/virtual-networks-overview.md). In dit geval is de Azure virtueel netwerk niet verbonden met een netwerk op gebouwen.

Ook worden geïnteresseerd in deze Verwante onderwerpen:

- Zie voor een video waarin deze stappen, [het installeren van een nieuw Active Directory-forest op een virtueel netwerk van Azure](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- U kunt desgewenst [configureren van een VPN website](../vpn-gateway/vpn-gateway-site-to-site-create.md) en installeer vervolgens een nieuw forest of een forest op gebouwen met een Azure virtueel netwerk uitbreiden. Zie het onderwerp over het [installeren van een Replica Active Directory-domeincontroller in een virtueel netwerk Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)voor deze stappen.
-  Zie [richtlijnen voor de implementatie van Windows Server Active Directory Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)voor conceptuele informatie over het installeren van Active Directory Domain Services (AD DS) op een virtueel netwerk van Azure.

## <a name="scenario-diagram"></a>Scenario-Diagram

In dit scenario moeten de externe gebruikers toegang krijgen tot toepassingen die worden uitgevoerd op servers die deel uitmaakt van een domein. Het VMs die de application servers worden uitgevoerd en de VMs-domeincontrollers die zijn geïnstalleerd in hun eigen cloud-service in een virtueel netwerk van Azure. Ze zijn ook opgenomen in een beschikbaarheid voor verbeterde fouttolerantie ingesteld.

![Active Directory-forest op een virtuele machines in Virtual Network Azure ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Hoe is dit het verschil in ruimten?

Er is niet veel verschil tussen een domeincontroller installeren op Azure versus op gebouwen. In de volgende tabel staan de belangrijkste verschillen.

Configureren...  | In gebouwen  | Azure virtueel netwerk
------------- | -------------  | ------------
**IP-adres van de domeincontroller**  | Statisch IP-adres van de netwerkadapter eigenschappen toewijzen   | Voer de cmdlet Set-AzureStaticVNetIP om een statisch IP-adres toewijzen
**DNS-clientomzetting**  | Adres van de voorkeurs- en alternatieve DNS-server op het netwerk adaptereigenschappen van leden van een domein instellen   | Adres van de DNS-server instellen voor de eigenschappen van het virtuele netwerk
**Opslag in Active Directory-database**  | Wijzig desgewenst de standaardopslaglocatie van C:\  | Moet u de standaardopslaglocatie wijzigen van C:\



## <a name="create-an-azure-virtual-network"></a>Een Azure virtueel netwerk maken

1. Log in om de klassieke Azure portal.
2. Een virtueel netwerk maken. Klik op **netwerken** > **een virtueel netwerk maken**. De waarden in de volgende tabel gebruiken om de wizard te voltooien.

    Op deze wizardpagina...  | Deze waarden opgeven
    ------------- | -------------
    **Virtueel netwerkdetails**  | <p>Naam: Voer een naam voor het virtuele netwerk</p><p>Regio: Kies de dichtstbijzijnde regio</p>
    **DNS- en VPN**  | <p>Laat de DNS-server leeg</p><p>Een VPN-optie niet selecteren</p>
    **Virtueel netwerk adresruimten**  | <p>Subnetnaam: Voer een naam voor het subnet</p><p>Begin IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>VMs om uit te voeren van de domeincontroller en DNS-server-rollen maken

Herhaal de volgende stappen voor het maken van VMs als host voor de rol van domeincontroller indien nodig. U moet ten minste twee virtuele DCs fouttolerantie en redundantie implementeren. Als de Azure virtueel netwerk omvat ten minste twee domeincontrollers die op dezelfde manier zijn geconfigureerd (ze zijn beide GC, DNS-server uitvoeren, en geen van beide bezit is van een FSMO-functie, enzovoort) plaats het VMs die die in een beschikbaar stellen voor verbeterde fouttolerantie DCs uitgevoerd.

Zie de VMs maken met behulp van Windows PowerShell in plaats van de gebruikersinterface [Met Azure PowerShell maken en vooraf configureren van Windows-gebaseerde virtuele Machines](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Klik op **Nieuw**in het portal voor klassieke > **berekenen** > **virtuele Machine** > **Uit galerie**. De volgende waarden gebruiken om de wizard te voltooien. Accepteer de standaardwaarde voor de instelling, tenzij een andere waarde wordt voorgesteld of vereist.

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

## <a name="install-windows-server-active-directory"></a>Windows Server Active Directory installeren

Gebruik dezelfde routine [AD DS installeren](https://technet.microsoft.com/library/jj574166.aspx) op locatie te gebruiken (dat wil zeggen, kunt u de gebruikersinterface, een antwoordbestand of Windows PowerShell). U moet beheerdersreferenties om een nieuw forest te installeren. Wanneer u de locatie voor de Active Directory-database, logboekbestanden en SYSVOL, wijzigt de standaardopslaglocatie van het station naar de schijf voor aanvullende gegevens die u hebt gekoppeld aan de VM.

Nadat de DC-installatie is voltooid, opnieuw verbinding met de VM en meld u aan bij de domeincontroller. Vergeet niet om op te geven van de domeinreferenties.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>De DNS-server voor de Azure virtueel netwerk opnieuw instellen

1. De instelling van de DNS-doorstuurserver op de nieuwe domeincontroller en DNS-server opnieuw.
  1. In Serverbeheer, klik op **Extra** > **DNS**.
  2. In **DNS-beheer**, klik met de rechtermuisknop op de naam van de DNS-server en klik op **Eigenschappen**.
  3. Het IP-adres van de doorstuurserver en klik op **bewerken**op het tabblad **doorstuurservers** .  Selecteer het IP-adres en klik op **verwijderen**.
  4. Klik op **OK** om de editor te sluiten en **Ok** te sluiten van de eigenschappen van de DNS-server.
2. De DNS-server de instelling voor het virtuele netwerk bijwerken.
  1. Klik op de **Virtuele netwerken** > Dubbelklik op het virtuele netwerk dat u hebt gemaakt > **Configure** > **DNS-servers**, typt u de naam en de DIP van één van de VMs die de functie van domeincontroller en DNS-server wordt uitgevoerd en klik op **Opslaan**.
  2. De VM en klik op **opnieuw** activeren van de VM DNS-resolver om instellingen te configureren met het IP-adres van de nieuwe DNS-server.


## <a name="create-vms-for-domain-members"></a>VMs voor leden van een domein maken

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


## <a name="see-also"></a>Zie ook

-  [Een nieuw Active Directory-forest installeren op een virtueel netwerk van Azure](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Richtlijnen voor de implementatie van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [Configureren van een VPN website](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Een Replica Active Directory-domeincontroller installeren in een virtueel netwerk van Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: Fundamentals (01) Virtual Machine](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en connectiviteit tussen ruimten](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Virtueel netwerk-overzicht](../virtual-network/virtual-networks-overview.md)
-  [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure Cmdlet referentie](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Azure VM statisch IP-adres instellen](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Statisch IP-adres toewijzen aan Azure VM](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Een Active Directory-Forest installeren](https://technet.microsoft.com/library/jj574166.aspx)
-  [Inleiding tot Active Directory Domain Services (AD DS) virtualisatie (Level 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
