<properties
    pageTitle="Azure AD Domain Services: Netwerken richtlijnen | Microsoft Azure"
    description="Overwegingen bij het netwerk voor Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Overwegingen voor het AD-Domain Azure Services toegang

## <a name="how-to-select-an-azure-virtual-network"></a>Het selecteren van een Azure virtueel netwerk
De volgende richtlijnen kunnen u een virtueel netwerk gebruiken met Azure AD Domain Services selecteren.

### <a name="type-of-azure-virtual-network"></a>Type Azure virtueel netwerk

- U kunt de Azure AD Domain Services in een klassieke Azure virtueel netwerk inschakelen.

- Azure AD Domain Services **in de virtuele netwerken die zijn gemaakt met behulp van Azure Resource Manager kan niet worden ingeschakeld**.

- Kunt u een virtuele Resource Manager-netwerk een klassieke virtueel netwerk waarin Azure AD Domain Services is ingeschakeld. Daarna kunt u Azure AD Domain Services in het virtuele netwerk op basis van een Resource Manager. Zie voor meer informatie de sectie [verbinding met het netwerk](active-directory-ds-networking.md#network-connectivity) .

- **Regionale virtuele netwerken**: als u een bestaande virtuele netwerk te gebruiken, er zorg voor dat een regionale virtueel netwerk.

    - Virtuele netwerken die gebruikmaken van het mechanisme voor oudere affiniteit groepen kunnen niet worden gebruikt met Azure AD Domain Services.

    - Azure AD Domain Services, [migreert u oude virtuele netwerken met regionale virtuele netwerken](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)gebruiken.


### <a name="azure-region-for-the-virtual-network"></a>Azure regio voor het virtuele netwerk

- Uw Azure AD Domain Services beheerde domein is geïmplementeerd in hetzelfde gebied, Azure als het virtuele netwerk u de service in te schakelen.

- Selecteer een virtueel netwerk in een Azure regio worden ondersteund door Azure AD Domain Services.

- Zie de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) te weten de Azure gebieden waarin Azure AD Domain Services beschikbaar is.


### <a name="requirements-for-the-virtual-network"></a>Vereisten voor het virtuele netwerk

- **Nabijheid van uw werkbelasting Azure**: Selecteer het virtuele netwerk waarmee u momenteel host/host voor virtuele machines die toegang tot Azure AD Domain Services hebben.

- **Aangepaste/brengen zelf DNS-servers**: Zorg ervoor dat er geen aangepaste DNS-servers die zijn geconfigureerd voor het virtuele netwerk.

- **Bestaande domeinen met dezelfde domeinnaam**: Zorg ervoor dat er geen een bestaand domein met dezelfde naam van het domein op dat virtuele netwerk beschikbaar. Bijvoorbeeld, gaan we uit van een domein met de naam 'contoso.com' al beschikbaar op de geselecteerde virtuele netwerk. Later, probeert u een beheerde Azure AD Domain Services-domein met dezelfde domeinnaam (dat is 'contoso.com') op die virtuele netwerk inschakelen. U kunt een fout optreden als u Azure AD Domain Services inschakelen. Deze fout is het gevolg van naamconflicten voor de domeinnaam in die virtuele netwerk. In dit geval moet u een andere naam voor het instellen van uw beheerde Azure AD Domain Services-domein. U kunt ook uit het bestaande domein inrichten en gaat u verder met het AD-Domain Azure Services inschakelen.

> [AZURE.WARNING] U kunt Domain Services niet verplaatsen naar een ander virtueel netwerk nadat u de service hebt ingeschakeld.


## <a name="network-security-groups-and-subnet-design"></a>Beveiligingsgroepen netwerk en subnet-ontwerp
Een [Network Security Group (NSG)](../virtual-network/virtual-networks-nsg.md) bevat een lijst van de lijst ACL (Access Control)-regels die toestaan of weigeren netwerkverkeer naar de VM-exemplaren in een virtueel netwerk. NSGs kunnen worden gekoppeld aan de subnetten of afzonderlijke exemplaren van de VM op dat subnet. Wanneer een NSG gekoppeld aan een subnet is, gelden de regels van de ACL voor de VM exemplaren in dat subnet. Bovendien een afzonderlijke VM-verkeer kan worden beperkt door het koppelen van een NSG rechtstreeks naar die VM verder.

