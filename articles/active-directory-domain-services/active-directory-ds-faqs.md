<properties
    pageTitle="Veelgestelde vragen - Azure Active Directory Domain Services | Microsoft Azure"
    description="Veelgestelde vragen over Azure Active Directory Domain Services"
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
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Veelgestelde vragen (FAQ's)

Deze pagina antwoorden op veelgestelde vragen over Azure Active Directory Domain Services. Regelmatig terug voor updates.

### <a name="troubleshooting-guide"></a>Problemen oplossen met
Zie onze [handleiding voor probleemoplossing](active-directory-ds-troubleshooting.md) voor oplossingen voor veelvoorkomende problemen bij het configureren en beheren van Azure AD Domain Services.


### <a name="configuration"></a>Configuratie

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Kan ik meerdere domeinen voor één maken Azure AD directory?
Nr. U kunt slechts één domein dat wordt onderhouden door Azure AD Domain Services voor één maken Azure AD directory.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Kan ik Azure AD Domain Services in een virtueel netwerk Azure bronbeheer inschakelen?
Nr. Azure AD Domain Services kunnen alleen worden ingeschakeld in een klassieke Azure virtueel netwerk. Met een virtueel netwerk virtueel netwerk peering als u wilt uw beheerde domein in een virtueel netwerk voor bronbeheer gebruiken met behulp van Resource Manager kunt u de klassieke virtueel netwerk.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Ik maak Azure AD Domain Services beschikbaar in meerdere virtuele netwerken binnen mijn abonnement?
De service zelf ondersteunt dit scenario niet rechtstreeks. Azure AD Domain Services is beschikbaar in slechts één virtueel netwerk tegelijk. U kunt echter verbinding tussen meerdere virtuele netwerken te openbaren Azure AD Domain Services aan andere virtuele netwerken configureren. In dit artikel wordt beschreven hoe u kunt [verbinding maken met virtuele netwerken in Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Kan ik een AD-domein Azure Services met PowerShell inschakelen?
PowerShell/geautomatiseerde implementatie van Azure AD Domain Services is momenteel niet beschikbaar.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Azure AD Domain Services beschikbaar is in de nieuwe Azure portal?
Nr. Azure AD Domain Services kunnen alleen in de [Azure klassieke portal](https://manage.windowsazure.com)worden geconfigureerd. We verwachten ondersteuning voor de [Azure portal](https://portal.azure.com) in de toekomst uitbreiden.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Kan ik een beheerde Azure AD Domain Services-domein domeincontrollers toevoegen?
Nr. Het domein die door Azure AD Domain Services is een beheerde domein. U hoeft niet te creëren, configureren of beheren van domeincontrollers voor dit domein - anders worden deze activiteiten als een service door Microsoft geleverd. U kunt extra domeincontrollers (lezen / schrijven of alleen-lezen) voor de beheerde domein daarom niet toevoegen.

### <a name="administration-and-operations"></a>Bewerkingen en beheer

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Kan ik verbinding maken met de domeincontroller voor mijn beheerde domein met extern bureaublad?
Nr. U bent niet gemachtigd om verbinding maken met domeincontrollers voor het domein beheerd via Extern bureaublad. Leden van de groep 'Beheerders AAD DC' kunnen de beheerde domein met AD-beheerprogramma's zoals de Active Directory Administration Center (ADAC) of AD PowerShell beheren. Deze hulpprogramma's zijn geïnstalleerd op een Windows-server die is verbonden met het domein beheerd met behulp van de functie 'Remote Server Administration Tools'.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Ik heb de Azure AD Domain Services hebt ingeschakeld. Welke gebruikersaccount moet ik gebruiken voor domein join-computers naar dit domein?
Gebruikers die u hebt toegevoegd aan de beheergroep (bijvoorbeeld ' AAD DC beheerders') kunnen domeinen aan machines. Bovendien gebruikers in deze groep remote desktop toegang verleend tot computers die zijn samengevoegd met het domein.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Kan ik nauwkeuriger domein administrator-bevoegdheden voor het domein door Azure AD Domain Services geleverd?
Nr. U kunt beheerdersrechten voor het domein beheerd worden niet verleend. Zowel beheerder van domein- en Enterprise-beheerder machtigingen zijn niet beschikbaar voor gebruik binnen het domein. Bestaande domeinbeheerder of enterprise administratorgroepen binnen de map Azure AD zijn ook niet toegestaan domein/enterprise administrator-bevoegdheden voor het domein.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Kan ik met behulp van LDAP of andere beheerprogramma's van AD op domeinen die door het AD-Domain Azure Services groepslidmaatschappen wijzigen?
Nr. Groepslidmaatschappen worden niet op domeinen die worden verwerkt door het AD-Domain Azure Services gewijzigd. Hetzelfde geldt voor de gebruikerskenmerken. U kunt echter groepslidmaatschappen of gebruikerskenmerken wijzigen in Azure AD of in uw domein op gebouwen. Dergelijke wijzigingen worden automatisch gesynchroniseerd naar Azure AD Domain Services.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Kan ik het schema van het domein die door AD-Domain Azure Services uitbreiden?
Nr. Het schema wordt beheerd door Microsoft voor het domein beheerd. Schema-uitbreidingen worden niet ondersteund door Azure AD Domain Services.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Kan ik wijzigen of de DNS-records toevoegen aan mijn beheerde domein?
Ja. Gebruikers die tot de groep 'Beheerders AAD DC behoren' krijgen ' DNS' beheerdersbevoegdheden, DNS-records in de beheerde domein wijzigen. Deze gebruikers kunnen op een computer met Windows Server is toegevoegd aan het domein beheerd, de console DNS-beheer gebruiken voor het beheren van DNS. Gebruik de console DNS-beheer, 'DNS-Server extra", die deel van de optionele functie 'Remote Server Administration Tools' op de server uitmaakt te installeren. Meer informatie over [hulpprogramma's voor beheer, controle en probleemoplossing van DNS](https://technet.microsoft.com/library/cc753579.aspx) is beschikbaar op TechNet.


### <a name="billing-and-availability"></a>Facturering en beschikbaarheid

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Is dat Azure AD Domain Services een betaalde service?
Ja. Zie de [prijzen pagina](https://azure.microsoft.com/pricing/details/active-directory-ds/)voor meer informatie.

#### <a name="is-there-a-free-trial-for-the-service"></a>Is er een gratis proefversie voor de service?
Deze service is opgenomen in de gratis proefversie voor Azure. U kunt zich aanmelden voor een [gratis proefperiode van een maand van Azure](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Krijg ik Azure AD Domain Services als onderdeel van Enterprise Mobility Suite (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Moet ik Azure AD Premium Azure AD Domain Services gebruiken?
Nr. Azure AD Domain Services is een pay-as-you-go Azure service en is geen onderdeel van EMS. Azure AD Domain Services beschikbaar zijn voor alle edities van Azure advertentie (gratis, Basic en Premium) en worden gefactureerd op uurbasis, afhankelijk van het gebruik.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Welke gebieden Azure is de service beschikbaar?
Raadpleeg de pagina [Azure Services per regio](https://azure.microsoft.com/regions/#services/) voor een overzicht van de Azure regio's waar de Azure AD Domain Services beschikbaar is.
