## <a name="virtual-network-basics"></a>Grondbeginselen van virtueel netwerk

### <a name="what-is-an-azure-virtual-network-vnet"></a>Wat is een Azure Virtual network (VNet)?

U kunt VNets gebruiken om in te richten en virtuele particuliere netwerken (VPN's) beheren in Azure en, eventueel koppelen de VNets met andere VNets in Azure, of met uw locatie op IT-infrastructuur oplossingen voor hybride of cross-ruimte maken. Elke VNet die u maakt heeft een eigen CIDR-blok en kan worden gekoppeld aan andere netwerken VNets en op bedrijven als de CIDR-blokken daarmee niet in tegenspraak. U hebt ook controle van de instellingen van de DNS-server voor VNets en segmentatie van de VNet in subnetten.

VNets te gebruiken:

- Een speciale persoonlijke cloud alleen virtueel netwerk maken

    Hebt u nodig niet een configuratie met meerdere lokalen soms voor uw oplossing. Wanneer u een VNet maakt, kunnen de services en VMs binnen uw VNet rechtstreeks en veilig met elkaar communiceren in de cloud. Dit houdt het verkeer veilig binnen het VNet, maar nog steeds kunt u configureren verbindingen eindpunt voor de VMs en services waarvoor Internet-communicatie als onderdeel van de oplossing.

- Veilig uw datacenter uitbreiden

    Met VNets, kunt u traditionele site naar site (S2S) VPN-verbindingen wilt schalen veilig de capaciteit van uw datacenter bouwen. IPSEC VPN's S2S gebruiken om een beveiligde verbinding tussen uw bedrijf VPN-gateway en Azure.

- Hybride cloud scenario's mogelijk

    VNets geven u de flexibiliteit om ondersteuning van een aantal hybride cloud-scenario's. Cloud-toepassingen naar een systeem van bedrijfsruimten zoals mainframes en Unix-systemen kunt u veilig verbinden.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Hoe weet ik of ik een virtueel netwerk nodig?

Bezoek het [Virtuele netwerk-overzicht](../articles/virtual-network/virtual-networks-overview.md) voor een beschikking van de tabel waarmee u de beste optie voor het ontwerp van netwerk voor u bepalen.

### <a name="how-do-i-get-started"></a>Hoe moet ik beginnen?

Ga naar [de virtueel netwerk documentatie](https://azure.microsoft.com/documentation/services/virtual-network/) aan de slag. Deze pagina bevat koppelingen naar algemene configuratiestappen, alsmede informatie waarmee u de dingen die hebt je in aanmerking te nemen bij het ontwerpen van het virtuele netwerk begrijpt.

### <a name="what-services-can-i-use-with-vnets"></a>Welke diensten kan ik gebruiken met VNets

VNets kan worden gebruikt met een aantal verschillende Azure services zoals Cloud Services (PaaS) en virtuele Machines, Web Apps. Er zijn echter enkele services die niet worden ondersteund op een VNet. Controleer of de specifieke service die u wilt gebruiken en controleren of deze compatibel is.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Kan ik VNets gebruiken zonder verbinding met meerdere gebouwen

Ja. U kunt een VNet gebruiken zonder verbinding van site naar site. Dit is vooral handig als u wilt dat voor het uitvoeren van domeincontrollers en SharePoint-farms in Azure.

## <a name="virtual-network-configuration"></a>Virtueel netwerkconfiguratie

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Welke hulpmiddelen gebruik ik een VNet maken?

U kunt de volgende hulpprogramma's maken of configureren van een virtueel netwerk:

- Azure Portal (voor klassieke en VNets Resource Manager).

- Een netwerk-configuratiebestand (netcfg - voor klassieke VNets alleen). Zie [een virtueel netwerk met behulp van een configuratiebestand netwerk configureren](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShell (voor klassieke en VNets Resource Manager).

- Azure CLI (voor klassieke en VNets Resource Manager).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Welke adresbereiken kan ik gebruiken in mijn VNets?

Kunt u bereiken van openbare IP-adres en een IP-adresbereik is gedefinieerd in [RFC 1918](http://tools.ietf.org/html/rfc1918).

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Kan ik in mijn VNets openbare IP-adressen hebben?

Ja. Zie voor meer informatie over openbare IP-adresbereiken, [openbare IP-adresruimte in een virtueel netwerk (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Houd er rekening mee dat uw openbare IP-adressen niet direct toegankelijk is via het Internet worden.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Is er een limiet aan het aantal subnetten in het virtuele netwerk?

Er is geen limiet op het aantal subnetten die u binnen een VNet gebruikt. Alle subnetten moeten volledig zijn opgenomen in de adresruimte van het virtuele netwerk en niet met elkaar moeten overlappen.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Zijn er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?

Azure gereserveerd aantal IP-adressen in elk subnet. De eerste en laatste IP-adressen van de subnetten zijn gereserveerd voor protocol-conformiteit met 3 meer adressen voor Azure services gebruikt.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Hoe klein en hoe groot kan worden VNets en subnetten?

Het kleinste subnet dat wij ondersteunen is een /29 en de grootste is een /8 (met CIDR subnet definities).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Kan ik doen om mijn VLAN's naar Azure met VNets?

Nr. VNets zijn Layer 3-overlays. Azure biedt geen ondersteuning voor een Layer 2-semantiek.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Kan ik aangepaste routeringsbeleid op mijn VNets en subnetten opgeven?

Ja. U kunt de gebruiker gedefinieerd routering (UDR). Ga naar de [gebruiker gedefinieerd Routes en doorsturen via IP](../articles/virtual-network/virtual-networks-udr-overview.md)voor meer informatie over UDR.

### <a name="do-vnets-support-multicast-or-broadcast"></a>Ondersteunen VNets multicast- of broadcast?

Nr. Wij bieden geen ondersteuning voor multicast- of broadcast.

### <a name="what-protocols-can-i-use-within-vnets"></a>Welke protocollen kan ik gebruiken in VNets?

U kunt de standaard IP-protocollen binnen de VNets. Generic Routing Encapsulation (GRE)-pakketten en multicast-, broadcast, IP-in-IP-ingekapselde pakketten worden geblokkeerd in VNets. Standaard-protocollen werken omvatten:

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kan ik mijn standaard-routers in een VNet pingen?

Nr.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Kan ik bij de diagnose van connectiviteit tracert gebruiken?

Nr.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Kan ik subnetten toevoegen nadat de VNet is gemaakt?

Ja. Subnetten kunnen worden toegevoegd aan de VNets op elk gewenst moment, zolang het subnetadres geen deel uit van een ander subnet in de VNet maakt.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Kan ik de grootte van mijn subnet wijzigen nadat ik deze heb gemaakt?

U kunt toevoegen, verwijderen, uitbreiden of verkleinen van een subnet als er geen VMs of services geïmplementeerd binnen het via PowerShell-cmdlets of het bestand NETCFG. U kunt ook toevoegen, verwijderen, uitbreiden of verkleinen geen voorvoegsels, zolang de subnetten met VMs of services worden niet beïnvloed door de wijziging.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Kan ik subnetten wijzigen nadat ik ze heb gemaakt?

Ja. U kunt toevoegen, verwijderen en aanpassen van de CIDR-blokken door een VNet gebruikt.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Kan ik verbinding maken met het internet als ik mijn diensten in een VNet?

Ja. Alle services geïmplementeerd in een VNet kunnen verbinding maken met het internet. Elke cloud-service geïmplementeerd in Azure is een openbaar adresseerbare VIP toegewezen. Hebt u voor het definiëren van invoer eindpunten voor rollen PaaS- en eindpunten voor virtuele machines voor het inschakelen van deze services verbindingen vanaf het internet te accepteren.

### <a name="do-vnets-support-ipv6"></a>VNets bieden ondersteuning voor IPv6?

Nr. U kunt IPv6 met VNets op dit moment gebruiken.

### <a name="can-a-vnet-span-regions"></a>Kan een VNet regio's omvatten?

Nr. Een VNet is beperkt tot een bepaalde regio.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Kan ik een VNet verbinden met een andere VNet in Azure?

Ja. VNet communicatie kunt met REST API's of met Windows PowerShell u VNet maken. Ook kunt u VNets via Peering VNet. Meer informatie over peering [hier.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Naamomzetting (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Wat zijn de DNS-opties voor VNets?

Gebruik de tabel besluit op de pagina voor [Naamomzetting voor VMs en exemplaren van de rol](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) als leidraad voor alle DNS-opties die beschikbaar zijn.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Kan ik DNS-servers opgeven voor een VNet?

Ja. U kunt IP-adressen van DNS-servers in de VNet instellingen opgeven. Dit wordt toegepast als de standaard DNS-server (s) voor alle VMs in de VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Hoeveel DNS-servers kan ik opgeven?

U kunt maximaal 12 DNS-servers opgeven.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Kan ik mijn DNS-servers wijzigen nadat ik het netwerk hebt gemaakt?

Ja. U kunt de lijst met DNS-server voor uw VNet op elk gewenst moment wijzigen. Als u de lijst met DNS-server wijzigt, moet u elk van de VMs in uw VNet zodat ze de nieuwe DNS-server te starten.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Wat is DNS geleverde Azure en werkt het met VNets?

Azure geleverde DNS is een huurder met meerdere DNS-service die wordt aangeboden door Microsoft. Azure registreert alle VMs en rol exemplaren in deze service. Deze service biedt naamomzetting voor VMs en de rol van instanties die zich in dezelfde service cloud-hostnaam en FQDN-naam voor het VMs en instanties in de VNet met dezelfde rol.

> [AZURE.NOTE] Er is een beperking op dit moment op de eerste 100 cloud-services in het virtuele netwerk voor cross-huurder naamomzetting via DNS Azure-voorwaarde. Als u uw eigen DNS-server gebruikt, is deze beperking niet van toepassing.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Kan ik mijn DNS-instellingen op een per-VM / basis service?

Ja. U kunt DNS-servers instellen op een basis per cloud service overschrijven de standaardinstellingen van het netwerk. We raden echter aan zo veel mogelijk gehele netwerk-DNS te gebruiken.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kan ik mijn eigen DNS-achtervoegsel meenemen?

Nr. U kunt een aangepaste DNS-achtervoegsel opgeven voor uw VNets.

## <a name="vnets-and-vms"></a>VNets en VMs

### <a name="can-i-deploy-vms-to-a-vnet"></a>Kan ik VMs implementeren naar een VNet?

Ja.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Kan ik Linux VMs implementeren naar een VNet?

Ja. U kunt een distro Linux ondersteund door Azure implementeren.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Wat is het verschil tussen een openbare VIP en een intern IP-adres?

- Een intern IP-adres is een IP-adres dat is toegewezen aan elke VM binnen een VNet door DHCP. Het is geen openbare facing. Als u een VNet hebt gemaakt, wordt het interne IP-adres van het bereik dat u hebt opgegeven in de subnetinstellingen van de VNet toegewezen. Als er niet een VNet, wordt nog steeds een interne IP-adres toegewezen. Het interne IP-adres blijft met de VM voor de levensduur, tenzij die VM wordt vrijgegeven.

- Een openbare VIP is het openbare IP-adres dat is toegewezen aan uw cloud service of load balancer. Het is niet rechtstreeks toegewezen aan uw VM NIC. De VIP blijft met de cloud-service die is toegewezen aan totdat alle VMs in die cloud-service worden opgeheven of verwijderd. Op dat moment wordt vrijgegeven.

### <a name="what-ip-address-will-my-vm-receive"></a>Welk IP-adres ontvangt mijn VM?

- **Interne IP-adres-** Als u een VM op een VNet implementeren, ontvangt de VM een intern IP-adres uit een groep van interne IP-adressen die u opgeeft. VMs communiceren binnen de VNets met behulp van interne IP-adressen. Hoewel Azure een dynamische interne IP-adres toewijst, kunt u een statisch adres voor uw VM kunt aanvragen. Bezoek voor meer informatie over statische IP-adressen van interne, [het instellen van een statisch interne IP-adres](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** Uw VM is ook gekoppeld aan een VIP, hoewel een VIP is nooit voor VM rechtstreeks toegewezen. Een VIP is een openbaar IP-adres dat aan uw cloud-service kan worden toegewezen. Desgewenst kunt u een VIP reserveren voor uw cloud-service.

- **ILPIP-** U kunt ook een exemplaar-openbare IP-adres (ILPIP). ILPIPs zijn rechtstreeks gekoppeld aan de VM in plaats van de cloud-service. Bezoek voor meer informatie over ILPIPs, [Instantieniveau openbare IP-overzicht](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kan ik een intern IP-adres reserveren voor een VM die ik op een later tijdstip wilt maken?

Nr. Een intern IP-adres kan niet worden gereserveerd. Als een intern IP-adres beschikbaar is wordt deze door de DHCP-server op een rol of VM instantie worden toegewezen. Die VM al kan dan niet dat u wilt dat het interne IP-adres moet worden toegewezen aan. U kunt echter het interne IP-adres van een reeds gemaakte VM om alle beschikbare interne IP-adres te wijzigen.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Interne IP-adressen wijzigen voor VMs in een VNet doen?

Ja. Interne IP-adressen blijven met de VM de levensduur tenzij de VM wordt vrijgegeven. Wanneer een VM wordt vrijgegeven, wordt het interne IP-adres vrijgegeven tenzij u een statische interne IP-adres voor uw VM hebt gedefinieerd. Als de VM is gewoon gestopt (en niet is opgeslagen in de status **(Deallocated) gestopt**) blijven het IP-adres toegewezen aan de VM.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Kan ik IP-adressen handmatig toewijzen aan NIC in VMs?

Nr. U moet elke interface-eigenschappen van VMs niet wijzigen. Wijzigingen kunnen leiden tot verlies van connectiviteit mogelijk voor VM.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Wat gebeurt er met Mijn IP-adressen als ik een VM afsluiten?

Er is niets. De IP-adressen (openbare VIP en interne IP-adres) blijven met uw service cloud of VM.

> [AZURE.NOTE] Als u wilt gewoon af te sluiten de VM, gebruik niet de Portal Management te doen. Op dit moment wordt de virtuele machine deallocate, de knop Afsluiten.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Kan ik verplaatsen VMs uit één subnet naar een ander subnet in een VNet zonder opnieuw implementeren?

Ja. Meer informatie vindt u [hier](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kan ik een MAC-adres statisch configureren voor mijn VM

Nr. Een MAC-adres worden niet statisch geconfigureerd.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Het MAC-adres blijft hetzelfde voor mijn VM eenmaal is gemaakt?

Ja, het MAC-adres blijft hetzelfde voor een VM, hoewel de VM is gestopt (deallocated) en een nieuw leven ingeblazen.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Kan ik verbinding maken met het internet vanuit een VM in een VNet?

Ja. Alle services geïmplementeerd in een VNet kunnen verbinding maken met het Internet. Bovendien heeft elke cloud-service geïmplementeerd in Azure een openbaar adresseerbare VIP toegewezen. U hebt voor het definiëren van invoer eindpunten voor rollen PaaS- en eindpunten voor VMs inschakelen van deze services verbindingen vanaf het Internet te accepteren.

## <a name="vnets-and-services"></a>VNets en -Services

### <a name="what-services-can-i-use-with-vnets"></a>Welke diensten kan ik gebruiken met VNets

U kunt alleen compute diensten in VNets. COMPUTE-services zijn beperkt tot Cloud Services (web- en werknemer rollen) en VMs.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Kan ik met een virtueel netwerk Web Apps gebruiken?

Ja. U kunt Web-Apps binnen een VNet met behulp van ASE (App-omgeving) implementeren. Toe te voegen aan dat Web Apps kan veilig verbinding maken en toegang krijgen tot bronnen in uw Azure-VNet als u een punt-naar-site voor uw VNet geconfigureerd hebt. Zie de volgende onderwerpen voor meer informatie:


- [Web Apps maken in een App-omgeving](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Integratie met het Web Apps virtueel netwerk](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [VNet-integratie en hybride verbindingen gebruiken met Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Integreren in een virtueel netwerk Azure een web app.](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Kan ik de cloud-services met web- en werknemer rollen (PaaS) in een VNet implementeren?

Ja. U kunt services PaaS binnen VNets implementeren.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Hoe kan ik PaaS rollen om een VNet te implementeren?

U kunt dit doen door de naam VNet en de toewijzingen van de /subnet rol te geven in de sectie netwerk configuratie van de configuratie van de service. U hoeft niet de update voor de binaire bestanden.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Kan ik mijn diensten en naar de VNets verplaatsen?

Nr. U kunt services en VNets niet verplaatsen. U moet verwijderen en opnieuw implementeren van de service te verplaatsen naar een andere VNet.

## <a name="vnets-and-security"></a>VNets en beveiliging

### <a name="what-is-the-security-model-for-vnets"></a>Wat is het beveiligingsmodel voor VNets?

VNets zijn volledig geïsoleerd van elkaar en andere services van de infrastructuur van Azure. Een VNet is de grens van een vertrouwensrelatie.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Kan ik ACL's of NSGs definiëren op mijn VNets?

Nr. ACL's of NSGs te VNets kunt u niet koppelen. Echter, ACL's kunnen worden gedefinieerd op de invoer van eindpunten voor VMs die zijn geïmplementeerd op een VNets en NSGs kan worden gekoppeld aan de subnetten of NIC's.

### <a name="is-there-a-vnet-security-whitepaper"></a>Is er een VNet security whitepaper?

Ja. U kunt het downloaden [hier](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>API's, schema's en hulpprogramma 's

### <a name="can-i-manage-vnets-from-code"></a>Kan ik VNets beheren vanuit code?

Ja. U kunt REST API's VNets en cross-premises connectiviteit beheren. Meer informatie vindt u [hier](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Is er ondersteuning voor VNets tooling?

Ja. Extra PowerShell en de opdrachtregel kunt u voor verschillende platforms. Meer informatie vindt u [hier](http://go.microsoft.com/fwlink/?LinkId=317721).
