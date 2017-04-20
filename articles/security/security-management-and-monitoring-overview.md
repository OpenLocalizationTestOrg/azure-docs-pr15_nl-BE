<properties
   pageTitle="Azure Security Management en een overzicht van bewaking | Microsoft Azure"
   description=" Azure biedt beveiligingsmechanismen om u te helpen bij het beheer en de controle van Azure cloud services en virtuele machines.  Dit artikel biedt een overzicht van deze basisbeveiligingsfuncties en services. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Azure Security Management en een overzicht van bewaking

Azure biedt beveiligingsmechanismen om u te helpen bij het beheer en de controle van Azure cloud services en virtuele machines. Dit artikel biedt een overzicht van deze basisbeveiligingsfuncties en services. Koppelingen worden naar artikelen waarin de details van elk geven zal zodat u meer informatie verstrekt.

De beveiliging van uw Microsoft cloud services is een partnerschap en gedeelde verantwoordelijkheid tussen u en Microsoft. Gedeelde verantwoordelijkheid betekent dat Microsoft verantwoordelijk is voor de Microsoft Azure en fysieke beveiliging van de data centers (met behulp van de beveiliging beveiligingen zoals vergrendelde badge post deuren, hekken en schermen). Azure biedt bovendien sterke niveaus van beveiliging van de wolk op de software-laag die voldoet aan de beveiliging, privacy en naleving van de behoeften van zijn veeleisende klanten.

Eigenaar van uw gegevens en identiteiten, de verantwoordelijkheid voor de bescherming van hen, de beveiliging van uw resources in ruimten en de veiligheid van cloud onderdelen waarover u controle hebt. Microsoft beschikt u over beveiligingsfuncties en mogelijkheden voor het beveiligen van uw gegevens en toepassingen. De verantwoordelijkheid voor de beveiliging is gebaseerd op het type service cloud.

De volgende tabel bevat een overzicht van het saldo van de verantwoordelijkheid voor zowel Microsoft als de klant.

![Gedeelde verantwoordelijkheid][1]

Zie [Security management in Azure](azure-security-management.md)voor een diepere Kennismaking in het beveiligingsbeheer van de.

Hier vindt u de kernfuncties zullen worden behandeld in dit artikel:

- Role-Based Access Control
- Antimalware
- Meerledige verificatie
- ExpressRoute
- Virtueel netwerkgateways
- Beschermde identiteitsbeheer
- Bescherming van de identiteit
- Beveiligingscentrum

## <a name="role-based-access-control"></a>Role-Based Access Control

Role-Based Access Control RBAC () biedt verfijnde toegangsbeheer voor Azure resources. RBAC gebruikt, kunt u geven mensen alleen het bedrag van de toegang die ze nodig hebben om hun taken uitvoeren.  RBAC kunt u ervoor te zorgen dat wanneer mensen de organisatie verlaten zij toegang tot bronnen in de cloud.

Meer informatie:

