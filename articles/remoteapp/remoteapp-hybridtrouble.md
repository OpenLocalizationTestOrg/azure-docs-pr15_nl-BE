
<properties
    pageTitle="Problemen maken RemoteApp-hybride collecties | Microsoft Azure"
    description="Informatie over hoe u problemen met RemoteApp-hybride collectie maken"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Problemen oplossen met maken Azure RemoteApp-hybride collecties

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Een verzameling hybride wordt gehost in en worden gegevens opgeslagen in Azure cloud maar kunnen gebruikers toegang tot gegevens en bronnen die zijn opgeslagen op uw lokale netwerk. Gebruikers hebben toegang tot apps door aan te melden met hun zakelijke referenties gesynchroniseerd of federatieve met Azure Active Directory. U kunt een hybride-collectie die een virtueel netwerk van bestaande Azure implementeren of kunt u een nieuw virtueel netwerk. Wij raden aan dat u maakt of een virtueel netwerk subnet met een CIDR-bereik groot genoeg is voor de verwachte toekomstige groei voor Azure RemoteApp.

Nog niet gemaakt uw collectie nog? Zie [maken een hybride-collectie](remoteapp-create-hybrid-deployment.md) voor de stappen.

Als u problemen ondervindt bij het maken van uw verzameling, of als u de collectie niet werkt zoals u denkt dat het moet, kijk dan eens de volgende informatie.

## <a name="your-image-is-invalid"></a>Uw afbeelding is ongeldig ##
Als u een bericht zoals 'GoldImageInvalid' wanneer u wacht Azure uw collectie inrichten, betekent dit dat uw Sjabloonafbeelding voldoet niet aan de [vereisten image gedefinieerd](remoteapp-imagereqs.md). Dus ga lezen deze [vereisten](remoteapp-imagereqs.md), uw image te herstellen en maakt uw verzameling opnieuw.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Heeft uw VNET network security groepen, definitie ##
Hebt u een netwerk-beveiligingsgroepen die zijn gedefinieerd in het subnet dat u voor uw verzameling gebruikt, moet deze [URL's en poorten](remoteapp-ports.md) via uw subnet toegankelijk zijn.

Op de door u worden ingezet in het subnet voor een preciezere controle VMs kunt u extra beveiligingsgroepen toevoegen.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Gebruikt u uw eigen DNS-servers? En zijn ze toegankelijk via het subnet VNET? ##
>[AZURE.NOTE] U moet Controleer of dat de DNS-servers in uw VNET zijn altijd omhoog en altijd kunnen worden omgezet van de virtuele machines die worden gehost in de VNET. Gebruik geen Google DNS voor dit.


Voor hybride verzamelingen kunt u uw eigen DNS-servers gebruiken. U ze in uw netwerk configuratieschema of via de portal management bij het maken van het virtuele netwerk. DNS-servers worden gebruikt in de volgorde waarin deze zijn opgegeven in een failover-wijze (in plaats van round robin).  
Zie [Naamomzetting voor VMs en rol instanties](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) om ervoor te zorgen dat de DNS-servers zijn geconfigureerd correcly.

Controleer of de DNS-servers voor uw verzameling zijn toegankelijk en beschikbaar via het subnet van de VNET die u hebt opgegeven voor deze verzameling.

Bijvoorbeeld:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definieer uw DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Gebruikt u een Active Directory-domeincontroller in uw collectie? ##
Momenteel slechts één Active Directory-domein gekoppeld aan een RemoteApp Azure worden kan. De hybride-collectie ondersteunt alleen Azure Active Directory-accounts gesynchroniseerd met DirSync gereedschap uit een Windows Server Active Directory-distributie; met name een gesynchroniseerd met de optie Wachtwoordsynchronisatie of gesynchroniseerd met Active Directory Federation Services (AD FS) federation geconfigureerd. Moet u een aangepaste domein dat overeenkomt met het domein UPN-achtervoegsel voor uw domein op gebouwen maken en instellen van de directory-integratie.

Zie [Active Directory voor Azure RemoteApp configureren](remoteapp-ad.md) voor meer informatie.

Zorg ervoor dat de informatie domein geldig zijn en de domeincontroller bereikbaar is via de gemaakt in het subnet gebruikt voor externe App Azure VM. Controleer ook of de service-account opgegeven referenties machtigingen computers toevoegen aan het opgegeven domein hebben en dat de naam van de advertentie opgelost in de DNS-server die is opgegeven in de VNET worden kan.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Welke domeinnaam hebt u opgegeven bij het maken van uw verzameling? ##

De domeinnaam die u hebt gemaakt of toegevoegd moet een interne domeinnaam (niet uw Azure AD domeinnaam) en moet omgezet DNS-indeling (contoso.local). Bijvoorbeeld, u een interne naam voor Active Directory (contoso.local) en een Active Directory-UPN (contoso.com) - u moet de naam van de interne gebruiken bij het maken van uw verzameling.