![Aanbevolen subnet ontwerp](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Aanbevolen procedures voor het kiezen van een subnet
- Azure AD Domain Services implementeren op een **afzonderlijke specifieke subnet** binnen uw Azure virtueel netwerk.

- NSGs niet van toepassing op het specifiek subnet voor uw domein beheerd. Als NSGs u op het subnet toegewezen toepassen moet, controleert u **de vereiste poorten voor het onderhouden en beheren van uw domein niet geblokkeerd**.

- Niet overmatig beperken het aantal IP-adressen beschikbaar in specifieke subnet voor uw domein beheerd. Deze beperking voorkomt dat de service twee domeincontrollers beschikbaar maken voor uw domein beheerd.

- **Azure AD Domain Services in het subnet van de gateway niet inschakelen** van het virtuele netwerk.


> [AZURE.WARNING] Als u koppelt een NSG met een subnet waarin Azure AD Domain Services is ingeschakeld, wordt u de mogelijkheid Microsoft service en het beheren van het domein kan verstoren. Bovendien is de synchronisatie tussen uw huurder Azure AD en uw beheerde domein verstoord. **De SLA heeft geen betrekking op installaties waar een NSG is toegepast, die blokkeert Azure AD Domain Services uit bijwerken en beheren van uw domein.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Vereiste poorten voor Azure AD Domain Services
De volgende poorten vereist zijn voor AD-domein Azure Services met service en onderhouden van uw beheerde domein. Zorg ervoor dat deze poorten niet zijn geblokkeerd voor het subnet waarin u uw beheerde domein hebt ingeschakeld.

| Nummer van de poort | Doel |
|---|---|
| 443 | Synchronisatie met uw huurder Azure AD |
| 3389 | Beheer van uw domein |
| 5986 | Beheer van uw domein |
| 636 | Veilige LDAP (LDAPS) toegang tot uw beheerde domein |



## <a name="network-connectivity"></a>Verbinding met het netwerk
Een beheerde Azure AD Domain Services-domein kan alleen binnen een enkele klassieke virtueel netwerk in Azure worden ingeschakeld. Virtuele netwerken die zijn gemaakt met behulp van bronbeheer Azure worden niet ondersteund.


### <a name="scenarios-for-connecting-azure-networks"></a>Scenario's voor het verbinden van netwerken Azure
Verbinding Azure virtuele netwerken voor het gebruik van het beheerde domein in een van de volgende implementatiescenario's:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Het beheerde domein in meer dan één Azure klassieke virtueel netwerk gebruiken
U kunt andere Azure klassieke virtuele netwerken verbinden met de Azure klassieke virtueel netwerk waar u Azure AD Domain Services hebt ingeschakeld. Deze VPN-verbinding kunt u de beheerde domein met uw werkbelasting in andere virtuele netwerken worden geïmplementeerd.

![Klassieke virtueel netwerkconnectiviteit](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Het beheerde domein in een virtueel netwerk met bronbeheer gebruiken
Kunt u een virtuele netwerk op basis van een Resource Manager de Azure klassieke virtueel netwerk waar u Azure AD Domain Services hebt ingeschakeld. Deze verbinding kunt u de beheerde domein met uw werkbelasting in het virtuele netwerk op basis van een bronnenbeheerder geïmplementeerd.

![Resource Manager kan klassieke virtueel netwerkconnectiviteit](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Opties voor een netwerkverbinding

- **VNet-VNet - verbindingen met behulp van VPN-verbindingen van site naar site**: een virtueel netwerk verbinding te maken met een andere virtuele netwerk (VNet VNet) is vergelijkbaar met een virtueel netwerk verbinding te maken met een locatie in de ruimten. Een VPN-gateway beide typen verbindingen gebruiken om een beveiligde tunnel met IPsec/IKE.

    ![Verbinding met het virtuele netwerk met behulp van VPN-Gateway](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Meer informatie - virtuele netwerken met behulp van VPN-gateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **VNet-VNet - verbindingen met behulp van virtueel netwerk peering**: virtueel netwerk peering is een mechanisme dat twee virtuele netwerken in dezelfde regio via de Azure backbone-netwerk verbindt. Zodra peered, worden de twee virtuele netwerken weergegeven als een voor alle verbindingen. Ze nog steeds als afzonderlijke bronnen worden beheerd, maar de virtuele machines in deze virtuele netwerken met elkaar kunnen communiceren via particuliere IP-adressen.

    ![Verbinding met het virtuele netwerk peering gebruiken](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Meer informatie - virtueel netwerk peering](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Verwante inhoud

- [Azure virtueel netwerk peering](../virtual-network/virtual-network-peering-overview.md)

- [Een VNet-VNet-verbinding voor het implementatiemodel klassiek configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure netwerk beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md)