- [Active Directory-teamblog op RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware

Met Azure kunt u uw virtuele machines beveiligen tegen schadelijke bestanden, adware en andere bedreigingen antimalware software van toonaangevende leveranciers zoals Microsoft, Symantec, Trend Micro, McAfee en Kaspersky.

Microsoft Antimalware biedt u een agent antimalware voor rollen PaaS en virtuele machines te installeren. Op basis van System Center Endpoint Protection, brengt deze functie op ruimten is bewezen technologie naar de cloud.

Wij bieden ook naadloze integratie van Trend [Diep beveiliging](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ en [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™-producten in de Azure platform. DeepSecurity is een antivirusoplossing en SecureCloud is een oplossing voor codering. DeepSecurity worden geïmplementeerd in VMs met behulp van een model voor de uitbreiding. Met behulp van de portal UI en PowerShell, kunt u het gebruik van DeepSecurity in nieuwe VMs die zijn omhoog wordt gesponnen of bestaande VMs die al zijn geïmplementeerd.

Symantec eindpunt beveiliging (SEP) wordt ondersteund op Azure. Door integratie van portal hebben-klanten de mogelijkheid om op te geven dat zij voornemens zijn te gebruiken SEP binnen een VM. SEP kan worden geïnstalleerd op een nieuwe VM via het Portal Azure of kan worden geïnstalleerd op een bestaande VM met PowerShell.

Meer informatie:

- [Implementatie van Antimalware oplossingen op Azure virtuele Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsoft Antimalware voor Azure Cloud Services en virtuele Machines](../security/azure-security-antimalware.md)
- [Het installeren en configureren van diep Security van Trend Micro als een Service op een Windows VM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Het installeren en configureren van Symantec Endpoint Protection op een Windows VM](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nieuwe Antimalware-opties voor het beveiligen van Azure Virtual Machines – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Meerledige verificatie

Azure meerledige verificatie (MVR gesloten) is een methode voor verificatie vereist het gebruik van meer dan één verificatiemethode en een kritische tweede beveiligingslaag toegevoegd op aanmeldingen van gebruikers en transacties. MVR gesloten helpt beschermen de toegang tot gegevens en toepassingen bij het voldoen aan eisen van de gebruiker voor een eenvoudig aanmelden proces. Levert een sterke verificatie via een reeks opties voor verificatie, telefoongesprek, tekstbericht of mobiele app kennisgevings- of code en 3de partij EDE tokens.

Meer informatie:

- [Meerledige verificatie](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Wat is Azure meerledige verificatie?](../multi-factor-authentication/multi-factor-authentication.md)
- [De werking van Azure meerledige verificatie](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure ExpressRoute kunt u uw netwerken op ruimten uitbreiden naar de cloud met Microsoft via een specifieke persoonlijke verbinding vergemakkelijkt door een provider connectiviteit. Met ExpressRoute, kunt u verbindingen met Microsoft cloud services, zoals Microsoft Azure, Office 365 en CRM Online maken. Verbinding kan van een any-to-any (VPN IP)-netwerk, een point-to-point Ethernet-netwerk of een virtuele cross-verbinding via een provider connectiviteit in een inrichting collocatie worden. ExpressRoute verbindingen gaan niet via het openbare Internet. Hierdoor ExpressRoute verbindingen biedt meer betrouwbaarheid, hogere snelheden, lagere vertragingstijden en een hogere mate van beveiliging dan de normale verbindingen via het Internet.

Meer informatie:

- [ExpressRoute-technisch overzicht](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtueel netwerkgateways

VPN-Gateways, Azure virtuele netwerkgateways, ook wel gebruikt voor het verzenden van netwerkverkeer tussen virtuele netwerken en locaties op gebouwen. Ze worden ook gebruikt voor het verzenden van verkeer tussen meerdere virtuele netwerken in Azure (VNet VNet).  VPN-gateways verzorgen beveiligde cross-ruimten tussen Azure en de infrastructuur.

Meer informatie:

- [Over VPN-gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Azure netwerk beveiliging-overzicht](security-network-overview.md)

## <a name="privileged-identity-management"></a>Beschermde Identity Management

Gebruikers moeten soms om de begunstigde verrichtingen in Azure resources of andere SaaS-toepassingen uit te voeren. Dit betekent vaak organisaties hebben ze permanent bevoorrechte toegang geven in Azure Active Directory (AD Azure). Dit is een toenemend risico voor resources cloud gehost omdat organisaties voldoende kunnen niet controleren wat de gebruikers doen met hun bevoorrechte toegang.
Ook als een gebruikersaccount met bevoorrechte toegang is aangetast, die een inbreuk kan van invloed zijn uw algehele beveiliging in de cloud. Azure rechten AD Identity Management helpt dit risico oplossen door de belichtingstijd van bevoegdheden te verlagen en meer inzicht in gebruik.  

Beschermde Identity Management introduceert het concept van een tijdelijke admin voor een rol of 'just in time"beheerderstoegang, wat een gebruiker die moet worden geactiveerd voor die rol toegewezen voltooid is. Het activeringsproces wordt de toewijzing van de gebruiker een rol in Azure AD, gedurende een bepaalde tijd actief inactief periode bijvoorbeeld 8 uur.

Meer informatie:

- [Azure AD bevoorrechte Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Aan de slag met Azure AD beschermde Identity Management](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Bescherming van de identiteit

Azure Active Directory (AD) identiteit bescherming biedt een totaaloverzicht van verdachte-in activiteiten en potentiële beveiligingsproblemen voor het beschermen van uw bedrijf. Bescherming van identiteiten detecteert verdachte activiteiten voor gebruikers en rechten (admin) identiteiten, op basis van signalen zoals brute-force aanvallen, referenties en aanmeldingen van onbekende locaties uitgelekt en geïnfecteerde apparaten.

Door middel van meldingen en aanbevolen herstel helpt identiteit bescherming risico's in real-time te beperken. Gebruiker risico ernst wordt berekend en u kunt beleidsregels op basis van het risico om toepassing van vrijwaringsmaatregelen tegen toekomstige bedreigingen toegang automatisch te configureren.

Meer informatie:

- [Identiteitsbeveiliging Azure Active Directory](../active-directory/active-directory-identityprotection.md)
- [Channel 9: Azure AD en identiteit weergeven: identiteit bescherming Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Beveiligingscentrum

Azure Beveiligingscentrum helpt u bij het voorkomen, opsporen en reageren op bedreigingen en biedt dat u meer inzicht in, en controle over, de beveiliging van uw resources Azure. Het biedt geïntegreerde beveiliging beleid en het toezicht op verschillende de Azure abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

Beveiligingscentrum helpt u bij het optimaliseren en controleren van de beveiliging van uw Azure bronnen door:

- Zodat u kunt het beleid voor uw resources Azure abonnement beveiligingsbehoeften van uw bedrijf en het type van toepassingen of de gevoeligheid van de gegevens in elk abonnement te definiëren.
- Controle van de status van uw Azure virtuele machines, netwerken en toepassingen.
- Biedt een lijst met naar prioriteit gerangschikte beveiligingswaarschuwingen, inclusief waarschuwingen van de voor geïntegreerde partneroplossingen, en de informatie die u nodig hebt snel te onderzoeken en aanbevelingen voor het verhelpen van een aanval.

Meer informatie:

- [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
